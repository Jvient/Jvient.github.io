---
layout: page
title: U-Net ML Workshop
description: Hands-on workshop on deep learning for sea surface temperature reconstruction
img: assets/img/unet-workshop.png
importance: 2
category: development
github: https://github.com/Jvient/U-Net_ML_Workshop
---

A pedagogical workshop introducing deep learning concepts applied to oceanographic data, designed for engineering students at UniLaSalle. Through a guided Jupyter notebook, participants build a **U-Net architecture** to reconstruct **Sea Surface Temperature (SST)** fields from partial satellite observations.

## Learning objectives

- Discover the principles of convolutional neural networks and the U-Net architecture.
- Apply deep learning to a real geophysical problem — reconstructing missing SST data caused by cloud coverage in satellite imagery.
- Manipulate spatio-temporal oceanographic datasets in Python (NumPy, PyTorch, Xarray).
- Train, evaluate and visualize a neural network reconstruction pipeline end to end.

## Content

The workshop is built around a self-contained Jupyter notebook (`Unilasalle_LS_SST.ipynb`) that guides students step by step:

- **Data exploration**: loading and visualizing SST satellite products.
- **Problem framing**: simulating realistic cloud masks to create a supervised reconstruction task.
- **Model building**: implementing a U-Net from scratch with PyTorch.
- **Training & evaluation**: optimizing the network, monitoring losses, and assessing reconstruction quality.
- **Discussion**: limitations, possible extensions, and links to operational oceanography.

## Audience

This workshop is targeted at **engineering students** with a basic background in Python and machine learning, willing to discover concrete applications of deep learning in Earth and ocean sciences. It can be run in a few hours during a dedicated session.

## Links

- GitHub repository: [Jvient/U-Net_ML_Workshop](https://github.com/Jvient/U-Net_ML_Workshop)
- Notebook: [Unilasalle_LS_SST.ipynb](https://github.com/Jvient/U-Net_ML_Workshop/blob/main/Unilasalle_LS_SST.ipynb)
