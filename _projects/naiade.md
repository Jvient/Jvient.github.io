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

**NAIADE** is an open-source Python library designed to help oceanographers and Earth scientists apply modern **artificial intelligence** methods particularly **deep learning** and **data assimilation** to their data, without needing to be specialists in machine learning.

## Why NAIADE?

Working with ocean data is challenging: satellite observations are often **incomplete** (clouds, swaths, revisit gaps), in-situ measurements are **sparse**, and physical models depend on uncertain forcings and parameterizations. Reconstructing continuous, physically consistent fields from this fragmented information is a recurrent problem in oceanography.

Modern AI offers powerful tools to address this challenge. **Neural networks** can learn complex patterns from large datasets, while **data assimilation** combines observations and model knowledge in a rigorous way. NAIADE provides a unified framework that brings these two worlds together, with a focus on real-world oceanographic applications.

The goal is to **lower the entry barrier**: a scientist familiar with Python and oceanography should be able to set up a meaningful AI experiment in a few hours, rather than weeks of boilerplate engineering.

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/rl_progression.gif" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    NAIADE in action — neural reconstruction of observations.
</div>

## What you can do with NAIADE

- **Reconstruct missing data** in satellite products (e.g., fill cloud gaps in sea surface temperature, ocean colour, sea level anomalies).
- **Interpolate sparse measurements** in space and time using learned representations of the underlying dynamics.
- **Forecast** the evolution of geophysical fields by combining historical observations with model outputs.
- **Combine observations and models** through neural data assimilation, where the AI learns how to optimally merge the two sources of information.
- **Benchmark methods** on common oceanographic test cases, with reproducible training and evaluation pipelines.

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="/assets/img/gnn_network_analysis.png" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    GNN evaluation with buoy networks.
</div>

## Key ideas behind the library

NAIADE is built around a few core concepts:

- **Modular building blocks**. Datasets, neural architectures, loss functions, training loops and evaluation metrics are decoupled, so that you can swap one component without rewriting the rest. Want to test a different architecture on the same problem? Change a few lines.
- **Physics-aware design**. Most modules are designed with geophysical fields in mind: spatio-temporal structure, missing data masks, observation operators, and physical constraints can be naturally integrated into the learning process.
- **From research to operations**. The same code can be used to prototype an idea in a Jupyter notebook and to run a reproducible experiment on a GPU cluster, which makes the transition from exploration to operational deployment much smoother.
- **Open and collaborative**. NAIADE is open source and intended to grow as a shared toolbox for the ocean data science community.
- 
<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="/assets/img/ae_network_evaluation.png" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    AE evaluation with generated buoy nbetwork.
</div>

## Who is it for?

- **Oceanographers and Earth scientists** curious about deep learning, who want a starting point that speaks their language rather than the language of computer vision benchmarks.
- **Data scientists and engineers** working on environmental applications, looking for a structured codebase tailored to geophysical data.
- **Students and teachers** in ocean science, remote sensing or applied AI, who can use NAIADE as a playground to learn and to teach.

## A short example of use case

With NAIADE, you can train a neural network to **learn the spatio-temporal patterns of the ocean** from the available data, and then use it to reconstruct missing pixels in a way that remains consistent with the dynamics it has seen. The same approach can be extended to **fuse observations with hydrodynamic model outputs**, getting the best of both worlds.

## Status & links

NAIADE is under active development, in close connection with my research on coastal turbidity and ocean data assimilation. Contributions, feedback and collaborations are very welcome.

- GitHub repository: [Jvient/naiade](https://github.com/Jvient/naiade)
