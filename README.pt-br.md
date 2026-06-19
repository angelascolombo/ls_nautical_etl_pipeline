# LH Nautical Data Insights: Pipeline de Vendas Marítimas e Modelagem Dimensional

🌐 Read this in [English](README.md)

---

Este projeto desenvolve um Pipeline de Processamento de Dados (ETL) ponta a ponta e uma Modelagem Dimensional com foco no histórico de vendas, gestão de clientes via CRM, custos de importação e taxas de câmbio para uma empresa de equipamentos náuticos e marítimos.

O objetivo principal é transformar dados brutos em multiformatos (CSV, JSON) em uma estrutura analítica otimizada para a extração de insights de negócios, cálculo de margens de lucro e análise de desempenho.

## 🚀 Tecnologias Utilizadas

* **Python 3.12**
* **Pandas:** Limpeza, manipulação e transformação de dados.
* **Regex (Expressões Regulares):** Padronização de textos e extração complexa de strings.
* **Jupyter Notebook / Google Colab:** Ambiente de desenvolvimento e prototipagem.

## 🛠️ Processo de ETL & Qualidade de Dados

O projeto mitiga as inconsistências de dados dos sistemas de origem através das seguintes etapas do pipeline:
1. **Padronização de Localização:** Uso de Regex para extrair as siglas dos estados (UF) e isolar os nomes dos municípios a partir de strings desalinhadas na base do CRM.
2. **Tratamento de Strings Complexas:** Correção e unificação de e-mails que continham caracteres inválidos (por exemplo, substituindo `#` por `@`).
3. **Normalização de Categorias de Produtos:** Mapeamento via Expressões Regulares para consolidar mais de 30 variações de texto mal escritas em apenas 3 macrocategorias oficiais (*eletrônicos, propulsão e ancoragem*).
4. **Tipagem de Dados (Casting):** Conversão de strings monetárias (ex: `R$ 33122.52`) para formatos numéricos adequados (`float64`) para cálculos matemáticos.
5. **Tratamento de dados duplicados:** Identificação e remoção de registros duplicados para garantir a unicidade das chaves primárias.

## 📐 Modelagem Dimensional (Star Schema)

Para garantir alta performance em consultas analíticas e facilitar a integração com ferramentas de Visualização de Dados (como Power BI ou Tableau), a arquitetura de dados segue o padrão **Star Schema** (Modelo Estrela).

A estrutura consiste em uma tabela fato central cercada por tabelas dimensão, permitindo análises granulares por cliente, produto e vendas.

## 🏗️ Arquitetura do Modelo

### Tabela Fato

* **`fact_sales.csv`**
  * Tabela central do modelo. Armazena todas as transações de vendas ocorridas e as métricas quantitativas.
  * **Esquema:**
    ```sql
    ├── sale_id (PK)
    ├── client_id (FK)
    ├── product_id (FK)
    ├── sale_date 
    ├── product_qty (Quantidade de produtos vendidos)
    └── sale_total_BRL 
    ```

### Tabelas Dimensão

* **`dim_customers.csv`**
  * Armazena informações descritivas e atributos sobre os clientes.
  * **Esquema:**
    ```sql
    ├── client_id (PK)
    ├── client_name
    ├── client_email
    ├── client_state
    └── client_city
    ```

* **`dim_products.csv`**
  * Armazena informações do catálogo, categorias e detalhes de preços de todos os produtos.
  * **Esquema:**
    ```sql
    ├── product_id (PK)
    ├── product_name
    ├── product_category
    └── list_price_BRL (Preço unitário do produto)
    ```

### Tabelas Auxiliares

* **`aux_costs.csv`**
  * Armazena informações sobre os custos de compra dos produtos. Como os produtos são importados, eles são adquiridos em Dólares Americanos (USD) e convertidos para o Real Brasileiro (BRL).
  * **Esquema:**
    ```sql
    ├── purchase_id (PK)
    ├── product_id (FK)
    ├── purchase_date 
    ├── unit_cost_USD 
    ├── exchange_date 
    ├── exchange_rate 
    └── unit_cost_BRL
    ```

* **`aux_financial_summary.csv`**
  * Uma visão consolidada ou relatório que agrega os dados de vendas com os custos de produção/importação para exibir métricas de lucratividade.
  * **Esquema:**
    ```sql
    ├── sale_id (FK)
    ├── purchase_id (FK)
    ├── sale_total_BRL
    ├── total_cost_BRL
    └── gross_profit_BRL
    ```

---

## 📈 Próximos Passos

* **Análise de Negócios Avançada:** Implementar métricas de negócios e segmentação avançada, como a **Análise RFV** (Recência, Frequência, Valor Monetário), para descobrir padrões de comportamento dos clientes.
* **Dashboard Interativo:** Construir um dashboard dinâmico para visualizar o desempenho das vendas, tendências de lucratividade e os principais indicadores de desempenho (KPIs).
