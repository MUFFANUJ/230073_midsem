# Dataset Information

## Synthetic Time Series Dataset for Unsupervised-Shapelet Clustering

### Overview
This dataset is synthetically generated for reproducing and testing the Unsupervised-Shapelets algorithm from the paper:
**"Clustering Time Series Using Unsupervised-Shapelets"** (Zakaria, Mueen, Keogh — ICDM 2012)

### How the Dataset is Obtained
The dataset is generated programmatically inside the notebooks (`task_2_1.ipynb`) using NumPy's random number generator with a fixed seed (`RANDOM_SEED = 42`) for full reproducibility.

No external download is needed.

### Dataset Description
- **Number of samples**: 150 time series (50 per class)
- **Time series length**: 128 time points each
- **Number of classes**: 3
- **Structure**: Each class has a distinct short local motif (shapelet-like pattern) embedded at a random position within a noisy background signal.
  - **Class 0**: Embedded sine-wave bump motif
  - **Class 1**: Embedded triangular spike motif
  - **Class 2**: Embedded square pulse motif
- **Noise**: Additive Gaussian noise (σ = 0.5) on a smooth random-walk background

### How it is Used
- **task_2_1.ipynb**: Dataset generation, visualisation, and preprocessing
- **task_2_2.ipynb**: Core u-shapelet algorithm implementation and clustering
- **task_2_3.ipynb**: Result comparison, visualisation, and reproducibility checklist
- **task_3_1.ipynb**: Ablation experiments on the same dataset
- **task_3_2.ipynb**: Failure mode analysis uses a different synthetic dataset (global-pattern dataset) generated within that notebook

### Limitations Compared to the Original Paper
- The original paper uses the UCR Time Series Archive datasets (e.g., CBF, Trace, etc.) which contain real-world time series with more complex patterns.
- Our toy dataset has cleaner, more controlled motifs, making the clustering task somewhat easier.
- The original datasets have varying lengths and more subtle discriminative patterns.

### Saved Files
- `synthetic_ts_data.npy`: NumPy array of shape (150, 128) containing the time series
- `synthetic_ts_labels.npy`: NumPy array of shape (150,) containing the ground-truth labels (0, 1, 2)
