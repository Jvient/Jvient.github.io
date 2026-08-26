---
layout: page
title: NAIADE
description: Neural Architecture for Inference and Assimilation of Dynamic Environments
img: assets/img/rl_progression.gif
importance: 1
category: development
related_publications: false
github: https://github.com/Jvient/naiade
---
**NAIADE** (*Network AI for Adaptive Design of Experiments*) is an open-source Python framework exploring how modern **artificial intelligence** can help design and optimize **oceanographic observation networks**. Given a large ocean area to monitor with a limited number of instruments, the central question becomes: *where should we place them, and how many are enough?*

## The scientific question

Ocean observations are expensive to deploy and maintain, and can only sample a tiny fraction of the ocean at any given time. This turns network design into a classical **Optimal Experimental Design (OED)** problem: choosing the sensor configuration that maximizes the information collected about the system state, under budget constraints.

NAIADE addresses this problem with three complementary AI paradigms — **generative reconstruction**, **graph representation learning** and **reinforcement learning** — applied to the same synthetic ocean and the same initial network, so their outputs can be cross-analyzed and combined.

## The synthetic testbed

To ensure reproducibility and control, NAIADE runs on a **synthetic 2D+T nature run** with realistic physical structures: double gyre, meandering zonal front, mesoscale eddies, seasonal cycle, k⁻³ spectral turbulence, and a Sea Surface Salinity (SSS) field coupled to Sea Surface Temperature (SST).

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/ocean_nature_run.png" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Example of the synthetic nature run — SST and SSS fields used as ground truth for all three modules.
</div>

## Three complementary AI modules

Each module targets a different facet of the OED question. They can be run **independently** or chained together in a **full pipeline** (RL → GNN → AE).

### 1 — Field reconstruction & uncertainty quantification (AE-UNet + MC-Dropout)

The first module trains an **Autoencoder built on a U-Net backbone** to reconstruct SST and SSS fields from sparse sensor measurements. The training objective combines a **Huber reconstruction loss** with **spatial-gradient regularization**, a **loss weighting on unobserved pixels** and **FiLM conditioning** on the observation mask.

Epistemic uncertainty is quantified via **Monte-Carlo Dropout**: at inference, dropout layers remain active and multiple stochastic forward passes are aggregated to produce a mean reconstruction and a per-pixel standard-deviation map, turning the network into a proxy for a Bayesian posterior over field reconstructions.

On top of this, NAIADE computes **Leave-One-Out (LOO) scores** per sensor and identifies **D-optimal buoy candidates** from the uncertainty maps — regions where adding a sensor would most reduce reconstruction variance.

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/ae_network_evaluation.png" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Autoencoder outputs — reconstruction, MC-Dropout uncertainty (σ), Leave-One-Out scores, gap zones and D-optimal buoy proposals.
</div>

### 2 — Graph representation & inductive evaluation (GAT + GraphSAGE)

The second module represents the observation network as a **graph**: each sensor is a node and edges are built from **temporal correlations** (above a configurable threshold) combined with **k-nearest-neighbor spatial connectivity**. Two graph neural networks are trained on this representation:

- A **Graph Attention Network (GAT)** learns attention weights over neighbors, providing an interpretable measure of which connections matter most for encoding the network state.
- A **GraphSAGE** model provides **inductive** embeddings, meaning it generalizes to **hypothetical sensors** at unseen positions without retraining — a key property when planning a network extension.

The module produces network-level diagnostics: sensor **contribution**, **redundancy** with neighbors, **coverage** maps, and **inductive scores** for candidate sensor positions provided by the user.

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/gnn_network_analysis.png" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    GNN network analysis — sensor contribution, uniqueness, correlation graph, coverage and per-sensor barplot.
</div>

### 3 — Sensor placement optimization (PPO on a candidate grid)

The third module casts sensor placement as a **sequential decision problem** solved by **Reinforcement Learning**. A **Proximal Policy Optimization (PPO)** agent operates on a discretized candidate grid, choosing at each step which position to activate or deactivate. The reward combines an **information gain** term (linked to the AE reconstruction error under the current configuration) and a **budget penalty** term (per active sensor).

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/rl_progression.gif" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Progression of the PPO agent — the sensor configuration evolves as the policy improves.
</div>

The evaluation produces a **Pareto front** of information versus network size, and NAIADE offers three complementary methods to pick an "optimal N*":

- **`pareto`**: information-vs-N sweep with policy and random baselines, Kneedle elbow detection on the front.
- **`efficiency`**: maximization of η(N) = info(N) / (1 + log N), with a soft log-penalty on network size.
- **`scalarized`**: multiple PPO trainings with increasing λ (marginal cost per sensor), best run selected by η.

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/rl_pareto_front.png" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Pareto front — information gained versus number of active sensors, with marginal-gain analysis.
</div>

The optimized **N*** configuration is systematically compared to a **light** configuration (**N*/2**), which is often the most operationally relevant question in observation network planning.

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/rl_two_configs.png" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Dense (N*) versus light (N*/2) sensor configurations — a key trade-off for real-world deployment.
</div>

## From modules to a full pipeline

The three modules are designed to work together. In `pipeline` mode:

1. **RL** proposes an optimized configuration on the candidate grid.
2. **GNN** analyzes this configuration to characterize redundancy, coverage and inductive value.
3. **AE** evaluates reconstruction quality on the optimized network and quantifies residual uncertainty.

All modules share the same nature run and the same **global seed control** (numpy, PyTorch CPU/GPU, deterministic cuDNN), so that experiments are fully reproducible from the two seeds `seed_ocean` and `seed_buoys`.

## Why it matters

Although NAIADE is currently demonstrated on synthetic SST/SSS fields, the underlying methodology is generic. The same AE + GNN + RL stack can be applied to a wide range of ocean and Earth observation problems where the central question is *"where and how many sensors should we deploy?"* — from coastal networks to open-ocean monitoring, from physical to biogeochemical variables.

NAIADE is intended as a **research and prototyping playground**: a modular, reproducible and physics-aware environment to experiment with AI-driven observation design, without having to reimplement each component from scratch.

## Under the hood

- **Language & frameworks**: Python, PyTorch, NumPy, SciPy, Matplotlib; optionally PyTorch Geometric for native `GATConv` / `SAGEConv` (with a manual fallback otherwise).
- **Data**: fully synthetic ocean generator (`dataset.py`) — no external dataset required.
- **Structure**: shared configuration and seed control (`config.py`), three standalone module scripts (`01_autoencoder.py`, `02_gnn.py`, `03_rl.py`), and an orchestrator (`run_demo.py`) supporting both `individual` and `pipeline` modes.
- **Outputs**: timestamped folders with checkpoints (`ae_best.pt`, `gnn_best.pt`, `sage_best.pt`, `rl_best.pt`), diagnostic figures, animated GIFs of the RL progression, and text reports.

A minimal run looks like:

```bash
# Full pipeline (RL → GNN → AE) with demo parameters
python run_demo.py --mode pipeline

# Pipeline + lightweight config evaluation (N*/2)
python run_demo.py --mode pipeline --eval_light

# Independent modules
python run_demo.py --mode individual
```

More detailed instructions, arguments and reproducibility recipes are provided in the [README](https://github.com/Jvient/NAIADE#readme).

## Status & links

NAIADE is under active development, in close connection with my research on AI-driven ocean observation. Feedback, discussions and collaborations are very welcome.

- GitHub repository: [Jvient/NAIADE](https://github.com/Jvient/NAIADE)
- Contact: [jean-marie.vient@shom.fr](mailto:jean-marie.vient@shom.fr)
