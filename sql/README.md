# SQL Transformations

Esta pasta contém as principais transformações SQL utilizadas na camada analítica do projeto.

## denormalized_sales_model.sql

Responsável pela construção da tabela Gold utilizada para consumo analítico e visualização no dashboard, consolidando dados provenientes das dimensões e fatos processados ao longo do pipeline.

## parent_company_incremental_merge.sql

Responsável pela carga incremental da camada Gold corporativa utilizando os dados consolidados da subsidiária.