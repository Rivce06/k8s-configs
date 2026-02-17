# 🌐 Kubernetes Platform Config (GitOps)

This repository contains the **GitOps configuration layer** for the multi-cloud SRE platform.

It defines the Kubernetes platform components deployed via **Argo CD**, including security policies, secrets management, observability, and the SRE automation agent.

----------

## 🎯 Purpose

This repo represents the **Platform Layer** in a GitOps architecture:

-   Deploy platform services
    
-   Enforce security policies
    
-   Manage secrets securely
    
-   Enable observability & automation
    
-   Integrate the SRE Agent
    

It works alongside:

-   **Repo 1 (Infrastructure)** → Terraform + Terragrunt
    
-   **Repo 3 (Application)** → sre-agent source code
    

----------

## 🧭 Architecture Overview

```
Repo 1 → provisions GKE & cloud resources
Repo 2 → defines platform & policies (this repo)
Repo 3 → application workloads (sre-agent)
```

Deployment flow:

1.  ArgoCD bootstraps platform components
    
2.  Platform services become operational
    
3.  sre-agent is deployed and integrated
    
4.  Policies & security controls enforce compliance
    

----------

## 📦 Platform Components

### 🔐 Secrets Management

-   HashiCorp Vault
    
-   Sidecar injection (memory-only secrets)
    
-   Kubernetes auth method
    
-   Zero-trust secret delivery
    

👉 Secrets never stored in etcd.

----------

### 🛡 Policy Enforcement

-   Kyverno
    
-   Pod security controls
    
-   Resource limit enforcement
    
-   Image & privilege restrictions
    

----------

### 📊 Observability

-   Prometheus metrics collection
    
-   Alerting integration ready
    
-   sre-agent metrics ingestion
    

----------

### 🤖 SRE Automation

-   sre-agent deployment
    
-   Vault integration
    
-   Metrics & alert ingestion ready
    

----------

## 🚀 GitOps Structure

```
bootstrap/
  root-app.yaml         # App-of-Apps bootstrap

platform/
  kyverno/
  vault/
  monitoring/
  sre-agent/
```

The **App-of-Apps** pattern enables centralized lifecycle management.

----------

## 🔐 Zero-Trust Secret Delivery

The Vault Sidecar Injector:

✔ injects secrets in-memory  
✔ never writes secrets to disk  
✔ prevents exposure via etcd  
✔ enables short-lived credentials

👉 This is **Zero Trust by design**.

----------

## 🧪 Lab Environment vs 🏭 Production

This repository is optimized for **low-cost lab environments**, but designed with production hardening in mind.

### Current Lab Configuration

-   Ephemeral storage
    
-   Audit-mode policies
    
-   Minimal resource footprint
    
-   Local or small clusters
    

----------

## 🏭 Production Hardening (Roadmap)

### 🔐 Vault Production Architecture

Production Vault clusters should enable:

-   TLS enabled
    
-   HA mode
    
-   Rafts storage backend
    
-   Auto-unseal via GCP KMS
    
-   Workload Identity authentication
    

Production clusters require persistent storage and long-term retention.

> NOTE: In production, `tlsDisable` must be **false**.

Reference:  
[https://developer.hashicorp.com/vault/docs/concepts/seal#gcpckms](https://developer.hashicorp.com/vault/docs/concepts/seal#gcpckms)

----------

### 📦 Storage & Retention

Production deployments require:

-   PersistentVolumeClaims (e.g., `pd-balanced`)
    
-   15+ days retention
    
-   backup strategy
    

> Current configuration is optimized for ephemeral lab costs.

----------

## 🔄 Lab vs Production Roadmap

Component

Lab Implementation

Production Roadmap

Secret Mgmt

Vault (Standalone)

Vault HA + Rafts + Cloud KMS Unseal

Security

Kyverno (Audit)

Kyverno (Enforce) + Gatekeeper

Observability

Prometheus (Ephemeral)

Prometheus + Thnos/Cortex + PV

GitOps

App-of-Apps

ApplicationSets (Multi-cluster)

Networking

Authorized Networks

Private GKE + Cloud NAT + mTLS (Istio)

----------

## 🌐 Production Kubernetes Security

Production clusters should implement:

✔ TLS everywhere  
✔ private control plane  
✔ Workload Identity  
✔ network isolation  
✔ mutual TLS (service mesh)  
✔ audit logging

----------

## ⚙️ Deployment

### Bootstrap ArgoCD

```
kubectl apply -f bootstrap/root-app.yaml
```

ArgoCD will deploy the platform components automatically.

----------

## 🔭 Future Enhancements

-   AI-assisted incident analysis
    
-   automated remediation workflows
    
-   multi-cluster GitOps
    
-   policy-as-code pipelines
    
-   security posture scanning
    

----------

## 🧠 Design Principles

✔ GitOps-driven operations  
✔ Zero-trust security model  
✔ declarative platform management  
✔ cloud-native portability  
✔ production-first architecture

----------

## 📌 Status

✔ Lab environment validated  
✔ GitOps flow operational  
✔ Security & policy framework active  
✔ Ready for GKE production deployment

----------

## 📄 License

MIT
