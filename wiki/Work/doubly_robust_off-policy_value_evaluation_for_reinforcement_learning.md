# Doubly Robust Off-policy Value Evaluation for Reinforcement Learning
- This one only focuses on value estimation
- Empirical evidence for their method, some theoretical results showing overall hardness
- Again seems to consider direct and importance sampling based methods as baselines, this time for RL
- seems to make use of state and state-action value estimates, defined recursively
- Results seem to suggest that WIS is better than IS, DR-v2 is better than baseline DR, DR-v2 better than WIS, REG (DM?) is quite competetive

## Introduction
- 2 big ways to do OPE:
	1. Fit MDP, low variance but bias can be high and impossible to quantify
	2. Importance sampling: unbiased but high variance, increasing in length of trajectory

## Related Work
- Only works on value estimation for starting states, not 'full policy' value estimation
- Also considers use of work for policy iteration

## Background
- $H$ is finite-length horizon
- $\pi_0$ is behavior policy, $\pi_1$ is target
- Regression estimate: learn MDP model $\hat{M}$ from data and use to calculate recursive value (could also just sample trajectories or learn to directly estimate)
- Importance sampling: same as for bandits, but on importance ratio defined on per-step basis
		- importance weighting can be done on a trajectory level:
				$V_{IS} = \rho_{1:H} \sum_{t=1}^H \gamma^{t-1} r_t$
		- can also be done on  a per-step level:
				$V_{step-IS} = \sum_{t=1}^H \gamma^{t-1} \rho_{1:t} r_t$
- With either IS-approach, value estimate is empirical average over a dataset
- For weighted importance sampling (biased but consistent): choose weight$w_t = \sum_{i=1}^{|D|} \rho_{1:t}^{(i)}/|D|$
	as the average cumulative importance weight for a given horizon time step, then weight importance weights by this
- Stepwise-weighted IS considered most practical point estimator in IS family
- Doubly Robust estimator (for CB):
	$V_{DR} = \hat{V}(s) + \frac{\pi_1(a|s)}{\pi_0(a|s)}(r-\hat{R}(s, a))$
- the data used to fit $\pi_1$ is assumed to be independent of evaluation data, the fit MDP should be as well, 'not required that $\pi_1$ and $\hat{R}$ be independent of each other' (?!)

## DR Estimator for Sequential Setting
- essentially, observe that step-IS breaks problem of $H$-length trajectory RL OPE into $H$ contextual bandit problems
- Stepwise-IS:
	$V^0_{step-IS}=0$
	$V^{H+1-t}_{step-IS} = \rho_t (r_t + \gamma V^{H-t}_{step-IS})$
	it then follows that $V^H_{step-IS} = V_{step-IS}$
- Similarly, for DR:
	$V^0_{DR} = 0$
	$V^{H+1-t}_{DR} = \hat{V}(s_t) + \rho_t (r_t + \gamma V_{DR}^{H-t} - \hat{Q}(s_t, a_t))$
- Some variance analysis I don't care to dig into
- We want confidence intervals to characterize uncertainty in estimates
- Can get these using concentration bounds (eg: Hoeffding) since estimator unbiased and trajectories assumed i.i.d., typically use 'normal' approximation of error (assuming normal distribution? References a Thomas paper)
- As an extension: can estimate transition dynamics to predict $s_{t+1}$, then expand $Q(s_t, a_t) = \hat{R}(s_t, a_t) + \gamma \hat{V}(s_{t+1})$, introduces bias in approximating $s_{t+1}$ but reduces variance (by how much?)
## Hardness of Off-policy Value Evaluation
- Provides a Cramer-Rao lower-bound on the variance of OPE in special and more general case (tree-MDP and DAG-MDP), similar to Shengpu stuff
- Doesn't look fun
- take-away: same variance as DR with perfect Q function
## Experiments
- target policy a mix between optimal policy (on dataset collected from $\pi_0$) and $\pi_0$ controlled by $\alpha$
- Evaluate on sepearate dataset, where $\hat{Q}$ estimated on seperate subset, can just use k-fold CV to get full estimate
- DR-BSL uses a constant baseline estimate of Q
- really, I don't think these results are too compelling, REG is very powerful (presumably data-dependent?), it's only when policy is mostly old that IS is any good, RMSEs of around .1