# 01 - Hosting Platform, Billing & Infrastructure Strategy

**LIONGATE SARL - Internal Technical Handbook**
_Version 1.0 - Initial release_

---

## Table of Contents

1. [Overview](#overview)
2. [Hosting Provider: Hetzner](#hosting-provider-hetzner)
3. [Domain Provider: United Domains](#domain-provider-united-domains)
4. [Infrastructure Service Catalog](#infrastructure-service-catalog)
5. [Environment Tiers](#environment-tiers)
6. [Project Type Profiles](#project-type-profiles)
7. [Pricing Model & Cost Estimates](#pricing-model--cost-estimates)
8. [Infrastructure Selection Rules](#infrastructure-selection-rules)
9. [Network & Security Architecture](#network--security-architecture)
10. [Backup & Disaster Recovery Strategy](#backup--disaster-recovery-strategy)
11. [Monitoring & Observability](#monitoring--observability)
12. [Scaling Strategy](#scaling-strategy)
13. [Domain Management with United Domains](#domain-management-with-united-domains)
14. [Infrastructure-as-Code Standards](#infrastructure-as-code-standards)
15. [Cost Control Policies](#cost-control-policies)

---

## Overview

LIONGATE SARL is a startup engineering company that builds internal products and client projects from requirements to production. The infrastructure strategy documented here governs all hosting decisions, billing expectations, environment structures, and domain management practices.

### Guiding Principles

| Principle                 | Description                                                               |
| ------------------------- | ------------------------------------------------------------------------- |
| **Cost-awareness**        | Every project must have an estimated monthly cost before provisioning     |
| **Environment isolation** | Dev, staging, and production are always separate                          |
| **Right-sizing**          | Resources are sized for actual workload, not theoretical maximum          |
| **Reproducibility**       | Infrastructure is defined as code and can be recreated from scratch       |
| **Security-first**        | No server is exposed unnecessarily; firewall rules are always restrictive |
| **Backup-mandatory**      | Every production environment has automated, tested backups                |

---

## Hosting Provider: Hetzner

### Why Hetzner

Hetzner is a German hosting provider offering excellent price-to-performance for European and international deployments. It is LIONGATE SARL's primary infrastructure provider.

| Advantage            | Detail                                                  |
| -------------------- | ------------------------------------------------------- |
| **Price**            | Among the lowest EUR-priced VPS/bare metal in Europe    |
| **GDPR compliance**  | German/Finnish data centers, EU data sovereignty        |
| **Bare metal**       | Dedicated servers available at competitive prices       |
| **Cloud API**        | Full REST API and Terraform provider available          |
| **Object storage**   | S3-compatible storage via Hetzner Storage Box or S3 API |
| **Private networks** | VLAN-level isolation between servers                    |
| **Snapshots**        | Per-server snapshot support for quick recovery          |

### Hetzner Product Lines

| Product                       | Use case                        | Price range   |
| ----------------------------- | ------------------------------- | ------------- |
| **Cloud VPS (CX/CPX series)** | Dev, staging, small production  | €3.29–€55/mo  |
| **Cloud VPS (CCX series)**    | CPU-intensive production        | €12–€130/mo   |
| **Bare Metal (AX/EX/SB)**     | High-performance production     | €35–€220/mo   |
| **Managed databases**         | PostgreSQL, MySQL (optional)    | €20–€120/mo   |
| **Load balancers**            | Multi-server HA setups          | €5–€40/mo     |
| **Storage Box**               | Backup / SFTP / NFS storage     | €3.43–€58/mo  |
| **Volumes**                   | Block storage attachable to VPS | €0.052/GB/mo  |
| **Snapshots**                 | VM snapshots for recovery       | €0.0119/GB/mo |
| **Floating IPs**              | Static IPs, reattachable        | €2/mo each    |
| **Object Storage (S3)**       | File/media/backup storage       | €0.012/GB/mo  |

---

## Domain Provider: United Domains

All LIONGATE SARL domains are registered and managed through **United Domains** (`united-domains.de` / `united-domains.com`).

### Domain Workflow

```
United Domains (Registrar)
        │
        ├── DNS Zone Management
        │     ├── A Records → Hetzner Floating IP or Server IP
        │     ├── AAAA Records → IPv6 (if used)
        │     ├── CNAME Records → Subdomains
        │     ├── MX Records → Mail provider
        │     ├── TXT Records → SPF, DKIM, domain verification
        │     └── NS Records → Optionally delegate to Hetzner DNS
        │
        └── Domain Renewal Tracking (annual, auto-renewal enabled)
```

### DNS Management Options

| Option                           | Where                           | When to use                        |
| -------------------------------- | ------------------------------- | ---------------------------------- |
| **Manage DNS in United Domains** | United Domains control panel    | Simple projects, single server     |
| **Delegate to Hetzner DNS**      | Point NS records to Hetzner DNS | Complex setups, IaC-managed DNS    |
| **Delegate to Cloudflare**       | Point NS records to Cloudflare  | CDN, DDoS protection, proxy needed |

### Standard DNS Record Types

| Record  | Example                       | Purpose                              |
| ------- | ----------------------------- | ------------------------------------ |
| `A`     | `app.client.com → 95.216.x.x` | Map subdomain to server IP           |
| `CNAME` | `www → app.client.com`        | Alias record                         |
| `TXT`   | `v=spf1 ...`                  | Email auth, domain verification      |
| `MX`    | `mail.client.com`             | Email routing                        |
| `CAA`   | `letsencrypt.org`             | Restrict SSL certificate authorities |

### Domain Naming Convention

```
# Production
app.clientname.com
api.clientname.com
admin.clientname.com

# Staging
staging.clientname.com
staging-api.clientname.com

# Internal tools
erp.liongate.com
git.liongate.com
monitor.liongate.com
```

---

## Infrastructure Service Catalog

### Compute (Cloud Servers / VPS)

#### Hetzner Cloud Server Tiers

| Tier      | vCPU | RAM   | Storage | Price/mo | Use case                  |
| --------- | ---- | ----- | ------- | -------- | ------------------------- |
| **CX22**  | 2    | 4 GB  | 40 GB   | €4.35    | Dev, lightweight staging  |
| **CX32**  | 4    | 8 GB  | 80 GB   | €8.21    | Staging, small production |
| **CX42**  | 8    | 16 GB | 160 GB  | €17.77   | Medium production         |
| **CX52**  | 16   | 32 GB | 320 GB  | €35.54   | Heavy production          |
| **CCX23** | 4    | 16 GB | 160 GB  | €22.75   | CPU-dedicated medium      |
| **CCX33** | 8    | 32 GB | 240 GB  | €45.18   | CPU-dedicated large       |
| **CCX43** | 16   | 64 GB | 360 GB  | €90.35   | CPU-dedicated XL          |

#### Bare Metal (for high-performance use)

| Server        | CPU              | RAM    | Storage         | Price/mo |
| ------------- | ---------------- | ------ | --------------- | -------- |
| **AX41-NVMe** | AMD Ryzen 5 3600 | 64 GB  | 2x NVMe 512 GB  | ~€37     |
| **AX52**      | AMD Ryzen 9 3900 | 128 GB | 2x NVMe 1.92 TB | ~€65     |
| **EX44**      | Intel i9-9900K   | 128 GB | 2x NVMe 1.92 TB | ~€70     |

> **Note:** Bare metal provisioning takes 24–48 hours. Plan ahead.

### Database Options

| Option                 | Engine        | Price/mo | Notes                      |
| ---------------------- | ------------- | -------- | -------------------------- |
| **Self-hosted on VPS** | PostgreSQL 16 | €0 extra | Included in server cost    |
| **Hetzner Managed DB** | PostgreSQL    | €20–€120 | HA option, managed patches |
| **Separate DB server** | PostgreSQL 16 | €8–€35   | Dedicated DB VPS           |

> **LIONGATE default:** Self-hosted PostgreSQL on a dedicated DB server per production environment. Hetzner Managed DB is an option for clients requiring SLA-backed managed services.

### Storage & Volumes

| Service                | Type            | Price        | Use                                |
| ---------------------- | --------------- | ------------ | ---------------------------------- |
| **Hetzner Volumes**    | Block storage   | €0.052/GB/mo | Persistent Docker volumes, DB data |
| **Storage Box (BX11)** | 100 GB NAS/SFTP | €3.43/mo     | Dev/staging backups                |
| **Storage Box (BX31)** | 1 TB NAS/SFTP   | €8.69/mo     | Production backups                 |
| **Storage Box (BX61)** | 5 TB NAS/SFTP   | €34.45/mo    | Large backup archives              |
| **Object Storage**     | S3-compatible   | €0.012/GB/mo | Static assets, media, archives     |

### Load Balancers

| Type     | Price/mo | Max targets | Notes        |
| -------- | -------- | ----------- | ------------ |
| **LB11** | €5.39    | 5           | Small setups |
| **LB21** | €10.79   | 25          | Mid-scale    |
| **LB31** | €21.59   | 50          | Production   |

### Networking

| Resource                              | Price                                    | Notes                              |
| ------------------------------------- | ---------------------------------------- | ---------------------------------- |
| **Inbound traffic**                   | Free                                     | No charge                          |
| **Outbound traffic (within Hetzner)** | Free                                     | Between servers on private network |
| **Outbound traffic (internet)**       | Included in server plan (20 TB+ typical) |                                    |
| **Private network (VLAN)**            | Free                                     | Internal communication             |
| **Floating IP**                       | €2/mo                                    | Static, reassignable IP            |
| **IPv6**                              | Free                                     | Per server                         |

### SSL / TLS

| Method                                          | Cost        | Use case                           |
| ----------------------------------------------- | ----------- | ---------------------------------- |
| **Let's Encrypt (Certbot)**                     | Free        | Default for all servers            |
| **Wildcard cert (Let's Encrypt DNS challenge)** | Free        | `*.client.com` multi-subdomain     |
| **Commercial SSL**                              | €50–€300/yr | Enterprise client requirement only |

---

## Environment Tiers

Every project at LIONGATE SARL follows a structured environment model. Each environment has its own infrastructure and is **never shared with another project**.

### Environment Overview

| Environment                     | Purpose                     | Data                 | Access                   | Cost level            |
| ------------------------------- | --------------------------- | -------------------- | ------------------------ | --------------------- |
| **Development (dev)**           | Local dev / feature work    | Synthetic / seeded   | Internal devs            | Minimal (often local) |
| **Staging**                     | Integration testing, UAT    | Anonymized prod copy | Internal + client review | Low                   |
| **Production**                  | Live user-facing system     | Real data            | Controlled               | Medium–High           |
| **High-performance production** | High-traffic or SLA-bound   | Real data            | Controlled               | High                  |
| **Low-cost production**         | Internal tools, low traffic | Real data            | Controlled               | Low                   |

### Environment Infrastructure Profiles

#### Development Environment

```
┌─────────────────────────────────────────┐
│  Local machine or shared dev server      │
│  Docker Compose (all services locally)  │
│  No external access required            │
│  Seeded / synthetic data only           │
└─────────────────────────────────────────┘
Recommended: Docker Desktop / local compose
External hosting: Optional - CX22 if shared team dev server needed
Cost: €0 (local) or ~€4/mo (shared dev VPS)
```

#### Staging Environment

```
┌─────────────────────────────────────────────┐
│  Hetzner Cloud VPS (CX32 or CX42)           │
│  App + DB on same server or separate        │
│  SSL enabled, domain: staging.project.com   │
│  Accessible to internal team + client       │
│  Reset/seed data regularly                  │
└─────────────────────────────────────────────┘
Cost: €8–€20/mo per project
```

#### Standard Production Environment

```
┌────────────────────────────────────────────────────┐
│  App Server: Hetzner CX42 (8 vCPU, 16 GB RAM)     │
│  DB Server: Hetzner CX32 (4 vCPU, 8 GB RAM)       │
│  Volume: 100 GB block storage for DB data          │
│  Backup: Storage Box BX31 (1 TB)                   │
│  Floating IP: Yes                                  │
│  SSL: Let's Encrypt                                │
│  Monitoring: Self-hosted (Grafana stack)           │
└────────────────────────────────────────────────────┘
Cost: ~€30–€55/mo
```

#### High-Performance Production Environment

```
┌────────────────────────────────────────────────────────┐
│  App Server: Hetzner CCX33 or bare metal AX41          │
│  DB Server: CCX23 (dedicated CPU, 16 GB RAM)           │
│  Load Balancer: LB21                                   │
│  Volume: 200–500 GB block storage                      │
│  Backup: Storage Box BX61 (5 TB)                       │
│  Floating IP: Yes (2 for HA)                           │
│  Monitoring: Grafana + alerting                        │
│  Optional: Redis cache server (CX22)                   │
└────────────────────────────────────────────────────────┘
Cost: ~€100–€200/mo
```

#### Low-Cost Production Environment

```
┌──────────────────────────────────────────────────┐
│  Single server: Hetzner CX32 (app + DB)          │
│  Volume: 50 GB block storage                     │
│  Backup: Storage Box BX11 (100 GB)               │
│  SSL: Let's Encrypt                              │
│  Monitoring: Lightweight (UptimeRobot free tier) │
└──────────────────────────────────────────────────┘
Cost: ~€12–€18/mo
Best for: Internal tools, admin dashboards, low-traffic sites
```

---

## Project Type Profiles

Each project type has defined infrastructure needs. This section maps project type to infrastructure.

### Profile 1 - Standard Web Application

```
Backend: Node.js / Python / etc.
Frontend: React / Vue (served as static or via server)
Database: PostgreSQL
Traffic: Low to medium (< 10k req/day)

Recommended setup:
  - App + reverse proxy: CX32 or CX42
  - Database: Separate CX22 or included on same server
  - Storage: 40–80 GB volume
  - Backup: Storage Box BX11
  - SSL: Let's Encrypt
  - Monitoring: Basic

Estimated monthly cost: €15–€35
```

### Profile 2 - Database-Heavy Application

```
Backend: API-heavy, complex queries, large datasets
Database: PostgreSQL with large schema and significant storage

Recommended setup:
  - App server: CX42
  - DB server: CX42 or CCX23 (dedicated CPU)
  - Volume: 200–500 GB
  - Backup: Storage Box BX31 + S3
  - Read replica: Optional CX32

Estimated monthly cost: €45–€100
```

### Profile 3 - Odoo Instance

```
Odoo ERP / CRM deployment for internal use or client

Recommended setup:
  - App server: CX42 or CCX23 (Odoo is CPU/RAM-intensive)
  - DB server: CX32 or CX42 dedicated
  - Volume: 100–300 GB (for filestore + DB)
  - Backup: Storage Box BX31
  - Workers: 4–8 Odoo workers
  - Cron: 2 dedicated workers

Minimum viable: CX42 all-in-one (8 vCPU, 16 GB RAM)
Recommended: App CX42 + DB CX32 separated

Estimated monthly cost: €30–€70
See: 02-odoo-deployment-docker-image.md and 03-odoo-deployment-github-source.md
```

### Profile 4 - Internal Enterprise Tools

```
Admin panels, dashboards, internal portals

Recommended setup:
  - CX32 all-in-one
  - Small volume (20–50 GB)
  - Basic backup

Estimated monthly cost: €10–€18
```

### Profile 5 - High-Traffic Client Project

```
E-commerce, SaaS product, platform with real-time features

Recommended setup:
  - 2+ App servers behind load balancer
  - Dedicated DB server (CCX series)
  - Redis cache
  - CDN (Cloudflare free tier or paid)
  - Volume: 200+ GB
  - Backup: BX61 + S3
  - Monitoring: Full Grafana/Prometheus stack
  - CI/CD: GitHub Actions + auto-deploy

Estimated monthly cost: €120–€250+
```

---

## Pricing Model & Cost Estimates

### Monthly Cost Summary Table

| Environment      | Compute | DB      | Storage | Backup | LB  | SSL  | Monitoring | Total/mo       |
| ---------------- | ------- | ------- | ------- | ------ | --- | ---- | ---------- | -------------- |
| Dev (local)      | €0      | €0      | €0      | €0     | €0  | €0   | €0         | **€0**         |
| Dev (shared VPS) | €4      | incl.   | incl.   | €0     | €0  | Free | €0         | **~€4**        |
| Staging          | €8–€18  | incl.   | €2–€5   | €3.43  | €0  | Free | €0         | **~€13–€26**   |
| Prod (low)       | €8      | incl.   | €5      | €3.43  | €0  | Free | Free tier  | **~€17**       |
| Prod (standard)  | €18–€36 | €8–€18  | €5–€10  | €8.69  | €0  | Free | €5         | **~€40–€70**   |
| Prod (high-perf) | €45–€90 | €23–€45 | €10–€26 | €34    | €11 | Free | €10        | **~€130–€215** |

### Hidden Costs to Account For

| Cost                           | Frequency | Notes                                       |
| ------------------------------ | --------- | ------------------------------------------- |
| **Domain registration**        | Annual    | €10–€30/year per domain                     |
| **Floating IP**                | Monthly   | €2/IP - assign per production environment   |
| **Snapshot storage**           | Monthly   | €0.0119/GB × snapshot size                  |
| **Bandwidth overage**          | Rare      | >20 TB outbound (unlikely at startup scale) |
| **Hetzner Managed DB premium** | Monthly   | +€20–€60 vs self-hosted                     |
| **Cloudflare paid plan**       | Monthly   | €20+/mo if advanced WAF needed              |
| **Commercial SSL**             | Annual    | Only if client mandates                     |
| **External email provider**    | Monthly   | Mailgun / Postmark / Brevo                  |

### Annual Budget Estimation Template

```
Project: [Name]
Type: [Standard / Odoo / High-traffic / Internal]
Environments: [Dev, Staging, Production]

Monthly cost breakdown:
  Production:   € ___
  Staging:      € ___
  Dev server:   € ___
  Domains:      € ___ / 12 (amortized)
  Email:        € ___
  Other:        € ___

Total/month:    € ___
Total/year:     € ___

Reviewed by: [Name]
Date: [Date]
```

---

## Infrastructure Selection Rules

Use these decision rules when provisioning new infrastructure.

### Server Selection Decision Tree

```
Is this a development environment?
├── YES → Docker locally or shared CX22 dev server
└── NO → Continue

Is this a staging environment?
├── YES → CX32 (combined) or CX22+CX22 (split)
└── NO → Continue

Is this production?
└── YES → Apply production rules below

What is the expected traffic / resource profile?
├── Low (< 1k users/day, internal tools)
│   └── CX32 all-in-one + BX11 backup
├── Medium (1k–50k users/day, standard app)
│   └── CX42 app + CX32 db + BX31 backup
├── High (50k+ users/day, SaaS/e-commerce)
│   └── CCX33 app × 2 + LB + CCX23 db + BX61 backup
└── Odoo ERP
    └── CX42 app + CX32 db + 100 GB volume + BX31 backup
```

### Database Placement Rules

| Scenario            | Decision                           |
| ------------------- | ---------------------------------- |
| Dev / staging       | DB on same server as app (simpler) |
| Production (any)    | DB on dedicated server             |
| Production with SLA | Consider Hetzner Managed DB        |
| High write load     | Dedicated CCX DB server            |
| Read-heavy          | Add read replica                   |

### Volume Rules

| Data type                 | Volume?                     | Size                |
| ------------------------- | --------------------------- | ------------------- |
| PostgreSQL data directory | **Mandatory in production** | 2× expected DB size |
| Odoo filestore            | **Mandatory**               | 50 GB minimum       |
| Application logs          | Optional                    | 20–50 GB            |
| Media / uploads           | **Mandatory**               | Based on project    |

---

## Network & Security Architecture

### Network Architecture Per Environment

```
Internet
    │
    ▼
Hetzner Floating IP (static, reassignable)
    │
    ▼
Firewall (Hetzner Firewall Rules)
    │
    ├── Port 80  → NGINX (redirect to HTTPS)
    ├── Port 443 → NGINX (SSL termination)
    └── Port 22  → SSH (restricted to LIONGATE IPs only)
    │
    ▼
Private Network (10.0.0.0/16)
    │
    ├── App Server (10.0.0.10)
    ├── DB Server  (10.0.0.20)   ← No public IP
    └── Cache      (10.0.0.30)   ← No public IP (if applicable)
```

### Hetzner Firewall Standard Rules

```hcl
# Inbound allowed
- TCP 22   → Source: LIONGATE office IPs only (whitelist)
- TCP 80   → Source: 0.0.0.0/0 (redirect to 443)
- TCP 443  → Source: 0.0.0.0/0

# Inbound blocked (everything else)
- TCP 5432 → BLOCKED from internet (PostgreSQL)
- TCP 6379 → BLOCKED from internet (Redis)
- TCP 8069 → BLOCKED from internet (Odoo direct)

# Outbound
- Allow all (servers need to reach package repos, etc.)
```

---

## Backup & Disaster Recovery Strategy

### Backup Tiers

| Tier                    | Frequency | Retention   | Target              | Method               |
| ----------------------- | --------- | ----------- | ------------------- | -------------------- |
| **Database snapshots**  | Daily     | 7 days      | Storage Box         | `pg_dump` via cron   |
| **Database snapshots**  | Weekly    | 4 weeks     | Storage Box         | `pg_dump` via cron   |
| **Database snapshots**  | Monthly   | 12 months   | S3/Object Storage   | `pg_dump` compressed |
| **VM snapshots**        | Weekly    | 2 snapshots | Hetzner Snapshots   | Hetzner API          |
| **Filestore / uploads** | Daily     | 7 days      | Storage Box         | `rsync`              |
| **Application config**  | On change | Git history | GitHub private repo | Manual + CI          |

### Backup Storage Sizing Guide

```
DB size × 7 daily = weekly storage needed (uncompressed)
Apply 60% compression ratio for pg_dump .gz
Add 20% overhead

Example: 20 GB DB
  Daily compressed dump: ~8 GB
  7-day retention: ~56 GB
  Monthly archive: ~8 GB × 12 = ~96 GB/yr
  → Storage Box BX31 (1 TB) sufficient for most projects
```

### Recovery Time Objectives

| Scenario                 | RTO       | RPO | Method                                   |
| ------------------------ | --------- | --- | ---------------------------------------- |
| Server crash             | 15–30 min | 24h | Restore VM snapshot, restore from backup |
| DB corruption            | 30–60 min | 24h | Restore pg_dump to clean DB              |
| Accidental data deletion | 1–4h      | 24h | Restore from daily dump                  |
| Complete datacenter loss | 2–8h      | 24h | Rebuild from IaC + restore DB backup     |

### Backup Testing Schedule

- Monthly: Restore one DB backup to a test server and validate
- Quarterly: Full disaster recovery drill (rebuild from scratch)
- Document results in internal runbook

---

## Monitoring & Observability

### Monitoring Stack

| Tool                  | Purpose               | Deployment                  |
| --------------------- | --------------------- | --------------------------- |
| **Prometheus**        | Metrics collection    | Docker on monitoring server |
| **Grafana**           | Dashboards            | Docker on monitoring server |
| **Node Exporter**     | Server metrics        | On every server             |
| **Postgres Exporter** | DB metrics            | On DB servers               |
| **Nginx Exporter**    | HTTP metrics          | On app servers              |
| **Loki**              | Log aggregation       | Docker on monitoring server |
| **Alertmanager**      | Alert routing         | Docker on monitoring server |
| **UptimeRobot**       | External uptime check | Free tier (5 monitors)      |

### Standard Alerts

| Alert              | Threshold       | Severity |
| ------------------ | --------------- | -------- |
| CPU usage          | > 85% for 5 min | Warning  |
| CPU usage          | > 95% for 2 min | Critical |
| Memory usage       | > 80%           | Warning  |
| Disk usage         | > 75%           | Warning  |
| Disk usage         | > 90%           | Critical |
| DB connection pool | > 80%           | Warning  |
| HTTP 5xx rate      | > 1%            | Warning  |
| HTTP 5xx rate      | > 5%            | Critical |
| Backup job failure | Any failure     | Critical |
| SSL cert expiry    | < 14 days       | Warning  |

---

## Scaling Strategy

### Vertical Scaling (Scale Up)

Resize the server to a larger plan in Hetzner. This requires a brief downtime (5–10 min).

```
Trigger: CPU/RAM consistently > 80% for 1 week
Action:
  1. Snapshot the server
  2. Stop the server
  3. Resize in Hetzner panel (or API)
  4. Start and validate
Downtime: 5–15 minutes
```

### Horizontal Scaling (Scale Out)

Add more app servers behind a load balancer.

```
Trigger: High traffic, app servers saturated
Requirements:
  - Stateless app (no local sessions/files)
  - Shared DB server
  - Shared file storage (volumes or S3)
  - Load balancer configured
Steps:
  1. Deploy second app server
  2. Configure Hetzner Load Balancer
  3. Update health checks
  4. Update floating IP to point to LB
```

### Database Scaling

```
Read-heavy: Add read replica PostgreSQL
Write-heavy: Scale DB server vertically (CCX series)
Data volume: Add/resize Hetzner volume
Connection pooling: Use PgBouncer in front of PostgreSQL
```

---

## Infrastructure-as-Code Standards

All LIONGATE infrastructure must be defined as code. This enables:

- Reproducibility
- Audit trail
- Onboarding efficiency
- Disaster recovery

### Tooling

| Tool                                | Purpose                                                 |
| ----------------------------------- | ------------------------------------------------------- |
| **Terraform**                       | Provision Hetzner servers, networks, firewalls, volumes |
| **Ansible**                         | Configure servers (OS setup, packages, services)        |
| **Docker Compose**                  | Application stack definition                            |
| **GitHub Actions**                  | CI/CD pipelines                                         |
| **Vault / `.env` + Secret Manager** | Secrets management                                      |

### Repository Structure

```
infrastructure/
├── terraform/
│   ├── environments/
│   │   ├── staging/
│   │   └── production/
│   ├── modules/
│   │   ├── server/
│   │   ├── database/
│   │   ├── network/
│   │   └── backup/
│   └── README.md
├── ansible/
│   ├── playbooks/
│   │   ├── base-server.yml
│   │   ├── install-docker.yml
│   │   └── configure-nginx.yml
│   └── inventory/
└── README.md
```

---

## Cost Control Policies

### Rules

1. **No server is provisioned without a cost estimate in the project ticket**
2. **Dev servers are destroyed or stopped when not in use**
3. **Staging servers are stopped outside business hours** (saves ~60% on compute)
4. **VM snapshots are limited** - old ones must be deleted after each rotation
5. **Monthly billing review** is mandatory - alert if actual > estimate by > 20%
6. **No floating IP is left unattached** - they still cost €2/mo

### Monthly Review Checklist

- [ ] Review Hetzner billing dashboard
- [ ] Check for orphaned servers, volumes, snapshots
- [ ] Validate that stopped servers are stopped (staging)
- [ ] Compare actual spend to project budget
- [ ] Archive or delete backups beyond retention policy
- [ ] Renew domains expiring within 60 days
- [ ] Review SSL certificate expiration dates

---

_Document owner: LIONGATE SARL IT/DevOps Team_
_Last updated: See Git history_
_Next review: Quarterly_
