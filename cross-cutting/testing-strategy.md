# Cross-Cutting: Testing Strategy

_Last reviewed: 2026-07-02 · Review cadence: quarterly_

How a team proves the system works — before users do. A TPM doesn't write tests, but should be able to tell a real testing strategy from "we clicked around and it seemed fine," and should know that in regulated environments testing *is* part of the compliance evidence.

> **TL;DR**
>
> - Aim for the **test pyramid**: many fast unit tests, fewer integration tests, a thin layer of end-to-end tests. An inverted pyramid (mostly slow, flaky E2E) is a smell.
> - **Functional testing isn't enough.** Ask about the non-functional kinds — **performance/load, security, resilience/chaos** — because that's where launches actually fail.
> - The one that bites regulated teams: **test-data management.** No real PHI/PII in lower environments; use synthetic or masked data.

---

## The test pyramid

| Layer | What it checks | Speed / cost | Should be |
|-------|----------------|--------------|-----------|
| **Unit** | One function/module in isolation | Fast, cheap | The bulk of your tests |
| **Integration** | Components working together (service ↔ DB ↔ queue) | Medium | A solid middle layer |
| **End-to-end (E2E)** | A full user journey through the whole system | Slow, brittle | A thin, high-value layer |

> **Inverted pyramid = trouble.** If most confidence comes from slow, flaky E2E tests, the suite is expensive to run, painful to maintain, and gets ignored when it goes red. You want fast feedback low in the stack.

---

## Beyond functional — the testing that catches launch failures

Functional tests confirm the system does what it should. These confirm it holds up in the real world:

| Type | Answers | When a TPM asks |
|------|---------|-----------------|
| **Performance / load** | Does it hold up at expected (and peak) traffic? | Before any launch with real scale |
| **Stress / soak** | Where does it break, and does it leak/degrade over hours? | For always-on / high-availability systems |
| **Security testing** | SAST/DAST, dependency scanning, pen test | Before launch; continuously in CI |
| **Resilience / chaos** | Does it survive a failed dependency/zone? | For anything with real reliability targets (see [reliability](reliability-and-observability.md)) |
| **Accessibility** | Is it usable with assistive tech (WCAG)? | Public-facing / regulated UIs |
| **Regression** | Did this change break something that worked? | Automated, every release |
| **UAT** | Do actual users agree it meets the need? | Before sign-off |

---

## Test-data management (the regulated-environment trap)

Where does the data used for testing come from? This is easy to get catastrophically wrong.

- **No production PHI/PII in lower environments.** Copying prod data into dev/test/staging is a common, serious breach vector — and a compliance violation under [HIPAA/GDPR/DPDP](security-and-compliance.md).
- Use **synthetic data** (generated to look real) or **masked/anonymized** data instead.
- Test data must still be **representative** — realistic volumes, edge cases, and messy inputs — or tests pass on tidy data and reality breaks them.
- In **GxP** contexts, test evidence is part of validation; test data and results may need to be controlled and retained.

---

## Automation & where testing lives

- Tests belong in the **[CI/CD pipeline](cicd-and-environments.md)** and should **gate** it — a red build blocks the merge/deploy, not just warns.
- **Flaky tests are a liability**, not a nuisance: a suite people don't trust is a suite people ignore. Quarantine and fix flakiness deliberately.
- **Coverage is a signal, not a target.** High coverage of trivial code proves little; chasing a coverage number invents low-value tests. Ask what's tested, not just how much.
- For [ML](../archetypes/ml-model.md)/[GenAI](../archetypes/genai-llm.md), "testing" also means **evaluation harnesses** on a golden set — a different discipline, same principle: measure quality automatically on every change.

---

## Green flags

- A pyramid-shaped suite: fast tests carry most of the load.
- Non-functional testing (perf, security, resilience) is planned, not an afterthought.
- Tests **gate** CI; the team doesn't merge past red.
- Test data is synthetic/masked — no prod PHI/PII in lower environments.
- Flakiness is actively managed.

## Red flags / anti-patterns

- "**We tested it manually**" as the whole strategy.
- **Inverted pyramid** — mostly slow, flaky E2E tests.
- **No performance testing** before a launch that will see real load.
- **Prod data copied** into dev/test (breach + compliance risk).
- Tests are **advisory**, not gating — red builds get merged anyway.
- **Coverage theater** — a number chased with meaningless tests.

---

## TPM question bank

- What does the **test pyramid** look like here — where does our confidence actually come from?
- Have we done **performance/load testing** at expected and peak volume? What broke first?
- What **security testing** runs — SAST/DAST, dependency scanning, pen test — and how often?
- Where does **test data** come from? Is any real PHI/PII in non-prod environments?
- Do tests **gate** the pipeline, or are they advisory?
- How **flaky** is the suite, and who owns fixing that?
- For ML/GenAI: what's the **evaluation harness**, and what's the quality bar?

[← Back to index](../README.md)
