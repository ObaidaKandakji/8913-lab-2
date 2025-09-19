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
 

---

## Trade-Offs
- **IaaS:** Maximum control (custom configs, OS choice) but higher ops burden (patching, HA, backup scripts).  
- **PaaS:** Simplifies deployment and operations (built-in scaling, monitoring, SSL, HA), but less control over infrastructure details.  
