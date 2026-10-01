# Day 02 — Azure Fundamentals

A comprehensive foundational guide to Microsoft Azure, cloud computing service models, global architecture, resilience, economics, and resource management.

---

## 📌 Table of Contents

- [1. What is Azure?](#1-what-is-azure)
- [2. Why Cloud?](#2-why-cloud)
- [3. Cloud Service Models](#3-cloud-service-models)
  - [IaaS — Infrastructure as a Service](#iaas--infrastructure-as-a-service)
  - [PaaS — Platform as a Service](#paas--platform-as-a-service)
  - [SaaS — Software as a Service](#saas--software-as-a-service)
  - [Comparing IaaS vs. PaaS vs. SaaS](#comparing-iaas-vs-paas-vs-saas)
- [4. Azure Regions](#4-azure-regions)
- [5. Why Regions Matter](#5-why-regions-matter)
- [6. Availability Zones (AZs) ⭐](#6-availability-zones-azs-)
- [7. Region vs. Availability Zone](#7-region-vs-availability-zone)
- [8. High Availability (HA)](#8-high-availability-ha)
- [9. Scalability: Vertical vs. Horizontal](#9-scalability-vertical-vs-horizontal)
- [10. Elasticity](#10-elasticity)
- [11. Fault Tolerance](#11-fault-tolerance)
- [12. Disaster Recovery (DR)](#12-disaster-recovery-dr)
- [13. RPO vs. RTO (Key DevOps & SRE Metrics)](#13-rpo-vs-rto-key-devops--sre-metrics)
- [14. CapEx vs. OpEx](#14-capex-vs-opex)
- [15. Azure Resource Manager (ARM)](#15-azure-resource-manager-arm)
- [16. Azure Management Hierarchy](#16-azure-management-hierarchy)
- [17. Knowledge Check & Summary Checklist](#17-knowledge-check--summary-checklist)

---

## 1. What is Azure?

**Microsoft Azure** is Microsoft's public cloud computing platform. Instead of purchasing, racking, and maintaining physical servers yourself, you rent compute, networking, and storage capacity on-demand over the internet.

### Traditional Infrastructure vs. Azure

**🏢 Traditional On-Premises:**
```text
Your Company
     │
     ├── Physical Servers
     ├── Storage Area Networks (SAN)
     ├── Switches & Routers
     ├── Firewalls
     └── Physical Data Center
```

**☁️ With Azure:**
```text
Your Company
     │
     ↓
   Azure
     │
     ├── Compute
     ├── Storage
     ├── Networking
     ├── Databases
     ├── Security
     └── Monitoring
```

> **DevOps Perspective:** Think of Azure as an elastic, software-defined collection of infrastructure and managed services that you can provision, automate, and manage as code (IaC).

---

## 2. Why Cloud?

Consider what happens when your organization needs a new application server:

### Traditional Approach (Days to Weeks)
- Purchase physical hardware
- Wait for vendor shipment & delivery
- Rack and stack hardware in the datacenter
- Configure physical cabling and network switches
- Install host operating system and patches
- Provision storage arrays
- Configure hardware firewalls and access controls
- Maintain hardware and replace failed power supplies or hard drives

### With Azure (Minutes)
```text
Select VM Size (CPU / RAM)
          ↓
Choose Base OS Image (Linux / Windows)
          ↓
Configure Virtual Networking & Subnet
          ↓
Click Deploy / Run Terraform
          ↓
VM Ready & Accessible in Minutes 🚀
```

---

## 3. Cloud Service Models

Understanding the division of responsibility between you and the cloud provider is essential.

### IaaS — Infrastructure as a Service

Azure delivers virtualized computing resources. You manage the operating system, runtimes, middleware, and application layer.

```text
Azure Cloud
     │
     └── Virtual Machine (VM)
              │
              ├── Operating System (Ubuntu / Windows)
              ├── Container Runtime (Docker / Containerd)
              ├── Orchestration (Kubernetes)
              └── Application Code
```

- **Examples:** Azure Virtual Machines, Virtual Machine Scale Sets (VMSS), Azure Virtual Networks (VNet).
- **Mental Model:** *"Give me raw infrastructure; I will configure and manage the OS and software stack."*

---

### PaaS — Platform as a Service

Azure provides a managed environment for building, deploying, and scaling web apps and APIs without the burden of maintaining VMs or OS patches.

```text
Azure App Service / Azure SQL
              ↓
  Your Application Code & Data
(OS, runtime, and patches managed by Azure)
```

- **Examples:** Azure App Service, Azure Functions (Serverless), Azure SQL Database, Azure Cosmos DB.
- **Mental Model:** *"I want to deploy my application code without managing VMs, runtimes, or OS updates."*

---

### SaaS — Software as a Service

A complete software product managed and operated entirely by the provider. You simply consume the software via web or API.

```text
Microsoft 365 / GitHub / Salesforce
              ↓
       End User / Consumer
```

- **You do NOT manage:** Servers, OS, runtimes, database clustering, or infrastructure code.
- **Mental Model:** *"Just provide the working software; I will use it."*

---

### Comparing IaaS vs. PaaS vs. SaaS

#### Shared Responsibility Matrix

| Layer | Traditional (On-Prem) | IaaS | PaaS | SaaS |
|:---|:---:|:---:|:---:|:---:|
| **Applications** | **YOU** | **YOU** | **YOU** | **Azure** |
| **Data & Access** | **YOU** | **YOU** | **YOU** | **YOU** |
| **Runtime & Framework** | **YOU** | **YOU** | **Azure** | **Azure** |
| **Operating System** | **YOU** | **YOU** | **Azure** | **Azure** |
| **Virtualization** | **YOU** | **Azure** | **Azure** | **Azure** |
| **Compute / Hardware** | **YOU** | **Azure** | **Azure** | **Azure** |
| **Storage & Networking** | **YOU** | **Azure** | **Azure** | **Azure** |
| **Physical Datacenter** | **YOU** | **Azure** | **Azure** | **Azure** |

#### Quick Rule of Thumb:
```text
                    Management Effort
                           ↓
IaaS   ──▶ Manage OS, runtime, and applications
PaaS   ──▶ Manage only application code and data
SaaS   ──▶ Manage only user access and configuration
```

---

## 4. Azure Regions

Azure infrastructure is globally distributed across dozens of **Regions**. A region is a geographical area containing one or more datacenters networked together through a dedicated low-latency network.

```text
Azure Global Infrastructure
│
├── East US (Virginia)
├── West Europe (Netherlands)
├── Central India (Pune)
├── Southeast Asia (Singapore)
└── Australia East (New South Wales)
```

When provisioning services, you choose the region where your resources will reside:

```text
Application
     ↓
Target Region: West Europe
     ├── AKS Cluster
     ├── PostgreSQL Database
     └── Azure Storage Account
```

---

## 5. Why Regions Matter

Choosing the right region is an architectural decision based on four primary factors:

### 1. Latency (User Experience)
Deploy resources close to your user base to minimize round-trip network time:
```text
User in India ──▶ Azure Central India Region  ──▶  Low Latency  ⚡
User in India ──▶ Azure East US Region        ──▶  High Latency 🐢
```

### 2. Data Residency & Compliance
Many governments and regulated industries (e.g., GDPR in the EU, HIPAA in the US, RBI in India) require customer data to remain within specific sovereign geographical borders.

### 3. Service Availability
Not all Azure services, SKUs, or VM sizes are available in every region simultaneously. Newer services and specialized GPU series often roll out in flagship regions first.

### 4. Cost Variation
Pricing differs between regions due to local datacenter construction costs, electricity rates, and local taxation.

---

## 6. Availability Zones (AZs) ⭐

> **Critical Architecture Concept:** Availability Zones protect applications and data from datacenter-level failures.

An **Availability Zone** is a physically separate datacenter within an Azure region. Each zone has **independent power, cooling, and networking**.

```text
                     Azure Region
                          │
       ┌──────────────────┼──────────────────┐
       ↓                  ↓                  ↓
 Availability       Availability       Availability
    Zone 1             Zone 2             Zone 3
       │                  │                  │
 ┌───────────┐      ┌───────────┐      ┌───────────┐
 │Datacenter │      │Datacenter │      │Datacenter │
 │Power/Cool │      │Power/Cool │      │Power/Cool │
 └───────────┘      └───────────┘      └───────────┘
```

If one datacenter experiences a flood, power outage, or hardware disruption, workloads hosted in the remaining zones continue running without downtime.

---

## 7. Region vs. Availability Zone

```text
Azure Region
  ├── Availability Zone 1  ──▶ [App Instance A]
  ├── Availability Zone 2  ──▶ [App Instance B]
  └── Availability Zone 3  ──▶ [App Instance C]
```

- **Region:** A geographical boundary (e.g., `East US`, `West Europe`).
- **Availability Zone:** Distinct physical locations *inside* a region connected by high-performance fiber networks (<2ms latency).

---

## 8. High Availability (HA)

**High Availability** ensures systems remain operational and accessible with minimal or zero downtime during hardware or network failures.

### Single Point of Failure (Anti-Pattern)
```text
Client ──▶ Application (Single VM)
                │
                └── ❌ VM Crashes ──▶ Entire Application Offline
```

### High Availability Architecture (Best Practice)
Deploy multiple instances behind an Azure Load Balancer across multiple Availability Zones:

```text
                    Azure Load Balancer
                     /       |       \
                    /        |        \
                   ↓         ↓         ↓
                 VM 1      VM 2      VM 3
               (Zone 1)  (Zone 2)  (Zone 3)
```

If `VM 2` experiences a hardware failure:
```text
                    Azure Load Balancer
                     /               \
                    /                 \
                   ↓                   ↓
                 VM 1                VM 3
              (Healthy)           (Healthy)
```
The load balancer automatically redirects traffic to healthy instances with zero downtime to end users.

---

## 9. Scalability: Vertical vs. Horizontal

**Scalability** is the ability of an application or infrastructure to handle increasing workload volume.

```text
Vertical Scaling (Scale Up)          Horizontal Scaling (Scale Out)

     ┌─────────┐                          ┌───┐ ┌───┐ ┌───┐ ┌───┐
     │ 2 vCPU  │                          │VM │ │VM │ │VM │ │VM │
     │ 8 GB RAM│                          └───┘ └───┘ └───┘ └───┘
          │                                  ▲     ▲     ▲     ▲
          ▼                                  └─────┴─────┴─────┘
     ┌─────────┐                          Add more instances behind
     │ 8 vCPU  │                          a load balancer / VMSS
     │ 32 GB   │
     └─────────┘
  Increase capacity of a
     single instance
```

- **Vertical Scaling (Scale Up/Down):** Resize an existing machine to more/less powerful hardware. Has hard physical ceilings and usually requires a restart.
- **Horizontal Scaling (Scale Out/In):** Add or remove instances (e.g., in a Virtual Machine Scale Set or AKS cluster). **Crucial for modern cloud-native architectures.**

---

## 10. Elasticity

**Elasticity** is the capability to *automatically* scale computing resources up or down in real time to match immediate demand.

```text
    Traffic Volume                   Active VM Count
    
    Morning Peak (10,000 req/s) ────▶ Auto-scales to 10 Instances
               │
               ▼
    Night Lull (50 req/s)       ────▶ Scales in to 2 Instances
```

> **Benefit:** Elasticity eliminates the cost of over-provisioning 24/7 for peak capacity while protecting performance during sudden traffic surges.

---

## 11. Fault Tolerance

**Fault tolerance** means the system is designed to seamlessly absorb component failures without disrupting user operations.

```text
                        Application Service
                       /         |         \
                      ↓          ↓          ↓
                    VM 1       VM 2       VM 3
                     ✅         ❌         ✅
                               Fault
```
*Even when VM 2 suffers a critical error, the system continues functioning correctly without dropping transactions.*

---

## 12. Disaster Recovery (DR)

Disaster Recovery covers strategies and procedures for recovering infrastructure, applications, and data after catastrophic events (regional network outages, natural disasters, or ransomware).

```text
Primary Region (East US)               Secondary Region (West US)
        │                                          │
   Application                                Application (Standby)
   Database ─── Cross-Region Data Replication ──▶ Database
   Storage  ─── (Async / GRS Backup)        ──▶ Storage
```

### Common Triggers for DR
- Regional cloud outage
- Catastrophic storage corruption
- Accidental resource deletion
- Malicious security breaches

---

## 13. RPO vs. RTO (Key DevOps & SRE Metrics)

These two metrics define an organization's Business Continuity and Disaster Recovery (BCDR) service level agreements:

```text
                 Incident Occurs
                       │
◄── Data Loss ─────────┼────────── Downtime ─────────►
[Last Valid Backup]    │                     [System Restored]
                       │
◄─────── RPO ─────────►│◄──────── RTO ───────────────►
```

| Metric | Stands For | Core Question | Focus Area |
|:---|:---|:---|:---|
| **RPO** | **Recovery Point Objective** | *"How much data can we afford to lose?"* | Data freshness, replication frequency, backup schedule |
| **RTO** | **Recovery Time Objective** | *"How quickly must service be restored?"* | Downtime duration, failover automation, MTTR |

### Example Scenario
- **Database SLA:** RPO = 15 minutes, RTO = 30 minutes.
- **Meaning:** In the event of a total failure, you must not lose more than 15 minutes of transactional data, and the application must be back online within 30 minutes.

---

## 14. CapEx vs. OpEx

Cloud computing transforms corporate IT financial models from capital investment to operational agility:

| Attribute | CapEx (Capital Expenditure) | OpEx (Operational Expenditure) |
|:---|:---|:---|
| **Model** | Traditional On-Premises | Public Cloud (Azure) |
| **Cost Timing** | Large upfront capital investment | Ongoing operational payment |
| **Purchases** | Physical servers, storage arrays, datacenters | Pay-as-you-go cloud services |
| **Depreciation** | Assets depreciate over 3–5 years | Deducted as an operational expense immediately |
| **Flexibility** | Rigid; unused hardware incurs sunk costs | Highly agile; turn off resources to stop paying |

---

## 15. Azure Resource Manager (ARM)

**Azure Resource Manager (ARM)** is the unified deployment, management, and governance layer for all Azure resources.

```text
 Azure Portal     Azure CLI     Azure PowerShell     Terraform     Bicep / ARM Templates
       │              │                │                 │                   │
       └──────────────┴────────┬───────┴─────────────────┴───────────────────┘
                               │
                               ▼
               Azure Resource Manager (ARM API)
                               │
           ┌───────────────────┼───────────────────┐
           ↓                   ↓                   ↓
    Virtual Networks    Virtual Machines    Storage Accounts
```

### Key ARM Features
- **Consistent API:** Whether using the Portal, CLI, Terraform, or REST APIs, all calls go through ARM.
- **Declarative Templates:** Define infrastructure declaratively (JSON/Bicep).
- **Access Control:** RBAC permissions are enforced at the ARM layer.
- **Tagging:** Organize and track costs across departments using resource tags.

---

## 16. Azure Management Hierarchy

To manage access, policies, and billing effectively, Azure organizes resources into a 4-tier hierarchy:

```text
Microsoft Entra Tenant       (Identity Boundary)
        │
        └── Management Groups  (Governance & Policy across Subscriptions)
                │
                └── Subscriptions     (Billing & Resource Boundary)
                        │
                        └── Resource Groups  (Logical Lifecycle Container)
                                │
                                └── Resources       (VMs, Storage, AKS, DBs)
```

### Practical Enterprise Example
```text
Company Root Management Group
│
├── Production Subscription
│     │
│     ├── rg-prod-network
│     │     └── VNet, NSG, Firewalls
│     │
│     └── rg-prod-aks
│           ├── AKS Cluster
│           ├── Key Vault
│           └── Azure Container Registry
│
└── Development Subscription
      │
      └── rg-dev-sandbox
            ├── Sandbox AKS
            └── Storage Account
```

---

## 17. Knowledge Check & Summary Checklist

### Core Mental Model
```text
                             AZURE
                               │
               ┌───────────────┴───────────────┐
               │                               │
           Geography                        Services
               │                               │
            Region                     ┌───────┼───────┐
               │                       │       │       │
       Availability Zones           Compute Storage Network
               │
               ▼
      Resources & Workloads
```

### Checklist: Can You Explain These Concepts?
- [ ] What is Azure and how does it differ from traditional on-prem datacenters?
- [ ] What are the differences between **IaaS**, **PaaS**, and **SaaS**?
- [ ] What is an **Azure Region** and what factors dictate region choice?
- [ ] What is an **Availability Zone** and how does it enable High Availability?
- [ ] What is the difference between **Vertical** and **Horizontal** scaling?
- [ ] What is **Elasticity** vs. **Scalability**?
- [ ] What do **RPO** and **RTO** measure in Disaster Recovery planning?
- [ ] How does **OpEx** differ from **CapEx**?
- [ ] What is the role of **Azure Resource Manager (ARM)**?
- [ ] What is the 4-level **Azure Management Hierarchy**?

---

> **Next Step:** Proceed to **Management Hierarchy + Microsoft Entra ID + RBAC**. These identity and access concepts form the security backbone for everything deployed in Azure (AKS, Terraform, Key Vault, and CI/CD pipelines).