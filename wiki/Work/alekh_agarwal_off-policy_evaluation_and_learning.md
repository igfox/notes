# Off-policy Evaluation and Learning
from: http://alekhagarwal.net/bandits_and_rl/off_policy.pdf
- Focuses on a contextual bandit setting
- First taking a look at http://alekhagarwal.net/bandits_and_rl/cb_intro.pdf
        really more of a rabbit hole, but would be interesting to look into more sometime
        point on how systemic disagreement between target and behavior is irreconcilable
## Formal Problem Setting
- could just use target with some prob at data collection time, but often unknown (or multiple targets of interest)
- Question: given set of known target policies, what is the most efficient behavior policy to evaluate them? Probably just uniform sampling, but maybe something there
- The problem of 'evaluating' target is counterfactual, may not be causal statement about future if data distribution changes
- In off-policy learning, need to find policy that has max value, for evaluation just need an estimate of the value for a particular policy
- Assume rewards are bounded [0, 1], and that all state-action pairs are covered in logging data (at least for possible state-action pairs in target policy)
## Off Policy Eval
- In contextual bandits, just need reward estimation
### Inverse Propsensity Scoring
- propensity is prob $\mu(a_i, x_i)$ where $\mu$ is behavior policy
- The inverse propensity score is $R_{IPS}(x_i, a) = r_i \frac{1(a_i=a)}{\mu(a_i|x_i)}$
- The basic idea is to upweight the results of unlikely actions, but why if reward is already conditioned on action? I believe the answer is that we need it because the score is not intended to serve a complete functional interface, but a bridge between particular $r_i$ values and a true reward estimate. This is used in the value estimate to get a pretty natural looking formula for the value of a policy. Not sure about this though, need to consider my thought some more.
- From the proof of thm 7.3 (unbiasedness of IPS), we see the expected value of the reward wrt the behavior policy is the true reward, the inverse propsensity cancels out the prob introduced by expectation
- Building off of this to the value of a policy: take avg over all datapoints, for each possible action the target policy could take multiplied by IPS estimate

I'm not sure if it's worth going deeper here