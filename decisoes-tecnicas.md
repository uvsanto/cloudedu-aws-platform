# Justificativa das Escolhas Técnicas

A definição da arquitetura não decorreu da adoção automática dos serviços mais populares da AWS, mas de um processo de avaliação de alternativas, ponderando requisitos do problema, custo, complexidade operacional e adequação ao perfil de tráfego da Escola Tech.

## 1. Camada de borda: Route 53, CloudFront e AWS WAF

**O problema exigia** um ponto de entrada capaz de absorver picos súbitos de tráfego antes que atingissem a camada de aplicação, reduzindo a carga que chega às instâncias, além de um mecanismo de verificação de disponibilidade do serviço a nível de DNS.

**Alternativa considerada:** expor o ALB diretamente à internet, sem camada de cache ou filtragem, deixando toda a proteção a cargo apenas dos Security Groups. Essa abordagem foi descartada por não resolver diretamente o requisito central do escopo — absorver picos de tráfego de campanhas de marketing —, transferindo toda a carga imediatamente para o Auto Scaling Group, com maior tempo de reação a ameaças e maior custo de escalonamento.

**Optou-se por Amazon Route 53**, com health check do endpoint público, combinado a **Amazon CloudFront integrado ao AWS WAF**, utilizando a integração nativa de um clique disponível no console do ALB desde novembro de 2024. O CloudFront atua como ponto único de entrada distribuído, armazenando em cache conteúdo estático (HTML, CSS, JS, imagens) próximo ao usuário, enquanto o WAF filtra ameaças comuns antes que o tráfego alcance o ALB. Como refinamento do Security Group, o ALB passa a aceitar tráfego nas portas 80/443 exclusivamente a partir do prefixo gerenciado do CloudFront, em vez da internet aberta — endurecendo o critério de segurança de rede além do que o escopo original exigia.

## 2. Balanceamento de carga: Application Load Balancer

**O problema exigia** um mecanismo capaz de distribuir requisições HTTP/HTTPS entre múltiplas instâncias, com suporte a health checks e integração nativa com o Auto Scaling Group.

**Alternativas consideradas:** Classic Load Balancer (CLB) e Network Load Balancer (NLB). O CLB foi descartado por ser um serviço legado, sem roteamento baseado em conteúdo. O NLB opera na camada de transporte (L4) e é otimizado para altíssimo volume de conexões TCP/UDP de baixa latência — perfil que não representa a demanda, majoritariamente HTTP, da Escola Tech.

**Optou-se pelo ALB** por operar na camada de aplicação (L7), permitindo roteamento baseado em conteúdo, integração nativa com Target Groups e Auto Scaling, e terminação SSL/TLS centralizada via AWS Certificate Manager.

## 3. Camada de computação: EC2 com Auto Scaling

**O problema exigia** uma camada de processamento com controle sobre o ambiente de execução e capacidade de escalar automaticamente durante picos de campanhas de marketing.

**Alternativas consideradas:** AWS Lambda (serverless) e Amazon ECS com Fargate. O Lambda foi descartado por limitações de tempo de execução e cold start em picos súbitos, além de exigir adaptação da aplicação ao modelo de funções orientadas a evento. O Fargate foi considerado, mas com custo por vCPU/memória superior a instâncias equivalentes e exigência de containerização, fora do escopo do projeto.

**Optou-se por EC2 com Auto Scaling Group**, por oferecer compatibilidade imediata com a aplicação existente e previsibilidade de custo por tipo de instância.

**Tipo de instância**: recomenda-se a família Graviton (ARM), como T4g para carga leve/intermitente — perfil compatível com o tráfego da Escola Tech fora de campanhas — em vez de instâncias x86 equivalentes (ex: t3.micro), pela melhor relação custo-benefício. Essa escolha exige compatibilidade da aplicação com a arquitetura ARM (aarch64), o que deve ser validado antes da migração.

**Estratégia de escalonamento**: em vez de depender apenas de uma política reativa, recomenda-se combinar três mecanismos do Auto Scaling: **Target Tracking** (reage a picos inesperados de CPU), **Scheduled Scaling** (reduz a capacidade durante a madrugada e aumenta no horário comercial, atendendo diretamente ao requisito de custo do escopo) e **Predictive Scaling** (usa aprendizado de máquina para antecipar padrões cíclicos diários/semanais e provisionar capacidade minutos antes do pico). Para reduzir a latência do scale-out durante picos súbitos de campanha, também se recomenda o uso de **Warm Pools**, que mantêm instâncias pré-inicializadas prontas para entrar em produção rapidamente.

## 4. Rede: VPC Multi-AZ

**O problema exigia** que a indisponibilidade de um data center da AWS não interrompesse o serviço.

**Alternativa considerada:** implantação em Availability Zone única, reduzindo complexidade e custo marginal de transferência entre zonas.

**Optou-se pela distribuição em duas Availability Zones**, único modelo capaz de atender ao requisito de resiliência do escopo — a indisponibilidade do site durante uma campanha de marketing tem impacto direto na captação de novos alunos. Cada AZ possui seu próprio NAT Gateway, evitando que essa dependência de rede se torne, ela própria, um ponto único de falha.

## 5. Camada de dados: Amazon RDS

**O problema exigia** armazenamento persistente e estruturado para registros acadêmicos (matrículas, notas, dados de alunos), com integridade transacional e backup automatizado — a corrupção de registros por quedas de energia sem redundância era uma das falhas centrais do cenário local.

**Alternativas consideradas:** SGBD instalado em uma instância EC2, ou manutenção do banco no servidor local em modelo híbrido. A primeira opção foi descartada por transferir à equipe a responsabilidade operacional de patching, backup manual e configuração de alta disponibilidade, tarefas que o RDS gerencia nativamente. A segunda foi descartada por não resolver o problema de resiliência que motivou o projeto.

**Optou-se pelo Amazon RDS** em modo Multi-AZ, com backups automáticos e failover gerenciado, eliminando a fragilidade de banco de dados sem redundância identificada na causa-raiz do problema.

## 6. Armazenamento de arquivos: Amazon S3

**O problema exigia** um repositório para materiais didáticos, arquivos de mídia e os assets estáticos do próprio site de matrículas (imagens, CSS, JS), com alta durabilidade e custo compatível com o volume de dados educacionais.

**Alternativas consideradas:** Amazon EFS e armazenamento em disco local anexado às instâncias. O EFS foi descartado por custo por GB superior ao S3 para um padrão de acesso predominantemente de leitura. O armazenamento local foi descartado por acoplar os dados ao ciclo de vida das instâncias EC2 — problemático em um ambiente de Auto Scaling, no qual instâncias são criadas e destruídas dinamicamente.

**Optou-se pelo Amazon S3**, com durabilidade de 99,999999999% (11 noves) e desacoplamento total entre armazenamento e computação. Servir os assets estáticos diretamente do S3, com cache pelo CloudFront, reduz a carga sobre as instâncias EC2, que passam a processar apenas a lógica de negócio da aplicação.

## 7. HTTPS e certificados

A terminação SSL/TLS ocorre tanto no CloudFront quanto no Application Load Balancer, com certificado emitido e renovado automaticamente pelo AWS Certificate Manager (ACM). O listener HTTP (porta 80) redireciona permanentemente (301) para HTTPS (porta 443), garantindo que nenhuma requisição trafegue sem criptografia.

## 8. Controles de segurança adicionais

Complementando os Security Groups em camadas — o ALB aceita tráfego nas portas 80/443 exclusivamente do prefixo gerenciado do CloudFront (não da internet aberta), e o EC2 aceita apenas tráfego do Security Group do ALB —, a arquitetura adota três controles alinhados ao pilar de Segurança do AWS Well-Architected Framework:

- **IAM com privilégio mínimo**: cada instância EC2 recebe uma IAM Role restrita aos recursos que efetivamente utiliza (S3, RDS), sem permissões administrativas amplas.
- **AWS Secrets Manager**: credenciais de acesso ao RDS não ficam em código-fonte ou variáveis de ambiente estáticas, sendo recuperadas dinamicamente em tempo de execução.
- **Criptografia em repouso e em trânsito**: dados no RDS e no S3 criptografados com AES-256; comunicação sempre via HTTPS/TLS.
