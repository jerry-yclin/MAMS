# Part 2. Choosing design parameters and stopping boundaries

In Part 1 we argued why multi‑arm multi‑stage (MAMS) / platform trials are more efficient and ethical than a sequence of independent RCTs, and saw how this plays out in STAMPEDE, I‑SPY 2 and REMAP‑CAP. 

In this part we will investigate:

1. How to describe a MAMS design mathematically.
2. How the joint distribution of test statistics looks across arms and stages.
3. How to choose:

   * number of arms $K$ and stages $J$,
   * information times $t_j$,
   * allocation ratio between control and treatment arms.
4. How to construct **efficacy** and **futility** boundaries with family‑wise error‑rate (FWER) control in the multi‑arm, multi‑stage setting.
5. What “optimal” and “near‑optimal” MAMS designs (e.g., Wason–Jaki) try to optimise, and how software such as `MAMS` and `gsMAMS` implements these ideas. 

We’ll mostly use a continuous endpoint for clarity, but everything can be extended to accommodate binary and time‑to‑event outcomes.

## 2.1 Basic set‑up and notation

We consider a trial with:

* One **control** arm, indexed $k = 0$.
* $K$ **treatment** arms, indexed $k = 1,\dots,K$.
* $J$ **analyses** (stages), indexed $j = 1,\dots,J$.

Analyses happen at increasing **information times** $0 < t_1 < t_2 < \dots < t_J = 1$,
where $t_j$ is the fraction of the final information accumulated at analysis $j$. 

### 2.1.1 Model and effect sizes

Suppose each patient on arm $k$ has a continuous outcome $Y_{ki} \sim \mathsf{N}(\mu_k,\sigma^2), \quad i=1,2,\dots$ with common variance $\sigma^2$.

We are interested in the **standardized treatment effect** $\theta_k = \frac{\mu_k - \mu_0}{\sigma}, \quad k=1,\dots,K$.

For each treatment arm we test the one‑sided hypothesis
$H_{0k}: \theta_k = 0
\quad\text{vs}\quad
H_{Ak}: \theta_k > 0,\qquad k=1,\dots,K$.


### 2.1.2 Cumulative sample sizes and information

By analysis $j$:

* $N_{0j}$: total number of control patients accrued so far.
* $N_{kj}$: total number of patients accrued to treatment arm $k$.

Let $\bar{Y}\_{0j}$ and $\bar{Y}\_{kj}$ be the sample means (control arm and treatment $k$) at analysis $j$. A natural estimator of the standardized effect is

$$\widehat{\theta}\_{kj} = \frac{\bar{Y}\_{kj} - \bar{Y}\_{0j}}{\sigma}$$

with variance

$$\text{Var}(\widehat{\theta}\_{kj}) = \frac{1}{N_{kj}} + \frac{1}{N_{0j}}$$

The **(Fisher) information** for treatment $k$ vs. control at analysis $j$ is

$$
I_{kj} = \frac{1}{\text{Var}(\widehat{\theta}\_{kj})}
= \frac{1}{\tfrac{1}{N\_{kj}} + \tfrac{1}{N_{0j}}}
= \frac{N_{kj} N_{0j}}{N_{kj} + N_{0j}}.
$$

Define a **Z‑statistic**

$$Z_{kj} = \sqrt{I_{kj}}\widehat{\theta}_{kj}$$

Then approximately

* under $H_{0k}$: $Z_{kj} \sim \mathsf{N}(0,1)$,
* under $H_{Ak}$ with true effect $\theta_k: Z_{kj} \sim \mathsf{N}(\theta_k \sqrt{I_{kj}}, 1)$.

If we enforce the same allocation ratio for all treatment arms at each stage/analysis, then the information is the same for each arm:

$$I_{1j} = I_{2j} = \dots = I_{Kj} = I_j$$

We then define the **information fraction**: 

$$t_j = \frac{I_j}{I_J},\qquad j=1,\dots,J,\quad t_J = 1$$

The pair $(I_J, t_1,\dots,t_{J-1})$ is often easier to specify than calendar times and sample sizes: the calendar timeline is derived after you know how many patients and stages you need.

## 2.2 Correlation structure across arms and stages

The key technical ingredient in MAMS design is the **joint correlation structure** of all the $Z$‑statistics.

Let

$$ \mathbf{Z} = (Z_{11},\dots,Z_{K1}, Z_{12},\dots,Z_{K2}, \dots, Z_{1J},\dots,Z_{KJ})^\top $$

Under the true effects $\theta = (\theta_1,\dots,\theta_K)$, we have

$$ \mathbf{Z} \sim N_{KJ}(\boldsymbol{\mu}(\theta), \Sigma) $$

for some mean vector $\boldsymbol{\mu}(\theta)$ and covariance matrix $\Sigma$.

The mean vector is simple:

$$\mu_{kj}(\theta) = \theta_k \sqrt{I_j}$$

The interesting bit is $\Sigma$, which captures:

1. **Correlation across stages** for the *same arm* (repeated looks at the same data).
2. **Correlation across arms** at the *same stage* (shared control group).

### 2.2.1 Same arm, different stages

Fix $k$. The sequence $(Z_{k1},\dots,Z_{kJ})$ has a multivariate normal distribution with

$$\text{Corr}(Z_{kj}, Z_{k\ell}) = \sqrt{\frac{I_j}{I_\ell}} = \sqrt{\frac{t_j}{t_\ell}}, \qquad j \le \ell $$

Intuition:

* You can think of an underlying **Brownian motion** $B_k(t)$ with variance parameter $t$.
* At analysis $j$, the statistic can be expressed (under $H_{0k}$) as

$$Z_{kj} = \frac{B_k(t_j)}{\sqrt{t_j}}$$

  so $\text{Corr}(Z_{kj}, Z_{k\ell}) = \sqrt{t_j/t_\ell}$ when $j \le \ell$.


### 2.2.2 Different arms, same stage

Now fix an analysis $j$ and consider two arms $k \neq k'$.

Assume at stage $j$:

* $N_{kj} = N_{k'j} = n_j$ on each treatment arm.
* $N_{0j} = n_{0j}$ on the control/placebo arm.
* All arms share the same phenotypic variance $\sigma^2$.

We have

$$\widehat{\theta}_{kj} = \frac{\bar{Y}_{kj} - \bar{Y}_{0j}}{\sigma}, \widehat{\theta}_{k'j} = \frac{\bar{Y}_{k'j} - \bar{Y}_{0j}}{\sigma}$$

The covariance between these two estimators arises entirely from the shared control mean $\bar{Y}_{0j}$:

$$\text{Cov}(\widehat{\theta}_{kj}, \widehat{\theta}_{k'j}) = \frac{1}{\sigma^2} \text{Var}(\bar{Y}_{0j}) = \frac{1}{\sigma^2} \cdot \frac{\sigma^2}{n_{0j}} = \frac{1}{n_{0j}}$$

Moreover, the variance of each term is

$$\text{Var}(\widehat{\theta}_{kj}) = \frac{1}{n_j} + \frac{1}{n_{0j}}$$

and similarly for $k'$. So the correlation is

$$\text{Corr}(\widehat{\theta}_{kj}, \widehat{\theta}_{k'j}) = \frac{1/n_{0j}}{1/n_j + 1/n_{0j}} = \frac{n_j}{n_j + n_{0j}}$$

The $Z$‑statistics are just scaled versions of these, so the correlation between $Z_{kj}$ and $Z_{k'j}$ is the same:

$$\text{Corr}(Z_{kj}, Z_{k'j}) = \frac{n_j}{n_j + n_{0j}}$$

It is often helpful to express this in terms of a **randomization ratio**.

Suppose at stage $j$ we randomize using the ratio

$$ \text{control} : \text{each arm} = r_0 : r$$

Then $n_{0j} : n_j = r_0 : r$ and

$$\text{Corr}(Z_{kj}, Z_{k'j})= \frac{r}{r_0 + r}, \qquad k \ne k'$$

Examples:

* 1:1:1:1 allocation $(r_0 = r)$ ⇒ correlation $= \tfrac12$.
* 2:1 in favour of control $(r_0 = 2r)$ ⇒ correlation $= \tfrac13$.

The more **heavily** we allocate to control, the *less* correlated the arms are at each look, because the control mean is more precisely estimated.

