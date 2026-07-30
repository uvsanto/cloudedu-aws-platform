<div align="center">

<img src="docs/banner.png" alt="Banner CloudEdu AWS Platform" width="100%"/>

### Projeto de Trabalho de Conclusão de Curso (TCC) 

Arquitetura de referência em Amazon Web Services (AWS) para hospedagem resiliente, escalável, segura e otimizada em custos da plataforma fictícia **Escola Tech**, desenvolvida para suportar grandes campanhas de matrícula com alta disponibilidade e excelente experiência do usuário.
<img src="docs/logogrupo.png"/>


</div>

[![AWS](https://img.shields.io/badge/AWS-Cloud-orange)]()
[![Cloud Computing](https://img.shields.io/badge/Cloud-Computing-blue)]()
[![Well-Architected](https://img.shields.io/badge/AWS-Well--Architected-success)]()
[![FinOps](https://img.shields.io/badge/FinOps-Foundation-green)]()
[![IaC](https://img.shields.io/badge/IaC-CloudFormation-yellow)]()
[![License](https://img.shields.io/badge/license-MIT-blue)]()

---

## 📖 Resumo Executivo
O CloudEdu AWS Platform, desenvolvido pelo Team 3 da Escola da Nuvem, apresenta uma arquitetura de referência em Amazon Web Services (AWS) para a plataforma fictícia Escola Tech.
O objetivo é demonstrar como a nuvem pode oferecer alta disponibilidade, elasticidade, segurança em múltiplas camadas, observabilidade e controle de custos em cenários de grande demanda.

O projeto contempla dois cenários complementares:

Modelo A (Free Tier / Acadêmico): explora a viabilidade de construir uma solução funcional na AWS utilizando créditos e serviços gratuitos, sem custo inicial.

Modelo B (Enterprise): simula um ambiente corporativo com orçamento controlado de R$ 2.000/mês, ajustável em períodos críticos de matrícula e rematrícula, assegurando previsibilidade tecnológica e financeira.
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

## ✅ Principais Características
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

🌎 Visão Geral da Solução
A arquitetura proposta combina serviços gerenciados da AWS para atender aos requisitos de disponibilidade, elasticidade e segurança:

Elastic Load Balancing (ALB): distribui requisições de forma inteligente entre instâncias, evitando sobrecarga.

Amazon EC2 com Auto Scaling: hospeda o site em instâncias que aumentam ou reduzem automaticamente conforme a demanda.

Amazon VPC (Multi-AZ): rede privada que garante redundância entre zonas de disponibilidade.

Amazon RDS (Multi-AZ): banco de dados resiliente com failover automático.

Amazon S3: armazenamento de ativos estáticos e mídias educacionais.

Pontos de Atenção
Segurança: uso de Security Groups, IAM e WAF para proteger cada camada.

Elasticidade vs. Escalabilidade: foco em elasticidade — reduzir recursos em períodos de baixa e expandir em picos.

Health Checks: ALB monitora instâncias e o Auto Scaling substitui automaticamente servidores não saudáveis.
---

## 🎯 Contexto do Problema
A Escola Tech é uma plataforma de cursos online que precisa lançar uma nova página de matrículas.
O desafio: lidar com tráfego constante e picos súbitos de acessos em campanhas digitais, sem travamentos e sem custos excessivos.

Atualmente, o site roda em infraestrutura local (on-premises), que falha em momentos de alta demanda. A solução proposta é migrar para a AWS, garantindo:

Resiliência: manter o site disponível mesmo em caso de falhas.

Escalabilidade automática: absorver picos de tráfego sem intervenção manual.

Custos otimizados: reduzir recursos em períodos de baixa utilização.

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

<img src="docs/AWS Architecture2.jpeg"/>   
---

## 🏢 Modelo B (Enterprise)
- Multi-AZ, RDS Aurora (ou RDS Multi-AZ), ALB, Auto Scaling com políticas Target Tracking e Scheduled, WAF, Shield, Secrets Manager, KMS, CloudTrail, GuardDuty, Security Hub.
- Objetivo: produção com governança, segurança e FinOps.
<img src="docs/Diagrama - Modelo B.png"/>
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
