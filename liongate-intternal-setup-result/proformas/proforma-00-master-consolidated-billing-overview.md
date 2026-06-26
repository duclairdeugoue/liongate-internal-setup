# PROFORMA — Master Consolidated Billing Overview

**LIONGATE SARL — Consolidated Infrastructure & Tooling Cost Plan**
**Document:** PROFORMA-00 (Master)
**Currency:** EUR (€)
**Version:** 1.0
**Source documents:** PROFORMA-01 through PROFORMA-06

---

> **Purpose of this document:**
> This is the **executive cost-planning reference** for LIONGATE SARL.
> It consolidates every individual proforma (Odoo, standard web apps, high-traffic projects,
> internal tools, DevOps, project management) into a single view, then proposes three
> ready-to-budget company scenarios for **Year 1**, **Year 2**, and **Year 3** of operation.
> Use this as the input to financial planning, client quoting, and headcount decisions.

---

## 1. Source Proformas — Index

| #         | Document                                                       | Scope                                                  |
| --------- | -------------------------------------------------------------- | ------------------------------------------------------ |
| **00**    | `proforma-00-master-consolidated-billing-overview.md`          | **This document — consolidated view**                  |
| 01        | `proforma-01-odoo-instance-billing.md`                         | Self-hosted Odoo ERP                                   |
| 02        | `proforma-02-standard-web-application-billing.md`              | Standard web app (NestJS/Next.js + Postgres)           |
| 03        | `proforma-03-high-traffic-client-project-billing.md`           | High-traffic public SaaS / e-commerce                  |
| 04        | `proforma-04-internal-tools-billing.md`                        | Internal-staff-only tools                              |
| 05        | `proforma-05-infrastructure-devops-shared-tools-billing.md`    | Company-wide DevOps & engineering tooling              |
| 06        | `proforma-06-project-management-tools-billing.md`              | PM tooling (Case A enterprise / Case B Odoo+GitHub)    |

---

## 2. Tier Cross-reference

All proformas use compatible tier names. The table below allows a single project to be
sized at a glance.

| Proforma | T1 / Min   | T2 / Recommended | T3 / Full        | T4 / HA / Enterprise   |
| -------- | ---------- | ---------------- | ---------------- | ---------------------- |
| 01 Odoo  | €33.75/mo  | €76.00/mo        | €150.00/mo       | ~€350/mo (HA)          |
| 02 Web   | €20.03/mo  | €70.08/mo        | €103.98/mo       | €219.11/mo             |
| 03 HighT | €226.86/mo | €615.83/mo       | €1,378.05/mo     | €4,638.68/mo           |
| 04 Intl  | €18.84/mo  | €63.80/mo        | €146.89/mo       | —                      |
| 05 DevOps| €76.55/mo  | €604.54/mo       | €3,131.52/mo     | —                      |
| 06A PM   | €230.56/mo | €983.26/mo       | €3,081.70/mo     | —                      |
| 06B PM   | €109.11/mo | €666.56/mo       | €2,547.50/mo     | —                      |

> 💡 Proforma-01..04 are **per-project**. Proforma-05 and Proforma-06 are **company-wide** (paid once, regardless of project count).

---

## 3. Cost Anatomy of a Single Project

For any new project, the monthly cost equation is:

```
Project cost = Project hosting (PROFORMA-01..04, by type+tier)
             + Marginal DevOps cost (PROFORMA-05 ÷ active projects)
             + Marginal PM cost     (PROFORMA-06 ÷ active projects)
```

### Worked example — One client web app, Tier 2

```
Project hosting (PROFORMA-02 T2):          €70.08
DevOps share (PROFORMA-05 T-B €604.54 / 4 projects): €151.14
PM share     (PROFORMA-06 Case B T2 €666.56 / 4):    €166.64
-------------------------------------------------------------
True fully-loaded monthly cost:           €387.86
True fully-loaded annual cost:            €4,654.32
```

> 💡 **Quoting rule of thumb:** when pricing a client, multiply true monthly infra cost by **2.5× to 4×** to cover engineering labour, overhead, and margin.

---

## 4. Year-by-Year Company Budget Scenarios

Three forward-looking scenarios assuming LIONGATE's current trajectory (startup phase, 2026 → 2028).

### 4.1 Scenario — Year 1 (Bootstrapping, 2–5 engineers)

**Assumptions:**

- 3 engineers + 1 founder
- 1 self-hosted Odoo (internal use)
- 2 client projects (Tier 1 web apps)
- 1 lightweight internal tool

| Component                                   | Reference            | Monthly (€) | Annual (€)  |
| ------------------------------------------- | -------------------- | ----------- | ----------- |
| Odoo (internal) — T1                        | PROFORMA-01 T1       | 33.75       | 405.00      |
| Client project A — Web T1                   | PROFORMA-02 T1       | 20.03       | 240.36      |
| Client project B — Web T1                   | PROFORMA-02 T1       | 20.03       | 240.36      |
| Internal tool — T1                          | PROFORMA-04 T1       | 18.84       | 226.08      |
| DevOps shared tooling — Tier A              | PROFORMA-05 Tier A   | 76.55       | 918.60      |
| Project management — Case B T1              | PROFORMA-06 B-T1     | 109.11      | 1,309.32    |
| **YEAR 1 TOTAL (before contingency)**       |                      | **€278.31** | **€3,339.72** |
| **+15% contingency**                        |                      | **€320.06** | **€3,840.68** |

### 4.2 Scenario — Year 2 (Growth, 6–12 engineers)

**Assumptions:**

- 8 engineers + 2 founders + 1 PM
- 1 Odoo (internal — upgraded to T2)
- 4 client web apps (mix: 2× T2, 1× T3, 1× T1)
- 1 high-traffic launch (T1)
- 2 internal tools (T2)

| Component                                          | Reference            | Monthly (€) | Annual (€)   |
| -------------------------------------------------- | -------------------- | ----------- | ------------ |
| Odoo (internal) — T2                               | PROFORMA-01 T2       | 76.00       | 912.00       |
| Client web app A — T2                              | PROFORMA-02 T2       | 70.08       | 840.96       |
| Client web app B — T2                              | PROFORMA-02 T2       | 70.08       | 840.96       |
| Client web app C — T3                              | PROFORMA-02 T3       | 103.98      | 1,247.76     |
| Client web app D — T1                              | PROFORMA-02 T1       | 20.03       | 240.36       |
| High-traffic launch — T1                           | PROFORMA-03 T1       | 226.86      | 2,722.32     |
| Internal tools (shared host, T2)                   | PROFORMA-04 T2       | 63.80       | 765.60       |
| DevOps shared tooling — Tier B                     | PROFORMA-05 Tier B   | 604.54      | 7,254.48     |
| Project management — Case B T2                     | PROFORMA-06 B-T2     | 666.56      | 7,998.72     |
| **YEAR 2 TOTAL (before contingency)**              |                      | **€1,901.93** | **€22,823.16** |
| **+15% contingency**                               |                      | **€2,187.22** | **€26,246.63** |

### 4.3 Scenario — Year 3 (Established, 15–25 engineers)

**Assumptions:**

- 18 engineers + 2 founders + 3 PMs + 2 ops
- 1 Odoo (internal — T3)
- 6 client web apps (mix: 1× T1, 3× T2, 2× T3)
- 2 high-traffic projects (1× T1, 1× T2)
- 3 internal tools (consolidated on T3 stack)

| Component                                          | Reference            | Monthly (€)   | Annual (€)     |
| -------------------------------------------------- | -------------------- | ------------- | -------------- |
| Odoo (internal) — T3                               | PROFORMA-01 T3       | 150.00        | 1,800.00       |
| Client web app A — T1                              | PROFORMA-02 T1       | 20.03         | 240.36         |
| Client web apps B/C/D — T2 (×3)                    | PROFORMA-02 T2       | 210.24        | 2,522.88       |
| Client web apps E/F — T3 (×2)                      | PROFORMA-02 T3       | 207.96        | 2,495.52       |
| High-traffic project X — T1                        | PROFORMA-03 T1       | 226.86        | 2,722.32       |
| High-traffic project Y — T2                        | PROFORMA-03 T2       | 615.83        | 7,389.96       |
| Internal tools — T3                                | PROFORMA-04 T3       | 146.89        | 1,762.68       |
| DevOps shared tooling — Tier C                     | PROFORMA-05 Tier C   | 3,131.52      | 37,590.24      |
| Project management — Case B T3                     | PROFORMA-06 B-T3     | 2,547.50      | 30,570.00      |
| **YEAR 3 TOTAL (before contingency)**              |                      | **€7,256.83** | **€87,093.96** |
| **+15% contingency**                               |                      | **€8,345.35** | **€100,158.05** |

---

## 5. Consolidated Year-Over-Year Forecast

| Year  | Headcount | Active projects | Monthly (€)   | Annual (€)      | Avg cost per engineer/yr |
| ----- | --------- | --------------- | ------------- | --------------- | ------------------------ |
| Y1    | ~4        | 4               | €320.06       | €3,840.68       | €960.17                  |
| Y2    | ~11       | 8               | €2,187.22     | €26,246.63      | €2,386.06                |
| Y3    | ~25       | 12              | €8,345.35     | €100,158.05     | €4,006.32                |

### Cost-share breakdown (Y2 example, % of monthly)

| Category                              | Monthly (€) | Share  |
| ------------------------------------- | ----------- | ------ |
| Per-project hosting (PROFORMA-01..04) | €630.83     | 33.2%  |
| DevOps shared tooling                 | €604.54     | 31.8%  |
| Project management tooling            | €666.56     | 35.0%  |

> 💡 **Key insight:** From Y2 onward, **company-wide tooling (DevOps + PM)** typically exceeds per-project hosting cost. Optimising headcount and seat-based SaaS becomes more impactful than optimising server tiers.

---

## 6. Recommended Defaults for LIONGATE SARL

Based on the analysis across all proformas, the **recommended defaults** are:

| Decision                                 | Recommendation                                           |
| ---------------------------------------- | -------------------------------------------------------- |
| **Hosting provider**                     | Hetzner Cloud (primary) + Hetzner Robot (Tier 4 only)    |
| **Domain registrar**                     | United Domains (already chosen)                          |
| **DNS / edge**                           | Cloudflare Free → Pro at first paying client             |
| **Odoo edition**                         | Community (Enterprise only if a client mandates it)      |
| **Project management model**             | **Case B — Odoo + GitHub Projects** (default)            |
| **DevOps tier transitions**              | Tier A → Tier B at the 3rd active client project         |
| **Internal tools strategy**              | One shared `internal-apps` server (PROFORMA-04 T2)       |
| **Default client project tier**          | PROFORMA-02 Tier 2 (recommended), upsell to T3 if SLAs   |
| **High-traffic default**                 | PROFORMA-03 Tier 1 at launch; pre-scoped path to T2/T3   |
| **Quoting markup**                       | 2.5–4× true infra cost for client billing                |
| **Annual contingency buffer**            | 15% for infra, 20% for SaaS                              |

---

## 7. Client Quoting Quick-Reference

Use this table to price the **infrastructure pass-through** to a client.

| Client project type                    | Suggested tier            | Pass-through / mo | Pass-through / yr |
| -------------------------------------- | ------------------------- | ----------------- | ----------------- |
| MVP / prototype / internal pilot       | PROFORMA-02 T1            | €20–€30           | €240–€370         |
| Standard business web app              | PROFORMA-02 T2            | €70–€90           | €840–€1,100       |
| Active SaaS product                    | PROFORMA-02 T3            | €100–€135         | €1,250–€1,575     |
| Public launch (high-traffic)           | PROFORMA-03 T1            | €225–€285         | €2,720–€3,420     |
| Growing SaaS with read replica         | PROFORMA-03 T2            | €615–€745         | €7,390–€8,936     |
| HA / mission-critical SaaS             | PROFORMA-03 T3            | €1,378–€1,640     | €16,540–€19,660   |
| Multi-region / SLA-backed              | PROFORMA-03 T4            | €4,640–€5,420     | €55,665–€65,030   |
| Odoo deployment for client             | PROFORMA-01 T2 or T3      | €76–€150          | €912–€1,800       |

> Always quote infra **separately** from engineering labour. Engineering labour is **not** in any proforma.

---

## 8. Master Cost Optimisation Levers

Ranked from highest impact to lowest:

| #   | Lever                                                          | Estimated saving                  |
| --- | -------------------------------------------------------------- | --------------------------------- |
| 1   | Use **Case B (Odoo + GitHub Projects)** instead of Case A      | €1,400–€6,400/yr depending on size |
| 2   | Self-host Doppler → Vault                                      | €180–€900/mo                      |
| 3   | Self-host Notion → Outline                                     | €115–€420/mo                      |
| 4   | Use Cloudflare Access Free instead of Tailscale Business       | €450/mo at 25 users               |
| 5   | Stop staging servers off-hours                                 | 50–60% of staging cost            |
| 6   | Co-locate internal tools on one shared `internal-apps` server  | €40–€80/mo per tool avoided       |
| 7   | Prefer Hetzner Object Storage over Hetzner Storage Box for cold backups | €5–€20/mo per TB         |
| 8   | Use Let's Encrypt rather than commercial SSL                   | €50–€500/yr per cert              |
| 9   | Consolidate domains on United Domains rather than per-project registrars | €5–€10/yr per domain    |
| 10  | Use community Odoo + self-built modules instead of Odoo Enterprise | €31.10/user/mo                |

---

## 9. Master Budget Planning Cheat Sheet

```
LIONGATE SARL — MASTER BUDGET CHEAT SHEET
==========================================

YEAR 1 (Bootstrapping, ~4 people, 4 projects)
  Conservative annual budget:   €3,841    (€320/mo)
  Per-engineer/yr:              ~€960

YEAR 2 (Growth, ~11 people, 8 projects)
  Conservative annual budget:   €26,247   (€2,187/mo)
  Per-engineer/yr:              ~€2,386

YEAR 3 (Established, ~25 people, 12 projects)
  Conservative annual budget:   €100,158  (€8,345/mo)
  Per-engineer/yr:              ~€4,006

3-YEAR TOTAL INFRA + TOOLING:   ~€130,246
```

---

## 10. Maintenance & Review

| Activity                              | Cadence                  | Owner                |
| ------------------------------------- | ------------------------ | -------------------- |
| Review Hetzner pricing changes        | Quarterly                | DevOps lead          |
| Review SaaS seat utilisation          | Monthly                  | Operations / Finance |
| Revalidate per-project tiers          | At each project kickoff  | Tech lead            |
| Re-issue this master proforma         | Every 6 months           | DevOps lead          |
| Audit hidden costs vs forecast        | Quarterly                | Finance              |
| Annual contingency true-up            | Annually (calendar Q1)   | Finance              |

---

## 11. Assumptions, Caveats, Open Questions

### Assumptions

- All prices are **Hetzner public list rates** (EUR, VAT excluded) at time of writing.
- SaaS prices are **monthly-billed list prices** unless noted. Annual prepay typically saves 15–20%.
- A working day = 8 h. Engineering labour cost is **out of scope**.
- LIONGATE remains the operator (no managed/white-glove provider involved).
- All domains routed through United Domains; Cloudflare used only for DNS/CDN.

### Caveats

- Hetzner pricing has changed multiple times in 2024–2025. Re-verify before contracts.
- SaaS providers (Atlassian, Notion, Slack) frequently restructure tiers. Re-verify quarterly.
- Bandwidth costs assume traffic within Hetzner free egress allowances (20 TB/mo per server).
- Backup restoration time and DR RTO/RPO are **not** budgeted here — covered in deployment docs.

### Open questions for leadership

1. Will LIONGATE adopt **Odoo Enterprise** at any tier? (Currently assumed Community.)
2. Is **24/7 on-call** within scope for any current client? (Adds €500–€2,500/mo per project.)
3. Will LIONGATE target **SOC 2 / ISO 27001** within 18 months? (Adds €5k–€25k/yr.)
4. Should we standardise on **Tailscale paid plan** or stay on Cloudflare Access Free?
5. What is the maximum **per-project infra ceiling** acceptable without finance sign-off?

---

_Document: PROFORMA-00 (Master) | Context: Consolidated billing overview_
_Aggregates: PROFORMA-01 through PROFORMA-06_
_Review cadence: Every 6 months, or at any tier transition_
_Prepared by: LIONGATE SARL Engineering Team_
