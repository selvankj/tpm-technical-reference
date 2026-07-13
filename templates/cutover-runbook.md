# Template: Cut-Over Runbook

_Last reviewed: 2026-07-03 · Review cadence: quarterly_

For migrations and major releases. Guidance: [migration archetype](../archetypes/migration-modernization.md). Fill every owner cell — an unowned step is a stalled cut-over at 2am.

**System:** ______  **Cut-over window:** ______  **Runbook owner:** ______
**Bridge / war-room:** ______  **Rollback decision owner:** ______

## Pre-cut-over (T-1 week → T-0)

| # | Step | Owner | Planned time | Done |
|---|------|-------|--------------|------|
| 1 | Freeze announced to affected teams | | | ☐ |
| 2 | Final data sync / delta captured | | | ☐ |
| 3 | Backups taken and restore-verified | | | ☐ |
| 4 | Go/no-go gate #1: readiness confirmed | | | ☐ |

## Cut-over sequence

| # | Step | Owner | Planned time | Verify by | Done |
|---|------|-------|--------------|-----------|------|
| 5 | | | | | ☐ |
| 6 | | | | | ☐ |
| 7 | Go/no-go gate #2: validation checkpoint | | | | ☐ |

## Validation

| # | Check | Expected result | Owner | Pass |
|---|-------|-----------------|-------|------|
| V1 | Reconciliation: record counts vs. source | Match ±0 | | ☐ |
| V2 | Smoke test: critical user journeys | All pass | | ☐ |
| V3 | Integrations: downstream consumers receiving | Confirmed | | ☐ |

## Rollback plan (pre-agreed)

- **Abort triggers:** ______ (specific, measurable — decide *before* the window)
- **Rollback steps:** ______
- **Time required to roll back:** ______  **Last tested:** ______

## Post-cut-over

- [ ] Hypercare period defined (duration, coverage, exit criteria)
- [ ] Old system decommission criteria and date agreed
- [ ] Retro scheduled

[← Back to index](../README.md)
