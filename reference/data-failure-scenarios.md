# Reference: Data Pipeline Failure Scenarios

_Last reviewed: 2026-07-03 · Review cadence: quarterly_

A catalog of the ways data pipelines fail **silently** — the job shows green while the data is wrong. Each scenario: what happens, why orchestration stays green, and the gate that prevents it. Use it in design reviews, incident retros, and PR reviews.

> **TL;DR**
>
> - The most dangerous pipeline failures don't look like failures. **Green means the job ran; only data checks mean it worked.**
> - Almost every scenario below is prevented by one of four gates: **contract tests** on interfaces, **data-quality checks that block** (not just alert), **reconciliation against source**, and **idempotent, stage-tested changes**.
> - Fix the system, not the person: after any silent failure, the retro deliverable is a **gate**, not a promise to be more careful.

---

## The core rule

**Job status and data correctness are different signals.** Orchestration reports whether code executed without an unhandled error. It knows nothing about whether the output is complete, well-formed, fresh, or true. Any process (or person) that treats green as "verified" will eventually ship garbage with a clean conscience.

---

## The catalog

### 1. Silent contract break between producer and consumer

**What happens:** A stored procedure / job that produces output for a downstream consumer changes its format — e.g. an escaped JSON string becomes a clean JSON object, a delimiter changes, a field is renamed. The producer's job runs fine. The consumer chokes or mis-parses, but by then the producer already reported success.
**Why it stays green:** The producing job has no idea a consumer exists; the consumer's failure isn't wired back to anything.
**The gate:** Treat every output format as an **interface**. Document the lineage (who consumes this?), add a **contract test** that asserts the output shape (a malformed record fails the build in stage), and make the consumer's parse failure **fail the pipeline loudly** instead of being swallowed.

### 2. Green orchestration, swallowed error

**What happens:** A task wraps its logic in a try/catch that logs the error and exits 0, or a shell step ignores a non-zero exit code. The DAG shows every box green; one of them did nothing.
**Why it stays green:** The failure was caught and never re-raised; orchestration only sees exit codes.
**The gate:** Code-review rule: **no silent catch in pipeline tasks** — log, then re-raise. Lint for `exit 0` after failures. Alert on *output* signals (rows written, files produced), not just task status.

### 3. Success on empty

**What happens:** The source returns zero rows (API silently changed, filter typo, upstream table truncated). The job "processes" nothing, writes nothing, finishes instantly, reports success. Dashboards go stale or zero.
**Why it stays green:** Processing an empty set is not an error to the code.
**The gate:** **Volume assertions** — fail (or quarantine) when row counts fall outside an expected band vs. history. A job that normally moves 2M rows and moved 0 should never be green. Bonus signal: runtime anomaly — a 40-minute job finishing in 90 seconds is a red flag on its own.

### 4. Partial load / mid-stream truncation

**What happens:** The job loads 10% of the data, then a pagination bug, timeout, or expired token stops the stream *without throwing* — or a retry resumes from the wrong offset. The table has data, just not all of it.
**Why it stays green:** Some data landed; nothing errored hard.
**The gate:** **Reconciliation** — compare landed counts (and checksums or sums of key measures) against the source per batch. Make "source says 2.1M, we landed 1.9M" a blocking failure.

### 5. Expired credential causing quiet degradation

**What happens:** A token/secret expires. Depending on the client, this surfaces as an instant failure (lucky), or as empty responses / partial auth that the code treats as "no data" (unlucky — combines with #3/#4).
**Why it stays green:** The API returned 200-with-nothing, or the error was retried into silence.
**The gate:** **Secret expiry monitoring** ahead of time (alert at T-14 days), plus the volume assertions from #3 as the backstop. Treat auth errors as fatal, never as empty results.

### 6. Non-idempotent re-run → duplicates

**What happens:** A job fails midway and is re-run (or a scheduler double-fires). Inserts happen twice; revenue doubles in the mart. Everything is green — twice.
**Why it stays green:** Duplicate rows are valid rows to the loader.
**The gate:** **Idempotency by design** — merge/upsert on business keys, or delete-partition-then-write. Plus a **uniqueness test** on primary keys in the served layer that blocks promotion.

### 7. Schema drift from the source

**What happens:** A source system adds, renames, or re-types a column. Depending on the pipeline, the new column is silently dropped, a renamed column loads as all-null, or a type change coerces (int→string, precision loss).
**Why it stays green:** Most loaders tolerate shape changes by design.
**The gate:** **Schema contract / drift detection** — capture the expected schema, fail or quarantine on unexpected change, and route "new column appeared" to a human decision instead of a silent drop. Not-null tests on critical columns catch the rename-to-null case.

### 8. Join key mismatch → silent row loss

**What happens:** A transformation joins on a key whose format changed (case, padding, type), or an inner join quietly drops unmatched rows. Output is well-formed, plausible, and missing 30% of the business.
**Why it stays green:** A join dropping rows is correct SQL.
**The gate:** **Row-count flow checks** across transformation steps (input vs. output within expected ratio) and **referential-integrity tests** (every fact row finds its dimension). Prefer explicit handling of unmatched rows (route to an errors table) over silent inner-join loss.

### 9. Freshness failure — stale data served

**What happens:** The pipeline breaks or falls behind, and consumers keep reading yesterday's (or last week's) data without knowing. Decisions get made on stale numbers.
**Why it stays green:** Nothing is failing *now*; the failure already happened or the job simply hasn't run.
**The gate:** **Freshness SLAs per dataset** — "this table must be updated by 06:00 with data through T-1" — monitored independently of job status, alerting the dataset **owner**. Surface last-updated timestamps to consumers.

### 10. Backfill overwrites good data

**What happens:** A backfill meant for one broken week runs with the wrong date window or wrong logic version and tramples months of correct history. The job, of course, is green.
**Why it stays green:** Overwriting is what backfills do.
**The gate:** Backfill as a **designed, parameterized capability** with dry-run counts ("this will rewrite partitions X–Y, ~N rows — confirm"), immutable raw layer to recover from, and post-backfill reconciliation against source.

### 11. Encoding / escaping / delimiter corruption

**What happens:** Data containing quotes, commas, newlines, or non-UTF8 characters breaks a CSV/ndjson boundary. Rows shift columns; values land in the wrong fields. Files parse "successfully" — into nonsense.
**Why it stays green:** The file is still syntactically readable; it's semantically scrambled.
**The gate:** Prefer **typed formats** (Parquet/Avro) over delimited text at internal boundaries. Where text formats are required (like an ndjson handoff), add a **format assertion step** — parse and validate a sample (or all) of the output before declaring success. This is the second gate that would have caught scenario #1.

### 12. Semantic drift — right pipeline, wrong meaning

**What happens:** A business definition changes (what counts as an "active" customer, a status code's meaning, a timezone convention) and the pipeline keeps computing the old logic. Every technical check passes; the numbers are subtly wrong for months.
**Why it stays green:** Nothing technical broke. This is the hardest class.
**The gate:** **Owned metric definitions** (a semantic layer or metrics doc with named owners), change notifications from source-system teams into the data team's intake, and periodic **reconciliation against an independent source of truth** (finance numbers, the operational system's own reports).

---

## The four gates, summarized

Nearly everything above collapses into four investments:

| Gate | What it catches | Scenarios |
|------|-----------------|-----------|
| **Contract tests on interfaces** (schemas, formats, APIs between producer and consumer) | Format and schema breaks before prod | 1, 7, 11 |
| **Blocking data-quality checks** (volume bands, uniqueness, not-null, referential integrity, freshness) | Empty, partial, duplicate, dropped, stale | 3, 4, 6, 8, 9 |
| **Reconciliation against source of truth** (counts, checksums, key measures) | Anything that slipped past the first two | 4, 10, 12 |
| **Idempotent, stage-tested, loudly-failing changes** (no silent catches, stage parity, dry-run backfills) | Human and process failures | 2, 5, 6, 10 |

---

## Pipeline-change PR checklist

For any change to a pipeline, SP, or transformation — the reviewer asks:

- [ ] **Who consumes this output?** Lineage checked; downstream owners notified if the shape changes
- [ ] **Ran in stage** against representative data — not just "compiles"
- [ ] **Data verified, not just job status** — evidence in the PR (counts, sample output, DQ results)
- [ ] Output **format asserted** (contract test updated if the interface changed)
- [ ] **Re-run safe?** Idempotency confirmed for the changed step
- [ ] **Rollback path** identified (previous SP version / partition restore)
- [ ] Failure is **loud** — if this change breaks, will the pipeline actually go red?

> The last question is the one this catalog exists for. A pipeline that can fail green is a defect in itself — every incident where "it showed green" is the system lying, and the fix belongs in the system, not in a promise to be more careful.

> See also: [Data engineering archetype](../archetypes/data-engineering.md) · [Event-driven / streaming](../archetypes/event-driven-streaming.md) · [Testing strategy](../cross-cutting/testing-strategy.md) · [Reliability & observability](../cross-cutting/reliability-and-observability.md)

[← Back to index](../README.md)
