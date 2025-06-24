# Global AI Hackathon 2025 - 2nd Place Winning Solution

Welcome to my **2nd place winning solution** for the **Global AI Hackathon 2025 by Elucidata**! This repository contains the **complete pipeline**, from data preprocessing and model training to inference and submission.
[Competition Page](https://www.kaggle.com/competitions/el-hackathon-2025/overview)

---

## Problem Statement

Predict **cell-type abundance** from **stain-normalized histology images**. The dataset includes spatial metadata and gene expression targets for various cell types across annotated spots.

---

## Highlights of My Approach

* **Stain Normalization** using Vahadane's method via `staintools`
* **Backbone**: Pretrained `ConvNeXt-Small` with fine-tuning on the last blocks
* **Coordinate Fusion**: Fuses image features with normalized spatial coordinates
* **Custom Loss**: Combination of MSE and Soft Spearman Rank Loss using `torchsort`
* **TTA Inference**: Uses flip-based augmentations for robust predictions
* **Postprocessing**: Spatial Gaussian smoothing on predictions

---

## Data Preprocessing

* **Reference Image**: Slide S\_1 used to normalize all other slides
* **Vahadane Method**: For color deconvolution and stain normalization
* **Augmentation and Standardization**: Applied during training and evaluation via torchvision.transforms


---

## Model Architecture: `SpatialCellModel`

* **Backbone**: `ConvNeXt-Small` (features partially frozen)
* **Coordinate Encoder**: Linear layers to encode (x, y)
* **Fusion**: Element-wise product of image and coordinate features
* **Head**: Deep FC layers with dropout and batchnorm

---

## Custom Loss Function

A combination of:

* `MSELoss`
* Differentiable **Spearman correlation** loss (using `torchsort`)

```python
loss = alpha * spearman_loss + (1 - alpha) * mse_loss
```

---

## Training Details

* **Epochs**: 5
* **Batch Size**: 32
* **Optimizer**: AdamW
* **LR**: 1e-4 (head), 1e-5 (last ConvNeXt blocks)
* **Validation**: Manual region-based spot selection from slides

---

## Inference: TTA + Smoothing

* **TTA**: Horizontal, vertical, both flips
* **Spatial Smoothing**: Gaussian filtering based on (x, y) coordinates



## Results

* **Ranked 2nd**
* Spearman ρ: 0.72 (val), 0.759 (public LB), 0.71 (private LB)

---

## Let's Connect

Feel free to reach out for discussions, collaborations, or questions!

**Author**: Mohamed Masry
**Email**: [mohamed.masry.ai@gmail.com](mailto:mohamedmasry0120@gmail.com)
**LinkedIn**: [linkedin.com/in/mohamedmasry](https://www.linkedin.com/in/mohamed-masry-648790249/)

---

> This project demonstrates the power of combining histology imaging, deep learning, and spatial awareness. Thanks to Elucidata for organizing an inspiring hackathon!
