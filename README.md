# Capa

- **Título do Projeto**: RabbitSuites – Gestão de Quartos, Reservas e Emissão de NF-e com Chatbot WhatsApp  
- **Nome do Estudante**: Gustavo José Rosa  
- **Curso**: Engenharia de Software  
- **Data de Entrega**: [Data]

# Resumo

Este documento apresenta a proposta de desenvolvimento de **RabbitSuites**, uma plataforma web para gestão hoteleira que unifica:

- Controle de quartos e reservas (via web e WhatsApp, usando Baileys)  
- Fila de mensagens RabbitMQ para orquestração de criação/consulta de reservas  
- Automação diária: envio de e-mail de NF-e ao hóspede e lembrete de check-out  
- Dashboards analíticos em tempo real  
- Emissão de notas fiscais eletrônicas (NF-e)  *A se pensar na possibilidade - fazer piloto com só um hotel*
- Sistema de **Token Bucket** para limitação de taxa em chamadas críticas  
- **Circuit Breaker** para isolar falhas em serviços externos (NF-e, gateway de pagamento)  
- **Caching Distribuído** (Redis) para acelerar consultas caras e projeções de dashboard  
- Chatbot no WhatsApp para reservas e alertas  
- Área administrativa para dono/gestor do hotel  

O pipeline de CI/CD garante testes automatizados e deploy contínuo: back-end e banco em AWS; front-end e chatbot na Vercel.

## 1. Introdução

- **Contexto**  
  O setor hoteleiro exige soluções de alta disponibilidade e performance para suportar picos de acesso (web e WhatsApp), emissão de documentos fiscais e comunicação em tempo real.  

- **Justificativa**  
  Com grande volume de mensagens e integrações externas, adotamos RabbitMQ, Token Bucket e Circuit Breaker para resiliência, além de Caching Distribuído para reduzir latência e carga no banco de dados.  

- **Objetivos**  
  - **Principal:** Desenvolver plataforma integrada para gestão de quartos, reservas, NF-e e atendimento via chatbot.  
  - **Secundários:**  
    - Orquestrar mensagens com RabbitMQ.  
    - Controlar taxa de requisições críticas (Token Bucket).  
    - Isolar falhas externas (Circuit Breaker).  
    - Acelerar consultas com Redis (Caching).  
    - Criar automações diárias de e-mail.

## 2. Descrição do Projeto

- **Tema do Projeto**  
  Plataforma de Gestão Hoteleira com filas de mensagens, automações, resiliência e chatbot WhatsApp.

- **Problemas a Resolver**  
  1. Picos de tráfego de criação/consulta de reservas causando lentidão.  
  2. Falhas em serviços externos (emissão de NF-e, gateway de pagamento) impactando a experiência.  
  3. Consultas repetitivas ao banco gerando alta latência nos dashboards.  
  4. Necessidade de automação no envio de NF-e e lembretes de diária.

- **Limitações**  
  - Integração com OTAs e serviços de quarto ficam para versões futuras.  
  - Circuit Breaker cobre apenas serviços críticos de NF-e e pagamento.  
  - Cache será atualizado por TTL ou invalidação programada.

## 3. Especificação Técnica

### 3.1 Requisitos de Software

**Requisitos Funcionais (RF)**  
1. Fila RabbitMQ para orquestração de criação/consulta de reservas.  
2. Enfileirar e processar eventos de geração de NF-e.  
3. Automação diária de envio de e-mail de NF-e para hóspedes.  
4. Automação diária de lembrete de check-out por e-mail.  
5. Limitação de taxa de chamadas críticas via Token Bucket.  
6. Isolamento de falhas externas via Circuit Breaker.  
7. Caching Distribuído para consultas de disponibilidade e projeções de dashboard.  
8. Reservas online (web e WhatsApp via Baileys).  
9. Cadastro de hóspedes e funcionários.  
10. Emissão de NF-e integrada ao ERP fiscal.  
11. Dashboards em tempo real com dados cacheados.  
12. Área de gestão via painel (dono/gestor).

**Requisitos Não Funcionais (RNF)**  
1. Back-end (.NET 7 em C#), front-end (Angular/Next.js).  
2. Banco de dados MySQL ou SQL Server.  
3. RabbitMQ como broker de mensagens.  
4. Serviço de e-mail (SMTP ou SendGrid).  
5. Redis para Token Bucket e Caching Distribuído.  
6. Circuit Breaker implementado com Polly (ou biblioteca similar).  
7. LGPD: criptografia AES-256 em repouso e TLS 1.2+ em trânsito.  
8. CI/CD com rollback automático (GitHub Actions).  
9. Escalabilidade via Docker/Kubernetes em AWS.

### 3.2 Considerações de Design

- **Mensageria (RabbitMQ)**  
  - Produtor (API de reservas/NF-e) publica mensagens em filas `reservas` e `notificações`.  
  - Consumer workers processam filas para persistência e envio de e-mail.

- **Rate Limiting (Token Bucket)**  
  - Middleware .NET insere tokens num bucket por intervalo, bloqueando requisições sem token.

- **Circuit Breaker**  
  - Middleware que monitora falhas em chamadas a serviços externos (NF-e, pagamento).  
  - Após número de erros configurado, dispara “open state” e aplica fallback ou mensagem amigável ao usuário.  
  - Biblioteca sugerida: Polly.

- **Caching Distribuído**  
  - Redis para cache de disponibilidade de quartos, projeções de dashboard e respostas frequentes do chatbot.  
  - TTL configurável por entidade e invalidação via eventos (ex.: nova reserva).

- **Automação Diária**  
  - Hangfire (ou cronexpression) agenda jobs que:  
    1. Verificam NF-e geradas, enviam e-mail ao hóspede.  
    2. Verificam check-outs do dia, disparam lembrete por e-mail.

- **C4 Views**  
  - **Contexto:** Usuários (hóspedes, funcionários, gestor) e sistemas externos (ERP fiscal).  
  - **Contêineres:** API Gateway, Serviços de Reserva, Faturamento, Notificações, Chatbot; Redis; RabbitMQ; Banco de Dados.  
  - **Componentes:** Módulos de mensageria, throttle, circuito, cache, jobs agendados.  
  - **Código:** Organização em camadas (API, Serviços, Repositório, Workers).

### 3.3 Stack Tecnológica

- **Back-end:**  
  - .NET 7 (C#)  
  - RabbitMQ  
  - Polly (Circuit Breaker)  
  - Hangfire ou cronexpression  
  - Redis (Token Bucket + Cache)  

- **Front-end:**  
  - Angular 14 ou Next.js  
  - Vercel (deploy)  

- **Chatbot:**  
  - Node.js + NestJS + Baileys  

- **Banco de Dados:**  
  - MySQL ou SQL Server (RDS na AWS)  

- **Infraestrutura:**  
  - AWS (ECS/EKS, RDS, S3)  
  - GitHub Actions para CI/CD  

- **E-mail:**  
  - SendGrid ou SMTP empresarial  

### 3.4 Considerações de Segurança

- **Rate Limiting & Resiliência:** Token Bucket e Circuit Breaker protegem a API de overload e falhas externas.  
- **Mensageria Segura:** SSL/TLS em RabbitMQ e Redis.  
- **Proteção de Dados:** LGPD, criptografia AES-256 em repouso e TLS 1.2+ em trânsito.  
- **Validação Rigorosa:** Sanitização de inputs e tratamento centralizado de exceções.

## 4. Próximos Passos

1. Provisionar **Redis** e **RabbitMQ** em ambiente de dev.  
2. Implementar middleware de **Token Bucket** e **Circuit Breaker** na API.  
3. Criar workers para processamento de filas e jobs agendados (Hangfire).  
4. Configurar **caching** de consultas críticas no Redis.  
5. Ajustar pipeline CI/CD para testes de resiliência (caixas d’água) e performance de cache.  
6. Realizar testes de carga simulando falhas externas e picos de reservas.  
7. Documentar diagramas C4 e sequência de fallback do Circuit Breaker.

## 5. Referências

- RabbitMQ Documentation  
- Polly Circuit Breaker Patterns  
- Redis Caching Strategies  
- Baileys WhatsApp Web API Guide  
- LGPD: Lei Geral de Proteção de Dados  

## 6. Apêndices (Opcionais)

- Diagrama UML de sequência para fluxo de reservas em RabbitMQ e fallback de Circuit Breaker.  
- Exemplo de C4 Container Diagram detalhado.

## 7. Avaliações de Professores

- **Considerações Professor/a:**  
- **Considerações Professor/a:**  
- **Considerações Professor/a:**  
