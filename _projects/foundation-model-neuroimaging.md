---
layout: project
title: Learning Sparse Latent Predictive Foundation Model for Multimodal Neuroimaging
excerpt: Neuro-JEPA is a sparse multimodal neuroimaging foundation model that combines a latent predictive objective with a Mixture-of-Experts architecture to encode brain MRI across T1w, T2w, and FLAIR imaging.
technologies:
  - Python
  - PyTorch
  - Vision Transformer
  - Mixture-of-Experts
  - Self-Supervised Learning
  - Medical Imaging
---

## Paper

📄 **[Download Full Paper (PDF)](/assets/papers/foundation-model-neuroimaging.pdf)**

<div style="width: 100%; height: 900px; margin: 30px 0; border: 2px solid #ddd; background: #f5f5f5; padding: 10px; border-radius: 4px; overflow: hidden;">
  <embed
    src="/assets/papers/foundation-model-neuroimaging.pdf#toolbar=1&navpanes=1&scrollbar=1"
    type="application/pdf"
    width="100%"
    height="100%"
    style="border: none;">
  <iframe
    src="https://docs.google.com/viewer?url=https://itsjinuhyun.github.io/assets/papers/foundation-model-neuroimaging.pdf&embedded=true"
    width="100%"
    height="100%"
    style="border: none; background: white; display: block;"
    frameborder="0">
    <p style="padding: 20px; text-align: center;">
      Your browser does not support embedded PDFs. <a href="/assets/papers/foundation-model-neuroimaging.pdf" target="_blank">Click here to download or view the PDF</a>.
    </p>
  </iframe>
</div>

## Overview

Brain MRIs are routinely acquired as multiple complementary sequences with unique contrast weighting, including T1-weighted (T1w) anatomic and fluid-sensitive T2-weighted (T2w) contrasts. However, methods for learning unified representations across the multitude of MRI contrast mechanisms at health-system scale have been lacking. This work introduces Neuro-JEPA, a sparse multimodal neuroimaging foundation model that combines a latent predictive objective with a Mixture-of-Experts (MoE) architecture to encode brain MRI across core T1w, T2w, and fluid-suppressed FLAIR imaging.

The study also conducts a systematic analysis of architectural, masking, objective, and sparsity design choices beneficial for robust neuroimaging multimodal representation learning.

## Key Details

- **Pretraining Scale**: Trained on 1,551,862 multimodal MRI scans from 428,647 studies (282,693 patients) after modality-specific preprocessing with data curation across three core structural brain MRI sequences.
- **Architecture**: Combines a Vision Transformer (ViT) backbone with joint-embedding predictive learning (JEPA) and a Mixture-of-Experts design, using a ViT-base configuration with 86 million activated parameters out of 122 million total.
- **Broad Evaluation**: Assessed on 25 tasks across three health systems (NYU Langone, NYU Long Island, and Massachusetts General Hospital) and 22 tasks from 12 public datasets, spanning unimodal, multimodal, and cross-domain configurations.
- **Baselines**: Benchmarked against frontier neuroimaging foundation models including BrainIAC, VoCo, and NeuroVFM.

## Results

Existing neuroimaging foundation models showed inconsistent gains over a simple convolutional neural network (CNN) baseline, whereas Neuro-JEPA achieved stronger and more consistent performance across all evaluated settings. These results establish a scalable framework for multimodal neuroimaging representation learning and highlight the need for foundation model evaluation protocols that include simple baselines, clinically heterogeneous cohorts, and controlled multimodal comparisons.

**Authors**: Haoxu Huang, Long Chen, Jingyun Chen, Jinu Hyun, James Ryan Loftus, Kara Melmed, Daniel Orringer, Jennifer Frontera, Seena Dehkharghani, Arjun Masurkar, Narges Razavian — NYU Center for Data Science and NYU Grossman School of Medicine.
