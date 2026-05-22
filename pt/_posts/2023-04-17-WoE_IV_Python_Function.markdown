---
# multilingual page pair id, this must pair with translations of this page. (This name must be unique)
lng_pair: 1_WoE_IV_Python_function
title: "Funções em Python para cálculo de Weight of Evidence (WoE) e Information Value (IV)"

# post specific
# if not specified, .name will be used from _data/owner/[language].yml
#author: " "
# multiple category is not supported
category: Análise de dados
# multiple tag entries are possible
tags: [Python, Ciência de Dados, Machine Learning, Regressão Logística, Feature Engineering, Seleção de Variáveis, Weight of Evidence, Information Value, WoE, IV, Titanic Dataset]
# thumbnail image for post
img: ":IV_and_WoE.jpg"
img_alt: "Cálculo de Weight of Evidence (WoE) e Information Value (IV) em Python para seleção de variáveis"
# disable comments on this page
#comments_disable: true

# publish date
date: 2023-04-17 13:57:53 +0900

# seo
# if not specified, date will be used.
#meta_modify_date: 2021-08-10 11:32:53 +0900
# check the meta_common_description in _data/owner/[language].yml
meta_description: "Aprenda a calcular Weight of Evidence (WoE) e Information Value (IV) em Python para seleção de variáveis em regressão logística. Inclui exemplos práticos com o dataset Titanic e funções completas em Python."
# optional
# please use the "image_viewer_on" below to enable image viewer for individual pages or posts (_posts/ or [language]/_posts folders).
# image viewer can be enabled or disabled for all posts using the "image_viewer_posts: true" setting in _data/conf/main.yml.
image_viewer_on: true
# please use the "image_lazy_loader_on" below to enable image lazy loader for individual pages or posts (_posts/ or [language]/_posts folders).
# image lazy loader can be enabled or disabled for all posts using the "image_lazy_loader_posts: true" setting in _data/conf/main.yml.
image_lazy_loader_on: true
# exclude from on site search
#on_site_search_exclude: true
# exclude from search engines
#search_engine_exclude: true
# to disable this page, simply set published: false or delete this file
#published: false

excerpt: "Veja como utilizar WoE e IV em Python para análise preditiva, seleção de variáveis e regressão logística."

---

<!-- outline-start -->

Aprenda a calcular Weight of Evidence (WoE) e Information Value (IV) em Python para seleção de variáveis em regressão logística, utilizando exemplos práticos com o dataset Titanic.

<!-- outline-end -->

O **Weight of Evidence (WoE)** e o **Information Value (IV)** são métricas amplamente utilizadas em problemas de classificação binária, especialmente em modelos de regressão logística, análise de risco de crédito e scorecards.

Essas métricas auxiliam na avaliação:

- do poder preditivo de uma variável;
- da relação entre categorias e o evento de interesse;
- da qualidade das transformações aplicadas às variáveis.

Durante alguns projetos em Python, senti falta de funções simples e reutilizáveis para calcular WoE e IV de maneira prática. Por isso, decidi desenvolver minhas próprias funções para:

- variáveis categóricas;
- variáveis contínuas;
- geração de tabelas consolidadas com WoE e IV.

Neste post, apresento essas funções utilizando o clássico [conjunto de dados Titanic](https://www.kaggle.com/competitions/titanic/data).


## O que é WoE?

O **Weight of Evidence (WoE)** mede a relação entre a distribuição de eventos e não eventos em uma categoria específica.

$$
WoE = \ln\left(\frac{\%\,\text{não evento}}{\%\,\text{evento}}\right)
$$

Valores positivos indicam maior concentração de não eventos, enquanto valores negativos indicam maior concentração de eventos.

## O que é IV?

O **Information Value (IV)** mede o poder preditivo de uma variável.

$$
IV = \sum (\%\,\text{não evento} - \%\,\text{evento}) \times WoE
$$

Uma interpretação comum do IV é:

| IV | Interpretação |
|---|---|
| < 0.02 | Sem poder preditivo |
| 0.02 – 0.1 | Fraco |
| 0.1 – 0.3 | Médio |
| 0.3 – 0.5 | Forte |
| > 0.5 | Muito forte |




## WoE e IV para variáveis discretas

Comecei desenvolvendo uma função para variáveis discretas e, a partir dela, fui refinando a abordagem. Talvez isso também te incentive a criar suas próprias funções e aprofundar suas análises de dados.

<script src="https://gist.github.com/DeborahBarbedo/08ed242316fe3b9ed3350460e2a140f3.js"></script>


Em seguida, utilizei dados da competição [Titanic - Machine Learning from Disaster](https://www.kaggle.com/competitions/titanic/data) para aplicar a função de WoE e IV em Python. Os resultados foram bastante interessantes:

| Survived | 0 | 1 | Distr | WoE | IV | IV_total |
|----------|---|---|-------|-----|----|----------|
| female | 0.147541 | 0.681287 | 0.216562 | -1.529877 | 0.816565 | 1.341681 |
| male | 0.852459 | 0.318713 | 2.674688 | 0.983833 | 0.525116 | 1.341681 |
| C | 0.136612 | 0.273529 | 0.499442 | -0.694264 | 0.095057 | 0.122728 |
| Q | 0.085610 | 0.088235 | 0.970249 | -0.030203 | 0.000079 | 0.122728 |
| S | 0.777778 | 0.638235 | 1.218638 | 0.197734 | 0.027592 | 0.122728 |

A função gerou uma tabela que evidencia claramente os valores de WoE e IV para cada categoria, permitindo compreender melhor quais variáveis têm maior capacidade de discriminar o evento de interesse.

Se você estiver trabalhando com esse mesmo conjunto de dados (ou qualquer outro), recomendo fortemente testar essa abordagem. Trata-se de uma ferramenta bastante útil para exploração de dados e melhoria de modelos preditivos.

## WoE e IV para variáveis contínuas

No caso de variáveis contínuas, o processo se torna um pouco mais desafiador, principalmente na definição da melhor estratégia de discretização. Isso porque diferentes problemas podem exigir diferentes formas de binning.

Após algumas tentativas e rascunhos, optei por utilizar a divisão em decis, o que permite uma análise mais estável da distribuição e, caso necessário, a agregação posterior em quartis ou outros agrupamentos.

Essa escolha resultou em uma abordagem mais interpretável e consistente para o cálculo de WoE e IV em variáveis contínuas.

<script src="https://gist.github.com/DeborahBarbedo/d9ddd529f9b4359e4a867a649ab9544b.js"></script>


A função final passou a gerar tabelas mais estruturadas, facilitando a análise do poder preditivo de cada faixa das variáveis:

| variável | faixa | 0 | 1 | distr | WoE | IV |
|----------|------|---|---|-------|-----|----|
| Age | <=[14.] | 0.058288 | 0.131579 | 0.442987 | -0.814214 | 0.06 |
| Age | [14.] a [19.] | 0.096539 | 0.099415 | 0.971070 | -0.029356 | 0.00 |
| Age | [19.] a [22.] | 0.087432 | 0.055556 | 1.573770 | 0.453474 | 0.01 |
| Age | [22.] a [25.] | 0.080146 | 0.076023 | 1.054224 | 0.052805 | 0.00 |
| Age | [25.] a [28.] | 0.067395 | 0.070175 | 0.960383 | -0.040424 | 0.00 |
| Age | [28.] a [31.8] | 0.072860 | 0.076023 | 0.958386 | -0.042505 | 0.00 |
| Age | [31.8] a [36.] | 0.085610 | 0.128655 | 0.665425 | -0.407330 | 0.02 |
| Age | [36.] a [41.] | 0.061931 | 0.055556 | 1.114754 | 0.108634 | 0.00 |
| Age | [41.] a [50.] | 0.085610 | 0.090643 | 0.944474 | -0.057127 | 0.00 |
| Age | total | 1.000000 | 1.000000 | 1.000000 | 0.000000 | 0.09 |
| Fare | <=[7.55] | 0.143898 | 0.038012 | 3.785624 | 1.331211 | 0.14 |
| Fare | [7.55] a [7.8542] | 0.111111 | 0.076023 | 1.461538 | 0.379490 | 0.01 |
| Fare | [7.8542] a [8.05] | 0.158470 | 0.055556 | 2.852459 | 1.048181 | 0.11 |
| Fare | [8.05] a [10.5] | 0.109290 | 0.052632 | 2.076503 | 0.730685 | 0.04 |
| Fare | [10.5] a [14.4542] | 0.087432 | 0.105263 | 0.830601 | -0.185606 | 0.00 |
| Fare | [14.4542] a [21.6792] | 0.092896 | 0.108187 | 0.858662 | -0.152380 | 0.00 |
| Fare | [21.6792] a [27.] | 0.078324 | 0.134503 | 0.582324 | -0.540729 | 0.03 |
| Fare | [27.] a [39.6875] | 0.103825 | 0.099415 | 1.044359 | 0.043403 | 0.00 |
| Fare | [39.6875] a [77.9583] | 0.076503 | 0.137427 | 0.556679 | -0.585766 | 0.04 |
| Fare | total | 1.000000 | 1.000000 | 1.000000 | 0.000000 | 0.37 |

## Função consolidada para variáveis contínuas e categóricas

Após superar os desafios iniciais, decidi avançar na implementação e desenvolver uma função consolidada capaz de calcular WoE e IV tanto para variáveis categóricas quanto contínuas em uma única estrutura.

Essa abordagem simplifica o processo de análise, permitindo uma visão unificada do poder preditivo das variáveis.

<script src="https://gist.github.com/DeborahBarbedo/bc3597b64ad2fcd54266664c62adbe3f.js"></script>

A tabela final obtida é apresentada abaixo:

| variável | faixa | 0 | 1 | distr | WoE | IV | IV_total |
|----------|------|---|---|-------|-----|----|----------|
| female |  | 0.147541 | 0.681287 | 0.216562 | -1.529877 | 0.816565 | 1.341681 |
| male |  | 0.852459 | 0.318713 | 2.674688 | 0.983833 | 0.525116 | 1.341681 |
| C |  | 0.136612 | 0.273529 | 0.499442 | -0.694264 | 0.095057 | 0.122728 |
| Q |  | 0.085610 | 0.088235 | 0.970249 | -0.030203 | 0.000079 | 0.122728 |
| S |  | 0.777778 | 0.638235 | 0.970249 | 0.197734 | 0.027592 | 0.122728 |
| Age | <=[14.] | 0.058288 | 0.131579 | 0.442987 | -0.814214 | 0.060000 |  |
| Age | [14.] a [19.] | 0.096539 | 0.099415 | 0.971070 | -0.029356 | 0.000000 |  |
| Age | [19.] a [22.] | 0.087432 | 0.055556 | 1.573770 | 0.453474 | 0.010000 |  |
| Age | [22.] a [25.] | 0.080146 | 0.076023 | 1.054224 | 0.052805 | 0.000000 |  |
| Age | [25.] a [28.] | 0.067395 | 0.070175 | 0.960383 | -0.040424 | 0.000000 |  |
| Age | [28.] a [31.8] | 0.072860 | 0.076023 | 0.958386 | -0.042505 | 0.000000 |  |
| Age | [31.8] a [36.] | 0.085610 | 0.128655 | 0.665425 | -0.407330 | 0.020000 |  |
| Age | [36.] a [41.] | 0.061931 | 0.055556 | 1.114754 | 0.108634 | 0.000000 |  |
| Age | [41.] a [50.] | 0.085610 | 0.090643 | 0.944474 | -0.057127 | 0.000000 |  |
| Age | total | 1.000000 | 1.000000 | 1.000000 | 0.000000 | 0.090000 |  |
| Fare | <=[7.55] | 0.143898 | 0.038012 | 3.785624 | 1.331211 | 0.140000 |  |
| Fare | [7.55] a [7.8542] | 0.111111 | 0.076023 | 1.461538 | 0.379490 | 0.010000 |  |
| Fare | [7.8542] a [8.05] | 0.158470 | 0.055556 | 2.852459 | 1.048181 | 0.110000 |  |
| Fare | [8.05] a [10.5] | 0.109290 | 0.052632 | 2.076503 | 0.730685 | 0.040000 |  |
| Fare | [10.5] a [14.4542] | 0.087432 | 0.105263 | 0.830601 | -0.185606 | 0.000000 |  |
| Fare | [14.4542] a [21.6792] | 0.092896 | 0.108187 | 0.858662 | -0.152380 | 0.000000 |  |
| Fare | [21.6792] a [27.] | 0.078324 | 0.134503 | 0.582324 | -0.540729 | 0.030000 |  |
| Fare | [27.] a [39.6875] | 0.103825 | 0.099415 | 1.044359 | 0.043403 | 0.000000 |  |
| Fare | [39.6875] a [77.9583] | 0.076503 | 0.137427 | 0.556679 | -0.585766 | 0.040000 |  |
| Fare | total | 1.000000 | 1.000000 | 1.000000 | 0.000000 | 0.370000 |  |

## Interpretando os resultados

Observando os resultados obtidos no conjunto de dados Titanic, é possível extrair algumas conclusões importantes sobre o poder preditivo das variáveis:

- A variável **Sex** apresentou IV superior a 1.3, indicando altíssimo poder preditivo.
- A variável **Fare** apresentou IV próximo de 0.37, sugerindo forte capacidade de discriminação.
- Já a variável **Age** apresentou IV mais baixo (0.09), indicando menor contribuição preditiva quando analisada isoladamente.

Essas métricas são amplamente utilizadas em tarefas de modelagem preditiva e podem apoiar decisões como:

- seleção de variáveis;
- construção de scorecards;
- transformação de variáveis para regressão logística;
- identificação de relações não lineares entre variáveis e o evento de interesse.


## Conclusão

O uso de WoE e IV pode melhorar a interpretabilidade e a capacidade preditiva de modelos de regressão logística.

Além de auxiliar na seleção de variáveis, essas métricas permitem compreender como diferentes faixas ou categorias influenciam a probabilidade do evento de interesse.

As funções desenvolvidas neste post permitem:

- calcular WoE e IV para variáveis categóricas;
- discretizar variáveis contínuas automaticamente;
- consolidar os resultados em uma única tabela para análise exploratória.

---

## Código e materiais complementares

- [Interpretação e Seleção de Variáveis com WoE e IV](https://deborahbarbedo.github.io/pt/2023-05-08-Unpacking_WOE_and_IV)
- [Como WoE e IV são calculados](https://deborahbarbedo.github.io/pt/2023-06-12-WoE_IV_Calculation)

Os códigos utilizados neste post estão disponíveis no GitHub:

- [Materiais de suporte no GitHub](https://github.com/DeborahBarbedo/Supporting_materials/tree/main/IV_WoE)

O repositório contém as funções desenvolvidas para o cálculo de WoE e IV, incluindo:

- variáveis categóricas;
- variáveis contínuas;
- função consolidada;
- exemplos de aplicação no [dataset Titanic](https://www.kaggle.com/competitions/titanic/data).


## Referências:

* Anderson, Raymond. The Credit Scoring Toolkit: Theory and Practice for Retail Credit Risk Management and Decision Automation. Oxford University Press, 2007.

* Siddiqi, Naeem. Credit Risk Scorecards: Developing and Implementing Intelligent Credit Scoring. Wiley, 2006.

* Sudarson Mothilal Thoppay (2015). woe: Computes Weight of Evidence and Information Values. R package version 0.2.
  [https://CRAN.R-project.org/package=woe](https://CRAN.R-project.org/package=woe)

* Thilo Eichenberg (2018). woeBinning: Supervised Weight of Evidence Binning of Numeric Variables and Factors. R package
  version 0.1.6. [https://CRAN.R-project.org/package=woeBinning](https://CRAN.R-project.org/package=woeBinning)