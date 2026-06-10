# Questões Discursivas: Arquitetura e Soluções no Elastic Beanstalk

Este arquivo contém as respostas técnicas estruturadas para os cenários e problemas apresentados.

---

## Questão Discursiva 1

### a) Descrição da Arquitetura Completa
A infraestrutura proposta atende a todos os requisitos de produção através dos seguintes pilares:
* **AWS Elastic Beanstalk (Camada de Aplicação):** Configura um **Application Load Balancer (ALB)** público que recebe o tráfego do e-commerce e o distribui dinamicamente entre instâncias **Amazon EC2**. Estas máquinas rodam a aplicação Django REST e ficam distribuídas em múltiplas **Zonas de Disponibilidade (Multi-AZ)**. Se uma zona sofrer uma falha física, o balanceador redireciona o tráfego exclusivamente para as zonas saudáveis (**Alta Disponibilidade**). A flutuação de tráfego é gerenciada por um **Auto Scaling Group (ASG)**, que adiciona ou remove réplicas de servidores horizontalmente com base no uso de CPU ou volume de requisições (**Escalabilidade automática**).
* **Amazon RDS MySQL (Camada de Dados):** Banco de dados totalmente gerenciado operando em modo **Multi-AZ**. Os dados escritos no banco primário são replicados de forma síncrona para uma instância stand-by em outra Zona de Disponibilidade, permitindo failover automático. Backups automatizados diários garantem a resiliência (**Banco de dados gerenciado**).
* **Amazon S3 (Camada de Objetos):** Atua como repositório imutável fora das máquinas virtuais para armazenar as fotos dos produtos, servindo os arquivos diretamente aos clientes de forma consistente (**Arquivos de mídia persistentes**).
* **Environment Properties (Segurança):** Chaves criptográficas e senhas do banco são injetadas direto na memória do sistema operacional da AWS para o Django, impedindo que dados sensíveis sejam salvos no repositório Git (**Segurança**).
* **Políticas de Rolling Deploys:** O Elastic Beanstalk atualiza as instâncias em lotes, garantindo que sempre haja instâncias ativas processando pedidos durante a atualização (**Zero-downtime deploys**).

### b) Configuração de Variáveis de Ambiente e Estrutura do `settings.py`
As credenciais devem ser inseridas no painel do Elastic Beanstalk (*Configuration > Software > Environment properties*). No arquivo `settings.py` da aplicação Django, o código captura essas propriedades nativas do sistema operacional:

```python
import os
from pathlib import Path

# Banco de Dados Gerenciado (RDS MySQL)
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.mysql',
        'NAME': os.environ.get('DB_NAME'),
        'USER': os.environ.get('DB_USER'),
        'PASSWORD': os.environ.get('DB_PASSWORD'),
        'HOST': os.environ.get('DB_HOST'),
        'PORT': os.environ.get('DB_PORT', '3306'),
    }
}

# Integração de Armazenamento com o Amazon S3
INSTALLED_APPS += ['storages']

AWS_ACCESS_KEY_ID = os.environ.get('AWS_ACCESS_KEY_ID')
AWS_SECRET_ACCESS_KEY = os.environ.get('AWS_SECRET_ACCESS_KEY')
AWS_STORAGE_BUCKET_NAME = os.environ.get('AWS_STORAGE_BUCKET_NAME')
AWS_S3_REGION_NAME = os.environ.get('AWS_S3_REGION_NAME', 'us-east-1')

# Redireciona o upload de mídias para o S3 de forma persistente
DEFAULT_FILE_STORAGE = 'storages.backends.s3boto3.S3Boto3Storage'
```

### c) Automação de Migrações no Deploy
Para executar os comandos do banco de maneira automática a cada atualização, deve-se criar o arquivo `.ebextensions/db_migrate.config` na raiz do projeto Django com a seguinte estrutura:

```yaml
container_commands:
  01_execute_migrations:
    command: "source /var/app/venv/*/bin/activate && python manage.py migrate --noinput"
    leader_only: true
```
* **Significado técnico:** O comando ativa a *Virtualenv* do Python criada internamente na distribuição Linux do Elastic Beanstalk e roda a migração. A linha `leader_only: true` instrui o Beanstalk a eleger uma única máquina do cluster para rodar a tarefa, impedindo falhas por concorrência simultânea no RDS MySQL.

### d) Viabilização do Zero-Downtime Deploy
O Elastic Beanstalk elimina a indisponibilidade gerenciando o ciclo de vida das instâncias por meio de estratégias de deploy inteligentes controladas pelo Load Balancer. Para ambientes produtivos, utiliza-se a política **Rolling com lote adicional (Rolling with additional batch)** ou **Imutável (Immutable)**. 

No modo *Rolling com lote adicional*, o ecossistema cria temporariamente novas máquinas EC2 para receber a versão atualizada do código. O Load Balancer executes os testes de integridade (*health checks*); assim que as novas instâncias respondem com sucesso, elas começam a receber as requisições dos clientes enquanto as instâncias antigas entram em manutenção sequencial. Isso garante que a capacidade total do cluster nunca caia durante as atualizações.
* **Número mínimo de instâncias:** Para atingir os requisitos de Alta Disponibilidade em produção, o número mínimo configurado deve ser de **2 instâncias EC2**, distribuídas entre diferentes Zonas de Disponibilidade.

---

## Questão Discursiva 2

### a) Resolução do Problema 1 (Indisponibilidade pós-deploy)
* **Causa Raiz:** O ambiente está configurado com a política de deploy padrão **All at once (Tudo de uma vez)**. Essa estratégia derruba simultaneamente todas as instâncias ativas para aplicar a nova versão de código, deixando a aplicação totalmente inacessível para o usuário enquanto o Django inicializa os processos de rede (causando os 30 segundos de timeout).
* **Solução Definitiva:** Modificar a política de deploy do ambiente nas configurações do Elastic Beanstalk para **Rolling com lote adicional** ou **Imutável**. Isso mudará o comportamento da AWS, fazendo-a criar novos servidores com o código atualizado sem desligar a estrutura existente, mantendo o sistema online durante as atualizações.

### b) Resolução do Problema 2 (Uploads de imagem inconsistentes)
* **Causa Raiz:** As imagens de upload estão sendo salvas no sistema de arquivos local do disco rígido da instância EC2 correspondente. Como o ecossistema opera com múltiplas instâncias atrás de um balanceador devido ao Auto Scaling, um usuário faz o upload na máquina "A", mas quando atualiza a página, o balanceador redireciona sua requisição para a máquina "B" (que não possui o arquivo físico em seu disco local), gerando quebras de imagens e arquivos corrompidos para cerca de 5% dos acessos.
* **Solução Definitiva:** Configurar a biblioteca `django-storages` apontando para um bucket centralizado do **Amazon S3**. Ao alterar a diretiva `DEFAULT_FILE_STORAGE` no arquivo `settings.py`, as fotos enviadas pelos clientes ignoram os discos locais e ficam centralizadas e persistidas de forma unificada na AWS.

### c) Resolução do Problema 3 (Performance degradada em horário de pico)
Para solucionar gargalos de pico previsíveis (18h às 22h), podem ser adotadas duas estratégias:
1. **Gatilhamento por Métrica de Escala (Metric-based Scaling):** Ajustar as configurações do Auto Scaling no painel do Elastic Beanstalk para adicionar novos servidores ao cluster de forma responsiva assim que a média de uso de CPU do parque ultrapassar 60% por 3 minutos consecutivos, ou com base na métrica de contagem de requisições recebidas pelo Load Balancer.
2. **Escalabilidade Agendada (Time-based Scaling):** Como o horário do pico é previsível, cria-se uma regra programada no Elastic Beanstalk para forçar o aumento do número mínimo de servidores para, por exemplo, 4 ou 5 instâncias a partir das 17h30, e outra regra para reduzir o cluster de volta ao tamanho básico às 22h30.

### d) Resolução do Problema 4 (Logs limitados)
* **Habilitação do Log Streaming:** No menu lateral do ambiente no console Elastic Beanstalk, navegue em *Configuration > Software* (ou seção de *Logs*) e mude a flag **Log streaming** para **Enabled**. Isso ativa o agente interno unificado do CloudWatch Logs nas máquinas para transmitir os logs em tempo real.
* **Configuração de Logs Customizados do Django:** Modifique o dicionário de logs `LOGGING` no `settings.py` para canalizar todas as mensagens para o fluxo de saída padrão do console (`stdout`). O Elastic Beanstalk coleta nativamente o conteúdo exposto no console pelas ferramentas ASGI/WSGI (como Gunicorn) salvando-as no arquivo `/var/log/web.stdout.log`, que por sua vez será transmitido de maneira íntegra para o console do CloudWatch:

```python
LOGGING = {
    'version': 1,
    'disable_existing_loggers': False,
    'handlers': {
        'console': {
            'class': 'logging.StreamHandler',
        },
    },
    'root': {
        'handlers': ['console'],
        'level': 'INFO',
    },
    'loggers': {
        'django': {
            'handlers': ['console'],
            'level': 'ERROR',
            'propagate': True,
        },
    },
}
```
