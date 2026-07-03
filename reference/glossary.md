# Reference: Glossary

_Last reviewed: 2026-07-02 · Review cadence: quarterly_

Terms a TPM hears in technical reviews and should be able to use correctly. Grouped by area.

## Architecture & infrastructure

- **Availability Zone (AZ)** — an isolated datacenter within a cloud region. "Multi-AZ" means surviving the loss of one.
- **Region** — a geographic cluster of AZs. Multi-region = surviving the loss of a whole region (and often data-residency control).
- **IaC (Infrastructure as Code)** — infrastructure defined in versioned files (Terraform, Bicep, CloudFormation) so it's reproducible. The opposite is "ClickOps."
- **ClickOps** — building infra by hand in the console. A reproducibility/reliability smell.
- **Blast radius** — how much breaks when one component fails. You want it small.
- **SPOF (Single Point of Failure)** — a component whose failure takes the system down.
- **Horizontal vs. vertical scaling** — add more machines (horizontal) vs. a bigger machine (vertical).
- **Stateless vs. stateful** — stateless components hold no data between requests (easy to scale); stateful ones (databases) are the hard part.

## Reliability & operations

- **SLI / SLO / SLA** — measured indicator / internal target / external promise. See [reliability](../cross-cutting/reliability-and-observability.md).
- **Error budget** — the allowed amount of failure (100% − SLO). Governs ship-vs-harden decisions.
- **RTO / RPO** — how fast you recover / how much data you can lose. Drive DR design.
- **DR (Disaster Recovery) / BCP** — recovering from a major failure / keeping the business running through it.
- **Observability** — logs, metrics, traces; the ability to understand system state from the outside.
- **Idempotent** — doing an operation twice has the same effect as once. Essential for safe retries.
- **Circuit breaker** — stops calling a failing dependency to prevent cascading failure.
- **Graceful degradation** — doing something useful when a dependency is down, instead of fully failing.
- **Runbook** — step-by-step guide for handling a specific operational situation.
- **Toil** — repetitive manual ops work that should be automated.

## Delivery & change

- **CI / CD** — automated build+test / automated delivery to environments.
- **Blue/green, canary, rolling, feature flags** — release strategies. See [CI/CD](../cross-cutting/cicd-and-environments.md).
- **Rollback** — reverting to the previous known-good version.
- **ADR (Architecture Decision Record)** — a short doc capturing one significant decision and its trade-offs.
- **Tech debt** — shortcuts taken for speed that cost more to live with later. Track it; don't pretend it's free.

## Data

- **OLTP vs. OLAP** — transactional systems (run the app) vs. analytical systems (analyze data).
- **ETL vs. ELT** — transform-then-load vs. load-then-transform (the modern lakehouse default).
- **Medallion (bronze/silver/gold)** — layered data architecture: raw → cleaned → business-ready.
- **Idempotent pipeline / backfill** — re-runnable without corruption / reprocessing historical data.
- **Data lineage** — the traceable path of data from source to consumption.
- **Schema drift** — source structure changing and silently breaking downstream.
- **Data contract** — an agreed, versioned schema between a data producer and consumer.
- **Reconciliation** — proving the data in one system matches the source of truth.

## Security & compliance

- **Least privilege** — granting only the access strictly needed.
- **IAM** — identity and access management.
- **Secrets** — credentials/keys/tokens. Belong in a vault, never in code.
- **Encryption in transit / at rest** — protecting data on the wire / on disk.
- **PII / PHI** — personally identifiable information / protected health information.
- **HIPAA / GxP / SOC 2 / GDPR / DPDP** — compliance regimes. See [security & compliance](../cross-cutting/security-and-compliance.md).
- **BAA / DPA** — Business Associate Agreement (HIPAA) / Data Processing Agreement (GDPR) with a vendor handling regulated data.
- **CSV (Computer System Validation)** — documented proof a regulated system does what it's specified to do (GxP).
- **ALCOA+** — data-integrity principles for regulated data (Attributable, Legible, Contemporaneous, Original, Accurate, +more).
- **Pen test** — authorized simulated attack to find vulnerabilities.

## ML / GenAI

- **RAG (Retrieval-Augmented Generation)** — retrieve relevant context, then have an LLM generate using it.
- **Embedding / vector DB** — numeric representation of meaning / a store you search by similarity.
- **Eval / golden set** — a fixed test set used to measure model quality on every change.
- **Hallucination** — a model producing confident but wrong output.
- **Prompt injection** — malicious input that overrides the model's instructions.
- **Drift** — the world moving away from training data, degrading a model over time.
- **Train/serve skew** — features computed differently in training vs. production, hurting accuracy.
- **Data leakage** — information that wouldn't exist at prediction time leaking into training, inflating offline scores.

## Multi-tenancy & cost

- **Tenant** — one customer organization in a shared SaaS.
- **Pooled / siloed tenancy** — shared resources with logical isolation / separate resources per tenant.
- **Noisy neighbor** — one tenant's heavy usage degrading others.
- **Row-level security (RLS)** — database-enforced per-row access control.
- **FinOps** — the practice of managing cloud cost as a team discipline.
- **Egress** — data leaving a zone/region/cloud; a common hidden cost.
- **Reserved / spot / on-demand** — cloud pricing models. See [FinOps](../cross-cutting/finops-cost.md).

[← Back to index](../README.md)
