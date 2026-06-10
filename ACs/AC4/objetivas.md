# Questões Objetivas: Django REST na AWS Elastic Beanstalk (Novo Formulário)

Este arquivo contém as respostas corrigidas, revisadas e justificadas para as novas questões de múltipla escolha.

---

### Questão 1 (2 Pontos)
Um desenvolvedor precisa implantar uma aplicação Django REST na AWS com o menor esforço de gerenciamento de infraestrutura possível. A aplicação tem previsão de crescimento de tráfego e precisa escalar automaticamente. O desenvolvedor deseja uma solução que integre automaticamente EC2, load balancer, auto-scaling e monitoramento.

Considerando o cenário descrito, qual serviço AWS é mais adequado para esta implantação?

- [ ] a) Amazon EC2 com configuração manual de Auto Scaling Groups
- [x] **b) AWS Elastic Beanstalk**
- [ ] c) Amazon Lightsail
- [ ] d) AWS CloudFormation
- [ ] e) Amazon ECS com Fargate

> **Justificativa:** O Elastic Beanstalk atua como uma Plataforma como Serviço (PaaS), abstraindo a complexidade da infraestrutura. Ele cria e interliga automaticamente servidores (EC2), balanceadores de carga (ALB), grupos de auto-scaling e alarmes de monitoramento com base no código enviado, minimizando o esforço de gerência.

---

### Questão 2 (2 Pontos)
Uma equipe de desenvolvimento está implantando uma aplicação Django no Elastic Beanstalk e precisa manter três ambientes separados: desenvolvimento (dev), homologação (staging) e produção (prod). Cada ambiente deve ter seus próprios recursos isolados (banco de dados RDS, bucket S3 e variáveis de ambiente). A equipe não quer que alterações no ambiente de desenvolvimento afetem a produção.

Qual estratégia a equipe deve adotar para atender a esses requisitos?

- [ ] a) Criar uma única aplicação Elastic Beanstalk com múltiplas versões e usar variáveis de ambiente para diferenciar os ambientes
- [ ] b) Criar múltiplas aplicações Elastic Beanstalk, uma para cada ambiente
- [x] **c) Criar uma única aplicação Elastic Beanstalk com múltiplos ambientes (environments), cada um com suas próprias configurações de variáveis, RDS e S3**
- [ ] d) Usar apenas uma instância EC2 e trocar as configurações manualmente
- [ ] e) Implantar tudo em um único ambiente e usar branches do Git para controle

> **Justificativa:** No Elastic Beanstalk, o conceito de "Aplicação" é o escopo lógico do projeto, enquanto os "Ambientes" (*environments*) isolam os recursos computacionais e dados. Criar um ambiente para cada estágio (dev, staging, prod) dentro da mesma aplicação garante o isolamento ideal e independente pedido pela equipe.

---

### Questão 3 (2 Pontos)
Após o deploy de uma aplicação Django no Elastic Beanstalk, a aplicação está retornando erro 500 (Internal Server Error), mas não há informações detalhadas no console do EB. O desenvolvedor precisa acessar os logs detalhados da aplicação para identificar a causa do erro.

Qual comando ou ação o desenvolvedor deve executar para obter os logs completos da aplicação?

- [x] **a) eb logs (comando EB CLI)**
- [ ] b) Acessar a instância EC2 via SSH e ler /var/log/django/error.log
- [ ] c) aws cloudwatch get-log-events --log-group-name /aws/elasticbeanstalk/meu-app/var/log/eb-activity.log
- [ ] d) Acessar o CloudWatch Logs no console AWS e navegar até o grupo de logs do EB
- [ ] e) Todas as alternativas acima podem ser usadas para obter logs, dependendo da configuração

> **Atenção para o Gabarito Escolar:** Como o enunciado cita que o erro aconteceu logo após o deploy e não há detalhes no console do EB, assume-se o cenário padrão onde o recurso de streaming para o CloudWatch Logs **vem desativado por padrão**. Portanto, a alternativa **a)** é a única resposta universal correta, pois ela instrui a CLI a compactar e baixar os logs gerados localmente no servidor de forma direta.

---

### Questão 4 (2 Pontos)
Uma aplicação Django com RDS MySQL foi implantada no Elastic Beanstalk. O desenvolvedor adicionou um novo campo a um modelo existente e precisa executar `python manage.py migrate` no ambiente de produção para atualizar o esquema do banco de dados.

Como o desenvolvedor deve executar as migrações no ambiente de produção de forma segura and automatizada?

- [ ] a) Conectar via SSH à instância EC2 do EB e executar o comando manualmente
- [x] **b) Adicionar um comando de migração no arquivo .ebextensions para executar durante o deploy**
- [ ] c) Executar migrate localmente e confiar que a replicação do RDS irá sincronizar
- [ ] d) Desabilitar as migrações e atualizar o banco manualmente via console RDS
- [ ] e) Criar uma Lambda function que execute as migrações periodicamente

> **Justificativa:** Configurar as migrações sob a diretiva `container_commands` dentro de arquivos da pasta `.ebextensions` assegura a automação no momento exato do deploy. A utilização conjunta da propriedade `leader_only: true` garante que apenas um servidor execute as alterações, impedindo problemas de simultaneidade ou travamento de tabelas no RDS.

---

### Questão 5 (2 Pontos)
Uma aplicação Django REST permite upload de imagens de perfil de usuários. O desenvolvedor está usando o sistema de arquivos local para armazenar essas imagens, mas percebeu que as imagens são perdidas sempre que o ambiente do Elastic Beanstalk é recriado (deploy, scaling, falha de instância).

Qual é a solução definitiva para armazenar arquivos de forma persistente em uma aplicação Django no Elastic Beanstalk?

- [ ] a) Configurar um volume EBS adicional e montá-lo na instância EC2
- [x] **b) Utilizar Amazon S3 com django-storages para armazenar todos os arquivos de mídia**
- [ ] c) Salvar as imagens no banco de dados RDS como BLOB
- [ ] d) Configurar o Elastic Beanstalk para usar instâncias com armazenamento de instância persistente
- [ ] e) Fazer backup periódico dos arquivos para um bucket S3 via cron job

> **Justificativa:** Instâncias de aplicação EC2 controladas por serviços PaaS/Auto Scaling são efêmeras (*stateless*). A boa prática de arquitetura em nuvem dita que os arquivos de mídia carregados por usuários (`MEDIA_ROOT`) devem ser externalizados e salvos em um serviço gerenciado de armazenamento de objetos durável, como o Amazon S3, utilizando a biblioteca `django-storages`.
