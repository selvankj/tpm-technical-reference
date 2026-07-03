# TPM Operating Model

_Last reviewed: 2026-07-02 · Review cadence: quarterly_

How a TPM runs technical delivery, regardless of the project type. The [archetype cards](README.md#archetypes) tell you *what* to inspect; this tells you *when* and *how*.

> **TL;DR**
>
> - Your job shifts by phase: in **discovery** you de-risk assumptions, in **design** you challenge the architecture, in **build** you track flow and unblock, in **hardening** you drive readiness, at **launch** you own the go/no-go, in **run** you watch the SLOs.
> - Three artifacts carry most of the weight: a live **RAID log**, a short stack of **ADRs**, and a **launch-readiness checklist**.
> - Escalate on **trajectory**, not on the day something is already on fire.

---

## Phase map

| Phase | Your primary job | What "done" looks like |
|-------|------------------|------------------------|
| **Discovery** | Pin down scope, users, constraints, and the *riskiest assumptions*. Force a spike on anything unproven. | Problem, success metrics, and non-goals written down and agreed. Top risks have validation plans. |
| **Design** | Get a real architecture review. Apply the [six lenses](README.md#the-six-lenses). Make trade-offs explicit via ADRs. | Architecture documented, reviewed, and signed off. Security/compliance looped in *before* build. |
| **Build** | Track flow, not activity. Manage dependencies and cross-team handoffs. Keep the RAID live. | Vertical slices shipping to a non-prod environment. Demos against real acceptance criteria. |
| **Hardening** | Drive the readiness checklist: perf, security, DR, runbooks, observability. | Load/security tests passed, runbooks exist, dashboards and alerts live, rollback tested. |
| **Launch** | Own the go/no-go. Confirm rollback, comms, support coverage, and the success/abort criteria. | Decision made on evidence. Everyone knows the plan, the owners, and the abort triggers. |
| **Run** | Watch SLOs and the error budget. Run the retro. Track tech debt taken on under deadline. | Steady state, incidents handled by the on-call rotation, debt logged and prioritized. |

---

## RAID log

The TPM's core instrument. Review it weekly; it should never go stale.

- **Risks** — might happen, would hurt. Each has an owner, a likelihood/impact, and a *mitigation or trigger*. A risk with no plan is just anxiety.
- **Assumptions** — things you're treating as true but haven't proven. The dangerous ones are unstated. Surface them and assign validation.
- **Issues** — already happening. Owner + path to resolution + date.
- **Dependencies** — what you need from whom by when, especially cross-team and third-party. Map these early; they cause most slips.

> **Dependency mapping tip:** for each external dependency, record *who owns it, what you need, the date you need it, and what you'll do if it's late.* The last column is what separates a plan from a wish.

---

## ADRs (Architecture Decision Records)

Short documents that capture *one significant decision*: the context, the options, the choice, and the consequences. You don't write them — engineers and architects do — but you should **insist they exist** for anything load-bearing.

Why a TPM cares:
- They make trade-offs reviewable instead of buried in someone's head.
- They're your defense when a decision is questioned six months later.
- A team that can't articulate *why not the alternative* hasn't finished thinking.

Ask for an ADR whenever a choice is expensive to reverse: datastore, cloud, auth model, sync vs. async, build vs. buy, tenancy model.

---

## Launch-readiness review (go / no-go)

Run this as a structured gate, not a vibe check. Generic checklist — layer the archetype-specific launch checklist on top.

**Functionality**
- [ ] Acceptance criteria met and demonstrated against real data
- [ ] Known defects triaged; no open blockers/criticals

**Reliability & ops**
- [ ] SLOs defined; dashboards and alerts live and pointing at the right people
- [ ] Runbooks exist for the top failure modes
- [ ] Rollback path tested, not just documented
- [ ] On-call / support coverage confirmed for the launch window

**Security & compliance**
- [ ] Security review / pen test findings closed or risk-accepted in writing
- [ ] Required compliance evidence captured (see [security & compliance](cross-cutting/security-and-compliance.md))
- [ ] Secrets managed properly; no credentials in code/config

**Data & DR**
- [ ] Backups verified by an actual restore test
- [ ] RTO/RPO defined and achievable
- [ ] Data migration (if any) validated with reconciliation

**Comms & decision**
- [ ] Stakeholders know the date, the plan, and the owners
- [ ] **Abort criteria defined** — what specifically makes us roll back?
- [ ] Go/no-go owner and decision time set

> A launch with no pre-agreed **abort criteria** isn't a launch, it's a gamble. Decide what "bad enough to roll back" means *before* you're emotionally invested in shipping.

---

## Status & escalation

**Status that's actually useful** answers three things: are we on track (and how confident), what changed since last time, and what decision/help is needed *now*. Red/amber/green is fine — but an amber with no ask is noise.

**Escalate on trajectory.** The goal is to escalate while there's still room to act. Good triggers:
- A dependency is going to miss and you can't absorb it.
- A risk's likelihood just jumped and the mitigation needs a decision above your level.
- Scope, date, and resources can no longer all hold — and someone with authority has to choose which one gives.

Frame an escalation as: *situation → impact → options → your recommendation → the decision you need.* Never escalate a problem without at least one proposed path.

---

[← Back to index](README.md)
