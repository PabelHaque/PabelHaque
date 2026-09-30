# Md. Mominul Haque (Pabel)

**Product manager, microfinance ERP and digital transformation · Dhaka, Bangladesh**

From 2024 to 2026 I carried the product function for the core banking platform of a
400-branch microfinance institution, and I built the analytical and operational systems
that platform needed but did not have. The two halves of that work call for different
skills, and this profile keeps them distinct so that each can be judged on its own terms.

| | Microzen ERP: product management | The systems around it: design and build |
|---|---|---|
| **Scale** | 4,000+ staff · 600,000+ clients · 400+ branches · loan portfolio above BDT 37 billion | 2,600 field officers · 400+ branch offices · 50+ recurring reports automated |
| **My work** | Requirements discovery, 14+ BRDs, four PRDs and an API contract, end-to-end member lifecycle mapping aligned with the IGACAIR credit-rating model, vendor backlog and UAT, CCAC committee coordination, stakeholder alignment across Operations, Finance, Risk and ICT | Architecture, data model, code, tests and rollout for target allocation, the MF Plan Tracker dashboard, weekly-file reconciliation, mobile-wallet onboarding, ERP automation and NID reconciliation, on an AI-assisted, human-gated pipeline |
| **Delivered by** | Analyzen Bangladesh Ltd., to specification | Me, with no vendor and no budget |
| **Evidence** | Specifications, scenario suites, governance records | Repositories, tests, release tags, live adoption |

Underneath both sits a three-year analytics practice: 50+ recurring regulator, finance, management
and donor reports automated from one monthly export, built as 73 analysis notebooks in seven programmes,
plus direct SQL extraction from the ERP database for whatever the front end does not expose.

Case studies with the problem, the before-and-after, the numbers and the rule each system
refuses to break: **[pabelhaque.github.io/portfolio](https://pabelhaque.github.io/portfolio/)**.
Code is private because it ran against organisational data. **Working simulations of seven
systems, with invented data, are open to anyone: [pabelhaque.github.io/portfolio/demos](https://pabelhaque.github.io/portfolio/demos/)**.
Architecture documents and walkthroughs are available on request.

---

## How the systems fit together

```mermaid
flowchart TB
    subgraph PM["Product-managed · vendor-built"]
        MZ["Microzen ERP (core banking)<br/>4,000+ staff · 600K+ clients · 400+ branches"]
        LEAP["LEAP e-commerce<br/>enterprise wing's sales platform"]
        MZ <-- "6 REST APIs: verify loan before sale,<br/>collection data for incentives" --> LEAP
    end

    subgraph BUILT["Designed and built"]
        CM["Weekly CM report<br/>730,000 rows × 95 cols"]
        SM["SaiMarz<br/>joins this week to last in under 5 minutes"]
        TS["Field-Staff Target System<br/>weekly targets for 2,600 officers"]
        BP["MF Plan Tracker dashboard<br/>target vs achievement, role-scoped"]
        MFS["MFS Branch Wallet portal<br/>400+ branch offices · bKash / Nagad / Upay"]
        AUTO["ERP automation suite<br/>transfers · concessions · rebates"]
        NID["NID → KYC reconciliation"]
        AN["Analytics practice<br/>50+ reports automated · 73 notebooks"]
    end

    MZ -- "monthly + weekly exports" --> CM
    CM --> SM --> BP
    CM --> AN
    TS -- "weekly target file" --> BP
    MZ -. "browser automation reads<br/>member & loan data" .-> AUTO
    MZ -. "KYC fields" .-> NID
    MFS -- "branch wallets for<br/>collection & disbursement" --> MZ
    AN -- "cohort & PAR findings<br/>became system rules" --> TS
```

Every arrow above is a real data flow. The dotted ones are browser automation against the ERP
because it exposes no API for that data.

---

## Microzen: the core ERP, product-managed

Microzen is the core banking system of Padakhep Manabik Unnayan Kendra (PMUK), delivered
and maintained by Analyzen Bangladesh Ltd. From August 2024 to 2026 I held the product
function on the organisation's side: what gets built, in what order, to what standard, and
whether it is accepted.

| What | Detail |
|---|---|
| Microzen 1.0, the legacy system | Diagnosed and resolved the issues underlying the legacy ERP, ran the enhancement backlog and UAT with the vendor, kept Operations, Finance, Risk and ICT aligned on priorities; 450+ support cases resolved in the first year |
| Microzen 2.0 requirements | 14+ BRDs and four PRDs form the build specification for the rebuild, positioned as a multi-tenant SaaS for other MFIs. I authored eight of them: system administration, member lifecycle, loan processing, risk monitoring, incident management, MRA/PKSF regulatory reporting and loan classification |
| Amortised loans | Specified declining-balance loan servicing through a 50-scenario suite worked out with the team; rolled out across 400+ branches from September 2025 |
| IGACAIR and the member lifecycle | Mapped the end-to-end member lifecycle and aligned it with IGACAIR, the organisation's credit-rating model, so both are implemented consistently in 2.0 |
| Digital KYC | Drove field adoption; removed paper re-entry from member onboarding |
| Governed rollback | Designed the governed correction path Microzen 1.0 lacked: two-tier approval, scoped to specific transactions, immutable log |
| Business Pulse metrics | Specified the operations metric set (PAR, OTR, collections, savings, staff productivity) with drill-down from organisation to individual officer |
| Governance | Organise and coordinate the Central Coordination Automation Committee (CCAC), where BRDs and PRDs are approved and vendors selected, keeping its members and top management aligned with the digitalisation programme |


---

## LEAP e-commerce and the Microzen–LEAP API

LEAP (Padakhep Life Enhancement Program) is the enterprise wing's sales platform: members buy
products against their loans, and field staff earn incentives on the collections that follow.
I managed its enhancement roadmap and authored the integration contract with the core ERP.

Six REST/JSON APIs, specified in October 2025, link the two systems: member lookup, loan
verification before a sale, sales confirmation, collection data per loan and in bulk, and
employee mapping. Two rules they enforce: no product can be sold against a loan that has not
been disbursed, and staff incentives are computed from verified collections, not from
self-reported sales.

---|---|---|
| Member information | LEAP → Microzen | Auto-fill name, mobile and NID from the member ID; no retyping at the point of sale |
| Loan verification before sale | LEAP → Microzen | A "check disbursement status" step; **no product can be sold against a loan that has not been disbursed** |
| Sales confirmation | LEAP → Microzen | Each sale recorded against its loan and the staff member who made it |
| Collection data, per loan | LEAP → Microzen, quarterly | Percentage collected per staff member with overdue and tenure flags, so incentives are computed from verified repayment, not from self-reported sales |
| Collection data, bulk | LEAP → Microzen, quarterly | The same for all loans at once |
| Employee information | LEAP → Microzen | Branch, area and zone mapping for attribution |

Business rules in the contract: excluded income-generating-activity types, overdue and
active-tenure flags, retroactive incentive when a member regularises, and incentive credited
to the staff member who achieves the regularisation. A later pre-disbursement check runs the
other way: Microzen asks LEAP whether ordered products were received at the branch before a
loan is disbursed. Also settled: a product's price may not exceed 10% of the approved loan,
and branch names are harmonised across both systems so records join cleanly.

---

## Systems designed and built

| System | One line | Number | Status | Code |
|---|---|---|---|---|
| Field-Staff Target System | Splits each branch target across its officers by a weighted formula so the parts sum exactly to the whole, with a full audit trail | About 18,000 weekly targets for 2,600 officers in 400+ branches | Weekly process live since July 2026 | `PMUK-Target-System` (private) · [case study](https://pabelhaque.github.io/portfolio/projects/pmuk-target-system.html) · [**try the simulation**](https://pabelhaque.github.io/portfolio/demos/PMUK_Field_Staff_Target_System/PMUK_Field_Staff_Target_System.html) |
| MF Plan Tracker dashboard | Weekly target vs achievement, rolled up officer → division, each manager sees only their own units | Weekly targets and achievements for 2,600 officers | In weekly use since July 2026 | `AK47-Dashboard` (private) · [case study](https://pabelhaque.github.io/portfolio/projects/ak47-dashboard.html) |
| SaiMarz | Offline desktop tool that replaces VLOOKUP for large Excel files: joins any two workbooks on a shared key, flags duplicate and unmatched keys, keeps leading zeros; also combines files, converts Excel to CSV and splits output by branch | Under 5 minutes per merge, was 20–30 minutes and a frozen laptop | In weekly use by the team | `saimarz` (private) · [case study](https://pabelhaque.github.io/portfolio/projects/saimarz.html) · [**try the simulation**](https://pabelhaque.github.io/portfolio/demos/SaiMarz/SaiMarz.html) |
| MFS Branch Wallet portal | bKash / Nagad / Upay merchant-wallet onboarding for 400+ branch offices with a 13-state maker-checker workflow and generated provider packs | 750+ branch users · 194 registrations · 57 provider batches · median 0.8-day verification | Live since June 2026 | Not published · [case study](https://pabelhaque.github.io/portfolio/projects/mfs-branch-wallet.html) · [**try the simulation**](https://pabelhaque.github.io/portfolio/demos/MFS_Branch_Wallet_Portal/MFS_Branch_Wallet_Portal.html) |
| ERP automation suite | Three tools on one browser-automation layer: transfer-eligibility checks, a concession register mined from WhatsApp, a rebate pipeline from email to verified SMS list | 257 applications across 141 branches; every rebate validated against the ERP before the SMS list | Single-operator tools | `transfer-automation` · `padakhep-rebate-automation` · `special-rebate-automation` (private) · simulations: [transfers](https://pabelhaque.github.io/portfolio/demos/Transfer_Eligibility_Control_Tower/Transfer_Eligibility_Control_Tower.html) · [permissions](https://pabelhaque.github.io/portfolio/demos/Special_Permission_Register/Special_Permission_Register.html) · [rebates](https://pabelhaque.github.io/portfolio/demos/Special_Rebate_Automation/Special_Rebate_Automation.html) |
| NID → KYC reconciliation | Reads the NID card barcode first, OCR as fallback, resolves the member by NID then name + date of birth, and queues mismatches for review | 4 of 5 stages built | Under review at head office for rollout | `document-extraction-engine` (private) · [case study](https://pabelhaque.github.io/portfolio/projects/document-extraction-engine.html) · [**try the simulation**](https://pabelhaque.github.io/portfolio/demos/Document_Extraction_Engine/Document_Extraction_Engine.html) |
| Reports and Analytics | 50+ recurring regulator (MRA, PKSF), finance, management and donor reports automated from one monthly export; work of days per cycle now runs in minutes | 73 analysis notebooks in 7 programmes | Practice, 2023–2026 | `reports-and-analytics` (private) · [case study](https://pabelhaque.github.io/portfolio/projects/reports-and-analytics.html) |

---

## How I build

A gated, AI-assisted delivery pipeline of my own: 71 specialist agents, 38 stages and four
human approval gates, with artefact verification and an adversarial review step. It built the
target system, the dashboard and the study platform. Repository: `factory-agent-overlay`
(private) · [case study](https://pabelhaque.github.io/portfolio/projects/factory-agent-overlay.html).

Design habits that show up in every system above: the parts must sum to the whole; unknown
stays unknown rather than guessed; nothing is auto-approved or auto-sent; every decision leaves
an audit row; Bengali text and leading-zero identifiers are handled on purpose.

---

## Contact

mdmominul97haque@gmail.com · [LinkedIn](https://www.linkedin.com/in/md-mominul-haque) ·
[Portfolio](https://pabelhaque.github.io/portfolio/)
