# PROFORMA — Internal Enterprise Tools Infrastructure Billing

**LIONGATE SARL — Infrastructure Cost Estimation**
**Document:** PROFORMA-04
**Context:** Internal Tools (HR, CRM-lite, dashboards, ops portals, employee apps)
**Hosting Provider:** Hetzner Cloud
**Currency:** EUR (€)
**Version:** 1.0

---

> **How to read this document:**
> This proforma covers infrastructure to host **internal tools used by LIONGATE staff only** —
> typically behind VPN/SSO, low concurrency (5–80 users), but business-critical.
> Three tiers are presented because high-availability is rarely required for internal tools.

---

## Reference Stack

| Layer        | Technology                                 | Notes                              |
| ------------ | ------------------------------------------ | ---------------------------------- |
| Frontend     | Next.js / Refine / React Admin             | Admin-style UIs                    |
| Backend      | NestJS / Django / FastAPI                  | REST or RPC                        |
| Database     | PostgreSQL 16                              | Shared cluster for multiple tools  |
| Auth         | Authentik / Keycloak / Cloudflare Access   | SSO for all internal apps          |
| VPN          | WireGuard / Tailscale (free for ≤ 3 users) | Private access                     |
| Reverse proxy| Nginx / Traefik                            | Routes per-subdomain               |
| Container    | Docker Compose                             | One node hosts multiple tools      |
| Storage      | Hetzner Volume + Storage Box (backups)     | Shared                             |
| Monitoring   | Uptime Kuma (self-hosted)                  | Free, lightweight                  |

> 💡 Internal tools are normally **co-located on a shared internal-apps server** to minimise cost.
> Each tool runs as its own Docker stack with its own database schema.

---

## Tier 1 — Minimum Viable Internal Stack (Single shared host)

> One internal-apps server hosting up to ~5 small tools + shared Postgres + shared auth.
> **Audience:** ≤ 15 employees. **Concurrency:** ≤ 10 simultaneous users.

### Production (single shared host)

| #     | Resource                              | Spec                                | Unit Price | Qty | Monthly (€) | Annual (€)  |
| ----- | ------------------------------------- | ----------------------------------- | ---------- | --- | ----------- | ----------- |
| 1     | Internal Apps Server                  | CX32 — 4 vCPU, 8 GB RAM, 80 GB NVMe | €8.21      | 1   | 8.21        | 98.52       |
| 2     | Hetzner Volume (shared data)          | 100 GB                              | €5.20      | 1   | 5.20        | 62.40       |
| 3     | Storage Box (backups)                 | BX11 — 100 GB                       | €3.43      | 1   | 3.43        | 41.16       |
| 4     | Floating IP                           | Static IPv4                         | €2.00      | 1   | 2.00        | 24.00       |
| 5     | SSL                                   | Let's Encrypt wildcard              | Free       | 1   | 0.00        | 0.00        |
| 6     | Internal subdomain (`*.tools.liongate.app`) | Same domain zone              | Included   | 1   | 0.00        | 0.00        |
| 7     | SSO / Auth (Authentik self-hosted)    | Runs on same server                 | Free       | 1   | 0.00        | 0.00        |
| 8     | VPN (Tailscale Free ≤ 3 users)        | Or WireGuard self-hosted            | Free       | 1   | 0.00        | 0.00        |
| 9     | Outbound email                        | Brevo Free — 300/day                | Free       | 1   | 0.00        | 0.00        |
| 10    | Uptime Kuma (self-hosted)             | Same server                         | Free       | 1   | 0.00        | 0.00        |
| **—** | **SUBTOTAL**                          |                                     |            |     | **€18.84**  | **€226.08** |

### Tier 1 — Total Summary

| Environment      | Monthly (€) | Annual (€)  |
| ---------------- | ----------- | ----------- |
| Production       | 18.84       | 226.08      |
| Staging          | _(not used — preview locally or on dev branch deploys)_ | — |
| Development      | 0.00 (local Docker) | 0.00 |
| **TOTAL TIER 1** | **€18.84**  | **€226.08** |

---

## Tier 2 — Recommended Internal Stack (Split DB + SSO)

> App server hosts internal tools; PostgreSQL on its own server; Authentik in its own container set.
> **Audience:** 15–60 employees. **Concurrency:** 10–30 simultaneous users.

### Production (Recommended)

| #     | Resource                                 | Spec                                | Unit Price | Qty | Monthly (€) | Annual (€)  |
| ----- | ---------------------------------------- | ----------------------------------- | ---------- | --- | ----------- | ----------- |
| 1     | Internal Apps Server                     | CX42 — 8 vCPU, 16 GB RAM            | €17.77     | 1   | 17.77       | 213.24      |
| 2     | DB Server (shared across tools)          | CX32 — 4 vCPU, 8 GB RAM             | €8.21      | 1   | 8.21        | 98.52       |
| 3     | Volume — DB data                         | 100 GB                              | €5.20      | 1   | 5.20        | 62.40       |
| 4     | Volume — Apps data / file uploads        | 100 GB                              | €5.20      | 1   | 5.20        | 62.40       |
| 5     | Private Network (VLAN)                   | App ↔ DB                            | Free       | 1   | 0.00        | 0.00        |
| 6     | Storage Box (backups)                    | BX21 — 500 GB                       | €5.94      | 1   | 5.94        | 71.28       |
| 7     | Floating IP                              | Static IPv4                         | €2.00      | 1   | 2.00        | 24.00       |
| 8     | SSL                                      | Let's Encrypt wildcard              | Free       | 1   | 0.00        | 0.00        |
| 9     | SSO (Authentik self-hosted on app server)| Free                                | Free       | 1   | 0.00        | 0.00        |
| 10    | Outbound email                           | Brevo Starter — 20k/mo              | €19.00     | 1   | 19.00       | 228.00      |
| 11    | VPN (Tailscale Premium, 10 users)        | Per user                            | €6.00      | 10  | 60.00       | 720.00      |
| 12    | Snapshots (weekly ×2)                    | ~40 GB                              | €0.48      | 1   | 0.48        | 5.76        |
| **—** | **SUBTOTAL**                             |                                     |            |     | **€123.80** | **€1,485.60** |

> 💡 If using Tailscale Free (max 3 users) or self-hosted WireGuard, **save €60/mo**.
> Recommended saving: self-host WireGuard → reduces Tier 2 to **€63.80/mo (€765.60/yr)**.

### Tier 2 — Total Summary

| Environment      | Monthly MIN (€)               | Monthly MAX (€) | Annual MIN (€) | Annual MAX (€) |
| ---------------- | ----------------------------- | --------------- | -------------- | -------------- |
| Production       | 63.80 _(self-hosted VPN)_     | 123.80          | 765.60         | 1,485.60       |
| Staging          | _(branch deploys on app srv)_ | —               | —              | —              |
| Development      | 0.00 (local Docker)           | 0.00            | 0.00           | 0.00           |
| **TOTAL TIER 2** | **€63.80**                    | **€123.80**     | **€765.60**    | **€1,485.60**  |

---

## Tier 3 — Full Internal Stack (Production-grade)

> Dedicated SSO server, dedicated monitoring, dedicated backup orchestration, dedicated CI runner.
> **Audience:** 60+ employees, multiple departments, business-critical tools (HR, finance).

### Production (Full)

| #     | Resource                                | Spec                                | Unit Price | Qty | Monthly (€) | Annual (€)    |
| ----- | --------------------------------------- | ----------------------------------- | ---------- | --- | ----------- | ------------- |
| 1     | Internal Apps Server                    | CX42 — 8 vCPU, 16 GB RAM            | €17.77     | 1   | 17.77       | 213.24        |
| 2     | DB Server                               | CX42 — 8 vCPU, 16 GB RAM            | €17.77     | 1   | 17.77       | 213.24        |
| 3     | SSO Server (Authentik / Keycloak)       | CX22                                | €4.35      | 1   | 4.35        | 52.20         |
| 4     | Monitoring Server (Grafana + Uptime)    | CX22                                | €4.35      | 1   | 4.35        | 52.20         |
| 5     | Self-hosted CI runner                   | CX32                                | €8.21      | 1   | 8.21        | 98.52         |
| 6     | Volume — DB data                        | 200 GB                              | €10.40     | 1   | 10.40       | 124.80        |
| 7     | Volume — Apps shared data               | 200 GB                              | €10.40     | 1   | 10.40       | 124.80        |
| 8     | Storage Box (backups)                   | BX31 — 1 TB                         | €8.69      | 1   | 8.69        | 104.28        |
| 9     | Floating IP × 2                         | Static IPv4 (apps + SSO)            | €2.00      | 2   | 4.00        | 48.00         |
| 10    | Private Network                         | VLAN                                | Free       | 1   | 0.00        | 0.00          |
| 11    | SSL                                     | Let's Encrypt wildcard              | Free       | 1   | 0.00        | 0.00          |
| 12    | Outbound email                          | Brevo Business — 100k/mo            | €49.00     | 1   | 49.00       | 588.00        |
| 13    | Tailscale Business (per user)           | 25 users                            | €18.00     | 25  | 450.00      | 5,400.00      |
| 14    | External uptime monitoring              | UptimeRobot Pro                     | €7.00      | 1   | 7.00        | 84.00         |
| 15    | Snapshots (rotation ×4)                 | ~80 GB                              | €0.95      | 1   | 0.95        | 11.40         |
| **—** | **SUBTOTAL**                            |                                     |            |     | **€596.89** | **€7,114.68** |

> 💡 **Tailscale is the biggest single line item.** Alternative recommended for cost-sensitive companies:
> - Self-host WireGuard or Headscale → near-zero ongoing cost
> - Use Cloudflare Access (Zero Trust Free up to 50 users) → €0 instead of €450/mo
>
> Replacing Tailscale Business with **Cloudflare Access Free** reduces Tier 3 to **€146.89/mo (€1,762.68/yr)**.

### Tier 3 — Total Summary

| Environment      | Monthly MIN (€)             | Monthly MAX (€) | Annual MIN (€) | Annual MAX (€) |
| ---------------- | --------------------------- | --------------- | -------------- | -------------- |
| Production       | 146.89 _(CF Access Free)_   | 596.89          | 1,762.68       | 7,114.68       |
| Staging          | _(branch deploys)_          | 8.21            | —              | 98.52          |
| Development      | 0.00 (local Docker)         | 0.00            | 0.00           | 0.00           |
| **TOTAL TIER 3** | **€146.89**                 | **€605.10**     | **€1,762.68**  | **€7,213.20**  |

---

## Internal Tools — Tier Comparison Dashboard

| Tier   | Setup Type                              | Monthly MIN | Monthly MAX | Annual MIN | Annual MAX | Best for                          |
| ------ | --------------------------------------- | ----------- | ----------- | ---------- | ---------- | --------------------------------- |
| **T1** | Single shared host                      | €18.84      | €18.84      | €226       | €226       | ≤ 15 employees, < 5 tools         |
| **T2** | Split DB + SSO + VPN                    | €63.80      | €123.80     | €766       | €1,486     | 15–60 employees, business tools   |
| **T3** | Full stack with SSO, CI, monitoring     | €146.89     | €605.10     | €1,763     | €7,213     | 60+ employees, critical workflows |

---

## Tool Co-location Strategy (Cost Optimisation)

Internal tools should **share infrastructure** wherever possible. Recommended co-location pattern:

| Server             | Hosts                                                            |
| ------------------ | ---------------------------------------------------------------- |
| internal-apps-01   | All internal Next.js/NestJS apps via Docker Compose + Traefik    |
| internal-db-01     | Single Postgres cluster with one schema per tool                 |
| sso-01             | Authentik / Keycloak (Tier 3 only)                               |
| ops-01             | Grafana, Loki, Prometheus, Uptime Kuma                           |
| ci-01              | Self-hosted GitHub Actions runner                                |

> 💡 Adding one more internal tool to T2 costs **€0 of extra infrastructure** if app server has spare RAM.
> Only DB size and backup volume grow. **Real marginal cost: ~€2–€5/mo per added tool.**

---

## Hidden & Variable Costs

| Cost Item                          | Frequency | Estimated Amount    | Notes                                       |
| ---------------------------------- | --------- | ------------------- | ------------------------------------------- |
| Marginal new internal tool         | Monthly   | +€2–€5              | If co-located on shared host                |
| VPN seat growth                    | Monthly   | +€6–€18 per user    | Tailscale paid tiers                        |
| DB volume growth                   | Monthly   | +€0.52 per +10 GB   | Hetzner volume                              |
| SSO licence (Authentik Enterprise) | Annual    | €0 (community) / €5,000+ (enterprise) | Community is sufficient for ≤ 100 users |
| Storage Box upgrade                | Monthly   | +€2–€6              | When backups grow past 1 TB                 |
| Audit log retention                | Monthly   | +€2–€10             | Loki/MinIO storage                          |

---

## Budget Planning Summary (Internal Tools)

```
LIONGATE SARL — Internal Tools Budget Reference
================================================

MINIMUM MONTHLY BUDGET:       €18.84    (Tier 1 — single shared host)
RECOMMENDED MONTHLY BUDGET:   €63.80    (Tier 2 — self-hosted VPN)
FULL PRODUCTION BUDGET:       €146.89   (Tier 3 — Cloudflare Access Free)
PREMIUM BUDGET (Tailscale):   €596.89   (Tier 3 — full Tailscale Business)

MINIMUM ANNUAL BUDGET:        €226.08
RECOMMENDED ANNUAL BUDGET:    €765.60
FULL PRODUCTION ANNUAL:       €1,762.68
PREMIUM ANNUAL:               €7,114.68

CONTINGENCY BUFFER (+10%, low volatility):
  Tier 2 recommended: €765.60   × 1.10 = €842.16/yr
  Tier 3 full:        €1,762.68 × 1.10 = €1,938.95/yr
```

---

## Decision Guide — Which tier for internal tools?

| Question                                          | If YES → recommend           |
| ------------------------------------------------- | ---------------------------- |
| Fewer than 15 staff, 1–4 small internal tools     | Tier 1                       |
| HR/Finance/CRM-lite tools used daily by ≥ 15 staff | Tier 2                       |
| Multi-department, role-based SSO required         | Tier 3                       |
| Need GitHub Actions self-hosted runner            | Tier 3                       |
| Compliance/audit logging required                 | Tier 3 + Loki retention      |
| Want zero ongoing VPN cost                        | Use Cloudflare Access Free   |

---

_Document: PROFORMA-04 | Context: Internal Enterprise Tools_
_Prices: Hetzner Cloud + Tailscale/Brevo list prices_
_Review: Annually or when staff count crosses tier threshold_
_Prepared by: LIONGATE SARL Engineering Team_
