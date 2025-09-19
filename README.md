# Lab 2 – Cloud Deployment Options for a 3-Tier Web App
**Application:** React (UI) + Flask (API) + PostgreSQL (DB)  
**Student:** <Your Name>  
**Date:** <Date>  

---

## Q1. IaaS Deployment (Azure Virtual Machines)

### Diagram
+-------------------------+
| Users (Browser/Client)  |
+-------------------------+
            │
            ▼
+-------------------------+
| Azure Load Balancer     |
+-------------------------+
            │
            ▼
+-------------------------+
| VM Scale Set (Flask API)|
|   + React (static)      |
+-------------------------+
            │
            ▼
+-------------------------+
| VM: PostgreSQL Database |
+-------------------------+

### Description
- **Compute:** Use **Azure Virtual Machines** in a **VM Scale Set** for Flask. We install Nginx + Gunicorn manually.  
- **Load Balancing:** **Azure Load Balancer** distributes web/API traffic across VMs.  
- **React Front End:** Static files served by Nginx on the same VMs or a separate small VM.  
- **Database:** **PostgreSQL** runs on a dedicated Azure VM in a private subnet.  
- **Networking & Security:** **VNets/Subnets** isolate layers. **NSGs** restrict ports: only 80/443 open to LB; DB only accepts private traffic.  
- **Ops Responsibility:** We patch OS, configure scaling, manage DB backups, and handle monitoring (Azure Monitor/Log Analytics).  

---

## Q2. PaaS Deployment (Azure Managed Services)

+-------------------------+
| Users (Browser/Client)  |
+-------------------------+
            │
            ▼
+-------------------------------+
| Azure Front Door / CDN        |
+-------------------------------+
   │                     │
   ▼                     ▼
+-----------------+   +-------------------------+
| Static Web App  |   | Azure App Service (API) |
| (React)         |   | Flask runtime managed   |
+-----------------+   +-------------------------+
                             │
                             ▼
                   +------------------------------+
                   | Azure Database for PostgreSQL|
                   | (Fully managed service)      |
                   +------------------------------+  

### Description
- **UI (React):** Deployed to **Azure Static Web Apps** — global CDN, free SSL, auto-build from GitHub.  
- **API (Flask):** Hosted on **Azure App Service** with autoscaling, deployment slots, built-in TLS.  
- **Database:** Use **Azure Database for PostgreSQL – Flexible Server** with automated backups and HA.  
- **Networking & Security:** Use **Private Endpoints** and **VNet integration** to secure traffic. **Managed Identity** + **Azure Key Vault** for secrets.  
- **Monitoring:** **Azure Monitor** + **Application Insights** for telemetry and performance tracking.  
- **Ops Responsibility:** Minimal — Azure manages OS, runtime, scaling, patching, and backups. We focus on app code and configs.  

---

## Trade-Offs
- **IaaS:** Maximum control (custom configs, OS choice) but higher ops burden (patching, HA, backup scripts).  
- **PaaS:** Simplifies deployment and operations (built-in scaling, monitoring, SSL, HA), but less control over infrastructure details.  
