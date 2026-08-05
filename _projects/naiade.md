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

**NAIADE** (*Network AI for Adaptive Design of Experiments*) is an open-source Python framework exploring how modern **artificial intelligence** can help design and optimize **oceanographic observation networks**. In other words: given a large ocean area to monitor with a limited number of instruments, *where should we place them?*

## The question NAIADE tries to answer

Ocean observations — from moored buoys to autonomous platforms — are expensive to deploy and maintain, and can only cover a tiny fraction of the ocean at any given time. A recurrent scientific and operational question is therefore:

> Given the physical dynamics of a region and a limited number of sensors, **which configuration will bring us the most information about the ocean state?**

This kind of problem is known as **Optimal Experimental Design (OED)**. NAIADE approaches it with three complementary AI methods, working on the same synthetic ocean and the same initial network of sensors, so their results can be compared and combined.

## Three complementary AI modules

NAIADE is organized around three modules, each answering a different facet of the OED question. They can be run **independently** or chained together in a **full pipeline**.

### 1 — Reconstructing the ocean from sparse observations (Autoencoder + Uncertainty)

The first module trains a **neural network (an AE-UNet with MC-Dropout)** to reconstruct 2D fields of **Sea Surface Temperature (SST)** and **Sea Surface Salinity (SSS)** from a handful of sensor measurements. In practice, the network learns what the full ocean field "typically looks like" from many training examples, and can then fill in the blanks when only a few sensors are available.

Because the network is stochastic, it also provides an **uncertainty map**: areas where the reconstruction is confident, and areas where it is not — the latter being natural candidates for adding new sensors. Combined with a **Leave-One-Out score** (removing one sensor at a time), this tells us how much each sensor really contributes to the reconstruction, and where the network of observations has meaningful gaps.

### 2 — Understanding the structure of a sensor network (Graph Neural Networks)

The second module represents the observation network as a **graph**, where each sensor is a node and edges connect sensors whose measurements are correlated in time and space. A **Graph Attention Network (GAT)** and a **GraphSAGE** model then analyze this graph.

The result is a much clearer picture of the network's structure: which sensors bring unique information, which ones are partly redundant, which regions are poorly covered. The GraphSAGE model is also **inductive**, meaning it can evaluate **hypothetical sensors** at new positions without retraining — a useful tool when planning a network extension.

### 3 — Optimizing sensor placement (Reinforcement Learning)

The third module treats sensor placement as a **decision problem**: an agent progressively chooses positions on a candidate grid, receives a reward based on the information gathered, and pays a cost for each active sensor. This agent is trained with **Reinforcement Learning (PPO)** and progressively learns placement strategies that balance **information gain** and **network size**.

The output is not a single configuration, but a **Pareto front** of trade-offs between the number of sensors and the information they provide, together with several methods (Kneedle elbow, efficiency criterion, scalarized reward) to identify a reasonable "optimal N*". NAIADE can also compare a **dense** configuration (N*) with a **lighter** one (N*/2), which is often the most operationally relevant question.

## From modules to a full pipeline

The three modules can be combined in a natural workflow:

1. **RL** proposes an optimized sensor configuration.
2. **GNN** analyzes this configuration to characterize redundancies and gaps.
3. **Autoencoder** evaluates how well this configuration reconstructs the ocean fields, with quantified uncertainty.

All modules share the same **synthetic ocean nature run** — a 2D+T simulation with realistic physical structures (double gyre, meandering zonal front, mesoscale eddies, seasonal cycle, spectral turbulence, and an SSS field coupled to SST) — and the same **seed control**, so that experiments are fully reproducible.

## Why it matters

The methodology is deliberately generic. Although NAIADE is currently demonstrated on synthetic SST/SSS fields, the same tools can be applied to a wide range of ocean and Earth observation problems where the central question is *"where and how many sensors should we deploy?"* — from coastal networks to open-ocean monitoring, from physical to biogeochemical variables.

NAIADE is intended as a **research and prototyping playground**: a place to experiment with AI-driven observation design in a reproducible, modular and physically-aware way, without having to reimplement everything from scratch.

## Under the hood

For those interested in the technical stack:

- **Language & frameworks**: Python, PyTorch, NumPy, SciPy, Matplotlib, optionally PyTorch Geometric.
- **Data**: fully synthetic ocean generator (`dataset.py`) — no external dataset needed to get started.
- **Structure**: shared configuration (`config.py`), three standalone module scripts (`01_autoencoder.py`, `02_gnn.py`, `03_rl.py`), and an orchestrator (`run_demo.py`) for the full pipeline.
- **Outputs**: timestamped folders with checkpoints, diagnostic figures, animated GIFs of the RL agent learning, and text reports.

A minimal run looks like:

```bash
# Full pipeline (RL → GNN → AE) with demo parameters
python run_demo.py --mode pipeline

# Or individual modules
python run_demo.py --mode individual
```

More detailed instructions, arguments and reproducibility recipes are provided in the [README](https://github.com/Jvient/NAIADE#readme).

## Status & links

NAIADE is under active development, in close connection with my research on AI-driven ocean observation. Feedback, discussions and collaborations are very welcome.

- GitHub repository: [Jvient/NAIADE](https://github.com/Jvient/NAIADE)
- Contact: [jean-marie.vient@shom.fr](mailto:jean-marie.vient@shom.fr)
