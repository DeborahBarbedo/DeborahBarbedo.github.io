---
# multilingual page pair id, this must pair with translations of this page. (This name must be unique)
lng_pair: 2_WoE_IV_Python_Unpacking
title: "WoE e IV em Regressão Logística: Interpretação e Seleção de Variáveis"

# post specific
# if not specified, .name will be used from _data/owner/[language].yml
#author: "Deborah Cholodoysky Barbedo Pereira"
# multiple category is not supported
category: Ciência de Dados
# multiple tag entries are possible
tags: [Python, Regressão Logística, Feature Engineering, Seleção de Variáveis, Weight of Evidence, Information Value, WoE, IV, Credit Scoring, Titanic Dataset]
# thumbnail image for post
img: ":IV_and_WoE.jpg"
img_alt: "Interpretação de Weight of Evidence (WoE) e Information Value (IV) em regressão logística"
# disable comments on this page
#comments_disable: true

# publish date
date: 2023-05-08 19:57:53 +0900

# seo
# if not specified, date will be used.
#meta_modify_date: 2021-08-10 11:32:53 +0900
# check the meta_common_description in _data/owner/[language].yml
meta_description: "Aprenda a interpretar Weight of Evidence (WoE) e Information Value (IV) em modelos de regressão logística. Descubra como utilizar essas métricas para seleção de variáveis, análise preditiva e agrupamento de categorias com exemplos práticos utilizando o dataset Titanic."
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
---

<!-- outline-start -->

Aprenda a interpretar **Weight of Evidence (WoE)** e **Information Value (IV)** para seleção de variáveis, análise preditiva e construção de modelos de regressão logística.

<!-- outline-end -->

Construir um bom modelo de classificação não significa apenas escolher algoritmos: é fundamental compreender o comportamento das variáveis e sua relação com o evento de interesse.

É nesse contexto que surgem o **Weight of Evidence (WoE)** e o **Information Value (IV)**, duas métricas amplamente utilizadas em *credit scoring*, seleção de variáveis e modelagem preditiva.

Essas técnicas ajudam a medir o poder discriminatório de uma variável, além de fornecer insights sobre a direção e a intensidade da relação entre os atributos e a variável alvo.

Neste artigo, você aprenderá:

- como interpretar WoE e IV na prática;
- como essas métricas auxiliam na construção de modelos de regressão logística;
- como utilizar WoE e IV no agrupamento de categorias (*binning*);
- como transformar variáveis em atributos mais interpretáveis e preditivos.

Para tornar os conceitos mais intuitivos, utilizaremos exemplos práticos com o conjunto de dados da [competição Titanic do Kaggle](https://www.kaggle.com/competitions/titanic/data).


## Apresentação das Métricas

No post anterior sobre [como calcular WoE e IV em Python](https://deborahbarbedo.github.io/pt/2023-04-17-WoE_IV_Python_Function), vimos como construir as funções responsáveis pelo cálculo dessas métricas. Agora, vamos entender como interpretar os resultados e extrair insights para modelos de regressão logística.

Essas métricas ajudam a avaliar como cada variável explicativa se relaciona com a variável resposta.

As principais métricas são:

- **Proporção das classes (`0` e `1`)**  
  Representa a distribuição da variável resposta dentro de cada segmento da variável analisada. Essa métrica ajuda a entender como os eventos estão distribuídos entre as categorias.

- **Weight of Evidence (WoE)**  
  Mede o poder discriminatório de cada categoria. Quanto mais distante de zero for o valor de WoE, maior tende a ser a capacidade discriminatória do segmento.

  Em geral:

  - `WoE > 0` - maior concentração relativa de não eventos (`target₀`);
  - `WoE < 0` - maior concentração relativa de eventos (`target₁`);
  - `WoE ≈ 0` - distribuição semelhante entre as classes.

- **Information Value (IV)**  
  Mede a capacidade preditiva da variável como um todo. O IV total é obtido pela soma das contribuições de cada segmento.

  É importante observar que categorias raras podem apresentar WoE extremos, mas ainda assim contribuírem pouco para o IV total devido à baixa representatividade populacional.

---

### Interpretação dos Valores de IV

A tabela abaixo apresenta uma classificação frequentemente utilizada para interpretação do poder preditivo de uma variável:

| IV | Interpretação |
|:---|:---|
| `IV ≤ 0.02` | Variável sem poder preditivo |
| `0.02 < IV ≤ 0.10` | Poder preditivo fraco |
| `0.10 < IV ≤ 0.30` | Poder preditivo médio |
| `0.30 < IV ≤ 0.50` | Poder preditivo forte |
| `IV > 0.50` | Poder preditivo muito forte (*possível data leakage*) |

Valores excessivamente altos de IV podem indicar vazamento de informação (*data leakage*), especialmente quando a variável possui relação direta com o evento alvo.

---

Essas métricas são extremamente úteis em etapas de:

- seleção de variáveis;
- análise exploratória;
- *feature engineering*;
- agrupamento de categorias (*binning*);
- interpretação de modelos de regressão logística.

Ao interpretar corretamente WoE e IV, torna-se possível construir modelos mais robustos, interpretáveis e alinhados ao comportamento dos dados.

## Criação de Variáveis por Agrupamento (*Binning*)

O agrupamento de categorias (*binning*) é uma estratégia amplamente utilizada na criação de variáveis para modelos preditivos, especialmente em problemas de regressão logística e *credit scoring*.

A ideia consiste em combinar categorias ou intervalos que apresentem comportamento semelhante em relação à variável resposta, utilizando métricas como **Weight of Evidence (WoE)** e **Information Value (IV)** como apoio na análise.

Esse processo oferece diversas vantagens:

- simplificação das variáveis;
- redução da dimensionalidade;
- diminuição do risco de *overfitting*;
- maior estabilidade estatística;
- melhoria na interpretabilidade do modelo;
- relações mais lineares entre as variáveis e o logit da regressão logística.

Entretanto, alguns cuidados são importantes durante o agrupamento:

- categorias agrupadas devem possuir comportamentos semelhantes;
- segmentos com WoE muito diferentes não devem ser combinados;
- agrupamentos excessivos podem reduzir significativamente o poder preditivo da variável;
- categorias muito raras podem gerar WoE instáveis.

Em geral, ao agrupar categorias, ocorre uma redução natural do **Information Value (IV)**, já que parte da capacidade discriminatória da variável é suavizada. Por isso, o objetivo do *binning* deve ser encontrar um equilíbrio entre:

- simplificação do modelo;
- estabilidade estatística;
- manutenção da informação relevante.

Quando realizado corretamente, o agrupamento de categorias pode melhorar significativamente a robustez e a capacidade de generalização do modelo preditivo.



## Aplicação Prática

Agora vamos aplicar, na prática, os conceitos de **Weight of Evidence (WoE)** e **Information Value (IV)** utilizando o dataset da competição [Titanic - Machine Learning from Disaster](https://www.kaggle.com/competitions/titanic/data).

Nosso objetivo será interpretar as métricas, identificar variáveis preditivas e entender como utilizar WoE e IV na criação de novas variáveis para modelos de regressão logística.

---

### Exemplo com Variável Discreta

Ao aplicarmos a função [Woe_IV_Discrete](https://gist.github.com/DeborahBarbedo/08ed242316fe3b9ed3350460e2a140f3) na variável `Sex`, obtemos a seguinte tabela:

| Sex | `target₀` | `target₁` | `Distr` | `WoE` | `IV` | `IV_total` |
|:------|-----------:|-----------:|---------:|-------:|------:|------------:|
| female | 0.147541 | 0.681287 | 0.216562 | -1.529877 | 0.816565 | 1.341681 |
| male   | 0.852459 | 0.318713 | 2.674688 | 0.983833  | 0.525116 | 1.341681 |

A interpretação dos resultados é bastante intuitiva:

- o valor negativo de `WoE` para `female` indica maior associação com sobrevivência (`target₁`);
- o valor positivo de `WoE` para `male` indica maior associação com não sobrevivência (`target₀`);
- o `IV_total = 1.34` indica altíssimo poder discriminatório da variável `Sex`.

Em problemas de credit scoring, variáveis com IV muito elevado costumam exigir atenção adicional, pois podem indicar separação excessiva entre as classes ou possível vazamento de informação.

---

### Exemplo com Variável Contínua

Além de variáveis categóricas, WoE e IV também podem ser aplicados em variáveis contínuas após o processo de discretização (*binning*).


| Variável | Intervalo | `target₀` | `target₁` | `Distr` | `WoE` | `IV` |
|:---|:---|---:|---:|---:|---:|---:|
| Fare | `<= 7.55` | 0.143898 | 0.038012 | 3.785624 | 1.331211 | 0.14 |
| Fare | `7.55 – 7.8542` | 0.111111 | 0.076023 | 1.461538 | 0.379490 | 0.01 |
| Fare | `7.8542 – 8.05` | 0.158470 | 0.055556 | 2.852459 | 1.048181 | 0.11 |
| Fare | `8.05 – 10.5` | 0.109290 | 0.052632 | 2.076503 | 0.730685 | 0.04 |
| Fare | `10.5 – 14.4542` | 0.087432 | 0.105263 | 0.830601 | -0.185606 | 0.00 |
| Fare | `14.4542 – 21.6792` | 0.092896 | 0.108187 | 0.858662 | -0.152380 | 0.00 |
| Fare | `21.6792 – 27` | 0.078324 | 0.134503 | 0.582324 | -0.540729 | 0.03 |
| Fare | `27 – 39.6875` | 0.103825 | 0.099415 | 1.044359 | 0.043403 | 0.00 |
| Fare | `39.6875 – 77.9583` | 0.076503 | 0.137427 | 0.556679 | -0.585766 | 0.04 |
| **Total** | — | **1.000000** | **1.000000** | **1.000000** | **0.000000** | **0.37** |

A variável `Fare` também apresenta forte capacidade preditiva (`IV = 0.37`).

Além disso, a análise do WoE permite identificar faixas com comportamentos semelhantes, possibilitando o agrupamento de categorias (*binning*).

Observe que:

- faixas com `WoE > 0` tendem a estar mais associadas à não sobrevivência;
- faixas com `WoE < 0` apresentam maior associação com sobrevivência;
- valores de `WoE` próximos de zero indicam comportamento neutro.

Com base nesses resultados, podemos criar uma variável binária indicando, por exemplo, se a tarifa é menor ou igual a `10.5`.

---

### Criação de Novas Variáveis

A partir da análise de WoE e IV, podemos construir variáveis derivadas que tornam o modelo mais simples e interpretável.

Exemplo:

| PassengerId | Survived | Sex | Fare | `FLG_female` | `FLG_Fare_leq_10.5` |
|---:|---:|:---|---:|---:|---:|
| 1 | 0 | male | 7.2500 | 0 | 1 |
| 2 | 1 | female | 71.2833 | 1 | 0 |
| 3 | 1 | female | 7.9250 | 1 | 1 |
| ... | ... | ... | ... | ... | ... |
| 891 | 0 | male | 7.7500 | 0 | 1 |

Nesse exemplo:

- `FLG_female` identifica passageiros do sexo feminino;
- `FLG_Fare_leq_10.5` identifica tarifas menores ou iguais a `10.5`.

Essas transformações podem melhorar:

- interpretabilidade do modelo;
- estabilidade estatística;
- generalização;
- desempenho preditivo.

---

## Conclusão

As métricas **Weight of Evidence (WoE)** e **Information Value (IV)** são ferramentas extremamente úteis para:

- seleção de variáveis;
- análise exploratória;
- agrupamento de categorias (*binning*);
- criação de variáveis derivadas;
- interpretação de modelos de regressão logística.

Além de contribuírem para modelos mais interpretáveis e robustos, WoE e IV permitem compreender o comportamento das variáveis ao longo dos diferentes segmentos da população, auxiliando tanto na seleção de atributos quanto na engenharia de variáveis para problemas de classificação binária.

---

## Recursos Complementares

- [Materiais de suporte no GitHub](https://github.com/DeborahBarbedo/Supporting_materials/tree/main/IV_WoE)
- [Função para cálculo de WoE e IV em Python](https://deborahbarbedo.github.io/pt/2023-04-17-WoE_IV_Python_Function)
- [Como WoE e IV são calculados](https://deborahbarbedo.github.io/pt/2023-06-12-WoE_IV_Calculation)

## Referências:

* Anderson, Raymond. The Credit Scoring Toolkit: Theory and Practice for Retail Credit Risk Management and Decision Automation. Oxford University Press, 2007.

* Siddiqi, Naeem. Credit Risk Scorecards: Developing and Implementing Intelligent Credit Scoring. Wiley, 2006.

* Sudarson Mothilal Thoppay (2015). woe: Computes Weight of Evidence and Information Values. R package version 0.2.
  [https://CRAN.R-project.org/package=woe](https://CRAN.R-project.org/package=woe)

* Thilo Eichenberg (2018). woeBinning: Supervised Weight of Evidence Binning of Numeric Variables and Factors. R package
  version 0.1.6. [https://CRAN.R-project.org/package=woeBinning](https://CRAN.R-project.org/package=woeBinning)
