# Cross-Cutting: Security & Compliance

_Last reviewed: 2026-07-02 · Review cadence: quarterly_

Applies to every archetype. Security is *how* you protect the system; compliance is *proving* you meet an external obligation. A TPM doesn't run the security program, but owns making sure it's **looped in early** and that **evidence exists**.

> **TL;DR**
>
> - Security basics to verify everywhere: **least-privilege identity**, **secrets in a vault** (never in code), **encryption in transit and at rest**, and a **security review before launch** with findings closed or risk-accepted in writing.
> - Compliance is about **obligations + evidence**. Know which regimes bind your project (**HIPAA, GxP, SOC 2, GDPR, DPDP**), and that compliance work is **continuous and designed-in**, not bolted on at the end.
> - The cardinal sin: discovering security/compliance requirements *during hardening*. They belong in the design phase.

---

## Security essentials (verify on every project)

| Area | What good looks like | The TPM question |
|------|----------------------|------------------|
| **Identity & access** | Least privilege; roles not shared keys; MFA; regular access review | "Who can reach what, and why that much?" |
| **Secrets** | In a vault (Secrets Manager / Key Vault); rotated; never in code/config/repos | "Where do secrets live, and is anything in source control?" |
| **Encryption** | TLS in transit; encryption at rest on data stores & backups | "Is sensitive data encrypted in transit *and* at rest?" |
| **Network** | Private subnets; minimal public exposure; WAF on public edges | "What's reachable from the public internet?" |
| **Data classification** | Sensitive data identified and handled per its class | "Where does our most sensitive data live, and who can see it?" |
| **Vulnerability mgmt** | Dependency scanning, image scanning, patching cadence | "How do we find and fix known vulnerabilities?" |
| **Audit logging** | Who did what, when — tamper-resistant, retained | "Can we reconstruct who accessed what?" |
| **Review before launch** | Security review / pen test; findings closed or risk-accepted | "What did the security review find, and is it closed?" |

> **Shift left.** The cheapest time to fix a security problem is in design. Get security into the [design phase](../TPM-OPERATING-MODEL.md#phase-map), not the launch gate.

---

## Compliance regimes a TPM commonly meets

You don't need to be an auditor — you need to know **which apply**, **what they demand of delivery**, and **where the evidence is**.

### HIPAA (US healthcare — PHI)
- **Applies when:** handling Protected Health Information (patient data).
- **Demands:** access controls, audit logs, encryption, a **Business Associate Agreement (BAA)** with any vendor touching PHI (including cloud providers and any AI model vendor), breach-notification processes.
- **TPM watch-items:** Is there a signed **BAA** with every vendor in the data path? Is PHI access logged and minimized? For [GenAI](../archetypes/genai-llm.md), does PHI leave your boundary to a model, and is that covered?

### GxP / Computer System Validation (life sciences — FDA/EMA regulated)
- **Applies when:** the system affects product quality, safety, or regulatory submissions (pharma, medical devices, clinical).
- **Demands:** **validation** (documented evidence the system does what it's specified to do), traceability from requirement → test, controlled changes, **audit trails**, electronic-records/signatures integrity (**21 CFR Part 11**), and **ALCOA+** data integrity (Attributable, Legible, Contemporaneous, Original, Accurate, +Complete/Consistent/Enduring/Available).
- **Frameworks to know:** **GAMP 5** (industry guidance for a risk-based approach to computerized-system validation) is the reference most life-sciences teams work from. **CSA (Computer Software Assurance)** is the FDA's newer, more risk-based direction that emphasizes critical thinking and testing effort proportional to risk, rather than exhaustive documentation for its own sake — increasingly the modern take on CSV. If a team is still doing heavyweight document-everything validation on low-risk systems, that's a flag worth raising.
- **TPM watch-items:** Is there a **validation plan** and a **requirements-to-test traceability matrix**? Is validation effort **scaled to risk** (GAMP 5 / CSA), or is everything treated as high-risk? Are changes controlled? Validation effort is significant — it must be in the plan from the start, not discovered late.

### SOC 2 (B2B trust)
- **Applies when:** selling SaaS to businesses; effectively table stakes for [multi-tenant SaaS](../archetypes/saas-multitenant.md).
- **Demands:** documented controls across security, availability, confidentiality (and optionally processing integrity, privacy), audited over a period (Type II).
- **TPM watch-items:** Are the controls real and operating, or aspirational? Type II requires evidence **over time**, so processes must run consistently, not just before the audit.

### HITRUST (US healthcare — certifiable security framework)
- **Applies when:** a healthcare vendor needs to *demonstrate* a strong security posture to hospital/payer customers. Effectively near-mandatory for many US healthcare SaaS deals.
- **Demands:** implementation and certification against the HITRUST CSF, which harmonizes HIPAA, NIST, ISO 27001, and others into one auditable control set.
- **TPM watch-items:** Is HITRUST a customer/contractual requirement? Certification is a multi-month effort with evidence collected over time — if it's needed, it belongs in the roadmap early, like SOC 2 Type II.

### GDPR (EU) / DPDP (India) — personal data & privacy
- **Applies when:** processing personal data of EU residents (GDPR) or, increasingly, Indian residents (DPDP Act).
- **Demands:** lawful basis, consent management, **data-subject rights** (access, deletion/erasure, portability), breach notification, data-residency considerations.
- **TPM watch-items:** Can we **export and delete** one person's / one tenant's data on request? (Ties directly to the [SaaS](../archetypes/saas-multitenant.md) offboarding capability.) Do we know where personal data is stored and processed?

> Regimes **stack**. A healthcare SaaS sold to EU hospitals can be subject to HIPAA-equivalent rules, GDPR, HITRUST, *and* SOC 2 at once. Map them explicitly.

**Others you'll meet depending on context:** **PCI-DSS** (handling card payments), **ISO 27001** (general information-security certification, common in enterprise/EU deals), **FedRAMP** (selling cloud services to US federal government). The same principle applies to all of them — know which bind you, what evidence they demand, and get them into the design phase, not the launch gate.

---

## How compliance shows up in delivery

- **Design phase:** requirements include the compliance controls; architecture accommodates audit logging, encryption, data residency.
- **Build phase:** controls implemented as features, not afterthoughts; evidence captured as you go.
- **Hardening:** validation/testing, security review, evidence collection.
- **Run:** controls operate continuously; access reviews and audits recur.

**Red flag:** a project that treats compliance as a launch-gate checkbox. By then, retrofitting audit trails, validation, or data-residency is expensive and slips the date.

---

## TPM question bank (security & compliance)

- Which **compliance regimes** bind this project? Who confirmed that?
- Has **security** been in the room since **design**, or are we bolting it on?
- Where do **secrets** live, and is anything sensitive in source control?
- Is sensitive data **encrypted in transit and at rest**, including backups?
- Is access **least-privilege**, logged, and reviewed?
- For regulated data: is there a **BAA / DPA** with every vendor in the data path (including AI vendors)?
- For GxP: where's the **validation plan** and the **traceability matrix**, and is validation effort **scaled to risk** (GAMP 5 / CSA)?
- Do any customers require **HITRUST**, **ISO 27001**, or **PCI-DSS**? Is that in the roadmap early enough?
- Can we satisfy a **data-deletion / data-export** request for one person or tenant?
- What did the **security review / pen test** find, and is everything closed or formally risk-accepted?

[← Back to index](../README.md)
