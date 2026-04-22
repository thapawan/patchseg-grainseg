# patchseg‑grainseg

**Lightweight Patch‑Based Grain Segmentation for Geoscience Images**

***

## Overview

`patchseg-grainseg` is a lightweight, transparent Python package for **grain and clast segmentation** in geoscience imagery, including gravel-bed photos, petrographic thin sections, and laboratory photographs of granular materials.

The package combines:

*   **Patch‑based image processing** for arbitrarily large images
*   A **small, user‑trained U‑Net** for robust grain detection
*   **Classical watershed segmentation** for instance separation
*   **Minimal dependencies** and full algorithmic transparency

Unlike recent tools based on large pretrained foundation models, `patchseg-grainseg` emphasizes **reproducibility, clarity, and scientific control**.

***

## Key Features

*   ✅ Patch‑based inference with Hann‑window blending (no GPU memory constraints)
*   ✅ Lightweight U‑Net (\~150k parameters), trained directly on user data
*   ✅ Classical watershed instance segmentation (no SAM, no Cellpose)
*   ✅ Fully redistributable models and code
*   ✅ Pixel‑level and grain‑level evaluation metrics
*   ✅ Publication‑ready visualizations and exports

***

## Design Philosophy

Many modern image segmentation tools rely on large pretrained models with opaque behavior and hidden dependencies.  
`patchseg-grainseg` intentionally takes a different approach:

> **Simple models + classical algorithms + full transparency**

This makes the package suitable for:

*   research reproducibility
*   teaching image analysis
*   method development
*   long‑term archival workflows

***

## Installation

Core dependencies:

```bash
pip install numpy scipy scikit-image matplotlib pandas
```

Optional (for CNN training and inference):

```bash
pip install tensorflow
```

***

## Quick Start

### 1️⃣ Train a tiny U‑Net on your own image

```python
from grainbench_production import GrainBenchProduction

gb = GrainBenchProduction(
    patch_size=128,
    stride=64
)

gb.train(
    img_path="4_P1060348_3.jpg",
    mask_path="4_P1060348_3_mask.tif",
    epochs=50
)
```

This trains a **small, fully defined U‑Net** using only your labeled image.

***

### 2️⃣ Segment the image

```python
from skimage import io, color

image = color.rgb2gray(io.imread("4_P1060348_3.jpg"))
labels = gb.segment(image)
```

The segmentation workflow:

1.  Patch‑based CNN inference
2.  Hann‑window blending
3.  Automatic thresholding
4.  Distance‑transform watershed

***

### 3️⃣ Export results

```python
from skimage import measure
import pandas as pd

features = pd.DataFrame(
    measure.regionprops_table(
        labels,
        properties=["area", "eccentricity", "solidity"]
    )
)

features.to_csv("grain_features.csv", index=False)
```

***

## Outputs

The pipeline can automatically generate:

*   ✅ Instance segmentation mask (`.tif`)
*   ✅ Grain morphometric tables (`.csv`)
*   ✅ Evaluation metrics (`.json`)
*   ✅ Publication‑quality figures (`.png`)

These outputs are directly usable in GIS, statistical tools, and publications.

***

## Evaluation

Segmentation quality is evaluated using:

*   Precision
*   Recall
*   F1 score
*   Intersection‑over‑Union (IoU)
*   Grain count accuracy

Classical segmentation results are provided as baselines, and the hybrid CNN + watershed approach consistently improves performance on challenging gravel imagery.

***

## Comparison with Existing Tools

| Feature                      | patchseg‑grainseg | ImageGrains | SAM‑based tools |
| ---------------------------- | ----------------- | ----------- | --------------- |
| Pretrained foundation models | ❌ No              | ✅ Yes       | ✅ Yes           |
| User‑trainable CNN           | ✅ Yes             | ❌ No        | ❌ No            |
| Patch‑based scaling          | ✅ Yes             | ✅ Yes       | ❌ Limited       |
| Transparency                 | ✅ High            | ⚠️ Medium   | ⚠️ Low          |
| GPU required                 | ❌ No              | ⚠️ Optional | ✅ Often         |

***

## Citation

If you use this software, please cite:

> Thapa, P. (2026). *patchseg-grainseg: Lightweight patch‑based grain segmentation for geoscience images*. Journal of Open Source Software, Under Submission.

(Exact citation will be updated after acceptance.)

***

## License

MIT License free for research, teaching, and commercial use.

***

## Acknowledgements

Inspired by prior work on grain segmentation in geoscience imagery and classical mathematical morphology. This package was developed with an emphasis on openness, reproducibility, and scientific rigor.

***

