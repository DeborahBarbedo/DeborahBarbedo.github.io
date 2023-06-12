---
# multilingual page pair id, this must pair with translations of this page. (This name must be unique)
lng_pair: 2_WoE_IV_Python_Unpacking
title: "Dominando a Regressão Logística: Desvendando as Métricas WoE e IV para Seleção e Interpretação de Variáveis."

# post specific
# if not specified, .name will be used from _data/owner/[language].yml
#author: "Deborah Cholodoysky Barbedo Pereira"
# multiple category is not supported
category: Análise de dados
# multiple tag entries are possible
tags: [Análise de dados, Regressão logística, Seleção de variáveis, Valor de Informação (IV), Peso de Evidência (WoE), Conjunto de Dados Titanic]
# thumbnail image for post
img: ":IV_and_WoE_desvenda.jpg"
# disable comments on this page
#comments_disable: true

# publish date
date: 2023-05-08 19:57:53 +0900

# seo
# if not specified, date will be used.
#meta_modify_date: 2021-08-10 11:32:53 +0900
# check the meta_common_description in _data/owner/[language].yml
meta_description: "Aprenda a selecionar e interpretar variáveis para modelos de regressão logística com as métricas WoE e IV. Descubra como criar variáveis por agrupamento para melhorar a eficácia da análise e tomar decisões mais embasadas. Domine essas métricas com a ajuda de exemplos práticos baseados nos dados da competição Titanic do Kaggle. Leia agora!"
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

WoE e IV para Seleção e Interpretação de Variáveis.

<!-- outline-end -->

Você já ouviu falar em WoE e IV? Essas siglas significam Valor de Informação e Peso de Evidência, respectivamente, e são muito importantes na hora de construir, selecionar e interpretar variáveis para modelos de regressão logística.

Essas métricas ajudam a ter insights sobre a eficácia de uma variável em prever a resposta desejada, além de descobrir a direção em que essa variável está inclinando a resposta.

Ao final deste post, você será capaz de interpretar essas métricas e criar variáveis por agrupamento, tudo com a ajuda de exemplos práticos baseados nos dados da [competição Titanic do Kaggle](https://www.kaggle.com/competitions/titanic/data). Vamos juntos nessa jornada?

# Apresentação das métricas

Lembra do post anterior sobre [como criar as funções WoE e IV em Python]( https://deborahbarbedo.github.io/pt/2023-04-17-WoE_IV_Python_Function)? Agora eu vou te mostrar como interpretar a tabela e extrair ainda mais insights!

Abaixo são explicadas as diversas métricas apresentadas na tabela que são úteis para avaliar a relação entre a variável estudada e a ocorrência de resultados negativos ou positivos.

-	**Proporção 0 ou 1 na variável resposta** para cada setor da variável estudada, o que ajuda a entender a distribuição da variável em relação aos resultados.
-	**WoE**, uma métrica útil para avaliar a discriminação da variável. Quanto mais longe de 0 o WoE estiver, mais discriminatória será a variável. Um WoE negativo indica que a variável favorece a ocorrência da variável resposta, enquanto um WoE positivo indica que a variável não favorece a ocorrência.
-	**IV**, o Valor de Informação que ajuda a avaliar a capacidade preditiva das variáveis. É importante destacar que se um setor da variável indicar uma forte associação com a variável resposta, mas aparecer com pouca frequência na população, seu IV não será alto. O Valor de Informação Total de uma variável é a soma dos IVs para cada setor estudado.

Além disso, temos uma tabela que indica a classificação dos valores de IV:

| IV        | Classificação            |
|-----------|---------------------------|
| ≤ 0.02   | Não é útil para prever |
| 0.02 -0.1 | Poder de previsão fraco     |
| 0.1 - 0.3 | Poder de previsão moderado |
| 0.3 - 0.5 | Poder de previsão forte   |
| \> 0.5     | Poder de previsão suspeito  |


Essa tabela é importante para avaliar a qualidade da capacidade preditiva da variável, de acordo com o valor do IV. É interessante ficar atento à classificação de cada valor e utilizá-la como referência para a interpretação dos resultados.

Essas métricas permitem que você tenha uma visão ainda mais clara dos seus dados! Com elas, você pode compreender melhor a relação entre as variáveis estudadas e o resultado que você está buscando prever. Não deixe de utilizá-las para melhorar a qualidade da sua análise e tomar decisões mais embasadas!

# Criação de variáveis por agrupamento
Agrupar categorias é uma alternativa na criação de variáveis para modelos preditivos. Isso envolve analisar a similaridade na discriminação das variáveis resposta e avaliar casos representativos em cada atributo, resultando em categorias agrupadas de uma forma que faça sentido. O agrupamento de categorias com base na análise de IV e WoE apresenta várias vantagens, como simplificar a equação, reduzir o risco de overfitting e tornar as variáveis mais adequadas para o modelo. No entanto, é importante lembrar que o valor de informação sempre diminui quando as categorias são agrupadas, e apenas categorias com WoE semelhantes devem ser combinadas para evitar a perda de informações importantes. Portanto, ao realizar essa etapa, é fundamental buscar um equilíbrio entre a simplificação e a manutenção da coerência das informações.



# Mão na massa
Vamos colocar em prática tudo o que aprendemos e ir além na análise dos dados.

Ao aplicarmos a função [Woe_IV_Discrete](https://gist.github.com/DeborahBarbedo/08ed242316fe3b9ed3350460e2a140f3) na variável "Sex" dos dados da competição [Titanic - Machine Learning from Disaster](https://www.kaggle.com/competitions/titanic/data) do [Kaggle]( https://www.kaggle.com/), encontramos alguns insights interessantes.

| Survived |        0 |        1 |    Distr |       WoE |       IV | IV_total |
|---------:|---------:|---------:|---------:|----------:|---------:|---------:|
|      Sex |          |          |          |           |          |          |
|   female | 0.147541 | 0.681287 | 0.216562 | -1.529877 | 0.816565 | 1.341681 |
|     male | 0.852459 | 0.318713 | 2.674688 |  0.983833 | 0.525116 | 1.341681 |


 A métrica WoE confirma que ser do sexo feminino favorece a sobrevivência. A métrica IV indica que a variável sexo está fortemente relacionada à variável resposta, o que sugere um alto poder preditivo.

Vamos dar uma olhada na tabela gerada pela função [Woe_IV_Continuous](https://gist.github.com/DeborahBarbedo/d9ddd529f9b4359e4a867a649ab9544b) para a variável "Fare".


| variable |                 limit |        0 |        1 |    Distr |       WoE |   IV |
|---------:|----------------------:|---------:|---------:|---------:|----------:|-----:|
|     Fare |              <=[7.55] | 0.143898 | 0.038012 | 3.785624 |  1.331211 | 0.14 |
|     Fare |     [7.55] a [7.8542] | 0.111111 | 0.076023 | 1.461538 |  0.379490 | 0.01 |
|     Fare |     [7.8542] a [8.05] | 0.158470 | 0.055556 | 2.852459 |  1.048181 | 0.11 |
|     Fare |       [8.05] a [10.5] | 0.109290 | 0.052632 | 2.076503 |  0.730685 | 0.04 |
|     Fare |    [10.5] a [14.4542] | 0.087432 | 0.105263 | 0.830601 | -0.185606 | 0.00 |
|     Fare | [14.4542] a [21.6792] | 0.092896 | 0.108187 | 0.858662 | -0.152380 | 0.00 |
|     Fare |     [21.6792] a [27.] | 0.078324 | 0.134503 | 0.582324 | -0.540729 | 0.03 |
|     Fare |     [27.] a [39.6875] | 0.103825 | 0.099415 | 1.044359 |  0.043403 | 0.00 |
|     Fare | [39.6875] a [77.9583] | 0.076503 | 0.137427 | 0.556679 | -0.585766 | 0.04 |
|     Fare |                       | 1.000000 | 1.000000 | 1.000000 |  0.000000 | 0.37 |

Essa variável também tem um IV alto, indicando forte poder preditivo. Para melhorar o modelo, é recomendável criar uma variável binária indicando se o valor é menor ou igual a 10,5. Ao agrupar categorias, é importante lembrar que o valor da informação tende a diminuir. Outro ponto necessário se atentar, é indicado agrupar apenas categorias com WoE semelhantes. No caso da variável "Fare", a linha 7 tem um WoE positivo próximo de zero, o que sugere uma faixa neutra em relação à sobrevivência. Assim, incluí-la no grupo que favorece a sobrevivência é seguro.

Com base nesta análise rápida, pudemos criar duas variáveis (FLG_Fare_leq_10.5 e FLG_female) que serão úteis na construção do modelo de regressão logística. Os detalhes podem ser vistos na tabela abaixo.

| PassengerId | Survived | Pclass |    Sex | ... |    Fare | Cabin | Embarked | FLG_female | FLG_Fare_leq_10.5 |
|------------:|---------:|-------:|-------:|----:|--------:|------:|---------:|-----------:|------------------:|
|           1 |        0 |      3 |   male | ... |  7.2500 |   NaN |        S |          0 |                 1 |
|           2 |        1 |      1 | female | ... | 71.2833 |   C85 |        C |          1 |                 0 |
|           3 |        1 |      3 | female | ... |  7.9250 |   NaN |        S |          1 |                 1 |
|         ... |      ... |    ... |    ... | ... |     ... |   ... |      ... |        ... |               ... |
|         889 |        0 |      3 | female | ... | 23.4500 |   NaN |        S |          1 |                 0 |
|         890 |        1 |      1 |   male | ... | 30.0000 |  C148 |        C |          0 |                 0 |
|         891 |        0 |      3 |   male | ... |  7.7500 |   NaN |        Q |          0 |                 1 |

Em resumo, analisar as métricas WoE, IV e outras é crucial para identificar variáveis preditivas e agrupar categorias de forma eficaz para melhorar o desempenho do modelo. Espero que a explicação sobre como interpretar as métricas na tabela tenha fornecido o conhecimento necessário para criar, interpretar e selecionar variáveis para o modelo de regressão logística.

Você pode encontrar todos os [materiais de suporte](https://github.com/DeborahBarbedo/Supporting_materials/tree/main/IV_WoE) na minha página do Github. E caso você tenha perdido, meu post anterior apresentou a [função para calcular WoE e IV em Python]( https://deborahbarbedo.github.io/pt/2023-04-17-WoE_IV_Python_Function). Mas adivinhe só? Estou planejando ir ainda mais a fundo em um post futuro e explicar [como essas métricas são calculadas](https://deborahbarbedo.github.io/pt/2023-06-12-WoE_IV_Calculation). Então fique ligado! E se você tiver alguma dúvida, não hesite em me perguntar.

# Referências:

* Anderson, Raymond. The Credit Scoring Toolkit: Theory and Practice for Retail Credit Risk Management and Decision Automation. Oxford University Press, 2007.

* Siddiqi, Naeem. Credit Risk Scorecards: Developing and Implementing Intelligent Credit Scoring. Wiley, 2006.

* [Sudarson Mothilal Thoppay (2015). woe: Computes Weight of Evidence and Information Values. R package version 0.2.
  https://CRAN.R-project.org/package=woe](https://CRAN.R-project.org/package=woe)

* [Thilo Eichenberg (2018). woeBinning: Supervised Weight of Evidence Binning of Numeric Variables and Factors. R package
  version 0.1.6. https://CRAN.R-project.org/package=woeBinning](https://CRAN.R-project.org/package=woeBinning)
