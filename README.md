# Capa

- **Título do Projeto**: Sistema de Gestão Hoteleira
- **Nome do Estudante**: Gustavo José Rosa
- **Curso**: Engenharia de Software
- **Data de Entrega**: [Data]

# Resumo

Este documento apresenta a proposta de desenvolvimento de um Sistema de Gestão Hoteleira que integra reservas online (inclusive via WhatsApp), gerenciamento financeiro, dashboards analíticos em tempo real e atendimento automatizado via chatbot. O projeto utiliza tecnologias modernas, com pipeline de CI/CD para testes automatizados e deploy contínuo, e será implementado utilizando AWS para o back-end e banco de dados, enquanto o chatbot e o front-end serão deployados na Vercel.

## 1. Introdução

- **Contexto**:  
  O setor hoteleiro demanda soluções integradas que automatizem o controle de reservas, pagamentos e comunicação com os clientes. Este projeto visa modernizar a gestão hoteleira, utilizando tecnologias de ponta e práticas ágeis para oferecer uma solução robusta e escalável.

- **Justificativa**:  
  Com a concorrência crescente e a necessidade de otimizar processos, a centralização e automação dos sistemas gerenciais são essenciais para reduzir erros operacionais, melhorar a experiência dos usuários e agilizar o desenvolvimento por meio de CI/CD.

- **Objetivos**:  
  - **Principal:** Desenvolver uma plataforma web para gestão hoteleira com reservas online, controle financeiro, dashboards analíticos e atendimento via chatbot com integração para WhatsApp.  
  - **Secundários:**  
    - Implementar notificações e dashboards em tempo real.  
    - Integrar com plataformas de pagamento online.  
    - Estabelecer um pipeline de CI/CD para garantir testes automatizados, monitoramento e deploy contínuo.  
    - Implantar o back-end e o banco de dados em instâncias AWS, enquanto o chatbot e o front-end serão deployados na Vercel.

## 2. Descrição do Projeto

- **Tema do Projeto**:  
  Desenvolvimento de um Sistema de Gestão Hoteleira moderno que automatiza processos críticos, integrando reservas online, gestão financeira, dashboards e atendimento automatizado.

- **Problemas a Resolver**:  
  - Controle manual de reservas e disponibilidade dos quartos.  
  - Falta de centralização na gestão financeira e emissão de notas fiscais.  
  - Comunicação ineficiente com os clientes.  
  - Ausência de um processo automatizado que garanta qualidade e agilidade nas atualizações.

- **Limitações**:  
  - O sistema não incluirá funcionalidades específicas para gerenciamento de restaurantes ou serviços de quarto nesta versão.  
  - A integração com plataformas externas de reservas (ex.: OTAs) será considerada apenas em futuras atualizações.

## 3. Especificação Técnica

Descrição detalhada da proposta, abordando os requisitos de software, processos, integrações e a implantação em diferentes ambientes.

### 3.1. Requisitos de Software

- **Lista de Requisitos:**  
  **Requisitos Funcionais (RF):**
  1. Permitir reservas online realizadas pelos clientes, inclusive via WhatsApp.
  2. Gerenciar o cadastro de hóspedes com informações pessoais e histórico de estadias.
  3. Controlar a disponibilidade e o cadastro de quartos.
  4. Processar pagamentos online e emitir notas fiscais.
  5. Gerar relatórios financeiros e operacionais detalhados.
  6. Integrar um chatbot para atendimento automatizado, permitindo consultas e envio de análises (diárias, semanais e mensais).
  7. Enviar notificações em tempo real sobre alterações nas reservas e atualizações do sistema.
  8. Disponibilizar dashboards analíticos em tempo real para a gestão do hotel.
  9. Implementar um pipeline de CI/CD com testes automatizados (unitários, integração e regressão) e deploy automático.

  **Requisitos Não Funcionais (RNF):**
  1. Utilizar .NET para o desenvolvimento do back-end.
  2. Desenvolver o front-end com Angular ou Next.js, assegurando responsividade.
  3. Empregar MySQL ou SQL Server para o gerenciamento dos dados.
  4. Assegurar a segurança dos dados, seguindo as normas da LGPD, com criptografia e controle de acesso.
  5. Suportar múltiplos usuários simultâneos mantendo desempenho adequado.
  6. Otimizar o tempo de build e deploy com rollback imediato em caso de falhas.

- **Representação dos Requisitos:**  
![UML](./Assets/images/uml.svg)

### 3.2. Considerações de Design

- **Discussão sobre as Escolhas de Design:**  
  Foram avaliadas diversas alternativas para garantir escalabilidade, modularidade e facilidade de manutenção. A solução adotada privilegia a separação clara entre as camadas de apresentação, aplicação e dados, facilitando a integração contínua e futuras expansões.

- **Visão Inicial da Arquitetura:**  
  - **Camada de Apresentação:** Desenvolvida com Angular ou Next.js, garantindo uma interface responsiva.  
  - **Camada de Aplicação:** Lógica de negócio implementada em .NET com comunicação via APIs RESTful.  
  - **Camada de Dados:** Gerenciamento dos dados utilizando MySQL ou SQL Server.  
  - **Camada de Integração (CI/CD):** Pipeline automatizado para build, testes, deploy e monitoramento contínuo.

- **Padrões de Arquitetura:**  
  O projeto adota o padrão MVC para o back-end e práticas de Microserviços, quando aplicável, facilitando a manutenção e escalabilidade.

- **Modelos C4:**  
  A arquitetura será detalhada em quatro níveis:  
  - **Contexto:** Visão geral do sistema e seus usuários.  
  - **Contêineres:** Separação entre interface, lógica de negócio e armazenamento de dados.  
  - **Componentes:** Divisão interna dos contêineres em módulos funcionais.  
  - **Código:** Estrutura detalhada para implementação e manutenção do código.

### 3.3. Stack Tecnológica

- **Linguagens de Programação:**  
  - Back-end: C# com .NET  
  - Front-end: TypeScript/JavaScript com Angular ou Next.js

- **Frameworks e Bibliotecas:**  
  - .NET para desenvolvimento do servidor  
  - Angular ou Next.js para a interface do usuário  
  - Bibliotecas para integração de APIs RESTful e autenticação

- **Ferramentas de Desenvolvimento e Gestão de Projeto:**  
  - Versionamento: Git  
  - Gestão de projetos: Trello, Jira ou ferramenta similar  
  - Pipeline de CI/CD: GitHub Actions, GitLab CI ou Jenkins

- **Infraestrutura e Implantação:**  
  - **AWS:**  
    - O back-end e o banco de dados serão deployados em instâncias AWS, garantindo escalabilidade, segurança e alta disponibilidade.  
  - **Vercel:**  
    - O front-end e o chatbot serão deployados na Vercel, aproveitando sua capacidade de oferecer deploys rápidos e gerenciamento simplificado de aplicações front-end.

### 3.4. Considerações de Segurança

- **Medidas de Segurança:**  
  - Criptografia de dados sensíveis (tanto em trânsito quanto em repouso).  
  - Implementação de autenticação e autorização robusta.  
  - Validação de entradas para mitigar riscos de SQL Injection e Cross-Site Scripting (XSS).  
  - Adoção de práticas conformes à LGPD para garantir a privacidade dos dados dos usuários.

## 4. Próximos Passos

Após a finalização deste documento, os próximos passos incluem:
- Refinamento dos requisitos e ajustes na arquitetura com base em feedback de revisões.
- Início do desenvolvimento e configuração do ambiente de CI/CD, iniciando pela definição do pipeline e execução dos testes automatizados.
- Configuração das instâncias AWS para o back-end e banco de dados, e preparação do deploy do front-end e chatbot na Vercel.
- Planejamento detalhado das fases de desenvolvimento para Portfólio I e Portfólio II, com acompanhamento contínuo dos processos de build e deploy.

## 5. Referências

- Documentação oficial do .NET  
- Documentação do Angular / Next.js  
- Guias e tutoriais sobre CI/CD (GitHub Actions, GitLab CI, Jenkins)  
- Documentação da AWS (Amazon Web Services)  
- Documentação da Vercel  
- Normas e diretrizes da LGPD para proteção de dados

## 6. Apêndices (Opcionais)

- Diagramas UML ilustrando os Casos de Uso e a arquitetura do sistema.  
- Especificações técnicas detalhadas dos módulos e integrações.  
- Logs e métricas dos testes automatizados (quando disponíveis).

## 7. Avaliações de Professores

- Considerações Professor/a:  
- Considerações Professor/a:  
- Considerações Professor/a:
