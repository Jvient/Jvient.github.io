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

NAIADE is a Python framework dedicated to the application of deep learning and data assimilation methods to oceanographic data. It provides modular building blocks to develop, train and evaluate neural architectures for the reconstruction, interpolation and forecasting of geophysical fields from partial and noisy observations.

## Main features

- Modular components for neural interpolation and data-driven assimilation of geophysical variables.
- Support for satellite-derived products (ocean colour, SST, altimetry) and model outputs.
  
<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="/assets/img/ae_network_evaluation.png" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    AE evaluation with generated buoy nbetwork.
</div>

- Flexible training pipelines, with utilities for handling spatio-temporal datasets, missing data masks and physical constraints.
- Designed for both research experimentation and integration into operational oceanography workflows.
  
<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="/assets/img/gnn_network_analysis.png" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    GNN evaluation with buoy networks.
</div>

## Context

NAIADE is developed in the context of ocean data science research, with a focus on coastal and shelf-sea applications such as turbidity, suspended sediments and surface dynamics. The library aims to bridge methodological advances in machine learning with the operational needs of physical oceanography.

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/rl_progression.gif" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    NAIADE in action — neural reconstruction of partial satellite observations.
</div>

## Links

- GitHub repository: [Jvient/naiade](https://github.com/Jvient/naiade)
