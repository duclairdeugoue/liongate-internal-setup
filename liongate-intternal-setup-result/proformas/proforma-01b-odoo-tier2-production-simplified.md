# PROFORMA — Odoo Instance (Tier 2 — Production, Simplified)

**LIONGATE SARL — Infrastructure Cost Estimation**
**Document:** PROFORMA-01B
**Context:** Odoo ERP Deployment — Tier 2 Production only
**Scope adjustments:** No Storage Box, no Floating IP, no Domain, no Outbound email
**Hosting Provider:** Hetzner Cloud
**Currency:** EUR (€)
**Version:** 1.0

---

## Tier 2 — Production (Simplified)

| #     | Resource                         | Spec                            | Unit Price | Qty | Monthly (€) | Annual (€)  |
| ----- | -------------------------------- | ------------------------------- | ---------- | --- | ----------- | ----------- |
| 1     | App Server                       | CX42 — 8 vCPU, 16 GB RAM        | €17.77     | 1   | 17.77       | 213.24      |
| 2     | DB Server                        | CX32 — 4 vCPU, 8 GB RAM         | €8.21      | 1   | 8.21        | 98.52       |
| 3     | Hetzner Volume (PostgreSQL data) | 100 GB — attached to DB server  | €5.20      | 1   | 5.20        | 62.40       |
| 4     | Hetzner Volume (Odoo filestore)  | 50 GB — attached to App server  | €2.60      | 1   | 2.60        | 31.20       |
| 5     | Private Network (VLAN)           | App ↔ DB isolated communication | Free       | 1   | 0.00        | 0.00        |
| 6     | SSL Certificate                  | Let's Encrypt                   | Free       | 1   | 0.00        | 0.00        |
| 7     | External uptime monitoring       | UptimeRobot Free                | Free       | 1   | 0.00        | 0.00        |
| **—** | **TOTAL**                        |                                 |            |     | **€33.78**  | **€405.36** |

---

_Document: PROFORMA-01B | Context: Odoo Tier 2 — Production (Simplified)_
_Prices: Hetzner Cloud published rates_
_Prepared by: LIONGATE SARL Engineering Team_
