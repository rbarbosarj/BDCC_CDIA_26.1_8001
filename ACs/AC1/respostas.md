# AC1 – Computação em Nuvem

**Aluno:** Renan Barbosa
**Disciplina:** Computação em Nuvem
**Ano:** 2026

---

## 1. Questões Objetivas

### Questão 1 – Modelos de Implantação

**Resposta:** d) II e III, apenas

**Justificativa:**
A afirmativa II está correta, pois a nuvem híbrida permite a integração entre ambientes on-premises e nuvem pública.
A afirmativa III está correta, pois a nuvem comunitária é compartilhada entre organizações com interesses comuns.
A afirmativa I está incorreta, pois descreve a nuvem privada, e não a nuvem pública.

---

### Questão 2 – Região AWS

**Resposta:** c) sa-east-1 (São Paulo)

**Justificativa:**
A região de São Paulo é a mais adequada para usuários brasileiros, pois oferece menor latência e facilita a conformidade com a LGPD.

---

### Questão 3 – Característica da Nuvem

**Resposta:** b) Elasticidade

**Justificativa:**
A elasticidade permite ajustar automaticamente os recursos computacionais conforme a demanda, atendendo diretamente ao cenário apresentado.

---

### Questão 4 – Responsabilidade Compartilhada

**Resposta:** e) 1, 2 e 3

**Justificativa:**
A AWS é responsável pela segurança da infraestrutura (segurança da nuvem), enquanto o cliente é responsável pelos dados e configurações (segurança na nuvem).
Em serviços gerenciados como o Amazon RDS, a AWS também gerencia o sistema operacional.

---

### Questão 5 – Modelo Financeiro (OpEx)

**Resposta:** c) Eliminação de investimentos iniciais altos, pagando pelo uso

**Justificativa:**
O modelo OpEx elimina grandes investimentos iniciais (CapEx), permitindo pagamento conforme o consumo, o que reduz riscos financeiros.

---

## 2. Questão Discursiva 1 – Elasticidade e Alta Disponibilidade

A migração para a computação em nuvem AWS resolve o problema de indisponibilidade ao permitir escalabilidade automática dos recursos. Em ambientes on-premises, a capacidade é limitada ao hardware disponível, o que pode gerar falhas em picos de acesso.

Na AWS, a elasticidade permite que novas instâncias sejam provisionadas automaticamente durante períodos de alta demanda, como na Black Friday, garantindo a continuidade do serviço. Após o pico, os recursos são reduzidos, evitando custos desnecessários.

Além disso, arquiteturas baseadas em múltiplas zonas de disponibilidade (Multi-AZ) garantem alta disponibilidade e tolerância a falhas (fault tolerance), assegurando que o sistema permaneça operacional mesmo diante de falhas em um data center.

Dois serviços AWS recomendados são:

* Amazon EC2 com Auto Scaling: ajusta automaticamente a quantidade de instâncias conforme a demanda
* Elastic Load Balancing (ELB): distribui o tráfego entre múltiplos servidores, evitando sobrecarga

---

## 3. Questão Discursiva 2 – CapEx vs OpEx e Armazenamento

No modelo on-premises (CapEx), a empresa precisa realizar altos investimentos iniciais em infraestrutura, além de arcar com custos de manutenção e baixa flexibilidade para expansão.

Já no modelo em nuvem (OpEx), não há necessidade de investimento inicial em hardware. A empresa paga apenas pelos recursos utilizados, permitindo maior flexibilidade e escalabilidade.

A escalabilidade horizontal permite aumentar a capacidade do sistema automaticamente conforme o crescimento da base de usuários, indo de 1.000 para 100.000 usuários sem necessidade de novos investimentos em infraestrutura física.

Para armazenamento de vídeos, o serviço mais adequado é o Amazon S3, pois oferece alta durabilidade, escalabilidade praticamente ilimitada e baixo custo.

Além disso, a integração com o Amazon CloudFront (CDN) permite distribuição eficiente de conteúdo com baixa latência em todo o país, melhorando significativamente a experiência do usuário.

