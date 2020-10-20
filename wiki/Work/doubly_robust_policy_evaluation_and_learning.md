# Doubly Robust Policy Evaluation and Learning
By Miroslav Dudik, John Langford, and Lihong Li
## Abstract
- Contextual bandits: reward is partially observed and function of action + context
- past approaches: models of rewards (large bias) or models of past policy (large variance), need to make sure I understand what these approaches are (seems like IPS and DM)
- Doubly robust: method that accurately estimates value when either a good model of rewards or model (neither needs to be consistent?)
## Intro
- Two approaches to offline learning in contextual bandits:
    - 1. Direct Method: 'directly' estimates the reward function from data and uses in place of actual reward to evaluate target policy on set of contexts
    - 2. Inverse Propsensity Score (IPS): importance weighting to adjust observed data ratios to correct for mismatch between behavior and target policy
- DM requires accurate reward model (very hard, bias), IPS requires accurate model of past policy to generate inverse propensity scores (even with accurate behavior policy estimate if dissimilar to target policy high variance)
- History of doubly robust estimation: combines DM and IPS such that if either correct, estimate unbiased
- Example of a survey: ask questions about age, sex, etc. Can either use this info to inverse weight responses to control for selection bias, or learn to directly predict survey responses
## Problem Definition and Approach
- Overview of notation, policy evaluation and learning
- An issue with DM: since estimate of reward function $\rho$ is made independent of target policy $\pi$, can 'waste' capacity modeling irrelevant aspects
- DM Value approx: $V_{DM} = \frac{1}{|S|} \sum_{x \in S} \hat{\rho}_{\pi(x)}(x)$   
- IPS Value approx: $V_{IPS} = \frac{1}{|S|} \sum_{(x,a,r_a) \in S} \frac{1(\pi(x)=a)}{\hat{\mu}(x)} r_a$ 
- Doubly Robust (DR) Value aprox: $V_{DR} = \frac{1}{|S|} \sum_{(x,a,r_a) \in S} \frac{1(\pi(x)=a)}{\hat{\mu}(x)} (r_a-\hat{\rho}_a(x)) + \hat{\rho}_{\pi(x)}(x)$ 
- One way to think about this: uses IPS when $\pi(x)=a$, else uses DM 
## Bias Analysis
- Have additive deviation of $\rho$: $\Delta$ and multiplicative deviation of $\mu$: $\delta$
- The expected value of this estimator is $V + E_{x}[\Delta \delta]$ 
## Variance Analysis
- derives 3-term eqn from squared expectation, compared with IPS final term can be less depending on multiplicative deviation (compared with action-state conditioned expected reward)
## Experiments
- multiclass classification w/ bandit feedback: classes are actions, make cost of classification action 1 for all classes but true class
- assume uniform behavior policy
- for target policy: use direct loss minimization alg (some sort of gradient based alg)
- to evaluate, estimate performance of policy on test data sampled in same way as train (ie: uniform), do this 500 times to get good estimate of possible 
- Needs a DM component (which amounts to classification), uses linear model with least squares ridge
- bias is low for IPS and DR, high for DM
- variance is somewhat lower for DR than IPS, often DM variance is higher (odd)
- also looks at training policy using value estimate, sees DR is slightly better than IPS, similar to offset tree (whatever that is)
- less convincing results with study on user ads, but not bad at very low data

20200930163446
