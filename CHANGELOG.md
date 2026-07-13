# Changelog

Notable changes to the TPM Technical Reference. Newest first.

The guide uses a simple review convention: each page carries a
`_Last reviewed: YYYY-MM-DD · Review cadence: quarterly_` line under its title.
When you review a page and it's still accurate, bump the date. When you change
content, add an entry here.

## 2026-07-03

**Added**
- ADR section: reversal-cost rule of thumb and two more ADR-worthy decision examples (service boundaries, event-driven vs. request/response).
- **About page**, **Templates section** (RAID log, launch-readiness, cut-over runbook, pipeline-change PR checklist), and the **"From the field"** convention for real anonymized experience callouts — first one on the data failure scenarios page.
- Positioning tagline on homepage and README (curated personal reference, not an official standard); license changed from internal-only to **CC BY 4.0** for public sharing.
- New reference page: **Data pipeline failure scenarios** — 12 silent-failure modes ("green but broken"), the four prevention gates, and a pipeline-change PR checklist.

## 2026-07-02

**Added**
- **Hero landing page** (`HOME.md`, site-only): gradient hero, clickable card grids for all archetypes and playbooks, six-lenses summary. CI stages it as the site's `index.md`; `README.md` remains the GitHub-facing page, untouched.
- **Visual theme** (`stylesheets/extra.css`): gradient palette, TL;DR callout cards with pill badges, styled tables, top nav tabs, emoji nav icons, card hover effects, dark-mode variants. Fixed blockquote bullet rendering across 19 files.
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
