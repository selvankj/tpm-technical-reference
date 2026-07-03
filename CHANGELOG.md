# Changelog

Notable changes to the TPM Technical Reference. Newest first.

The guide uses a simple review convention: each page carries a
`_Last reviewed: YYYY-MM-DD · Review cadence: quarterly_` line under its title.
When you review a page and it's still accurate, bump the date. When you change
content, add an entry here.

## 2026-07-02

**Added**
- **MkDocs Material site** (`mkdocs.yml` + `.github/workflows/docs.yml`) — renders the existing markdown as a searchable, navigable site deployed to GitHub Pages. No content duplication.
- **Scale notes** on every archetype: full "Scale tiers" tables (small / mid-large / hyperscale) on the AWS and Azure cards, and a tailored scale note on each of the others.

- New cross-cutting playbook: **Testing strategy** (test pyramid, non-functional testing, test-data management for regulated environments).
- New archetype: **COTS / vendor-platform implementation** (configuration vs. customization, vendor-as-dependency, adoption).
- Governance: `LICENSE`, `CODEOWNERS`, this `CHANGELOG`, and a per-page *Last reviewed* convention.

**Changed**
- `reference/cloud-service-map.md`: fixed GCP relational entry (**AlloyDB**, was mislabeled); updated **Kinesis Data Analytics → Managed Service for Apache Flink**; reworded the Terraform note.
- `cross-cutting/security-and-compliance.md`: added **HITRUST**, **GAMP 5 / CSA** for GxP, and a note on PCI-DSS / ISO 27001 / FedRAMP.
- `cross-cutting/reliability-and-observability.md`: added **bulkheads** and **chaos engineering / game days**.
- `archetypes/ml-model.md`: fixed a Mermaid subgraph label that could fail to render.
- `archetypes/saas-multitenant.md`: added a nuance note that row-level security is a trade-off, not a free backstop.
- `README.md`: added a "defaults, not dogma" note under the six lenses.

## 2026-07-01

**Added**
- Initial repository: README index, TPM operating model, 9 archetype cards, 4 cross-cutting playbooks, cloud service map, glossary, and `CONTRIBUTING.md`.
- Second pass: `event-driven-streaming` and `infrastructure-platform` archetypes; contributing template.

> Dates are illustrative of the build sequence — adjust to your actual commit history.
