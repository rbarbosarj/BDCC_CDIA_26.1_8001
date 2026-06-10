# 🚀 AC4 – Deploy de Aplicações Django na AWS 

Este repositório contém a resolução completa da **AC4 (Atividade Avaliativa 4)** da disciplina de **Big Data & Cloud Computing** do curso de **Engenharia de Computação (Período 2026.1)**. 

A atividade aborda o deploy, gerenciamento e solução de problemas de aplicações Django REST na AWS utilizando serviços gerenciados de computação em nuvem, com foco em arquiteturas distribuídas e resilientes.

---

## 📋 Descrição do Projeto e Contexto Acadêmico

*   **Curso:** Engenharia de Computação
*   **Disciplina:** Big Data & Cloud Computing
*   **Período:** 2026.1
*   **Contexto:** Atividade focada na implantação de uma aplicação Django REST Framework utilizando os serviços computacionais e de armazenamento da AWS (Amazon Web Services), aplicando conceitos de infraestrutura de nuvem *stateless*, alta disponibilidade e segurança.

### 🎯 Objetivos de Aprendizagem
*   Compreender o funcionamento do **AWS Elastic Beanstalk** como uma solução PaaS.
*   Implantar e gerenciar aplicações Django REST em ambiente cloud.
*   Dominar conceitos práticos de **Auto Scaling** e **Load Balancing**.
*   Configurar e injetar variáveis de ambiente com segurança em produção.
*   Integrar sistemas de microsserviços e persistência com **Amazon RDS (MySQL)**.
*   Configurar armazenamento persistente e distribuído utilizando **Amazon S3** via `django-storages`.
*   Aplicar boas práticas de arquitetura cloud, segurança e integridade de dados.

---

## 🛠️ Tecnologias e Serviços AWS Abordados

*   **AWS Elastic Beanstalk:** Orquestração automatizada (Platform as a Service - PaaS).
*   **Amazon EC2:** Camada de computação virtualizada.
*   **Elastic Load Balancer (ELB/ALB):** Distribuição inteligente de tráfego e Health Checks.
*   **Auto Scaling Groups:** Elasticidade e escalabilidade horizontal automática.
*   **Amazon RDS MySQL:** Banco de dados relacional totalmente gerenciado e multi-AZ.
*   **Amazon S3:** Armazenamento persistente de objetos (imagens de perfil/produtos).
*   **django-storages & boto3:** Bibliotecas de integração do ecossistema Python/Django com a AWS.
*   **Django REST Framework:** Framework back-end para construção da API.

---

## 📂 Estrutura de Entrega da Atividade

Após uma atualização no formato do questionário aplicado na disciplina, as resoluções foram devidamente organizadas e segregadas nos seguintes arquivos:

*   **[`objetivas.md`](objetivas.md):** Contém todo o gabarito oficial e atualizado das questões de múltipla escolha (Questões 1 a 5), acompanhado de profundas justificativas de arquitetura de nuvem.
*   **[`discursivas.md`](discursivas.md):** Detalha as respostas teóricas e práticas (Questões 1 e 2), incluindo exemplos reais de códigos de infraestrutura e aplicação para o arquivo `settings.py`, arquivos de configuração YAML da pasta `.ebextensions/` e estratégias de monitoramento.
*   **[`respostas.md`](respostas.md):** Mantido no repositório como histórico de documentação técnica, detalhando os motivos da transição estrutural e servindo como redirecionamento para o novo padrão de entrega da atividade.

---

## 🚀 Conceitos Computacionais Consolidados

A resolução desta atividade consolida o entendimento prático sobre:
1.  **Arquitetura Distribuída e Stateless:** Desacoplamento da camada de processamento (EC2), dados (RDS) e arquivos estáticos/mídia (S3).
2.  **Alta Disponibilidade (High Availability):** Redundância via implantações em múltiplas Zonas de Disponibilidade (Multi-AZ) tolerantes a falhas físicas.
3.  **Deploy Automatizado e Zero-Downtime:** Atualizações de ambiente sem interrupção de serviço através de políticas avançadas de deploy do Elastic Beanstalk (*Rolling with additional batch*).
4.  **Segurança e Governança:** Proteção de chaves e segredos em memória via *Environment Properties*, mantendo o código-fonte limpo no Git.

---
💡 *Material técnico desenvolvido como parte dos requisitos de avaliação da disciplina Big Data & Cloud Computing.*
