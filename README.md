# DynaMMo Synthetic Dataset Overview

This project involves the reproduction and evaluation of the **DynaMMo algorithm**, designed for missing value imputation in co-evolving sequences.

## Focus: Synthetic Dataset Creation

To robustly test the algorithm, a purely synthetic air-quality dataset generated from a Linear Dynamical System (LDS) was created. The synthetic data generation process is primarily driven by the `generate_dataset` function in `task_2_1.ipynb`.

### Dataset Formulation and Parameters
- **Sequence Length ($T$):** 300 timesteps.
- **Sensor Count ($d$):** 8 co-evolving dimensions representing varying air quality sensors (e.g., CO, NOx, NO2, O3, plus 4 temperature channels).
- **Latent Factors ($k$):** The data strictly stems from $2$ true hidden latent factors that drive the system:
  1. A multi-frequency **periodic traffic cycle**, simulated via sine wave composition (`sin(t) + 0.3*sin(3*t)`).
  2. A **slow autoregressive (AR) weather drift**, modelled as cumulative normally distributed noise (`cumsum(randn)`).

### The Generative Process
The synthetic dataset perfectly satisfies the generative model assumed by DynaMMo, providing an ideal zero-mismatch baseline representation of:
1. **Latent Dynamics:** $z_t = A_{true} z_{t-1} + \epsilon_t$
2. **Sensor Observations:** $x_t = C_{true} z_t + \delta_t$

By adhering strictly to the LDS assumptions, the generated dataset behaves identically to the paper’s fundamental mathematical construct.

### Missing Value Injection Setup
To stress-test different data recovery modalities, the observed values ($X_{true}$) are intentionally corrupted to produce input data ($X_{miss}$) by two missingness strategies:
- **Random Independent Missingness:** Injected consistently with a configurable base probability (typically 15% rate).
- **Block Missingness Gaps:** Simulates a harder case of total contiguous sensor dropout. Randomly injected intervals composed of 5 to 20 continuous missing values across varied sensors.

### Purpose of Emphasizing Synthetic Traces over Real Datasets (UCI Chlorine, CMU Motion Capture)
By testing the LDS model against this specifically engineered air-quality data ($X_{true}$), we obtain a mathematically exact ground truth. Unlike empirical real-world datasets which consist of complex dynamics that present substantial model mismatch, the purely gaussian synthetic data allows for a highly controlled evaluation of pure algorithmic correctness (such as verifying precisely higher-yielding RMSE scores).
