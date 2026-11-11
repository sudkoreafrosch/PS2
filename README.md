---
title: "PSIR Graduate Methods Problem Set 2"
author: "Seo-young Silvia Kim"
date: "`r format(Sys.Date(), '%Y-%m-%d')`"
colorlinks: true
urlcolor: blue
linkcolor: black
citecolor: blue
header-includes:
- \usepackage{amsmath}
- \usepackage{amssymb}
- \usepackage{amsthm}
output:
  pdf_document:
    toc: yes
    toc_depth: '3'

---

# Instructions

* All answers must be typed up and submitted electronically as a PDF file, either via LaTeX or R Markdown. Submit the corresponding .Rmd or .R file as well.
* Most problem sets should be completed within the individual assigned groups. Collaboration is not allowed for the problem sets between set groups/individuals.
* Substitute "author" with your own names for the submission.
* Problem sets will be completed in groups of 4--5.

All datasets in this problem set come from the `wooldridge` package. Load it once at the top of your document; run `install.packages("wooldridge")` first if it is not installed.

\newpage

# Interactions

## Question 1

We will quickly check the Frisch-Waugh-Lovell theorem in practice, using the dataset from Wooldridge Example 3.2 (`wage1`).

```{r}
library(wooldridge)
data(wage1)
names(wage1) ## see what variables are included
```

```{r, eval = FALSE}
?wage1 ## data description; not evaluated when knitting
```

Confirm the partialling out interpretation of the OLS estimates by explicitly doing the partialling out for Example 3.2. This first requires regressing `educ` on `exper` and `tenure` and saving the residuals, $\hat{r}_1$. Then, regress `log(wage)` on $\hat{r}_1$. Compare the coefficient on $\hat{r}_1$ with the coefficient on `educ` in the regression of `log(wage)` on `educ`, `exper`, and `tenure.`

------------------------------------------------------------------------

[PUT YOUR ANSWER HERE]
두 계수는 0.0920으로 정확히 일치한다. $\hat{r}_1$은 educ에서 exper, tenure로 설명되는 부분을 걷어 내고 남은 부분이다. 다중회귀에서 educ의 계수도 바로 이 남은 부분으로 계산되므로 두 값이 같아진다. 이것이 FWL 정리이다.

```{r}
full <- lm(log(wage) ~ educ + exper + tenure, data = wage1)

wage1$r1 <- resid(lm(educ ~ exper + tenure, data = wage1))
fwl <- lm(log(wage) ~ r1, data = wage1)

c(educ_full = unname(coef(full)["educ"]), r1_partialled = unname(coef(fwl)["r1"]))
```

------------------------------------------------------------------------

## Question 2

We will now implement an interaction model, again with the `wage1` dataset.

1. You suspect that the predictors used in Question 1 may have differential effects by gender. Set up a model with interaction effects and write the equation. Justify the model. (There's no single answer to this. Write a new model that makes sense to you!)

2. Estimate the model and interpret the results, especially the coefficient for the interaction term(s).

------------------------------------------------------------------------

[PUT YOUR ANSWER HERE]
1. female을 question 1의 모든 설명변수와 상호작용시킨다.

$$\begin{aligned}\log(wage) = {} & \beta_0 + \delta_0 female + \beta_1 educ + \beta_2 exper + \beta_3 tenure \\& + \delta_1 female \cdot educ + \delta_2 female \cdot exper + \delta_3 female \cdot tenure + u .\end{aligned}$$

female 더미만 넣으면 교육, 경력, 근속 1년이 임금을 올리는 폭이 남녀 모두 같다고 가정하게 된다. 이 경우 성별 임금격차는 교육이나 경력 수준과 상관없이 늘 일정하다. 그러나 같은 1년이 지나도 여성의 임금은 덜 오를 수 있다. 여성이 임금이 낮은 직종에 몰려 있거나 승진에서 불리하면 교육과 근속의 효과가 작아진다. 

이 모형에서 각 변수 1년의 효과는 남성이 $\beta_j$, 여성이 $\beta_j + \delta_j$이다. 즉 상호작용 계수 $\delta_j$는 남녀 간 효과의 차이이며, $H_0: \delta_1 = \delta_2 = \delta_3 = 0$은 세 변수의 효과가 남녀 간에 같은지를 검정한다.

2. 결과(n = 526, $R^2 = 0.403$)
상호작용항 계수는 여성과 남성의 효과 차이이다. 세 계수가 모두 음수이므로 교육, 경력, 근속이 1년 늘 때 여성의 임금은 남성보다 덜 오른다. 예컨대 경력 1년은 남성 임금을 약 0.8% 올리지만 여성 임금은 약 0.2%만 올린다.

```{r}
m_add <- lm(log(wage) ~ female + educ + exper + tenure, data = wage1)
m_int <- lm(log(wage) ~ female * (educ + exper + tenure), data = wage1)
round(summary(m_int)$coefficients, 4)
```

------------------------------------------------------------------------

## Question 3

We will use the `wage2` dataset now and try to explain variations in `wage`.

```{r}
data(wage2)
names(wage2)
```

```{r, eval = FALSE}
?wage2 ## data description; not evaluated when knitting
```

The following model allows the return to education to depend upon the total amount of both parents' education, called `pareduc`:

$$log(wage) = \beta_0 + \beta_1 educ + \beta_2 educ \cdot pareduc + \beta_3 exper + \beta_4 tenure + u$$

Note that the lower-level term for `pareduc` is not contained in this model.

1. What is the return to another year of education?
2. What sign do you expect for $\beta_2$? Why?
3. Using the data in `wage2`, the estimated equation is

$$
\widehat{\log(wage)} = \underset{(0.13)}{5.65} + \underset{(0.010)}{0.047}\, educ + \underset{(0.00021)}{0.00078}\, educ \cdot pareduc + \underset{(0.004)}{0.019}\, exper + \underset{(0.003)}{0.010}\, tenure,
$$

with $n = 722$ and $R^2 = 0.169$. Standard errors are in parentheses. Only 722 observations have complete information on both parents' education.

First, replicate the result. To see how you would drop the lower term, see [this Stack Overflow post](https://stackoverflow.com/questions/40729701/how-to-use-formula-in-r-to-exclude-main-effect-but-retain-interaction).

The dataset does not contain the `pareduc` variable, so it must be created:

```{r}
wage2$pareduc <- wage2$feduc + wage2$meduc
```

4. Interpret the coefficient on the interaction term. It might help to choose two specific values for `pareduc`---for example, `pareduc = 32` if both parents have a college education, or `pareduc = 24` if both parents have a high school education---and to compare the estimated return to `educ`.
5. Now add the missing lower-term `pareduc` back in as a separate variable. Run the regression.
6. Does the estimated return to education now depend positively on parent education? Test the null hypothesis that the return to education does not depend on parent education.

------------------------------------------------------------------------

[PUT YOUR ANSWER HERE]
**1.** $\partial \log(wage) / \partial educ = \beta_1 + \beta_2\, pareduc$이다. 즉 `exper`와 `tenure`가 같을 때 교육 1년은 임금을 약 $100(\beta_1 + \beta_2\, pareduc)\%$ 올리며, 그 크기가 부모의 교육수준에 따라 달라진다.

**2.** $\beta_2 > 0$으로 예상한다. 학력이 높은 부모일수록 좋은 학교, 교육 자원, 진로 조언, 인맥을 제공하기 쉬우므로, 자녀는 같은 1년의 교육으로 더 높은 임금을 얻을 것이다.

**3.** 아래 회귀 결과는 제시된 식과 같다. 계수는 $5.65$, $0.047$, $0.00078$, $0.019$, $0.010$이고 $n = 722$, $R^2 = 0.169$이다.

**4.** 부모의 교육연수 합이 1년 늘면 교육 1년의 수익률이 약 0.078%p 커진다 ($t = 3.68$). 부모가 모두 대졸이면 교육 1년의 수익률이 약 0.6%p 높다.

**5.** `pareduc`를 따로 넣어 추정한 결과는 다음과 같다 ($n = 722$, $R^2 = 0.174$).
$$\widehat{\log(wage)} = \underset{(0.38)}{4.94} + \underset{(0.027)}{0.097}\, educ + \underset{(0.017)}{0.033}\, pareduc - \underset{(0.0012)}{0.0016}\, educ \cdot pareduc + \underset{(0.004)}{0.020}\, exper + \underset{(0.003)}{0.010}\, tenure .$$

**6.** 아니다. 상호작용 계수가 음수로 바뀌어, 수익률 $0.097 - 0.0016\, pareduc$은 오히려 부모 학력이 높을수록 *작아진다* (`pareduc = 24`에서 5.9%, `pareduc = 32`에서 4.7%). 다만 이 계수는 유의하지 않다. $H_0: \beta_{educ \cdot pareduc} = 0$을 양측 검정하면 $t = -1.31$, 자유도 716, $p = 0.19$이므로 귀무가설을 기각하지 못한다. 즉 교육수익률이 부모 학력에 따라 달라진다는 증거는 없다.

```{r}
## Part 3: interaction without the lower-order pareduc term
m3 <- lm(log(wage) ~ educ + educ:pareduc + exper + tenure, data = wage2)
round(summary(m3)$coefficients, 5)
c(n = nobs(m3), R2 = round(summary(m3)$r.squared, 3))

## Part 4: return to educ at pareduc = 24 and 32
coef(m3)["educ"] + coef(m3)["educ:pareduc"] * c(hs = 24, college = 32)

## Parts 5-6: add the lower-order term back in
m3_full <- lm(log(wage) ~ educ * pareduc + exper + tenure, data = wage2)
round(summary(m3_full)$coefficients, 5)
c(n = nobs(m3_full), R2 = round(summary(m3_full)$r.squared, 3))
coef(m3_full)["educ"] + coef(m3_full)["educ:pareduc"] * c(hs = 24, college = 32)
```

------------------------------------------------------------------------

## Question 4

Consider a model in which the return to education depends upon the amount of work experience (and vice versa):

$$log(wage) = \beta_0 + \beta_1 educ + \beta_2 exper + \beta_3 educ \cdot exper + u$$

1. What is the return to another year of education, holding `exper` fixed?
2. State the null hypothesis that the return to education does not depend on the level of `exper`. What do you think is the appropriate alternative?
3. Use the data in `wage2` to test the null hypothesis in part 2 against your stated alternative.

------------------------------------------------------------------------

[PUT YOUR ANSWER HERE]
**1.** $\partial \log(wage) / \partial educ = \beta_1 + \beta_3\, exper$이다. 즉 교육 1년은 임금을 약 $100(\beta_1 + \beta_3\, exper)\%$ 올리며, 그 크기가 경력에 따라 달라진다.

**2.** $H_0: \beta_3 = 0$ (교육수익률은 경력과 무관하다), $H_1: \beta_3 > 0$. 교육을 많이 받은 사람일수록 일하면서 더 많이 배우고 승진도 빠르므로, 경력이 쌓일수록 교육의 효과가 커질 것으로 보기 때문이다.

**3.** $\hat\beta_3 = 0.00320$ (표준오차 $0.00153$), $t = 2.09$, 자유도 $931$, 단측 $p = 0.018$ (양측 $0.036$)이다. 따라서 5% 수준에서 $H_0$를 기각한다 (1% 수준에서는 기각하지 못한다). 경력이 1년 늘 때마다 교육수익률이 약 0.32%p 커지며, 예컨대 경력 10년이면 $0.0440 + 0.0032 \times 10 = 7.6\%$이다.

```{r}
m4 <- lm(log(wage) ~ educ * exper, data = wage2)
round(summary(m4)$coefficients, 5)

t4 <- summary(m4)$coefficients["educ:exper", "t value"]
c(t = t4, df = df.residual(m4),
  p_one_sided = pt(t4, df.residual(m4), lower.tail = FALSE))
```

------------------------------------------------------------------------

\newpage

# Uncertainty and Properties of the OLS Estimator

Use conventional homoskedastic standard errors in Questions 8--9.

## Question 5

Let

$$\mathbb{E}[\hat{\theta}_n] = \theta,\qquad\operatorname{Var}(\hat{\theta}_n) = \frac{4}{n},\qquad\tilde{\theta}_n = 0.8\hat{\theta}_n.$$

1. Derive each estimator's bias, variance, and MSE for estimating $\theta$.
2. For $\theta=1$, compare their MSEs at $n=25$ and $n=400$. Which estimator is preferable in each case?
3. Is each estimator consistent for $\theta$? Does your answer depend on $\theta$?

------------------------------------------------------------------------

[PUT YOUR ANSWER HERE]
**1.** $\hat\theta_n$은 편의가 없다. $\tilde\theta_n$은 $\hat\theta_n$을 0.8배로 줄인 것이어서, 편의가 생기는 대신 분산이 작아진다. $\text{MSE} = \text{Var} + \text{Bias}^2$ (분산 + 편의의 제곱)을 이용하면 다음과 같다.

| | 편의 | 분산 | MSE |
|:--|:-:|:-:|:-:|
| $\hat\theta_n$ | $0$ | $4/n$ | $4/n$ |
| $\tilde\theta_n$ | $0.8\theta - \theta = -0.2\theta$ | $0.8^2 \cdot 4/n = 2.56/n$ | $2.56/n + 0.04\theta^2$ |

**2.** $\theta = 1$일 때:

| $n$ | MSE($\hat\theta_n$) | MSE($\tilde\theta_n$) | 더 나은 추정량 |
|:-:|:-:|:-:|:-:|
| 25 | $0.16$ | $0.1024 + 0.04 = 0.1424$ | $\tilde\theta_n$ |
| 400 | $0.01$ | $0.0064 + 0.04 = 0.0464$ | $\hat\theta_n$ |

표본이 작으면 분산이 크므로, 분산을 줄여 얻는 이득이 편의로 잃는 손해보다 크다. 표본이 크면 분산은 거의 0이 되지만 편의는 줄지 않으므로 불편추정량이 낫다. 두 MSE는 $1.44/n = 0.04\theta^2$, 즉 $n = 36/\theta^2$ ($\theta = 1$이면 $n = 36$)에서 같아진다.

**3.** $\hat\theta_n$은 $\theta$ 값과 관계없이 일치추정량이다. MSE $4/n$이 0으로 가므로, 체비셰프 부등식에 의해 $\hat\theta_n \xrightarrow{p} \theta$이다. 반면 $\tilde\theta_n$은 $n$이 커져도 $\theta$가 아니라 $0.8\theta$로 수렴하고 MSE도 $0.04\theta^2$에 머문다. 따라서 $\tilde\theta_n$은 $\theta = 0$일 때만 일치추정량이며, 답이 $\theta$에 따라 달라지는 것은 $\tilde\theta_n$뿐이다.

------------------------------------------------------------------------

## Question 6

Consider

$$Y_i = \beta_0 + \beta_1 X_i + u_i.$$

Write $\mathbf{X}=(X_1,\ldots,X_n)$ and $S_{XX}=\sum_i(X_i-\bar{X})^2>0$.

1. Under SLR.1--SLR.4, derive $\hat{\beta}_1-\beta_1=\sum_i(X_i-\bar{X})u_i/S_{XX}$ and show that $\mathbb{E}[\hat{\beta}_1\mid\mathbf{X}]=\beta_1$.
2. Add SLR.5 and derive $\operatorname{Var}(\hat{\beta}_1\mid\mathbf{X})=\sigma^2/S_{XX}$. Which result survives if homoskedasticity fails?

------------------------------------------------------------------------

[PUT YOUR ANSWER HERE]
**1.** 핵심은 $\hat\beta_1$을 "참값 + 오차의 가중합"으로 쓰는 것이다. $\sum_i (X_i - \bar X) = 0$이므로 OLS 기울기는 $\hat\beta_1 = \sum_i (X_i - \bar X) Y_i / S_{XX}$이다. SLR.3에 의해 $S_{XX} > 0$이므로 이 나눗셈은 가능하다. 여기에 $Y_i = \beta_0 + \beta_1 X_i + u_i$ (SLR.1)를 대입하고 $\sum_i (X_i - \bar X) = 0$, $\sum_i (X_i - \bar X) X_i = S_{XX}$를 이용하면

$$\hat\beta_1 = \frac{\beta_0 \sum_i (X_i - \bar X) + \beta_1 \sum_i (X_i - \bar X) X_i + \sum_i (X_i - \bar X) u_i}{S_{XX}}= \beta_1 + \frac{\sum_i (X_i - \bar X) u_i}{S_{XX}} .$$

$\mathbf{X}$를 고정하면 가중치 $(X_i - \bar X)/S_{XX}$는 상수이다. 또 확률표본(SLR.2)과 조건부 평균 0 (SLR.4)에 의해 $\mathbb{E}[u_i \mid \mathbf{X}] = \mathbb{E}[u_i \mid X_i] = 0$이므로, 기댓값을 취하면 오차항이 사라진다.

$$\mathbb{E}[\hat\beta_1 \mid \mathbf{X}] = \beta_1 + \frac{\sum_i (X_i - \bar X)\, \mathbb{E}[u_i \mid \mathbf{X}]}{S_{XX}} = \beta_1 .$$

**2.** 1번 식에서 $\beta_1$은 상수이므로, 분산은 오차의 가중합에서만 나온다. 확률표본이므로 관측치 간 오차는 서로 상관이 없고, SLR.5에 의해 각 오차의 분산은 $\sigma^2$로 같다. 따라서

$$\operatorname{Var}(\hat\beta_1 \mid \mathbf{X}) = \frac{1}{S_{XX}^2} \sum_i (X_i - \bar X)^2 \operatorname{Var}(u_i \mid \mathbf{X}) = \frac{\sigma^2 S_{XX}}{S_{XX}^2} = \frac{\sigma^2}{S_{XX}} .$$

동분산성이 깨져도 1번의 불편성은 그대로이다. 1번에서는 SLR.5를 쓰지 않았기 때문이다. 깨지는 것은 분산 공식이다. 실제 분산은 $\sum_i (X_i - \bar X)^2 \sigma_i^2 / S_{XX}^2$이 되므로, 통상적 표준오차와 이를 쓰는 $t$, $F$ 검정은 틀리게 되고 OLS는 더 이상 BLUE가 아니다.

------------------------------------------------------------------------

## Question 7

State whether each claim is true or false. Justify your answers and correct false claims.

1. OLS requires normally distributed errors to be unbiased.
2. Mean-zero residuals orthogonal to the regressors establish the zero conditional mean assumption.
3. The Gauss--Markov theorem gives OLS the smallest MSE among all estimators, including biased estimators.

------------------------------------------------------------------------

[PUT YOUR ANSWER HERE]
**1. 거짓.** 불편성에는 MLR.1--MLR.4 (선형성, 확률표본, 완전한 다중공선성 없음, 조건부 평균 0)만 있으면 된다. 정규성은 불편성이 아니라, $t$·$F$ 통계량이 소표본에서도 *정확히* $t$·$F$ 분포를 따르게 하는 데 필요하다.

**2. 거짓.** 잔차의 평균이 0이고 설명변수와 직교하는 것 ($\sum_i \hat u_i = 0$, $\sum_i X_i \hat u_i = 0$)은 OLS가 원래 그렇게 계산되기 때문이다 (1계 조건). 절편이 있으면 가정이 맞든 틀리든 어떤 표본에서나 항상 성립한다. 반면 조건부 평균 0은 관측할 수 없는 *오차* $u$에 대한 가정이므로 잔차로는 확인할 수 없고, 연구설계로 정당화해야 한다.

**3. 거짓.** 가우스-마코프 정리는 MLR.1--MLR.5하에서 OLS가 *선형이면서 불편인* 추정량 가운데 분산이 가장 작다는 것(BLUE)이다. 편의추정량까지 포함하면 OLS보다 MSE가 작은 추정량이 있을 수 있다. 능형(ridge) 같은 축소추정량, 그리고 Question 5의 $n = 25$에서 $\tilde\theta_n$이 그 예이다.

------------------------------------------------------------------------

## Question 8

For Questions 8--9, use one `wage1` sample with complete `wage`, `educ`, `exper`, and `tenure`, and positive `wage`.

Estimate

$$\log(wage_i) = \beta_0 + \beta_1 educ_i + u_i.$$

1. Report $n$ and interpret $\hat{\beta}_1$. Calculate $\hat{\sigma}^2$ and the education coefficient's standard error from the residuals and regressor values; check against `summary()`.
2. Calculate a 95% $t$ confidence interval and two-sided $t$ tests of $\beta_1=0$ and $\beta_1=0.08$ at the 5% level. Report the test statistics, degrees of freedom, and $p$-values.
3. State when these $t$ tests are exact and when they are only approximately valid.

------------------------------------------------------------------------

[PUT YOUR ANSWER HERE]
**1.** $n = 526$이다. $\hat\beta_1 = 0.0827$이므로 교육이 1년 많으면 시간당 임금이 약 8.3% 높다. 잔차와 `educ` 값으로 직접 계산하면

$$\hat\sigma^2 = \frac{\sum_i \hat u_i^2}{n - 2} = \frac{120.77}{524} = 0.2305,\qquad\operatorname{se}(\hat\beta_1) = \sqrt{\frac{\hat\sigma^2}{S_{XX}}} = \sqrt{\frac{0.2305}{4025.43}} = 0.00757$$

이고, 둘 다 `summary()`의 값과 같다.

**2.** 자유도 524의 임계값 $t_{0.975,\,524} = 1.9645$를 쓰면 95% 신뢰구간은 $0.0827 \pm 1.9645 \times 0.00757 = [0.0679,\ 0.0976]$이다.

| $H_0$ | $t$ | 자유도 | $p$값 | 5% 수준 판정 |
|:-:|:-:|:-:|:-:|:-:|
| $\beta_1 = 0$ | $10.94$ | $524$ | $3.3 \times 10^{-25}$ | 기각 |
| $\beta_1 = 0.08$ | $0.363$ | $524$ | $0.717$ | 기각 실패 |

신뢰구간이 $0$은 포함하지 않고 $0.08$은 포함하므로, 두 검정의 결론과 일치한다.

**3.**

* **정확한 경우:** MLR.1--MLR.5에 더해 오차가 정규분포를 따르면 ($u \mid X \sim N(0, \sigma^2)$), 표본 크기와 관계없이 $t$ 통계량은 정확히 $t_{n-2}$ 분포를 따른다.
* **근사적인 경우:** 정규성 없이 MLR.1--MLR.5만 성립하면, 중심극한정리에 의해 표본이 클 때만 $t$ 통계량이 대략 표준정규분포를 따른다. $n = 526$이면 근사가 충분히 좋다.
* 동분산성(MLR.5)이 깨지면 통상적 표준오차를 쓴 검정은 표본이 커도 타당하지 않다.

```{r}
d <- subset(wage1, complete.cases(wage, educ, exper, tenure) & wage > 0)
n <- nrow(d)
m8 <- lm(log(wage) ~ educ, data = d)

## Part 1: by hand
sigma2_hat <- sum(resid(m8)^2) / (n - 2)
sxx <- sum((d$educ - mean(d$educ))^2)
se_b1 <- sqrt(sigma2_hat / sxx)
c(n = n, sigma2_hat = sigma2_hat, se_b1 = se_b1)

## check against summary()
c(sigma2_hat = summary(m8)$sigma^2,
  se_b1 = summary(m8)$coefficients["educ", "Std. Error"])

## Part 2: confidence interval and t tests
b1 <- unname(coef(m8)["educ"])
df8 <- n - 2
b1 + c(-1, 1) * qt(0.975, df8) * se_b1

t_0 <- b1 / se_b1
t_08 <- (b1 - 0.08) / se_b1
data.frame(H0 = c("beta1 = 0", "beta1 = 0.08"),
           t = c(t_0, t_08), df = df8,
           p = 2 * pt(abs(c(t_0, t_08)), df8, lower.tail = FALSE))
```

------------------------------------------------------------------------

## Question 9

Using the same sample, estimate

$$\log(wage_i) = \beta_0 + \beta_1 educ_i + \beta_2 exper_i + \beta_3 tenure_i + u_i.$$

1. Reconstruct the education coefficient's standard error using $\hat{\sigma}^2$, education's total sum of squares, and the $R^2$ from regressing `educ` on an intercept, `exper`, and `tenure`. Compare it with Question 8. Must adding controls improve precision? Explain.
2. Test $H_0:\beta_2=\beta_3=0$ against the alternative that at least one is nonzero. Calculate $F$, its degrees of freedom, and its $p$-value from the restricted and unrestricted residual sums of squares; check with `anova()`. How does this null differ from the overall $F$ test in `summary()`?

------------------------------------------------------------------------

[PUT YOUR ANSWER HERE]
**1.** $\hat\sigma^2 = 101.46/522 = 0.1944$, $SST_{educ} = 4025.43$, $R^2_{educ} = 0.1013$이므로

$$\operatorname{se}(\hat\beta_1) = \sqrt{\frac{\hat\sigma^2}{SST_{educ}\,(1 - R^2_{educ})}} = \sqrt{\frac{0.1944}{4025.43 \times 0.8987}} = 0.00733$$

이다. `summary()`의 값과 같고, Question 8의 $0.00757$보다 약간 작다.

통제변수를 넣는다고 정밀도가 꼭 좋아지지는 않는다. 위 식에서 두 힘이 반대로 작용하기 때문이다.

* **분자 $\hat\sigma^2$:** 통제변수가 임금을 잘 설명하면 잔차가 줄어 표준오차가 작아진다 (여기서 $0.2305 \to 0.1944$).
* **분모 $1 - R^2_{educ}$:** 통제변수가 `educ`와 상관되어 있으면 `educ`만의 고유한 변동이 줄어 표준오차가 커진다 (여기서 분산이 $1/(1 - 0.1013) = 1.11$배).

여기서는 첫째 효과가 더 커서 표준오차가 줄었다. 반대로 `educ`와는 상관이 높지만 임금은 거의 설명하지 못하는 변수를 넣으면 표준오차가 커진다.

**2.** 제약모형은 `exper`와 `tenure`를 뺀 Question 8의 회귀 ($SSR_r = 120.77$), 비제약모형은 이번 회귀 ($SSR_{ur} = 101.46$)이다. 제약의 수는 $q = 2$, 비제약모형의 자유도는 $n - k - 1 = 522$이다.

$$
F = \frac{(SSR_r - SSR_{ur})/q}{SSR_{ur}/(n - k - 1)} = \frac{(120.77 - 101.46)/2}{101.46/522} = 49.69
$$

자유도는 $(2, 522)$, $p = 1.8 \times 10^{-20}$이므로 $H_0$를 기각한다. 즉 `exper`와 `tenure`는 함께 보면 유의하며, `anova()`도 같은 결과를 준다.

`summary()`의 전체 $F$ 검정과는 귀무가설이 다르다.

* **이 검정:** $\beta_2 = \beta_3 = 0$. `educ`는 남겨 두고, `exper`와 `tenure`를 더하면 설명력이 나아지는지를 묻는다.
* **전체 $F$ 검정:** $\beta_1 = \beta_2 = \beta_3 = 0$. 세 변수가 모두 쓸모없는지, 즉 절편만 있는 모형과 비교한다 ($F = 80.39$, 자유도 $(3, 522)$).

```{r}
m9 <- lm(log(wage) ~ educ + exper + tenure, data = d)

## Part 1: standard error from its components
sigma2_9 <- sum(resid(m9)^2) / (n - 4)
sst_educ <- sum((d$educ - mean(d$educ))^2)
r2_educ <- summary(lm(educ ~ exper + tenure, data = d))$r.squared
se_b1_9 <- sqrt(sigma2_9 / (sst_educ * (1 - r2_educ)))

c(sigma2_hat = sigma2_9, SST_educ = sst_educ, R2_educ = r2_educ,
  se_by_hand = se_b1_9,
  se_summary = summary(m9)$coefficients["educ", "Std. Error"],
  se_Q8 = se_b1)

## Part 2: F test from residual sums of squares
ssr_r <- sum(resid(m8)^2)
ssr_ur <- sum(resid(m9)^2)
q <- 2
df_ur <- n - 4
F_stat <- ((ssr_r - ssr_ur) / q) / (ssr_ur / df_ur)
c(SSR_r = ssr_r, SSR_ur = ssr_ur, F = F_stat, df1 = q, df2 = df_ur,
  p = pf(F_stat, q, df_ur, lower.tail = FALSE))

anova(m8, m9)          ## check
summary(m9)$fstatistic ## overall F test, for comparison
```

------------------------------------------------------------------------

\newpage

# Peer Review Exercise

We're going to read an unpublished working paper and do peer-review on the paper. You have two choices according to your interest: 

* Sanghoon Kim-Leffingwell and Sunkyoung Park (2026), "Revisiting the Past with a Partisan Lens: An Experimental Study of Partisanship and Authoritarian Nostalgia." 
* Hwalmin Jin (2026), "Coup-Proofing in the Shadow of External Security Threats."

The papers will be distributed separately. See [LINK](https://www.cambridge.org/core/services/aop-file-manager/file/5a1eb62e67f405260662a0df/Refreshed-Guide-Peer-Review-Journal.pdf) for basics on peer review.

Provide structured feedback; divide comments into major and minor comments. Be civil. The main question is: is the causal inference valid and the conclusion plausible? You can also freely comment on minor issues like typos and readability of the text.

--------------------------------------------------------------------------------

\newpage

# Digesting Cutting Edge Methods Papers into Key Takeaways (2nd Exercise)

See [https://github.com/sysilviakim/quant-social-science-skills/tree/main/contrib](https://github.com/sysilviakim/quant-social-science-skills/tree/main/contrib). We are going to build an open-source reviewer agent focusing on quantitative methods. 

Remember that the TA has randomly assigned two papers to you. See `EXAMPLE-atsusaka_kim_2025.md` in the GitHub repository; your task is to carefully read the papers and create something equivalent for your assigned papers.

For PS2, choose the remaining one of the two assigned papers. Same as before: submit the following two items:

1. Show me that you actually did the reading. Print out the paper, take highlights and memos, and submit the outcome in person (I don't care if the memos and highlights are messy).
2. Submit a `.md` file that summarizes the paper's main points and explains what the reviewer should check for methodological integrity.
