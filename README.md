# Previsão da Produção de Arroz com AutoML

Projeto desenvolvido para a disciplina **SCC0233 - Aplicações de Aprendizado de Máquina e Mineração de Dados**, do **ICMC/USP**, em 2026.

## Sobre o projeto

O objetivo é avaliar o uso de **Automated Machine Learning (AutoML)** para prever a produção anual de arroz em países da América do Sul. O trabalho reproduz os experimentos de um estudo de referência, que utiliza Regressão Linear, Support Vector Regression (SVR) e XGBoost, e compara seus resultados com modelos selecionados automaticamente pela biblioteca **FLAML**.

## Dados e metodologia

Os dados foram obtidos do [repositório do estudo original](https://github.com/rgrion/mlarrozprevisao) e incluem informações agrícolas da **FAOSTAT** e, em um dos experimentos, dados climáticos do **NASA POWER**.

- **Variável-alvo:** produção de arroz, em toneladas (`Production (t)`).
- **Preditores agrícolas:** ano e área colhida.
- **Preditores adicionais no experimento climático:** produtividade, temperatura média e precipitação.
- **Estratégia:** treinamento separado por país, com divisão temporal entre treino e teste.
- **AutoML:** FLAML, com busca entre LightGBM, XGBoost, Random Forest e Elastic Net.
- **Avaliação:** erro percentual absoluto médio (**MAPE**).

A busca do FLAML utilizou validação cruzada com **2 folds**, limite de **10 segundos** e até **35 iterações por país**. Foram avaliados cinco cenários, com diferentes períodos de treinamento e teste.

## Resultados

**MAPE (%) no conjunto de teste — quanto menor, melhor.** Os valores dos modelos de referência foram obtidos pela execução dos notebooks originais.

| Experimento | Regressão Linear | XGBoost | SVR | AutoML (FLAML) |
|---|---:|---:|---:|---:|
| 5 anos | 15,61 | **13,21** | 14,17 | 13,24 |
| 10 anos | 12,78 | 13,30 | 19,96 | **12,61** |
| 15 anos | 11,46 | 12,75 | 24,91 | **11,44** |
| Final | 10,60 | **10,47** | 14,34 | 10,96 |
| Dados climáticos | **2,13** | 9,14 | 14,32 | 2,23 |

O FLAML obteve o menor MAPE em **2 dos 5 cenários**, mas com diferenças pequenas em relação ao melhor modelo de referência. O **Elastic Net** foi selecionado em **46 dos 60 ajustes (76,7%)**, mostrando que modelos mais simples podem ser competitivos em conjuntos de dados pequenos.

A busca automática também apresentou maior custo computacional. Além disso, o experimento climático inclui a variável produtividade (`Yield`), diretamente relacionada à produção; portanto, a melhora observada nesse cenário não pode ser atribuída apenas aos dados de temperatura e precipitação.

## Como executar

O notebook foi preparado para o **Google Colab**:

1. Abra [`rice_production_automl.ipynb`](rice_production_automl.ipynb) no Google Colab.
2. Execute as células na ordem apresentada.
3. A primeira etapa clona o repositório do artigo original e instala suas dependências, além do `flaml[automl]==2.6.0`.
4. As demais etapas preparam os dados, executam os experimentos e apresentam as comparações de desempenho, modelos selecionados e tempos de execução.

> **Observação:** o código utiliza caminhos do ambiente Colab (`/content`). Para executá-lo localmente, é necessário adaptar esses caminhos. Os resultados e tempos podem variar conforme o ambiente e as versões das bibliotecas.

## Referência

GRION, R. et al. **Predictive modeling of rice production in South America using machine learning**. *Anais do XXII Simpósio Brasileiro de Sistemas de Informação*, 2026, p. 577–596. [Artigo](https://sol.sbc.org.br/index.php/sbsi/article/view/41343).
