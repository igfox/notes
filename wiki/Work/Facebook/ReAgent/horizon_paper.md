### Note: the below is an outdated version of the paper:
https://research.fb.com/wp-content/uploads/2018/10/Horizon-Facebooks-Open-Source-Applied-Reinforcement-Learning-Platform.pdf
End-to-end platform designed to solve industry applied RL
datasets large (millions to billions)
feedback slow
experiments must be careful
production use-case top concern
high dimensional feature and action space
Pytorch for modeling/training, Caffe2 for model serving (still? probably not, deprecated)
Data preprocessing: spark pipeline
Feature processing: automatically extracts metadata about feature types and normalizes
Model implementations: originally DQN, DDQN, Dueling [D]DQN
CPE: Stepwise importance samper, sped-wise direct sampling estimator, step-wise and sequential doubly-robust, MAGIC
optimized serving, transfers using ONNX to Caffe2 (originally)
testing algorithms using gridworld and some gym models

Data preprocessing
start with logged data in format
    MDP ID identifying chain (user?)
    sequence number (timestamp, location of the state)
    features
    action and prob
    reward
    possible actions at current step
 Processes to add in some new features, like next state/action
     reward timeline
     
Feature Normalization
features identified as binary, prob, cont, enum, quantile, boxcox
    for boxcox: essentially first check if approx normal, then do boxcox to see if approx normal, if not then quantile feature
    
Models
Discrete DQN: interested in distributional, multi-step, noisy-nets
Parametric: very large and ephemeral action space
    could have policy gradient with KNN search, but not needed if available actions small
    instead just represent actions with set of features
DDPG for tuning hyperparameters

Training
can be done on CPU, GPU, or multiple GPUs (across multiple machines internally)

Reporting and Eval
TD loss, if increasing unbounded know optimization is too aggressive (minibatch too small, lr too big)
MC loss: difference between Q value and observed estimate
CPE: do at end of each epoch, while batch RL assumes uniform shuffling many CPE assume sequential presentation from episode, seemingly resort after training

Model Serving
Can do deterministic or stochastic serving (using softmax over scores vs max)
uses ONNX to port models to Caffe2
production teams either execute policies, or use estimated scores for their own policies, logging possible actions, propensity for actions, chosen action, and reward (may come much later)

Platform testing
tests on some custom gridworld envs and openai baselines
continuous integration tests, running all unit tests upon push to master
tests run on pre-build Docker image

Case Study: Push Notifications
push notifications for time sensitive updates
filter possible selections with ML, predict click through rate (CTR) and likelihood notification leads to 'meaningful interaction', then filter based off of resulting combined score
However, no capture of long-term effect, and threshold not personalized
problem formulation:
    state: features about person and notification candidate
    action: send or drop notification candidate
    reward: interactions and activity on facebook, negative reward for each notification sent
    model: DQN
re-tuned daily with some exploration during serving
    batches in the 10s of millions
also used for notifications with page admins
Other applications:
    - 360 video team uses for adaptive bitrate

Future Work:
New models and model improvements
    getting the juiciest stuff from academia
CPE integrated with real metrics
    difficult to derive single reward scalar
    instead, metric panel, use CPE to estimate changes to panel under different changes
20200915163002


### More up-to-date version: 
https://arxiv.org/pdf/1811.00260.pdf