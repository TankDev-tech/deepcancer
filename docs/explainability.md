# Explainability

## Overview

DeepCancer incorporates **Grad-CAM (Gradient-weighted Class Activation Mapping)** as a visual interpretability layer for the histopathological image classification workflow.

The objective is to provide additional information about image regions associated with a model prediction rather than exposing only a predicted class and probability.

<pre>
Histopathology Image
        │
        ▼
   Hybrid Model
        │
        ├──────────────► Classification
        │
        ▼
Feature Activations
        │
        ▼
     Grad-CAM
        │
        ▼
Visual Interpretation
</pre>

Grad-CAM is used as a research-oriented model inspection mechanism.

It does not establish whether a prediction is medically correct and does not constitute clinical validation.

## Interpretability Output

A representative DeepCancer interpretability output is shown below.

![DeepCancer Grad-CAM interpretability](../assets/gradcam-interpretability.png)

The prototype visualization exposes four complementary views:

1. Original histopathological image
2. Combined Grad-CAM visualization
3. Global context branch visualization
4. Local feature branch visualization

This allows the model output to be inspected alongside visual information associated with different components of the hybrid architecture.

## Original Image

The original histopathological image provides the visual reference against which activation patterns can be compared.

It remains visible alongside the interpretability outputs so that highlighted regions can be examined in their original tissue context.

## Combined Grad-CAM

The combined Grad-CAM view provides an aggregated visualization associated with the model output.

<pre>
Model Prediction
       │
       ▼
Relevant Activations
       │
       ▼
Gradient Information
       │
       ▼
Activation Map
       │
       ▼
Overlay on Histopathology Image
</pre>

Higher-intensity regions indicate areas receiving stronger emphasis in the generated activation visualization.

The heatmap should be interpreted as a representation of model behavior, not as an independently validated pathological annotation.

## Global Context Branch

DeepCancer combines convolutional and transformer-based representations.

The **Swin Transformer** component is intended to capture contextual relationships through window-based self-attention and shifted-window processing.

The prototype interpretability interface includes a global/context-oriented visualization associated with this side of the hybrid representation.

This view can be used for research inspection of broader spatial relationships contributing to model behavior.

## Local Feature Branch

The **ConvNeXt** component provides convolution-based feature extraction focused on spatial and local image characteristics.

The prototype includes a local-feature-oriented visualization associated with this representation.

This provides another perspective for inspecting model behavior around localized tissue and morphological structures.

## Hybrid Interpretation

The interpretability interface is designed around the complementary roles of the two architectural branches.

<pre>
                  Histopathology Image
                          │
             ┌────────────┴────────────┐
             │                         │
             ▼                         ▼
        ConvNeXt                  Swin Transformer
             │                         │
             ▼                         ▼
     Local Representation      Contextual Representation
             │                         │
             └────────────┬────────────┘
                          │
                          ▼
                  Hybrid Prediction
                          │
                          ▼
                Combined Explanation
</pre>

The purpose of exposing these views is to make the model's internal behavior more inspectable during research and evaluation.

## Prediction and Explanation

The classification output and Grad-CAM visualization serve different purposes.

### Prediction

The classifier produces a class probability distribution and a resulting predicted class.

Example:

![Example malignant-class model output](../assets/prototype-result-malignant.png)

The probability displayed here belongs to an **individual analyzed sample**.

It must not be interpreted as overall model accuracy, sensitivity, specificity, or clinical confidence.

### Explanation

The Grad-CAM output provides a visual representation of regions associated with the model's decision process.

It does not independently verify that:

- The highlighted tissue is pathologically significant
- The highlighted region corresponds to a clinically relevant lesion
- The model reasoning matches expert pathological reasoning
- The prediction is medically correct

These questions require separate expert and clinical evaluation.

## Why Explainability Matters

A classification probability alone provides limited information about model behavior.

For research involving histopathological images, visual inspection can help investigate questions such as:

- Which image regions are associated with a prediction?
- Is model attention concentrated or distributed?
- Do different architectural branches exhibit different activation behavior?
- Are predictions potentially influenced by irrelevant image regions?
- How does model behavior change across different samples?

Grad-CAM can assist with these investigations, but it should be treated as an interpretability technique rather than proof of model correctness.

## Explainability Boundary

Visual explanations can appear intuitive while still being incomplete or misleading.

Accordingly, DeepCancer does not treat Grad-CAM as:

- Ground-truth lesion localization
- A pathology annotation
- A segmentation result
- Proof of causal reasoning
- Evidence of clinical validity
- A substitute for expert review

The visualization describes aspects of model activation associated with a prediction.

## Research Direction

The published research identifies further interpretability work as an important part of future development.

Potential research directions include:

- Additional gradient-based visualization methods
- Clinically meaningful attention analysis
- Comparison with expert pathology annotations
- Evaluation of explanation consistency
- Investigation of failure cases
- Analysis across different tissue and tumor subtypes

These are research directions and should not be interpreted as completed validation work.

## Related Documentation

- [Architecture](./architecture.md)
- [Experimental Methodology](./methodology.md)
- [Experimental Results](./results.md)
- [Prototype Interface](./interface.md)
- [Project Overview](../README.md)

## Published Research

**Histopathological Breast Cancer Classification Using a Hybrid Swin Transformer and ConvNeXt Architecture**

Murat Onur Kaderoğlu · Emre Şatır

[DOI: 10.5281/zenodo.18138734](https://doi.org/10.5281/zenodo.18138734)

---

**Research & Education Only — Not for Clinical Diagnosis**
