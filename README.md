# TPM Technical Reference

_Last reviewed: 2026-07-02 · Review cadence: quarterly_

📖 **Read it as a site:** [selvankj.github.io/tpm-technical-reference](https://selvankj.github.io/tpm-technical-reference/) (searchable, with navigation)

A ready reference for Technical Program Managers who get handed a project and need to know **what to look for and what to ask** — not how to build it.

> **Core principle:** A TPM does not design the system. A TPM ensures the system gets *designed, reviewed, de-risked, and delivered*. Every page here is oriented toward that job.

Each page is **layered**: a TL;DR you can read in 60 seconds, then the detail when you need to go deeper.

---

## How to use this

1. **New to a project type?** Open the matching [archetype card](#archetypes). Read the TL;DR, skim the reference architecture, then take the question bank into your first design review.
2. **Sitting in an architecture or readiness review?** Jump straight to the **TPM Question Bank** and **Red Flags** sections of the relevant card.
3. **Any project, any phase?** The [cross-cutting playbooks](#cross-cutting-playbooks) and the [operating model](TPM-OPERATING-MODEL.md) apply everywhere.

---

## The six lenses

Apply these to *anything* — an AWS app, a data pipeline, a mobile app, a migration. If you can give a confident answer on all six, you understand the project well enough to run it.

| Lens | The question you're really asking |
|------|-----------------------------------|
| **Scope & scale** | What is it, who uses it, how much load, and how does that grow? |
| **Security** | Who can reach what, and is sensitive data protected in transit and at rest? |
| **Compliance** | What regulatory/contractual obligations bind this, and where's the evidence? |
| **Reliability** | What's the promised uptime, and what happens when a piece fails? |
| **Cost** | What drives the bill, and is spend proportional to value? |
| **Operability** | When it breaks at 2am, who knows, how, and how fast can they fix it? |

> **These are defaults, not dogma.** The green/red flags throughout this guide describe what's usually right for a production system that matters. Context overrides them — a throwaway internal tool may legitimately be single-AZ and manually deployed. The skill is knowing *why* a default exists so you can tell a reasoned exception from a corner being cut.

---

## Generic architecture-review question bank

Works for any system. Pull the archetype-specific banks on top of this.

**Boundaries & data flow**
- Walk me through a single request, end to end. Where does it enter, what does it touch, where does it leave?
- What's the trust boundary? What's exposed to the public internet vs. internal-only?
- Where does sensitive data live, and how is it classified?

**Failure & scale**
- What's the single biggest bottleneck, and what happens when it's saturated?
- Which components are single points of failure? What's the blast radius if each one dies?
- How does this behave at 10× today's load? What breaks first?

**Change & operations**
- How does a code change get from a developer's laptop to production? How long does that take?
- How would we roll back a bad release? Have we tested that?
- How do we know it's healthy *right now* without asking a human?

**Dependencies & risk**
- What does this depend on that we don't control (third-party APIs, vendor SLAs, another team's service)?
- What's the riskiest assumption in this design, and how do we validate it early?
- What did you *decide against*, and why? (Surfaces the trade-offs that matter.)

---

## Archetypes

Each card follows the same template: **TL;DR → What it is → Reference architecture → Components → Green flags → Red flags → TPM question bank → Key risks → Launch checklist.**

| Card | Use when you're overseeing… |
|------|-----------------------------|
| [AWS application](archetypes/aws-application.md) | A web/API app or service hosted on AWS |
| [Azure application](archetypes/azure-application.md) | A web/API app or service hosted on Azure |
| [Data engineering](archetypes/data-engineering.md) | Ingestion → lakehouse/warehouse → transformation → consumption |
| [GenAI / LLM app](archetypes/genai-llm.md) | RAG, agents, or LLM-backed product features |
| [ML model project](archetypes/ml-model.md) | Train → evaluate → serve → monitor a predictive model |
| [Migration / modernization](archetypes/migration-modernization.md) | Moving or re-architecting an existing system |
| [Integration / API program](archetypes/integration-api.md) | Connecting systems point-to-point, B2B/EDI |
| [Event-driven / streaming](archetypes/event-driven-streaming.md) | High-throughput event streams, real-time/stream processing |
| [Mobile app](archetypes/mobile-app.md) | iOS/Android or cross-platform client + backend |
| [SaaS multi-tenant](archetypes/saas-multitenant.md) | A product serving many customer organizations |
| [Infrastructure / platform](archetypes/infrastructure-platform.md) | Foundations/internal platform whose users are other teams |
| [COTS / vendor implementation](archetypes/cots-vendor-implementation.md) | Configuring a vendor product (eClinical, CRM, ERP) rather than building |

---

## Cross-cutting playbooks

Apply across every archetype.

| Playbook | Covers |
|----------|--------|
| [Security & compliance](cross-cutting/security-and-compliance.md) | IAM, secrets, encryption, HIPAA / GxP / HITRUST / SOC 2 / GDPR / DPDP |
| [Reliability & observability](cross-cutting/reliability-and-observability.md) | SLA/SLO/SLI, error budgets, DR/RTO/RPO, chaos, logs/metrics/traces |
| [Testing strategy](cross-cutting/testing-strategy.md) | Test pyramid, non-functional testing, test-data management |
| [FinOps & cost](cross-cutting/finops-cost.md) | Cost drivers per service class, how to challenge a bill |
| [CI/CD & environments](cross-cutting/cicd-and-environments.md) | Pipelines, environments, release & rollback strategy |

---

## Reference

| Page | Contents |
|------|----------|
| [Cloud service map](reference/cloud-service-map.md) | AWS ↔ Azure ↔ GCP equivalents by category |
| [Glossary](reference/glossary.md) | Terms a TPM hears and should be able to use |
| [Data failure scenarios](reference/data-failure-scenarios.md) | 12 silent-failure modes and the gates that prevent them |
| [Operating model](TPM-OPERATING-MODEL.md) | Phase map, RAID, ADRs, launch-readiness review, escalation |

---

## Maintaining this guide

Each page carries a `_Last reviewed_` date under its title. Cloud names, compliance rules, and default patterns drift — so pages are reviewed on a **quarterly cadence**; bump the date when you confirm a page is still accurate, and log content changes in [CHANGELOG.md](CHANGELOG.md). The [cloud service map](reference/cloud-service-map.md) drifts fastest and deserves the closest eye. Ownership is defined in [CODEOWNERS](https://github.com/selvankj/tpm-technical-reference/blob/main/CODEOWNERS).

---

*Contributions welcome — see [CONTRIBUTING.md](CONTRIBUTING.md) for the card template and house style. Keep the layered format (TL;DR first), keep it about **what to ask**, not how to build.*
