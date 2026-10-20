# Manifestos Kubernetes GitOps para Produção

[![CI](https://github.com/FelipeMadson/kubernetes-gitops-production-manifests/actions/workflows/ci.yml/badge.svg)](https://github.com/FelipeMadson/kubernetes-gitops-production-manifests/actions)
[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)
[![Release](https://img.shields.io/badge/release-v1.0.0-green.svg)](https://github.com/FelipeMadson/kubernetes-gitops-production-manifests/releases)

Arquitetura declarativa de Kubernetes para produção e configurações GitOps concebidas por [Felipe Madison](https://github.com/FelipeMadson). Implementa isolamento zero-trust com `NetworkPolicy`, padrões estritos de segurança de pods (PSS Restricted), escalonamento horizontal automático (HPA) e reconciliação contínua com ArgoCD.

## Topologia & Arquitetura de Rede

```text
┌────────────────────────────────────────────────────────────────────────┐
│                        CONTROLADOR INGRESS & TLS                       │
│   • Cert-Manager com Certificados TLS Automatizados                    │
│   • Gateway Resiliente (Roteamento de Rotas e Rate Limit)              │
└───────────────────────────────────┬────────────────────────────────────┘
                                    │
┌───────────────────────────────────▼────────────────────────────────────┐
│                  PERÍMETRO ZERO-TRUST DE NETWORK POLICY                │
│   • Default-Deny de Entrada e Saída em todos os Namespaces             │
│   • Liberação Explícita Pod-a-Pod                                      │
└──────────────┬────────────────────┬────────────────────┬───────────────┘
               │                    │                    │
               ▼                    ▼                    ▼
      ┌──────────────────┐ ┌──────────────────┐ ┌──────────────────┐
      │  FINTECH ENGINE  │ │   DJANGO SAAS    │ │    FLASK BUS     │
      │  (Spring Boot 3) │ │   (Tenant RLS)   │ │  (Outbox PubSub) │
      │  Non-root, HPA   │ │   Non-root, HPA  │ │   Non-root, HPA  │
      └──────────────────┘ └──────────────────┘ └──────────────────┘
```

## Destaques de Segurança & Resiliência

- **Padrão de Segurança de Pod (Restricted)**: Execução forçada como usuário não-root, sistema de arquivos somente-leitura e descarte total de capacidades POSIX.
- **Isolamento de Rede Zero-Trust**: Bloqueio de tráfego lateral entre pods sem autorização explícita.
- **HPA e Quotas Estritas**: Proteção contra consumo descontrolado de CPU e memória.

## Autor

Desenvolvido por **[Felipe Madison](https://github.com/FelipeMadson)**.
