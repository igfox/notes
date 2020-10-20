# Deep Reinforcement Learning for Visual Field Testing
Review for AAAI 2021

## Abstract
Visual field testing to determine field of vision
Algorithms exist based on MLE to try and speed up process
Evaluates deep RL on synthetic data, RL approach faster and more accurate
'comprehensive evaluation', looks at how well it generalizes
Looking over results:
    ZEST main baseline
    uniform, bimodal, exponential sensitivies (I think)
    normal, abnormal, combined patients
    results by MAE-N (n number of samples?)
    
## Introduction 
- Nice historical notes on visual field testing
- Originally SAP: 15-20 min per eye
- Want to find map of sensitivies (in decibels) at which stimulus detected 50% of time
- SAP w/ 24-2 protocol on Humphrey Field Analyzer gold standard
- Longer testing times lead to decline in reliability indices (think about this point some)
- In current algs, sensitivity of a location is pdf over decibels, at each update show intesnsity at mean level of prior, then uses results to derive posterior, iterates for set number of times or until std low enough
- Speed of convergence sensitive to prior choice

## Related Work
- Several forms of visual field testing after ZEST: VAA, GOANNA, SWeLZ
- Surprising amount of work in deep RL for opthamology
- Deep RL used for landmark detection in incomplete volumetric data
- Optical coherence tomography guided corneal needle insertions
- Optimize treatment of macular degeneration
- I think it would be good to have example of what a contrast difference looks like

## Proposed Method
- visual field testing on single position (eye position?) is sequence of tuples $p_j = <i_j, y_j>$ where $i_j = [i^L, i^H]$ is intensity of $j^{th}$ stimululus and has upper and lower bound, binary $y_j$ indiciates if tester responds, $N$ total rounds of testing
- Each location has true sensitivity $C$, variability in response due to variety of factors (ie: boredom, looking away from center), gives rise to frequency of seeing curve (FOS) 
- Assume response has bernoulli distribution with prob $=1-P_{fn} = (1-P_{fn}-P_{fp}) * \Phi(i|C, \sigma_r)$ where $\Phi$ is normal CDF with mean $C$ and std $\sigma_r$ 
- Assume a particular equation for $\sigma_r$ which depends on $C$  (where does this come from?)
- Should it vary with number of trials?
- Cost function is expected value of absolute difference between $C$ and estimate plus number of trials (with weighting term $\alpha$) 
- Great explanation of ZEST relative to overall problem statement
- 276: 'The deep reinforcement learning'
MDP formulation
    State: likelihood vector of different (integer) sensitivies from lower to higher bound, also has indicator for no stimulus needed
    Actions: intensity to disploy from low to high and stop action
    - 303: 'based on tester's response'
    Reward: stepwise improvement in estimate (not observable!)
        how to pick $\alpha$?
    - 340: wrong cite for DQN

## Experiments
Dataset description
- 119 visual fields (patients?)
- 55 w/out loss, 64 with
- assume false positive and negative 3%
- varies prior sensitivity densities: uniform, bimodal, generalized logistic (fit to training data)
- Don't understand paragraph at 469
- How were hyperparameters determined/tuned?
- Hmm, don't like 511: $\alpha$ set arbitrarily? $\delta$ set so 'close'
- From table 2: pdfe hard to draw conclusions, seems on tradeoff curve
- 519: under pdfb std 2.237 vs 2.266
- 521: strongly disagree with this
- 526: indicating more accurate and robust, any strong claims? Is difference meaningful
- Like presentation style of figure 6 more, also shows about what I'd expect (ZEST more or less equivalent when prior good)