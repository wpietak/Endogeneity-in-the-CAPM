# **Endogeneity in the CAPM**

This repository involves my own research on the problem of endogeneity in the Capital Asset Pricing Model (CAPM). I present the problem and the sources of endogeneity in CAPM, show how to approximate the resulting estimation bias and assess its impact in certain uses, such as VaR, and propose ways of adjusting the estimator to reduce the bias. Apart from the basic definitions, all the ideas, formulas, derivations and calculations are developed fully by me, unless specified otherwise.

## Introduction

CAPM is a broadly utilized model in the financial industry. Although it was primarily meant to describe the expected returns of portfolios in the equilibrium in relation to the expected excess return of the market portfolio, its usefulness in terms of describing the structure of relationship between different market factors is very often leveraged. More specifically, an additional assumption of linear relationship is taken, and the risk-free rate is removed from the equation, so that a simple linear regression formula with a single explanatory variable and no intercept is obtained. 

$$E[r_i]=r_f+\beta_i(E[r_m]-r_f)$$
$$r_i = \beta_i r_I + \varepsilon_i$$
