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

The pair $(I_J, t_1,\dots,t_{J-1})$ is often easier to specify than calendar times and sample sizes: the calendar timeline is derived after you know how many patients and events you need.

