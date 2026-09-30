# ☁️ Azure Zero to Mastery

[![Azure](https://img.shields.io/badge/Microsoft_Azure-0089D6?style=for-the-badge&logo=microsoft-azure&logoColor=white)](https://azure.microsoft.com/)
[![Kubernetes](https://img.shields.io/badge/Kubernetes_AKS-326CE5?style=for-the-badge&logo=kubernetes&logoColor=white)](https://azure.microsoft.com/en-us/products/kubernetes-service)
[![Terraform](https://img.shields.io/badge/Terraform_IaC-7B42BC?style=for-the-badge&logo=terraform&logoColor=white)](https://www.terraform.io/)
[![DevOps](https://img.shields.io/badge/DevOps-CI%2FCD-2496ED?style=for-the-badge&logo=azure-devops&logoColor=white)](https://azure.microsoft.com/en-us/products/devops)
[![License](https://img.shields.io/badge/License-MIT-green.svg?style=for-the-badge)](LICENSE)

A structured, practical, and hands-on roadmap designed to take you from foundational cloud concepts to mastering **Microsoft Azure** architecture, platform engineering, and enterprise operations.

---

## 📌 Table of Contents

- [Overview](#-overview)
- [Architecture Overview](#-architecture-overview)
- [Roadmap & Learning Phases](#-roadmap--learning-phases)
- [Repository Structure](#-repository-structure)
- [Who This Is For](#-who-this-is-for)
- [Getting Started](#-getting-started)
- [Author](#-author)

---

## 📖 Overview

This repository documents an end-to-end journey mastering Microsoft Azure, specifically tailored for **Cloud, DevOps, and Platform Engineers**. 

Rather than focusing merely on certification cramming or basic portal clicks, this guide focuses on:
- **Architectural depth:** Understanding resource boundaries, tenant hierarchies, and network routing.
- **Enterprise-grade security:** Managed Identities, Workload Identity, Zero Trust, and Private Endpoints.
- **Production Kubernetes (AKS):** In-depth ingress controllers, CNI networking, CSI storage drivers, and autoscaling.
- **Infrastructure as Code (IaC):** Automated provisioning via Terraform with state locking and secure storage backends.
- **Continuous Delivery & Observability:** GitOps, CI/CD pipelines, KQL querying, and central monitoring.

---

## 🏗️ Architecture Overview

Think of the Azure ecosystem across these core layers:

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

## 🗺️ Roadmap & Learning Phases

The full breakdown with concepts, diagrams, and deep dives is available in **[Day-01: Azure Learning Roadmap](Day-01/Roadmap.md)**.

| Phase | Module | Focus Area & Key Topics | Weight / Priority |
|:---:|:---|:---|:---:|
| **01** | [Azure Fundamentals](Day-01/Roadmap.md#phase-1--azure-fundamentals) | Cloud models (IaaS/PaaS/SaaS), Regions, Availability Zones, HA, DR, CapEx vs. OpEx | ⭐️ |
| **02** | [Azure Structure](Day-01/Roadmap.md#phase-2--azure-structure-) | Tenant, Management Groups, Subscriptions, Resource Groups, and Resources | ⭐️ |
| **03** | [Azure Identity](Day-01/Roadmap.md#phase-3--azure-identity-) | Microsoft Entra ID, Service Principals, Managed Identities, Roles & RBAC scopes | ⭐️⭐️⭐️ |
| **04** | [Azure Networking](Day-01/Roadmap.md#phase-4--azure-networking-) | VNets, Subnets, CIDR, NSGs, Layer 4 Load Balancer, Layer 7 App Gateway, Private Endpoints | ⭐️⭐️⭐️ |
| **05** | [Compute](Day-01/Roadmap.md#phase-5--compute) | VMs, Disks, NICs, Availability Sets, Availability Zones, VMSS | ⭐️⭐️ |
| **06** | [Storage](Day-01/Roadmap.md#phase-6--storage) | Blob Storage, Access tiers (Hot/Cool/Cold/Archive), SAS tokens, LRS/ZRS/GRS/GZRS | ⭐️⭐️ |
| **07** | [Azure Containers](Day-01/Roadmap.md#phase-7--azure-containers-) | Azure Container Registry (ACR), repositories, secure pull, and integration | ⭐️⭐️⭐️ |
| **08** | [AKS (Kubernetes)](Day-01/Roadmap.md#phase-8--azure-kubernetes-service-aks-) | Node pools, Azure CNI vs. Kubenet, Workload Identity, Ingress, AGIC, Private Clusters | ⭐️⭐️⭐️⭐️⭐️ |
| **09** | [Azure Key Vault](Day-01/Roadmap.md#phase-9--azure-key-vault-) | Secrets, Keys, Certificates, Secrets Store CSI Driver integration with AKS | ⭐️⭐️⭐️ |
| **10** | [Azure Databases](Day-01/Roadmap.md#phase-10--azure-databases) | Azure SQL, Flexible PostgreSQL, Cosmos DB (Partitions, Consistency models) | ⭐️⭐️ |
| **11** | [Azure DevOps & CI/CD](Day-01/Roadmap.md#phase-11--azure-devops--cicd) | Azure Repos, YAML Pipelines, Self-hosted runners, OIDC with GitHub Actions | ⭐️⭐️⭐️ |
| **12** | [Infrastructure as Code](Day-01/Roadmap.md#phase-12--infrastructure-as-code-iac-) | Terraform (`azurerm`), Remote State with Blob lease locking, Modules, Bicep overview | ⭐️⭐️⭐️⭐️⭐️ |
| **13** | [Monitoring & Observability](Day-01/Roadmap.md#phase-13--monitoring--observability-) | Azure Monitor, Log Analytics, KQL queries, App Insights, Prometheus/Grafana parity | ⭐️⭐️⭐️⭐️ |
| **14** | [Security & Governance](Day-01/Roadmap.md#phase-14--azure-security--governance) | Microsoft Defender for Cloud, Conditional Access, Azure Policy (Audit/Deny/Remediate) | ⭐️⭐️⭐️⭐️ |

---

## 📁 Repository Structure

```text
azure-zero-to-mastery/
├── README.md               # Repository documentation and index
└── Day-01/
    └── Roadmap.md          # 14-Phase comprehensive Azure learning roadmap
```

*More day-by-day practical exercises, Terraform configurations, and deployment manifests will be added as the series progresses.*

---

## 🎯 Who This Is For

- **DevOps Engineers & SREs:** Looking to gain enterprise-grade Azure knowledge, IaC skills, and CI/CD best practices.
- **Platform Engineers & Kubernetes Administrators:** Transitioning Kubernetes knowledge into production Azure Kubernetes Service (AKS).
- **Cloud Engineers:** Aiming to design resilient, secure, and cost-effective Azure architectures.

---

## 🚀 Getting Started

1. Clone the repository:
   ```bash
   git clone https://github.com/sathish-nalladevagari/azure-zero-to-mastery.git
   cd azure-zero-to-mastery
   ```
2. Start by reviewing the full foundational roadmap in:
   - **[Day-01/Roadmap.md](Day-01/Roadmap.md)**
3. Install the recommended tooling:
   - [Azure CLI (`az`)](https://learn.microsoft.com/en-us/cli/azure/install-azure-cli)
   - [Terraform CLI](https://developer.hashicorp.com/terraform/install)
   - [Kubectl](https://kubernetes.io/docs/tasks/tools/)
   - [Docker](https://docs.docker.com/get-docker/)

---

## 👤 Author

**Sathish Reddy**
- GitHub: [@sathish-nalladevagari](https://github.com/sathish-nalladevagari)
