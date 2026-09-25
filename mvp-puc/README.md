# MVPs

Material desenvolvido durante a Pós-graduação em Ciência de Dados e Analytics da PUC-Rio.

**1) Machine Learning & Analytics**

Este projeto desenvolve um modelo de Machine Learning (ML) para prever a produtividade de soja e algodão no ano seguinte (Ex: dados de 2022 - previsão da produtividade de 2023), por Unidade da Federação (UF), utilizando dados públicos da Pesquisa Agrícola Municipal (PAM/IBGE). O estudo considera essas culturas no contexto da cadeia de produção do biodiesel.

O arquivo dataset_ibge_soja_algodao.csv contém uma cópia da base bruta obtida pela API pública do IBGE/PAM. Ele foi incluído para garantir a reprodutibilidade do notebook caso a API esteja temporariamente indisponível.

[📂 Acessar o MVP de Machine Learning & Analytics](sprint_machine_learning_analytics/README.md)

## 2) Engenharia de Dados

Este projeto desenvolve um pipeline de dados em nuvem para integrar dados públicos do IBGE/PAM e da ANP relacionados à produção de soja e à cadeia produtiva do biodiesel no Brasil.

A solução foi implementada no Databricks utilizando PySpark, tabelas Delta e arquitetura Medallion, com organização dos dados nas camadas Bronze, Silver e Gold. A partir dos dados tratados e integrados, foram realizadas análises históricas e regionais sobre a produção de soja, a produção de biodiesel e a utilização de matérias-primas derivadas da soja.

📂 [Acessar o MVP de Engenharia de Dados](sprint_engenharia_dados/README.md)
