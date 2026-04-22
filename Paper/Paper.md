---
title: "patchseg-grainseg: Lightweight Patch-Based Grain Segmentation for Geoscience Images"
tags:
  - image segmentation
  - geomorphology
  - sedimentology
  - image analysis
  - geoscience software
authors:
  - name: Pawan Thapa
    orcid: 0000-0002-4331-5315
    affiliation: 1
affiliations:
  - name: Department of Geography, University of Alabama, USA
date: 2026-04-20
bibliography: paper.bib
---

## Summary

Quantitative characterization of grain size and shape from images is central to sedimentology, geomorphology, petrography, and related geoscience fields. Manual grain delineation is time-consuming and difficult to reproduce, motivating automated image-based approaches. Recent segmentation pipelines increasingly rely on large pretrained foundation models, which can limit transparency, reproducibility, and long-term usability.

`patchseg-grainseg` is a lightweight, open-source Python package for grain and clast segmentation in geoscience imagery. The software combines patch-based image processing, a compact user-trained convolutional neural network (CNN), and classical watershed segmentation to generate non-overlapping grain masks. All algorithmic components are fully defined in code, require only modest computational resources, and can be trained directly on user-provided labeled images.

---

## Statement of Need

Grain-scale image analysis underpins studies of sediment transport, abrasion, fluvial dynamics, reservoir characterization, and granular material properties. Classical image-processing approaches—such as thresholding, morphological operations, and watershed segmentation—remain widely used but struggle in images with heterogeneous textures and touching grains [@Soille2003].

Convolutional neural networks have substantially improved grain detection accuracy across a range of geoscientific settings [@Mair2022; @Sylvester2025]. However, many modern tools rely on large pretrained models, including foundation models such as the Segment Anything Model [@Kirillov2023], which introduce heavy dependencies, opaque behavior, and difficulties in long-term reproducibility.

`patchseg-grainseg` addresses this gap by providing a transparent alternative that integrates a small, user-trainable CNN with classical instance segmentation. The package emphasizes reproducibility, interpretability, and accessibility rather than reliance on externally trained models.

---

## Methods

### Patch-Based Inference

Input images are subdivided into overlapping square patches to enable processing of arbitrarily large images without excessive memory requirements. Overlapping predictions are merged using Hann-window weighting, which reduces patch-edge artifacts and improves spatial consistency [@Pielawski2020].

### Lightweight CNN for Grain Detection

A compact U-Net–style convolutional neural network, containing approximately 150,000 trainable parameters, is used to generate pixel-wise grain probability maps. The network is trained directly on user-provided annotations using Dice loss to mitigate class imbalance. Unlike pretrained foundation models, all network weights are learned locally and fully reproducible.

### Instance Segmentation via Watershed

Binary grain masks produced by the CNN are converted into instance-level segmentations using marker-controlled watershed segmentation on a distance-transform energy surface. This classical approach effectively separates touching grains while maintaining algorithmic transparency [@Soille2003].

---

## Validation and Example Application

The method is demonstrated on a manually annotated gravel-bed image. Performance is evaluated using pixel-level precision, recall, F1 score, and Intersection-over-Union (IoU). An ablation analysis shows that classical watershed segmentation alone provides a strong but limited baseline, while CNN-seeded watershed significantly improves segmentation performance on complex imagery, consistent with previous findings in geoscientific image analysis [@Mair2022].

---

## Availability

- **Source code:** https://github.com/thapawan/patchseg-grainseg
- **License:** MIT
- **Dependencies:** NumPy, SciPy, scikit-image, Matplotlib, Pandas  
- **Optional dependency:** TensorFlow (for CNN training and inference)

---

## Acknowledgements

The author acknowledges the use of **Google Gemini** and **Microsoft Copilot** as assistive tools during development for code debugging, documentation drafting, and exploratory refactoring. All algorithmic decisions, implementations, experiments, and evaluations were designed, executed, and validated by the author.

---
