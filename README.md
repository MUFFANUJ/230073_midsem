# Advanced Machine Learning - Mid-Semester Examination

**Student**: Anuj Kumar Singh  
**Roll Number**: 230073  
**Course**: Advanced Machine Learning (Semester 6)  

This repository contains the Mid-Semester Examination submission for the Advanced Machine Learning course. The examination focuses on the reproduction, experimentation, and analysis of a selected classical machine learning paper.

## Selected Paper
- **Title**: *Clustering Time Series Using Unsupervised-Shapelets*
- **Authors**: Jesin Zakaria, Abdullah Mueen, Eamonn J. Keogh
- **Venue**: ICDM 2012
- **Method Category**: ARIMA / Time Series

## Repository Structure

The repository is structured into two main parts corresponding to the examination phases:

### Part A
File at the root of the repository related to Part A:
- `llm_usage_partA.json`: Disclosure of LLM interactions during the paper selection and verification process.

### Part B (`partB/`)
The `partB/` directory contains the complete reproduction and analysis of the selected paper.

- **Question 1: Paper Understanding**
  - `task_1_1.ipynb`: Core contribution and architecture explained step-by-step.
  - `task_1_2.ipynb`: Key algorithm assumptions with violation scenarios.
  - `task_1_3.ipynb`: Analysis of baselines and method limitations.

- **Question 2: Toy Dataset Reproduction**
  - `task_2_1.ipynb`: Generation and preprocessing of a 3-class synthetic time series dataset with embedded local motifs.
  - `task_2_2.ipynb`: Core implementation of the u-shapelet discovery algorithm, gap metric, distance map construction, and k-Means clustering.
  - `task_2_3.ipynb`: Result comparison against the paper, visualisations, and a reproducibility checklist.

- **Question 3: Ablation Study**
  - `task_3_1.ipynb`: Two independent ablation experiments (removing the gap metric and removing iterative separation).
  - `task_3_2.ipynb`: Constructive failure mode scenario (global-frequency variations) where the u-shapelet assumption breaks down.

- **Question 4: Report and LLM Usage**
  - `report.pdf`: 2-page synthesis report summarising findings across all tasks.
  - `llm_task_1_1.json` to `llm_task_4_2.json`: 10 mandatory LLM usage disclosure logs detailing interactions for each task.

- **Supporting Files**
  - `requirements.txt`: CPU-installable Python dependencies.
  - `data/`: Contains the synthetic dataset arrays (`.npy`) and a `README.md` explaining the generation script.
  - `results/`: Contains all generated plots and visualisations formatted as `png` images.

## Setup and Reproducibility

To run the notebooks locally:

1. Setup a standard Python virtual environment (Python 3.12+ recommended).
2. Install dependencies:
   ```bash
   pip install -r partB/requirements.txt
   ```
3. Run notebooks sequentially. The dataset generation (`task_2_1.ipynb`) must be run prior to executing other notebooks.

All random seeds are fixed (`np.random.seed(42)`). No external datasets are required.
