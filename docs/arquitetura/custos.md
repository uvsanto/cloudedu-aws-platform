# Estimativa de Custos

Estimativa mensal aproximada, região us-east-1, tráfego moderado com picos sazonais durante campanhas de matrícula.

| Componente | Cálculo | Custo estimado |
|---|---|---|
| EC2 t3.micro (2 instâncias, mínimo 24/7) | US$ 7,59/mês por instância × 2 | ≈ US$ 15,18 |
| EC2 t3.micro (picos, até +4 instâncias) | Ativas apenas durante campanhas | ≈ US$ 3,00 – 5,00 |
| Application Load Balancer | Taxa fixa (~US$ 16,20/mês) + LCU | ≈ US$ 22,00 – 28,00 |
| NAT Gateway | Taxa horária fixa + processamento de dados | ≈ US$ 33,00 |
| Amazon RDS (instância pequena, Multi-AZ) | A validar no Pricing Calculator | *pendente de dimensionamento* |
| Amazon S3 | Armazenamento de objetos + requisições | *pendente de estimativa de volume* |
| **Total aproximado (sem RDS/S3)** | | **≈ US$ 75,00 – 90,00/mês** |

## Otimizações consideradas

- **Free Tier**: instâncias t3.micro e as primeiras 750 horas do ALB por mês estão no nível gratuito durante os primeiros 12 meses da conta, reduzindo custo em ambiente de homologação/testes.
- **Sazonalidade escolar**: durante períodos de baixa demanda (férias), o Auto Scaling Group reduz a frota ao mínimo configurado, gerando economia real frente ao servidor local, que consumiria energia e manutenção mesmo ocioso.
- **NAT Gateway** é, proporcionalmente, o item mais caro da arquitetura — ponto de otimização futura via VPC Endpoints para reduzir dependência de tráfego pelo NAT.
- **AWS Budgets e Billing Alerts**: configurados para notificar a gestão caso o consumo mensal exceda 80% do orçamento previsto.

## Modelo A (Free Tier) x Modelo B (Enterprise)

Duas formas de dimensionar a mesma arquitetura, dependendo do estágio do projeto:

| Aspecto | Modelo A — Free Tier | Modelo B — Enterprise |
|---|---|---|
| Instâncias EC2 | t4g.micro / t3.micro (Free Tier, 12 meses) | Instâncias Graviton dimensionadas por carga real (ex: m8g.large) |
| Auto Scaling | Mínimo 1–2, máximo 4 | Mínimo 2–4, máximo 10+, com Predictive Scaling |
| RDS | db.t3.micro Single-AZ (Free Tier) | Instância dedicada Multi-AZ, réplica de leitura se necessário |
| CloudFront/WAF | Camada gratuita (1 TB de saída, WAF cobrado à parte) | Uso pleno, com regras WAF customizadas adicionais |
| Disponibilidade | Alta, mas sem redundância total em todos os componentes | Alta disponibilidade ponta a ponta, com failover testado |
| Custo mensal estimado | Próximo de US$ 0–20 (dentro do Free Tier) | US$ 150–300+, dependendo do tráfego real |
| Indicado para | Ambiente de estudo, homologação, demonstração do TCC | Produção real, com SLA e tráfego consistente |

**Vantagens e riscos de cada lado:** o Modelo A elimina custo, mas replica menos fielmente um cenário de produção — health checks, failover e Auto Scaling se comportam de forma diferente sob carga real versus carga simulada em ambiente Free Tier. O Modelo B reflete melhor a operação real da Escola Tech, mas exige orçamento contínuo. Para os fins deste TCC, o **Modelo A é o recomendado para a implantação de demonstração**, com o Modelo B documentado como o cenário de produção que a arquitetura foi desenhada para suportar.

**Teto de orçamento proposto**: como prática de governança, sugere-se não exceder R$ 2.500/mês (≈ R$ 30.000/ano) em ambiente de produção real — valor de referência para dimensionar alertas, não um custo esperado da carga atual do projeto (a estimativa da tabela acima já fica bem abaixo desse teto).

## Ferramentas de governança de custo (AWS)

| Ferramenta | Função |
|---|---|
| AWS Cost Explorer | Visualiza e analisa custos históricos (até 12 meses), filtra por tags de projeto |
| AWS Budgets | Define orçamentos e alertas proativos por e-mail quando o custo real ou previsto ultrapassa o limite |
| AWS Cost Anomaly Detection | Usa machine learning para detectar picos de gasto fora do padrão |
| AWS Trusted Advisor | Recomenda otimizações: identifica instâncias EC2 subutilizadas e volumes ociosos |
| AWS Pricing Calculator | Simula custo antes de implantar qualquer mudança na arquitetura |
| Tags de alocação de custo | Rastreiam consumo por projeto/ambiente (ex: `Project=CloudEdu`, `Environment=Homologacao`) |

## Magalu Cloud como alternativa nacional

| Aspecto | AWS (usado no projeto) | Magalu Cloud |
|---|---|---|
| Moeda de cobrança | Dólar (sujeito a variação cambial) | Real, sem variação cambial |
| Free Tier permanente | Sim (12 meses, cotas definidas) | Não identificado — modelo focado em preço transparente por uso |
| Banco de dados gerenciado | RDS (Multi-AZ configurável) | DBaaS MySQL/PostgreSQL, cluster com failover automático incluso |
| Gestão de identidade (IAM) | IAM (parte da conta, sem custo direto) | Turia IAM, gratuito |
| Maturidade de serviços de borda (CDN/WAF) | CloudFront + AWS WAF, maduro e global | Não identificado no escopo desta pesquisa |
| Melhor cenário de uso | Projetos que exigem catálogo amplo de serviços gerenciados e presença global | Projetos com sensibilidade a câmbio e preferência por previsibilidade em reais |

**Conclusão para o TCC**: a Magalu Cloud é uma alternativa nacional legítima a se mencionar, principalmente pelo argumento de custo em reais sem exposição cambial — relevante para uma instituição de ensino brasileira como a Escola Tech. Mas o catálogo de serviços maduros de borda (CDN + WAF) e a integração nativa entre ALB/ASG que a arquitetura do projeto depende diretamente ainda são mais consolidados na AWS. Recomendo citar como alternativa nacional viável para cenários futuros de redução de custo cambial, não como substituição da arquitetura atual.

> Valores de referência para fins acadêmicos. Recomenda-se validar os números finais, incluindo RDS e S3, no [AWS Pricing Calculator](https://calculator.aws/) antes de fechar a documentação.



