# Especificação Funcional do Projeto: ERP Core & Expansion Roadmap

## 1. Visão Geral e Estratégia
Este documento define as especificações funcionais para o **ERP Core** e estabelece o roteiro de evolução técnica do produto. O projeto está dividido em duas macro-etapas:
1.  **ERP Core (Fase Atual):** Implementação de um backend monolítico modular focado em regras de negócio complexas, integridade transacional e arquitetura limpa.
2.  **Roadmap de Expansão (Fase Futura):** Evolução para uma arquitetura distribuída, com interfaces web, automação de infraestrutura e inteligência de dados.

> **Nota:** A execução das fases de expansão (Seção 6) está condicionada à conclusão e estabilização completa dos requisitos do ERP Core (Seções 2 a 5).

---

## 2. Escopo do Negócio (ERP Core)
O sistema deve gerenciar o ciclo de vida de produtos, desde o cadastro e precificação, passando pela movimentação de estoque multilocais, até a efetivação de pedidos de venda com cálculo tributário dinâmico.

---

## 3. Requisitos Funcionais (Core)

### 3.1. Gestão de Catálogo e Produtos
* **RF001 - Cadastro de Produto:** Deve incluir SKU, descrição, unidade de medida, NCM e peso bruto/líquido.
* **RF002 - Versionamento de Preço:** Manter histórico auditável de alterações de preço de venda.
* **RF003 - Bloqueio Lógico:** Produtos inativos não podem ser movimentados, mas devem constar em históricos.

### 3.2. Motor de Estoque e Armazenagem
* **RF004 - Controle Multilocal:** Suporte a múltiplos armazéns (Multi-Warehouse).
* **RF005 - Movimentação Transacional (Kardex):** Registro imutável de entradas e saídas contendo data, tipo, quantidade e custo unitário.
* **RF006 - Custo Médio Ponderado:** Recálculo automático do custo médio a cada entrada.
* **RF007 - Controle de Concorrência:** Prevenção de "overselling" através de validação de saldo atômica.

### 3.3. Motor Fiscal e Tributário
* **RF008 - Regra de Tributação Dinâmica:** Cálculo baseado na matriz: Estado Origem × Estado Destino × NCM.
* **RF009 - Persistência de Memória de Cálculo:** Gravação do "snapshot" das alíquotas utilizadas no momento da venda.

### 3.4. Gestão de Pedidos
* **RF010 - Ciclo de Vida:** Estados estritos (Rascunho -> Validado -> Aprovado -> Faturado -> Cancelado).
* **RF011 - Validação de Regras:** Bloqueio de pedidos para clientes inadimplentes ou com margem de lucro insuficiente.
* **RF012 - Idempotência:** Garantia de não duplicação de baixas de estoque em requisições repetidas.

---

## 4. Requisitos Não-Funcionais (Core)
* **RNF001 - Consistência ACID:** Atomicidade em operações financeiras e de estoque.
* **RNF002 - Performance de Leitura:** Otimização de relatórios de posição de estoque.
* **RNF003 - Isolamento de Domínio:** Encapsulamento estrito das regras de negócio.
* **RNF004 - Auditoria:** Logs detalhados (User, Timestamp, OldValue, NewValue) para entidades críticas.

---

## 5. Relatórios Gerenciais (Core)
1.  **Curva ABC:** Classificação de relevância de faturamento (80/15/5).
2.  **Giro de Estoque:** Análise de velocidade de saída vs. saldo médio.
3.  **Extrato Kardex:** Visão cronológica de movimentações.

---

## 6. Planejamento de Expansão (Pós-Core)

Esta seção detalha os módulos de expansão a serem desenvolvidos após a entrega da Fase Core.

### 6.1. Expansão I: Interface Web (Full Stack)
**Objetivo:** Prover uma interface visual para operação do ERP, transformando o projeto em uma solução Full Stack.
* **EXP001 - Aplicação SPA (Single Page Application):** Desenvolvimento de interface (Angular ou React) para consumo da API existente.
* **EXP002 - Dashboard Executivo:** Visualização gráfica dos relatórios Core (Curva ABC e Giro de Estoque).
* **EXP003 - Tratamento de Erros de Negócio:** Implementação de interceptadores no frontend para exibir notificações amigáveis baseadas no padrão `ProblemDetails` da API.

### 6.2. Expansão II: Infraestrutura e DevOps (Cloud Native)
**Objetivo:** Migrar o ambiente de desenvolvimento local para um cenário de produção em nuvem com automação.
* **EXP004 - Pipeline de CI (Integração Contínua):** Configuração de workflows (GitHub Actions) para execução automática de testes unitários e build a cada commit.
* **EXP005 - Pipeline de CD (Entrega Contínua):** Automação do deploy da aplicação em provedor de nuvem (AWS ou Azure) utilizando containers.
* **EXP006 - Infraestrutura como Código (IaC):** Provisionamento de recursos de banco de dados e rede utilizando Terraform.

### 6.3. Expansão III: Arquitetura Distribuída e Inteligência de Dados
**Objetivo:** Desacoplar processos críticos e implementar inteligência analítica avançada.
* **EXP007 - Processamento Assíncrono (Event-Driven):** Implementação de mensageria (RabbitMQ/Kafka) para o cálculo fiscal. O pedido é recebido e processado em segundo plano para aumentar a resiliência.
* **EXP008 - ETL de Dados (Python):** Scripts para extração e transformação de dados do banco transacional para formato analítico (Parquet/Data Lake).
* **EXP009 - Business Intelligence:** Conexão de ferramenta de BI (PowerBI) à base analítica para geração de insights sem impactar a performance do ERP.
