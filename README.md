# Fair k-Means Clustering for Continuous Attributes

## Overview

Traditional fair clustering methods primarily focus on **categorical sensitive attributes** such as gender, race, or ethnicity. However, many real-world fairness concerns involve **continuous sensitive attributes** such as age, income, or credit score, where an inherent ordering exists between values.

This project implements a fairness-aware extension of the classical **k-Means clustering algorithm** for continuous sensitive attributes. Inspired by recent research in fair clustering, the proposed approach introduces a **pooling-window based fairness objective** that promotes proportional representation of sensitive groups across clusters while preserving clustering utility.

The implementation is evaluated on the **UCI Adult Dataset**, using **Age** as the sensitive attribute.

---

## Problem Statement

Standard k-Means clustering optimizes only for cluster quality by minimizing the distance between data points and cluster centroids.

While this often produces high-quality clusters, it can also lead to demographic imbalance. For example, some clusters may become dominated by younger individuals while others predominantly contain older individuals.

Such disparities can become problematic when clusters are used for:

* Resource allocation
* Customer segmentation
* Healthcare planning
* Hiring and recruitment analysis
* Policy decision making

The goal of this project is to generate clusters that are:

1. **Useful** (high clustering quality)
2. **Fair** (better representation of age groups)

---

## Motivation

### Limitation 1: Matching Means is Not Enough

A common approach for handling continuous sensitive attributes is to ensure that each cluster has a similar mean sensitive attribute value as the overall dataset.

However, two distributions may have the same mean while having completely different shapes.

As a result, matching only the mean fails to guarantee proportional representation across the full sensitive attribute distribution.

### Limitation 2: KL Divergence Ignores Ordering

Many fair clustering approaches compare distributions using divergence measures such as KL Divergence.

However, KL Divergence treats sensitive groups as unordered categories.

For example:

* Age 20 and Age 21 are very similar.
* Age 20 and Age 80 are very different.

Traditional divergence measures fail to capture this natural ordering.

---

## Proposed Method

The proposed Fair k-Means algorithm augments the classical k-Means objective with a fairness objective.

### Utility Objective

The standard k-Means objective minimizes the total squared distance between points and cluster centroids. We call that utility loss(**J_U**)

### Fairness Objective

A novel pooling-window mechanism is used to compare the age distribution inside each cluster with the age distribution of the entire dataset.

Instead of treating age values independently, neighboring age groups are aggregated using sliding pooling windows.

This allows the fairness objective to account for the ordered nature of continuous attributes. We call this as the fairness loss(**J_F**)

### Combined Objective

The final optimization objective is:

J = (1-λ)J_U + λJ_F

where:

* J_U = utility loss
* J_F = fairness loss
* λ = fairness-utility tradeoff parameter

---

## Methodology

```text
Adult Dataset
      │
      ▼
Data Preprocessing
      │
      ▼
K-Means Initialization
      │
      ▼
Pooling Window Fairness Computation
      │
      ▼
Composite Objective Evaluation
      │
      ▼
Cluster Reassignment
      │
      ▼
Centroid Update
      │
      ▼
Convergence
```

The optimization alternates between:

1. Cluster Assignment Step
2. Centroid Update Step

until convergence.

---

## Dataset

### UCI Adult Dataset

The Adult dataset contains demographic and socioeconomic information collected from the 1994 U.S. Census.

**Sensitive Attribute**

* Age

**Examples of Features**

* Education
* Occupation
* Workclass
* Marital Status
* Hours Worked
* Capital Gain/Loss

---

## Experimental Evaluation

The proposed approach was evaluated against standard k-Means baselines.

### Results

| Method                 | Utility Loss | Fairness Loss |
| ---------------------- | ------------ | ------------- |
| Standard K-Means       | 0.0567       | 0.0917        |
| K-Means (Age Excluded) | 0.0479       | 0.0649        |
| Proposed Fair K-Means  | 0.0707       | 0.0034        |

### Key Findings

* Reduced fairness loss by approximately **96%** compared to standard k-Means.
* Achieved substantial improvement in demographic representation across clusters.
* Preserved clustering utility with only a modest increase in utility loss.
* Demonstrated that fairness and utility can be balanced through the proposed objective function.

---

## Technologies Used

* Python
* NumPy
* Pandas
* Scikit-Learn
* Matplotlib
* Jupyter Notebook

---

## Future Work

* Support multiple continuous sensitive attributes.
* Extend to additional benchmark datasets.
* Explore adaptive pooling-window strategies.
* Investigate scalable implementations for larger datasets.
* Compare against additional state-of-the-art fair clustering algorithms.

---

## References

1. Fair k-Means Clustering with a Continuous Sensitive Attribute.
2. UCI Adult Dataset.
3. Lloyd's k-Means Algorithm.
4. Recent literature on algorithmic fairness and fair clustering.
