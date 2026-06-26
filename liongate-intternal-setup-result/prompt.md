- TASK 1: Deployment Hosting platform
  - website: https://hetzner.com
  - its a german platoform
  - We should create a markdown document file stating the hosting bills for servers, database, and other services for whicb we will be using, take noe that each project can have different environment setup (eg: case of odoo instance where we need a specific xtics for server, database and so on and another case of other projects that might need standard performance or higher performance, or low performance etc ). let it be will documented
  - We will be using our domain names coming from united-domain service

- TASK 2: Deployment of Odoo project (2 cases: Using both docker image and using official github source code)
  - For each case, We need to have a markdown file having a full architecture and tech stack of an on-premise deployment of an odoo instance stating steps to setup it locally and online deployment, in that same file, the complete steps that we should follow inoder to have the odoo instance fully deployed and working an the hetzner servers (from setting up the server, configuring the project dependencies, databbase setup, environment, attaching the domain to the project and testing to making sure it works completely and ready to be use for production).

- TASK 3 Internal stack for application concerning backend, frontend, database and devops
  - This time around, our enteprise (LIONGATE SARL), since we are still setting up the enteprise as a startup for the developers and the enginners, We will need production grade markdown documentation files for tech stack that we can use for building internal projects, or external client project, taking into consideration we build from stratch for the clients from requirement till deployment. for each step of the SDLC, we need to know exactly what we will be using, also files related to the project management of a software project stating how task are managed and so on, make sure to take 2 cases ie the case that most enterprise uses in thier company and the other case, using odoo to manage the project and github projects too.

AI ready prompt

# Master AI Prompt — LIONGATE SARL Deployment, Odoo, and Internal Tech Stack Documentation

You are a **senior DevOps architect, solution architect, ERP deployment engineer, and technical documentation specialist**.

Your mission is to produce **production-grade markdown documentation files** for **LIONGATE SARL** covering:

1. **Hosting platform strategy and cost planning**
2. **Odoo deployment architecture and implementation**
3. **Internal enterprise tech stack and SDLC/project management standards**

The goal is to create documentation that is **clear, actionable, scalable, and suitable for a startup that wants to build client projects from scratch and deploy them reliably in production**.

---

## 1) Context

### Company

- **Company name:** LIONGATE SARL
- **Business stage:** Startup / enterprise setup phase
- **Use case:** Building internal products and external client projects from scratch, from requirements gathering to production deployment

### Hosting provider

- **Primary hosting provider:** Hetzner
- **Provider website:** https://hetzner.com
- **Provider type:** German hosting platform
- Assume the infrastructure can include:
  - Bare metal servers
  - Cloud servers / VPS
  - Managed or self-managed databases
  - Storage volumes
  - Backups
  - Load balancing
  - Networking / firewall
  - Monitoring / alerting
  - DNS / domain mapping

### Domain provider

- Domains come from **United Domains** (united-domain service)
- The documentation must include how to connect and manage domains from this provider to hosted services.

---

## 2) What you must produce

Create **multiple markdown documentation files** with a professional, production-ready structure.

### Required output files

#### File 1 — Hosting platform strategy and billing

**Suggested filename:** `01-hosting-platform-billing-and-infrastructure-strategy.md`

This file must document:

- Hosting bill expectations for:
  - Servers
  - Databases
  - Storage
  - Backups
  - Load balancers
  - Monitoring
  - Networking
  - Optional services
- Different environment tiers:
  - Development
  - Staging
  - Production
  - High-performance production
  - Low-performance / lightweight production
- Different project types and resource profiles:
  - Standard web app
  - Database-heavy app
  - Odoo instance
  - Internal enterprise tools
  - High-traffic client project
- A clear explanation that **each project may require a different server/database setup**
- A documented pricing strategy and cost model:
  - Monthly estimates
  - Per-environment costs
  - Possible hidden costs
  - Scaling costs
  - Backup and disaster recovery costs
- Domain management strategy using United Domains
- Clear rules for selecting infrastructure based on workload size

#### File 2 — Odoo deployment using Docker image

**Suggested filename:** `02-odoo-deployment-docker-image.md`

This file must cover:

- Full architecture and tech stack for an **on-premise style Odoo deployment**
- Using an **official or trusted Docker image**
- Local deployment steps
- Online deployment steps on Hetzner
- Full production deployment path:
  - server provisioning
  - OS preparation
  - firewall setup
  - Docker installation
  - container architecture
  - PostgreSQL database setup
  - Odoo configuration
  - volumes and persistence
  - environment variables
  - reverse proxy
  - SSL/TLS
  - domain attachment
  - backup strategy
  - logging and monitoring
  - testing and validation
- Explain how to make the system production-ready
- Include a deployment checklist

#### File 3 — Odoo deployment from official GitHub source code

**Suggested filename:** `03-odoo-deployment-github-source.md`

This file must cover:

- Full architecture and tech stack for an **on-premise style Odoo deployment from source**
- Local setup
- Online deployment on Hetzner
- Full step-by-step implementation:
  - server provisioning
  - dependencies installation
  - Python environment
  - PostgreSQL setup
  - Odoo source checkout
  - addons path
  - config file setup
  - service manager setup
  - reverse proxy
  - SSL/TLS
  - domain mapping
  - performance tuning
  - production hardening
  - backups
  - monitoring
  - validation/testing
- Compare source deployment vs Docker deployment
- Include pros, cons, risks, operational overhead, and recommended use cases

#### File 4 — Internal enterprise tech stack

**Suggested filename:** `04-enterprise-tech-stack-for-products-and-client-projects.md`

This file must define a **production-grade internal stack** for LIONGATE SARL, covering:

- Backend stack
- Frontend stack
- Database stack
- DevOps stack
- Testing stack
- Observability stack
- Security stack
- API design stack
- CI/CD stack
- File and repository structure standards
- Environment management standards
- Versioning strategy
- Code quality standards

This documentation must include:

- The recommended stack for most enterprise projects
- Alternative stack options for lighter or more advanced projects
- A justified recommendation for what the company should standardize on
- A clear separation between:
  - internal products
  - client projects
  - prototype / MVP projects
  - enterprise-grade projects

#### File 5 — SDLC and project management process

**Suggested filename:** `05-sdlc-and-project-management-process.md`

This file must define how software projects are managed from:

- discovery
- requirements gathering
- estimation
- architecture
- development
- code review
- testing
- staging
- deployment
- monitoring
- maintenance
- change management
- support

It must also define:

- how tasks are created
- how tasks are assigned
- how priorities are managed
- how milestones are tracked
- how bugs are handled
- how releases are approved
- how documentation is maintained
- how technical debt is tracked

#### File 6 — Project management using enterprise standard tools and Odoo + GitHub Projects

**Suggested filename:** `06-project-management-using-enterprise-standard-and-odoo-github-projects.md`

This file must contain **two cases**:

##### Case A: Enterprise-standard project management

Document how most mature enterprises manage projects using tools and processes such as:

- Jira / equivalent issue tracking
- Confluence / equivalent documentation
- Slack / Teams communication
- Git-based workflows
- sprint planning
- backlog refinement
- release planning
- QA and acceptance workflow
- incident management
- change requests

##### Case B: Odoo + GitHub Projects

Document how LIONGATE SARL can manage projects using:

- Odoo for business/project management
- GitHub Projects for engineering execution
- GitHub Issues / Pull Requests
- milestones
- labels
- boards
- workflows
- approval flow
- release tracking
- traceability between business tasks and engineering tasks

The document must compare both cases and recommend when to use each one.

---

## 3) Mandatory quality requirements

For all documents:

### You must make the documentation:

- **Production-grade**
- **Practical**
- **Detailed**
- **Implementation-oriented**
- **Structured**
- **Easy to maintain**
- **Suitable for a company wiki / handbook / technical runbook**
- **Written in clean, professional markdown**

### You must include:

- Clear headings and subheadings
- Tables where useful
- Checklists where useful
- Step-by-step implementation instructions
- Decision criteria
- Alternatives and trade-offs
- Risks and mitigations
- Security considerations
- Scalability considerations
- Backup and disaster recovery considerations
- Monitoring/observability considerations
- Deployment validation steps
- Production readiness criteria

### You must avoid:

- Vague explanations
- High-level fluff without execution steps
- Missing assumptions
- Unexplained acronyms
- Non-actionable advice

---

## 4) Infrastructure and architecture expectations

When writing the documentation, assume the following architectural principles:

### Hosting strategy

Different projects can have different requirements, for example:

- **Odoo** may need specific server and database characteristics
- Some projects may need **standard performance**
- Some may need **low performance** and low cost
- Some may need **high performance** and higher availability
- Some may need isolated environments per client

### Must document environment options

For each architecture, describe:

- Minimum viable setup
- Recommended setup
- Production setup
- Scalable / high-availability setup

### Must document services

For each project type, define the expected usage of:

- Compute
- Database
- Storage
- Backup
- DNS
- SSL
- Reverse proxy
- Monitoring
- Logs
- Secrets management
- CI/CD runners or build agents
- Optional caching or queueing services

---

## 5) Odoo-specific requirements

For the Odoo documentation, include:

- Odoo version assumptions
- PostgreSQL version assumptions
- Python/runtime dependencies
- Recommended file system layout
- Custom addons strategy
- Community vs enterprise considerations if relevant
- Persistent volumes and backups
- Email/SMTP configuration
- Incoming/outgoing network requirements
- Cron jobs / scheduled actions
- Worker/process configuration
- Proxy and trusted host configuration
- Security hardening
- Upgrade path
- Recovery and restore strategy

The documentation must be usable by a technical team that wants to:

1. Deploy locally
2. Deploy on Hetzner
3. Connect to a real domain from United Domains
4. Run the system in production
5. Troubleshoot issues
6. Restore service after failure

---

## 6) Internal company stack requirements

For the enterprise stack document, include recommendations for:

### Backend

- preferred language and framework
- API style
- authentication and authorization
- background jobs
- caching
- validation
- error handling
- logging standards

### Frontend

- preferred frontend framework
- component architecture
- state management
- forms
- data fetching
- testing
- build and deployment strategy

### Database

- primary database
- migration strategy
- schema management
- seeding
- backups
- restore procedures
- performance tuning

### DevOps

- containerization
- orchestration
- IaC
- CI/CD pipelines
- environments
- secrets
- monitoring
- alerting
- logging
- incident response

### Quality engineering

- unit tests
- integration tests
- end-to-end tests
- linting
- formatting
- security scans
- performance testing

### Security

- secrets handling
- access control
- role-based permissions
- audit logging
- dependency scanning
- backup encryption
- SSL/TLS
- server hardening

---

## 7) Project management requirements

The project management documents must define:

- The standard lifecycle of a software project
- The responsibilities of:
  - product owner
  - project manager
  - tech lead
  - backend engineer
  - frontend engineer
  - DevOps engineer
  - QA engineer
  - designer
  - support engineer
- How work moves from idea to delivery
- How to organize:
  - epics
  - stories
  - tasks
  - subtasks
  - bugs
  - incidents
  - releases
- How to estimate work
- How to prioritize work
- How to define done
- How to handle scope changes
- How to track dependencies
- How to document decisions

---

## 8) Output format requirements

### The response must be structured as follows:

1. A short executive summary
2. A list of the generated documentation files
3. The full markdown content for each file
4. A final section with:
   - assumptions made
   - decisions taken
   - open questions
   - recommended next actions

### For each file:

- Start with the filename as a markdown heading
- Use clean markdown formatting
- Make the content ready to copy into a repository or knowledge base

---

## 9) Decision rules

When there are multiple possible answers:

- Prefer **production-ready** over minimal
- Prefer **clear standards** over ambiguity
- Prefer **scalable architecture** over quick hacks
- Prefer **maintainability** over cleverness
- Prefer **documented trade-offs** over hidden assumptions

If there are multiple valid stacks, always provide:

- a recommended default
- at least one alternative
- the reason for the recommendation

---

## 10) Writing style

Write as if you are producing documentation for:

- engineers
- DevOps teams
- project managers
- technical founders
- client delivery teams

Tone:

- professional
- direct
- practical
- precise

---

## 11) Final instruction

Produce the documentation files in markdown with enough depth that the team at **LIONGATE SARL** can use them as a real foundation for:

- hosting strategy
- Odoo deployment
- internal engineering standards
- project management workflow
- production operations

Do not stay theoretical. Make it implementable.

I want to continue creating the remaining proforma files. you need to create the following files in the proformas/ directory

proforma-02-standard-web-application-billing.md
proforma-03-high-traffic-client-project-billing.md
proforma-04-internal-tools-billing.md
proforma-05-infrastructure-devops-shared-tools-billing.md
proforma-06-project-management-tools-billing.md
proforma-00-master-consolidated-billing-overview.md
