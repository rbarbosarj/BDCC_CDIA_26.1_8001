# AC04 – Deploy de Aplicações Django na AWS

**Aluno:** Renan Barbosa
**Curso:** Engenharia de Computação
**Disciplina:** Big Data & Cloud Computing
**Avaliação:** AC04
**Período:** 2026.1

---

# Questões Objetivas

## Questão 1 – Conceitos de Elastic Beanstalk

### Resposta:

**b) AWS Elastic Beanstalk**

### Justificativa:

O AWS Elastic Beanstalk é a solução mais adequada porque abstrai grande parte do gerenciamento de infraestrutura, permitindo deploy simplificado de aplicações Django REST.

O serviço integra automaticamente:

* Amazon EC2
* Elastic Load Balancer
* Auto Scaling Groups
* CloudWatch
* Monitoramento e deploy

Além disso, o Elastic Beanstalk reduz significativamente a complexidade operacional, permitindo que o desenvolvedor foque na aplicação e não na infraestrutura.

---

## Questão 2 – Conceitos de Elastic Beanstalk

### Resposta:

**b) AWS Elastic Beanstalk**

### Justificativa:

A questão apresenta o mesmo cenário da Questão 1. O Elastic Beanstalk continua sendo a solução mais adequada devido à capacidade de provisionamento automatizado, escalabilidade automática e integração nativa com serviços AWS.

---

## Questão 3 – Configuração de Variáveis de Ambiente

### Resposta:

**b) Opção B**

### Justificativa:

A Opção B utiliza variáveis de ambiente através do módulo `os.environ.get()`, seguindo boas práticas de segurança e configuração em aplicações cloud.

Essa abordagem:

* evita credenciais hardcoded
* facilita deploy entre ambientes
* melhora segurança
* simplifica gerenciamento no Elastic Beanstalk

Exemplo:

```python
import os

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
```

---

## Questão 4 – Configuração do S3 com django-storages

### Resposta:

**c) Opção C**

### Justificativa:

A Opção C apresenta a configuração mais completa para integração do Django com Amazon S3 utilizando `django-storages`.

Ela inclui:

* credenciais AWS
* bucket S3
* região AWS
* parâmetros de cache
* backend correto do storage

Essa configuração permite armazenamento persistente e distribuído de arquivos de mídia em ambientes cloud escaláveis.

---

## Questão 5 – Armazenamento Persistente com S3

### Resposta:

**b) Utilizar Amazon S3 com django-storages**

### Justificativa:

Instâncias do Elastic Beanstalk são efêmeras, podendo ser recriadas durante:

* deploys
* auto scaling
* substituição de instâncias
* falhas de infraestrutura

Portanto, armazenar arquivos localmente não garante persistência.

O Amazon S3 é a solução ideal porque oferece:

* alta durabilidade
* escalabilidade
* persistência
* integração nativa com AWS
* alta disponibilidade
