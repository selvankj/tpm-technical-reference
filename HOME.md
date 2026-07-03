---
hide:
  - navigation
  - toc
---

<!-- This file is the SITE homepage only. CI renames it to index.md during staging.
     README.md remains the GitHub-facing page. -->

<div class="ttr-hero" markdown>

# TPM Technical Reference

**Handed a project you've never run before?** Find your project type below — each card tells you what to look for, what to ask, and what "good" looks like. Not how to build it.

[:material-compass: Start with the operating model](TPM-OPERATING-MODEL.md){ .md-button .md-button--primary }
[:material-github: View on GitHub](https://github.com/selvankj/tpm-technical-reference){ .md-button }

<div class="ttr-stats" markdown>
**12** archetypes · **5** cross-cutting playbooks · **1** cloud service map · reviewed **quarterly**
</div>

</div>

## :material-shape-outline: Pick your project type

<div class="grid cards" markdown>

-   :material-cloud:{ .lg .middle } **AWS application**

    ---

    Web/API app on AWS — HA architecture, IAM, rollback, scale tiers

    [:octicons-arrow-right-24: Open the card](archetypes/aws-application.md)

-   :material-microsoft-azure:{ .lg .middle } **Azure application**

    ---

    Same shape on Azure — zones, Managed Identity, Key Vault

    [:octicons-arrow-right-24: Open the card](archetypes/azure-application.md)

-   :material-database:{ .lg .middle } **Data engineering**

    ---

    Ingest → lakehouse → serve; idempotency, quality gates, lineage

    [:octicons-arrow-right-24: Open the card](archetypes/data-engineering.md)

-   :material-creation:{ .lg .middle } **GenAI / LLM app**

    ---

    RAG, agents, evals, guardrails, cost-per-request

    [:octicons-arrow-right-24: Open the card](archetypes/genai-llm.md)

-   :material-robot:{ .lg .middle } **ML model**

    ---

    Train → evaluate → serve → monitor; drift, leakage, retraining

    [:octicons-arrow-right-24: Open the card](archetypes/ml-model.md)

-   :material-truck:{ .lg .middle } **Migration / modernization**

    ---

    The 6 Rs, cut-over runbooks, reconciliation, rollback

    [:octicons-arrow-right-24: Open the card](archetypes/migration-modernization.md)

-   :material-swap-horizontal:{ .lg .middle } **Integration / API**

    ---

    Sync vs async, contracts, idempotency, dead-letter queues

    [:octicons-arrow-right-24: Open the card](archetypes/integration-api.md)

-   :material-waves:{ .lg .middle } **Event-driven / streaming**

    ---

    Delivery guarantees, ordering, replay, consumer lag

    [:octicons-arrow-right-24: Open the card](archetypes/event-driven-streaming.md)

-   :material-cellphone:{ .lg .middle } **Mobile app**

    ---

    Version fragmentation, offline, store review, staged rollout

    [:octicons-arrow-right-24: Open the card](archetypes/mobile-app.md)

-   :material-office-building:{ .lg .middle } **SaaS multi-tenant**

    ---

    Tenant isolation, noisy neighbors, per-tenant everything

    [:octicons-arrow-right-24: Open the card](archetypes/saas-multitenant.md)

-   :material-server:{ .lg .middle } **Infrastructure / platform**

    ---

    Golden paths, guardrails-as-code, adoption over output

    [:octicons-arrow-right-24: Open the card](archetypes/infrastructure-platform.md)

-   :material-package-variant:{ .lg .middle } **COTS / vendor implementation**

    ---

    Configuration vs customization, vendor-as-dependency, adoption

    [:octicons-arrow-right-24: Open the card](archetypes/cots-vendor-implementation.md)

</div>

## :material-layers-outline: Applies to every project

<div class="grid cards" markdown>

-   :material-shield-lock:{ .lg .middle } **Security & compliance**

    ---

    IAM, secrets, HIPAA / GxP / HITRUST / SOC 2 / GDPR / DPDP

    [:octicons-arrow-right-24: Open](cross-cutting/security-and-compliance.md)

-   :material-chart-line:{ .lg .middle } **Reliability & observability**

    ---

    SLOs, error budgets, RTO/RPO, chaos, the three pillars

    [:octicons-arrow-right-24: Open](cross-cutting/reliability-and-observability.md)

-   :material-test-tube:{ .lg .middle } **Testing strategy**

    ---

    Test pyramid, non-functional testing, test-data management

    [:octicons-arrow-right-24: Open](cross-cutting/testing-strategy.md)

-   :material-currency-usd:{ .lg .middle } **FinOps & cost**

    ---

    Where the money goes and how to challenge a bill

    [:octicons-arrow-right-24: Open](cross-cutting/finops-cost.md)

-   :material-sync:{ .lg .middle } **CI/CD & environments**

    ---

    Pipelines, boring deploys, tested rollback

    [:octicons-arrow-right-24: Open](cross-cutting/cicd-and-environments.md)

-   :material-map:{ .lg .middle } **Cloud service map**

    ---

    AWS ↔ Azure ↔ GCP equivalents, side by side

    [:octicons-arrow-right-24: Open](reference/cloud-service-map.md)

</div>

## :material-glasses: The six lenses

Apply these to *anything*. If you can answer all six with confidence, you understand the project well enough to run it.

| Lens | The question you're really asking |
|------|-----------------------------------|
| **Scope & scale** | What is it, who uses it, how much load, and how does that grow? |
| **Security** | Who can reach what, and is sensitive data protected in transit and at rest? |
| **Compliance** | What regulatory obligations bind this, and where's the evidence? |
| **Reliability** | What's the promised uptime, and what happens when a piece fails? |
| **Cost** | What drives the bill, and is spend proportional to value? |
| **Operability** | When it breaks at 2am, who knows, how, and how fast can they fix it? |

> **Defaults, not dogma.** The green/red flags in this guide describe what's usually right for a production system that matters. Context overrides them — the skill is knowing *why* a default exists so you can tell a reasoned exception from a corner being cut.

---

:material-book-open-variant: [Glossary](reference/glossary.md) · :material-handshake: [Contributing](CONTRIBUTING.md) · :material-note-text: [Changelog](CHANGELOG.md) · :material-compass: [Operating model](TPM-OPERATING-MODEL.md)
