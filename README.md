# 🚀 FMCG Data Consolidation Pipeline

![Databricks](https://img.shields.io/badge/Databricks-Lakehouse-red)
![Delta Live Tables](https://img.shields.io/badge/Delta_Live_Tables-ETL-orange)
![Unity Catalog](https://img.shields.io/badge/Unity_Catalog-Governance-blue)
![PySpark](https://img.shields.io/badge/PySpark-Data_Engineering-yellow)
![AWS S3](https://img.shields.io/badge/AWS-S3-orange)
![GitHub](https://img.shields.io/badge/GitHub-Version_Control-black)

Projeto de Engenharia de Dados desenvolvido na plataforma Databricks para simular um cenário real de consolidação de dados após a aquisição de uma empresa do setor FMCG (Fast-Moving Consumer Goods).

A solução demonstra a implementação de uma arquitetura Lakehouse moderna utilizando Delta Live Tables (DLT), Unity Catalog, processamento incremental, governança de dados e orquestração automatizada.

---

# 📖 Contexto do Negócio

Uma grande empresa do setor FMCG adquiriu uma startup e precisava consolidar dados operacionais provenientes de diferentes sistemas, processos e estruturas organizacionais.

O desafio consistia em construir uma plataforma de dados capaz de:

* Integrar dados da empresa adquirida e da empresa matriz.
* Garantir governança e rastreabilidade.
* Automatizar cargas históricas e incrementais.
* Disponibilizar dados confiáveis para análises corporativas.
* Permitir consultas analíticas através de dashboards e linguagem natural.

---

# 🏗️ Arquitetura da Solução

## Arquitetura Geral

![Arquitetura](docs/fmcg_architecture.png)

A solução foi desenvolvida seguindo os princípios da arquitetura Lakehouse da Databricks.

```text
AWS S3
   │
   ▼
Delta Live Tables
   │
   ▼
Bronze Layer
   │
   ▼
Silver Layer
   │
   ▼
Gold Layer (Empresa Adquirida)
   │
   ▼
Gold Corporativa (Empresa Matriz)
   │
   ▼
Dashboards + Genie AI
```

---

# 🛠️ Stack Tecnológica

| Categoria             | Tecnologia                |
| --------------------- | ------------------------- |
| Plataforma            | Databricks Free Edition   |
| Data Lake             | AWS S3                    |
| Processamento         | PySpark                   |
| Linguagem de Consulta | Spark SQL                 |
| Arquitetura           | Medallion Architecture    |
| Governança            | Unity Catalog             |
| ETL                   | Delta Live Tables (DLT)   |
| Orquestração          | Databricks Workflows      |
| Versionamento         | GitHub + Databricks Repos |
| Analytics             | Databricks Dashboards     |
| IA Generativa         | Databricks Genie          |

---

# 🏛️ Arquitetura Lakehouse

## Landing Zone

Os dados operacionais são armazenados inicialmente no AWS S3.

```text
s3://landing-zone
│
├── raw/
└── archive/
```

### Estratégia Utilizada

* Arquivos novos são identificados automaticamente.
* Após processamento, os arquivos são movidos para a área de arquivamento.
* Todo o processo mantém rastreabilidade completa dos dados.

---

# 📚 Governança com Unity Catalog

Toda a plataforma é governada através do Unity Catalog.

## Estrutura do Catálogo

```text
fmcg
│
├── bronze
├── silver
└── gold
```

### Recursos de Governança

* Controle centralizado de acesso.
* Data Lineage.
* Catálogo corporativo.
* Gerenciamento de metadados.
* Compartilhamento seguro de dados.

---

# 🥉 Camada Bronze

Responsável pela ingestão dos dados brutos.

### Objetivos

* Preservar os dados originais.
* Registrar informações de auditoria.
* Garantir rastreabilidade.

### Metadados Adicionados

```python
ingestion_timestamp
source_file
processing_date
```

---

# 🥈 Camada Silver

Responsável pelo tratamento e padronização dos dados.

### Processamento de Clientes

* Padronização de informações.
* Tratamento de valores nulos.
* Remoção de duplicidades.

### Processamento de Produtos

* Normalização de atributos.
* Conversão de tipos de dados.
* Correção de inconsistências.

### Processamento de Preços

* Validação de regras de negócio.
* Padronização de formatos.
* Controle de qualidade.

---

# 🥇 Camada Gold

Disponibiliza dados prontos para consumo analítico.

## Modelo Dimensional

### Dimensões

```text
dim_customers
dim_products
dim_gross_price
```

### Fatos

```text
fact_orders
```

---

# 🏢 Estratégia de Consolidação Corporativa

Uma das principais decisões arquiteturais deste projeto foi a separação entre os domínios da empresa adquirida e da empresa matriz.

## Empresa Adquirida (Child Company)

```text
Bronze
   ↓
Silver
   ↓
Gold
```

## Empresa Matriz (Parent Company)

```text
Gold da Subsidiária
           +
Dados Corporativos
           ↓
Gold Corporativa
```

Essa abordagem permite autonomia dos domínios de dados e, ao mesmo tempo, viabiliza análises corporativas consolidadas.

---

# ⚡ Delta Live Tables (DLT)

O pipeline principal foi implementado utilizando Delta Live Tables.

### Benefícios

* Desenvolvimento declarativo.
* Dependências automáticas.
* Monitoramento nativo.
* Qualidade de dados integrada.
* Menor esforço operacional.

### Fluxo do Pipeline

```text
Bronze
   ↓
Silver
   ↓
Gold
```

---

# 🔄 Estratégia de Carga

A plataforma suporta dois tipos de processamento.

## Carga Histórica (Full Load)

Utilizada durante a inicialização da plataforma.

```text
Dados Brutos
      ↓
Carga Histórica
      ↓
Gold
```

## Carga Incremental

Executada diariamente.

```text
Novos Arquivos
        ↓
Processamento Incremental
        ↓
Atualização das Tabelas Gold
```

### Benefícios

* Menor custo computacional.
* Maior velocidade de processamento.
* Atualizações frequentes dos dados.

---

# ⚙️ Orquestração

A execução dos pipelines é realizada através de Databricks Workflows.

## Pipeline Incremental

![Workflow](docs/workflow.png)

Fluxo de execução:

```text
dim_processing_customers
           ↓
dim_processing_products
           ↓
dim_processing_prices
           ↓
fact_processing_orders
```

### Características

* Dependências explícitas.
* Execução automatizada.
* Recuperação de falhas.
* Monitoramento centralizado.

---

# 📂 Estrutura do Projeto

```text
consolidation_pipeline/
│
├── 1_setup/
│   ├── setup_catalog
│   ├── dim_delta_table_creation
│   └── utilities
│
├── 2_dimension_data_processing/
│   ├── customer_data_processing
│   ├── products_data_processing
│   └── pricing_data_processing
│
├── 3_fact_data_processing/
│   ├── full_load_fact
│   └── incremental_load_fact
│
├── docs/
│   ├── fmcg_architecture.png
│   └── workflow.png
│
└── README.md
```

---

# 🔗 Integração com GitHub

O projeto utiliza Databricks Repos integrado diretamente ao GitHub.

### Benefícios

* Controle de versão.
* Histórico de alterações.
* Colaboração entre desenvolvedores.
* Preparação para CI/CD.

---

# 📊 Camada Analítica

Os dados processados são disponibilizados através de recursos nativos da plataforma.

## Databricks Dashboards

Disponibilização de indicadores como:

* Receita.
* Volume de vendas.
* Performance de produtos.
* Indicadores de clientes.

## Databricks Genie

Consultas em linguagem natural utilizando IA Generativa.

### Exemplo

```text
Qual foi a receita total gerada no último trimestre?
```

O Genie converte automaticamente a pergunta em SQL e retorna os resultados.

---

# 🎯 Competências Demonstradas

Este projeto evidencia conhecimentos práticos em:

* Databricks Lakehouse Platform
* Delta Live Tables (DLT)
* Unity Catalog
* Medallion Architecture
* Governança de Dados
* Data Lineage
* Processamento Incremental
* Orquestração de Pipelines
* AWS S3
* PySpark
* Spark SQL
* Analytics Engineering
* GitHub Integration

---

# 🚀 Evoluções Futuras

* Implementação de testes automatizados de qualidade.
* GitHub Actions para CI/CD.
* Data Quality Expectations no DLT.
* Observabilidade e monitoramento.
* Controle de SLA dos pipelines.
* Expansão para múltiplas subsidiárias.

---

# 👩‍💻 Autora

**Manuella Melo**

Engenheira de Dados com foco em plataformas modernas de dados, arquitetura Lakehouse, governança e soluções analíticas escaláveis.

### Contato

* LinkedIn: https://www.linkedin.com/in/SEU-LINK
* GitHub: https://github.com/SEU-USUARIO
