# MVP - Engenharia de Dados

## Contexto de Negócios e Perguntas

### Contexto

A soja possui papel relevante em diferentes cadeias produtivas brasileiras, sendo utilizada tanto na cadeia de alimentos quanto como matéria-prima para a produção de biodiesel. Dessa forma, o crescimento da produção agrícola de soja não implica necessariamente aumento proporcional de sua utilização para fins energéticos.

Considerando a importância do óleo de soja na produção brasileira de biodiesel, a integração de dados agrícolas e energéticos pode contribuir para compreender como a evolução da produção de soja se relaciona com sua utilização na cadeia de biodiesel ao longo do tempo e entre diferentes regiões do Brasil.

### Problema

Os dados sobre a produção agrícola de soja e sobre a cadeia produtiva do biodiesel são disponibilizados por fontes distintas e apresentam diferentes estruturas e granularidades.

Essa fragmentação dificulta a análise conjunta das informações e a avaliação da relação entre a disponibilidade agrícola da soja, sua utilização como matéria-prima e a produção de biodiesel no Brasil.

### Objetivo

Construir um pipeline de dados em nuvem para coletar, armazenar, tratar, integrar e disponibilizar dados públicos do IBGE e da ANP relacionados à produção de soja e à produção de biodiesel no Brasil.

O pipeline organizará os dados nas camadas Bronze, Silver e Gold, permitindo realizar análises históricas e regionais e investigar a relação entre a produção agrícola de soja e a cadeia produtiva do biodiesel.

### Perguntas de negócio

1. Como evoluíram a produção de soja e a produção de biodiesel no Brasil ao longo do período analisado?
2. Como a produção de soja e a produção de biodiesel estão distribuídas entre as regiões brasileiras?
3. Como evoluiu a utilização de matérias-primas derivadas da soja na produção de biodiesel?
4. Existe associação entre a produção agrícola de soja e a produção de biodiesel ao longo do período analisado?

## Carga dos Dados

Os dados utilizados neste projeto foram obtidos de fontes públicas oficiais e carregados no ambiente Databricks, utilizado como plataforma em nuvem para armazenamento e processamento dos dados.

Foram utilizadas três bases de dados:

- **IBGE/PAM:** dados da Produção Agrícola Municipal referentes à cultura da soja, contendo informações como área plantada, área colhida, quantidade produzida, rendimento médio e valor da produção.
- **ANP - Produção de biodiesel:** dados históricos da produção de biodiesel, contendo informações de período, região, unidade da federação, produtor e volume produzido.
- **ANP - Matérias-primas:** dados sobre as matérias-primas utilizadas na produção de biodiesel, contendo período, região, estado, tipo de matéria-prima e quantidade utilizada.

Os dados do IBGE foram obtidos por meio da API SIDRA, enquanto os dados da ANP foram obtidos a partir dos arquivos públicos disponibilizados pela agência.

Após a coleta, os dados foram convertidos para DataFrames Spark e persistidos em tabelas Delta na camada Bronze:

- `bronze_ibge_pam_soja`
- `bronze_anp_biodiesel`
- `bronze_anp_materia_prima`

A camada Bronze preserva os dados próximos ao formato disponibilizado pelas fontes, realizando apenas os ajustes técnicos necessários para permitir sua persistência no ambiente. No caso dos arquivos da ANP, os nomes das colunas foram ajustados devido à presença de caracteres incompatíveis com a persistência padrão em tabelas Delta, sem alteração dos valores originais dos registros.

O período geral definido para a análise é de **2015 a 2023**. Entretanto, a base de matérias-primas da ANP possui registros disponíveis entre **janeiro de 2017 e agosto de 2023**. Essa diferença de cobertura temporal será considerada nas etapas de transformação e análise, sem realizar preenchimento artificial dos períodos ausentes.

## Modelagem e Catálogo de Dados

*Em desenvolvimento.*

## Pipeline de Dados

*Em desenvolvimento.*

## Qualidade de Dados

*Em desenvolvimento.*

## Análise de Dados

*Em desenvolvimento.*

## Autoavaliação

*Em desenvolvimento.*
