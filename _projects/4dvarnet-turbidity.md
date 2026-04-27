---
layout: page
title: 4DVarNet for Coastal Turbidity
description: End-to-end neural assimilation of ocean colour observations for sea surface suspended sediments
img: assets/img/Image3.gif
importance: 1
category: research
related_publications: true
github: https://github.com/Jvient/bob
---

This project gathers the methodological work conducted during my PhD on the **assimilation of the spatio-temporal dynamics of coastal turbidity** through the synergy between ocean colour satellite observations and hydrodynamic-sedimentary simulations.

The core idea is to combine **end-to-end neural architectures** (data-driven and 4DVarNet-style) with classical data assimilation principles, in order to reconstruct continuous sea surface suspended sediment concentration (SSC) fields from partial, cloud-affected satellite observations.

## Scientific context

Ocean colour remote sensing provides invaluable information on coastal water properties, but observations are limited by clouds, sun glint and revisit constraints. Hydrodynamic-sedimentary models (such as MARS3D coupled with sediment modules) can simulate SSC dynamics continuously, but suffer from uncertainties in forcings and parameterizations.

This project explores how deep learning can act as a bridge between observations and models, learning data-driven representations of the underlying dynamics and reconstructing missing information from sparse satellite measurements.

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/model.png" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
  
</div>

## Main contributions

- Development of **data-driven interpolation schemes** for sea surface SSC derived from ocean colour observations.
- Design of **end-to-end neural interpolation architectures** that jointly learn the dynamical prior and the inversion operator from data.
- Coupling of neural reconstructions with hydrodynamic-sedimentary model outputs to improve spatio-temporal coverage of turbidity fields in coastal environments.

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/4dvar.gif" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
</div>

## Associated publications & thesis

This project is associated with my PhD thesis and two peer-reviewed articles, all listed on the [publications page](/publications/):

- *PhD thesis* (2022) — Assimilation of the spatio-temporal dynamics of turbidity through synergy between ocean colour satellite measurements and hydrodynamic simulations. Université de Bretagne Occidentale.
- Vient et al., *Remote Sensing* (2022) — End-to-End Neural Interpolation of Satellite-Derived Sea Surface Suspended Sediment Concentrations.
- Vient et al., *Remote Sensing* (2021) — Data-Driven Interpolation of Sea Surface Suspended Concentrations Derived from Ocean Colour Remote Sensing Data.

## Links

- Code repository: [Jvient/bob](https://github.com/Jvient/bob)
