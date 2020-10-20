# MMNE: Multi-view Mobility Network Embedding for Individual Health Condition Prediction
Reviewed for AAAI 2020

## Abstract
- "To predict their health condition": who's?
- Using mobility data and demographics to predict outcomes
- need to model relationship between locations and infer missing data
- Learn user embeddings unsupervised
- Multi-view data embeddings, graphs of user-to-user, location-to-location, and user-location interaction

Results Survey: seems to beat out other graph embedding approaches compared to, ablations show it's mostly coming from (sem), whatever that is
## Introduction
- The main challenges in the paper:
    - user/location correlations
    - Incorporate semantic information
    - Missing data, specifically spatio-temporal data from different sources
    - Covington very odd cite for MLP
    - May want non-graph NN comparison
    - Are they first to study health sensing with individual location data?
    - Should get rid of section numbering

## Preliminaries
- Should they really be quaternions?
- Mobility record: User u visit location l at time t for duration d
- each user has demographic quaternion with gender, age, income level, education level
- each location has point of interest distribution vector which shows the number of POIs within location
- learns $\epsilon$ a low dimensional user embedding to predict health condition
    - but what is health condition?


## Methods
- too many uses of 'complicated correlation', overall repetition with major challenges
- Multiple graphs used for learning:
    - Spatial interaction: user to location, edge weight is visitng frequency
    - User with spatial link: co-locating frequency
    - User with semantic link: similarity between demographic tuple (cosine similarity)
    - Location spatial: exponential kernel on the distance between locations
    - location semantic: similarity between POI distribution (what similarity, what POI categories?)
- Intra-view and cross-view node representation learning (with user-location graph providing bridge?)
- In user-location graph: use cross entropy loss to make softmax dot product of location-user similar to fit (I believe) the visit frequency
- For missing data, bimodal deep autoencoder to get shared representation across semantic and spatial graph inputs for each domain
- I don't understand equation 5: reconstruct correlations based on embeddings, is this a reconstruction loss, what is second $L_{sem}$? Should there be a superscript?
- learned embeddings fed into 'off-the-shelf classifier model for health inference'
## Experiments
- 900 users, 461 healthy and 431 unhealthy (other 8?)
    - unhealthy determined by visiting hospital during 2 month period (location label leakage?)
- only use mobility records before first visit to hospital
- filter out users who never show up around hospital?
- users with <50 records filtered
- Evaluate using F1, Precision, Recall
- Baseline approaches: random guess, demographics and a few manual features, DeepWalk on user domain (which subgraph?), Walk2Friends on user-location graph, Heter-GCN on full graph
- MMNE (sem): semantic link for demographics and POI data, (spa) only uses spatial links, (user) and (loc) self-explanatory (though includes cross-view)
- Lasso classifier used, 5 random subsets of data used for cross-validation
- Are the differences in performance similar across similar user-splits?
- No mention of hyperparameters
- Does timespan of trace relate to eventual label?
- Why are performance subsets measured using accuracy in Figures 3 and 4

20200922174322