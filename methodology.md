# Experimental Methodology

## Overview

DeepCancer was evaluated as a breast histopathology image classification research system using the **BreaKHis (Breast Cancer Histopathological Image Classification)** dataset.

The experimental study investigates two separate classification tasks:

1. Binary classification — benign vs. malignant
2. Eight-class classification — breast tumor subtype classification

Separate models were trained and evaluated for each task.

## Dataset

The experiments use the open-access **BreaKHis** dataset introduced by Spanhol et al.

| Property | Value |
| --- | --- |
| Images | 7,909 |
| Patients | 82 |
| Staining | Hematoxylin and eosin (H&E) |
| Magnifications | 40×, 100×, 200×, 400× |
| Original image size | 700 × 460 |
| Image format | PNG |
| Color space | RGB |
| Model input size | 224 × 224 |

The dataset contains both benign and malignant breast tumor samples.

## Classification Tasks

### Binary Classification

The binary experiment distinguishes between:

- Benign
- Malignant

<pre>
Histopathology Image
        │
        ▼
   Preprocessing
        │
        ▼
   Hybrid Model
        │
        ▼
 ┌───────────────┐
 │               │
 ▼               ▼
Benign        Malignant
</pre>

### Eight-Class Classification

The multiclass experiment distinguishes eight tumor subtypes.

| Category | Subtype |
| --- | --- |
| Benign | Adenosis |
| Benign | Fibroadenoma |
| Benign | Phyllodes tumor |
| Benign | Tubular adenoma |
| Malignant | Ductal carcinoma |
| Malignant | Lobular carcinoma |
| Malignant | Mucinous carcinoma |
| Malignant | Papillary carcinoma |

## Image Preprocessing

Images were resized from their original **700 × 460** resolution to **224 × 224 pixels** for model compatibility.

The published experiments used different normalization strategies for the two classification configurations.

### Binary Configuration

The binary experiment used:

```text
Mean:              [0.5, 0.5, 0.5]
Standard deviation: [0.5, 0.5, 0.5]
```

### Eight-Class Configuration

The multiclass experiment used ImageNet normalization:

```text
Mean:              [0.485, 0.456, 0.406]
Standard deviation: [0.229, 0.224, 0.225]
```

## Model Architecture

The proposed model combines two parallel visual backbones:

- **ConvNeXt**
- **Swin Transformer**

ConvNeXt provides convolution-based local feature extraction, while Swin Transformer provides window-based attention for contextual representation.

<pre>
                     Input Image
                         │
                         ▼
                    Preprocessing
                         │
              ┌──────────┴──────────┐
              │                     │
              ▼                     ▼
           ConvNeXt          Swin Transformer
              │                     │
              ▼                     ▼
       Local Features       Contextual Features
              │                     │
              └──────────┬──────────┘
                         │
                         ▼
                 Hybrid Representation
                         │
                         ▼
                   Classification
</pre>

For a more detailed architectural description, see [Architecture](./architecture.md).

## Training Configuration

Training was performed in a **Google Colab** environment using an **NVIDIA A100 GPU** and the **PyTorch** framework.

The published experimental configuration includes:

| Parameter | Configuration |
| --- | --- |
| Framework | PyTorch |
| Training environment | Google Colab |
| GPU | NVIDIA A100 |
| Epochs | 20 |
| Optimizer | Adam |
| Loss function | Cross-Entropy Loss |
| Early stopping | Applied |

### Binary Training

The binary classification experiment used:

- Batch size: **32**
- Constant learning rate
- Cross-Entropy Loss
- Adam optimizer

### Multiclass Training

The eight-class experiment used:

- Adam optimizer
- Cross-Entropy Loss
- StepLR learning-rate scheduling
- `num_workers=4`
- `pin_memory=True`
- Early stopping

StepLR was used to reduce the learning rate during training according to the configured schedule.

## Loss Function

Cross-Entropy Loss was used during training.

The optimization objective is to minimize the discrepancy between the predicted class distribution and the ground-truth labels.

Model parameters were optimized according to this loss throughout training.

## Evaluation

Performance was evaluated using multiple complementary metrics rather than accuracy alone.

The published study reports:

- Accuracy
- Precision
- Recall
- Specificity
- F1-score
- Confusion matrices
- ROC curves
- AUC values

These measurements provide different views of classification behavior.

### Accuracy

Accuracy represents the proportion of correctly classified samples.

### Precision

Precision measures the proportion of positive predictions that correspond to positive ground-truth samples.

### Recall

Recall measures the proportion of positive ground-truth samples correctly identified by the model.

### F1-score

F1-score combines precision and recall through their harmonic mean.

### Confusion Matrix

Confusion matrices were used to inspect correct and incorrect predictions across classes.

### ROC / AUC

ROC analysis was used to evaluate class-wise discriminative behavior.

For the eight-class experiment, the published study reports AUC values exceeding **0.95 for several subtypes**.

## Experimental Workflow

<pre>
BreaKHis Dataset
       │
       ▼
Image Preparation
       │
       ▼
Resize to 224 × 224
       │
       ▼
Task-specific Normalization
       │
       ▼
Hybrid Swin Transformer + ConvNeXt
       │
       ▼
Training
       │
       ├── Adam
       ├── Cross-Entropy Loss
       ├── Early Stopping
       └── StepLR (multiclass)
       │
       ▼
Evaluation
       │
       ├── Accuracy
       ├── Precision
       ├── Recall
       ├── Specificity
       ├── F1-score
       ├── Confusion Matrix
       └── ROC / AUC
</pre>

## Experimental Scope

The methodology represents a **controlled benchmark experiment**.

The reported results were obtained using the BreaKHis dataset and should be interpreted within that experimental setting.

The study does not establish performance across:

- Independent clinical institutions
- Different pathology laboratories
- Different scanner systems
- Different staining protocols
- Different tissue preparation procedures
- Prospective patient populations

Accordingly, benchmark performance should not be interpreted as clinical validation.

## Reproducibility Boundary

This repository publicly documents the methodology and experimental configuration described in the research.

It does not currently distribute all artifacts required for complete independent reproduction of the experiment, including trained production model weights and the complete private implementation.

The BreaKHis dataset is maintained by its original dataset authors and is not redistributed through this repository.

## Related Documentation

- [Architecture](./architecture.md)
- [Experimental Results](./results.md)
- [Explainability](./explainability.md)
- [Prototype Interface](./interface.md)
- [Project Overview](../README.md)

## Published Research

**Histopathological Breast Cancer Classification Using a Hybrid Swin Transformer and ConvNeXt Architecture**

Murat Onur Kaderoğlu · Emre Şatır

[DOI: 10.5281/zenodo.18138734](https://doi.org/10.5281/zenodo.18138734)

---

**Research & Education Only — Not for Clinical Diagnosis**
