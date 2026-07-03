# Cross-Cutting: Reliability & Observability

_Last reviewed: 2026-07-02 · Review cadence: quarterly_

**Reliability** is the promise you make about uptime and correctness. **Observability** is how you know whether you're keeping it. Applies to every archetype.

> **TL;DR**
>
> - Define **SLOs** (the target) backed by **SLIs** (the measurement), and let the **error budget** decide when to slow down and harden vs. ship features.
> - Know your **RTO and RPO** (how fast you recover, how much data you can lose) and confirm a **restore has actually been tested** — backups you've never restored are a guess.
> - Demand the **three pillars** before launch: **logs, metrics, traces**, plus alerts that page a human on the symptoms users feel — not on every twitch.

---

## SLA / SLO / SLI — and error budgets

| Term | What it is | Example |
|------|------------|---------|
| **SLI** (indicator) | The actual measurement | % of requests served < 300ms; % of successful requests |
| **SLO** (objective) | Your internal target for the SLI | 99.9% of requests succeed over 30 days |
| **SLA** (agreement) | The external/contractual promise (with penalties) | 99.5% uptime or the customer gets credits |
| **Error budget** | 100% − SLO; how much failure you're *allowed* | 99.9% SLO ⇒ ~43 min/month of budget |

> The **error budget** is the most useful idea here. Budget left → you can take risks and ship. Budget burning fast → stop shipping features and fix reliability. It turns "are we reliable enough?" from an argument into a number.

**TPM use:** make sure SLOs exist and are tied to **what users actually feel** (success rate, latency), not vanity metrics (CPU%). Set the SLA *below* the SLO so you have margin before you owe penalties.

---

## DR: RTO & RPO

| Term | Question it answers | Set by |
|------|---------------------|--------|
| **RTO** (Recovery Time Objective) | How long can we be **down**? | Business tolerance for outage |
| **RPO** (Recovery Point Objective) | How much **data** can we lose? | Business tolerance for data loss |

These two numbers drive the entire DR design. Tight RPO → frequent backups / replication. Tight RTO → warm standby or multi-region, which costs more. The job is to make the cost/tightness trade-off **explicit and agreed**, not assumed.

> **The backup that's never been restored doesn't exist.** Insist on a **tested restore**. "We have backups" is not the same as "we have proven we can recover."

**Prove it, don't assume it.** The same logic extends to the whole DR/resilience story. **Chaos engineering** (deliberately injecting failure — killing an instance, a zone, a dependency) and scheduled **game days** (a rehearsed failure drill with the on-call team) turn "we think we'd recover" into evidence. A DR plan that's never been exercised is a document, not a capability. As a TPM, ask when the last game day was and what it found.

---

## Observability — the three pillars

| Pillar | Answers | Without it |
|--------|---------|------------|
| **Logs** | "What exactly happened?" | You're guessing during an incident |
| **Metrics** | "Is it healthy right now? Trending how?" | You don't know there's a problem until users tell you |
| **Traces** | "Where in the request did time/failure go?" | You can't find the bottleneck across services |

Add to these:
- **Dashboards** the team actually watches.
- **Alerting** on **symptoms users feel** (error rate, latency, queue age), routed to whoever's on call. Alert on the few things that mean "users are hurting," not on every metric — alert fatigue is a reliability risk in itself.
- **On-call + runbooks** so the alert leads to action, not panic.

---

## Resilience patterns to listen for

- **Redundancy / multi-AZ** — no single point of failure for stateful tiers.
- **Retries with backoff** + **idempotency** — transient failures self-heal without duplicating work.
- **Circuit breakers / timeouts** — a slow dependency doesn't cascade.
- **Bulkheads** — isolate resources (thread pools, connection pools, partitions) so one failing dependency or one heavy tenant can't exhaust the shared pool and sink everything else. The ship metaphor: a breach floods one compartment, not the whole hull.
- **Graceful degradation** — the system does *something* useful when a dependency is down, rather than fully failing.
- **Rate limiting / load shedding** — under overload, drop excess rather than collapse.

---

## TPM question bank (reliability & observability)

- What are the **SLOs**, and are they tied to what users actually experience?
- Is there an **SLA** with a customer? Does our SLO give margin under it?
- What are the **RTO and RPO**? Does the DR design actually meet them?
- When did we last **test a restore** from backup?
- When did we last run a **game day / chaos test**, and what did it surface?
- Do we have **logs, metrics, and traces**? Show me the dashboard.
- What **pages someone**, and is it the symptoms users feel — or noise?
- Who's **on call**, and are there **runbooks** for the top failure modes?
- What are the **single points of failure**, and what's the blast radius of each?
- How does the system behave under **overload** — degrade gracefully or fall over?

[← Back to index](../README.md)
