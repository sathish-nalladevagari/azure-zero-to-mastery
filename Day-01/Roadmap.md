# Azure Learning Roadmap

Think of Azure in these layers:

```text
                    AZURE
                      │
        ┌─────────────┴─────────────┐
        │                           │
   FUNDAMENTALS                 MANAGEMENT
        │                           │
 Subscription                 Azure Portal
 Resource Groups              Azure CLI
 Regions/Zones                Azure PowerShell
 RBAC                         ARM / Bicep
        │
        ├──────── COMPUTE ────────┐
        │                         │
       VM                       AKS
       VMSS                     Container Apps
       App Service              Functions
        │                         │
        ├──────── NETWORKING ─────┤
        │                         │
       VNet                     NSG
       Subnet                   Load Balancer
       Private Endpoint         Application Gateway
       DNS                      VPN / ExpressRoute
        │
        ├──────── STORAGE ────────┤
        │                         │
      Blob                      Files
      Disk                      Queue
      Table
        │
        ├──────── DATABASE ───────┤
        │                         │
     Azure SQL                 PostgreSQL
     Cosmos DB
        │
        ├──────── SECURITY ───────┤
        │                         │
       Entra ID                Key Vault
       RBAC                    Managed Identity
       Defender
        │
        ├──────── DEVOPS ─────────┤
        │                         │
     Azure DevOps              GitHub
     Pipelines                 ACR
     Artifacts                 Terraform
        │
        └────── MONITORING ───────┘
                                  │
                             Azure Monitor
                             Log Analytics
                             Application Insights
```

---

## Phase 1 — Azure Fundamentals

> **Note:** Start here before touching AKS. You probably already know most of these from DevOps, so don't spend too long here.

### 1. Cloud Fundamentals
Understand the core cloud concepts:
- **Cloud Models:** IaaS, PaaS, SaaS
- **Deployment Models:** Public, Private, Hybrid Cloud
- **Global Infrastructure:**
  - Regions
  - Availability Zones
  - Region pairs
- **Architecture Principles:**
  - Scalability vs. Elasticity
  - High Availability (HA)
  - Fault tolerance
  - Disaster Recovery (DR)
  - **RPO** (Recovery Point Objective) & **RTO** (Recovery Time Objective)
- **Economics:** CapEx vs. OpEx

---

## Phase 2 — Azure Structure ⭐

> **Important:** This is very fundamental and critical for architecture.

### The Azure Hierarchy

```text
Microsoft Entra Tenant
        │
        └── Management Groups
                │
                └── Subscriptions
                        │
                        └── Resource Groups
                                │
                                └── Resources
```

### Core Hierarchy Components

#### 1. Tenant
- Your organization's Azure identity boundary (Microsoft Entra ID).

#### 2. Management Group
- Used to organize and govern multiple subscriptions with common policies and RBAC.

```text
Company
│
├── Production Subscription
├── Development Subscription
└── Testing Subscription
```

#### 3. Subscription
- Billing and access/resource management boundary.

#### 4. Resource Group
- Logical container for resources deployed within an Azure solution.

```text
rg-prod-aks
│
├── AKS
├── Load Balancer
├── Public IP
├── Managed Identity
└── Key Vault
```

#### 5. Resource
- The actual instance of an Azure service.
- **Examples:** Virtual Machines (VM), Storage Accounts, AKS Clusters, Key Vaults, VNets, Databases.

---

## Phase 3 — Azure Identity ⭐⭐⭐

> **Focus:** For DevOps / Platform Engineering, learn Microsoft Entra ID very well.  
> *(Note: Microsoft renamed **Azure AD** → **Microsoft Entra ID**)*

### Key Identity Objects & Concepts
- **User**
- **Group**
- **Service Principal**
- **Managed Identity** (System-Assigned & User-Assigned)
- **Application Registration**
- **Enterprise Application**
- **Role-Based Access Control (RBAC)**

### Authentication vs. Authorization

```text
Authentication  ──▶  "Who are you?"
Authorization   ──▶  "What are you allowed to do?"
```

### Azure RBAC Model

```text
Principal ──▶ Role ──▶ Scope
```

**Example:**
```text
User ──▶ Contributor ──▶ Resource Group
```

#### Important Built-in Roles
- `Owner`: Full access to all resources including the right to delegate access to others.
- `Contributor`: Can create and manage all types of Azure resources, but cannot grant access to others.
- `Reader`: Can view existing Azure resources.
- `User Access Administrator`: Manages user access to Azure resources.

#### Scope Hierarchy
```text
Management Group
      ↓
Subscription
      ↓
Resource Group
      ↓
Resource
```

---

## Phase 4 — Azure Networking ⭐⭐⭐

> **Focus:** This should be one of your strongest areas.

### 1. VNet & Subnets
```text
VNet
│
├── Subnet A
├── Subnet B
└── Subnet C
```

### 2. CIDR & IP Addressing
Understand subnet sizing and IP allocation:
- `10.0.0.0/16`
- `10.0.1.0/24`
- `10.0.2.0/24`
- *You should be comfortable calculating subnet ranges and reserved IPs (Azure reserves 5 IPs per subnet).*

### 3. NSG (Network Security Group)
Basic packet filtering firewall:
```text
NSG
 │
 ├── Priority (100 - 4096)
 ├── Source (IP / Service Tag / ASG)
 ├── Destination
 ├── Port (Source & Destination)
 ├── Protocol (TCP / UDP / ICMP / Any)
 └── Action (Allow / Deny)
```

### 4. Public vs. Private IP
- Differences, public IP SKUs (Standard vs. Basic), and assignment methods (Static vs. Dynamic).

### 5. Azure Load Balancer
- **Layer 4** (TCP/UDP) Load Balancer.
- High throughput, ultra-low latency.

```text
Client
  ↓
Load Balancer
  ↓
VM1  VM2  VM3
```

### 6. Application Gateway
- **Layer 7** (HTTP/HTTPS) Load Balancer & Reverse Proxy.
- **Key Features:**
  - HTTP / HTTPS routing
  - URL / Path-based routing
  - TLS / SSL termination
  - **WAF** (Web Application Firewall)
  - Backend pools
  - Health probes

### 7. Private Endpoint ⭐
> **Crucial:** Very important for enterprise Azure security.

Connects private virtual networks securely to Azure PaaS services without traversing the public internet.

```text
AKS
 │
 │ Private Network (Private IP)
 ↓
Private Endpoint
 │
 ↓
Azure Key Vault
```

### 8. Private DNS
- Understand how private endpoints resolve DNS names internally using Azure Private DNS Zones.

### 9. VPN Gateway
- **Site-to-Site (S2S) VPN:** Connects on-premises network to Azure VNet.
- **Point-to-Site (P2S) VPN:** Connects individual developer/admin devices to Azure VNet.

### 10. ExpressRoute
- Dedicated, private, high-speed fiber connection from on-premises datacenter to Azure (bypassing the public internet).

---

## Phase 5 — Compute

### Virtual Machines (VM)
Understand:
- **VM Sizes & Families:** General Purpose, Compute Optimized, Memory Optimized, etc.
- **Images:** Marketplace images, custom images, Azure Compute Gallery.
- **Disks:** OS Disks, Data Disks, Managed Disks (Standard HDD, Standard SSD, Premium SSD, Ultra Disk).
- **Networking:** NIC, Public IP, NSG associations.
- **Resiliency:**
  - **Availability Sets:** Fault domains & Update domains.
  - **Availability Zones:** Physically separate datacenters within a region.
  - **Virtual Machine Scale Sets (VMSS):** Auto-scaling pools of identical VMs.

```text
VNet
 │
 └── Subnet
       │
       ├── VM 1
       ├── VM 2
       └── VMSS
```

---

## Phase 6 — Storage

### Storage Account Services
```text
Storage Account
│
├── Blob (Object storage)
├── File (SMB / NFS shares)
├── Queue (Simple messaging)
└── Table (NoSQL key-value)
```

### Blob Storage Deep Dive
Focus heavily on Blob Storage:
- **Hierarchical Structure:** Storage Account ➔ Containers ➔ Blobs
- **Access Tiers:**
  - **Hot:** Frequently accessed data
  - **Cool:** Infrequently accessed data (minimum 30 days)
  - **Cold:** Rarely accessed data (minimum 90 days)
  - **Archive:** Offline long-term storage (hours retrieval time)
- **Security & Access:**
  - Shared Access Signatures (**SAS**)
  - Account Storage Keys
  - Microsoft Entra ID (RBAC)
- **Data Management:**
  - Lifecycle management policies
  - Blob versioning & Soft delete

### Redundancy & Replication
Know why you'd choose each, not just the names:
- **LRS (Locally Redundant Storage):** 3 copies within a single datacenter in the primary region.
- **ZRS (Zone-Redundant Storage):** 3 copies across 3 availability zones in the primary region.
- **GRS (Geo-Redundant Storage):** LRS in primary region + LRS in a secondary paired region.
- **GZRS (Geo-Zone-Redundant Storage):** ZRS in primary region + LRS in secondary region.

---

## Phase 7 — Azure Containers ⭐⭐⭐

> **Focus:** This is where your existing DevOps knowledge becomes useful.

### Azure Container Registry (ACR)
```text
Developer ──▶ Docker Build ──▶ ACR (Image Registry) ──▶ AKS (Deployment)
```

Understand:
- Repositories & Image Tags
- Authentication (Admin user vs. Service Principal vs. Managed Identity `AcrPull`)
- Image pull secrets and seamless AKS integration
- Private registries and network access rules (Private Endpoints)

---

## Phase 8 — Azure Kubernetes Service (AKS) ⭐⭐⭐⭐⭐

> **Career Focus:** For your career and platform engineering profile, make AKS one of your biggest Azure topics. You already know Kubernetes, so focus on what Azure adds to Kubernetes.

### AKS Architecture

```text
                Azure
                  │
             AKS Cluster
                  │
       ┌──────────┴──────────┐
       │                     │
 Control Plane            Nodes
  (Microsoft-Managed)   (Customer-Managed)
                           │
                 ┌─────────┼─────────┐
                 │         │         │
                Pod       Pod       Pod
```

### AKS Core Topics to Master
- **Architecture:**
  - AKS Managed Control Plane (API server, etcd)
  - Node Pools: **System node pool** (critical system pods) vs. **User node pools** (application workloads)
  - VM sizes & OS types (Ubuntu, Azure Linux)
- **Scaling:**
  - Cluster Autoscaler (node-level)
  - Horizontal Pod Autoscaler (HPA - pod-level)
  - KEDA (Kubernetes Event-driven Autoscaling)
- **Networking:**
  - **Azure CNI** vs. **Kubenet**
  - Network policies (Azure Network Policy, Calico)
- **Identity & Security:**
  - Managed Identity for cluster infrastructure
  - **Azure AD Workload Identity** for pods to access Azure resources without secrets
- **Storage:**
  - Container Storage Interface (CSI) Drivers
  - Azure Disk (Block storage - ReadWriteOnce)
  - Azure Files (File shares - ReadWriteMany)
- **Ingress & Routing:**
  - Ingress Controllers (NGINX, Traefik)
  - Application Gateway Ingress Controller (**AGIC**)
- **Enterprise AKS:**
  - **Private AKS Cluster:** API server accessible only via private network
  - Azure Monitor Container Insights
  - Azure Key Vault Provider for Secrets Store CSI Driver

> **Tip:** Since you already work with AKS, these should become interview-level architectural concepts, not just hands-on commands.

---

## Phase 9 — Azure Key Vault ⭐⭐⭐

### Key Vault Contents
```text
Key Vault
│
├── Secrets (Passwords, API tokens, connection strings)
├── Keys (Encryption keys for HSM / Cryptographic operations)
└── Certificates (X.509 SSL/TLS certificates with auto-renewal)
```

### Zero-Secret Access Pattern
```text
Application ──▶ Managed Identity ──▶ Azure RBAC ──▶ Key Vault ──▶ Secret
```

### AKS Integration
- **Key Vault CSI Driver + AKS:**
  - Mounts secrets, keys, and certificates directly into pods as native Kubernetes volumes.
  - Automatically synchronizes with Kubernetes secrets if needed.
  - Particularly critical for Platform Engineering.

---

## Phase 10 — Azure Databases

> **Note:** Don't go extremely deep initially—focus on architectural understanding.

### 1. Azure SQL
- **Deployment options:** Single Database, Elastic Pool, Managed Instance.
- **Key Concepts:** Server, Firewall rules, Private Endpoint, Automated backups, High Availability (HA).

### 2. Azure Database for PostgreSQL
- **Flexible Server:** Architecture, custom maintenance windows, zone redundancy.
- **Key Concepts:** Private access (VNet integration), Backups, High Availability.

### 3. Azure Cosmos DB
- Globally distributed, multi-model NoSQL database service.
- **Key Concepts:**
  - Partition Key selection & Request Units (RUs)
  - Collections / Containers
  - Global replication & multi-region writes
  - Consistency levels (Strong, Bounded Staleness, Session, Consistent Prefix, Eventual)

---

## Phase 11 — Azure DevOps & CI/CD

> **Focus:** Since you're targeting DevOps/Platform roles:

### 1. Azure Repos
- Hosted Git repositories, branch policies, PR workflows.

### 2. Azure Pipelines ⭐⭐⭐
CI/CD workflow:
```text
Git Push ──▶ Pipeline Trigger ──▶ Build ──▶ Test ──▶ Docker Build ──▶ Push to ACR ──▶ Deploy to AKS
```

- **Core Pipeline Concepts:**
  - YAML pipelines
  - Stages, Jobs, Steps
  - Microsoft-hosted vs. Self-hosted agents
  - Variables & Variable Groups (Library)
  - Azure Key Vault secrets integration
  - Environments, Approvals, and Deployment Checks
  - Deployment strategies (Rolling, Canary, Blue-Green)

### 3. GitHub Actions + Azure
- Many modern organizations prefer GitHub Actions over Azure Repos/Pipelines.
- Understand authentication via **OpenID Connect (OIDC)** with Azure Workload Identity (federated credentials - no long-lived client secrets).

---

## Phase 12 — Infrastructure as Code (IaC) ⭐⭐⭐⭐⭐

### 1. Terraform (Primary Focus)
```text
Terraform Code ──▶ azurerm Provider ──▶ Azure Resource Manager ──▶ Azure Infrastructure
```

- **Key Concepts to Master:**
  - Providers & Resources
  - Variables, Outputs, and Locals
  - Reusable Modules
  - State management, Remote state, and State locking
  - Terraform lifecycle: `init`, `plan`, `apply`, `destroy`
  - Workspaces
  - **Azure Storage Backend:** Secure state storage in Azure Blob Storage with blob lease locking

### 2. Bicep (Azure-Native IaC)
- Understand Bicep at least conceptually:
  - **Terraform:** Cross-cloud, stateful IaC
  - **Bicep:** Azure-native, declarative, stateless (ARM-native) IaC

---

## Phase 13 — Monitoring & Observability ⭐⭐⭐⭐

### Azure Monitor Ecosystem
The central monitoring and observability platform for Azure.

```text
Azure Monitor
      │
      ├── Metrics (Numerical time-series data)
      ├── Logs (Structured event & telemetry data)
      ├── Alerts (Metric & Log-based alerts)
      └── Workbooks (Interactive visualization dashboards)
```

### Log Analytics & KQL
- Querying telemetry data using **Kusto Query Language (KQL)**.
- **Essential KQL Operators:**
  - `where`: Filter records
  - `project`: Select specific columns
  - `summarize`: Aggregate calculations (e.g., `count()`, `avg()`)
  - `extend`: Create calculated columns
  - `join`: Combine datasets

### Key Monitoring Components
- **Metrics & Logs:** Collection and retention.
- **Log Analytics Workspace:** Central repository for log data.
- **Application Insights:** Application Performance Monitoring (APM) for distributed tracing, requests, dependencies, and exceptions.
- **Diagnostic Settings:** Route resource logs and metrics to Log Analytics, Event Hub, or Storage Account.

### Connecting to Cloud Native Tooling
Translate Azure Monitor concepts to standard DevOps tooling:
- Azure Monitor Metrics ⟷ **Prometheus**
- Azure Workbooks ⟷ **Grafana**
- Log Analytics ⟷ **Loki** / Elastic
- Application Insights ⟷ **OpenTelemetry (OTel)**

---

## Phase 14 — Azure Security & Governance

### 1. Microsoft Defender for Cloud
- **Cloud Security Posture Management (CSPM):** Secure Score and security recommendations.
- **Cloud Workload Protection (CWPP):** Threat detection and protection for VMs, containers, databases, and storage.

### 2. Microsoft Entra ID Security
- Conditional Access policies
- Multi-Factor Authentication (MFA)
- Service Principals & Managed Identities
- Workload Identity Federation

### 3. Azure Policy (Crucial for Platform Engineering)
Enforce organizational standards and assess compliance at scale.

```text
Company Security Standard
        ↓
"All storage accounts must use private access / disable public access"
        ↓
Azure Policy Definition & Assignment
        ↓
Non-compliant resources detected (Audit / Deny / Remediate)
```

- **Key Concepts:**
  - Policy Definitions (rules & conditions)
  - Initiatives (Policy Sets - grouping related policies)
  - Policy Assignments (scoped to Management Group, Subscription, or Resource Group)
  - Effects: `Audit`, `Deny`, `Modify`, `DeployIfNotExists`
  - Compliance evaluation & remediation tasks