# Sistema de Gestão Hoteleira

**Nome do Estudante:** Gustavo José Rosa
**Curso:** Engenharia de Software  
**Data de Entrega:** [Data]

## Resumo

Este documento apresenta a proposta de desenvolvimento de um Sistema de Gestão Hoteleira, abrangendo funcionalidades como reservas online, gerenciamento financeiro e integração com um chatbot para atendimento ao cliente. O sistema será desenvolvido com back-end em .NET, front-end em Angular ou Next.js, e utilizará o banco de dados MySQL ou SQL Server.

## 1. Introdução

### 1.1. Contexto

A gestão eficiente de hotéis requer sistemas integrados que facilitem o controle de reservas, pagamentos e comunicação com os clientes. A adoção de tecnologias modernas pode otimizar esses processos, proporcionando uma melhor experiência tanto para os administradores quanto para os hóspedes.

### 1.2. Justificativa

A implementação de um sistema integrado de gestão hoteleira visa automatizar processos manuais, reduzir erros operacionais e melhorar a satisfação dos clientes através de um atendimento mais ágil e personalizado.

### 1.3. Objetivos

- **Objetivo Principal:** Desenvolver um sistema web para gestão de hotéis que inclua funcionalidades de reserva, controle financeiro e atendimento ao cliente via chatbot.
- **Objetivos Secundários:**
  - Implementar notificações em tempo real para atualizações de reservas.
  - Integrar o sistema com plataformas de pagamento online.
  - Fornecer relatórios gerenciais para apoio à tomada de decisão.

## 2. Descrição do Projeto

### 2.1. Tema do Projeto

Desenvolvimento de um Sistema de Gestão Hoteleira com funcionalidades de reservas online, gerenciamento financeiro e integração com chatbot para atendimento ao cliente.

### 2.2. Problemas a Resolver

- Dificuldade no controle manual de reservas e disponibilidade de quartos.
- Falta de um sistema centralizado para gestão financeira e emissão de notas fiscais.
- Comunicação ineficiente com os clientes, resultando em baixa satisfação.

### 2.3. Limitações

- O sistema não incluirá funcionalidades de gerenciamento de restaurantes ou serviços de quarto.
- A integração com plataformas externas de reservas (como OTAs) não será contemplada nesta versão.

## 3. Especificação Técnica

### 3.1. Requisitos de Software

#### 3.1.1. Requisitos Funcionais (RF)

1. **RF01:** O sistema deve permitir que clientes realizem reservas online.
2. **RF02:** O sistema deve gerenciar o cadastro de hóspedes, incluindo informações pessoais e histórico de estadias.
3. **RF03:** O sistema deve controlar a disponibilidade e o cadastro de quartos.
4. **RF04:** O sistema deve processar pagamentos online e emitir notas fiscais.
5. **RF05:** O sistema deve fornecer relatórios financeiros e operacionais.
6. **RF06:** O sistema deve integrar-se a um chatbot para atendimento ao cliente.
7. **RF07:** O sistema deve enviar notificações em tempo real sobre alterações nas reservas.

#### 3.1.2. Requisitos Não Funcionais (RNF)

1. **RNF01:** O sistema deve ser desenvolvido utilizando a plataforma .NET para o back-end.
2. **RNF02:** O front-end deve ser implementado em Angular ou Next.js.
3. **RNF03:** O banco de dados utilizado deve ser MySQL ou SQL Server.
4. **RNF04:** O sistema deve garantir a segurança dos dados dos usuários, seguindo as normas da LGPD.
5. **RNF05:** O sistema deve ser responsivo, adaptando-se a diferentes dispositivos e tamanhos de tela.
6. **RNF06:** O sistema deve suportar múltiplos usuários simultaneamente, garantindo desempenho adequado.

### 3.2. Representação dos Requisitos

**Diagrama de Casos de Uso:**

*Nota: Devido às limitações de texto, a representação gráfica do diagrama de casos de uso não pode ser exibida aqui. Recomenda-se utilizar uma ferramenta de modelagem UML para criar o diagrama, incluindo atores como "Cliente", "Administrador" e "Chatbot", e casos de uso correspondentes aos requisitos funcionais listados.*

### 3.3. Arquitetura do Sistema

O sistema será desenvolvido seguindo a arquitetura de três camadas:

1. **Camada de Apresentação:** Implementada com Angular ou Next.js, responsável pela interface com o usuário.
2. **Camada de Aplicação:** Back-end desenvolvido em .NET, contendo a lógica de negócios.
3. **Camada de Dados:** Banco de dados MySQL ou SQL Server para armazenamento das informações.

### 3.4. Integração com Chatbot

O chatbot será integrado ao sistema para fornecer atendimento automatizado aos clientes, permitindo consultas sobre disponibilidade de quartos, realização de reservas e esclarecimento de dúvidas comuns. A comunicação entre o chatbot e o sistema será realizada através de APIs RESTful.

### 3.5. Segurança

Serão implementadas medidas de segurança, incluindo:

- Criptografia de dados sensíveis.
- Autenticação e autorização de usuários.
- Validação de entradas para prevenir ataques como SQL Injection e Cross-Site Scripting (XSS).

## 4. Cronograma

| Fase                         | Descrição                                         | Duração Estimada |
|------------------------------|---------------------------------------------------|------------------|
| **1. Levantamento de Requisitos** | Coleta e análise das necessidades do sistema.     | 2 semanas        |
| **2. Design do Sistema**          | Definição da arquitetura e modelagem dos componentes. | 3 semanas        |
| **3. Desenvolvimento do Back-end**| Implementação da lógica de negócios em .NET.      | 4 semanas        |
| **4. Desenvolvimento do Front-end**| Criação da interface do usuário em Angular ou Next.js. | 4 semanas        |
| **5. Integração com Chatbot**     | Desenvolvimento e integração do chatbot ao sistema. | 3 semanas        |
| **6. Testes e Validação**         | Verificação e validação de todas as funcionalidades. | 2 semanas        |
| **7. Implantação**                | Deploy do sistema em ambiente de produção.        | 1 semana         |

## 5. Considerações Finais

Este projeto visa fornecer uma solução completa para a gestão hoteleira, integrando funcionalidades essenciais em uma plataforma única e moderna. A utilização de tecnologias como .NET, Angular/Next.js e MySQL/SQL Server garantirá a robustez e escalabilidade necessárias para atender às demandas do setor.

# A se pensar
- Chat-bot:
    - Envio de analises diarias/semanais/mensais para o usuário (Dono do hotel)
    - Acompanhamento mensal pelo chat-bot
    - Ser possivel realizar a reserva pelo whats-app
