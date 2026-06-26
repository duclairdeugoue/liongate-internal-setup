# PROFORMA — Project Management Tooling Billing

**LIONGATE SARL — Infrastructure Cost Estimation**
**Document:** PROFORMA-06
**Context:** Project management, issue tracking, documentation, communication tools
**Hosting Provider:** SaaS-led + Hetzner-hosted Odoo
**Currency:** EUR (€)
**Version:** 1.0

---

> **How to read this document:**
> This proforma covers the **tools required to run software projects** at LIONGATE — both for
> internal delivery and for client engagements. It mirrors the two cases defined in
> `06-project-management-using-enterprise-standard-and-odoo-github-projects.md`:
> **Case A — Enterprise-standard tools** vs **Case B — Odoo + GitHub Projects**.

---

## Tier Definitions (used in both cases)

| Tier   | Team size       | Project mix                                    |
| ------ | --------------- | ---------------------------------------------- |
| **T1** | 3–8 people      | 1–2 active projects, no formal PM role         |
| **T2** | 8–20 people     | 3–6 active projects, dedicated PM              |
| **T3** | 20+ people      | Multi-team, multiple PMs, formal governance    |

---

## CASE A — Enterprise-standard tools (Jira / Confluence / Slack / Linear ecosystem)

### Reference Stack

| Domain                | Primary Tool                  | Alternative                        |
| --------------------- | ----------------------------- | ---------------------------------- |
| Issue tracking        | Jira Software / Linear        | ClickUp, Asana, Shortcut           |
| Wiki / documentation  | Confluence / Notion           | Slab, Outline                      |
| Communication         | Slack                         | Microsoft Teams                    |
| Video calls           | Google Meet / Zoom            | Microsoft Teams                    |
| Diagrams              | Miro / FigJam / Lucidchart    | Excalidraw self-hosted             |
| Time tracking         | Toggl / Harvest / Clockify    | Built-in Jira                      |
| Roadmap / OKR         | Productboard / Linear Roadmap | Notion templates                   |
| Customer feedback     | Canny / Productboard          | GitHub Discussions                 |
| Forms / surveys       | Tally / Typeform              | Google Forms                       |
| File sharing          | Google Workspace / M365       | Nextcloud self-hosted              |

---

### Case A — Tier 1 (3–8 people)

| #     | Tool                  | Plan                     | Unit Price       | Qty | Monthly (€) | Annual (€)  |
| ----- | --------------------- | ------------------------ | ---------------- | --- | ----------- | ----------- |
| 1     | Jira Software         | Standard                 | €7.16/user       | 8   | 57.28       | 687.36      |
| 2     | Confluence            | Standard                 | €5.16/user       | 8   | 41.28       | 495.36      |
| 3     | Slack                 | Pro                      | €6.75/user       | 8   | 54.00       | 648.00      |
| 4     | Google Workspace      | Business Starter         | €5.75/user       | 8   | 46.00       | 552.00      |
| 5     | Miro                  | Starter                  | €8.00/user       | 4   | 32.00       | 384.00      |
| 6     | Toggl Track           | Free                     | Free             | 8   | 0.00        | 0.00        |
| 7     | Tally                 | Free                     | Free             | —   | 0.00        | 0.00        |
| **—** | **SUBTOTAL**          |                          |                  |     | **€230.56** | **€2,766.72** |

---

### Case A — Tier 2 (8–20 people)

| #     | Tool                  | Plan                     | Unit Price       | Qty | Monthly (€) | Annual (€)    |
| ----- | --------------------- | ------------------------ | ---------------- | --- | ----------- | ------------- |
| 1     | Jira Software         | Standard                 | €7.16/user       | 18  | 128.88      | 1,546.56      |
| 2     | Confluence            | Standard                 | €5.16/user       | 18  | 92.88       | 1,114.56      |
| 3     | Slack                 | Pro                      | €6.75/user       | 18  | 121.50      | 1,458.00      |
| 4     | Google Workspace      | Business Standard        | €11.50/user      | 18  | 207.00      | 2,484.00      |
| 5     | Miro                  | Business                 | €15.00/user      | 10  | 150.00      | 1,800.00      |
| 6     | Toggl Track           | Starter                  | €9.00/user       | 18  | 162.00      | 1,944.00      |
| 7     | Linear (optional, eng) | Standard                | €8.00/user       | 12  | 96.00       | 1,152.00      |
| 8     | Tally Pro             | Per workspace            | €25.00           | 1   | 25.00       | 300.00        |
| **—** | **SUBTOTAL**          |                          |                  |     | **€983.26** | **€11,799.12** |

---

### Case A — Tier 3 (20+ people)

| #     | Tool                  | Plan                     | Unit Price       | Qty | Monthly (€)   | Annual (€)     |
| ----- | --------------------- | ------------------------ | ---------------- | --- | ------------- | -------------- |
| 1     | Jira Software         | Premium                  | €13.83/user      | 30  | 414.90        | 4,978.80       |
| 2     | Confluence            | Premium                  | €10.06/user      | 30  | 301.80        | 3,621.60       |
| 3     | Slack                 | Business+                | €11.75/user      | 30  | 352.50        | 4,230.00       |
| 4     | Google Workspace      | Business Plus            | €17.25/user      | 30  | 517.50        | 6,210.00       |
| 5     | Miro                  | Business                 | €15.00/user      | 20  | 300.00        | 3,600.00       |
| 6     | Productboard          | Pro                      | €19.00/user      | 5   | 95.00         | 1,140.00       |
| 7     | Linear                | Business                 | €13.00/user      | 20  | 260.00        | 3,120.00       |
| 8     | PagerDuty (ops)       | Pro                      | €21.00/user      | 10  | 210.00        | 2,520.00       |
| 9     | Toggl Track           | Premium                  | €17.00/user      | 30  | 510.00        | 6,120.00       |
| 10    | Atlassian Access (SSO)| Per user                 | €4.00/user       | 30  | 120.00        | 1,440.00       |
| **—** | **SUBTOTAL**          |                          |                  |     | **€3,081.70** | **€36,980.40** |

---

### Case A — Summary

| Tier | Team size | Monthly (€)   | Annual (€)     |
| ---- | --------- | ------------- | -------------- |
| T1   | 3–8       | 230.56        | 2,766.72       |
| T2   | 8–20      | 983.26        | 11,799.12      |
| T3   | 20+       | 3,081.70      | 36,980.40      |

---

## CASE B — Odoo + GitHub Projects

> Odoo provides PM/CRM/HR/finance; GitHub Projects provides engineering execution.
> Communication and docs stay lightweight (Slack/Discord + Odoo Documents / Notion).

### Reference Stack

| Domain                | Primary Tool                              | Notes                                |
| --------------------- | ----------------------------------------- | ------------------------------------ |
| Business PM, CRM, HR  | **Odoo Community** self-hosted on Hetzner | See PROFORMA-01 for infra cost       |
| Engineering tracking  | **GitHub Projects + Issues**              | Included with GitHub repo            |
| Code review / VCS     | **GitHub Team / Enterprise**              | See PROFORMA-05                      |
| Docs                  | Outline self-hosted **or** Notion         | Lightweight setup                    |
| Communication         | Slack Free / Pro **or** Discord / Mattermost self-hosted | |
| Time tracking         | Odoo Timesheets module                    | Free with Odoo Community             |
| Forms / surveys       | Odoo Surveys                              | Free with Odoo Community             |
| Diagrams              | Excalidraw self-hosted                    | Free                                 |

> 💡 Odoo replaces 5–8 SaaS tools (Jira, Harvest, Productboard, Tally, partial Confluence, partial CRM, partial Workspace), at the cost of self-hosting it. Refer to **PROFORMA-01** for Odoo infra cost.

---

### Case B — Tier 1 (3–8 people)

| #     | Item                                       | Plan / Tier                    | Unit Price       | Qty | Monthly (€) | Annual (€)  |
| ----- | ------------------------------------------ | ------------------------------ | ---------------- | --- | ----------- | ----------- |
| 1     | Odoo infrastructure (Community)            | See PROFORMA-01 Tier 1         | —                | 1   | 33.75       | 405.00      |
| 2     | GitHub Team                                | Per seat                       | €3.67/user       | 8   | 29.36       | 352.32      |
| 3     | GitHub Projects                            | Included                       | Free             | —   | 0.00        | 0.00        |
| 4     | Slack Free                                 | Free                           | Free             | —   | 0.00        | 0.00        |
| 5     | Google Workspace                           | Business Starter               | €5.75/user       | 8   | 46.00       | 552.00      |
| 6     | Outline / Notion Free                      | Free tier                      | Free             | —   | 0.00        | 0.00        |
| 7     | Excalidraw self-hosted (on internal-apps)  | Free                           | Free             | —   | 0.00        | 0.00        |
| **—** | **SUBTOTAL**                               |                                |                  |     | **€109.11** | **€1,309.32** |

---

### Case B — Tier 2 (8–20 people)

| #     | Item                                       | Plan / Tier                    | Unit Price       | Qty | Monthly (€) | Annual (€)    |
| ----- | ------------------------------------------ | ------------------------------ | ---------------- | --- | ----------- | ------------- |
| 1     | Odoo infrastructure (Community)            | See PROFORMA-01 Tier 2         | —                | 1   | 76.00       | 912.00        |
| 2     | GitHub Team                                | Per seat                       | €3.67/user       | 18  | 66.06       | 792.72        |
| 3     | Slack Pro                                  | Per seat                       | €6.75/user       | 18  | 121.50      | 1,458.00      |
| 4     | Google Workspace                           | Business Standard              | €11.50/user      | 18  | 207.00      | 2,484.00      |
| 5     | Notion Team                                | Per seat                       | €9.50/user       | 18  | 171.00      | 2,052.00      |
| 6     | Excalidraw self-hosted                     | Free                           | Free             | —   | 0.00        | 0.00          |
| 7     | Tally Pro (forms when Odoo Surveys is insufficient) | Per workspace        | €25.00           | 1   | 25.00       | 300.00        |
| **—** | **SUBTOTAL**                               |                                |                  |     | **€666.56** | **€7,998.72** |

> Savings vs Case A Tier 2: **€316.70/mo (€3,800/yr)** by replacing Jira+Confluence+Toggl+Miro with Odoo + GitHub Projects + Excalidraw.

---

### Case B — Tier 3 (20+ people)

| #     | Item                                       | Plan / Tier                    | Unit Price       | Qty | Monthly (€)   | Annual (€)     |
| ----- | ------------------------------------------ | ------------------------------ | ---------------- | --- | ------------- | -------------- |
| 1     | Odoo infrastructure (Community + HA)       | See PROFORMA-01 Tier 3         | —                | 1   | 150.00        | 1,800.00       |
| 2     | GitHub Enterprise                          | Per seat                       | €19.25/user      | 30  | 577.50        | 6,930.00       |
| 3     | Slack Business+                            | Per seat                       | €11.75/user      | 30  | 352.50        | 4,230.00       |
| 4     | Google Workspace Business Plus             | Per seat                       | €17.25/user      | 30  | 517.50        | 6,210.00       |
| 5     | Notion Business                            | Per seat                       | €14.00/user      | 30  | 420.00        | 5,040.00       |
| 6     | PagerDuty Pro (ops)                        | Per seat                       | €21.00/user      | 10  | 210.00        | 2,520.00       |
| 7     | Excalidraw self-hosted                     | Free                           | Free             | —   | 0.00          | 0.00           |
| 8     | Odoo migration / dev maintenance reserve   | Fixed monthly engineering hours | €40/h × 8 h     | 1   | 320.00        | 3,840.00       |
| **—** | **SUBTOTAL**                               |                                |                  |     | **€2,547.50** | **€30,570.00** |

> Savings vs Case A Tier 3: **€534.20/mo (€6,410/yr)**. Bigger gap if Notion is also replaced by Outline self-hosted (saves another ~€420/mo).

---

### Case B — Summary

| Tier | Team size | Monthly (€)   | Annual (€)     |
| ---- | --------- | ------------- | -------------- |
| T1   | 3–8       | 109.11        | 1,309.32       |
| T2   | 8–20      | 666.56        | 7,998.72       |
| T3   | 20+       | 2,547.50      | 30,570.00      |

---

## Case A vs Case B — Comparison

| Tier   | Case A (Monthly) | Case B (Monthly) | Monthly Savings | Annual Savings | % Cheaper |
| ------ | ---------------- | ---------------- | --------------- | -------------- | --------- |
| **T1** | €230.56          | €109.11          | €121.45         | €1,457.40      | **52.7%** |
| **T2** | €983.26          | €666.56          | €316.70         | €3,800.40      | **32.2%** |
| **T3** | €3,081.70        | €2,547.50        | €534.20         | €6,410.40      | **17.3%** |

### Trade-offs

| Dimension                       | Case A (Jira-style)             | Case B (Odoo + GitHub)              |
| ------------------------------- | ------------------------------- | ----------------------------------- |
| Setup time                      | Hours                           | Days (Odoo install + config)        |
| Engineering ergonomics          | Excellent (Jira/Linear/GitHub)  | Excellent for GitHub side; weaker on Odoo task UX |
| Business-side ergonomics        | Good (Confluence + roadmaps)    | Excellent (full CRM/HR/finance via Odoo)         |
| Recurring SaaS cost             | High                            | Lower (especially T2)               |
| Ops burden                      | Near-zero (managed SaaS)        | Self-hosted Odoo to maintain        |
| Data ownership                  | Vendor-locked                   | Owned + portable                    |
| Best fit                        | Pure software shops             | Mixed engineering + business org    |

---

## Hidden & Variable Costs

| Cost Item                                  | Frequency | Estimated Amount     | Notes                                              |
| ------------------------------------------ | --------- | -------------------- | -------------------------------------------------- |
| Atlassian Access / SSO add-on              | Monthly   | +€4/user             | Required for SAML SSO                              |
| Notion AI / Jira AI add-ons                | Monthly   | +€8–€18/user         | Optional                                           |
| Odoo Enterprise licence (if upgraded)      | Annual    | €31.10/user/mo       | Significant — only if Enterprise modules needed    |
| Odoo custom module dev                     | One-off   | €1,500–€10,000       | When business workflows are non-standard           |
| Slack Connect for client channels          | Monthly   | Included (Pro+)      | Pro plan minimum                                   |
| Miro additional editor seats               | Monthly   | +€8–€15/user         | Watch as design org grows                          |
| GitHub Copilot Business (recommended)      | Monthly   | €19.00/user          | Out of scope for this proforma; budget separately  |

---

## Budget Planning Summary (Project Management)

```
LIONGATE SARL — Project Management Tooling Budget Reference
============================================================

CASE A (Enterprise-standard, Jira-style):
  T1 (3–8 people):   €230.56/mo    €2,766.72/yr
  T2 (8–20 people):  €983.26/mo    €11,799.12/yr
  T3 (20+ people):   €3,081.70/mo  €36,980.40/yr

CASE B (Odoo + GitHub Projects):
  T1 (3–8 people):   €109.11/mo    €1,309.32/yr
  T2 (8–20 people):  €666.56/mo    €7,998.72/yr
  T3 (20+ people):   €2,547.50/mo  €30,570.00/yr

RECOMMENDED FOR LIONGATE SARL (Year 1–2, startup phase):
  ➜  CASE B Tier 1 → Tier 2  (€109 → €667/mo as team grows)

CONTINGENCY BUFFER (+10%):
  Case B T2: €7,998.72 × 1.10 = €8,798.59/yr
```

---

## Decision Guide

| Question                                                  | Recommended case |
| --------------------------------------------------------- | ---------------- |
| Pure software product team, no CRM/HR/finance needs       | Case A           |
| Need CRM + project management + accounting in one tool    | Case B           |
| Limited ops capacity, no time to maintain self-hosted Odoo | Case A          |
| Cost is a primary constraint at < 20 people               | Case B           |
| Need SAML SSO from day 1                                  | Case A (Atlassian Access) or Case B (Authentik in front of Odoo) |
| Client requires Jira integration (e.g. enterprise client)  | Case A          |
| LIONGATE is the standard target operating model           | **Case B (recommended default)** |

---

_Document: PROFORMA-06 | Context: Project Management Tooling_
_Prices: Atlassian, Slack, Google, Notion, Linear, Productboard list prices + Odoo infra from PROFORMA-01_
_Review: Quarterly, or whenever team size crosses a tier boundary_
_Prepared by: LIONGATE SARL Engineering Team_
