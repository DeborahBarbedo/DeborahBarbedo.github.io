---
# multilingual page pair id, this must pair with translations of this page. (This name must be unique)
lng_pair: 1_WoE_IV_Python_function
title: "Função em Python para calcular WoE e IV"

# post specific
# if not specified, .name will be used from _data/owner/[language].yml
#author: "Deborah Cholodoysky Barbedo Pereira"
# multiple category is not supported
category: Análise de dados
# multiple tag entries are possible
tags: [Programação em Python, Análise de dados, Regressão logística, Seleção de variáveis, Valor de Informação (IV), Peso de Evidência (WoE), Conjunto de Dados Titanic]
# thumbnail image for post
img: ":IV_WoE_Funcao_Python.jpg"
# disable comments on this page
#comments_disable: true

# publish date
date: 2023-04-17 13:57:53 +0900

# seo
# if not specified, date will be used.
#meta_modify_date: 2021-08-10 11:32:53 +0900
# check the meta_common_description in _data/owner/[language].yml
meta_description: "Aprenda como aprimorar seus modelos preditivos com o poder do Valor da Informação (IV) e Peso da Evidência (WoE). Nesta página, você encontrará instruções passo a passo para criar funções personalizadas em Python que calculam WoE e IV, permitindo avaliar com precisão a preditividade das variáveis e melhorar o desempenho do seu modelo."
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

Melhore modelos de regressão logística com WoE e IV em Python.

<!-- outline-end -->

Você já ouviu falar de Valor de Informação e Peso de Evidência? Essas duas estatísticas são uma dupla dinâmica para a seleção de variáveis preditoras em modelos de regressão logística - elas trabalham juntas para melhorar a preditividade!

Com o Valor de Informação e o Peso de Evidência, você pode ter insights sobre a eficácia de uma variável em prever a resposta desejada, além de descobrir a direção em que essa variável está inclinando a resposta.

Mas quando comecei a usar Python para selecionar e criar variáveis, percebi que faltava uma função que pudesse trazer essas métricas para a análise. Então, resolvi criar minhas próprias funções em Python para gerar tabelas que mostrassem o WoE e o IV.


# WoE e IV para variáveis discretas

Comecei criando uma função para as variáveis discretas e fui me aprimorando a partir daí. Quem sabe, talvez você também possa criar suas próprias funções para aprimorar sua análise de dados!

<script src="https://gist.github.com/DeborahBarbedo/08ed242316fe3b9ed3350460e2a140f3.js"></script>


Então, inseri alguns dados da competição [Titanic - Machine Learning from Disaster](https://www.kaggle.com/competitions/titanic/data) na minha função de WoE e IV em Python, e deixe-me dizer, os resultados foram realmente surpreendentes!


| Survived |        0 |        1 |    Distr |       WoE |       IV | IV_total |
|---------:|---------:|---------:|---------:|----------:|---------:|---------:|
|   female | 0.147541 | 0.681287 | 0.216562 | -1.529877 | 0.816565 | 1.341681 |
|     male | 0.852459 | 0.318713 | 2.674688 |  0.983833 | 0.525116 | 1.341681 |
|        C | 0.136612 | 0.273529 | 0.499442 | -0.694264 | 0.095057 | 0.122728 |
|        Q | 0.085610 | 0.088235 | 0.970249 | -0.030203 | 0.000079 | 0.122728 |
|        S | 0.777778 | 0.638235 | 1.218638 |  0.197734 | 0.027592 | 0.122728 |


A função gerou uma tabela que claramente mostrou os valores de WoE e IV para cada variável no conjunto de dados, me dando uma compreensão melhor de quais fatores eram mais importantes para prever as taxas de sobrevivência.

Se você estiver trabalhando com esse mesmo conjunto de dados (ou qualquer outro, na verdade), eu altamente recomendo testar essa função. É uma ferramenta poderosa para obter insights sobre seus dados e melhorar seus modelos preditivos.

# WoE e IV para variáveis contínuas

Quando se trata das variáveis contínuas, a coisa ficou um pouco mais complicada para mim. Eu não tinha certeza de qual era a melhor forma de dividir essas variáveis para a análise, já que elas podem ser bem diferentes dependendo do problema.

Fiquei pensando e rascunhando várias ideias, até que finalmente decidi colocar tudo em decis, porque isso permitiria analisar o retorno pelo IV em partes maiores (quartis, por exemplo), se fosse necessário.

Então, aqui está como ficou a função para as variáveis contínuas:

<script src="https://gist.github.com/DeborahBarbedo/d9ddd529f9b4359e4a867a649ab9544b.js"></script>


Foi um trabalho árduo, mas o resultado foi bem satisfatório!


| variable |                 limit |        0 |        1 |    Distr |       WoE |   IV |
|---------:|----------------------:|---------:|---------:|---------:|----------:|-----:|
|      Age |               <=[14.] | 0.058288 | 0.131579 | 0.442987 | -0.814214 | 0.06 |
|      Age |         [14.] a [19.] | 0.096539 | 0.099415 | 0.971070 | -0.029356 | 0.00 |
|      Age |         [19.] a [22.] | 0.087432 | 0.055556 | 1.573770 |  0.453474 | 0.01 |
|      Age |         [22.] a [25.] | 0.080146 | 0.076023 | 1.054224 |  0.052805 | 0.00 |
|      Age |         [25.] a [28.] | 0.067395 | 0.070175 | 0.960383 | -0.040424 | 0.00 |
|      Age |        [28.] a [31.8] | 0.072860 | 0.076023 | 0.958386 | -0.042505 | 0.00 |
|      Age |        [31.8] a [36.] | 0.085610 | 0.128655 | 0.665425 | -0.407330 | 0.02 |
|      Age |         [36.] a [41.] | 0.061931 | 0.055556 | 1.114754 |  0.108634 | 0.00 |
|      Age |         [41.] a [50.] | 0.085610 | 0.090643 | 0.944474 | -0.057127 | 0.00 |
|      Age |                       | 1.000000 | 1.000000 | 1.000000 |  0.000000 | 0.09 |
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


# Tudo junto agora

Depois de superar os obstáculos iniciais, decidi me desafiar ainda mais e criar outra função incrível - aquela que uniria as métricas tanto das variáveis discretas quanto das contínuas em uma única tabela.

Confesso que essa função foi bem mais rápida de criar! Então, sem mais delongas, aqui está a função que fiz para juntar tudo isso em uma tabela só:

<script src="https://gist.github.com/DeborahBarbedo/bc3597b64ad2fcd54266664c62adbe3f.js"></script>

A tabela final ficou da seguinte forma:

| variable |                 limit |        0 |        1 |    Distr |       WoE |       IV | IV_total |
|---------:|----------------------:|---------:|---------:|---------:|----------:|---------:|---------:|
|   female |                       | 0.147541 | 0.681287 | 0.216562 | -1.529877 | 0.816565 | 1.341681 |
|     male |                       | 0.852459 | 0.318713 | 2.674688 |  0.983833 | 0.525116 | 1.341681 |
|        C |                       | 0.136612 | 0.273529 | 0.499442 | -0.694264 | 0.095057 | 0.122728 |
|        Q |                       | 0.085610 | 0.088235 | 0.970249 | -0.030203 | 0.000079 | 0.122728 |
|        S |                       | 0.777778 | 0.638235 | 1.218638 |  0.197734 | 0.027592 | 0.122728 |
|      Age |               <=[14.] | 0.058288 | 0.131579 | 0.442987 | -0.814214 | 0.060000 |          |
|      Age |         [14.] a [19.] | 0.096539 | 0.099415 | 0.971070 | -0.029356 | 0.000000 |          |
|      Age |         [19.] a [22.] | 0.087432 | 0.055556 | 1.573770 |  0.453474 | 0.010000 |          |
|      Age |         [22.] a [25.] | 0.080146 | 0.076023 | 1.054224 |  0.052805 | 0.000000 |          |
|      Age |         [25.] a [28.] | 0.067395 | 0.070175 | 0.960383 | -0.040424 | 0.000000 |          |
|      Age |        [28.] a [31.8] | 0.072860 | 0.076023 | 0.958386 | -0.042505 | 0.000000 |          |
|      Age |        [31.8] a [36.] | 0.085610 | 0.128655 | 0.665425 | -0.407330 | 0.020000 |          |
|      Age |         [36.] a [41.] | 0.061931 | 0.055556 | 1.114754 |  0.108634 | 0.000000 |          |
|      Age |         [41.] a [50.] | 0.085610 | 0.090643 | 0.944474 | -0.057127 | 0.000000 |          |
|      Age |                       | 1.000000 | 1.000000 | 1.000000 |  0.000000 | 0.090000 |          |
|     Fare |              <=[7.55] | 0.143898 | 0.038012 | 3.785624 |  1.331211 | 0.140000 |          |
|     Fare |     [7.55] a [7.8542] | 0.111111 | 0.076023 | 1.461538 |  0.379490 | 0.010000 |          |
|     Fare |     [7.8542] a [8.05] | 0.158470 | 0.055556 | 2.852459 |  1.048181 | 0.110000 |          |
|     Fare |       [8.05] a [10.5] | 0.109290 | 0.052632 | 2.076503 |  0.730685 | 0.040000 |          |
|     Fare |    [10.5] a [14.4542] | 0.087432 | 0.105263 | 0.830601 | -0.185606 | 0.000000 |          |
|     Fare | [14.4542] a [21.6792] | 0.092896 | 0.108187 | 0.858662 | -0.152380 | 0.000000 |          |
|     Fare |     [21.6792] a [27.] | 0.078324 | 0.134503 | 0.582324 | -0.540729 | 0.030000 |          |
|     Fare |     [27.] a [39.6875] | 0.103825 | 0.099415 | 1.044359 |  0.043403 | 0.000000 |          |
|     Fare | [39.6875] a [77.9583] | 0.076503 | 0.137427 | 0.556679 | -0.585766 | 0.040000 |          |
|     Fare |                       | 1.000000 | 1.000000 | 1.000000 |  0.000000 | 0.370000 |          |

Vocês gostaram das minhas funções para o cálculo do IV e WoE em Python? Eu amei criá-las, mas percebi que ainda tem muita gente por aí que fica meio perdida com essas métricas e não sabe como utilizá-las para melhorar seus modelos de regressão logística.

Por isso, decidi que é hora de criar um post e explicar tudo bem detalhadamente! Vou mostrar como é realizado o cálculo do IV e do WoE, e dar dicas valiosas sobre como tomar decisões e escolhas inteligentes para melhorar seus modelos com base nessas métricas.

Estou super empolgada para compartilhar tudo isso com vocês, então fiquem ligados(as)!

# Referências:

* Anderson, Raymond. The Credit Scoring Toolkit: Theory and Practice for Retail Credit Risk Management and Decision Automation. Oxford University Press, 2007.

* Siddiqi, Naeem. Credit Risk Scorecards: Developing and Implementing Intelligent Credit Scoring. Wiley, 2006.

* Sudarson Mothilal Thoppay (2015). woe: Computes Weight of Evidence and Information Values. R package version 0.2.
  https://CRAN.R-project.org/package=woe

* Thilo Eichenberg (2018). woeBinning: Supervised Weight of Evidence Binning of Numeric Variables and Factors. R package
  version 0.1.6. https://CRAN.R-project.org/package=woeBinning