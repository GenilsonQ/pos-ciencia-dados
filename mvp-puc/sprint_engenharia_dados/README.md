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

A arquitetura de dados do projeto foi estruturada seguindo o padrão Medallion, com organização dos dados nas camadas Bronze, Silver e Gold.

A camada **Bronze** armazena os dados provenientes das fontes externas em formato próximo ao original, preservando os registros coletados do IBGE e da ANP.

A camada **Silver** concentra as etapas de limpeza, padronização e transformação dos dados. Nessa etapa foram realizados ajustes de tipos, tratamento de caracteres, padronização das regiões brasileiras, seleção do período de análise e adequação das diferentes granularidades das fontes.

A camada **Gold** contém os dados preparados para consumo analítico. As informações agrícolas e energéticas foram agregadas por região e ano, possibilitando a integração entre a produção de soja e a produção de biodiesel. Também foi criada uma estrutura específica para análise das matérias-primas derivadas da soja utilizadas na produção de biodiesel.

### Modelo de dados

Para a camada Gold foi adotado um **modelo Flat por conceito**, mantendo em cada tabela os atributos necessários às análises sem a criação de dimensões e tabelas fato separadas.

Essa abordagem foi escolhida devido ao escopo analítico do MVP e à baixa complexidade das dimensões envolvidas. As principais análises utilizam as dimensões de ano e região, permitindo que os dados sejam organizados diretamente na granularidade necessária para responder às perguntas de negócio.

Dessa forma, foram definidos dois conceitos analíticos principais:

- **Produção de soja e biodiesel por região e ano:** integra os dados agrícolas do IBGE com os dados de produção de biodiesel da ANP.
- **Utilização de matérias-primas derivadas da soja por região e ano:** consolida os dados da ANP referentes às matérias-primas relacionadas à soja utilizadas na produção de biodiesel.

A adoção do modelo Flat evita a introdução de complexidade desnecessária para o escopo do projeto, mantendo as tabelas Gold simples e diretamente utilizáveis nas análises.

### Estrutura das camadas

| Camada | Tabela | Descrição |
| --- | --- | --- |
| Bronze | `bronze_ibge_pam_soja` | Dados brutos da Produção Agrícola Municipal referentes à soja. |
| Bronze | `bronze_anp_biodiesel` | Dados brutos da produção de biodiesel disponibilizados pela ANP. |
| Bronze | `bronze_anp_materia_prima` | Dados brutos das matérias-primas utilizadas na produção de biodiesel. |
| Silver | `silver_ibge_soja` | Dados da produção de soja tratados e organizados por unidade da federação e ano. |
| Silver | `silver_anp_biodiesel` | Dados da produção de biodiesel tratados e padronizados. |
| Silver | `silver_anp_materia_prima` | Dados das matérias-primas tratados, com período, região e produtos padronizados. |
| Gold | `gold_soja_biodiesel_regiao_ano` | Integração da produção de soja e da produção de biodiesel na granularidade região/ano. |
| Gold | `gold_materia_prima_soja_regiao_ano` | Quantidade de matérias-primas derivadas da soja utilizadas na produção de biodiesel, agregada por região e ano. |

### Catálogo de Dados

O catálogo a seguir apresenta os atributos das tabelas utilizadas nas etapas de transformação e análise, incluindo seus tipos, significado, unidade ou domínio e origem dos dados.

## Pipeline de Dados

*Em desenvolvimento.*

## Qualidade de Dados

*Em desenvolvimento.*

## Análise de Dados

*Em desenvolvimento.*

## Autoavaliação

*Em desenvolvimento.*
