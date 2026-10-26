# Análise de Exames Citopatológicos com Machine Learning

Projeto desenvolvido para o **Projeto Integrador em Computação IV**, com o objetivo de analisar dados históricos de exames citopatológicos realizados em mulheres de 25 a 64 anos e aplicar técnicas de Machine Learning para identificar padrões e produzir estimativas futuras.

## Objetivo

Desenvolver e avaliar uma solução baseada em dados capaz de:

- analisar a evolução histórica dos indicadores de exames citopatológicos;
- identificar diferenças temporais e geográficas;
- aplicar técnicas de Machine Learning para previsão;
- avaliar o desempenho dos modelos por meio de métricas estatísticas;
- disponibilizar os resultados em um dashboard interativo.

## Fonte dos dados

Os dados utilizados são provenientes de **dados abertos do SUS**.

A base analisada contém informações municipais referentes ao período de **2016 a 2025**.

Para a etapa de Machine Learning, foram selecionados **412 municípios elegíveis**, considerando a disponibilidade e a continuidade dos dados históricos necessários para a criação das variáveis preditoras.

## Tecnologias utilizadas

- Databricks
- Apache Spark / PySpark
- Spark SQL
- Spark MLlib
- Python
- Unity Catalog
- Databricks AI/BI Dashboards
- Git / GitHub

## Arquitetura dos dados

O projeto foi estruturado utilizando uma arquitetura em camadas:

### Bronze

Armazenamento dos dados brutos provenientes da fonte original.

### Silver

Etapa de limpeza, tratamento, padronização e seleção dos registros relevantes.

### Gold

Construção das bases analíticas utilizadas para:

- análise histórica;
- engenharia de atributos;
- treinamento dos modelos;
- avaliação;
- geração das previsões;
- alimentação do dashboard.

## Estrutura dos notebooks

```text
00_configuracao_ambiente
01_ingestao_bronze
02_tratamento_silver
03_indicadores_gold
04_analise_exploratoria
05_feature_engineering
06_machine_learning
07_avaliacao_modelos
08_previsoes
```

### 00 — Configuração do ambiente

Define as configurações gerais, catálogo, schema, volume e tabelas utilizadas no projeto.

### 01 — Ingestão Bronze

Realiza a leitura da base original e o armazenamento dos dados brutos.

### 02 — Tratamento Silver

Executa limpeza, padronização e preparação dos dados.

### 03 — Indicadores Gold

Constrói a base histórica e a base utilizada na modelagem.

### 04 — Análise exploratória

Analisa a evolução temporal e a distribuição dos indicadores.

### 05 — Feature Engineering

Cria variáveis históricas utilizadas pelos modelos, incluindo valores de anos anteriores, médias móveis e variações.

### 06 — Machine Learning

Treina e compara diferentes algoritmos de regressão.

Foram avaliados, entre outros:

- Regressão Linear;
- Random Forest;
- Gradient Boosted Trees (GBT);
- modelos com transformações logarítmicas;
- baseline baseado no valor do ano anterior.

### 07 — Avaliação dos modelos

Realiza a avaliação final utilizando o ano de **2025 como conjunto de teste**.

### 08 — Previsões

Treina o modelo com o histórico disponível e gera estimativas experimentais para **2026**.

## Divisão temporal dos dados

Para evitar vazamento de informação entre passado e futuro, a avaliação foi realizada de forma temporal:

- **Treinamento:** 2019 a 2023
- **Validação:** 2024
- **Teste final:** 2025

Após a seleção do modelo, o histórico disponível até 2025 foi utilizado para a geração das estimativas de 2026.

## Resultados do Machine Learning

Entre os algoritmos de Machine Learning avaliados, a **Regressão Linear** apresentou o melhor desempenho.

No teste final de 2025:

| Modelo | MAE | RMSE | R² |
|---|---:|---:|---:|
| Regressão Linear - ML | 2.054,35 | 3.894,15 | 0,9921 |
| Baseline - Ano anterior | 1.748,29 | 3.339,20 | 0,9942 |

Embora a Regressão Linear tenha apresentado elevado ajuste aos dados, o baseline baseado no valor do ano anterior apresentou desempenho superior no conjunto de teste.

Esse resultado evidencia uma forte **persistência temporal** nos dados analisados.

## Previsão para 2026

Para os **412 municípios modelados**:

- Total observado em 2025: aproximadamente **6,89 milhões**
- Previsão do modelo para 2026: aproximadamente **7,19 milhões**
- Variação agregada prevista: **+4,34%**

A previsão de 2026 deve ser interpretada como uma **estimativa experimental produzida pelo modelo**, e não como um valor observado.

## Dashboard interativo

Foi desenvolvido um dashboard no Databricks AI/BI contendo:

- indicadores principais;
- evolução histórica de 2016 a 2025;
- previsão para 2026;
- análise por Unidade Federativa;
- Top 10 municípios por previsão;
- comparação entre valores realizados e previstos;
- métricas de avaliação;
- municípios com maiores erros absolutos;
- gráfico de dispersão Realizado x Previsto;
- filtros por Região, UF e Município.

### Visão geral

<img width="944" height="340" alt="image" src="https://github.com/user-attachments/assets/b2af3041-868b-44ff-978e-118420a0a1fa" />


### Avaliação do modelo

<img width="944" height="340" alt="image" src="https://github.com/user-attachments/assets/5b26ebd6-eee0-496a-95ed-4e3496df390f" />


### Realizado x Previsto

<img width="944" height="427" alt="image" src="https://github.com/user-attachments/assets/88a8a2e9-4614-4c49-b5f9-30abc1fc38f0" />



> O arquivo de configuração do dashboard também está disponível neste repositório em formato `.lvdash.json`.

## Estrutura do repositório

```text
projeto-integrador/
│
├── notebooks/
│   ├── 00_configuracao_ambiente
│   ├── 01_ingestao_bronze
│   ├── 02_tratamento_silver
│   ├── 03_indicadores_gold
│   ├── 04_analise_exploratoria
│   ├── 05_feature_engineering
│   ├── 06_machine_learning
│   ├── 07_avaliacao_modelos
│   └── 08_previsoes
│
├── dashboard/
│   └── Analise_de_Exames_Citopatologicos.lvdash.json
│
├── imagens/
│   ├── dashboard_visao_geral.png
│   ├── dashboard_avaliacao.png
│   └── dashboard_real_previsto.png
│
└── README.md
```

## Considerações

Os resultados indicam que técnicas de Machine Learning conseguem representar com elevado ajuste o comportamento histórico dos indicadores analisados. Entretanto, a comparação com um baseline simples mostrou que o valor do ano anterior possui grande capacidade preditiva.

O projeto demonstra, portanto, não apenas a aplicação de Machine Learning, mas também a importância de comparar modelos mais complexos com métodos de referência simples.

As previsões apresentadas referem-se aos **412 municípios selecionados para a modelagem** e não devem ser interpretadas como uma previsão para todos os municípios brasileiros.
