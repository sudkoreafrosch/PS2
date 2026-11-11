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

```{r}
## [PUT YOUR CODE HERE]
```

------------------------------------------------------------------------

## Question 2

We will now implement an interaction model, again with the `wage1` dataset.

1. You suspect that the predictors used in Question 1 may have differential effects by gender. Set up a model with interaction effects and write the equation. Justify the model. (There's no single answer to this. Write a new model that makes sense to you!)

2. Estimate the model and interpret the results, especially the coefficient for the interaction term(s).

------------------------------------------------------------------------

[PUT YOUR ANSWER HERE]

```{r}
## [PUT YOUR CODE HERE]
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

```{r}
## [PUT YOUR CODE HERE]
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

```{r}
## [PUT YOUR CODE HERE]
```

------------------------------------------------------------------------

\newpage

# Uncertainty and Properties of the OLS Estimator

Use conventional homoskedastic standard errors in Questions 8--9.

## Question 5

Let

$$
\mathbb{E}[\hat{\theta}_n] = \theta,
\qquad
\operatorname{Var}(\hat{\theta}_n) = \frac{4}{n},
\qquad
\tilde{\theta}_n = 0.8\hat{\theta}_n.
$$

1. Derive each estimator's bias, variance, and MSE for estimating $\theta$.
2. For $\theta=1$, compare their MSEs at $n=25$ and $n=400$. Which estimator is preferable in each case?
3. Is each estimator consistent for $\theta$? Does your answer depend on $\theta$?

------------------------------------------------------------------------

[PUT YOUR ANSWER HERE]

------------------------------------------------------------------------

## Question 6

Consider

$$
Y_i = \beta_0 + \beta_1 X_i + u_i.
$$

Write $\mathbf{X}=(X_1,\ldots,X_n)$ and $S_{XX}=\sum_i(X_i-\bar{X})^2>0$.

1. Under SLR.1--SLR.4, derive $\hat{\beta}_1-\beta_1=\sum_i(X_i-\bar{X})u_i/S_{XX}$ and show that $\mathbb{E}[\hat{\beta}_1\mid\mathbf{X}]=\beta_1$.
2. Add SLR.5 and derive $\operatorname{Var}(\hat{\beta}_1\mid\mathbf{X})=\sigma^2/S_{XX}$. Which result survives if homoskedasticity fails?

------------------------------------------------------------------------

[PUT YOUR ANSWER HERE]

------------------------------------------------------------------------

## Question 7

State whether each claim is true or false. Justify your answers and correct false claims.

1. OLS requires normally distributed errors to be unbiased.
2. Mean-zero residuals orthogonal to the regressors establish the zero conditional mean assumption.
3. The Gauss--Markov theorem gives OLS the smallest MSE among all estimators, including biased estimators.

------------------------------------------------------------------------

[PUT YOUR ANSWER HERE]

------------------------------------------------------------------------

## Question 8

For Questions 8--9, use one `wage1` sample with complete `wage`, `educ`, `exper`, and `tenure`, and positive `wage`.

Estimate

$$
\log(wage_i) = \beta_0 + \beta_1 educ_i + u_i.
$$

1. Report $n$ and interpret $\hat{\beta}_1$. Calculate $\hat{\sigma}^2$ and the education coefficient's standard error from the residuals and regressor values; check against `summary()`.
2. Calculate a 95% $t$ confidence interval and two-sided $t$ tests of $\beta_1=0$ and $\beta_1=0.08$ at the 5% level. Report the test statistics, degrees of freedom, and $p$-values.
3. State when these $t$ tests are exact and when they are only approximately valid.

------------------------------------------------------------------------

[PUT YOUR ANSWER HERE]

```{r}
## [PUT YOUR CODE HERE]
```

------------------------------------------------------------------------

## Question 9

Using the same sample, estimate

$$
\log(wage_i) = \beta_0 + \beta_1 educ_i + \beta_2 exper_i + \beta_3 tenure_i + u_i.
$$

1. Reconstruct the education coefficient's standard error using $\hat{\sigma}^2$, education's total sum of squares, and the $R^2$ from regressing `educ` on an intercept, `exper`, and `tenure`. Compare it with Question 8. Must adding controls improve precision? Explain.
2. Test $H_0:\beta_2=\beta_3=0$ against the alternative that at least one is nonzero. Calculate $F$, its degrees of freedom, and its $p$-value from the restricted and unrestricted residual sums of squares; check with `anova()`. How does this null differ from the overall $F$ test in `summary()`?

------------------------------------------------------------------------

[PUT YOUR ANSWER HERE]

```{r}
## [PUT YOUR CODE HERE]
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
