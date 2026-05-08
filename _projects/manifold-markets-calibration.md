---
layout: project
title: A Machine Learning Analysis of Calibration on Manifold Markets
excerpt: An analysis of when prediction market prices are miscalibrated, using resolved binary markets from Manifold Markets and machine learning subgroup analysis.
technologies:
  - Python
  - Machine Learning
  - Prediction Markets
  - Calibration
  - Random Forest
  - Data Analysis
---

## Paper

📄 **[Download Full Paper (PDF)](/assets/papers/ml-paper.pdf)**

<div style="width: 100%; height: 900px; margin: 30px 0; border: 2px solid #ddd; background: #f5f5f5; padding: 10px; border-radius: 4px; overflow: hidden;">
  <embed
    src="/assets/papers/ml-paper.pdf#toolbar=1&navpanes=1&scrollbar=1"
    type="application/pdf"
    width="100%"
    height="100%"
    style="border: none;">
  <iframe
    src="https://docs.google.com/viewer?url=https://itsjinuhyun.github.io/assets/papers/ml-paper.pdf&embedded=true"
    width="100%"
    height="100%"
    style="border: none; background: white; display: block;"
    frameborder="0">
    <p style="padding: 20px; text-align: center;">
      Your browser does not support embedded PDFs. <a href="/assets/papers/ml-paper.pdf" target="_blank">Click here to download or view the PDF</a>.
    </p>
  </iframe>
</div>

## Overview

This paper studies calibration in Manifold Markets, a large play-money prediction market platform with public bulk data. The goal is to understand when crowd prices are reliable probability estimates and when structural market conditions create systematic miscalibration.

The analysis constructs a dataset of 25,275 resolved binary markets observed seven days before resolution, then trains five machine learning models against the raw market price. No model beats the crowd globally, which supports the view that the crowd is well-calibrated on average. Subgroup analysis, however, identifies market conditions where the crowd is miscalibrated.

## Key Findings

- Machine learning did not outperform the crowd globally, indicating strong average calibration across the platform.
- Subgroup analysis revealed structural cases where raw market prices were less reliable.
- In crypto-speculation markets, a Random Forest model improved on the crowd by exploiting systematic directional underpricing.
