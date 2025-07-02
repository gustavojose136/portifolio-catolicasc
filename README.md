# Capa

- **Título do Projeto**: AvenSuites – Gestão de Quartos, Reservas e Emissão de NF-e com Chatbot WhatsApp
- **Nome do Estudante**: Gustavo José Rosa
- **Curso**: Engenharia de Software
- **Data de Entrega**: [Data]

# Resumo

Gerenciar quartos, reservas, faturamento e atendimento em canais distintos é um processo fragmentado e sujeito a erros. Equipes hoteleiras perdem tempo sincronizando sistemas, emitindo NF-e manualmente e enviando lembretes, enquanto a satisfação do hóspede cai devido a confirmações tardias e falta de informações centralizadas.

**AvenSuites** surge para acabar com esse gargalo. Nossa plataforma unificada oferece:

- **Reservas 24/7** via web e WhatsApp (Baileys)
- **Emissão automática de NF-e** com integração direta ao ERP fiscal
- **Notificações inteligentes**: e-mail de NF-e e lembrete de check-out
- **Orquestração segura** com RabbitMQ, Token Bucket e Circuit Breaker
- **Dashboards em tempo real** alimentados por cache Redis
- **Deploy contínuo** (AWS/Vercel) com CI/CD e conformidade à LGPD

Importante ressaltar: o AvenSuites não é um CRM completo — nosso foco é otimizar a operação hoteleira, eliminando retrabalho e centralizando todo o fluxo de reservas e faturamento em um único painel.

---

## 1. Introdução

No mercado hoteleiro atual, velocidade e consistência são requisitos básicos para fidelizar hóspedes. Ainda assim, muitos estabelecimentos dependem de múltiplas planilhas, sistemas de reserva isolados e processos manuais de emissão fiscal — gerando retrabalho, erros contábeis e experiência fragmentada.

O **AvenSuites** foi desenhado para resolver esses pontos de dor. A plataforma centraliza em um só lugar:

- Gestão de quartos e reservas com sincronização instantânea
- Chatbot WhatsApp (Baileys) para interação ágil e notificações
- Emissão eletrônica de NF-e automatizada
- Orquestração de mensagens com RabbitMQ e limitação de taxa (Token Bucket)
- Resiliência garantida por Circuit Breaker em serviços externos
- Aceleramento de consultas por meio de Caching Distribuído (Redis)

Com AvenSuites, você reduz custos operacionais, elimina integrações manuais e entrega uma experiência de ponta a ponta ao hóspede — do check-in ao pós-venda.

## 2. Descrição do Projeto

**AvenSuites** é uma plataforma unificada, projetada para centralizar e automatizar todo o fluxo hoteleiro — da reserva à emissão de NF-e e pós-venda — em um único painel. Com integração via web e WhatsApp (Baileys), orquestração assíncrona (RabbitMQ), cache Redis e mecanismos de resiliência (Circuit Breaker e Token Bucket), entregamos uma solução robusta e escalável para hotéis que buscam alta disponibilidade e eficiência operacional.

O projeto nasceu para resolver problemas comuns e custosos no dia a dia hoteleiro:

- **Processos fragmentados** entre sistemas de reserva e canais de venda, que geram retrabalho e erros de disponibilidade.
- **Emissão manual de NF-e**, sujeita a falhas e atrasos contábeis.
- **Picos de acesso** que sobrecarregam servidores e afetam a experiência do hóspede.
- **Comunicação ineficiente**, sem lembretes automatizados ou notificações centralizadas.

**AvenSuites** elimina essas barreiras ao automatizar e orquestrar:

- **Reservas e disponibilidade** em tempo real, sincronizadas via web e WhatsApp.
- **Geração e envio de NF-e** diretamente para o ERP fiscal, com notificação instantânea ao hóspede.
- **Notificações diárias** por e-mail (check-out, confirmação de pagamento, lembretes).
- **Dashboards responsivos** alimentados por cache Redis, para monitoramento imediato.

É importante notar: AvenSuites não é um sistema de gestão completa de propriedades (PMS) nem um CRM. Nosso foco estratégico é otimizar reservas, faturamento e comunicação automatizada, reduzindo custos operacionais e permitindo que sua equipe se concentre na hospitalidade.

# 3. Especificação Técnica

### 3.. Resumo

O **AvenSuites** é uma plataforma multi‐serviço orientada a eventos, construída para orquestrar reservas, faturamento e notificações em hotéis. Cada microserviço segue DDD, expondo APIs REST/AMQP e publicando/consumindo eventos via RabbitMQ; o cache Redis e os mecanismos de resiliência (Token Bucket e Circuit Breaker) garantem performance e disponibilidade.

### 3.2. Componentes Principais do Sistema

. **API Gateway**

   - Roteia chamadas REST e WebSocket para os microserviços
   - Aplica autenticação, autorização e throttling (Token Bucket)

2. **Reservation Service (.NET)**

   - Domínio “Reserva” (Aggregates, Entities, Domain Events)
   - Persiste no MySQL via EF Core
   - Publica `BookingCreated` em RabbitMQ

3. **Room Service (.NET)**

   - Domínio “Quarto” e “Disponibilidade”
   - Cache Redis para consultas de disponibilidade

4. **Billing Service (.NET)**

   - Geração de NF-e e integração SOAP/REST com ERP fiscal
   - Publica `InvoiceCreated` em RabbitMQ

5. **Notification Worker (.NET + Hangfire)**

   - Consome filas de eventos (`BookingCreated`, `InvoiceCreated`)
   - Envia e-mails de confirmação, NF-e e lembrete de check-out

6. **Chatbot Service (NestJS + Baileys)**

   - Interface WhatsApp para reservas e alertas
   - Publica comandos em RabbitMQ e consulta Redis para estado de diálogo

7. **Infraestrutura de Mensageria e Cache**
   - **RabbitMQ**: filas `reservas`, `faturamento`, `notificações`
   - **Redis**: cache de dados quentes e Token Bucket distribuído

### 3.3. Requisitos de Software

### Módulo de Reserva
- **RF-RES-00**: Criar uma nova reserva via API REST/WhatsApp (Baileys).  
- **RF-RES-002**: Consultar disponibilidade de quartos em tempo real.  
- **RF-RES-003**: Listar reservas existentes por hóspede, data ou status.  
- **RF-RES-004**: Atualizar/cancelar reserva, com publicação de evento `BookingCancelled` em RabbitMQ.  

### Módulo de Quarto
- **RF-ROOM-00**: Cadastrar e editar informações de quartos (número, tipo, tarifa).  
- **RF-ROOM-002**: Marcar quarto como “manutenção” ou “indisponível”.  
- **RF-ROOM-003**: Consultar status de ocupação e histórico de ocupações.  
- **RF-ROOM-004**: Receber e processar eventos de reserva para atualizar cache Redis.  

### Módulo de Faturamento (Billing)
- **RF-BIL-00**: Gerar Nota Fiscal Eletrônica (NF-e) a partir de reserva confirmada.  
- **RF-BIL-002**: Publicar evento `InvoiceCreated` em RabbitMQ após emissão de NF-e.  
- **RF-BIL-003**: Integrar via SOAP/REST com ERP fiscal para validação e protocolo de NF-e.  
- **RF-BIL-004**: Permitir consulta de XML/PDF da NF-e pelo painel administrativo.  

### Módulo de Notificações
- **RF-NOT-00**: Consumir eventos `BookingCreated` e enviar e-mail de confirmação.  
- **RF-NOT-002**: Consumir eventos `InvoiceCreated` e enviar e-mail com NF-e ao hóspede.  
- **RF-NOT-003**: Agendar lembretes diários de check-out para hóspedes.  
- **RF-NOT-004**: Expor endpoint para reenvio manual de notificações, se necessário.  

### Módulo de Chatbot (WhatsApp)
- **RF-CBT-00**: Receber e interpretar comandos de reserva via WhatsApp (Baileys).  
- **RF-CBT-002**: Informar ao hóspede o status da reserva (confirmada, pendente, cancelada).  
- **RF-CBT-003**: Enviar alertas e lembretes via WhatsApp (check-in, check-out, NF-e).  
- **RF-CBT-004**: Publicar comandos/eventos em RabbitMQ para outros serviços consumirem.  

### Módulo Administrativo (Painel)
- **RF-ADM-00**: Tela de login/autenticação com roles (funcionário, gestor).  
- **RF-ADM-002**: Dashboard de KPIs (ocupação, receita, reservas por período).  
- **RF-ADM-003**: CRUD completo de hóspedes, funcionários e configurações de hotel.  
- **RF-ADM-004**: Filtros avançados (data, status, tipo de quarto) em listas de reservas e faturas.  


### 3.4. Considerações de Design

- Organização em microserviços DDD, desacoplados por filas e geridos via API Gateway.

### 3.5. Stack Tecnológica

- **Back-end:** .NET (C#), EF Core, Polly (Circuit Breaker)
- **Chatbot:** NestJS + Baileys
- **Broker:** RabbitMQ
- **Cache & Throttle:** Redis
- **Database:** MySQL/SQL Server
- **Agendamento:** Hangfire ou cronexpression
- **Infra & CI/CD:** Docker, Kubernetes, GitHub Actions, AWS, Vercel

### 3.6. Considerações de Segurança

- Circuit Breaker + Retry + Timeout
- Token Bucket distribuído
- TLS .2+ e AES-256
- Compliance LGPD

---

## 3.7. Modelos C4

### 3.7.1. Diagrama de Contexto (C4 Nível 1)

```mermaid
C4Context
    title AvenSuites – Contexto do Sistema

    Person(hospede, "Hóspede", "Reserva quartos e recebe confirmações")
    Person(funcionario, "Funcionário", "Gerencia quartos, reservas e NF-e")
    Person(gestor, "Gestor/Dono", "Monitora indicadores e relatórios")

    System_Boundary(avensuites, "AvenSuites") {
        System(webApp, "Web App", "Next.js – Interface para funcionários e gestores")
        System(chatbot, "Chatbot WhatsApp", "NestJS + Baileys – Atendimento e reservas")
        System(api, "API Gateway + Microservices", ".NET 7 – Lógica de domínio e orquestração")
        System(emailSvc, "Serviço de E-mail", "SMTP/SendGrid – Envio de notificações e NF-e")
        System(nfApi, "API de NFS-e Municipal", "SOAP/REST – Emissão de notas fiscais eletrônicas")
        System(aws, "Infraestrutura AWS", "ECS/EKS, RDS, S3, VPC")
    }

    Rel(hospede, chatbot, "Faz reservas e recebe alertas")
    Rel(hospede, webApp, "Faz reservas e consulta histórico")
    Rel(funcionario, webApp, "CRUD quartos, reservas e consulta NF-e")
    Rel(gestor, webApp, "Visualiza dashboards e relatórios")
    Rel(webApp, api, "Chama APIs REST")
    Rel(chatbot, api, "Chama APIs REST / publica eventos AMQP")
    Rel(api, emailSvc, "Envia e-mails")
    Rel(api, nfApi, "Solicita emissão de NF-e")
    Rel(api, aws, "Armazena dados e executa contêineres")

```

### 3.7.2. Diagrama de Contêineres (C4 Nível 2)

```mermaid
C4Container
    title AvenSuites – Contêineres

    /* Front-end */
    Container(webApp, "Web App", "Next.js", "Interface para funcionários e gestores")

    /* Chatbot */
    Container(chatbotSvc, "Chatbot Service", "NestJS + Baileys", "Atende WhatsApp e publica eventos")

    /* Back-end */
    Container(apiGateway, "API Gateway", "Ocelot/Kong", "Roteia requisições REST/WS e aplica rate limiting")
    Container(reservationSvc, "Reservation Service", ".NET 7", "Gerencia reservas; publica eventos AMQP")
    Container(roomSvc, "Room Service", ".NET 7", "Gerencia quartos; atualiza cache de disponibilidade")
    Container(billingSvc, "Billing Service", ".NET 7", "Gera NF-e; integra com API municipal de NFS-e")
    Container(notificationSvc, "Notification Worker", ".NET 7 + Hangfire", "Consome filas e envia e-mails")

    /* Sistemas Externos */
    Container(emailSvc, "Serviço de E-mail", "SMTP/SendGrid", "Envia notificações e NF-e")
    Container(nfApi, "API de NFS-e Municipal", "SOAP/REST", "Emissão de notas fiscais eletrônicas")
    Container(aws, "AWS", "ECS/EKS, RDS, S3", "Infraestrutura cloud")

    /* Infraestrutura Interna */
    ContainerDb(mysql, "MySQL/RDS", "MySQL", "Banco de dados relacional")
    ContainerDb(rabbit, "RabbitMQ", "AMQP broker", "Filas de eventos")
    ContainerDb(redis, "Redis", "Redis", "Cache distribuído e Token Bucket")

    /* Relacionamentos */
    Rel(webApp, apiGateway, "REST")
    Rel(chatbotSvc, apiGateway, "REST / AMQP")
    Rel(apiGateway, reservationSvc, "gRPC/REST")
    Rel(apiGateway, roomSvc, "gRPC/REST")
    Rel(apiGateway, billingSvc, "gRPC/REST")
    Rel(apiGateway, notificationSvc, "REST (jobs)")

    Rel(reservationSvc, rabbit, "publica BookingCreated")
    Rel(billingSvc, rabbit, "publica InvoiceCreated")
    Rel(notificationSvc, rabbit, "consome eventos")
    Rel(reservationSvc, mysql, "CRUD via EF Core")
    Rel(roomSvc, redis, "lê/grava cache de disponibilidade")
    Rel(billingSvc, nfApi, "SOAP/REST para emissão de NF-e")
    Rel(notificationSvc, emailSvc, "SMTP para envio de e-mail")
    Rel(notificationSvc, aws, "armazena relatórios em S3")

```

### 3.7.3. Diagrama de Componentes (C4 Nível 3) – Reservation Service

```mermaid
C4Component
    title Reservation Service – Componentes Internos

    Container(reservationSvc, "Reservation Service", ".NET 7", "Gerencia reservas e publica eventos")

    Component(controller, "ReservationController", "ASP.NET Core", "Expõe endpoints REST para reservas")
    Component(useCases, "ReservationUseCases", ".NET Classes", "Casos de uso: Create, Cancel, QueryAvailability")
    Component(domain, "Domain Model", ".NET Classes", "Entidades: Booking, Value Objects, Domain Events")
    Component(repo, "ReservationRepository", "EF Core", "Persistência de reservas no MySQL")
    Component(publisher, "BookingPublisher", "MassTransit/RabbitMQ", "Publica eventos BookingCreated")
    Component(throttle, "TokenBucketMiddleware", "ASP.NET Core Middleware", "Limita taxa de requisições")
    Component(circuit, "CircuitBreakerMiddleware", "Polly", "Isola falhas em chamadas externas")

    /* Recursos externos */
    ContainerDb(rabbit, "RabbitMQ", "AMQP broker", "Filas de eventos")
    ContainerDb(mysql, "MySQL/RDS", "MySQL", "Banco de dados relacional")

    /* Fluxo e relacionamentos */
    Rel(controller, throttle, "Passa requisições")
    Rel(controller, circuit, "Passa requisições")
    Rel(controller, useCases, "Invoca casos de uso")
    Rel(useCases, domain, "Aplica regras de negócio")
    Rel(useCases, repo, "Persiste/consulta dados")
    Rel(useCases, publisher, "Publica BookingCreated")
    Rel(repo, mysql, "CRUD")
    Rel(publisher, rabbit, "Envia mensagens")


```

## 4. Próximos Passos


## 5. Referências

- RabbitMQ Documentation
- Polly Circuit Breaker Patterns
- Redis Caching Strategies
- Baileys WhatsApp Web API Guide
- LGPD: Lei Geral de Proteção de 

## 6. Avaliações de Professores

- **Considerações Professor/a:**
- **Considerações Professor/a:**
- **Considerações Professor/a:**
