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

# FAQ da Banca — CloudEdu AWS Platform

**Objetivo:** respostas objetivas e referenciadas para as perguntas mais prováveis da banca.

---

## 1. Por que escolher a AWS para este projeto?
**Resposta:** A AWS foi escolhida por sua maturidade de mercado, amplitude de serviços, integração nativa entre componentes (ex.: CloudFront + WAF + ALB) e por ser a plataforma utilizada na formação da Escola da Nuvem. A decisão foi baseada em critérios técnicos (capacidade, integração, custos e suporte a práticas Well‑Architected), não apenas em preferência.

---

## 2. Por que EC2 em vez de ECS/EKS ou Serverless inicialmente?
**Resposta:** O objetivo do TCC é demonstrar fundamentos de IaaS (SO, Security Groups, Auto Scaling, health checks). EC2 permite explorar esses conceitos de forma didática. ECS/EKS e Serverless estão previstos no roadmap como evolução para reduzir esforço operacional e aumentar portabilidade.

---

## 3. Como a arquitetura garante alta disponibilidade?
**Resposta:** Multi‑AZ para EC2 e RDS, Application Load Balancer com health checks, Auto Scaling para recriar capacidade em zonas saudáveis e RDS Multi‑AZ com failover automático. Esses elementos combinados reduzem risco de downtime por falha de AZ.

---

## 4. Como a solução controla custos e evita estouro de orçamento?
**Resposta:** Implementação de FinOps: AWS Budgets com alertas (50/80/100%), Cost Explorer, Cost Anomaly Detection, tagging para alocação de custos, políticas de desligamento automático e uso de Spot/rightsizing quando aplicável.

---

## 5. Como a arquitetura atende à LGPD?
**Resposta:** Criptografia em trânsito (TLS) e em repouso (KMS), gerenciamento de segredos via Secrets Manager, controle de acesso via IAM com princípio do menor privilégio, auditoria com CloudTrail e monitoramento contínuo com GuardDuty e Security Hub. Observação: conformidade formal exige auditoria externa.

---

## 6. Quais pilares do Well‑Architected foram priorizados?
**Resposta:** Todos os seis pilares: Excelência Operacional, Segurança, Confiabilidade, Eficiência de Performance, Otimização de Custos e Sustentabilidade. Cada decisão técnica foi mapeada para um ou mais pilares (ver `docs/decisoes-tecnicas.md`).

---

## 7. O que acontece se uma Availability Zone ficar indisponível?
**Resposta:** ALB para de rotear para instâncias na AZ afetada; Auto Scaling recria instâncias em AZs saudáveis; RDS Multi‑AZ faz failover para a réplica. Impacto mínimo ao usuário final.

---

## 8. Por que não usar Cloudflare em vez do AWS WAF?
**Resposta:** AWS WAF foi escolhido pela integração nativa com CloudFront e ALB, simplificando gestão centralizada. Cloudflare é uma alternativa válida em cenários onde custo/latência ou funcionalidades específicas (ex.: CDN global com planos gratuitos) sejam mais vantajosas; pode ser reavaliado em otimizações futuras.

---

## 9. Como validaram que a arquitetura suporta picos?
**Resposta:** Projeto baseado em padrões AWS (ALB + Auto Scaling + CloudFront). Testes de carga reais são recomendados como próximo passo; o TCC documenta o roteiro de testes e as métricas esperadas (throughput, latência, erros).

---

## 10. Quais são os riscos técnicos remanescentes?
**Resposta:** riscos incluem configuração incorreta de IAM, custos por recursos esquecidos (NAT Gateway), e dependência de RDS gerenciado. Mitigações: políticas IaC com validação, automações de limpeza e alertas FinOps.

---

## 11. Como é feita a observabilidade?
**Resposta:** CloudWatch (métricas, logs, dashboards), CloudWatch Alarms, CloudTrail (auditoria), X‑Ray (tracing), ADOT/OpenTelemetry e integração opcional com Grafana/Prometheus.

---

## 12. Se o orçamento for reduzido pela metade, o que muda?
**Resposta:** Priorizar disponibilidade e segurança; migrar para Modelo A (Free Tier) temporariamente, reduzir capacidade, usar instâncias menores/Spot, e intensificar práticas de FinOps.

---

## 13. Quais próximos passos para produção?
**Resposta:** testes de carga, pipelines CI/CD completos, políticas de backup e DR testadas, auditoria de segurança, definição de SLAs/SLOs e automações de resposta a incidentes.

---

## 14. Referências rápidas
- Arquitetura e decisões técnicas: `docs/decisoes-tecnicas.md`  
- Custos detalhados: `docs/custos.md`  
- Materiais de defesa: `docs/apresentacao/`

# Speech — Roteiro de Apresentação (3–5 minutos)

**Duração estimada:** 3 a 5 minutos  
**Estrutura:** 6 slides, 30–50 segundos por slide

---

## Slide 1 — Abertura (20–30s)
**Fala:**  
“Bom dia/tarde. Somos o Team 3 da Escola da Nuvem. Apresentamos o CloudEdu AWS Platform — uma arquitetura de referência para a Escola Tech, projetada para garantir disponibilidade durante picos de matrícula, reduzir latência e controlar custos. Em 5 minutos mostro o problema, a solução, resultados esperados e próximos passos.”

**Objetivo do slide:** contextualizar problema e objetivo do projeto.

---

## Slide 2 — Problema e Requisitos (30–40s)
**Fala:**  
“A Escola Tech sofria indisponibilidade em campanhas de marketing por falta de elasticidade e redundância. Requisitos: alta disponibilidade, elasticidade automática, segurança, observabilidade e custo controlado. Nosso projeto atende a todos esses pontos.”

**Dica:** citar 2‑3 métricas alvo (ex.: disponibilidade 99,9%; latência média alvo 120ms).

---

## Slide 3 — Arquitetura (40–50s)
**Fala:**  
“Arquitetura: Route 53 → CloudFront → WAF → ALB → EC2 em Auto Scaling (multi‑AZ). Dados em RDS Multi‑AZ; assets em S3. Observabilidade com CloudWatch/X‑Ray; segurança com IAM, KMS, Secrets Manager, GuardDuty. Essa combinação garante resiliência e governança.”

**Objetivo:** mostrar o diagrama e destacar componentes críticos.

---

## Slide 4 — Modelos e Custos (30–40s)
**Fala:**  
“Oferecemos dois modelos: Modelo A (Free Tier) para estudos e PoC; Modelo B (Enterprise) para produção. Estimativa para tráfego moderado (região sa‑east‑1): R$ 420–500/mês. Políticas FinOps e Budgets protegem o orçamento.”

**Dica:** mencionar o teto orçamentário e alertas (50/80/100%).

---

## Slide 5 — Segurança, Observabilidade e DR (30–40s)
**Fala:**  
“Segurança em camadas: WAF, Security Groups, KMS, Secrets Manager e auditoria com CloudTrail. Observabilidade com CloudWatch/X‑Ray e alertas via SNS. DR: backups RDS, versionamento S3 e possibilidade de restauração em outra região.”

**Objetivo:** tranquilizar sobre proteção e continuidade.

---

## Slide 6 — Conclusão e Roadmap (30–40s)
**Fala:**  
“Conclusão: arquitetura madura, alinhada ao Well‑Architected Framework e práticas FinOps. Próximos passos: testes de carga, CI/CD completo, migração gradual para contêineres/Serverless e integração de observabilidade avançada. Obrigado — estamos prontos para perguntas.”

**Fechamento:** abrir para perguntas da banca.

---

## Notas para apresentação oral
- Fale devagar e com clareza; use o diagrama como guia visual.  
- Tenha 3 evidências rápidas à mão (print CloudWatch, alerta Budgets, log de deploy).  
- Se a banca pedir detalhes técnicos, direcione para `docs/decisoes-tecnicas.md` e `docs/apresentacao/Perguntas-Dificeis.md`.


## 🧑🏻‍💻 Autores

Desenvolvido pelo **Team 3** — Escola da Nuvem:

Jefferson Da Mata Dos Reis — https://github.com/ReisxJeff

Daniel Victor Moreira Braga

Evandro Gomes Lemos — https://www.linkedin.com/in/evandrogomeslemos/ (linkedin.com in Bing)

Daniel Tadao Silva Shimada — https://github.com/DanielTadao (github.com in Bing)

Vagner Tomaz dos Santos — https://github.com/uvsanto

Marcos Roberto De Andrade — https://github.com/MarcosRobertodeAndrade01 (github.com in Bing)

## 📄 Licença

*Este projeto está licenciado sob os termos da MIT License. Consulte o arquivo LICENSE.*
