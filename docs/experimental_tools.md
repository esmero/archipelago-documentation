---
title: Experimental Machine Learning (ML) Tools
tags:
  - Experimental ML Tools
  - Machine Learning
  - ML
---

# Experimental Machine Learning (ML) Tools

Archipelago 1.4.0 Local Deployment Release was shipped with a set of experimental Machine Learning (ML) Tools for testing and assessing the applicable use of such tools/workflows within a general repository environment. These tools should not be considered 'final product' integrations, and should not be exposed to end users--there are intentional configurations in place that limit exposure of these endpoints to authenticated users only.

Please review [Allison & Diego's recent 2024 Conference Presentations](presentations_events/#2024) related to this important topic in our shared field for a fuller understanding of our team's ethical perspectives and the considerations we keep related to ML tools and applications.

## What's under the hood enabling these tools?

- New Archipelago ML supporting code (lots of maths!)
- New Strawberry Runners Post-Processing pipelines for Image and Text Vectorization
- New Solr Fields for storing the vectorized image or text fragments
- New Views and contextual and exposed filters for enabling search and results from Solr
- New UI Interfaces for interacting with these tools

### 1. Experimental Image ML comparison (integrated)

As a logged in user, you can find the 'Image KNN Similarity Search' tool:

- Through the `Tools` menu > `ML Image Similarity Search`
- Directly at `/search_ml_images` 

Uses these pre-trained models:

- YOLO v8 (Structural/Layout)
- MobileNet V3 (Pattern, scale agnostic)
- InsightFace (Face feature encoding)

Also uses IIIF Content Search API Object detection of Integrated Annotations

#### How could I test this?

Conduct a search for an image in your Archipelago repository, then click on one of the images found in the results set. In the section below the top search results, you can review the output of the Image Similarity searches. You could also select a particular image Annotation to use for the Image Similarity search and comparison.

### 2. Experimental Text Similarity Search 

As a logged in user, you can find the 'Sbert KNN search on OCR-ed Pages Content' tool:

- Through the `Tools` menu > `ML Text Similarity Search`
- Directly at `/search_sbert/` 

Uses SBert/384 Vector Size embedding with assigned Task on short sentences over uncorrected OCR

#### How could I test this?

Conduct a search for a particular text fragment, written in plain language style, such as "Show me resources related to dogs". In the section below the text-based search, you can review the output of the Text Similarity searches. You will only see matches against documents that have OCR values.

## Into the future

We would like to reiterate that these are early explorations of ML tools within the Archipelago repository architecture. Expect changes, enhancements, and even removal of certain aspects of these experimental tools in future releases. 

__

Return to the [Archipelago Documentation main page](index.md).
