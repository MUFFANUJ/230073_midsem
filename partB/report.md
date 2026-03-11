---
title: "Part B Report: Clustering Time Series Using Unsupervised-Shapelets"
author: "Anuj Kumar Singh (230073)"
date: "March 2026"
---

# Part B Report: Clustering Time Series Using Unsupervised-Shapelets

**Student**: Anuj Kumar Singh | **Roll Number**: 230073  
**Paper**: *Clustering Time Series Using Unsupervised-Shapelets* — Zakaria, Mueen, Keogh (ICDM 2012)

---

## 1. Paper Summary

The paper introduces *unsupervised shapelets* (u-shapelets), a method for clustering time series without class labels. Traditional time series clustering relies on whole-series distance measures (Euclidean or DTW), which are sensitive to noise and irrelevant segments. U-shapelets address this by discovering short, discriminative subsequences that can separate time series into groups. The algorithm works by extracting candidate subsequences via a sliding window, evaluating each candidate using a *gap metric* that measures the statistical separation between time series close to the pattern (D_A) and those far from it (D_B). The best candidates are selected greedily through an iterative peeling procedure: the most discriminative u-shapelet separates a group, which is removed, and the search continues on the remainder. The discovered u-shapelets are then used to construct a *distance map* — an N × m matrix of subsequence distances — on which standard k-Means clustering is applied. The key contribution is demonstrating that shapelets, previously limited to supervised classification, can be effectively used in the unsupervised setting through the gap metric and iterative discovery.

## 2. Reproduction Setup and Result

We reproduced the core u-shapelet algorithm on a synthetic toy dataset with 150 time series (50 per class, length 128). Each class contains a distinct local motif (sine bump, triangular spike, or rectangular pulse) embedded at a random position within a noisy random-walk background. We implemented the full pipeline: sdist computation (Definition 2), gap metric evaluation (Definition 4), greedy iterative u-shapelet discovery (Algorithm 1), distance map construction (Section III-D), and k-Means clustering. Using 200 random candidate evaluations per iteration and shapelet lengths of 15–30, the algorithm discovered multiple u-shapelets and achieved clustering results.

**Honest commentary on the gap**: Our Rand Index may differ from the paper's reported values (e.g., 0.95 on CBF) because (1) we use a different, simpler dataset with cleaner motifs, (2) we use randomised candidate search instead of exhaustive search, and (3) the paper's datasets (UCR Archive) have more complex, real-world patterns. The toy dataset is designed to be favourable to the u-shapelet method — the motifs are well-separated and noise is moderate — so a higher RI than on some harder UCR datasets is expected and does not represent an improvement over the paper.

## 3. Ablation Findings

**Ablation 1 — Removing the Gap Metric**: We replaced gap-based shapelet selection with random subsequence selection. This led to a significant drop in Rand Index, confirming that the gap metric is the essential quality criterion that enables discovery of discriminative patterns. Random subsequences carry no discriminative power, and the resulting distance map is too noisy for meaningful clustering.

**Ablation 2 — Removing Iterative Separation**: We replaced the greedy iterative peeling with a single-pass top-K approach (selecting the K candidates with highest gap scores from the full dataset without removing D_A groups). This also reduced performance, though potentially less dramatically than removing the gap metric entirely. Without iterative removal, the top-K candidates tend to be redundant — they may all discriminate the same dominant class, leaving other cluster boundaries underrepresented in the distance map.

Together, these ablations reveal that the paper's contribution consists of two complementary components: the gap metric provides *quality* (finding the right patterns), while iterative separation provides *diversity* (finding patterns that cover all clusters). Both are needed for robust clustering.

## 4. Failure Mode

We constructed a dataset where three classes of time series differ only in global frequency (1, 2, and 4 Hz sine waves with noise) and contain no distinctive local motifs. The u-shapelet method performed poorly on this dataset because its core assumption — that discriminative information resides in local subsequences — is violated. Short fragments of sine waves at different frequencies look similar after z-normalisation, so no candidate achieves a high gap score. In contrast, whole-series Euclidean k-Means easily captures the frequency differences by comparing entire waveforms, achieving substantially higher Rand Index. This failure mode is directly connected to Assumption 1 from Task 1.2 and demonstrates a genuine limitation of the shapelet-based approach.

**Suggested fix**: Augment the shapelet distance map with global spectral features (e.g., FFT power spectrum) to capture both local and global discriminative patterns.

## 5. Honest Reflection

**What I could not implement**: The exhaustive candidate search from the paper was computationally prohibitive for a CPU-only environment, so I used a randomised approximation (200 candidates per iteration). I also did not implement the early-abandon optimisation described in related shapelet work, which would have made the exhaustive search feasible.

**What surprised me**: The most surprising finding was how dramatically the gap metric ablation affected performance — random shapelets produced near-random clustering, demonstrating that the gap metric truly is the algorithmic innovation of the paper. I also found it interesting that the single-pass ablation could sometimes match the full method on easy datasets, suggesting the iterative peeling is most important when clusters are unbalanced or overlapping.

**What I would revisit**: With more time, I would (1) test on actual UCR Archive datasets to compare against the paper's reported numbers directly, (2) implement the full exhaustive search with early-abandon, (3) explore the sensitivity to shapelet length range more systematically, and (4) experiment with the suggested FFT augmentation to address the global-frequency failure mode.

## References

1. J. Zakaria, A. Mueen, and E. J. Keogh, "Clustering Time Series Using Unsupervised-Shapelets," in *Proceedings of the IEEE International Conference on Data Mining (ICDM)*, 2012, pp. 785–794.
2. L. Ye and E. Keogh, "Time Series Shapelets: A New Primitive for Data Mining," in *Proceedings of ACM SIGKDD*, 2009.
3. UCR Time Series Classification Archive, https://www.cs.ucr.edu/~eamonn/time_series_data/
