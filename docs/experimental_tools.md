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

## What's under the hood powering these tools?

- New Archipelago ML supporting code (lots of maths!)
- New Strawberry Runners Post-Processing pipelines for Image and Text Vectorization
- New Solr Fields for storing the vectorized image or text fragments
- New Views and contextual and exposed filters for enabling search and results from Solr
- New UI Interfaces for interacting with these tools
- New `nlp` Docker Container (`esmero/esmero-nlp:1.1.1-multiarch`)

All of the above items, except for the new `nlp` Docker Container, are available out-of-the-box in default local deployment configurations. However, to use the tools you will first need to switch the `nlp` Docker Container being used from the default `esmero/esmero-nlp:fasttext-multiarch` to the `esmero/esmero-nlp:1.1.1-multiarch` in your `docker-compose.yml` file.

### Enabling the new `nlp` container & triggering related post-processing

1. In your `archipelago-deployment` folder, navigate to and open the `docker-compose.yml` file.
2. Comment out (add # at the beggining of) line 76 for `image: "esmero/esmero-nlp:fasttext-multiarch"`
3. Remove the comment (# at the begnning) for line 78 for `image: "esmero/esmero-nlp:1.4.1-arm64"` OR `"esmero/esmero-nlp:1.4.1-amd64"` (depending on your particular computer platform CPU type).
4. Save your changes.
5. In your terminal, while still in your `archipelago-deployment` folder, run:
```shell
docker-compose pull
```

6. After the new `nlp` container finishes pulling and downloading, run:
```shell
docker-compose up
docker ps
```
Then check that the new `nlp`container now shows `esmero/esmero-nlp:1.1.1-multiarch` under IMAGE.

7. While logged in as an adminsitrator, use [Archipelago's Advanced Batch Find and Replace](find_and_replace.md#available-actions) to select all of the objects ingested into your repository and run the `Trigger Strawberrry Runners process/reprocess for Archipelago Digital Objects content item` Action. If you have not yet ingested any objects into your Archipelago, you can skip this and proceed to the next step. 
8. Wait for the [Background Post-processing Queues](http://localhost:8001/admin/config/system/queue-ui) and your [Solr Index](http://localhost:8001/admin/config/search/search-api/index/default_solr_index) to finish updating after you have either run the AMI Demo Set and/or ingested objects through the webforms/your own AMI set(s). Feel free to give the [Cron](http://localhost:8001/admin/config/system/cron) a push to start ahead of the default 3 hour schedule. Please be patient with this post-processing and indexing process, as the ML parts of this process (image and text extraction, vector generation) can take some time to complete.

When all of the above steps are completed, you will be ready to start using the following experimental tools.

### Experimental Image ML comparison (integrated)

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

### Experimental Text Similarity Search 

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
