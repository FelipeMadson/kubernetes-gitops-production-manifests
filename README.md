# Kubernetes GitOps Production Manifests

[![CI](https://github.com/FelipeMadson/kubernetes-gitops-production-manifests/actions/workflows/ci.yml/badge.svg)](https://github.com/FelipeMadson/kubernetes-gitops-production-manifests/actions)
[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)
[![Release](https://img.shields.io/badge/release-v1.0.0-green.svg)](https://github.com/FelipeMadson/kubernetes-gitops-production-manifests/releases)

Declarative production-grade Kubernetes architecture and GitOps deployment configurations designed by [Felipe Madison](https://github.com/FelipeMadson). Features zero-trust `NetworkPolicy` isolation, hardened pod security standards, Horizontal Pod Autoscaling (HPA), and automated reconciliation via ArgoCD.

## Topology & Ingress Architecture

```text
┌────────────────────────────────────────────────────────────────────────┐
│                        INGRESS CONTROLLER & TLS                        │
│   • Cert-Manager Automated Let's Encrypt TLS                           │
│   • Reverse-Engineered Gateway (Path Routing & Token Bucket Rate Limit)│
└───────────────────────────────────┬────────────────────────────────────┘
                                    │
┌───────────────────────────────────▼────────────────────────────────────┐
│                    ZERO-TRUST NETWORK POLICY PERIMETER                 │
│   • Default-Deny Ingress and Egress for all application namespaces     │
│   • Explicit Pod-to-Pod Whitelisting                                   │
└──────────────┬────────────────────┬────────────────────┬───────────────┘
               │                    │                    │
               ▼                    ▼                    ▼
      ┌──────────────────┐ ┌──────────────────┐ ┌──────────────────┐
      │  FINTECH ENGINE  │ │   DJANGO SAAS    │ │    FLASK BUS     │
      │  (Spring Boot 3) │ │   (Tenant RLS)   │ │  (Outbox PubSub) │
      │  Non-root, HPA   │ │   Non-root, HPA  │ │   Non-root, HPA  │
      └──────────────────┘ └──────────────────┘ └──────────────────┘
```

## Key Security & Reliability Controls

- **Pod Security Standards (Restricted)**: Enforces `runAsNonRoot: true`, `readOnlyRootFilesystem: true`, and drops all POSIX capabilities (`drop: [ALL]`).
- **Zero-Trust Network Policies**: Mutual isolation preventing unauthorized lateral movement inside the cluster.
- **Resource Quotas & Limits**: Explicit CPU/Memory limits and requests to prevent noisy neighbor degradation.
- **Horizontal Pod Autoscaling**: Automatically scales pods based on CPU and custom memory thresholds.

## Author

Authored and maintained by **[Felipe Madison](https://github.com/FelipeMadson)**.
