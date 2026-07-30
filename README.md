<div align="center">

<img src="docs/imagens/banner.png" alt="Banner CloudEdu AWS Platform" width="100%"/>

# ☁️ CloudEdu AWS Platform

### Hospedagem resiliente e elástica para a Escola Tech  
**TCC – Escola da Nuvem · Team 3**

</div>

[![AWS](https://img.shields.io/badge/AWS-Cloud-orange)]()
[![Cloud Computing](https://img.shields.io/badge/Cloud-Computing-blue)]()
[![Well-Architected](https://img.shields.io/badge/AWS-Well--Architected-success)]()
[![FinOps](https://img.shields.io/badge/FinOps-Foundation-green)]()
[![IaC](https://img.shields.io/badge/IaC-CloudFormation-yellow)]()
[![License](https://img.shields.io/badge/license-MIT-blue)]()

---

## 📖 Resumo Executivo
O **CloudEdu AWS Platform** é uma arquitetura de referência em AWS para a plataforma fictícia **Escola Tech**, projetada para suportar picos de tráfego em campanhas de matrícula com **alta disponibilidade**, **elasticidade**, **segurança em camadas**, **observabilidade** e **controle de custos**. O projeto apresenta dois modelos: **Modelo A (Free Tier / Acadêmico)** e **Modelo B (Enterprise)**.

---

## ✅ Conformidade com Frameworks
| Framework | Status |
|---|:---:|
| AWS Well-Architected Framework | ✅ |
| AWS Cloud Adoption Framework (CAF) | ✅ |
| FinOps Foundation | ✅ |
| DevOps | ✅ |
| SRE | ✅ |
| LGPD | ✅ |
| NIST Cybersecurity Framework | ✅ |
| Observabilidade | ✅ |

---

## ✔️ Principais Características
| Característica | Status |
|---|:---:|
| Alta Disponibilidade | ✅ |
| Elasticidade | ✅ |
| Escalabilidade Automática | ✅ |
| Segurança em Camadas | ✅ |
| Observabilidade | ✅ |
| Governança | ✅ |
| Infraestrutura como Código (IaC) | ✅ |
| Backup & DR | ✅ |
| FinOps | ✅ |
| Multi-AZ | ✅ |
| Comparativo Multi-Cloud | ✅ |

---

## 📚 Índice
- [Visão Geral](#visão-geral)  
- [Contexto do Problema](#contexto-do-problema)  
- [Arquitetura e Diagramas](#arquitetura-e-diagramas)  
- [Modelo A (Free Tier)](#modelo-a-free-tier)  
- [Modelo B (Enterprise)](#modelo-b-enterprise)  
- [Estimativa de Custos](#estimativa-de-custos)  
- [Segurança](#segurança)  
- [Observabilidade](#observabilidade)  
- [FinOps](#finops)  
- [Como reproduzir / Deploy](#como-reproduzir--deploy)  
- [Materiais de Defesa](#materiais-de-defesa)  
- [Roadmap](#roadmap)  
- [Autores](#autores)  
- [Licença](#licença)

---

## 🌎 Visão Geral
O CloudEdu AWS Platform foi desenvolvido como TCC para propor uma arquitetura moderna em nuvem que substitua a infraestrutura on-premises da Escola Tech, eliminando pontos únicos de falha e permitindo escalabilidade automática durante campanhas de marketing.

---

## 🎯 Contexto do Problema
Durante campanhas, o portal de matrículas sofria indisponibilidade por falta de elasticidade e redundância. O objetivo é garantir disponibilidade contínua, reduzir latência e controlar custos operacionais.

---

## 🏗️ Arquitetura e Diagramas
**Principais componentes**: Route 53, CloudFront, AWS WAF, ALB, EC2 (Graviton) em Auto Scaling Group, RDS Multi-AZ, S3, IAM, Secrets Manager, CloudWatch, X-Ray, SNS.  
**Diagramas** (em `docs/arquitetura/`):
- `diagrama-arquitetura.png` — visão geral
- `diagrama-elasticidade.png` — fluxo de Auto Scaling
- `diagrama-seguranca.png` — camadas de segurança
- `diagrama-finops.png` — visão de custos

**Legenda**: *Fonte: Elaboração própria — Team 3*

---

## 🧩 Modelo A (Free Tier / Acadêmico)
- Uso de instâncias elegíveis ao Free Tier; CloudFront para cache; S3 para assets; RDS em configuração mínima.
- Objetivo: demonstração, PoC e laboratórios com custo reduzido.

---

## 🏢 Modelo B (Enterprise)
- Multi-AZ, RDS Aurora (ou RDS Multi-AZ), ALB, Auto Scaling com políticas Target Tracking e Scheduled, WAF, Shield, Secrets Manager, KMS, CloudTrail, GuardDuty, Security Hub.
- Objetivo: produção com governança, segurança e FinOps.

---

## 💰 Estimativa de Custos (região de referência: **sa-east-1**)
> Valores aproximados para tráfego moderado. Validar com AWS Pricing Calculator antes de implantação.

| Componente | Estimativa (mensal) |
|---|---:|
| EC2 (2 a 6 x t3.micro) | **R$ 90 – 120** |
| Application Load Balancer | **R$ 130 – 160** |
| NAT Gateway | **R$ 200** |
| **Total aproximado** | **R$ 420 – 500 / mês** |

---

## 🔒 Segurança
**Controles e serviços**: IAM (least privilege), MFA, Security Groups, NACLs, AWS WAF, AWS Shield, AWS KMS, AWS Secrets Manager, GuardDuty, Security Hub, AWS Config, CloudTrail.  
**Conformidade**: arquitetura alinhada a LGPD, NIST e ISO/IEC 27001 (observação: conformidade formal requer auditoria externa).

---

## 📈 Observabilidade
**Stack**: CloudWatch (metrics, logs, dashboards), CloudWatch Alarms, CloudTrail, AWS X-Ray, ADOT/OpenTelemetry, Grafana/Prometheus (opcional).  
**Alertas**: SNS para notificações e integração com canais de operação.

---

## 💸 FinOps & Governança
- **Teto orçamentário**: R$ 2.000/mês (exemplo de política).  
- **Ferramentas**: AWS Budgets (alertas 50/80/100%), Cost Explorer, Cost Anomaly Detection, Compute Optimizer, tagging para alocação de custos.  
- **Práticas**: desligamento automático de ambientes não críticos, uso de Spot Instances quando aplicável, rightsizing periódico.

---

## 🚀 Como reproduzir / Deploy (exemplos mínimos)
**Pré-requisitos**
- Conta AWS com permissões de administrador (ou role apropriada)
- AWS CLI configurado (`aws configure`)
- Terraform 1.x ou AWS CloudFormation CLI

**CloudFormation (exemplo)**
```bash
# validar template
aws cloudformation validate-template --template-body file://infraestrutura/template.yaml

# criar stack
aws cloudformation create-stack --stack-name cloudedu --template-body file://infraestrutura/template.yaml --capabilities CAPABILITY_NAMED_IAM

# atualizar stack
aws cloudformation update-stack --stack-name cloudedu --template-body file://infraestrutura/template.yaml --capabilities CAPABILITY_NAMED_IAM
