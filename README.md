![image](https://github.com/user-attachments/assets/55b5365d-eb3b-46aa-96ad-0defcc98a590)# **Endogeneity in the CAPM**

This repository involves my own research on the problem of endogeneity in the Capital Asset Pricing Model (CAPM). I present the problem and the sources of endogeneity in CAPM, show how to approximate the resulting estimation bias and assess its impact in certain uses, such as VaR, and propose ways of adjusting the estimator to reduce the bias. Apart from the basic definitions, all the ideas, formulas, derivations and calculations are developed fully by me, unless specified otherwise.

## Introduction

CAPM is a broadly utilized model in the financial industry. Although it was primarily meant to describe the expected returns of portfolios in the equilibrium in relation to the expected excess return of the market portfolio, its usefulness in terms of describing the structure of relationship between different market factors is very often leveraged. 

$$E[r_i]=r_f+\beta_i(E[r_m]-r_f)$$

More specifically, an additional assumption of linear relationship between the returns is taken, so that a simple linear regression formula with a single explanatory variable is obtained. Additionally, the risk-free rate can be omitted in the equation, and in certain cases (such as risk modelling) the intercept can be assumed to be equal to zero. Note that, if daily returns are considered, their expected values will usually be very close to 0 (especially for the risk-free rate), which justifies such approach to the analysis. Such model is sometimes referred to as Beta model. The most common practice is to take some major index on a given market as a proxy for the market portfolio, and use its returns ($r_I$) as an explanatory variable, which is supposed to represent the systematic component of a given market factor. 

$$r_i = \alpha_i + \beta_i r_I + \varepsilon_i$$

However, one should note that indices, or any other proxies of market portfolios, are never equivalent to the systematic factors. Otherwise, the relationship described by the above formula would lead to contradictions:
- For the market factors (e.g., stocks) constituting the corresponding index, how is it possible that the moves of this index cause their moves if they have been used to construct it in the first place?
- Even if a given market factor does not constitute the index, how would this causal relationship work?

In reality, there is a latent (unobservable) variable representing the systemic factor, which impacts the moves of particular market factors. It can be thought of as the extent of market participants decisions which is due to their perspective on general conditions on a given market.

## True DGP in the Beta Model

We assume we have a homogenous cluster of $𝑁$ risk factors, which returns are dependent on a systematic component ($𝑠$) and an idiosyncratic component. There is a single systematic component for all risk factors in the cluster, and there is a unique idiosyncratic component for each risk factor. Idiosyncratic components of each two different risk factors are independent. Thus, we have the following true Data Generating Process (DGP) in our Beta Model:

$$𝑟_𝑖=𝛽_𝑖 𝑟_𝑠+𝜀_𝑖,$$

where:
- $𝑟_𝑖$ – return of the risk factor $𝑖$, $𝑖=1,…,𝑁$;
- $𝛽_𝑖$ – beta coefficient of risk factor $𝑖$;
- $𝑟_𝑠$ – return of the systematic component;
- $𝜀_𝑖$ – idiosyncratic component of the risk factor $𝑖$ return.

Distinction into systematic component and idiosyncratic component is abstract. Return of the systemic component is unobservable (as is the idiosyncratic component). In reality we can only observe return of an index ($𝐼$), which is supposed to reflect the impact of systematic component. We assume that return of the index is a weighted average of the returns of $𝑛$ risk factors belonging to the cluster:

$$𝑟_𝐼 = 𝑟_1 𝑤_1 + 𝑟_2 𝑤_2 + … + 𝑟_𝑛 𝑤_𝑛 = \sum_{𝑗=1}^{𝑛} \left(𝛽_𝑗 𝑟_𝑠 + 𝜀_𝑗 \right) 𝑤_𝑗 = \sum_{𝑗=1}^{𝑛} \left(𝛽_𝑗×𝑟_𝑠 \right) 𝑤_𝑗 + \sum_{𝑗=1}^{𝑛} 𝜀_𝑗×𝑤_𝑗,$$

where:
- $𝑟_𝐼$ – return of the index $𝐼$;
- $𝑤_𝑗$ – weight of the risk factor $𝑗$, $𝑗=1,…,𝑛≤𝑁$;
- $𝑤_1+𝑤_2+…+𝑤_𝑛=1$.

In general, the higher the $𝑛$ is (or, the lower the weights are), the better the index reflects the systematic component.

## **The phenomena of endogeneity and its implications**

An explanatory variable is said to be endogenous when it is correlated with error term, i.e., $𝐸[𝜀|𝑥]≠0$.

Consequences:

1. Bias of the least squares estimator:

$$ E\left[{\hat{𝛽}}^{𝑂𝐿𝑆} \right] = 𝛽 + (𝑿^{𝑇} 𝑿)^{−1} 𝑿^{𝑇} 𝐸[\varepsilon|𝑿] $$

2. Inconsistency of the least squares estimator:

$$ p-\lim_{N \to {+\infty}} ⁡{\hat{𝛽}}^{𝑂𝐿𝑆} = \beta + p-\lim_{N \to {+\infty}}⁡ { \left( \frac{1}{N} 𝑿^{𝑇} 𝑿 \right)^{−1} \frac{1}{N} 𝑿^{𝑇} 𝐸[\varepsilon|𝑿]} $$

Standard sources of endogeneity:

1) <b>Omitted variable bias</b>

2) <b>Measurement error</b>

3) Simultaneous causality

## **Sources of endogeneity in the Beta Model**

1. Omitted variable in the Beta model – idiosyncratic component

$$ 𝑐𝑜𝑣(𝑟_𝐼,𝜀_(𝑘≤𝑛) )=𝑐𝑜𝑣(∑_(𝑗=1)^𝑛▒〖(𝛽_𝑗×𝑟_𝑠 )×𝑤_𝑗 〗+∑_(𝑗=1)^𝑛▒〖𝜀_𝑗×𝑤_𝑗 〗,𝜀_(𝑘≤𝑛) )=𝐸[𝑤_(𝑘≤𝑛)×𝜀_(𝑘≤𝑛)^2 ]≠0 $$

2. Measurement error – index is not equivalent to the systemic component

$$ 𝑟_𝑖 = 𝛽_𝑖 𝑟_𝑠 + 𝜀_𝑖 $$
$$ 𝑟_𝐼=𝑟_𝑠×∑_(𝑗=1)^𝑛▒〖𝛽_𝑗×𝑤_𝑗 〗+∑_(𝑗=1)^𝑛▒〖𝜀_𝑗×𝑤_𝑗 〗⇒𝑟_𝑠=1/(∑_(𝑗=1)^𝑛▒〖𝛽_𝑗×𝑤_𝑗 〗)×𝑟_𝐼−(∑_(𝑗=1)^𝑛▒〖𝜀_𝑗×𝑤_𝑗 〗)/(∑_(𝑗=1)^𝑛▒〖𝛽_𝑗×𝑤_𝑗 〗) $$
$$ 𝑟_𝑖=𝛽_𝑖×(1/(∑_(𝑗=1)^𝑛▒〖𝛽_𝑗×𝑤_𝑗 〗)×𝑟_𝐼−(∑_(𝑗=1)^𝑛▒〖𝜀_𝑗×𝑤_𝑗 〗)/(∑_(𝑗=1)^𝑛▒〖𝛽_𝑗×𝑤_𝑗 〗))+𝜀_𝑖=𝛽_𝑖/(∑_(𝑗=1)^𝑛▒〖𝛽_𝑗×𝑤_𝑗 〗)×𝑟_𝐼+(−𝛽_𝑖/(∑_(𝑗=1)^𝑛▒〖𝛽_𝑗×𝑤_𝑗 〗)×(∑_(𝑗=1)^𝑛▒〖𝜀_𝑗×𝑤_𝑗 〗)+𝜀_𝑖 ) $$

Even if $𝑖>𝑛$, we have: 

$$ 𝑐𝑜𝑣(𝑟_𝑠×∑_(𝑗=1)^𝑛▒〖𝛽_𝑗×𝑤_𝑗 〗+∑_(𝑗=1)^𝑛▒〖𝜀_𝑗×𝑤_𝑗 〗,(−𝛽_𝑖/(∑_(𝑗=1)^𝑛▒〖𝛽_𝑗×𝑤_𝑗 〗)×(∑_(𝑗=1)^𝑛▒〖𝜀_𝑗×𝑤_𝑗 〗)+𝜀_𝑖 ))=−𝛽_𝑖/(∑_(𝑗=1)^𝑛▒〖𝛽_𝑗×𝑤_𝑗 〗)×𝐸[(∑_(𝑗=1)^𝑛▒〖𝜀_𝑗×𝑤_𝑗 〗)^2 ]≠0 $$

First issue concerns only index components, while the second concerns all risk factors in a given cluster. 














