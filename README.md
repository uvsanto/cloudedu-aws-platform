# ☁️ CloudEdu AWS Platform

> Projeto de Trabalho de Conclusão de Curso (TCC) – Escola da Nuvem – Team 3

Arquitetura de referência em Amazon Web Services (AWS) para hospedagem resiliente, escalável, segura e otimizada em custos da plataforma fictícia **Escola Tech**, desenvolvida para suportar grandes campanhas de matrícula com alta disponibilidade e excelente experiência do usuário.

<p align="center">
<img src="docs/imagens/banner.png" width="100%">
</p>

<p align="center">
  <img src="https://img.shields.io/badge/AWS-Cloud-orange" alt="AWS Cloud">
  <img src="https://img.shields.io/badge/Cloud-Computing-blue" alt="Cloud Computing">
  <img src="https://img.shields.io/badge/AWS-Well--Architected-success" alt="Well-Architected">
  <img src="https://img.shields.io/badge/FinOps-Foundation-green" alt="FinOps">
  <img src="https://img.shields.io/badge/IaC-CloudFormation-yellow" alt="IaC">
  <img src="https://img.shields.io/badge/license-MIT-blue" alt="License">
</p>

---

## 📖 Resumo Executivo

O **CloudEdu AWS Platform** apresenta uma arquitetura moderna em nuvem desenvolvida para solucionar problemas de indisponibilidade enfrentados pela plataforma fictícia Escola Tech durante períodos de grande volume de acessos. 

A solução foi projetada utilizando serviços gerenciados da Amazon Web Services (AWS), adotando práticas recomendadas de arquitetura para garantir alta disponibilidade, escalabilidade automática, segurança em múltiplas camadas, observabilidade, governança e otimização de custos. O projeto contempla duas abordagens de maturidade:
* **Modelo A (Free Tier / Acadêmico):** Arquitetura simplificada voltada para estudos, laboratórios e demonstrações de baixo custo.
* **Modelo B (Enterprise):** Arquitetura corporativa de alta performance, redundância e segurança para ambiente de produção.

---

## ✅ Conformidade com Frameworks e Requisitos

| Framework / Pilar | Status |
|-------------------|:------:|
| AWS Well-Architected Framework | ✅ Atendido |
| AWS Cloud Adoption Framework (CAF) | ✅ Atendido |
| FinOps Foundation | ✅ Atendido |
| NIST Cybersecurity Framework | ✅ Atendido |
| LGPD (Proteção de Dados) | ✅ Atendido |
| DevOps & SRE | ✅ Atendido |
| Observabilidade Proativa | ✅ Atendido |

| Requisito Técnico da Solução | Status |
|------------------------------|:------:|
| Alta Disponibilidade (Multi-AZ) | ✅ |
| Elasticidade e Escalabilidade Automática | ✅ |
| Segurança em Camadas (Defense in Depth) | ✅ |
| Governança Financeira (FinOps) | ✅ |
| Infraestrutura como Código (IaC) | ✅ |
| Backup e Disaster Recovery | ✅ |

---

## 🌎 1. Visão Geral e Contexto do Problema

Durante campanhas de marketing digital, a Escola Tech registrava um aumento expressivo no número de acessos simultâneos ao portal de matrículas. A infraestrutura local (*on-premises*) existente não possuía mecanismos automáticos de escalabilidade ou alta disponibilidade, ocasionando travamentos, perda de inscrições e impactos na credibilidade da instituição.

O projeto resolve esse desafio implementando uma infraestrutura baseada nos seguintes pilares:
* **Computação Elástica:** Amazon EC2 gerenciado por Auto Scaling Groups e Application Load Balancer (ALB).
* **Banco de Dados Resiliente:** Amazon RDS configurado em múltiplas Zonas de Disponibilidade (Multi-AZ).
* **Desempenho e Armazenamento:** Amazon S3 para ativos estáticos e mídias educacionais.

---

## 💳 2. Estratégia de Custos: Free Tier vs. Planos Pagos

### Plano Gratuito (AWS Free Tier) e Créditos
A AWS oferece um programa inicial para novos clientes que permite explorar a plataforma sem compromisso financeiro:
* Até **US$ 200 em créditos promocionais** (US$ 100 iniciais + US$ 100 ao explorar serviços fundamentais).
* Utilização de mais de 30 serviços elegíveis sem custo por até 6 meses.
* Ideal para ambientes de laboratório, Provas de Conceito (PoC) e o **Modelo A** deste projeto.

### Plano Pago (Pay-as-you-go)
Para ambientes produtivos corporativos (como o **Modelo B**):
* Cobrança baseada estritamente no consumo real (*Pay-as-you-go*).
* Acesso a mais de 150 serviços avançados da AWS.
* Governança financeira amparada por ferramentas como *AWS Budgets*, *Cost Explorer*, *Cost Allocation Tags* e *Trusted Advisor*.

---

## 🛡️ 3. Pilares de Segurança, Governança e Observabilidade

* **Segurança em Camadas:** Emprego de IAM (menor privilégio), Security Groups, AWS WAF, AWS Shield, Secrets Manager, KMS, GuardDuty e CloudTrail para conformidade com a LGPD e o NIST.
* **Governança (AWS CAF):** Alinhado às perspectivas de Pessoas, Plataforma, Segurança, Operações e Negócio.
* **Observabilidade (Golden Signals):** Monitoramento contínuo de latência, tráfego, erros e saturação através do Amazon CloudWatch, CloudWatch Logs, Dashboards, Alarmes e AWS X-Ray.

---

## 📁 4. Estrutura do Repositório (File Tree)

O projeto segue o padrão rigoroso de documentação corporativa EADD:

```text
CloudEdu-AWS-Platform/
├── README.md               # Vitrine executiva do projeto
├── docs/                   # Documentação gerencial e guias de apoio
│   ├── FAQ_Banca.md        # Perguntas e respostas técnicas (Apêndice A)
│   ├── Speech.md           # Roteiro de apresentação oral
│   ├── Roadmap.md          # Fases evolutivas da solução
│   └── Lessons-Learned.md  # Relatório de aprendizados do Team 3
├── arquitetura/            # Documentos de desenho HLD / LLD
├── custos/                 # Simulações financeiras e precificação
├── diagramas/              # Topologias visuais oficiais do projeto
└── iac/                    # Templates de Infraestrutura como Código
