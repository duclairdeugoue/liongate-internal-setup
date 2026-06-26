# PROFORMA — High-Traffic Client Project Infrastructure Billing

**LIONGATE SARL — Infrastructure Cost Estimation**
**Document:** PROFORMA-03
**Context:** High-Traffic Client Project (Public SaaS / E-commerce / Multi-tenant Platform)
**Hosting Provider:** Hetzner Cloud (+ Cloudflare edge)
**Currency:** EUR (€)
**Version:** 1.0

---

> **How to read this document:**
> This proforma covers infrastructure costs for client projects that must serve **high concurrent
> traffic**, with strong **availability, performance, and security guarantees** (SLA-backed).
> Four tiers are presented, scaling from "launching public product" to "regional-scale platform".

---

## Reference Stack

| Layer            | Technology                                | Notes                                   |
| ---------------- | ----------------------------------------- | --------------------------------------- |
| Edge / CDN / WAF | Cloudflare Pro/Business                   | DDoS, caching, WAF, image optimization  |
| Load Balancer    | Hetzner LB21 / LB31                       | TCP+HTTP, multi-target, health checks   |
| App tier         | Node.js / NestJS / Next.js                | Horizontally scaled (≥2 nodes)          |
| DB primary       | PostgreSQL 16 (dedicated CPU)             | CCX-series, tuned for OLTP              |
| DB replica       | PostgreSQL streaming replica              | Read scaling + standby                  |
| Cache / Queue    | Redis 7                                   | Sessions, queue, rate limiting          |
| Object storage   | Hetzner Object Storage (S3-compatible)    | Media, exports, uploads                 |
| Search (opt.)    | Meilisearch / OpenSearch                  | Product/document search                 |
| Observability    | Grafana + Prometheus + Loki + Alertmanager | Dedicated monitoring node               |
| CI/CD            | GitHub Actions + self-hosted runner       | Build + deploy + DB migration gates     |
| Secrets          | Doppler / HashiCorp Vault (cloud)         | Production-grade secret management      |

---

## Tier 1 — Launch Setup (Single-region, scalable)

> Production-ready launch for a public client product. Single AZ but split tiers.
> **Traffic assumption:** 50k–150k requests/day, 1,000–2,000 concurrent users, < 50 req/s sustained.

### Environment: Production (Launch)

| #     | Resource                            | Spec                                | Unit Price | Qty | Monthly (€) | Annual (€)    |
| ----- | ----------------------------------- | ----------------------------------- | ---------- | --- | ----------- | ------------- |
| 1     | App Servers (behind LB)             | CX42 — 8 vCPU, 16 GB RAM            | €17.77     | 2   | 35.54       | 426.48        |
| 2     | DB Server (dedicated CPU)           | CCX23 — 4 vCPU dedicated, 16 GB RAM | €22.75     | 1   | 22.75       | 273.00        |
| 3     | Redis Server                        | CX32 — 4 vCPU, 8 GB RAM             | €8.21      | 1   | 8.21        | 98.52         |
| 4     | Load Balancer                       | LB21 — up to 25 targets             | €10.79     | 1   | 10.79       | 129.48        |
| 5     | Volume — DB data                    | 250 GB                              | €13.00     | 1   | 13.00       | 156.00        |
| 6     | Volume — shared uploads             | 200 GB                              | €10.40     | 1   | 10.40       | 124.80        |
| 7     | Private Network (VLAN)              | App ↔ DB ↔ Redis                    | Free       | 1   | 0.00        | 0.00          |
| 8     | Storage Box (Backups)               | BX31 — 1 TB                         | €8.69      | 1   | 8.69        | 104.28        |
| 9     | Object Storage (media)              | S3 — 100 GB avg                     | €1.20      | 1   | 1.20        | 14.40         |
| 10    | Floating IPs                        | Static IPv4 (LB + admin)            | €2.00      | 2   | 4.00        | 48.00         |
| 11    | Monitoring node                     | CX22 — Grafana/Prometheus/Loki      | €4.35      | 1   | 4.35        | 52.20         |
| 12    | Cloudflare Pro (CDN + WAF)          | Per zone                            | €20.00     | 1   | 20.00       | 240.00        |
| 13    | SSL                                 | Let's Encrypt wildcard              | Free       | 1   | 0.00        | 0.00          |
| 14    | Domain (amortised)                  | Annual                              | €15.00/yr  | 1   | 1.25        | 15.00         |
| 15    | Transactional email                 | Brevo Business — 100k/mo            | €49.00     | 1   | 49.00       | 588.00        |
| 16    | External uptime monitoring          | UptimeRobot Pro                     | €7.00      | 1   | 7.00        | 84.00         |
| 17    | Secrets management                  | Doppler Team — 5 seats              | €18.00     | 1   | 18.00       | 216.00        |
| 18    | VM Snapshots (rotation ×4)          | ~80 GB avg                          | €0.95      | 1   | 0.95        | 11.40         |
| **—** | **SUBTOTAL**                        |                                     |            |     | **€215.13** | **€2,581.56** |

### Environment: Staging

| #     | Resource     | Spec                       | Monthly (€) | Annual (€)  |
| ----- | ------------ | -------------------------- | ----------- | ----------- |
| 1     | App+DB host  | CX42                       | 17.77       | 213.24      |
| 2     | Redis        | CX22                       | 4.35        | 52.20       |
| 3     | Volume       | 100 GB                     | 5.20        | 62.40       |
| 4     | Floating IP  | Static IPv4                | 2.00        | 24.00       |
| 5     | Brevo (test) | Brevo Free                 | 0.00        | 0.00        |
| **—** | **SUBTOTAL** |                            | **€29.32**  | **€351.84** |

### Environment: Development (Shared)

| #     | Resource                | Spec                  | Monthly (€) | Annual (€) |
| ----- | ----------------------- | --------------------- | ----------- | ---------- |
| 1     | Local Docker Compose    | Developer workstation | 0.00        | 0.00       |
| 2     | Shared dev VPS (review) | CX32                  | 8.21        | 98.52      |
| **—** | **SUBTOTAL**            |                       | **€8.21**   | **€98.52** |

### Tier 1 — Total Summary

| Environment      | Monthly MIN (€) | Monthly MAX (€) | Annual MIN (€) | Annual MAX (€) |
| ---------------- | --------------- | --------------- | -------------- | -------------- |
| Production       | 215.13          | 247.40          | 2,581.56       | 2,968.79       |
| Staging          | 11.73 _(paused)_ | 29.32          | 140.76         | 351.84         |
| Development      | 0.00            | 8.21            | 0.00           | 98.52          |
| **TOTAL TIER 1** | **€226.86**     | **€284.93**     | **€2,722.32**  | **€3,419.15**  |

---

## Tier 2 — Growth Setup (Replica + autoscale-ready)

> Read replica added, dedicated app autoscaling pool, hardened observability.
> **Traffic assumption:** 150k–500k requests/day, 3,000–6,000 concurrent users, ~150 req/s sustained.

### Environment: Production (Growth)

| #     | Resource                            | Spec                                | Unit Price | Qty | Monthly (€) | Annual (€)    |
| ----- | ----------------------------------- | ----------------------------------- | ---------- | --- | ----------- | ------------- |
| 1     | App Servers (behind LB)             | CX42 — 8 vCPU, 16 GB RAM            | €17.77     | 3   | 53.31       | 639.72        |
| 2     | DB Primary (dedicated CPU)          | CCX33 — 8 vCPU dedicated, 32 GB RAM | €45.50     | 1   | 45.50       | 546.00        |
| 3     | DB Read Replica                     | CCX23 — 4 vCPU dedicated, 16 GB RAM | €22.75     | 1   | 22.75       | 273.00        |
| 4     | Redis (primary + replica)           | CX32                                | €8.21      | 2   | 16.42       | 197.04        |
| 5     | Load Balancer                       | LB31 — up to 75 targets             | €18.04     | 1   | 18.04       | 216.48        |
| 6     | Volume — DB data (primary)          | 500 GB                              | €26.00     | 1   | 26.00       | 312.00        |
| 7     | Volume — DB data (replica)          | 500 GB                              | €26.00     | 1   | 26.00       | 312.00        |
| 8     | Volume — shared uploads             | 500 GB                              | €26.00     | 1   | 26.00       | 312.00        |
| 9     | Object Storage (media + exports)    | S3 — 500 GB avg                     | €6.00      | 1   | 6.00        | 72.00         |
| 10    | Storage Box (Backups)               | BX41 — 5 TB                         | €15.21     | 1   | 15.21       | 182.52        |
| 11    | Floating IPs                        | Static IPv4                         | €2.00      | 2   | 4.00        | 48.00         |
| 12    | Monitoring node                     | CX32 — Grafana stack                | €8.21      | 1   | 8.21        | 98.52         |
| 13    | Log retention (Loki) volume         | 200 GB                              | €10.40     | 1   | 10.40       | 124.80        |
| 14    | Self-hosted CI runner               | CX42                                | €17.77     | 1   | 17.77       | 213.24        |
| 15    | Cloudflare Business                 | Per zone                            | €185.00    | 1   | 185.00      | 2,220.00      |
| 16    | Transactional email                 | Brevo Business — 100k/mo            | €49.00     | 1   | 49.00       | 588.00        |
| 17    | External uptime + APM               | Better Stack / Checkly              | €25.00     | 1   | 25.00       | 300.00        |
| 18    | Secrets management                  | Doppler Team — 10 seats             | €36.00     | 1   | 36.00       | 432.00        |
| 19    | Domain (amortised, 2 zones)         | Annual                              | €30.00/yr  | 1   | 2.50        | 30.00         |
| 20    | VM Snapshots                        | ~150 GB avg                         | €1.80      | 1   | 1.80        | 21.60         |
| **—** | **SUBTOTAL**                        |                                     |            |     | **€594.91** | **€7,138.92** |

### Environment: Staging (close-to-prod)

| #     | Resource     | Spec                | Monthly (€) | Annual (€)  |
| ----- | ------------ | ------------------- | ----------- | ----------- |
| 1     | App tier     | CX42 ×1             | 17.77       | 213.24      |
| 2     | DB           | CX42                | 17.77       | 213.24      |
| 3     | Redis        | CX22                | 4.35        | 52.20       |
| 4     | Volume       | 200 GB              | 10.40       | 124.80      |
| 5     | Floating IP  | Static IPv4         | 2.00        | 24.00       |
| **—** | **SUBTOTAL** |                     | **€52.29**  | **€627.48** |

### Tier 2 — Total Summary

| Environment      | Monthly MIN (€) | Monthly MAX (€) | Annual MIN (€) | Annual MAX (€) |
| ---------------- | --------------- | --------------- | -------------- | -------------- |
| Production       | 594.91          | 684.15          | 7,138.92       | 8,209.76       |
| Staging          | 20.92 _(paused)_ | 52.29          | 251.04         | 627.48         |
| Development      | 0.00            | 8.21            | 0.00           | 98.52          |
| **TOTAL TIER 2** | **€615.83**     | **€744.65**     | **€7,389.96**  | **€8,935.76**  |

---

## Tier 3 — Scale Setup (HA, multi-AZ-style)

> Active-passive DB failover, multiple Redis nodes, dedicated runners, full observability,
> blue/green deploys, hardened DR.
> **Traffic assumption:** 500k–2M requests/day, 10k+ concurrent users, ~500 req/s sustained.

### Environment: Production (Scale)

| #     | Resource                            | Spec                                | Unit Price | Qty | Monthly (€)   | Annual (€)     |
| ----- | ----------------------------------- | ----------------------------------- | ---------- | --- | ------------- | -------------- |
| 1     | App Servers (autoscaling pool)      | CX52 — 16 vCPU, 32 GB RAM           | €35.34     | 4   | 141.36        | 1,696.32       |
| 2     | DB Primary (dedicated CPU)          | CCX43 — 16 vCPU dedicated, 64 GB    | €91.00     | 1   | 91.00         | 1,092.00       |
| 3     | DB Standby + Replica                | CCX33 — 8 vCPU dedicated, 32 GB     | €45.50     | 2   | 91.00         | 1,092.00       |
| 4     | Redis Cluster (3 nodes)             | CX32                                | €8.21      | 3   | 24.63         | 295.56         |
| 5     | Load Balancer                       | LB31 — up to 75 targets             | €18.04     | 2   | 36.08         | 432.96         |
| 6     | Volume — DB data                    | 1 TB                                | €52.00     | 3   | 156.00        | 1,872.00       |
| 7     | Volume — shared uploads             | 1 TB                                | €52.00     | 1   | 52.00         | 624.00         |
| 8     | Object Storage                      | S3 — 2 TB avg                       | €24.00     | 1   | 24.00         | 288.00         |
| 9     | Storage Box (Backups)               | BX41 — 5 TB                         | €15.21     | 2   | 30.42         | 365.04         |
| 10    | Floating IPs                        | Static IPv4                         | €2.00      | 3   | 6.00          | 72.00          |
| 11    | Monitoring nodes (HA)               | CX32                                | €8.21      | 2   | 16.42         | 197.04         |
| 12    | Log retention (Loki) volume         | 500 GB                              | €26.00     | 1   | 26.00         | 312.00         |
| 13    | Self-hosted CI runners              | CX42                                | €17.77     | 2   | 35.54         | 426.48         |
| 14    | Cloudflare Business + Argo          | Per zone + smart routing            | €235.00    | 1   | 235.00        | 2,820.00       |
| 15    | Transactional email                 | Brevo Enterprise — 1M/mo            | €179.00    | 1   | 179.00        | 2,148.00       |
| 16    | External monitoring + APM           | Better Stack + Sentry Team          | €75.00     | 1   | 75.00         | 900.00         |
| 17    | Secrets management                  | Doppler Pro — 15 seats              | €120.00    | 1   | 120.00        | 1,440.00       |
| 18    | Domain (multi-zone, amortised)      | Annual                              | €60.00/yr  | 1   | 5.00          | 60.00          |
| 19    | VM Snapshots                        | ~300 GB avg                         | €3.60      | 1   | 3.60          | 43.20          |
| **—** | **SUBTOTAL**                        |                                     |            |     | **€1,348.05** | **€16,176.60** |

### Tier 3 — Total Summary

| Environment      | Monthly MIN (€) | Monthly MAX (€) | Annual MIN (€)  | Annual MAX (€)  |
| ---------------- | --------------- | --------------- | --------------- | --------------- |
| Production       | 1,348.05        | 1,550.26        | 16,176.60       | 18,603.09       |
| Staging          | 30.00 _(paused)_ | 80.00          | 360.00          | 960.00          |
| Development      | 0.00            | 8.21            | 0.00            | 98.52           |
| **TOTAL TIER 3** | **€1,378.05**   | **€1,638.47**   | **€16,536.60**  | **€19,661.61**  |

---

## Tier 4 — Enterprise / Regional Scale

> Multi-region active/active or large monolithic dedicated cluster, dedicated database servers,
> contractual SLAs, dedicated SRE tooling.
> **Traffic assumption:** 2M+ requests/day, 25k+ concurrent users, > 1,000 req/s sustained.

### Environment: Production (Enterprise)

| #     | Resource                                 | Spec                                       | Unit Price | Qty | Monthly (€)   | Annual (€)     |
| ----- | ---------------------------------------- | ------------------------------------------ | ---------- | --- | ------------- | -------------- |
| 1     | Dedicated server (Robot AX102)           | Ryzen 9, 128 GB, 2× NVMe — app pool host   | €119.00    | 3   | 357.00        | 4,284.00       |
| 2     | Dedicated server (Robot EX144)           | DB host — Xeon, 256 GB RAM, NVMe RAID      | €235.00    | 2   | 470.00        | 5,640.00       |
| 3     | Redis Cluster (HA, 6 nodes)              | CX42                                       | €17.77     | 6   | 106.62        | 1,279.44       |
| 4     | Load Balancers (multi-zone)              | LB31                                       | €18.04     | 4   | 72.16         | 865.92         |
| 5     | Object Storage                           | S3 — 10 TB avg                             | €120.00    | 1   | 120.00        | 1,440.00       |
| 6     | Storage Box (DR backups)                 | BX51 — 10 TB                               | €30.43     | 2   | 60.86         | 730.32         |
| 7     | Floating IPs                             | Static IPv4                                | €2.00      | 6   | 12.00         | 144.00         |
| 8     | Dedicated monitoring + APM stack         | CX42 ×3 + storage                          | €17.77     | 3   | 53.31         | 639.72         |
| 9     | Self-hosted CI farm                      | CX52                                       | €35.34     | 3   | 106.02        | 1,272.24       |
| 10    | Cloudflare Enterprise (negotiated)       | Per zone — contract                        | €1,500.00  | 1   | 1,500.00      | 18,000.00      |
| 11    | Transactional email                      | SendGrid / Postmark — 5M/mo                | €450.00    | 1   | 450.00        | 5,400.00       |
| 12    | Observability SaaS (Datadog/New Relic)   | Per host                                   | €600.00    | 1   | 600.00        | 7,200.00       |
| 13    | Secrets / IAM (Vault Cloud)              | Tier: Plus                                 | €350.00    | 1   | 350.00        | 4,200.00       |
| 14    | Domain (multi-region, amortised)         | Annual                                     | €150.00/yr | 1   | 12.50         | 150.00         |
| 15    | On-call paging (PagerDuty)               | 10 seats                                   | €210.00    | 1   | 210.00        | 2,520.00       |
| **—** | **SUBTOTAL**                             |                                            |            |     | **€4,480.47** | **€53,765.64** |

### Tier 4 — Total Summary

| Environment      | Monthly MIN (€) | Monthly MAX (€) | Annual MIN (€)  | Annual MAX (€)  |
| ---------------- | --------------- | --------------- | --------------- | --------------- |
| Production       | 4,480.47        | 5,152.54        | 53,765.64       | 61,830.49       |
| Staging (large)  | 150.00          | 250.00          | 1,800.00        | 3,000.00        |
| Development      | 8.21            | 16.42           | 98.52           | 197.04          |
| **TOTAL TIER 4** | **€4,638.68**   | **€5,418.96**   | **€55,664.16**  | **€65,027.53**  |

---

## High-Traffic — Tier Comparison Dashboard

| Tier   | Setup Type                       | Monthly MIN | Monthly MAX | Annual MIN | Annual MAX | Best for                          |
| ------ | -------------------------------- | ----------- | ----------- | ---------- | ---------- | --------------------------------- |
| **T1** | Launch — single region split     | €226.86     | €284.93     | €2,722     | €3,419     | Public product launch             |
| **T2** | Growth — read replica + autoscale | €615.83    | €744.65     | €7,390     | €8,936     | Active product, 100k+ users       |
| **T3** | Scale — HA + DR                  | €1,378.05   | €1,638.47   | €16,537    | €19,662    | Mission-critical SaaS             |
| **T4** | Enterprise — dedicated + multi-LB | €4,638.68 | €5,418.96   | €55,664    | €65,028    | Regional platform, SLA-backed     |

---

## Hidden & Variable Costs

| Cost Item                             | Frequency | Estimated Amount      | Notes                                             |
| ------------------------------------- | --------- | --------------------- | ------------------------------------------------- |
| Cloudflare egress / Workers           | Monthly   | €0–€200               | Depends on traffic shape                          |
| Egress to internet (Hetzner)          | Monthly   | €0–€50                | Includes 20 TB free per cloud server              |
| DB IOPS / volume burst                | Monthly   | +€10–€80              | If volume sized too small                         |
| Penetration test (annual)             | Annual    | €1,500–€6,000         | Required by client SLA                            |
| Compliance audit (SOC2 / ISO)         | Annual    | €5,000–€25,000        | Only if mandated                                  |
| 3rd-party APIs (Stripe, Twilio, etc.) | Monthly   | Variable              | Charged to client                                 |
| Sentry / error tracking overage       | Monthly   | +€26–€80              | Past free quota                                   |
| Backup restoration test (quarterly)   | Quarterly | €0 (engineering time) | Recommended for DR validation                     |
| Domain SSL EV / commercial cert       | Annual    | €100–€500             | Only if mandated                                  |
| 24/7 on-call premium                  | Monthly   | +€500–€2,500          | If client SLA requires 24/7 response              |

---

## Budget Planning Summary (High-Traffic Client Project)

```
LIONGATE SARL — High-Traffic Client Project Budget Reference
============================================================

LAUNCH MONTHLY BUDGET:        €226.86   (Tier 1 — single region)
GROWTH MONTHLY BUDGET:        €615.83   (Tier 2 — replica + autoscale)
SCALE MONTHLY BUDGET:         €1,378.05 (Tier 3 — HA + DR)
ENTERPRISE MONTHLY BUDGET:    €4,638.68 (Tier 4 — dedicated/regional)

LAUNCH ANNUAL BUDGET:         €2,722.32
GROWTH ANNUAL BUDGET:         €7,389.96
SCALE ANNUAL BUDGET:          €16,536.60
ENTERPRISE ANNUAL BUDGET:     €55,664.16

CONTINGENCY BUFFER (+20% for high-traffic uncertainty):
  Tier 2 growth:    €7,389.96  × 1.20 = €8,867.95/yr
  Tier 3 scale:     €16,536.60 × 1.20 = €19,843.92/yr
  Tier 4 enterprise:€55,664.16 × 1.20 = €66,797.00/yr
```

---

## Decision Guide — Which tier to quote a client?

| Question                                          | If YES → recommend |
| ------------------------------------------------- | ------------------ |
| Public launch, < 2k concurrent users expected     | Tier 1             |
| Active SaaS, recurring growth, read-heavy         | Tier 2             |
| 24/7 critical, contractual uptime ≥ 99.9%         | Tier 3             |
| Multi-region, regulated industry, SLA-backed      | Tier 4             |
| Client requires dedicated CPU + dedicated backups | Tier 2 minimum     |
| Client requires WAF + DDoS guarantees             | Tier 1 minimum     |

---

_Document: PROFORMA-03 | Context: High-Traffic Client Project_
_Prices: Hetzner Cloud + Hetzner Robot published rates + Cloudflare/Brevo/Doppler list prices_
_Review: Quarterly, or per client contract signing_
_Prepared by: LIONGATE SARL Engineering Team_
