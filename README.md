# **Endogeneity in the CAPM**

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

We assume we have a homogenous cluster of $𝑁$ market factors, which returns are dependent on a systematic component ($𝑠$) and an idiosyncratic component. There is a single systematic component for all market factors in the cluster, and there is a unique idiosyncratic component for each market factor. Idiosyncratic components of each two different market factors are independent. Thus, we have the following true Data Generating Process (DGP) in our Beta Model:

$$𝑟_𝑖=𝛽_𝑖 𝑟_𝑠+𝜀_𝑖,$$

where:
- $𝑟_𝑖$ – return of the market factor $𝑖$, $𝑖=1,…,𝑁$;
- $𝛽_𝑖$ – beta coefficient of market factor $𝑖$;
- $𝑟_𝑠$ – return of the systematic component;
- $𝜀_𝑖$ – idiosyncratic component of the market factor $𝑖$ return.

Distinction into systematic component and idiosyncratic component is abstract. Return of the systemic component is unobservable (as is the idiosyncratic component). In reality we can only observe return of an index ($𝐼$), which is supposed to reflect the impact of systematic component. We assume that return of the index is a weighted average of the returns of $𝑛$ market factors belonging to the cluster:

$$𝑟_𝐼 = 𝑟_1 𝑤_1 + 𝑟_2 𝑤_2 + … + 𝑟_𝑛 𝑤_𝑛 = \sum_{𝑗=1}^{𝑛} \left(𝛽_𝑗 𝑟_𝑠 + 𝜀_𝑗 \right) 𝑤_𝑗 = \sum_{𝑗=1}^{𝑛} \left(𝛽_𝑗×𝑟_𝑠 \right) 𝑤_𝑗 + \sum_{𝑗=1}^{𝑛} 𝜀_𝑗×𝑤_𝑗,$$

where:
- $𝑟_𝐼$ – return of the index $𝐼$;
- $𝑤_𝑗$ – weight of the market factor $𝑗$, $𝑗=1,…,𝑛≤𝑁$;
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

$$ 𝑐𝑜𝑣 \left(𝑟_𝐼, 𝜀_{𝑘≤𝑛} \right) = 𝑐𝑜𝑣 \left( \sum_{𝑗=1}^{𝑛} 𝑤_𝑗 𝛽_𝑗 𝑟_𝑠 + \sum_{𝑗=1}^{𝑛} 𝜀_𝑗 𝑤_𝑗, 𝜀_{𝑘≤𝑛} \right) = 𝐸 \left[ 𝑤_{𝑘≤𝑛} 𝜀_{𝑘≤𝑛}^{2} \right] ≠ 0 $$

2. Measurement error – index is not equivalent to the systemic component

$$ 𝑟_𝑖 = 𝛽_𝑖 𝑟_𝑠 + 𝜀_𝑖 $$
$$ 𝑟_𝐼 = 𝑟_𝑠 \sum_{𝑗=1}^{𝑛} 𝛽_𝑗 𝑤_𝑗 + \sum_{𝑗=1}^{𝑛} 𝜀_𝑗 𝑤_𝑗 ⇒ 𝑟_𝑠 = \frac{𝑟_𝐼}{\sum_{𝑗=1}^{𝑛} 𝛽_𝑗 𝑤_𝑗} − \frac{\sum_{𝑗=1}^{𝑛} 𝜀_𝑗 𝑤_𝑗}{\sum_{𝑗=1}^{𝑛} 𝛽_𝑗 𝑤_𝑗} $$
$$ 𝑟_𝑖 = 𝛽_𝑖 \left( \frac{𝑟_𝐼}{\sum_{𝑗=1}^{𝑛} 𝛽_𝑗 𝑤_𝑗} − \frac{\sum_{𝑗=1}^{𝑛} 𝜀_𝑗 𝑤_𝑗}{\sum_{𝑗=1}^{𝑛} 𝛽_𝑗 𝑤_𝑗} \right) + 𝜀_𝑖 = \frac{𝛽_𝑖 𝑟_𝐼}{\sum_{𝑗=1}^{𝑛} 𝛽_𝑗 𝑤_𝑗} + \left( − \frac{𝛽_𝑖}{\sum_{𝑗=1}^{𝑛} 𝛽_𝑗 𝑤_𝑗} \left( \sum_{𝑗=1}^{𝑛} 𝜀_𝑗 𝑤_𝑗 \right) + 𝜀_𝑖 \right) $$

Even if $𝑖>𝑛$, we have: 

$$ 𝑐𝑜𝑣 \left( \sum_{𝑗=1}^{𝑛} 𝑤_𝑗 𝛽_𝑗 𝑟_𝑠 + \sum_{𝑗=1}^{𝑛} 𝜀_𝑗 𝑤_𝑗 , \left( − \frac{𝛽_𝑖}{\sum_{𝑗=1}^{𝑛} 𝛽_𝑗 𝑤_𝑗} \left( \sum_{𝑗=1}^{𝑛} 𝜀_𝑗 𝑤_𝑗 \right) + 𝜀_𝑖 \right) \right) = - \frac{𝛽_𝑖}{\sum_{𝑗=1}^{𝑛} 𝛽_𝑗 𝑤_𝑗} 𝐸 \left[ \left( \sum_{𝑗=1}^{𝑛} 𝜀_𝑗 𝑤_𝑗 \right)^{2} \right] ≠ 0 $$

The first issue concerns only index components, while the second concerns all market factors in a given cluster. 


## **Coefficients in a simple framework**

Consider the following framework. We have $𝑁≥2$ market factors in a cluster, $𝑛∈[2,𝑁]$ of them compose an index. We assume all weights are equal, i.e., $𝑤_𝑗=\frac{1}{𝑛}$ for $𝑗=1,…,𝑛$, and all betas are equal, i.e., $𝛽_𝑖=𝛽$ for $𝑖=1,…,𝑁$. Additionally, we assume all market factors’ returns, as well as the systematic component return, follow a standard normal distribution, i.e., $𝑟_𝑠~𝑁(0,1)$ and $𝑟_𝑖~𝑁(0,1)$ for $𝑖=1,…,𝑁$. We want to calibrate coefficients of correlation with the systematic component for all market factors in a cluster over some time interval $𝑇$. 

Variance of the idiosyncratic component:

$$ \forall_{𝑖=1,…,𝑁} 𝜎_{𝑖}^{2} ≡ 𝑣𝑎𝑟(𝑟_𝑖) = 𝑣𝑎𝑟 \left( 𝛽 𝑟_𝑠 + 𝜀_𝑖 \right) = 1 ⟹ 𝑣𝑎𝑟 (𝜀_𝑖) = 1−𝛽^2 $$

Index return and its variance:

$$ 𝑟_𝐼 = \frac{(𝛽𝑟_𝑠+𝜀_1 ) + (𝛽𝑟_𝑠+𝜀_2) + … + (𝛽𝑟_𝑠+𝜀_𝑛)}{𝑛} = 𝛽 𝑟_𝑠 + \frac{1}{𝑛} \sum_{𝑗=1}^{𝑛} 𝜀_𝑗 $$

$$ 𝜎_{𝐼}^{2} ≡ 𝑣𝑎𝑟(𝑟_𝐼 ) = 𝑣𝑎𝑟 \left( 𝛽 𝑟_𝑠 + \frac{1}{𝑛} \sum_{𝑗=1}^{𝑛} 𝜀_𝑗 \right) = 𝛽^2 + \frac{1}{𝑛^2} \sum_{𝑗=1}^{𝑛} \left( 1−𝛽^2 \right) = 𝛽^2 + \frac{1−𝛽^2}{𝑛} $$

Covariance and correlation of a component market factor return:

$$ 𝑐𝑜𝑣 \left( 𝑟_𝐼 , 𝑟_{𝑘≤𝑛} \right) = 𝐸 \left[ \left( 𝛽 𝑟_𝑠 + \frac{1}{𝑛} \sum_{𝑗=1}^{𝑛} 𝜀_𝑗 \right) \left( 𝛽 𝑟_𝑠 + 𝜀_{𝑘≤𝑛} \right) \right] = 𝐸 \left[ 𝛽^2 𝑟_{𝑠}^{2} + \frac{𝜀_{𝑘≤𝑛}^{2}}{𝑛} \right] = 𝛽^2 + \frac{1−𝛽^2}{𝑛} $$

$$ 𝜌_{𝑟_𝐼, 𝑟_{𝑘≤𝑛}} = \frac{𝛽^2 + \frac{1−𝛽^2}{𝑛}}{\sqrt{𝛽^2 + \frac{1−𝛽^2}{𝑛}}} = sqrt{\frac{(𝑛−1) 𝛽^2 + 1}{𝑛}} ⟹ 𝛽 = \sqrt{ \frac{ 𝑛 𝜌_{𝑟_𝐼, 𝑟_{𝑘≤𝑛}}^{2} − 1}{𝑛−1} } $$

Note that, since the volatilities of all market factors and of the systematic component are equal to 1, betas are equal to correlations

$$ \tilde{ \hat{ 𝜌_{𝑟_𝐼, 𝑟_{𝑘≤𝑛}} } } = \sqrt{ \frac{ 𝑛 \hat{ 𝜌_{𝑟_𝐼, 𝑟_{𝑘≤𝑛}} }^{2} − 1}{𝑛−1} } $$

Covariance and correlation of a non-component market factor return:

$$ 𝑐𝑜𝑣 \left( 𝑟_𝐼 , 𝑟_{𝑘>𝑛} \right) = 𝐸 \left[ \left( 𝛽 𝑟_𝑠 + \frac{1}{𝑛} \sum_{𝑗=1}^{𝑛} 𝜀_𝑗 \right) \left( 𝛽 𝑟_𝑠 + 𝜀_{𝑘>𝑛} \right) \right] = 𝐸 \left[ 𝛽^2 𝑟_{𝑠}^{2} \right] = 𝛽^2 $$

$$ 𝜌_{𝑟_𝐼 , 𝑟_{𝑘>𝑛} } = \frac {𝛽^2/√(𝛽^2+(1−𝛽^2)/𝑛)⟹𝛽=√(𝜌_(𝑟_𝐼,𝑟_(𝑘>𝑛) ) 𝜎_𝐼 ) $$

$$ ^𝑎𝑑𝑗 (𝜌_(𝑟_𝐼,𝑟_(𝑘>𝑛) ) ) ̂=√((𝜌_(𝑟_𝐼,𝑟_(𝑘>𝑛) ) ) ̂𝜎_𝐼 ) $$
















