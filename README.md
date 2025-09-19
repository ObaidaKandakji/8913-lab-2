# Lab 2 – Cloud Deployment Options for a 3-Tier Web App
**App:** React (UI) + Flask (API) + PostgreSQL (DB)  
**Student:** <Your Name> • **Date:** <Date>

---

## Q1. IaaS Deployment (Diagram + Description)

### Diagram (IaaS)
Users
  │
  ▼
[ Azure Public Load Balancer ]
  │
  ├─> [ VM Scale Set: Linux VMs running Nginx + Gunicorn/Flask ]
  │         (NSG allows 80/443 from LB; 22 via Just-in-Time)
  │
  └─> [ VNet / Subnets ]
            │
            └─> [ VM: PostgreSQL ]
                   (Private IP only, NSG: 5432 from API subnet)

### Components & Rationale (IaaS)
- **Compute:** Azure **Virtual Machines** / **VM Scale Set** host Flask behind Nginx + Gunicorn. (AWS: EC2/ASG; GCP: MIG)
- **Load Balancing:** **Azure Load Balancer** distributes traffic to API VMs. (AWS: ALB/NLB; GCP: GLB)
- **Front End (React):** Served by the API VMs’ Nginx or a separate small VM / Nginx for static files.
- **Database:** **PostgreSQL on a VM** (managed by us); we handle patching, backups, and tuning.
- **Networking & Security:** **VNet/Subnets**, **NSGs**, only 80/443 open from LB; DB subnet private. Use **Key Vault** for secrets, **Azure Monitor/Log Analytics** for logs/metrics.
- **Ops Responsibility:** We manage OS patching, scaling policies, backup scripts, failover runbooks.

---

## Q2. PaaS Deployment (Diagram + Description)

### Diagram (PaaS)
Users
  │
  └─> [ Azure Front Door / CDN ] ──> [ Azure Static Web Apps (React) ]
                          │
                          └────> [ Azure App Service (Flask API) ]
                                       │
                                       └─> [ Azure Database for PostgreSQL (Flexible Server) ]
                                              (Private access / VNet integration)

### Components & Rationale (PaaS)
- **UI (React):** **Azure Static Web Apps** (builds on push, global CDN, HTTPS by default).
- **API (Flask):** **Azure App Service** (or **Azure Container Apps** if containerized) with autoscale, TLS, deployment slots.
- **Database:** **Azure Database for PostgreSQL – Flexible Server** (managed backups, patching, high availability).
- **Networking/Security:** **Private endpoints** or **VNet integration**, **Managed Identity** for secrets, **Key Vault** for connection strings.
- **Observability:** **Azure Monitor**, **Application Insights** for distributed traces and alerts.
- **Ops Responsibility:** Minimal—platform handles OS/runtime, scaling, patches, backups. We focus on code, configs, and cost limits.

---

## Notes on Trade-offs
- **IaaS:** Max control/flexibility; higher ops burden (patching, HA/DR, backups).
- **PaaS:** Faster delivery, built-in reliability/security; less infra control, service limits/quotas.
