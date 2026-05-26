---
# multilingual page pair id, this must pair with translations of this page. (This name must be unique)
lng_pair: 3_WoE_IV_Python_calculation
title: "Como Calcular WoE e IV: Guia Completo para Regressão Logística"


# post specific
# if not specified, .name will be used from _data/owner/[language].yml
#author: "Deborah Cholodoysky Barbedo Pereira"
# multiple category is not supported
category: Ciência de Dados
# multiple tag entries are possible
tags: [Python, Regressão Logística, Feature Engineering, Seleção de Variáveis, Weight of Evidence, Information Value, WoE, IV, Titanic Dataset, Credit Scoring]
# thumbnail image for post
img: ":IV_and_WoE.jpg"
img_alt: "Exemplo prático de cálculo de WoE e IV em Python usando o Titanic Dataset para modelos de credit scoring e regressão logística"
# disable comments on this page
#comments_disable: true

# publish date
date: 2023-06-12 19:57:53 +0900

# seo
# if not specified, date will be used.
#meta_modify_date: 2021-08-10 11:32:53 +0900
# check the meta_common_description in _data/owner/[language].yml
meta_description: "Aprenda a calcular Weight of Evidence (WoE) e Information Value (IV) em Python utilizando o dataset Titanic. Entenda como essas métricas ajudam na seleção de variáveis para modelos de regressão logística e credit scoring."
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

O **Weight of Evidence (WoE)** e o **Information Value (IV)** são métricas amplamente utilizadas em problemas de *credit scoring*, seleção de variáveis e modelos de regressão logística.

Essas técnicas são especialmente populares no mercado financeiro por permitirem medir o poder discriminatório de uma variável em relação a um evento de interesse.

<!-- outline-end -->

Tradicionalmente, os registros são divididos entre:

- **bons** (*good*) - clientes adimplentes;
- **maus** (*bad*) - clientes inadimplentes.

Neste artigo, utilizaremos o conjunto de dados da [competição Titanic do Kaggle](https://www.kaggle.com/competitions/titanic/data) para demonstrar, passo a passo, como calcular WoE e IV de maneira visual e intuitiva.

Nosso objetivo é transformar conceitos frequentemente vistos como abstratos em algo intuitivo, interpretável e facilmente aplicável na prática.

## Base de Dados Utilizada

Para demonstrar o cálculo do **Weight of Evidence (WoE)** e do **Information Value (IV)**, utilizaremos a variável **gênero** (`female` e `male`) do dataset Titanic em relação à sobrevivência dos passageiros.

A tabela abaixo apresenta a distribuição da variável alvo:

| Segmento | Não Sobreviveu (`target₀`) | Sobreviveu (`target₁`) |
|:----------|---------------------------:|-----------------------:|
| female    | 81                         | 233                    |
| male      | 468                        | 109                    |
| **Total** | **549**                    | **342**                |

Onde:

- `target₀` representa os passageiros que **não sobreviveram**;
- `target₁` representa os passageiros que **sobreviveram**.

## Percentual de Cada Classe por Segmento

O percentual de uma classe dentro de um segmento corresponde à proporção daquele grupo em relação ao total da respectiva classe. Essas proporções representam a **distribuição de não‑eventos** (`target₀`) e **eventos** (`target₁`) em cada segmento.


Para a classe `target₀`:

$$
\%target_{0,segment_i}
=
\frac{target_{0,segment_i}}{target₀}
$$

Considerando o segmento **female**:

$$
\%survived_{0,female}
=
\frac{81}{81 + 468}
=
\frac{81}{549}
\approx 0.1475
$$

Ou seja, aproximadamente **14,75%** dos passageiros que não sobreviveram eram mulheres.


---

Para a classe `target₁`:

$$
\%target_{1,segment_i}
=
\frac{target_{1,segment_i}}{target₁}
$$

No segmento **female**:

$$
\%survived_{1,female}
=
\frac{233}{233 + 109}
=
\frac{233}{342}
\approx 0.6813
$$

Assim, aproximadamente **68,13%** dos passageiros sobreviventes eram mulheres.


---

## Distribuição Percentual por Segmento

| Segmento | `target₀` | `target₁` | % `target₀` | % `target₁` |
|:----------|-----------:|-----------:|-------------:|-------------:|
| female    | 81         | 233        | 0.1475       | 0.6813       |
| male      | 468        | 109        | 0.8525       | 0.3187       |
| **Total** | **549**    | **342**    | **1.0**      | **1.0**      |

Observe que a soma das proporções de cada classe é igual a 1:

$$
\sum \%target₀ = 1
\quad\text{e}\quad
\sum \%target₁ = 1
$$

Essas distribuições serão utilizadas posteriormente no cálculo do **Weight of Evidence (WoE)** e do **Information Value (IV)**.


## Percentual da População por Segmento

O percentual populacional representa a participação de cada segmento em relação ao total da amostra. Essa medida será utilizada posteriormente no cálculo da **razão entre distribuições**, componente essencial do WoE.


Essa métrica é calculada por:

$$
\%population_{segment_i}
=
\frac{population_{segment_i}}{population}
$$

Para o segmento **female**:

$$
\%population_{female}
=
\frac{81 + 233}{81 + 233 + 468 + 109}
=
\frac{314}{891}
\approx 0.3524
$$

Portanto, mulheres representam aproximadamente **35,24%** da população analisada, ou seja, cerca de 35 a cada 100 passageiros.


---

### Distribuição Populacional por Segmento

| Segmento | População | % População |
|:----------|-----------:|-------------:|
| female    | 314        | 0.3524 |
| male      | 577        | 0.6476 |
| **Total** | **891**    | **1.0** |

Essa distribuição ajuda a entender a representatividade de cada segmento dentro da base de dados, sendo útil na interpretação do **Weight of Evidence (WoE)** e do **Information Value (IV)**.

Ela será comparada às distribuições de `target₀` e `target₁` para identificar diferenças entre a população geral e o comportamento das classes.


## Razão entre Distribuições (*Distribution Ratio*)

A razão entre distribuições compara a participação de um segmento entre as classes `target₀` e `target₁`.


Ela é definida por:

$$
Distr_{segment_i}
=
\frac{
\%target_{0,segment_i}
}{
\%target_{1,segment_i}
}
$$

Valores menores que 1 indicam que o segmento é proporcionalmente mais associado ao evento `target₁`, enquanto valores maiores que 1 indicam maior associação ao não‑evento `target₀`.

  | Distribution Ratio | Interpretação |
  |:---|:---|
  | Distr < 1 | o segmento é **menos representado** entre os não‑eventos (`target₀`) |
  | Distr > 1 | o segmento é **mais representado** entre os não‑eventos (`target₀`) |


Para o segmento **female**:

$$
Distr_{female}
=
\frac{0.1475}{0.6813}
\approx 0.2166
$$

Isso indica que a participação das mulheres entre os passageiros que não sobreviveram é proporcionalmente muito menor do que entre os passageiros que sobreviveram, ou seja, `female` está mais associada ao evento (`target₁`).

---

## Razão entre Distribuições por Segmento

| Segmento | % `target₀` | % `target₁` | `Distr` |
|:----------|-------------:|-------------:|---------:|
| female    | 0.1475 | 0.6813 | 0.2166 |
| male      | 0.8525 | 0.3187 | 2.6745 |
| **Total** | **1.0** | **1.0** | — |

Valores:

- menores que `1` indicam maior concentração relativa em `target₁`;
- maiores que `1` indicam maior concentração relativa em `target₀`.

## Weight of Evidence (WoE)


O **Weight of Evidence (WoE)** é obtido aplicando o logaritmo natural à razão entre distribuições. Ele quantifica a evidência de que um segmento está mais associado ao evento (`target₁`) ou ao não‑evento (`target₀`).

O uso do logaritmo torna a escala simétrica em torno de zero, o que facilita a interpretação e melhora a estabilidade em modelos lineares como a regressão logística.

Sua definição é dada por:

$$
WoE_{segment_i}
=
\ln\left(
Distr_{segment_i}
\right)
$$

Substituindo a definição de `Distr`:

$$
WoE_{segment_i}
=
\ln\left(
\frac{
\%target_{0,segment_i}
}{
\%target_{1,segment_i}
}
\right)
$$

Para o segmento **female**:

$$
WoE_{female}
=
\ln(0.2166)
\approx -1.5299
$$


O valor negativo indica que o segmento **female** possui maior concentração relativa em `target₁` do que em `target₀`, ou seja, há **evidência a favor do evento**.


---

## Weight of Evidence por Segmento

| Segmento | `Distr` | `WoE` |
|:----------|---------:|------:|
| female    | 0.2166 | -1.5299 |
| male      | 2.6745 | 0.9834 |
| **Total** | — | — |

Interpretação:

- `WoE < 0`: maior concentração relativa em `target₁`;
- `WoE > 0`: maior concentração relativa em `target₀`;
- `WoE ≈ 0`: distribuição semelhante entre as classes.

## Information Value (IV)

O **Information Value (IV)** mede o poder preditivo de uma variável em relação à variável alvo.
Ele combina a intensidade da evidência (WoE) com o tamanho da diferença entre as distribuições, capturando tanto a força quanto a relevância estatística do segmento.

A contribuição de cada segmento para o IV é calculada por:

$$
IV_{segment_i}
=
WoE_{segment_i}
\times
\left(
\%target_{0,segment_i}
-
\%target_{1,segment_i}
\right)
$$

Substituindo a definição de `WoE`:

$$
IV_{segment_i}
=
\ln\left(
\frac{
\%target_{0,segment_i}
}{
\%target_{1,segment_i}
}
\right)
\times
\left(
\%target_{0,segment_i}
-
\%target_{1,segment_i}
\right)
$$

Para o segmento **female**:

$$
IV_{female}
=
-1.5299
\times
(0.1475 - 0.6813)
\approx 0.8166
$$

---

### Contribuição do IV por Segmento

| Segmento | `WoE` | IV |
|:----------|------:|------:|
| female    | -1.5299 | 0.8166 |
| male      | 0.9834  | 0.5244 |
| **Total IV** | — | **1.3410** |

O **Information Value total** da variável é obtido pela soma das contribuições de todos os segmentos:


$$
IV
=
\sum_i IV_{segment_i}
$$

Como o IV é aditivo, variáveis com muitos segmentos podem inflar artificialmente o valor total.

Em geral:

- `IV < 0.02`: variável sem poder preditivo;
- `0.02 ≤ IV < 0.1`: poder fraco;
- `0.1 ≤ IV < 0.3`: poder médio;
- `0.3 ≤ IV < 0.5`: poder forte;
- `IV ≥ 0.5`: poder muito forte.

No exemplo apresentado, a variável **gênero** possui um IV igual a **1.3410**, indicando altíssimo poder discriminatório, valores tão elevados podem inclusive sugerir *data leakage* em cenários reais.


## Considerações Importantes

Ao utilizar **Weight of Evidence (WoE)** e **Information Value (IV)**, alguns cuidados são importantes:

- categorias com baixa frequência podem gerar valores instáveis;
- divisões por zero devem ser tratadas adequadamente;
- variáveis com IV excessivamente alto podem indicar *data leakage*;
- o WoE é amplamente utilizado em regressão logística por favorecer relações mais lineares entre as variáveis e o logit;
- processos de *binning* podem impactar significativamente os valores de WoE e IV.

Embora WoE tenha sido criado para regressão logística, ele também pode ser útil em modelos de árvore ao reduzir cardinalidade e ruído.

---

## Recursos Complementares

Caso queira aprofundar seus conhecimentos sobre WoE e IV:

- [Entendendo WoE e IV](https://deborahbarbedo.github.io/pt/2023-05-08-Unpacking_WOE_and_IV)
- [Funções em Python para cálculo de WoE e IV](https://deborahbarbedo.github.io/pt/2023-04-17-WoE_IV_Python_Function)
- [Materiais complementares no GitHub](https://github.com/DeborahBarbedo/Supporting_materials/tree/main/IV_WoE)

---

## Referências

- Anderson, Raymond. *The Credit Scoring Toolkit: Theory and Practice for Retail Credit Risk Management and Decision Automation*. Oxford University Press, 2007.

- Siddiqi, Naeem. *Credit Risk Scorecards: Developing and Implementing Intelligent Credit Scoring*. Wiley, 2006.

- Thoppay, Sudarson Mothilal (2015). *woe: Computes Weight of Evidence and Information Values*. R package version 0.2.  
  https://CRAN.R-project.org/package=woe

- Eichenberg, Thilo (2018). *woeBinning: Supervised Weight of Evidence Binning of Numeric Variables and Factors*. R package version 0.1.6.  
  https://CRAN.R-project.org/package=woeBinning