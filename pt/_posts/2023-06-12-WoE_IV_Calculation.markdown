---
# multilingual page pair id, this must pair with translations of this page. (This name must be unique)
lng_pair: 3_WoE_IV_Python_calculation
title: "Dominando a Regressão Logística: Um Guia Abrangente para o Cálculo de WoE e IV."

# post specific
# if not specified, .name will be used from _data/owner/[language].yml
#author: "Deborah Cholodoysky Barbedo Pereira"
# multiple category is not supported
category: Análise de dados
# multiple tag entries are possible
tags: [Análise de dados, Regressão logística, Seleção de variáveis, Valor de Informação (IV), Peso de Evidência (WoE), Conjunto de Dados Titanic]
# thumbnail image for post
img: ":WoE_IV_Python_calculo.jpg"
# disable comments on this page
#comments_disable: true

# publish date
date: 2023-06-12 19:57:53 +0900

# seo
# if not specified, date will be used.
#meta_modify_date: 2021-08-10 11:32:53 +0900
# check the meta_common_description in _data/owner/[language].yml
meta_description: "Aprenda como calcular e entender as métricas de Peso da Evidência (WoE) e Valor da Informação (IV), que são amplamente utilizadas para diferenciar indivíduos dignos de crédito dos não dignos de crédito. Esta postagem de blog explora esses cálculos usando o conjunto de dados Titanic, com foco em informações de sobrevivência segregadas por gênero. Desmistifique os conceitos de IV e WoE para torná-los mais acessíveis e tangíveis. Aprofunde-se nos dados e tabelas fornecidos para obter insights e aplicar essas métricas de forma eficaz."
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

Um Guia Abrangente para o Cálculo de WoE e IV.

<!-- outline-end -->

Essas métricas são amplamente reconhecidas por sua capacidade de distinguir entre indivíduos dignos de crédito e não dignos de crédito. Ao longo de nossa jornada para entender esses cálculos, frequentemente nos deparamos com os familiares rótulos de "bons" e "maus". Nesse contexto, os "maus clientes" são aqueles que não pagaram suas dívidas, enquanto os "bons clientes" são aqueles que cumpriram suas obrigações e quitaram o empréstimo.

Para clarificar esses conceitos, vamos extrair insights do conjunto de dados da [competição Titanic do Kaggle](https://www.kaggle.com/competitions/titanic/data), examinando especificamente as informações de sobrevivência segregadas por gênero. Nosso objetivo é desmistificar os cálculos de IV e WoE, tornando-os mais acessíveis e tangíveis. Utilizaremos os dados fornecidos na tabela abaixo como base.

| Segmento | # <img src="https://latex.codecogs.com/svg.image?\small&space;target_{0}" title="https://latex.codecogs.com/svg.image?\small target_{0}" /> | # <img src="https://latex.codecogs.com/svg.image?\small&space;target_{1}" title="https://latex.codecogs.com/svg.image?\small target_{1}" /> |
|--------:|--------:|--------:|
| female | 81      | 233     |
| male    | 468     | 109     |
| Total   | 549     | 342     |

# Percentual do rótulo no segmento de estudo:

O que é comumente chamado de 'bom' é o  <img src="https://latex.codecogs.com/svg.image?\small&space;target_{0}" title="https://latex.codecogs.com/svg.image?\small target_{0}" />.

<img src="https://latex.codecogs.com/svg.image?\small&space;%&space;target_{0,&space;sector_i}&space;=&space;\frac{&space;target_{0,&space;sector_i}}{&space;target_{0}}&space;" title="https://latex.codecogs.com/svg.image?\small % target_{0, sector_i} = \frac{ target_{0, sector_i}}{ target_{0}} " />

Vamos considerar o setor escolhido como feminino.

Para este problema:


<img src="https://latex.codecogs.com/svg.image?\small&space;%&space;survived_{0,&space;female&space;}&space;=&space;\frac{&space;survived_{0,&space;female}}{&space;survived_{0}}&space;=\frac{81}{81&plus;468}&space;\approx&space;0.147541&space;" title="https://latex.codecogs.com/svg.image?\small % survived_{0, female } = \frac{ survived_{0, female}}{ survived_{0}} =\frac{81}{81+468} \approx 0.147541 " />

O que é tipicamente descrito como 'mau' é o  <img src="https://latex.codecogs.com/svg.image?\small&space;target_{1}" title="https://latex.codecogs.com/svg.image?\small target_{1}" />.

<img src="https://latex.codecogs.com/svg.image?\small&space;%&space;target_{1,&space;sector_i}&space;=&space;\frac{&space;target_{1,&space;sector_i}}{&space;target_{1}}&space;" title="https://latex.codecogs.com/svg.image?\small % target_{1, sector_i} = \frac{ target_{1, sector_i}}{ target_{1}} " />

Para este problema, no segmento feminino:

<img src="https://latex.codecogs.com/svg.image?\small&space;%&space;survived_{1,&space;female}&space;=&space;\frac{&space;survived_{1,&space;female}}{&space;survived_{1}&space;}&space;=&space;\frac{233}{233&plus;109}&space;\approx&space;0.681287" title="https://latex.codecogs.com/svg.image?\small % survived_{1, female} = \frac{ survived_{1, female}}{ survived_{1} } = \frac{233}{233+109} \approx 0.681287" />

| Segmento | # <img src="https://latex.codecogs.com/svg.image?\small&space;target_{0}" title="https://latex.codecogs.com/svg.image?\small target_{0}" /> | # <img src="https://latex.codecogs.com/svg.image?target_{1}" title="https://latex.codecogs.com/svg.image?target_{1}" /> | % <img src="https://latex.codecogs.com/svg.image?\small&space;target_{0}" title="https://latex.codecogs.com/svg.image?\small target_{0}" /> | % <img src="https://latex.codecogs.com/svg.image?target_{1}" title="https://latex.codecogs.com/svg.image?target_{1}" /> |
|--------:|--------:|--------:|--------:|--------:|
| female  | 81      | 233     | <img src="https://latex.codecogs.com/svg.image?\tiny&space;\frac{81}{594}" title="https://latex.codecogs.com/svg.image?\tiny \frac{81}{594}" /> | <img src="https://latex.codecogs.com/svg.image?\tiny&space;\frac{233}{342}" title="https://latex.codecogs.com/svg.image?\tiny \frac{233}{342}" /> |
| male    | 468     | 109     | <img src="https://latex.codecogs.com/svg.image?\tiny&space;\frac{468}{594}" title="https://latex.codecogs.com/svg.image?\tiny \frac{468}{594}" /> | <img src="https://latex.codecogs.com/svg.image?\tiny&space;\frac{109}{342}" title="https://latex.codecogs.com/svg.image?\tiny \frac{109}{342}" /> |
| Total   | 549     | 342     | 1 | 1 |


# Percentual da população no segmento de estudo:

O Percentual da População no segmento de estudo é uma medida que indica a proporção da população total representada por um setor específico:


<img src="https://latex.codecogs.com/svg.image?\small&space;%&space;population_{sector_i}&space;=&space;\frac{&space;population_{sector_i}}{&space;population}" title="https://latex.codecogs.com/svg.image?\small % population_{sector_i} = \frac{ population_{sector_i}}{ population}" />

Vamos calcular o percentual da população para o segmento escolhido, que é o feminino neste caso:

<img src="https://latex.codecogs.com/svg.image?\small&space;%&space;population_{female}&space;=&space;\frac{&space;population_{female}}{&space;population&space;}&space;=&space;\frac{81&space;&plus;&space;233}{81&space;&plus;&space;233&space;&plus;&space;468&space;&plus;109}&space;\approx&space;0.352413" title="https://latex.codecogs.com/svg.image?\small % population_{female} = \frac{ population_{female}}{ population } = \frac{81 + 233}{81 + 233 + 468 +109} \approx 0.352413" />

Agora, vamos à tabela que apresenta as estatísticas:

| Segmento |        # <img src="https://latex.codecogs.com/svg.image?\small&space;target_{0}" title="https://latex.codecogs.com/svg.image?\small target_{0}" />  |        # <img src="https://latex.codecogs.com/svg.image?target_{1}" title="https://latex.codecogs.com/svg.image?target_{1}" />|    % <img src="https://latex.codecogs.com/svg.image?\small&space;target_{0}" title="https://latex.codecogs.com/svg.image?\small target_{0}" />  |        % <img src="https://latex.codecogs.com/svg.image?target_{1}" title="https://latex.codecogs.com/svg.image?target_{1}" />|    % População   |
|---------:|---------:|---------:|---------:|---------:|---------:|
|   female | 81 | 233| 0.15 | 0.68| <img src="https://latex.codecogs.com/svg.image?\tiny&space;\frac{314}{891}" title="https://latex.codecogs.com/svg.image?\tiny \frac{314}{891}" /> |
|     male  | 468 | 109|0.85| 0.32| <img src="https://latex.codecogs.com/svg.image?\tiny&space;\frac{577}{891}" title="https://latex.codecogs.com/svg.image?\tiny \frac{577}{891}" /> |
|     Total | 549 | 342 | 1 | 1 |

Essa medida nos fornece informações valiosas sobre a representação do setor de estudo dentro da população geral. Compreender essa distribuição é crucial para realizar uma análise abrangente dos resultados e tirar conclusões significativas a partir dos dados.

# Distribuição dos rótulos dentro de cada segmento (Distr):

A distribuição para o setor 'i' pode ser calculada como a proporção do segmento em estudo com rótulo de não ocorrências em relação à proporção do setor 'i' nos com rótulo de ocorrências:

<img src="https://latex.codecogs.com/svg.image?\small&space;Distr_{sector_i}&space;=&space;\frac{&space;%&space;target_{0,&space;sector_i}}{&space;%&space;target_{1,&space;sector_i}}" title="https://latex.codecogs.com/svg.image?\small Distr_{sector_i} = \frac{ % target_{0, sector_i}}{ % target_{1, sector_i}}" />

Da mesma forma, a divisão das distribuições para a categoria feminina pode ser calculada como a porcentagem de mulheres entre os falecidos em comparação com a porcentagem de mulheres entre os sobreviventes:

<img src="https://latex.codecogs.com/svg.image?\small&space;Distr_{female}&space;=&space;\frac{&space;%&space;survived_{0,&space;female}}{&space;%&space;survived_{1,&space;female&space;}}&space;=&space;\frac{&space;\frac{81}{81&plus;468}}{&space;\frac{233}{233&plus;109}&space;}&space;\approx&space;0.216562" title="https://latex.codecogs.com/svg.image?\small Distr_{female} = \frac{ % survived_{0, female}}{ % survived_{1, female }} = \frac{ \frac{81}{81+468}}{ \frac{233}{233+109} } \approx 0.216562" />

| Segmento |        # <img src="https://latex.codecogs.com/svg.image?\small&space;target_{0}" title="https://latex.codecogs.com/svg.image?\small target_{0}" />  |        # <img src="https://latex.codecogs.com/svg.image?target_{1}" title="https://latex.codecogs.com/svg.image?target_{1}" /> |    % <img src="https://latex.codecogs.com/svg.image?\small&space;target_{0}" title="https://latex.codecogs.com/svg.image?\small target_{0}" />  |        % <img src="https://latex.codecogs.com/svg.image?target_{1}" title="https://latex.codecogs.com/svg.image?target_{1}" />|    % Population   | Distr |
|---------:|---------:|---------:|---------:|---------:|---------:| ---------:|
|   female | 81 | 233| 0.15 | 0.68| 0.35| <img src="https://latex.codecogs.com/svg.image?\tiny&space;\frac{0.15}{0.68}\" title="https://latex.codecogs.com/svg.image?\tiny \frac{0.15}{0.68}" /> |
|     male  | 468 | 109|0.85| 0.32| 0.65| <img src="https://latex.codecogs.com/svg.image?\tiny&space;\frac{0.85}{0.32}" title="https://latex.codecogs.com/svg.image?\tiny \frac{0.85}{0.32}" /> |
|     Total | 549 | 342 | 1 | 1 |1| |


# Peso da Evidência (WoE):

Ele pode ser calculado usando o logaritmo natural da 'Distr' para cada setor:

<img src="https://latex.codecogs.com/svg.image?\small&space;WoE_{sector_i}&space;=&space;ln\left&space;(&space;Distr_{sector_i}&space;\right&space;)" title="https://latex.codecogs.com/svg.image?\small WoE_{sector_i} = ln\left ( Distr_{sector_i} \right )" />

Vamos considerar o segmento feminino como exemplo:

<img src="https://latex.codecogs.com/svg.image?\small&space;WoE_{female}&space;=&space;ln\left&space;(&space;Distr_{female}&space;\right&space;)&space;\approx&space;-1.529877" title="https://latex.codecogs.com/svg.image?\small WoE_{female} = ln\left ( Distr_{female} \right ) \approx -1.529877" />

Agora, vamos examinar a tabela que apresenta as estatísticas:

| Segmento |        # <img src="https://latex.codecogs.com/svg.image?\small&space;target_{0}" title="https://latex.codecogs.com/svg.image?\small target_{0}" />  |        # <img src="https://latex.codecogs.com/svg.image?target_{1}" title="https://latex.codecogs.com/svg.image?target_{1}" />|    % <img src="https://latex.codecogs.com/svg.image?\small&space;target_{0}" title="https://latex.codecogs.com/svg.image?\small target_{0}" />  |        % <img src="https://latex.codecogs.com/svg.image?target_{1}" title="https://latex.codecogs.com/svg.image?target_{1}" /> |    % Population   | Distr |    WoE |
|---------:|---------:|---------:|---------:|---------:|---------:| ---------:|---------:|
|   female | 81 | 233| 0.15 | 0.68| 0.35| 0.22|ln(0.22) |
|     male  | 468 | 109|0.85| 0.32| 0.65| 2.67|ln(2.67) |
|     Total | 549 | 342 | 1 | 1 |1| |  |

Ao analisar os valores de WoE, podemos obter insights sobre a natureza discriminante das variáveis na previsão do resultado desejado.

# Valor da Informação (IV):

Ele pode ser calculado usando a seguinte fórmula:

<img src="https://latex.codecogs.com/svg.image?\small&space;IV_{sector_i}&space;=&space;WoE_{sector_i}&space;\times&space;(%&space;target_{0,&space;sector_i}&space;-&space;%&space;target_{1,&space;sector_i}&space;)" title="https://latex.codecogs.com/svg.image?\small IV_{sector_i} = WoE_{sector_i} \times (% target_{0, sector_i} - % target_{1, sector_i} )" />

Vamos considerar o segmento feminino como exemplo:

<img src="https://latex.codecogs.com/svg.image?\small&space;IV_{female}&space;=&space;WoE_{female}&space;\times&space;(\%&space;survived_{0,&space;female}&space;-&space;\%&space;survived_{1,&space;female}&space;)&space;&space;=&space;-1.529877&space;\times&space;(0.681287&space;-&space;0.147541&space;)&space;\approx&space;0.816566" title="https://latex.codecogs.com/svg.image?\small IV_{female} = WoE_{female} \times (\% survived_{0, female} - \% survived_{1, female} ) = -1.529877 \times (0.147541 - 0.681287 ) \approx 0.816565" />

| Segmento |        # <img src="https://latex.codecogs.com/svg.image?\small&space;target_{0}" title="https://latex.codecogs.com/svg.image?\small target_{0}" />  |        # <img src="https://latex.codecogs.com/svg.image?target_{1}" title="https://latex.codecogs.com/svg.image?target_{1}" />|    % <img src="https://latex.codecogs.com/svg.image?\small&space;target_{0}" title="https://latex.codecogs.com/svg.image?\small target_{0}" />  |        % <img src="https://latex.codecogs.com/svg.image?target_{1}" title="https://latex.codecogs.com/svg.image?target_{1}" />|    % Population   | Distr |    WoE | IV |
|---------:|---------:|---------:|---------:|---------:|---------:| ---------:|---------:|---------:|
|   female | 81 | 233| 0.15 | 0.68| 0.35| 0.22|-1.53| <img src="https://latex.codecogs.com/svg.image?\tiny&space;-1.53&space;\times&space;(0.68-0.15&space;)&space;" title="https://latex.codecogs.com/svg.image?\tiny -1.53 \times (0.68-0.15 ) " />|
|     male  | 468 | 109|0.85| 0.32| 0.65| 2.67|0.98| <img src="https://latex.codecogs.com/svg.image?\tiny&space;0.98&space;\times&space;(0.32-0.85&space;)" title="https://latex.codecogs.com/svg.image?\tiny 0.98 \times (0.32-0.85 )" />|
|     Total | 549 | 342 | 1 | 1 |1| |  | | |
Se você tiver interesse em verificar a classificação dos valores de IV, você pode encontrá-la neste [link](https://deborahbarbedo.github.io/pt/2023-05-08-Unpacking_WOE_and_IV).

A tabela com todas as métricas calculadas é a seguinte:

| Sector |        # <img src="https://latex.codecogs.com/svg.image?\small&space;target_{0}" title="https://latex.codecogs.com/svg.image?\small target_{0}" />  |        # <img src="https://latex.codecogs.com/svg.image?target_{1}" title="https://latex.codecogs.com/svg.image?target_{1}" />|    % <img src="https://latex.codecogs.com/svg.image?\small&space;target_{0}" title="https://latex.codecogs.com/svg.image?\small target_{0}" />  |        % <img src="https://latex.codecogs.com/svg.image?target_{1}" title="https://latex.codecogs.com/svg.image?target_{1}" />|    % Population  | Distr |    WoE | IV |
|---------:|---------:|---------:|---------:|---------:|---------:| ---------:|---------:|---------:|
|   female | 81 | 233| 0.15 | 0.68| 0.35| 0.22|-1.53| 0.82|
|     male  | 468 | 109|0.85| 0.32| 0.65| 2.67|0.98| 0.53|
|     Total | 549 | 342 | 1 | 1 |1| |  |  1.35|


Para facilitar o entendimento de WoE e IV, eu preparei um artigo informativo que explica esses conceitos de forma detalhada. Você pode acessá-lo [aqui](https://deborahbarbedo.github.io/pt/2023-05-08-Unpacking_WOE_and_IV). O objetivo desse post é fornecer uma explicação completa e esclarecer as nuances dessas métricas.

Além disso, se você precisa fazer esses cálculos usando Python, criei outro post com as fórmulas correspondentes. Você pode conferir esse recurso neste [link](https://deborahbarbedo.github.io/pt/2023-04-17-WoE_IV_Python_Function). Com ele, você poderá realizar os cálculos de forma eficiente.

Para obter suporte adicional, compilei diversos materiais complementares no [meu GitHub](https://github.com/DeborahBarbedo) sobre esse tema. Esses recursos estão disponíveis no [repositório de materiais de suporte](https://github.com/DeborahBarbedo/Supporting_materials/tree/main/IV_WoE) e foram criados para ajudar você a entender melhor e aplicar na prática os cálculos de IV e WoE.

Se tiver mais alguma dúvida ou precisar de mais informações, estou aqui para ajudar!


# Referências:

* Anderson, Raymond. The Credit Scoring Toolkit: Theory and Practice for Retail Credit Risk Management and Decision Automation. Oxford University Press, 2007.

* Siddiqi, Naeem. Credit Risk Scorecards: Developing and Implementing Intelligent Credit Scoring. Wiley, 2006.

* Sudarson Mothilal Thoppay (2015). woe: Computes Weight of Evidence and Information Values. R package version 0.2.
  [https://CRAN.R-project.org/package=woe](https://CRAN.R-project.org/package=woe)

* Thilo Eichenberg (2018). woeBinning: Supervised Weight of Evidence Binning of Numeric Variables and Factors. R package
  version 0.1.6. [https://CRAN.R-project.org/package=woeBinning](https://CRAN.R-project.org/package=woeBinning)