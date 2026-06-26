# PROFORMA - Odoo Instance Infrastructure Billing

**LIONGATE SARL - Infrastructure Cost Estimation**
**Document:** PROFORMA-01
**Context:** Odoo ERP Deployment (Community Edition)
**Hosting Provider:** Hetzner Cloud
**Currency:** EUR (€)
**Version:** 1.0

---

> **How to read this document:**
> This proforma covers all infrastructure costs to deploy and operate a self-hosted Odoo 17.0 instance on Hetzner.
> Four tiers are presented: Minimum Viable, Recommended, Full Production, and High-Availability.
> All prices are based on Hetzner published rates. Monthly and yearly totals include a ±15% variance buffer.

---

## Tier 1 - Minimum Viable Setup

> Single server hosts both Odoo and PostgreSQL. Acceptable for internal low-traffic use only.
> **Not recommended for client-facing production.**

### Environment: Production (Minimum)

| #     | Resource                                      | Spec                                  | Unit Price | Qty | Monthly (€) | Annual (€)  |
| ----- | --------------------------------------------- | ------------------------------------- | ---------- | --- | ----------- | ----------- |
| 1     | Cloud Server (App + DB)                       | CX42 - 8 vCPU, 16 GB RAM, 160 GB NVMe | €17.77     | 1   | 17.77       | 213.24      |
| 2     | Hetzner Volume (Odoo filestore + DB overflow) | 100 GB block storage                  | €5.20      | 1   | 5.20        | 62.40       |
| 3     | Storage Box (Backups)                         | BX11 - 100 GB                         | €3.43      | 1   | 3.43        | 41.16       |
| 4     | Floating IP                                   | Static IPv4, reassignable             | €2.00      | 1   | 2.00        | 24.00       |
| 5     | SSL Certificate                               | Let's Encrypt - automated renewal     | Free       | 1   | 0.00        | 0.00        |
| 6     | Domain (amortised)                            | .com / .net - annual reg.             | €15.00/yr  | 1   | 1.25        | 15.00       |
| 7     | Outbound email (SMTP)                         | Brevo Free - 300 emails/day           | Free       | 1   | 0.00        | 0.00        |
| 8     | External uptime monitoring                    | UptimeRobot Free - 5 monitors         | Free       | 1   | 0.00        | 0.00        |
| **-** | **SUBTOTAL**                                  |                                       |            |     | **€29.65**  | **€355.80** |

### Environment: Staging (Minimum)

| #     | Resource                | Spec                                | Unit Price | Qty | Monthly (€) | Annual (€)  |
| ----- | ----------------------- | ----------------------------------- | ---------- | --- | ----------- | ----------- |
| 1     | Cloud Server (App + DB) | CX32 - 4 vCPU, 8 GB RAM, 80 GB NVMe | €8.21      | 1   | 8.21        | 98.52       |
| 2     | Floating IP             | Static IPv4                         | €2.00      | 1   | 2.00        | 24.00       |
| 3     | SSL Certificate         | Let's Encrypt                       | Free       | 1   | 0.00        | 0.00        |
| 4     | Domain subdomain        | staging.domain.com - same domain    | Included   | 1   | 0.00        | 0.00        |
| **-** | **SUBTOTAL**            |                                     |            |     | **€10.21**  | **€122.52** |

> 💡 **Tip:** Stop/pause staging server outside business hours to save ~60%. Effective staging cost ≈ €4.10/mo.

### Environment: Development

| #     | Resource                       | Spec                    | Monthly (€)       | Annual (€)         |
| ----- | ------------------------------ | ----------------------- | ----------------- | ------------------ |
| 1     | Local machine (Docker Desktop) | Developer workstation   | 0.00              | 0.00               |
| 2     | Optional shared dev VPS        | CX22 - 2 vCPU, 4 GB RAM | 4.35              | 52.20              |
| **-** | **SUBTOTAL**                   |                         | **€0.00 – €4.35** | **€0.00 – €52.20** |

---

### Tier 1 - Total Summary

| Environment      | Monthly MIN (€)            | Monthly MAX (€) | Annual MIN (€) | Annual MAX (€) |
| ---------------- | -------------------------- | --------------- | -------------- | -------------- |
| Production       | 29.65                      | 34.10           | 355.80         | 409.20         |
| Staging          | 4.10 _(stopped off-hours)_ | 10.21           | 49.20          | 122.52         |
| Development      | 0.00                       | 4.35            | 0.00           | 52.20          |
| **TOTAL TIER 1** | **€33.75**                 | **€48.66**      | **€405.00**    | **€583.92**    |

---

## Tier 2 - Recommended Setup

> App and database on separate servers. Production-grade. Suitable for most LIONGATE client Odoo deployments.

### Environment: Production (Recommended)

| #     | Resource                         | Spec                            | Unit Price | Qty | Monthly (€) | Annual (€)  |
| ----- | -------------------------------- | ------------------------------- | ---------- | --- | ----------- | ----------- |
| 1     | App Server                       | CX42 - 8 vCPU, 16 GB RAM        | €17.77     | 1   | 17.77       | 213.24      |
| 2     | DB Server                        | CX32 - 4 vCPU, 8 GB RAM         | €8.21      | 1   | 8.21        | 98.52       |
| 3     | Hetzner Volume (PostgreSQL data) | 100 GB - attached to DB server  | €5.20      | 1   | 5.20        | 62.40       |
| 4     | Hetzner Volume (Odoo filestore)  | 50 GB - attached to App server  | €2.60      | 1   | 2.60        | 31.20       |
| 5     | Private Network (VLAN)           | App ↔ DB isolated communication | Free       | 1   | 0.00        | 0.00        |
| 6     | Storage Box (Backups)            | BX31 - 1 TB                     | €8.69      | 1   | 8.69        | 104.28      |
| 7     | Floating IP                      | Static IPv4                     | €2.00      | 1   | 2.00        | 24.00       |
| 8     | SSL Certificate                  | Let's Encrypt                   | Free       | 1   | 0.00        | 0.00        |
| 9     | Domain (amortised)               | Annual registration             | €15.00/yr  | 1   | 1.25        | 15.00       |
| 10    | Outbound email (SMTP)            | Brevo Starter - up to 20k/mo    | €19.00     | 1   | 19.00       | 228.00      |
| 11    | External uptime monitoring       | UptimeRobot Free                | Free       | 1   | 0.00        | 0.00        |
| **-** | **SUBTOTAL**                     |                                 |            |     | **€64.72**  | **€776.64** |

### Environment: Staging (Recommended)

| #     | Resource                         | Spec                    | Unit Price | Qty | Monthly (€) | Annual (€)  |
| ----- | -------------------------------- | ----------------------- | ---------- | --- | ----------- | ----------- |
| 1     | Cloud Server (App + DB combined) | CX32 - 4 vCPU, 8 GB RAM | €8.21      | 1   | 8.21        | 98.52       |
| 2     | Volume (data)                    | 50 GB                   | €2.60      | 1   | 2.60        | 31.20       |
| 3     | Floating IP                      | Static IPv4             | €2.00      | 1   | 2.00        | 24.00       |
| 4     | SSL Certificate                  | Let's Encrypt           | Free       | 1   | 0.00        | 0.00        |
| **-** | **SUBTOTAL**                     |                         |            |     | **€12.81**  | **€153.72** |

> 💡 Pause staging server off-hours → effective cost ≈ **€5.12/mo**

### Environment: Development

| #     | Resource                  | Spec                 | Monthly (€)       | Annual (€)         |
| ----- | ------------------------- | -------------------- | ----------------- | ------------------ |
| 1     | Local Docker (dev)        | All services locally | 0.00              | 0.00               |
| 2     | Shared dev VPS (optional) | CX22 - team-shared   | 4.35              | 52.20              |
| **-** | **SUBTOTAL**              |                      | **€0.00 – €4.35** | **€0.00 – €52.20** |

---

### Tier 2 - Total Summary

| Environment      | Monthly MIN (€)           | Monthly MAX (€) | Annual MIN (€) | Annual MAX (€) |
| ---------------- | ------------------------- | --------------- | -------------- | -------------- |
| Production       | 64.72                     | 74.43           | 776.64         | 893.16         |
| Staging          | 5.12 _(paused off-hours)_ | 12.81           | 61.44          | 153.72         |
| Development      | 0.00                      | 4.35            | 0.00           | 52.20          |
| **TOTAL TIER 2** | **€69.84**                | **€91.59**      | **€838.08**    | **€1,099.08**  |

---

## Tier 3 - Full Production Setup

> Separate high-spec servers, larger volumes, full monitoring server, SMTP provider, production hardening.

### Environment: Production (Full)

| #     | Resource                               | Spec                               | Unit Price | Qty | Monthly (€) | Annual (€)    |
| ----- | -------------------------------------- | ---------------------------------- | ---------- | --- | ----------- | ------------- |
| 1     | App Server                             | CX42 - 8 vCPU, 16 GB RAM           | €17.77     | 1   | 17.77       | 213.24        |
| 2     | DB Server                              | CX42 - 8 vCPU, 16 GB RAM           | €17.77     | 1   | 17.77       | 213.24        |
| 3     | Volume - DB data                       | 200 GB attached to DB server       | €10.40     | 1   | 10.40       | 124.80        |
| 4     | Volume - Odoo filestore                | 100 GB attached to App server      | €5.20      | 1   | 5.20        | 62.40         |
| 5     | Private Network                        | App ↔ DB isolated                  | Free       | 1   | 0.00        | 0.00          |
| 6     | Storage Box (Backup)                   | BX31 - 1 TB                        | €8.69      | 1   | 8.69        | 104.28        |
| 7     | Object Storage (Long-term archive)     | S3-compatible - 50 GB avg          | €0.60      | 1   | 0.60        | 7.20          |
| 8     | Floating IP                            | Static IPv4                        | €2.00      | 1   | 2.00        | 24.00         |
| 9     | Monitoring Server (shared)             | CX22 - Grafana + Prometheus + Loki | €4.35      | 1   | 4.35        | 52.20         |
| 10    | SSL Certificate                        | Let's Encrypt                      | Free       | 1   | 0.00        | 0.00          |
| 11    | Domain (amortised)                     | Annual registration                | €15.00/yr  | 1   | 1.25        | 15.00         |
| 12    | Outbound email (SMTP)                  | Brevo Starter - 20k/mo             | €19.00     | 1   | 19.00       | 228.00        |
| 13    | External uptime monitoring             | UptimeRobot Pro - 50 monitors      | €7.00      | 1   | 7.00        | 84.00         |
| 14    | VM Snapshot (weekly rotation - 2 kept) | ~50 GB average                     | €0.60      | 1   | 0.60        | 7.20          |
| **-** | **SUBTOTAL**                           |                                    |            |     | **€94.63**  | **€1,135.56** |

### Environment: Staging

| #     | Resource     | Spec            | Monthly (€) | Annual (€)  |
| ----- | ------------ | --------------- | ----------- | ----------- |
| 1     | Cloud Server | CX32 - app + DB | 8.21        | 98.52       |
| 2     | Volume       | 50 GB           | 2.60        | 31.20       |
| 3     | Floating IP  | Static IPv4     | 2.00        | 24.00       |
| 4     | SSL          | Let's Encrypt   | 0.00        | 0.00        |
| **-** | **SUBTOTAL** |                 | **€12.81**  | **€153.72** |

### Environment: Development

| #     | Resource           | Spec                   | Monthly (€) | Annual (€) |
| ----- | ------------------ | ---------------------- | ----------- | ---------- |
| 1     | Local Docker (dev) | Developer workstations | 0.00        | 0.00       |
| **-** | **SUBTOTAL**       |                        | **€0.00**   | **€0.00**  |

---

### Tier 3 - Total Summary

| Environment      | Monthly MIN (€) | Monthly MAX (€) | Annual MIN (€) | Annual MAX (€) |
| ---------------- | --------------- | --------------- | -------------- | -------------- |
| Production       | 94.63           | 108.82          | 1,135.56       | 1,305.84       |
| Staging          | 5.12 _(paused)_ | 12.81           | 61.44          | 153.72         |
| Development      | 0.00            | 4.35            | 0.00           | 52.20          |
| **TOTAL TIER 3** | **€99.75**      | **€125.98**     | **€1,197.00**  | **€1,511.76**  |

---

## Tier 4 - High-Availability Setup

> Load-balanced, CPU-dedicated servers, full monitoring, maximum resilience.

### Environment: Production (HA)

| #     | Resource                      | Spec                                | Unit Price | Qty | Monthly (€) | Annual (€)    |
| ----- | ----------------------------- | ----------------------------------- | ---------- | --- | ----------- | ------------- |
| 1     | App Server                    | CCX23 - 4 vCPU dedicated, 16 GB RAM | €22.75     | 1   | 22.75       | 273.00        |
| 2     | DB Server                     | CCX23 - 4 vCPU dedicated, 16 GB RAM | €22.75     | 1   | 22.75       | 273.00        |
| 3     | Load Balancer                 | LB11 - 5 targets                    | €5.39      | 1   | 5.39        | 64.68         |
| 4     | Volume - DB data              | 300 GB                              | €15.60     | 1   | 15.60       | 187.20        |
| 5     | Volume - Odoo filestore       | 150 GB                              | €7.80      | 1   | 7.80        | 93.60         |
| 6     | Private Network               | VLAN                                | Free       | 1   | 0.00        | 0.00          |
| 7     | Storage Box (Backup)          | BX61 - 5 TB                         | €34.45     | 1   | 34.45       | 413.40        |
| 8     | Object Storage (Archive)      | S3-compatible - 100 GB avg          | €1.20      | 1   | 1.20        | 14.40         |
| 9     | Floating IP × 2               | Static IPv4 (app + LB)              | €2.00      | 2   | 4.00        | 48.00         |
| 10    | Monitoring Server (dedicated) | CX32 - Grafana full stack           | €8.21      | 1   | 8.21        | 98.52         |
| 11    | SSL Certificate               | Let's Encrypt wildcard              | Free       | 1   | 0.00        | 0.00          |
| 12    | Domain (amortised)            | Annual registration                 | €15.00/yr  | 1   | 1.25        | 15.00         |
| 13    | Outbound email (SMTP)         | Brevo Business - 100k/mo            | €49.00     | 1   | 49.00       | 588.00        |
| 14    | External uptime monitoring    | UptimeRobot Pro                     | €7.00      | 1   | 7.00        | 84.00         |
| 15    | VM Snapshots (rotation ×4)    | ~150 GB avg                         | €1.79      | 1   | 1.79        | 21.48         |
| 16    | Cloudflare Pro (CDN + WAF)    | DDoS protection, proxy              | €20.00     | 1   | 20.00       | 240.00        |
| **-** | **SUBTOTAL**                  |                                     |            |     | **€201.19** | **€2,414.28** |

---

### Tier 4 - Total Summary

| Environment      | Monthly MIN (€) | Monthly MAX (€) | Annual MIN (€) | Annual MAX (€) |
| ---------------- | --------------- | --------------- | -------------- | -------------- |
| Production       | 201.19          | 231.37          | 2,414.28       | 2,776.44       |
| Staging          | 5.12            | 12.81           | 61.44          | 153.72         |
| Development      | 0.00            | 4.35            | 0.00           | 52.20          |
| **TOTAL TIER 4** | **€206.31**     | **€248.53**     | **€2,475.72**  | **€2,982.36**  |

---

## Odoo Instance - Tier Comparison Dashboard

| Tier   | Setup Type                  | Monthly MIN | Monthly MAX | Annual MIN | Annual MAX | Best for                |
| ------ | --------------------------- | ----------- | ----------- | ---------- | ---------- | ----------------------- |
| **T1** | Minimum viable - all-in-one | €33.75      | €48.66      | €405       | €584       | Internal only, low risk |
| **T2** | Recommended - split app+db  | €69.84      | €91.59      | €838       | €1,099     | Most client projects    |
| **T3** | Full production             | €99.75      | €125.98     | €1,197     | €1,512     | Active business ERP     |
| **T4** | High-availability           | €206.31     | €248.53     | €2,476     | €2,982     | Enterprise / SLA-bound  |

---

## Hidden & Variable Costs

| Cost Item                                 | Frequency            | Estimated Amount      | Notes                                        |
| ----------------------------------------- | -------------------- | --------------------- | -------------------------------------------- |
| Domain renewal                            | Annual               | €10–€30               | Per domain registered                        |
| Bandwidth overage                         | Rare                 | €0 (included > 20 TB) | Standard Hetzner allowance                   |
| Snapshot storage growth                   | Monthly              | +€0.50–€2.00          | Grows with server/DB size                    |
| Odoo Enterprise license                   | Annual (if upgraded) | ~€240/user/yr         | Community = free; Enterprise = paid          |
| PostgreSQL DBA support                    | Per incident         | Variable              | If Hetzner Managed DB used: +€20–€60/mo      |
| Database read replica                     | Monthly              | +€8.21–€22.75         | Only for read-heavy workloads                |
| PgBouncer server                          | Monthly              | +€4.35                | Only if connection pool exhaustion occurs    |
| SSL commercial (if required)              | Annual               | €50–€300              | Only if client mandates non-Let's Encrypt    |
| Hetzner API overage                       | Rare                 | €0 (generous limits)  | Standard usage well within limits            |
| Disaster recovery drill (external server) | Quarterly            | €2–€8                 | Temporary test server for restore validation |

---

## Scaling Cost Reference

| Trigger                     | Current             | Upgrade to           | Added Monthly Cost |
| --------------------------- | ------------------- | -------------------- | ------------------ |
| Odoo app server at >80% CPU | CX42 (€17.77)       | CCX23 (€22.75)       | +€4.98             |
| Odoo app server at >90% RAM | CX42 (€17.77)       | CX52 (€35.54)        | +€17.77            |
| DB at >80% disk             | Volume 100 GB       | Volume 200 GB        | +€5.20             |
| DB at >80% CPU              | CX32 (€8.21)        | CX42 (€17.77)        | +€9.56             |
| High email volume (>20k/mo) | Brevo Starter (€19) | Brevo Business (€49) | +€30.00            |
| Filestore at >80%           | Volume 50 GB        | Volume 100 GB        | +€2.60             |

---

## Budget Planning Summary (Odoo Instance)

```
LIONGATE SARL - Odoo Instance Budget Reference
===============================================

MINIMUM MONTHLY BUDGET:       €33.75   (Tier 1 - single server)
RECOMMENDED MONTHLY BUDGET:   €69.84   (Tier 2 - split, all envs)
FULL PRODUCTION BUDGET:       €99.75   (Tier 3 - full stack)
HIGH-AVAILABILITY BUDGET:     €206.31  (Tier 4 - HA, all envs)

MINIMUM ANNUAL BUDGET:        €405.00
RECOMMENDED ANNUAL BUDGET:    €838.08
FULL PRODUCTION ANNUAL:       €1,197.00
HIGH-AVAILABILITY ANNUAL:     €2,475.72

CONTINGENCY BUFFER (+15%):
  Tier 2 recommended:  €838.08 × 1.15 = €963.79/yr
  Tier 3 full:         €1,197 × 1.15  = €1,376.55/yr
```

---

_Document: PROFORMA-01 | Context: Odoo Instance_
_Prices: Hetzner Cloud published rates + market SMTP/monitoring rates_
_Review: Quarterly or on Hetzner pricing change_
_Prepared by: LIONGATE SARL Engineering Team_
