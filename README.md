# Statistical & Practical Aspects of Multi‑Arm Multi‑Stage Platform Trials

This repository contains a self‑contained tutorial on the **design and analysis of Multi‑Arm Multi‑Stage (MAMS) platform trials**, with emphasis on the **practical implementation in real studies** (e.g., STAMPEDE, I‑SPY 2, REMAP‑CAP, TAILoR).

## What this tutorial covers

1. **Understand the motivation** for MAMS and platform designs  
   - Why multi‑arm, multi‑stage platforms are more efficient and ethical than a series of stand‑alone RCTs.  
   - How real platforms (STAMPEDE, I‑SPY 2, REMAP‑CAP) operate in practice.

2. **Choose design parameters and stopping boundaries**  
   - Number of treatment arms and stages, information time, and allocation ratio.  
   - Construction of efficacy and futility boundaries in a multi‑arm group‑sequential framework.  
   - Use of optimal/near‑optimal designs (e.g. Wason–Jaki) and R tools (MAMS/gsMAMS).

3. **Handle overwhelming efficacy and futility**  
   - Binding vs non‑binding futility rules.  
   - “Overwhelming efficacy” rules, alpha‑spending, and impact on type I error.

4. **Implement rules for stopping treatment arms**  
   - Stopping treatment arms for lack‑of‑benefit, harm, or success.  
   - Updating randomisation ratios as arms are dropped or succeed.  
   - ITT analysis and reporting when arms stop early.

5. **Add new treatment arms while controlling for type I & II errors**  
   - Pre‑planned arm additions with strong FWER control.  
   - Unplanned arm additions via conditional‑error methods.  
   - Impact of arm addition on power, use of concurrent controls, and the need for simulation.

6. **Use MAMS designs with ranking & selection**  
   - “Drop‑the‑losers / pick‑the‑winner” style designs.  
   - Formal selection rules (winner‑takes‑all, top‑r, threshold rules).  
   - Probability of correct selection vs confirmatory error‑rate control.



