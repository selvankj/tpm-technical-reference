# Template: Launch-Readiness Checklist (Go / No-Go)

_Last reviewed: 2026-07-03 · Review cadence: quarterly_

Run this as a structured gate, not a vibe check. Layer the archetype-specific launch checklist from the relevant [archetype card](../README.md#archetypes) on top. Guidance: [operating model](../TPM-OPERATING-MODEL.md#launch-readiness-review-go--no-go).

**Project:** ______  **Launch window:** ______  **Go/no-go owner:** ______  **Decision time:** ______

## Functionality
- [ ] Acceptance criteria met and demonstrated against real data
- [ ] Known defects triaged; no open blockers/criticals

## Reliability & operations
- [ ] SLOs defined; dashboards and alerts live and routed to on-call
- [ ] Runbooks exist for the top failure modes
- [ ] Rollback path **tested**, not just documented
- [ ] On-call / support coverage confirmed for the launch window

## Security & compliance
- [ ] Security review / pen test findings closed or risk-accepted in writing
- [ ] Required compliance evidence captured
- [ ] Secrets managed properly; no credentials in code/config

## Data & DR
- [ ] Backups verified by an actual restore test
- [ ] RTO/RPO defined and achievable
- [ ] Data migration (if any) validated with reconciliation

## Comms & decision
- [ ] Stakeholders know the date, plan, and owners
- [ ] **Abort criteria defined** — what specifically makes us roll back?
- [ ] Decision recorded: **GO / NO-GO** — by ______ on ______

> A launch with no pre-agreed abort criteria isn't a launch, it's a gamble.

[← Back to index](../README.md)
