# UCD Computational Network Analysis COMP47270 - case studies

This module focused on modern computational techniques to extract information from large-scale networks and the case studies explored
algorithms to find dense clusters in networks, community finding in social networks and trust networks and their application in recommender systems.  

The studies were done in Python and presented in Jupyter notebooks. While the recommended way to view these files is by running the notebooks locally,
github ipynb viewer does also a good job with some minor formatting issues.

## Case Study 1: Network Analysis and Modeling with Real-World Graphs 

This study explores large-scale network analysis using graphs from the SNAP repository, including:
- Stanford web graph (directed)
- Enron email network (undirected)

Key steps in the analysis include:

- Computing **Strongly Connected Components (SCCs)** using a custom iterative DFS implementation.
- Measuring **graph metrics** such as degree assortativity, clustering coefficient, and average shortest path (sampled for efficiency).
- Fitting degree distributions to a **power-law model** and estimating parameters β and α.

The study also explores network generation models:
- **Price’s preferential attachment model** – reproduces degree distributions and edge counts similar to real networks.
- **Stochastic Kronecker product graphs** – closely matches real-world metrics for undirected graphs, with some limitations in clustering coefficient.

Despite the computational challenges of large graphs, the models successfully capture key network properties, showing how simple algorithms can effectively simulate complex networks.

## Case Study 2: Community Detection with Ratio-Cut Algorithm

This study investigates community detection in networks by implementing a ratio-cut algorithm and comparing its performance
to NetworkX’s modularity-based community detection.

Key Components:
- **Ratio-Cut Algorithm** – recursively partitions a graph using the Fiedler eigenvector and selects the best cut based on modularity scores.
- **Quality Metrics** – modularity is used to assess partition quality, and the Rand Index is used to compare partitions between algorithms.

Experiments & Findings:
- **Real-world network (ca-GrQc)** – The ratio-cut algorithm struggled with small, uneven communities, producing a single large partition. NetworkX outperformed it in modularity scores.
- **Small synthetic network (500 nodes, LFR benchmark)** – Both ratio-cut and NetworkX successfully recovered communities, achieving high modularity and Rand Index close to the ground truth.
- **Large synthetic network (5000 nodes, LFR benchmark)** – Ratio-cut found some communities but often created uneven partitions, resulting in a few very large communities and lower modularity 
  compared to NetworkX.

Overall, the study shows that simple spectral methods can detect meaningful communities, though performance depends heavily on graph structure and algorithm refinement.

## Case Study 3: Trust-Enhanced Rating Prediction

This case study implements a user-based collaborative filtering algorithm for rating prediction and explores whether incorporating trust relationships between users improves prediction quality. The evaluation uses the Epinions dataset.

The dataset is extremely sparse, with far more items than users, and many users have very few ratings. The trust network is directed and incomplete, with a majority of users not expressing trust in others.

The algorithm is based on **Resnick’s prediction formula**, enhanced with trust propagation. Three trust integration strategies were tested:

1. **Trust-first (UTFA)**: Uses trust links only, falls back to similarity.
2. **Trust-add (UTAA)**: Adds trust weights to user similarity.
3. **Trust-boost (UTBA)**: Multiplies user similarity by trust weights, propagates indirect trust.

Evaluation was performed using **10-fold cross-validation** over multiple user groups (all users, cold users, active users, users in the trust network), comparing RMSE and prediction coverage.

Key findings:
- Trust incorporation had **minimal impact on RMSE** across all users due to the sparse trust network.
- For users **in the trust network**, especially cold users, trust propagation **improved coverage and slightly reduced RMSE**.
- The trust-boost (UTBA) approach was the most effective, extending the neighborhood and enabling predictions where standard user similarity could not.

A **top-10 recommender** was also implemented using the same trust-enhanced algorithm, demonstrating practical use for serving personalized recommendations.

Trust-enhanced similarity **can increase the number of predictions** for cold-start users and users in the trust network, even if average RMSE gains are modest. The method is particularly valuable 
for sparse datasets and enhancing coverage rather than dramatically improving accuracy.
