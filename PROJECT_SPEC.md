# Especificação Funcional do Projeto: ERP Core - Supply Chain Module

## 1. Visão Geral
Este documento define as especificações funcionais e requisitos de negócio para o módulo de **Cadeia de Suprimentos (Supply Chain)** e **Motor Fiscal** do projeto ERP Core.

## 2. Escopo do Negócio
O sistema deve gerenciar o ciclo de vida de produtos, desde o cadastro e precificação, passando pela movimentação de estoque multilocais, até a efetivação de pedidos de venda com cálculo tributário dinâmico.

## 3. Requisitos Funcionais

### 3.1. Gestão de Catálogo e Produtos
O sistema deve manter um cadastro de produtos robusto, suportando definições fiscais e logísticas.
* **RF001 - Cadastro de Produto:** Deve incluir SKU, descrição, unidade de medida, NCM (Nomenclatura Comum do Mercosul) e peso bruto/líquido.
* **RF002 - Versionamento de Preço:** O sistema deve manter histórico de alterações de preço de venda, permitindo auditoria de quem alterou e quando.
* **RF003 - Bloqueio Lógico:** Produtos inativos não podem ser utilizados em novas movimentações, mas devem ser preservados em relatórios históricos.

### 3.2. Motor de Estoque e Armazenagem
O núcleo do sistema deve garantir a acuracidade física e financeira do estoque.
* **RF004 - Controle Multilocal (Multi-Warehouse):** O sistema deve suportar múltiplos locais de estoque.
* **RF005 - Movimentação Transacional:** Toda entrada ou saída deve gerar um registro imutável de movimentação (Kardex), contendo data, tipo de operação, quantidade e custo unitário no momento da transação.
* **RF006 - Custo Médio Ponderado:** O sistema deve recalcular automaticamente o Custo Médio do produto a cada nova entrada.
* **RF007 - Controle de Concorrência:** O sistema deve impedir que o mesmo item seja reservado para dois pedidos simultâneos se o saldo for insuficiente (prevenção de "overselling").

### 3.3. Motor Fiscal e Tributário (Tax Engine)
O cálculo de impostos deve ser determinado por regras dinâmicas.
* **RF008 - Regra de Tributação Dinâmica:** O sistema deve calcular o valor do imposto com base em uma matriz de decisão:
    * *Input:* Estado de Origem, Estado de Destino, Tipo de Produto (NCM).
    * *Output:* Alíquota de ICMS, Base de Cálculo, Valor do Imposto.
* **RF009 - Persistência de Memória de Cálculo:** O pedido deve gravar não apenas o valor final, mas as alíquotas utilizadas no momento da venda (snapshot), para fins de compliance fiscal.

### 3.4. Gestão de Pedidos de Venda (Order Processing)
* **RF010 - Ciclo de Vida do Pedido:** O pedido deve transitar por estados estritos (Rascunho -> Validado -> Aprovado -> Faturado -> Cancelado).
* **RF011 - Validação de Regras de Negócio:**
    * Não permitir aprovação de pedido com cliente inadimplente.
    * Não permitir aprovação se a margem de lucro for inferior ao parâmetro mínimo configurado.
* **RF012 - Idempotência na Aprovação:** Garantir que uma requisição de aprovação enviada múltiplas vezes não duplique a baixa de estoque.

## 4. Requisitos Não-Funcionais

### 4.1. Integridade e Performance
* **RNF001 - Consistência ACID:** Todas as operações que envolvem financeiro e estoque devem ser atômicas. Falhas no cálculo de imposto devem reverter a reserva de estoque.
* **RNF002 - Performance de Leitura:** Relatórios de posição de estoque devem ser executados de forma otimizada.
* **RNF003 - Isolamento de Domínio:** As regras de negócio devem estar encapsuladas na camada de Domínio.

### 4.2. Auditoria e Segurança
* **RNF004 - Audit Log:** Alterações em entidades críticas (Preço, Estoque) devem gerar logs de auditoria contendo `UserId`, `Timestamp`, `OldValue` e `NewValue`.

## 5. Definição de Relatórios Gerenciais
Os relatórios abaixo devem ser implementados:

1.  **Curva ABC de Vendas:** Classificação de produtos por relevância de faturamento (A = 80%, B = 15%, C = 5%).
2.  **Giro de Estoque:** Análise da velocidade de saída dos produtos em relação ao saldo médio.
3.  **Extrato de Movimentação (Kardex):** Visão cronológica e sequencial de todas as entradas e saídas de um SKU específico.
