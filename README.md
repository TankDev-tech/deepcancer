# DeepCancer

**Explainable AI for Breast Histopathology Classification**

DeepCancer is an applied AI research project for breast histopathology image classification, combining **Swin Transformer** and **ConvNeXt** architectures with **Grad-CAM** explainability.

The project explores how convolutional neural networks and vision transformers can be combined for histopathological image analysis while providing visual explanations of model attention.

> **Research use only.** DeepCancer is a research and educational prototype. It is not a medical device and is not intended for clinical diagnosis or treatment decisions.

## Research Overview

The research evaluates a hybrid deep learning architecture on the **BreaKHis (Breast Cancer Histopathological Image Classification)** dataset.

The dataset contains:

- **7,909** histopathological images
- **82** patients
- **4** magnification levels: 40×, 100×, 200× and 400×
- H&E-stained breast tissue images
- Binary and eight-class classification tasks

Two independent classification problems were investigated:

**Binary classification**

`Benign ↔ Malignant`

**Eight-class classification**

Benign:
- Adenosis
- Fibroadenoma
- Phyllodes tumor
- Tubular adenoma

Malignant:
- Ductal carcinoma
- Lobular carcinoma
- Mucinous carcinoma
- Papillary carcinoma

## Hybrid Architecture

The proposed architecture combines two complementary feature-learning approaches.

<pre>
                 Histopathology Image
                         │
                         ▼
                    Preprocessing
                         │
              ┌──────────┴──────────┐
              │                     │
              ▼                     ▼
        ConvNeXt Backbone     Swin Transformer
              │                     │
              │                     │
       Local / Spatial        Global / Contextual
          Features                Features
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
          Prediction            Grad-CAM
                                    │
                                    ▼
                           Visual Explanation
</pre>

### ConvNeXt

ConvNeXt provides convolution-based spatial feature extraction and captures local tissue characteristics.

### Swin Transformer

Swin Transformer uses window-based self-attention and shifted windows to model contextual relationships across image regions.

### Hybrid Representation

The two backbones operate as complementary feature extractors.

The research investigates whether combining local convolutional representations with transformer-based contextual representations can improve histopathological classification performance.

## Experimental Setup

The BreaKHis images were rescaled to **224 × 224 pixels** for model compatibility.

Training was conducted using:

- **PyTorch**
- **NVIDIA A100 GPU**
- **Google Colab**
- **Adam optimizer**
- **Cross-Entropy Loss**
- **20 training epochs**
- Early stopping based on validation loss

Binary and multiclass models were trained separately.

The multiclass configuration additionally used StepLR learning-rate scheduling.

## Experimental Results

The following results were reported in the published study.

### Binary Classification

| Metric | Result |
| --- | ---: |
| Accuracy | **98.86%** |
| Precision | **99.07%** |
| Recall / Sensitivity | **99.25%** |
| F1-score | **99.16%** |

The binary task distinguishes **benign** from **malignant** breast histopathology images.

### Eight-Class Classification

| Metric | Result |
| --- | ---: |
| Accuracy | **93.49%** |
| Precision | **93.46%** |
| Recall | **93.49%** |
| F1-score | **93.43%** |

The multiclass task distinguishes eight histopathological tumor subtypes.

> These results are experimental benchmark results obtained using the BreaKHis dataset. They must not be interpreted as clinical validation or real-world diagnostic performance.

## Explainable AI with Grad-CAM

Classification probability alone does not explain which image regions contributed to a model prediction.

DeepCancer therefore incorporates **Grad-CAM** visualization into the prototype workflow.

<pre>
Histopathology Image
        │
        ▼
Hybrid Model
        │
        ├────────────► Classification
        │
        ▼
Activation Analysis
        │
        ▼
Grad-CAM Heatmap
        │
        ▼
Visual Interpretation
</pre>

The resulting visualization highlights image regions associated with the model's prediction and provides an additional layer for research-oriented model inspection.

Grad-CAM does not establish clinical correctness and should not be interpreted as a substitute for expert pathological evaluation.

## Research Prototype

The research was accompanied by an interactive software prototype.

The published study describes a Python-based desktop application providing:

- Histopathology image upload
- Model inference
- Prediction output
- Grad-CAM visualization
- Offline operation
- CPU fallback
- Windows and macOS compatibility

The current DeepCancer project also provides a web-based research experience for exploring the image-to-analysis workflow.

**Live research platform:** [DeepCancer.org](http://deepcancer.org/)

## Inference and Resource Constraints

The published prototype was designed to support inference without requiring dedicated high-end GPU hardware.

The study reports average inference times of approximately **2–3 seconds on low-resource hardware** for the lightweight desktop application.

This characteristic was investigated as part of making the research prototype accessible in resource-constrained environments.

Hardware, model configuration, operating system, and deployment environment can affect actual inference performance.

## Dataset

This research uses the open-access **BreaKHis** dataset introduced by Spanhol et al.

**Dataset characteristics**

| Property | Value |
| --- | --- |
| Images | 7,909 |
| Patients | 82 |
| Magnifications | 40×, 100×, 200×, 400× |
| Image format | PNG |
| Original resolution | 700 × 460 |
| Model input | 224 × 224 |
| Color space | RGB |
| Classification tasks | Binary + Eight-class |

The dataset itself is not distributed through this repository.

## Published Research

The research methodology and experimental results were published as:

**Histopathological Breast Cancer Classification Using a Hybrid Swin Transformer and ConvNeXt Architecture**

**Authors:** Murat Onur Kaderoğlu, Emre Şatır

**Journal:** Journal of Artificial Intelligence with Applications  
**Volume / Issue:** 6(1)  
**Pages:** 18–25  
**Year:** 2025

**DOI:** [10.5281/zenodo.18138734](https://doi.org/10.5281/zenodo.18138734)

[![DOI](https://zenodo.org/badge/DOI/10.5281/zenodo.18138734.svg)](https://doi.org/10.5281/zenodo.18138734)

## Research Limitations

The experimental results should be interpreted within the boundaries of the study.

Important limitations include:

- Training and evaluation were performed using a single public dataset
- Controlled benchmark evaluation does not represent clinical deployment
- Histopathological data can vary across institutions, scanners, staining protocols, and tissue-preparation procedures
- Distribution shifts may affect model performance
- Multicenter independent validation has not yet been demonstrated
- Prospective clinical validation would be required before clinical deployment

The published research identifies multi-institutional validation, domain adaptation, prospective evaluation with pathologists, and further interpretability work as important future directions.

## Clinical and Regulatory Boundary

DeepCancer is currently positioned as a **research and educational prototype**.

It is not presented as:

- A clinically validated diagnostic system
- An autonomous diagnostic tool
- A replacement for a pathologist
- An FDA- or CE-approved medical device
- A system for making treatment decisions

Experimental model outputs and Grad-CAM visualizations require appropriate expert interpretation.

Any future clinical use would require independent validation, appropriate regulatory processes, and evaluation within real clinical environments.

## Project Scope

This repository serves as a public technical and research showcase for DeepCancer.

### Publicly documented

- Research methodology
- Hybrid architecture
- Dataset characteristics
- Experimental setup
- Published benchmark results
- Explainability approach
- Prototype architecture
- Research limitations
- Publication metadata

### Not publicly distributed

- Production model weights
- Training infrastructure credentials
- Private deployment configuration
- Security-sensitive configuration
- Operational secrets
- Restricted or private datasets

## Documentation

Additional technical documentation will be maintained in this repository as the public project documentation develops.

- Architecture documentation
- Experimental methodology
- Explainability workflow
- Prototype interface
- Published research

## Project Resources

**Research platform:** [DeepCancer.org](http://deepcancer.org/)

**TankDev system overview:**  
[DeepCancer — Explainable AI for Breast Histopathology](https://tankdev.tech/tr/systems/deepcancer)

**Engineering case study:**  
[DeepCancer Case Study](https://tankdev.tech/tr/case-studies/deepcancer)

**Published research:**  
[Histopathological Breast Cancer Classification Using a Hybrid Swin Transformer and ConvNeXt Architecture](https://doi.org/10.5281/zenodo.18138734)

## Citation

If referencing the published research, please cite the associated journal article:

```text
Kaderoğlu, M. O., & Şatır, E. (2025).
Histopathological Breast Cancer Classification Using a Hybrid Swin Transformer
and ConvNeXt Architecture.
Journal of Artificial Intelligence with Applications, 6(1), 18–25.
https://doi.org/10.5281/zenodo.18138734
```

## About TankDev

[DeepCancer](http://deepcancer.org/) is an applied AI research project developed within the TankDev engineering portfolio.

[TankDev](https://tankdev.tech) works across custom software, applied artificial intelligence, process automation, web applications, and system integration.

---

**Research & Education Only — Not for Clinical Diagnosis**

Developed by **[TankDev](https://tankdev.tech)**  
[DeepCancer](http://deepcancer.org/) · [Research](https://doi.org/10.5281/zenodo.18138734) · [Case Study](https://tankdev.tech/tr/case-studies/deepcancer)
