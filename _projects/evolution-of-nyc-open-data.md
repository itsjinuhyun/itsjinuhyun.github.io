---
layout: project
title: The Evolution of NYC Open Data
excerpt: A longitudinal study revisiting NYC Open Data a decade after a landmark 2014 survey, measuring changes in scale, quality, schema heterogeneity, and joinability across the portal.
technologies:
  - Python
  - Socrata SODA API
  - Data Integration
  - MinHash
  - Vector Embeddings
  - Knowledge Graphs
---

## Paper

📄 **[Download Full Paper (PDF)](/assets/papers/de-paper.pdf)**

<div style="width: 100%; height: 900px; margin: 30px 0; border: 2px solid #ddd; background: #f5f5f5; padding: 10px; border-radius: 4px; overflow: hidden;">
  <embed
    src="/assets/papers/de-paper.pdf#toolbar=1&navpanes=1&scrollbar=1"
    type="application/pdf"
    width="100%"
    height="100%"
    style="border: none;">
  <iframe
    src="https://docs.google.com/viewer?url=https://itsjinuhyun.github.io/assets/papers/de-paper.pdf&embedded=true"
    width="100%"
    height="100%"
    style="border: none; background: white; display: block;"
    frameborder="0">
    <p style="padding: 20px; text-align: center;">
      Your browser does not support embedded PDFs. <a href="/assets/papers/de-paper.pdf" target="_blank">Click here to download or view the PDF</a>.
    </p>
  </iframe>
</div>

## Overview

This paper replicates and modernizes the 2014 landscape survey of NYC Open Data by Barbosa et al., revisiting the same portal a decade later to measure how the ecosystem has evolved in scale, quality, and integration potential.

Using the Socrata SODA API, we collected metadata and sample values from 2,391 datasets and compared the results against the 2014 baseline. The study finds that dataset volume remained stable, while category composition shifted substantially toward City Government and Education. Data quality improved through a higher proportion of tabular datasets and fewer never-modified datasets, but schema heterogeneity increased, making exact name-level matching less effective.

## Key Findings

- Lazo, a MinHash-based joinability method, achieved the best performance on human-labeled column pairs with F1 0.90, precision 0.92, and recall 0.90.
- Word-level Jaccard and vector embedding cosine similarity performed similarly, suggesting that value-domain overlap was more discriminative than column-name similarity.
- The joinability relationships were synthesized into a knowledge graph with 2,150 nodes and 182,098 edges, identifying key hub datasets in the NYC Open Data ecosystem.
