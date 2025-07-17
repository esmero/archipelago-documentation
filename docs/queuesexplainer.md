---
title: Archipelago Processing Queues Explainer
tags:
  - AMI
  - Archipelago Multi Importer
  - Queues
  - Strawberry Runners
  - HOCR
  - OCR
  - Post-processing
  - Background Processing
---

# Archipelago Processing Queues Explainer

Archipelago has multiple helpful background queues to help keep your repository workflows running smoothly. These different queues will be triggered based on different workflow events, such as one-off create/update/delete actions for digital objects, or batch ingests or updates through AMI or Find and Replace operations. 

## Primary Queue Manager

The primary queue manager handles the main Archipelago batch ingest and update operations, processed in a first in, first out (FIFO) basis.

You can access the primary Queue Manager:

- Through the `Manage` menu > `Configuration` > `System` > `Queue Manager`
- Directly at `/admin/config/system/queue-ui` 

![AMI Queue Manager Batch](images/AMIqueueMgrBatchProcess_updated_2025_July.png)

### Queue Actions & Inspect Button

Using/forcing any of the Actions on the Queue Manager will run in realtime, over your browser window. Only use the Actions if you have a stable internet connection and time to observe the resulting Actions.

#### Batch process
- This Action will start the Queue Operations for the selected items
- May be useful to [push along AMI ingests](AMIviaSpreadsheets.md#step-8-queue-manager-push-may-be-restricted-to-administrator-users-only) 

#### Remove Leases
- This Action will remove the 'leases'--aka holds tagged for particular digital objects/operations related to a Queue Operations
- May be useful to stop a queue'd process and release the impacted Digital Objects for a different or refreshed operations

#### Clear
- This Action will clear all of the Queue Operations for the selected items
- Recommendation is to always first use the 'Remove Leases' Queue Action, then use 'Clear' only if needed. This order of Actions will help ensure no orphan operations are left behind if you interrupt and reset Queues operations.

#### Inspect Button
- Found on the right-hand side of the Queue Operations table
- Allows you to review the individual processes enqueued, such as the particulars for the 'AMI CSV Expander and ADO Enqueuer Queue Worker' or the specific file being passed through the [Strawberry Runners Post Processing pipeline for HOCR extraction](strawberryrunners_pager_ocr.md). 

### Queue Workers

Every Queue Worker refers to a specific machine process, and has settings for when it executes based on your site's daily Cron runs.

#### Aggregator refresh
- Machine name: aggregator_feeds
- Cron time limit: 60 seconds
- Function: Drupal related operation
- Typically not used for Archipelago repository workflows

#### AMI LoD Reconciling Queue Worker
- Machine name: ami_lod_ado
- Cron disabled
- Function: processes AMI LoD Reconciliation one-by-one
- Can be useful for very large AMI Sets with hundreds of terms to be processed through LoD queries

#### AMI Digital Object Ingester & Action Queue Worker
- Machine name: ami_ingest_ado
- Cron disabled
- Function: processes the ingest of digital objects and collections enqueued in [AMI Set Processing](AMIviaSpreadsheets.md#step-7-ami-set-processing)

#### AMI CSV Expander and ADO Enqueuer Queue Worker
- Machine name: ami_csv_ado
- Cron disabled
- Function: expands an AMI Set CSV and assigns the individual digital object and collection rows as Queue items for the `AMI Digital Object Ingest & Action Queue Worker`

#### Thumbnail downloader
- Machine name: media_entity_thumbnail
- Cron time limit: 60 seconds
- Function: Drupal related operation
- Typically not used for Archipelago repository workflows

#### Strawberry Runners Process Webhook Payload Queue Worker
- Machine name: strawberryrunners_process_webhook_payload
- Cron time limit: 5 seconds
- Function: prepares the individual Archipelago digital object or collection files to pass through to the `Strawberry Runners Process on Background Queue Worker`

#### Strawberry Runners Process on Background Queue Worker
- Machine name: strawberryrunners_process_background
- Cron disabled
- Function: processes the JSON data for files sent through the `Strawberry Runners Process Webhook Payload Queue Worker`, assigns the correct ['Strawberry Runners' post-processor operation](strawberryrunners.md), such as OCR extraction, to each individual file, then passes to the `Strawberry Runners Process via Cron Queue Worker`

#### Strawberry Runners Process via Cron Queue Worker
- Machine name: strawberryrunners_process_index
- Cron time limit: 180 seconds
- Function: processes the complete ['Strawberry Runners' post-processor operation](strawberryrunners.md), such as HOCR extraction --> output mapped to a 'Strawberryflavour' field in Solr for the corresponding digital object

#### Archipelago Temporary File Composter Queue Worker
- Machine name: sbf_compost_file
- Cron disabled
- Function: processes the deletion of temporary (unnecessary copy) files

## Secondary Background / Hydroponics Queue 

You can access the secondary background / Hydroponics Service Queue Manager:

- Through the `Manage` menu > `Configuration` > `Archipelago` > `Queue Manager for Hydroponic Service`
- Directly at `/admin/config/archipelago/hydroponics` 




#### Advanced Settings

___

Return to the [Archipelago Documentation main page](index.md).
