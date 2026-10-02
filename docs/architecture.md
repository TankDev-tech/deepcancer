# DeepCancer Architecture

## Overview

DeepCancer is an explainable AI research prototype for breast histopathology image classification.

The research architecture combines two complementary visual feature extraction approaches:

- **ConvNeXt** for convolution-based local and spatial feature extraction
- **Swin Transformer** for window-based attention and contextual representation

The resulting representations are used for histopathological image classification, while Grad-CAM provides an additional visual layer for inspecting regions associated with model predictions.

## High-Level Architecture

<pre>
                    Histopathology Image
                            │
                            ▼
                       Preprocessing
                            │
                 ┌──────────┴──────────┐
                 │                     │
                 ▼                     ▼
         ConvNeXt Backbone      Swin Transformer
                 │                     │
                 ▼                     ▼
          Local / Spatial       Global / Contextual
             Features               Features
                 │                     │
                 └──────────┬──────────┘
                            │
                            ▼
                      Feature Fusion
                            │
                            ▼
                     Classification
                            │
                 ┌──────────┴──────────┐
                 │                     │
                 ▼                     ▼
          Class Prediction        Grad-CAM
                 │                     │
                 ▼                     ▼
          Probability Output    Visual Explanation
</pre>

## Input Processing

The published research uses histopathological images from the BreaKHis dataset.

Original images are:

- RGB
- PNG format
- 700 × 460 pixels

Images are rescaled to **224 × 224 pixels** for model compatibility.

Preprocessing differs between the binary and eight-class experiments.

### Binary Classification

Binary classification uses normalization with:

- Mean: `0.5, 0.5, 0.5`
- Standard deviation: `0.5, 0.5, 0.5`

### Eight-Class Classification

The multiclass configuration uses ImageNet normalization:

- Mean: `0.485, 0.456, 0.406`
- Standard deviation: `0.229, 0.224, 0.225`

## ConvNeXt Branch

ConvNeXt acts as the convolutional component of the hybrid architecture.

Its role is to capture spatial and local tissue characteristics through convolution-based feature extraction.

The architecture incorporates modern convolutional design elements including:

- Large convolution kernels
- Residual connections
- Layer normalization
- GELU activations

Within DeepCancer, this branch provides a representation focused on local image characteristics and cellular morphology.

## Swin Transformer Branch

The Swin Transformer provides the transformer-based component of the architecture.

Instead of applying global self-attention directly across the entire image, Swin Transformer uses window-based multi-head self-attention and shifted windows.

This allows the architecture to model contextual relationships while maintaining computational efficiency.

Within DeepCancer, this branch contributes contextual and spatial relationship information complementary to the ConvNeXt representation.

## Hybrid Representation

The central research idea is to combine the complementary characteristics of convolutional and transformer-based visual representations.

<pre>
ConvNeXt
Local Features ───────────┐
                          │
                          ├──► Hybrid Representation ──► Classification
                          │
Swin Transformer ─────────┘
Contextual Features
</pre>

The two branches are therefore not treated as independent final classifiers.

Their learned representations contribute to the hybrid classification architecture evaluated in the research.

## Classification Tasks

Two separate classification tasks were investigated.

### Binary Classification

<pre>
Histopathology Image
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

The multiclass experiment distinguishes:

**Benign**

- Adenosis
- Fibroadenoma
- Phyllodes tumor
- Tubular adenoma

**Malignant**

- Ductal carcinoma
- Lobular carcinoma
- Mucinous carcinoma
- Papillary carcinoma

Separate models were trained for the binary and multiclass tasks.

## Explainability Layer

DeepCancer incorporates Grad-CAM visualization as an interpretability mechanism.

The prototype can expose visual information associated with:

- The original histopathological image
- Combined model attention
- Global/contextual representation
- Local feature representation

A representative implementation output is shown below.

![DeepCancer Grad-CAM interpretability](../assets/gradcam-interpretability.png)

The visualization is intended to support research-oriented inspection of model behavior.

Grad-CAM does not establish that a prediction is medically correct and should not be interpreted as clinical validation.

## Research Prototype Flow

The implemented research platform exposes the model through an image-analysis workflow.

<pre>
Research-use Conditions
        │
        ▼
Histopathology Image Upload
        │
        ▼
Image Preprocessing
        │
        ▼
Hybrid Model Inference
        │
        ▼
Probability Distribution
        │
        ├────────────► Classification Result
        │
        ▼
Grad-CAM Processing
        │
        ▼
Interpretability Output
</pre>

### Upload Interface

![DeepCancer image upload interface](../assets/prototype-image-upload.png)

### Analysis

![DeepCancer analysis process](../assets/prototype-analysis.png)

### Example Output

![Example malignant-class model output](../assets/prototype-result-malignant.png)

The probability shown in the example interface represents the output for that individual sample. It is not an overall model accuracy metric.

## Training Environment

The published experiments were conducted using:

- **PyTorch**
- **Google Colab**
- **NVIDIA A100 GPU**
- **Adam optimizer**
- **Cross-Entropy Loss**
- **20 epochs**
- Early stopping based on validation loss

The multiclass experiment additionally used StepLR learning-rate scheduling.

Training infrastructure and inference infrastructure should be considered separately.

The published prototype supports CPU-based inference even though model training was performed using GPU resources.

## Prototype Runtime

The accompanying research prototype was designed to make trained-model inference accessible without requiring the original training environment.

The published desktop implementation includes:

- Image upload
- Model inference
- Prediction output
- Grad-CAM visualization
- Offline operation
- CPU fallback
- Windows and macOS support

The published study reports approximately **2–3 seconds average inference time on low-resource hardware** for the lightweight desktop application.

Actual runtime depends on hardware, software environment, model configuration, and deployment conditions.

## Architecture Boundaries

DeepCancer currently represents an experimental research architecture.

The system has not established clinical generalization across:

- Multiple medical institutions
- Different scanner manufacturers
- Diverse staining protocols
- Different tissue preparation procedures
- Independent prospective patient populations

These factors are relevant to future validation and are outside the scope of the current benchmark evaluation.

## Research and Clinical Boundary

The architecture and prototype are intended for **research and educational use**.

DeepCancer is not presented as:

- A clinically validated diagnostic system
- An autonomous medical decision-maker
- A replacement for pathological evaluation
- A regulatory-approved medical device

Any transition from benchmark research to clinical deployment would require independent validation, prospective evaluation, appropriate regulatory processes, and assessment in representative clinical environments.

## Related Documentation

- [Experimental Methodology](./methodology.md)
- [Experimental Results](./results.md)
- [Explainability](./explainability.md)
- [Prototype Interface](./interface.md)
- [Project Overview](../README.md)

## Research

**Histopathological Breast Cancer Classification Using a Hybrid Swin Transformer and ConvNeXt Architecture**

Murat Onur Kaderoğlu · Emre Şatır

[DOI: 10.5281/zenodo.18138734](https://doi.org/10.5281/zenodo.18138734)

---

DeepCancer is an applied AI research project within the [TankDev](https://tankdev.tech) engineering portfolio.
