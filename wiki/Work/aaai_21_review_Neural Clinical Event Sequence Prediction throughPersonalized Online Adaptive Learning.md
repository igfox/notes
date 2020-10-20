# Neural Clinical Event Sequence Prediction throughPersonalized Online Adaptive Learning
Review for AAAI 2021
## Abstract
- Prediction models for event sequences "of great importance for defning representations of a patient state and for improving care"
- patient variability important for this (in what sense?)
- Online model update
Seems to test on MIMIC 3
Baselines are GRU, CNN, RETAIN
Look at online parameter adaptation for output layer, transition layer, or full GRU network
model switching (maybe look at Jeeheh's stuff)
There seems to be something weird around non-repetitive and repetitive event prediction

## Introduction
- 28: 'the patient-specific variability'
- 35: 'a variability'
- 36: 'how to recover, at least'
- Big things: online gradient based model adaptation, and a switching system between global and personalized model

## Related Work
- Patient specific models:
    - Cites for line 77?
- Online adaptation
    - Direct adaptation mentioned (maybe not what this is)
- Online switching between candidate models
- Neural Clinical Event Sequence Prediction

## Methedology
- At each time step, predict $|E'|$-length binary event vector given input (also $y$)
    - Notation pretty non-standard and confusing, might want to change 
- 220: 'the novel learning framework'
- Exponential decay function on loss to bias towards recent terms
- 264 'deters the model'
- Issue with indexing in algorithm 1
- Algorithm 2 is either written wrong or there is a major problem

## Experiments
- should have 0 l2 option if this is used for fine-tuning
- Where do hyperparameter choices come from? How sensitive is performance to selection?
- AUPR differences over time increases
- 383: 'is almost close as'
- Some advantage to switching
- switching models results: slight improvement, but why select population model so often when AUPR worse?
- 438: "and it is somehow expected"
- Switching model helps for non-repetitive events, hmmmm
- Don't understand axis for figure 7, finding seems to contradict repetitive vs non-repetitive finding