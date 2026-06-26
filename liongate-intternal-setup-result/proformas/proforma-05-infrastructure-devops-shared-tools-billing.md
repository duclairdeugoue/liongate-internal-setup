# PROFORMA — Infrastructure, DevOps & Shared Tooling Billing

**LIONGATE SARL — Infrastructure Cost Estimation**
**Document:** PROFORMA-05
**Context:** Company-wide shared DevOps / Infrastructure / Engineering tooling
**Hosting Provider:** Hetzner Cloud + SaaS partners
**Currency:** EUR (€)
**Version:** 1.0

---

> **How to read this document:**
> This proforma covers **cross-project DevOps and engineering infrastructure** — the shared tooling
> every LIONGATE engineer uses regardless of which client/internal project they work on.
> These costs are **amortised across all projects**, not billed to a single product.

---

## Scope

This document covers:

- Source control (GitHub)
- CI/CD compute (runners)
- Container registry
- Secrets management
- Observability (logs, metrics, traces, errors)
- Status / uptime monitoring
- Backup orchestration
- Infrastructure-as-Code state
- DNS & domain management
- Developer environments (review apps)
- Engineering communication / docs

It does **not** cover:

- Per-project servers/databases — see PROFORMA-01..04
- Project management — see PROFORMA-06
- Office / non-engineering software

---

## Reference Stack

| Domain            | Primary Tool                       | Open-source Alternative              |
| ----------------- | ---------------------------------- | ------------------------------------ |
| Source control    | GitHub Team                        | Gitea self-hosted                    |
| CI/CD             | GitHub Actions (+ self-hosted)     | Drone / Woodpecker                   |
| Container registry| GitHub Container Registry (GHCR)   | Harbor self-hosted                   |
| Secrets           | Doppler Team                       | HashiCorp Vault self-hosted          |
| Logs              | Grafana Loki (self-hosted)         | Logtail / Better Stack               |
| Metrics           | Prometheus + Grafana (self-hosted) | Grafana Cloud                        |
| Tracing           | Tempo (self-hosted) / Sentry       | Honeycomb / Jaeger                   |
| Errors            | Sentry Team                        | GlitchTip self-hosted                |
| Uptime monitoring | UptimeRobot / Better Stack         | Uptime Kuma self-hosted              |
| Status page       | Atlassian Statuspage / cstate      | Uptime Kuma status page (free)       |
| IaC state         | Terraform Cloud free / S3 backend  | Hetzner Object Storage backend       |
| DNS               | Cloudflare DNS (Free)              | Hetzner DNS (Free)                   |
| Domains           | United Domains                     | —                                    |
| Review apps       | Vercel / Coolify self-hosted       | Hetzner-based per-branch container   |
| Comms             | Slack / Discord                    | Mattermost self-hosted               |
| Engineering docs  | Notion / Outline / Docusaurus      | Wiki.js self-hosted                  |

---

## Tier A — Minimum Viable DevOps Stack (Startup / 2–5 engineers)

> Maximum reliance on free tiers and bundled services. Suitable for early stage LIONGATE.

| #     | Service                              | Tier / Plan                           | Unit Price        | Qty   | Monthly (€) | Annual (€)  |
| ----- | ------------------------------------ | ------------------------------------- | ----------------- | ----- | ----------- | ----------- |
| 1     | GitHub                               | Free + Team for org (€3.67/user/mo)   | €3.67/user        | 5     | 18.35       | 220.20      |
| 2     | GitHub Actions minutes               | 2,000 free → likely €0                | Free / overage    | —     | 0.00        | 0.00        |
| 3     | GHCR storage                         | Free up to 500 MB private             | Free              | —     | 0.00        | 0.00        |
| 4     | Doppler                              | Developer Free                        | Free              | —     | 0.00        | 0.00        |
| 5     | Sentry                               | Developer Free (5k events/mo)         | Free              | —     | 0.00        | 0.00        |
| 6     | UptimeRobot                          | Free (50 monitors, 5-min checks)      | Free              | —     | 0.00        | 0.00        |
| 7     | Cloudflare DNS                       | Free                                  | Free              | —     | 0.00        | 0.00        |
| 8     | Terraform state                      | Hetzner Object Storage backend        | €0.001 per GB     | 1     | 0.10        | 1.20        |
| 9     | Notion                               | Plus, 5 seats                         | €9.50/user        | 5     | 47.50       | 570.00      |
| 10    | Slack                                | Free                                  | Free              | —     | 0.00        | 0.00        |
| 11    | Domains @ United Domains             | ~5 domains avg €15/yr each            | €15/yr            | 5     | 6.25        | 75.00       |
| 12    | Backup orchestration node (shared)   | CX22 — restic + cron + healthchecks   | €4.35             | 1     | 4.35        | 52.20       |
| 13    | Healthchecks.io                      | Hobbyist Free (20 checks)             | Free              | —     | 0.00        | 0.00        |
| **—** | **SUBTOTAL**                         |                                       |                   |       | **€76.55**  | **€918.60** |

### Tier A — Notes

- Total dominated by per-seat SaaS (GitHub, Notion).
- Self-hosting everything else keeps fixed infra under €5/mo.
- Brittle on observability: replace ASAP once you have ≥ 3 production projects.

---

## Tier B — Recommended DevOps Stack (Growing team, 5–15 engineers)

> Self-hosted observability cluster, paid Sentry & Doppler, full CI throughput.

| #     | Service                                    | Tier / Plan                           | Unit Price       | Qty   | Monthly (€) | Annual (€)    |
| ----- | ------------------------------------------ | ------------------------------------- | ---------------- | ----- | ----------- | ------------- |
| 1     | GitHub Team                                | Per seat                              | €3.67/user       | 12    | 44.04       | 528.48        |
| 2     | GitHub Actions overage (private repos)     | 20k min/mo above free                 | €0.008/min       | —     | 30.00       | 360.00        |
| 3     | Self-hosted GitHub Actions runner          | CX42                                  | €17.77           | 1     | 17.77       | 213.24        |
| 4     | GHCR storage                               | ~10 GB                                | €0.23/GB         | —     | 2.30        | 27.60         |
| 5     | Doppler Team                               | 10 seats                              | €18.00/seat      | 10    | 180.00      | 2,160.00      |
| 6     | Sentry Team                                | 50k events/mo                         | €26.00           | 1     | 26.00       | 312.00        |
| 7     | Observability cluster (self-hosted)        | CX42 — Grafana, Prometheus, Loki, Tempo | €17.77         | 1     | 17.77       | 213.24        |
| 8     | Observability volume                       | 500 GB                                | €26.00           | 1     | 26.00       | 312.00        |
| 9     | Off-site observability backup              | Storage Box BX21 — 500 GB             | €5.94            | 1     | 5.94        | 71.28         |
| 10    | UptimeRobot Pro                            | 1 plan                                | €7.00            | 1     | 7.00        | 84.00         |
| 11    | Status page (cstate self-hosted on Pages)  | Free                                  | Free             | —     | 0.00        | 0.00          |
| 12    | Cloudflare DNS / WAF                       | Free per zone                         | Free             | —     | 0.00        | 0.00          |
| 13    | Terraform state — Hetzner Object Storage   | ~1 GB                                 | €0.001/GB        | 1     | 0.10        | 1.20          |
| 14    | Notion Team                                | 12 seats                              | €9.50/user       | 12    | 114.00      | 1,368.00      |
| 15    | Slack Pro                                  | 12 seats                              | €6.75/user       | 12    | 81.00       | 972.00        |
| 16    | Domains @ United Domains                   | 10 avg                                | €15/yr           | 10    | 12.50       | 150.00        |
| 17    | Backup orchestration node                  | CX22                                  | €4.35            | 1     | 4.35        | 52.20         |
| 18    | Healthchecks.io Business                   | 100 checks                            | €18.00           | 1     | 18.00       | 216.00        |
| 19    | Review apps host (Coolify self-hosted)     | CX42                                  | €17.77           | 1     | 17.77       | 213.24        |
| **—** | **SUBTOTAL**                               |                                       |                  |       | **€604.54** | **€7,254.48** |

### Tier B — Notes

- Doppler + Notion + Slack drive ~62% of SaaS spend.
- Replacing Doppler with **self-hosted Vault** can save **€180/mo** (€2,160/yr) at the cost of one extra engineer-day per quarter for maintenance.
- Replacing Notion with **Outline self-hosted** can save **€114/mo** (€1,368/yr).
- Self-hosted alternatives reduce Tier B floor to ~**€310/mo**.

---

## Tier C — Full Engineering Org Stack (15+ engineers, multi-team)

> Multiple CI runners, full HA observability, premium SaaS for compliance & speed.

| #     | Service                                  | Tier / Plan                  | Unit Price        | Qty   | Monthly (€)   | Annual (€)     |
| ----- | ---------------------------------------- | ---------------------------- | ----------------- | ----- | ------------- | -------------- |
| 1     | GitHub Enterprise                        | Per seat                     | €19.25/user       | 25    | 481.25        | 5,775.00       |
| 2     | GitHub Actions overage                   | 100k min/mo                  | €0.008/min        | —     | 240.00        | 2,880.00       |
| 3     | Self-hosted runners pool                 | CX42                         | €17.77            | 3     | 53.31         | 639.72         |
| 4     | GHCR storage                             | ~50 GB                       | €0.23/GB          | —     | 11.50         | 138.00         |
| 5     | Doppler Pro                              | 25 seats                     | €36.00/seat       | 25    | 900.00        | 10,800.00      |
| 6     | Sentry Business                          | 250k events/mo               | €80.00            | 1     | 80.00         | 960.00         |
| 7     | Observability cluster (HA, 2 nodes)      | CX42                         | €17.77            | 2     | 35.54         | 426.48         |
| 8     | Observability volumes                    | 1 TB                         | €52.00            | 1     | 52.00         | 624.00         |
| 9     | Off-site obs backup                      | BX31 — 1 TB                  | €8.69             | 1     | 8.69          | 104.28         |
| 10    | Better Stack (uptime + incident)         | Team                         | €25.00            | 1     | 25.00         | 300.00         |
| 11    | Atlassian Statuspage                     | Starter                      | €27.00            | 1     | 27.00         | 324.00         |
| 12    | Cloudflare Business (org-wide)           | Per zone                     | €185.00           | 1     | 185.00        | 2,220.00       |
| 13    | Terraform Cloud Standard                 | 5 users                      | €18.50/user       | 5     | 92.50         | 1,110.00       |
| 14    | Notion Business                          | 25 seats                     | €14.00/user       | 25    | 350.00        | 4,200.00       |
| 15    | Slack Business+                          | 25 seats                     | €11.75/user       | 25    | 293.75        | 3,525.00       |
| 16    | Domains @ United Domains                 | 20 avg                       | €15/yr            | 20    | 25.00         | 300.00         |
| 17    | Backup orchestration node                | CX32                         | €8.21             | 1     | 8.21          | 98.52          |
| 18    | Healthchecks.io Business                 | 250 checks                   | €36.00            | 1     | 36.00         | 432.00         |
| 19    | Review apps host                         | CX42                         | €17.77            | 1     | 17.77         | 213.24         |
| 20    | PagerDuty                                | 10 seats                     | €21.00/user       | 10    | 210.00        | 2,520.00       |
| **—** | **SUBTOTAL**                             |                              |                   |       | **€3,131.52** | **€37,590.24** |

### Tier C — Notes

- GitHub Enterprise + Doppler Pro + PagerDuty + Notion are ~70% of total.
- Significant savings possible (~€1,200/mo) by self-hosting Doppler→Vault, Notion→Outline, Sentry→GlitchTip — at the cost of operational burden.
- This tier assumes LIONGATE has at least one dedicated DevOps/Platform engineer.

---

## DevOps Tier Comparison Dashboard

| Tier   | Setup Type                          | Monthly     | Annual       | Best for                              |
| ------ | ----------------------------------- | ----------- | ------------ | ------------------------------------- |
| **A**  | Free-tier-heavy, 2–5 engineers      | €76.55      | €918.60      | Year-1 LIONGATE                       |
| **B**  | Self-hosted obs, 5–15 engineers     | €604.54     | €7,254.48    | Year 2–3 growth phase                 |
| **C**  | Full org-grade, 15+ engineers       | €3,131.52   | €37,590.24   | Mature multi-team org                 |

---

## Per-Engineer Marginal Cost

Useful for forecasting hiring impact:

| Item                              | Per-engineer monthly | Per-engineer annual |
| --------------------------------- | -------------------- | ------------------- |
| GitHub seat (Team)                | €3.67                | €44.04              |
| GitHub seat (Enterprise)          | €19.25               | €231.00             |
| Doppler Team / Pro seat           | €18.00 / €36.00      | €216 / €432         |
| Notion Team / Business seat       | €9.50 / €14.00       | €114 / €168         |
| Slack Pro / Business+ seat        | €6.75 / €11.75       | €81 / €141          |
| Tailscale Business seat (if used) | €18.00               | €216.00             |
| PagerDuty seat                    | €21.00               | €252.00             |
| **Tier B all-in per engineer**    | **~€38**             | **~€455**           |
| **Tier C all-in per engineer**    | **~€114**            | **~€1,365**         |

> 💡 Use this table when planning headcount — every new engineer costs ~€455/yr (Tier B) to ~€1,365/yr (Tier C) in DevOps SaaS seats alone.

---

## Hidden & Variable Costs

| Cost Item                            | Frequency | Estimated Amount       | Notes                                          |
| ------------------------------------ | --------- | ---------------------- | ---------------------------------------------- |
| GitHub Actions overage               | Monthly   | +€0–€500               | Spikes with new release cadence                |
| Sentry event overage                 | Monthly   | +€26–€200              | Per 50k–250k event tier                        |
| GHCR/registry storage growth         | Monthly   | +€0.23/GB              | Especially with large Docker images            |
| Cloudflare Workers / R2              | Monthly   | €0–€50                 | If edge functions adopted                      |
| Vault/Doppler audit log retention    | Monthly   | +€10–€50               | If compliance enabled                          |
| SSO upgrade for SaaS (Notion, etc.)  | Monthly   | +€100–€500             | Triggered by enterprise plans                  |
| Domain renewal spikes                | Annual    | €100–€500              | When buying many domains for a launch          |
| Pen-test of DevOps stack             | Annual    | €1,500–€5,000          | Recommended every 12 months                    |

---

## Budget Planning Summary (DevOps Shared Tooling)

```
LIONGATE SARL — DevOps & Shared Tooling Budget Reference
========================================================

YEAR-1 STARTUP BUDGET:        €76.55     (Tier A — 2–5 engineers)
GROWTH-STAGE BUDGET:          €604.54    (Tier B — 5–15 engineers)
ORG-SCALE BUDGET:             €3,131.52  (Tier C — 15+ engineers)

YEAR-1 ANNUAL:                €918.60
GROWTH ANNUAL:                €7,254.48
ORG ANNUAL:                   €37,590.24

CONTINGENCY BUFFER (+15%, SaaS volatility high):
  Tier A: €918.60    × 1.15 = €1,056.39/yr
  Tier B: €7,254.48  × 1.15 = €8,342.65/yr
  Tier C: €37,590.24 × 1.15 = €43,228.78/yr

ALL-OPEN-SOURCE FLOOR (self-host everything you can):
  Year-1:  ~€35/mo   (~€420/yr)
  Growth:  ~€310/mo  (~€3,720/yr)
  Org:     ~€1,900/mo (~€22,800/yr)
```

---

## Decision Guide

| Question                                                | If YES → recommend                  |
| ------------------------------------------------------- | ----------------------------------- |
| < 5 engineers, no compliance pressure                   | Tier A                              |
| 5–15 engineers, ≥ 3 production clients                  | Tier B                              |
| Multi-team, on-call rotations, SOC2 or ISO planning     | Tier C                              |
| Want to minimise SaaS lock-in                           | Tier B with self-hosted Vault + Outline |
| Need formal incident management with paging             | Add PagerDuty (any tier)            |
| Strong cost pressure but observability is mandatory     | Tier B + self-hosted Grafana stack  |

---

_Document: PROFORMA-05 | Context: DevOps & Shared Tooling_
_Prices: Hetzner Cloud + published SaaS list rates (GitHub, Doppler, Sentry, Notion, Slack, Cloudflare)_
_Review: Quarterly; SaaS pricing changes regularly_
_Prepared by: LIONGATE SARL Engineering Team_
