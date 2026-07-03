# Cross-Cutting: FinOps & Cost

_Last reviewed: 2026-07-02 · Review cadence: quarterly_

Cloud cost is a delivery concern, not just a finance one — architecture decisions *are* spending decisions. A TPM should be able to read a bill, name the top drivers, and challenge spend that isn't proportional to value.

> **TL;DR**
>
> - A few categories drive most cloud bills: **compute, data transfer (egress), storage, managed databases/warehouses, and idle/forgotten resources.**
> - The cheapest wins are usually **turn off what's unused**, **right-size what's oversized**, and **use commitments** (reserved/savings plans) for steady workloads.
> - The TPM's job: insist on **tagging + cost attribution** so spend has an owner, set **budgets and alerts**, and treat a surprising bill as a question, not a fact.

---

## Where cloud money actually goes

| Driver | Why it surprises people | The lever |
|--------|-------------------------|-----------|
| **Compute** | Oversized or always-on instances; no auto-scaling | Right-size; auto-scale; serverless for spiky; commitments for steady |
| **Data egress / transfer** | Cross-AZ, cross-region, and internet egress are easy to ignore | Keep traffic in-zone/region; cache; the classic "NAT gateway / egress" shock |
| **Storage** | Data only grows; old data sits on hot tiers | Lifecycle policies; tier cold data; delete what's not needed |
| **Managed DB / warehouse** | Always-on clusters; oversized warehouses left running | Auto-suspend; right-size; reserved capacity |
| **Idle / orphaned resources** | Dev environments, unattached disks, old snapshots nobody owns | Tagging + scheduled cleanup; turn off non-prod off-hours |
| **Per-request services** (LLM APIs, serverless) | Cost scales with usage; a spike = a bill | Rate limits; caching; model the cost per request (see [GenAI](../archetypes/genai-llm.md)) |

---

## Pricing models — match the model to the workload

| Model | Best for | Trade-off |
|-------|----------|-----------|
| **On-demand** | Spiky, unpredictable, short-lived | Most expensive per unit |
| **Reserved / Savings Plans / Committed Use** | Steady, predictable baseline | Big discount, but you commit (1–3 yrs) |
| **Spot / low-priority** | Interruptible batch, fault-tolerant work | Cheapest, can be reclaimed anytime |
| **Serverless / consumption** | Variable load, scale-to-zero | Cheap when idle, can get pricey at high steady volume |

> A common smart pattern: **commitments for the steady baseline + on-demand for the peaks + spot for batch.** A flat "all on-demand" bill usually has easy savings.

---

## The FinOps loop

1. **Visibility** — tagging so every resource maps to a team/project/tenant. Without this, nothing else works.
2. **Attribution** — each cost has an owner who sees and answers for it.
3. **Optimization** — right-size, schedule off-hours, tier storage, buy commitments, kill orphans.
4. **Governance** — budgets, alerts, and guardrails (e.g. policies blocking oversized resources).

---

## TPM watch-items

- **Tagging discipline.** No tags → no attribution → no accountability. This is the foundation.
- **Budgets + alerts** set, so a runaway cost surfaces in hours, not at month-end.
- **Non-prod off-hours.** Dev/test environments running 24/7 are pure waste.
- **Commitment coverage** for steady workloads — are we paying on-demand for things that never turn off?
- **Egress awareness** in the architecture — cross-region chatter and NAT traffic are silent budget killers.
- **Per-request cost modeled** for usage-priced services before they scale.

---

## TPM question bank (cost)

- What are the **top three line items** on this bill, and who **owns** each?
- Is everything **tagged**? Can we attribute cost to team/project/tenant?
- Are there **budgets and alerts**? Who gets notified, and at what threshold?
- Are non-prod environments **shut off** outside working hours?
- For steady workloads, are we on **commitments**, or paying full on-demand?
- What's our **data-egress** exposure (cross-AZ/region/internet)?
- For usage-priced services (LLM APIs, serverless), what's the **cost per request** and the projected bill at expected volume?
- This bill jumped — **what changed?** (Treat surprises as a question.)

> See also: [SaaS multi-tenant](../archetypes/saas-multitenant.md) for per-tenant cost attribution · [GenAI / LLM](../archetypes/genai-llm.md) for per-request cost

[← Back to index](../README.md)
