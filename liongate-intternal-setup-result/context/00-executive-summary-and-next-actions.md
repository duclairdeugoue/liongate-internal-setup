# LIONGATE SARL - Documentation Package: Executive Summary

**Production-grade Technical & Project Management Documentation**
_Produced by: AI Architecture & Documentation System_
_Target: LIONGATE SARL Engineering & Leadership Teams_

---

## Executive Summary

This documentation package provides LIONGATE SARL with a complete operational foundation across six critical domains: infrastructure strategy, Odoo ERP deployment (two methods), internal engineering standards, SDLC process, and project management tooling.

All six documents are written at **production-grade depth**, structured for direct use in a company wiki, GitHub repository, or Notion/Confluence handbook. They are immediately actionable - every major section includes commands, templates, checklists, and decision criteria grounded in real-world implementation.

---

## Generated Documentation Files

| File                                                                           | Title                                               | Pages (approx.) | Primary audience          |
| ------------------------------------------------------------------------------ | --------------------------------------------------- | --------------- | ------------------------- |
| `01-hosting-platform-billing-and-infrastructure-strategy.pdf`                  | Hosting Platform, Billing & Infrastructure Strategy | ~30             | DevOps, PM, Founders      |
| `02-odoo-deployment-docker-image.pdf`                                          | Odoo Deployment: Docker Image                       | ~40             | DevOps, Backend Engineers |
| `03-odoo-deployment-github-source.pdf`                                         | Odoo Deployment: GitHub Source Code                 | ~35             | DevOps, Backend Engineers |
| `04-enterprise-tech-stack-for-products-and-client-projects.pdf`                | Enterprise Tech Stack                               | ~40             | All Engineers, TL         |
| `05-sdlc-and-project-management-process.pdf`                                   | SDLC & Project Management Process                   | ~35             | PM, TL, All Engineers     |
| `06-project-management-using-enterprise-standard-and-odoo-github-projects.pdf` | Project Management: Enterprise & Odoo+GitHub        | ~45             | PM, TL, Founders          |

**Total documentation:** ~225 pages of production-grade technical content

---

## Assumptions Made

The following assumptions were made during documentation production. Review each one and adjust the documents where your reality differs.

### Infrastructure Assumptions

| Assumption                                                                 | Impact if wrong                                                 |
| -------------------------------------------------------------------------- | --------------------------------------------------------------- |
| Hetzner Cloud (EU region: Nuremberg `nbg1`) is the primary datacenter      | Update region codes and latency considerations                  |
| Ubuntu 22.04 LTS is the operating system for all servers                   | Adjust package manager commands (`apt` → `dnf` or other)        |
| Servers are provisioned via Hetzner Cloud (VPS), not bare metal by default | Bare metal provisioning takes 24–48h; adjust planning timelines |
| All outbound SSH access is locked to a LIONGATE office IP                  | Adjust firewall rules if team is fully remote                   |
| Let's Encrypt is the default SSL provider                                  | If commercial SSL is required, adjust Nginx + Certbot setup     |
| United Domains is used exclusively for DNS management                      | If you use Cloudflare for DNS, adapt DNS management steps       |
| Hetzner Storage Box is used for backups                                    | Adjust backup scripts if using S3, Backblaze B2, or other       |
| Floating IPs are used for production servers                               | Required for zero-downtime server replacement; keep this        |
| Private network range is `10.0.0.0/16`                                     | Adjust subnet if conflicting with VPN or existing network       |

### Odoo Assumptions

| Assumption                                            | Impact if wrong                                                           |
| ----------------------------------------------------- | ------------------------------------------------------------------------- |
| Odoo 17.0 Community Edition                           | Enterprise Edition adds proprietary modules; source checkout differs      |
| PostgreSQL 16                                         | If using v14/v15, most commands are identical but check config            |
| `odoo:17.0` is the Docker Hub tag used                | Update image tag for 16.0, 18.0                                           |
| wkhtmltopdf 0.12.6.1 Jammy build                      | Required for PDF generation; verify package URL for other Ubuntu versions |
| Workers are sized for CX42 (8 vCPU, 16 GB RAM)        | Recalculate workers formula for different server sizes                    |
| SMTP provider is Brevo (Sendinblue)                   | Any SMTP provider works; adjust host/port/auth settings                   |
| Custom addons are stored in a separate git repository | Adapt workflow if custom addons are in a monorepo                         |
| Odoo filestore remains on the app server              | If using shared storage (NFS, S3), adapt volume mounts                    |

### Tech Stack Assumptions

| Assumption                                           | Impact if wrong                                                                      |
| ---------------------------------------------------- | ------------------------------------------------------------------------------------ |
| NestJS (TypeScript) is the primary backend framework | FastAPI (Python) documented as alternative; update CI/CD and Dockerfiles accordingly |
| Next.js is the primary frontend framework            | React without Next.js requires different SSR/build setup                             |
| PostgreSQL is the exclusive primary database         | If MySQL/MariaDB is required by client, adapt migrations and connection config       |
| Redis is used for queues and caching                 | If no Redis: Bull queues unavailable; use in-memory alternatives                     |
| GitHub is the Git hosting platform                   | GitLab or Bitbucket will require adapting CI/CD pipelines                            |
| Docker Compose v2 (plugin) is used                   | `docker compose` (no hyphen); if using v1 `docker-compose`, syntax differs           |
| GitHub Actions is the CI/CD platform                 | Adapt for GitLab CI, CircleCI, or Jenkins if required                                |

### Project Management Assumptions

| Assumption                                                           | Impact if wrong                                                        |
| -------------------------------------------------------------------- | ---------------------------------------------------------------------- |
| 2-week sprints are the standard iteration length                     | 1-week sprints: halve capacity calculations. 3-week: scale accordingly |
| Team velocity: ~25 story points per engineer per sprint              | Calibrate after first 3 sprints using actual data                      |
| Business hours: Monday–Friday, West/Central Africa Time (WAT, UTC+1) | Adjust SLA response times and monitoring alert schedules               |
| GitHub Free/Pro is used (not GitHub Enterprise)                      | GitHub Enterprise adds IP allowlisting, SAML SSO, audit logs           |
| Odoo Community Edition is self-hosted (not Odoo.sh)                  | Odoo.sh changes deployment and upgrade procedures significantly        |
| PO and PM are distinct roles                                         | In lean teams, one person may hold both; adjust RACI accordingly       |
| Client projects involve external clients with portal access          | For internal-only tools, disable Odoo portal                           |

---

## Decisions Taken

The following explicit decisions were made during documentation production, with rationale provided.

### Infrastructure Decisions

| Decision                                       | Alternative considered  | Reason chosen                                                |
| ---------------------------------------------- | ----------------------- | ------------------------------------------------------------ |
| **Self-hosted PostgreSQL** on dedicated server | Hetzner Managed DB      | Lower cost; team has DB ops experience                       |
| **Let's Encrypt** for SSL                      | Commercial SSL          | Free, auto-renewable, sufficient for all use cases           |
| **Nginx** as reverse proxy                     | Traefik, Caddy, HAProxy | Universal, well-documented, handles Odoo proxy correctly     |
| **Storage Box** for backups                    | S3/Object Storage       | Simpler SFTP/rsync workflow; S3 for long-term archive        |
| **Hetzner Volumes** for DB data                | Local NVMe only         | Volumes persist after server deletion; critical for recovery |
| **Floating IP** per production server          | Direct server IP        | Required for zero-downtime server replacement                |

### Odoo Decisions

| Decision                           | Alternative considered | Reason chosen                                                       |
| ---------------------------------- | ---------------------- | ------------------------------------------------------------------- |
| **Docker image** as primary method | Source-only            | Faster setup, managed Python, most teams prefer Docker              |
| **Source deployment** as secondary | Docker only            | Needed for deep patching, OCA compatibility work, hot-fix scenarios |
| **Odoo 17.0** as version target    | 16.0 or 18.0           | 17.0 is current stable LTS; 16.0 is mature but aging                |
| **Separated app + DB servers**     | All-in-one             | Security (DB not exposed), independent scaling, standard practice   |
| **pg_dump** format backups         | Full volume snapshots  | Faster restore, smaller size, DB-version portable                   |
| **Workers = (CPU×2)+1 formula**    | Fixed number           | Scales with server tier; avoid over/under-provisioning              |

### Tech Stack Decisions

| Decision                          | Alternative considered             | Reason chosen                                                           |
| --------------------------------- | ---------------------------------- | ----------------------------------------------------------------------- |
| **NestJS** as primary backend     | Express.js, Fastify, Django        | Opinionated architecture prevents drift; TypeScript safety              |
| **FastAPI** as Python alternative | Django REST, Flask                 | Auto-docs, Pydantic validation, async-first; better than Flask for APIs |
| **Next.js** as frontend           | Create React App, Vite+React, Vue  | SSR capability, file-based routing, production-ready out of box         |
| **Tailwind CSS** for styling      | Bootstrap, Material UI, Ant Design | Utility-first; prevents CSS debt; consistent with Next.js ecosystem     |
| **React Query** for server state  | Redux Toolkit Query, SWR           | Best developer experience; standardizes caching, invalidation           |
| **Zustand** for global state      | Redux, MobX                        | Minimal boilerplate; sufficient for 95% of use cases                    |
| **Jest** as test runner           | Vitest, Mocha                      | Ecosystem size; built-in mocking; integrates with NestJS                |
| **Playwright** for E2E            | Cypress                            | Cross-browser; faster; official Microsoft support                       |
| **Conventional Commits**          | Custom commit format               | Enables auto-changelog, semantic versioning                             |
| **Squash merge** as default       | Merge commit, rebase               | Clean linear history on main/develop                                    |

### Project Management Decisions

| Decision                                            | Alternative considered                  | Reason chosen                                                      |
| --------------------------------------------------- | --------------------------------------- | ------------------------------------------------------------------ |
| **Hybrid Odoo+GitHub** as LIONGATE default          | Pure Jira or pure GitHub                | Odoo manages business; GitHub manages engineering; zero added cost |
| **Case A (Jira)** documented for enterprise clients | Jira-only for all                       | Some clients require Jira; team must be ready to operate both      |
| **2-week sprints** as standard                      | 1-week (too short), 3-week (too long)   | Industry sweet spot; enough work per cycle without drift           |
| **MoSCoW prioritization**                           | RICE, Kano, Weighted Shortest Job First | Simple to communicate to non-technical stakeholders                |
| **Story points** for estimation                     | Hours, T-shirt sizes                    | Decouples estimate from time; forces relative comparison           |
| **GitHub Environments** with approval gate          | Manual deploy only                      | Formalizes production approval without extra tooling               |
| **Blameless post-mortems**                          | Blame-based                             | Produces better learning; industry-standard practice               |

---

## Open Questions

The following questions require decisions from the LIONGATE SARL team before full implementation.

### Infrastructure

1. **What is LIONGATE's primary Hetzner region?** - `nbg1` (Nuremberg) assumed. If clients are predominantly in Africa, consider `hel1` (Helsinki) or awaiting Hetzner Africa region launch.

2. **Will LIONGATE use a Hetzner Organization account or individual accounts?** - Organization accounts enable shared billing, team access, and project isolation.

3. **What is the static IP of the LIONGATE office?** - Required to configure firewall SSH allowlist rules. If team is remote, an alternative approach (VPN or jump host) is needed.

4. **Is there an existing VPN for remote team access?** - If yes, document the VPN IP range in firewall rules instead of individual IPs.

5. **What Storage Box plan will LIONGATE use?** - BX31 (1 TB at €8.69/mo) is documented as default. Confirm this fits budget and expected backup size.

6. **Will LIONGATE maintain a shared monitoring server?** - Recommended: one CX22 Grafana/Prometheus/Loki server shared across all projects to reduce per-project monitoring overhead.

### Odoo

7. **Odoo Community or Enterprise Edition?** - This documentation covers Community. Enterprise adds HR, Payroll, Sign, and other modules but costs ~€20/user/month from Odoo SA.

8. **How many Odoo databases will run?** - One DB per client/deployment is safest. Multi-database on one instance is possible but adds complexity.

9. **Will LIONGATE build custom Odoo modules?** - If yes, establish a separate private GitHub repository for custom addons and reference the custom addons deployment workflow in file `03`.

10. **What email provider will be used for Odoo outgoing mail?** - Brevo is documented as default. Confirm account creation, SMTP key, and sending domain verification.

11. **Who manages Odoo upgrades?** - Establish an upgrade owner (TL or DevOps). Minor upgrades monthly; major upgrades require planning 2–4 weeks in advance.

### Tech Stack

12. **What is the company's Python vs TypeScript proficiency split?** - This affects whether NestJS or FastAPI becomes the true default. Survey the team before standardizing.

13. **Will LIONGATE use GitHub Organizations or personal repos?** - Organization recommended for team access control, CODEOWNERS, and org-level GitHub Projects.

14. **Will GitHub Teams be used for role-based access?** - Recommended: create teams (`engineers`, `devops`, `tech-lead`) and use in CODEOWNERS and branch protection.

15. **Is there a container registry preference?** - GitHub Container Registry (GHCR) is documented as default (included with GitHub). Alternatives: Docker Hub, Hetzner Registry (not offered), self-hosted Gitea.

16. **Will LIONGATE use a shared development VPS or purely local development?** - Shared dev VPS is documented as optional (CX22). Confirm whether local Docker is sufficient for the team.

### Project Management

17. **Which Odoo modules will be enabled?** - Minimum recommended: Project, Timesheets, CRM, Invoicing. Confirm which are needed before initial Odoo setup.

18. **Will LIONGATE bill clients by time or fixed price?** - Odoo Timesheets workflow is only relevant if billing by time. Fixed-price projects still benefit from Odoo project tracking but not timesheet linking.

19. **What is the client communication channel?** - Odoo Customer Portal documented as default. Confirm if clients prefer email-only, WhatsApp, or dedicated portal.

20. **Will LIONGATE adopt Case A (Jira) or Case B (Odoo+GitHub) as the default?** - This document recommends Case B (hybrid Odoo+GitHub) for most projects, with Case A available for enterprise clients who mandate it.

21. **Does LIONGATE need on-call rotation?** - If production systems require 24/7 response, establish an on-call schedule and integrate with UptimeRobot or PagerDuty.

---

## Recommended Next Actions

### Week 1 - Foundation

| Priority    | Action                                                    | Owner     | Tool            |
| ----------- | --------------------------------------------------------- | --------- | --------------- |
| 🔴 Critical | Provision Hetzner account + create Organization           | DevOps    | Hetzner Console |
| 🔴 Critical | Create GitHub Organization: `github.com/liongate`         | DevOps    | GitHub          |
| 🔴 Critical | Register all domains in United Domains                    | DevOps/PM | United Domains  |
| 🔴 Critical | Create and configure Hetzner firewall rules               | DevOps    | Hetzner Console |
| 🔴 Critical | Deploy Odoo (Docker) on Hetzner - first internal instance | DevOps    | File `02`       |
| 🟡 High     | Set up private networking between app + DB servers        | DevOps    | File `01`       |
| 🟡 High     | Configure Storage Box and test backup script              | DevOps    | File `02`       |
| 🟡 High     | Obtain SSL certificate for internal Odoo domain           | DevOps    | File `02`       |

### Week 2 - Engineering Standards

| Priority    | Action                                                | Owner  | Tool            |
| ----------- | ----------------------------------------------------- | ------ | --------------- |
| 🔴 Critical | Create GitHub repository template with standard files | TL     | GitHub          |
| 🔴 Critical | Set up issue templates and PR template                | TL     | GitHub          |
| 🔴 Critical | Configure branch protection rules on all repos        | TL     | GitHub          |
| 🟡 High     | Set up first GitHub Project board for active project  | PM     | GitHub Projects |
| 🟡 High     | Create all standard labels in GitHub repos            | DevOps | GitHub CLI      |
| 🟡 High     | Configure GitHub Actions CI workflow on first project | DevOps | File `04`       |
| 🟡 High     | Add CODEOWNERS file to repos                          | TL     | GitHub          |
| 🟢 Medium   | Set up Grafana + Prometheus on monitoring VPS         | DevOps | File `04`       |

### Week 3 - Process Adoption

| Priority    | Action                                                       | Owner   | Tool            |
| ----------- | ------------------------------------------------------------ | ------- | --------------- |
| 🔴 Critical | Run first sprint planning meeting using process in File `05` | PM      | GitHub Projects |
| 🔴 Critical | Enable Odoo Project + Timesheets modules                     | DevOps  | Odoo            |
| 🔴 Critical | Create first Odoo project per active client                  | PM      | Odoo            |
| 🟡 High     | Document first Architecture Decision Record (ADR)            | TL      | GitHub repo     |
| 🟡 High     | Run first backlog refinement meeting                         | PM + TL | GitHub Issues   |
| 🟡 High     | Establish weekly PM sync (Odoo ↔ GitHub) protocol            | PM      | -               |
| 🟢 Medium   | Set up UptimeRobot monitors for all production URLs          | DevOps  | UptimeRobot     |
| 🟢 Medium   | Configure Slack notifications from GitHub + Grafana          | DevOps  | Slack           |

### Month 2 - Hardening

| Priority    | Action                                                      | Owner       | Tool                  |
| ----------- | ----------------------------------------------------------- | ----------- | --------------------- |
| 🔴 Critical | Run first backup restore test (monthly)                     | DevOps      | File `02/03`          |
| 🟡 High     | First dependency security scan in CI                        | DevOps      | npm audit / pip-audit |
| 🟡 High     | First sprint retrospective - refine process                 | PM + Team   | File `05`             |
| 🟡 High     | Document first post-mortem (even if no incident - practice) | TL          | GitHub                |
| 🟡 High     | Infrastructure cost review - compare actual vs estimate     | PM + DevOps | Hetzner Billing       |
| 🟢 Medium   | Set up Terraform for Hetzner infrastructure                 | DevOps      | File `01`             |
| 🟢 Medium   | Draft first company Architecture Standards ADR              | TL          | GitHub                |
| 🟢 Medium   | Calibrate team velocity from first 2 sprints                | PM          | GitHub Projects       |

### Month 3+ - Scale & Optimize

| Priority  | Action                                                     | Owner         |
| --------- | ---------------------------------------------------------- | ------------- |
| 🟡 High   | Review and update all 6 documents based on real usage      | TL + PM       |
| 🟡 High   | Establish quarterly infrastructure cost review ritual      | PM + DevOps   |
| 🟡 High   | Evaluate whether Odoo Enterprise modules are needed        | PM + Founders |
| 🟢 Medium | Explore Hetzner Terraform provider for full IaC            | DevOps        |
| 🟢 Medium | Evaluate Odoo upgrade path to next minor version           | DevOps + TL   |
| 🟢 Medium | Set up end-to-end test suite (Playwright) on first project | TL + QA       |
| 🟢 Medium | Conduct first formal security review                       | TL + DevOps   |

---

## Document Maintenance

These documents are **living documentation**. Assign ownership and review schedule:

| Document              | Owner           | Review cadence                    |
| --------------------- | --------------- | --------------------------------- |
| `01` Hosting strategy | DevOps Engineer | Quarterly                         |
| `02` Odoo Docker      | DevOps Engineer | After each Odoo version bump      |
| `03` Odoo Source      | DevOps Engineer | After each Odoo version bump      |
| `04` Tech stack       | Tech Lead       | Quarterly or on major tech change |
| `05` SDLC process     | Project Manager | After each retrospective cycle    |
| `06` PM tooling       | Project Manager | Quarterly                         |

**Storage recommendation:** Place all 6 files in a `docs/handbook/` directory in a dedicated LIONGATE private GitHub repository (e.g., `github.com/liongate/engineering-handbook`).

---

_Package produced for: LIONGATE SARL_
_Production date: 2024_
_Status: Ready for team review and implementation_
