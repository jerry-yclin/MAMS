# Part 1. Understanding the motivation for MAMS and platform designs

In this part we’ll set the stage:

1. Why a series of conventional two‑arm RCTs is statistically and operationally sub‑optimal.  
2. How **multi‑arm multi‑stage (MAMS)** trials fix some of those issues.  
3. How MAMS ideas are embedded in **platform trials** like **STAMPEDE**, **I‑SPY 2**, and **REMAP‑CAP**.

## 1.1 From “one‑question‑at‑a‑time” RCTs to multiple treatment arms

### 1.1.1 The classical development pathway

Traditional clinical development is built around a sequence of stand‑alone two‑arm RCTs:

- Trial A: Treatment A vs. control  
- Trial B: Treatment B vs. control  
- Trial C: Treatment C vs. control  
- … and so on.

Each trial:

- Has its own protocols. 
- Randomises patients 1:1 to treatment vs. control.  
- Tests a single primary hypothesis (Is the treatment effect statistically significant?).

This is simple and familiar, but deeply inefficient once you think of running multiple treatment arms concurrently, rather than one question at a time.

### 1.1.2 Where the classical approach wastes information

Suppose you have **K** candidate treatments you could plausibly compare with the same standard of care.

In the classical approach:

- You run **K separate 2‑arm trials**, each with:
  - Total sample size $2n$ ($n$ per arm) to achieve your chosen power for A vs. control.  
- $2nK$ patients in total.

The control arms in each trial are conceptually “the same treatment”, but you:

- Don’t share data across trials (even if eligibility and endpoints are similar).  
- Have to explicitly control for the multiple hypothesis testing problem: the chance of at least one false positive across all K trials.

At the same time:

- You pay **setup costs K times** (contracts, training, approvals).  
- You duplicate data management and trial governance structures.  
- You wait, sometimes years, between trials.  


## 1.2 Why multi‑arm trials are intrinsically more efficient

### 1.2.1 Sharing a common control

A multi‑arm trial compares several experimental arms against a **single, concurrently recruited control**.

Consider:

- Control arm: 0  
- Experimental arms: 1, …, K.  
- Equal randomisation across all arms.

If each arm (including control) has sample size \(n\), then:

- Total sample size in the multi‑arm trial: $(K + 1) n$.  
- For each comparison $k$ vs. control, the variance of the mean difference is $\sigma^2 (1/n + 1/n) = 2\sigma^2 / n$, **exactly the same** as in a two‑arm trial with n per arm.

So to give each treatment arm the same (per-treatment) power as a 2‑arm trial, we need:

- Separate RCTs: total size $2nK$.  
- One multi‑arm trial: total size $(K+1)n$.

For K = 3, that’s a **33% saving in total sample size** even before you add any interim stopping. For K = 5, you’re at ~60% of the sample size of five separate trials.

### 1.2.2 Adding stages: dropping losers early

A **multi‑arm multi‑stage (MAMS)** design adds one more layer: **interim analyses** at which you can:

- Drop arms for **lack‑of‑benefit (futility)**, often based on an intermediate endpoint.  
- Stop arms (or the whole trial) early for **efficacy**.

Even if total **maximum** sample size is similar to a fixed‑sample multi‑arm trial, the **expected** sample size can be much smaller because many arms stop early when they look ineffective.

For example, Wason & Jaki show that:

- In scenarios where several arms truly have no effect, MAMS designs can reduce expected sample size by 30–60% relative to separate fixed‑sample trials, **while preserving strong FWER control**.

To sum up:

- You’re learning **faster** which arms are worth carrying forward.  
- You spend **fewer patients** on arms that will eventually be shown to be ineffective.

## 1.3 Ethical arguments: fewer patients on inferior treatments

The statistical efficiency arguments translate directly into ethics.

### 1.3.1 Fewer patients on inactive or harmful interventions

Two key mechanisms:

1. **Common control**:  
   - In K separate RCTs, you can’t re‑use control patients.  
   - In a multi‑arm trial, each control patient contributes information to **all** treatment comparisons.  
   - So for the same precision in each comparison, you need fewer control patients overall — i.e. fewer patients randomised away from what might turn out to be the best available therapy.

2. **Early stopping for futility**:  
   - Unpromising treatment arms are terminated early, rather than continuing to randomise patients until a fixed final sample.  
   - This is particularly attractive when the intervention is burdensome or toxic.

STAMPEDE’s implementation of MAMS, for example, allowed several treatment strategies to be dropped when intermediate outcomes showed lack of benefit, limiting patient exposure to ineffective combinations.

### 1.3.2 Faster answers and more relevant control arms

Because MAMS trials:

- Evaluate several arms in parallel,  
- Use intermediate endpoints to make earlier decisions, and  
- Can be embedded within platforms that **update control** when a superior treatment is found,

patients entering later periods of the trial:

- **Benefit sooner** from new standards of care established within the same platform.  
- Are less likely to be randomised to outdated control arms.


## 1.4 From MAMS trials to platform trials

So far we’ve talked about a *fixed* MAMS trial: a finite set of arms and stages defined a priori. A **platform trial** takes this one step further.

### 1.4.1 What is a platform trial?

Informally, a platform trial is:

> “A trial run under a master protocol that allows multiple treatments to be evaluated simultaneously and sequentially, with arms entering and leaving over time, often using adaptive methods.”

Core features:

- **Multi‑arm**: two or more concurrent treatments, usually sharing a control.  
- **Multi‑stage / adaptive**: interim analyses with early stopping and/or response‑adaptive randomisation.  
- **Arm addition and removal**: new therapies can join; ineffective or harmful therapies leave.  
- **Possibly multifactorial**: combinations of interventions/treatments are given to trial participants.

A MAMS trial is often the **statistical engine** inside a platform:

- The rules for when to stop an arm for futility or efficacy.  
- The sample size and boundary calculations that control type I error.  
- Sometimes, the way intermediate vs. final endpoints are handled.

### 1.4.2 Why platforms, not just bigger trials?

Compared with a large one‑off MAMS trial, a platform adds:

- **Persistence**: it doesn’t “end” when one arm is successful; it continues as a standing infrastructure.  
- **Flexibility**: you can add treatment arms as science, drugs and external evidence evolve, without building a new trial from scratch each time.  
- **Consistency**: analyses across arms and over time are harmonised under one protocol and governance structure.

This is most obvious in STAMPEDE and REMAP‑CAP, which have been running for many years and have evaluated multiple interventions under the same master protocol.

## 1.5 How real platforms operate in practice

Now we zoom in on **three flagship platforms** and highlight how MAMS ideas show up in their day‑to‑day reality.

### 1.5.1 STAMPEDE: systemic therapy for advanced prostate cancer

**Context**

- Population: men with high‑risk, locally advanced or metastatic prostate cancer starting long‑term androgen deprivation therapy (ADT).  
- Control: ADT alone (with details evolving over time, e.g. whether docetaxel is allowed as part of standard of care).  
- Multiple “research arms”: combinations of ADT with additional systemic agents (e.g. docetaxel, zoledronic acid, abiraterone, enzalutamide) or radiotherapy to the primary tumour.  

**MAMS structure**

- Originally designed as a **6‑arm, 5‑stage** MAMS trial:
  - **Multi‑arm**: several experimental regimens vs ADT.  
  - **Multi‑stage**: interim “activity” analyses based on the intermediate endpoint.
- Intermediate endpoint: **failure‑free survival (FFS)** (biochemical, radiologic or clinical progression, or prostate‑cancer death).  
- Final endpoint: overall survival (OS).  

At pre‑specified FFS event counts in the control arm, they:

- Compute hazard ratios for each experimental arm vs control.  
- Compare them with **pre‑planned futility and efficacy boundaries**.  
- **Drop arms for lack‑of‑benefit** when FFS results are insufficiently promising.  
- Continue promising arms to later stages, and eventually evaluate OS.

**Platform behaviour**

- Arms such as **docetaxel + ADT** and **abiraterone + ADT** have shown clear OS benefit and changed clinical practice.  
- Several regimens (e.g. celecoxib‑only combinations) were stopped early for lack‑of‑benefit at interim analyses.  
- New arms have been **added mid‑trial**, under amendments to the master protocol, without stopping the core trial infrastructure.  

### 1.5.2 I‑SPY 2: biomarker‑driven neoadjuvant breast cancer platform

**I‑SPY 2** is a phase II adaptive platform trial for women with high‑risk, stage II–III breast cancer receiving neoadjuvant chemotherapy.

**Key features**

- **Subtypes**: patients are stratified into biomarker‑defined subtypes using:
  - Hormone receptor (HR) status;  
  - HER2 status;  
  - A 70‑gene expression signature (MammaPrint).  
- **Arms**: multiple arms (e.g. combinations with targeted agents or immunotherapies) are tested in addition to standard chemotherapy. New treatments can be added over time.  
- **Primary endpoint**: pathologic complete response (pCR) at surgery — an individual‑level surrogate for long‑term outcome.  

**Adaptive / MAMS‑like behaviour**

- **Adaptive randomisation**:
  - Within each biomarker‑defined subtype, patients are adaptively randomised to favour treatments with higher estimated probability of pCR.  
  - A Bayesian model updates treatment‑by‑subtype effects as data accrue.  

- **Graduation and dropping**:
  - Treatments “graduate” from the platform when their Bayesian predictive probability of success in a future 300‑patient phase III trial exceeds a pre‑specified threshold in at least one subtype.  
  - Treatments with very low predictive probability can be dropped for futility.  

Although I‑SPY 2 is formally Bayesian, the same ideas still apply:

- Multiple arms vs. a common control group.  
- Multiple stages (ongoing interims) where arms can graduate (efficacy) or be stopped (futility).  
- Re‑use of infrastructure to test many treatments over time, within one master protocol.

### 1.5.3 REMAP‑CAP: multifactorial platform for pneumonia

**REMAP‑CAP** (*Randomized, Embedded, Multifactorial, Adaptive Platform trial for Community‑Acquired Pneumonia*) is a global platform focused on severe CAP, including pandemic respiratory infections.

**Design philosophy**

- **Randomized**: patients admitted with severe CAP are randomised to treatment options.  
- **Embedded**: trial processes are integrated into routine clinical care (e.g. using electronic health records).  
- **Multifactorial**: treatments are grouped into **domains** (e.g. antibiotic strategies, immunomodulators, corticosteroids); patients are randomised across multiple domains simultaneously, creating factorial combinations.  
- **Adaptive platform**: the trial is intended to run indefinitely, adding new therapies and domains as needed, including during pandemics like COVID‑19.  

**Adaptation rules**

- Each domain has pre‑specified adaptation rules, often Bayesian:
  - Early stopping for superiority.
  - Early stopping for inferiority.  
  - Response‑adaptive randomisation to favour better‑performing interventions.  

From a MAMS perspective, you can think of each domain as:

- A **multi‑arm multi‑stage trial** of several treatments within that domain,  
- With embedding and factorial crossing across domains.

## 1.6 Take‑home messages

- **Statistical & operational efficiency**
  - Shared control arms → fewer patients needed for the same per‑comparison power.  
  - Interim looks → early stopping of ineffective or outstandingly effective arms.  
  - Single infrastructure → avoid repeated startup overheads.

- **Ethical advantages**
  - Fewer patients on treatment arms that will ultimately prove ineffective.  
  - Faster adoption of effective treatments as the trial proceeds.  
  - Particularly compelling when the disease context evolves quickly (e.g. pandemics).

- **Real‑world proof of concept**
  - **STAMPEDE**: classic frequentist MAMS platform in prostate cancer, with formal multi‑stage design and arm addition; has changed standard of care multiple times.  
  - **I‑SPY 2**: biomarker‑rich Bayesian platform for neoadjuvant breast cancer; exemplifies multi‑arm, multi‑stage behaviour with adaptive randomisation and graduation rules.  
  - **REMAP‑CAP**: multifactorial platform embedded in routine care; domain‑wise MAMS logic underpins its rapid learning in ICU settings and during pandemics.  


**Where we go next**

In the subsequent parts, we will:

1. Formalise the MAMS statistical framework: notation, joint distributions, and error‑rate control.  
2. Show how to choose **design parameters and stopping boundaries** (futility and efficacy).  
3. Discuss **overwhelming efficacy**, **stopping randomisation**, and **adding new arms** (pre‑planned and unplanned) with type I/II error control.  
4. Cover **ranking and selection** MAMS designs, and how they fit inside platforms.

But everything in those later parts rests on the central motivation we’ve covered here: **once you see a clinical trial program as a sequence of related questions, MAMS and platform designs become the natural, statistically coherent way to run that program.**
