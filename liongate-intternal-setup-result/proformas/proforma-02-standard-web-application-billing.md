# PROFORMA — Standard Web Application Infrastructure Billing

**LIONGATE SARL — Infrastructure Cost Estimation**
**Document:** PROFORMA-02
**Context:** Standard Web Application (Backend API + Frontend + Database)
**Hosting Provider:** Hetzner Cloud
**Currency:** EUR (€)
**Version:** 1.0

---

> **How to read this document:**
> This proforma covers all infrastructure costs to deploy and operate a standard web application
> (e.g., NestJS API + Next.js frontend + PostgreSQL) across Development, Staging, and Production
> environments. Four tiers are presented from minimum viable to high-availability.

---

## Reference Stack

| Layer         | Technology                              | Notes                |
| ------------- | --------------------------------------- | -------------------- |
| Backend       | NestJS (TypeScript) or FastAPI (Python) | Single API service   |
| Frontend      | Next.js (React)                         | SSR or static export |
| Database      | PostgreSQL 16                           | Primary data store   |
| Cache         | Redis (optional)                        | Sessions, queues     |
| Reverse Proxy | Nginx                                   | SSL termination      |
| Container     | Docker + Docker Compose                 | All services         |
| CI/CD         | GitHub Actions                          | Build + deploy       |
| SSL           | Let's Encrypt                           | Free, auto-renew     |

---

## Tier 1 — Minimum Viable Setup

> App + DB on a single server. For low-traffic apps, MVPs, or early-stage client projects.
> **Traffic assumption:** < 1,000 requests/day, < 100 concurrent users.

### Environment: Production (Minimum)

| #     | Resource                      | Spec                                | Unit Price | Qty | Monthly (€) | Annual (€)  |
| ----- | ----------------------------- | ----------------------------------- | ---------- | --- | ----------- | ----------- |
| 1     | Cloud Server (App + DB)       | CX32 — 4 vCPU, 8 GB RAM, 80 GB NVMe | €8.21      | 1   | 8.21        | 98.52       |
| 2     | Hetzner Volume (DB + uploads) | 50 GB block storage                 | €2.60      | 1   | 2.60        | 31.20       |
| 3     | Storage Box (Backups)         | BX11 — 100 GB                       | €3.43      | 1   | 3.43        | 41.16       |
| 4     | Floating IP                   | Static IPv4                         | €2.00      | 1   | 2.00        | 24.00       |
| 5     | SSL Certificate               | Let's Encrypt                       | Free       | 1   | 0.00        | 0.00        |
| 6     | Domain (amortised)            | .com annual registration            | €15.00/yr  | 1   | 1.25        | 15.00       |
| 7     | Outbound email                | Brevo Free — 300/day                | Free       | 1   | 0.00        | 0.00        |
| 8     | External uptime monitoring    | UptimeRobot Free                    | Free       | 1   | 0.00        | 0.00        |
| **—** | **SUBTOTAL**                  |                                     |            |     | **€17.49**  | **€209.88** |

### Environment: Staging

| #     | Resource                | Spec                    | Monthly (€) | Annual (€) |
| ----- | ----------------------- | ----------------------- | ----------- | ---------- |
| 1     | Cloud Server (App + DB) | CX22 — 2 vCPU, 4 GB RAM | 4.35        | 52.20      |
| 2     | Floating IP             | Static IPv4             | 2.00        | 24.00      |
| 3     | SSL                     | Let's Encrypt           | 0.00        | 0.00       |
| **—** | **SUBTOTAL**            |                         | **€6.35**   | **€76.20** |

> 💡 Stop staging server off-hours → effective cost ≈ **€2.54/mo**

### Environment: Development

| #     | Resource                  | Spec                  | Monthly (€)       | Annual (€)         |
| ----- | ------------------------- | --------------------- | ----------------- | ------------------ |
| 1     | Local Docker Compose      | Developer workstation | 0.00              | 0.00               |
| 2     | Shared dev VPS (optional) | CX22 — team-shared    | 4.35              | 52.20              |
| **—** | **SUBTOTAL**              |                       | **€0.00 – €4.35** | **€0.00 – €52.20** |

### Tier 1 — Total Summary

| Environment      | Monthly MIN (€) | Monthly MAX (€) | Annual MIN (€) | Annual MAX (€) |
| ---------------- | --------------- | --------------- | -------------- | -------------- |
| Production       | 17.49           | 20.11           | 209.88         | 241.32         |
| Staging          | 2.54 _(paused)_ | 6.35            | 30.48          | 76.20          |
| Development      | 0.00            | 4.35            | 0.00           | 52.20          |
| **TOTAL TIER 1** | **€20.03**      | **€30.81**      | **€240.36**    | **€369.72**    |

---

## Tier 2 — Recommended Setup

> App and database on separate servers. Standard production-grade for most client projects.
> **Traffic assumption:** 1,000–10,000 requests/day, up to 300 concurrent users.

### Environment: Production (Recommended)

| #     | Resource                            | Spec                           | Unit Price | Qty | Monthly (€) | Annual (€)  |
| ----- | ----------------------------------- | ------------------------------ | ---------- | --- | ----------- | ----------- |
| 1     | App Server (API + Frontend)         | CX42 — 8 vCPU, 16 GB RAM       | €17.77     | 1   | 17.77       | 213.24      |
| 2     | DB Server                           | CX32 — 4 vCPU, 8 GB RAM        | €8.21      | 1   | 8.21        | 98.52       |
| 3     | Volume — DB data                    | 100 GB — attached to DB server | €5.20      | 1   | 5.20        | 62.40       |
| 4     | Volume — Media / Uploads            | 50 GB — attached to App server | €2.60      | 1   | 2.60        | 31.20       |
| 5     | Private Network                     | App ↔ DB VLAN                  | Free       | 1   | 0.00        | 0.00        |
| 6     | Storage Box (Backups)               | BX31 — 1 TB                    | €8.69      | 1   | 8.69        | 104.28      |
| 7     | Floating IP                         | Static IPv4                    | €2.00      | 1   | 2.00        | 24.00       |
| 8     | SSL Certificate                     | Let's Encrypt                  | Free       | 1   | 0.00        | 0.00        |
| 9     | Domain (amortised)                  | Annual registration            | €15.00/yr  | 1   | 1.25        | 15.00       |
| 10    | Outbound email                      | Brevo Starter — 20k/mo         | €19.00     | 1   | 19.00       | 228.00      |
| 11    | External uptime monitoring          | UptimeRobot Free               | Free       | 1   | 0.00        | 0.00        |
| 12    | Object Storage (media CDN fallback) | S3 — 20 GB avg                 | €0.24      | 1   | 0.24        | 2.88        |
| **—** | **SUBTOTAL**                        |                                |            |     | **€64.96**  | **€779.52** |

### Environment: Staging

| #     | Resource                | Spec          | Monthly (€) | Annual (€)  |
| ----- | ----------------------- | ------------- | ----------- | ----------- |
| 1     | Cloud Server (App + DB) | CX32          | 8.21        | 98.52       |
| 2     | Volume                  | 50 GB         | 2.60        | 31.20       |
| 3     | Floating IP             | Static IPv4   | 2.00        | 24.00       |
| 4     | SSL                     | Let's Encrypt | 0.00        | 0.00        |
| **—** | **SUBTOTAL**            |               | **€12.81**  | **€153.72** |

### Environment: Development

| #     | Resource                  | Spec                  | Monthly (€)       | Annual (€)         |
| ----- | ------------------------- | --------------------- | ----------------- | ------------------ |
| 1     | Local Docker Compose      | Developer workstation | 0.00              | 0.00               |
| 2     | Shared dev VPS (optional) | CX22                  | 4.35              | 52.20              |
| **—** | **SUBTOTAL**              |                       | **€0.00 – €4.35** | **€0.00 – €52.20** |

### Tier 2 — Total Summary

| Environment      | Monthly MIN (€) | Monthly MAX (€) | Annual MIN (€) | Annual MAX (€) |
| ---------------- | --------------- | --------------- | -------------- | -------------- |
| Production       | 64.96           | 74.70           | 779.52         | 896.40         |
| Staging          | 5.12 _(paused)_ | 12.81           | 61.44          | 153.72         |
| Development      | 0.00            | 4.35            | 0.00           | 52.20          |
| **TOTAL TIER 2** | **€70.08**      | **€91.86**      | **€840.96**    | **€1,102.32**  |

---

## Tier 3 — Full Production Setup

> Full observability, Redis cache, dedicated monitoring, production hardening.
> **Traffic assumption:** 10,000–50,000 requests/day, up to 1,000 concurrent users.

### Environment: Production (Full)

| #     | Resource                     | Spec                               | Unit Price | Qty | Monthly (€) | Annual (€)    |
| ----- | ---------------------------- | ---------------------------------- | ---------- | --- | ----------- | ------------- |
| 1     | App Server                   | CX42 — 8 vCPU, 16 GB RAM           | €17.77     | 1   | 17.77       | 213.24        |
| 2     | DB Server                    | CX42 — 8 vCPU, 16 GB RAM           | €17.77     | 1   | 17.77       | 213.24        |
| 3     | Redis Server                 | CX22 — 2 vCPU, 4 GB RAM            | €4.35      | 1   | 4.35        | 52.20         |
| 4     | Volume — DB data             | 200 GB                             | €10.40     | 1   | 10.40       | 124.80        |
| 5     | Volume — Media / Uploads     | 100 GB                             | €5.20      | 1   | 5.20        | 62.40         |
| 6     | Private Network              | VLAN all servers                   | Free       | 1   | 0.00        | 0.00          |
| 7     | Storage Box (Backups)        | BX31 — 1 TB                        | €8.69      | 1   | 8.69        | 104.28        |
| 8     | Object Storage (media)       | S3 — 50 GB avg                     | €0.60      | 1   | 0.60        | 7.20          |
| 9     | Floating IP                  | Static IPv4                        | €2.00      | 1   | 2.00        | 24.00         |
| 10    | Monitoring Server (shared)   | CX22 — Grafana + Prometheus + Loki | €4.35      | 1   | 4.35        | 52.20         |
| 11    | SSL Certificate              | Let's Encrypt                      | Free       | 1   | 0.00        | 0.00          |
| 12    | Domain (amortised)           | Annual                             | €15.00/yr  | 1   | 1.25        | 15.00         |
| 13    | Outbound email               | Brevo Starter — 20k/mo             | €19.00     | 1   | 19.00       | 228.00        |
| 14    | External uptime monitoring   | UptimeRobot Pro                    | €7.00      | 1   | 7.00        | 84.00         |
| 15    | VM Snapshot (weekly, 2 kept) | ~40 GB avg                         | €0.48      | 1   | 0.48        | 5.76          |
| **—** | **SUBTOTAL**                 |                                    |            |     | **€98.86**  | **€1,186.32** |

### Environment: Staging

| #     | Resource     | Spec            | Monthly (€) | Annual (€)  |
| ----- | ------------ | --------------- | ----------- | ----------- |
| 1     | Cloud Server | CX32 — app + DB | 8.21        | 98.52       |
| 2     | Volume       | 50 GB           | 2.60        | 31.20       |
| 3     | Floating IP  | Static IPv4     | 2.00        | 24.00       |
| **—** | **SUBTOTAL** |                 | **€12.81**  | **€153.72** |

### Tier 3 — Total Summary

| Environment      | Monthly MIN (€) | Monthly MAX (€) | Annual MIN (€) | Annual MAX (€) |
| ---------------- | --------------- | --------------- | -------------- | -------------- |
| Production       | 98.86           | 113.69          | 1,186.32       | 1,364.28       |
| Staging          | 5.12 _(paused)_ | 12.81           | 61.44          | 153.72         |
| Development      | 0.00            | 4.35            | 0.00           | 52.20          |
| **TOTAL TIER 3** | **€103.98**     | **€130.85**     | **€1,247.76**  | **€1,570.20**  |

---

## Tier 4 — High-Availability Setup

> Load-balanced multi-instance app, dedicated CPU DB, CDN, full HA configuration.
> **Traffic assumption:** 50,000+ requests/day, 2,000+ concurrent users.

### Environment: Production (HA)

| #     | Resource                            | Spec                                | Unit Price | Qty | Monthly (€) | Annual (€)    |
| ----- | ----------------------------------- | ----------------------------------- | ---------- | --- | ----------- | ------------- |
| 1     | App Servers (×2, behind LB)         | CX42 — 8 vCPU, 16 GB RAM            | €17.77     | 2   | 35.54       | 426.48        |
| 2     | DB Server (dedicated CPU)           | CCX23 — 4 vCPU dedicated, 16 GB RAM | €22.75     | 1   | 22.75       | 273.00        |
| 3     | Redis Server                        | CX32 — 4 vCPU, 8 GB RAM             | €8.21      | 1   | 8.21        | 98.52         |
| 4     | Load Balancer                       | LB21 — up to 25 targets             | €10.79     | 1   | 10.79       | 129.48        |
| 5     | Volume — DB data                    | 300 GB                              | €15.60     | 1   | 15.60       | 187.20        |
| 6     | Volume — Shared uploads (×2 apps)   | 200 GB each                         | €10.40     | 2   | 20.80       | 249.60        |
| 7     | Private Network                     | VLAN                                | Free       | 1   | 0.00        | 0.00          |
| 8     | Storage Box (Backups)               | BX31 — 1 TB                         | €8.69      | 1   | 8.69        | 104.28        |
| 9     | Object Storage (media + CDN origin) | S3 — 100 GB avg                     | €1.20      | 1   | 1.20        | 14.40         |
| 10    | Floating IP × 2                     | Static IPv4 (app + LB)              | €2.00      | 2   | 4.00        | 48.00         |
| 11    | Monitoring Server (dedicated)       | CX32 — Grafana full stack           | €8.21      | 1   | 8.21        | 98.52         |
| 12    | SSL Certificate                     | Let's Encrypt wildcard              | Free       | 1   | 0.00        | 0.00          |
| 13    | Cloudflare Pro (CDN + WAF)          | DDoS + proxy                        | €20.00     | 1   | 20.00       | 240.00        |
| 14    | Domain (amortised)                  | Annual                              | €15.00/yr  | 1   | 1.25        | 15.00         |
| 15    | Outbound email                      | Brevo Business — 100k/mo            | €49.00     | 1   | 49.00       | 588.00        |
| 16    | External uptime monitoring          | UptimeRobot Pro                     | €7.00      | 1   | 7.00        | 84.00         |
| 17    | VM Snapshots (rotation ×4)          | ~80 GB avg                          | €0.95      | 1   | 0.95        | 11.40         |
| **—** | **SUBTOTAL**                        |                                     |            |     | **€213.99** | **€2,567.88** |

### Tier 4 — Total Summary

| Environment      | Monthly MIN (€) | Monthly MAX (€) | Annual MIN (€) | Annual MAX (€) |
| ---------------- | --------------- | --------------- | -------------- | -------------- |
| Production       | 213.99          | 246.09          | 2,567.88       | 2,953.08       |
| Staging          | 5.12 _(paused)_ | 12.81           | 61.44          | 153.72         |
| Development      | 0.00            | 4.35            | 0.00           | 52.20          |
| **TOTAL TIER 4** | **€219.11**     | **€263.25**     | **€2,629.32**  | **€3,159.00**  |

---

## Standard Web App — Tier Comparison Dashboard

| Tier   | Setup Type              | Monthly MIN | Monthly MAX | Annual MIN | Annual MAX | Best for                |
| ------ | ----------------------- | ----------- | ----------- | ---------- | ---------- | ----------------------- |
| **T1** | MVP — all-in-one CX32   | €20.03      | €30.81      | €240       | €370       | Prototype, MVP          |
| **T2** | Recommended — split     | €70.08      | €91.86      | €841       | €1,102     | Standard client project |
| **T3** | Full production + Redis | €103.98     | €130.85     | €1,248     | €1,570     | Active SaaS product     |
| **T4** | High-availability, LB   | €219.11     | €263.25     | €2,629     | €3,159     | High-traffic platform   |

---

## Hidden & Variable Costs

| Cost Item                    | Frequency | Estimated Amount             | Notes                              |
| ---------------------------- | --------- | ---------------------------- | ---------------------------------- |
| Domain renewal               | Annual    | €10–€30                      | Per domain                         |
| Snapshot storage growth      | Monthly   | +€0.50–€3.00                 | Grows with server size             |
| Additional subdomains        | Annual    | Included in domain           | Same registrar zone                |
| DB read replica (future)     | Monthly   | +€8.21–€22.75                | For read-heavy scaling             |
| Redis cluster (future)       | Monthly   | +€8.21–€17.77                | For high-availability cache        |
| Commercial SSL (if required) | Annual    | €50–€300                     | Only if client mandates            |
| Cloudflare Workers / Pages   | Monthly   | €0–€20                       | For edge compute or static hosting |
| GitHub Actions CI minutes    | Monthly   | €0 (free tier) or €0.008/min | 2,000 free min/mo                  |
| Container registry storage   | Monthly   | €0–€5                        | GHCR included with GitHub          |

---

## Budget Planning Summary (Standard Web App)

```
LIONGATE SARL — Standard Web App Budget Reference
==================================================

MINIMUM MONTHLY BUDGET:       €20.03   (Tier 1 — MVP / all-in-one)
RECOMMENDED MONTHLY BUDGET:   €70.08   (Tier 2 — split app+db)
FULL PRODUCTION BUDGET:       €103.98  (Tier 3 — Redis + monitoring)
HIGH-AVAILABILITY BUDGET:     €219.11  (Tier 4 — LB + multi-app)

MINIMUM ANNUAL BUDGET:        €240.36
RECOMMENDED ANNUAL BUDGET:    €840.96
FULL PRODUCTION ANNUAL:       €1,247.76
HIGH-AVAILABILITY ANNUAL:     €2,629.32

CONTINGENCY BUFFER (+15%):
  Tier 2 recommended:  €840.96 × 1.15 = €967.10/yr
  Tier 3 full:         €1,247.76 × 1.15 = €1,434.92/yr
```

---

_Document: PROFORMA-02 | Context: Standard Web Application_
_Prices: Hetzner Cloud published rates + market SMTP/monitoring rates_
_Review: Quarterly or on Hetzner pricing change_
_Prepared by: LIONGATE SARL Engineering Team_
