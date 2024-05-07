---
title: Reviewing and adjusting the `wacz_page_extractor` and `webpage` Post-Processor operations
tags:
  - Strawberry Runners
  - Fulltext Search
  - WACZ
  - Webpage
  - Post-processing
  - Background Processing
---

# Reviewing and adjusting the `wacz_page_extractor` and `webpage` Post-Processor operations

As stated on the [Strawberry Runners overview page](docs/strawberryrunners.md), the `wacz_page_extractor` action uses the 'Post processor that extracts/generates Indexed Page Content from WACZ files in an ADO' plugin. Nested one level in, the `webpage` action uses the 'Post processor that Indexes WACZ Frictionless data Search Index to Search API' plugin. The webpage operations will be executed after the completion of the wacz_page_extractor operations. The `ocr` operations will be executed after the completion of the `pager` operations.

## Wacz Page Extractor Settings

To review or adjust the configurations for the `wacz_page_extractor` operation, select `Edit` from the `Operations` menu.

In the `wacz_page_extractor` settings, you will see the following configuration options:

1. Label: 
    - Label for this Processor; which should be a unique machine-readable name
    - Can only contain lowercase letters, numbers, and underscores
    - We do not recommend changing this Label from the default `pager`. 

2. Strawberry Runner Post Processor Plugin:
    - The `Post processor that extracts/generates Indexed Page Content from WACZ in an ADO` should be selected.
    - We do not recommend changing this Plugin selection.

3. Checkbox to mark this processor plugin as active 
    - We recommend keeping this checked as `active` at all times, but you may wish to temporarily disable this if you are performing certain types of administrative review tasks such as running large test ingests where you plan on deleting the ADOs before a final ingest.
    - If you accidentally uncheck this and need to re-trigger the `pager` (and corresponding nested `ocr` action), you can use Archipelago's [Find and Replace](find_and_replace.md) to first select a specific group of Digital Objects you wish to target for Post-Processing, then select the `Trigger Strawberrry Runners process/reprocess for Archipelago Digital Objects content item` from the [Find and Replace](find_and_replace.md) `Actions menu`.

4. The JSON key that contains the desired source files:
    - By default, only the `as:document` key is selected.
    - We do not recommend changing this selection.

5. Mimetypes(s) to limit this Processor to:
    - A single Mimetype type or a comma separated list of mimetypes that qualify to be Processed.
    - Default mimetype for this Processor is: 'application/vnd.datapackage+zip'	  

6. ADO type(s) to limit this processor to:
    - A single ADO type or a comma delimited list of ado types that qualify to be Processed.
    - Leave empty to apply to all ADOs. If you do not provide any specific ADO types here, the processor will be applied for all ADOs with the JSON keys selected in the next step.
    - Default ADO type specified for this Processor: 'WebPage'	

7. The expected and desired output of this processor:
    - Only option for this Processor is JSON output
    - Default is set to: `Data/Values that can be serialized to JSON`

8. The queue to use this processor:
    - The primary queue will be execute in realtime while the Secondary will be execute in background
    - Default selection is for the 'Secondary queue in background' 

9. Timeout in seconds for this process.
    - Default is set to: 300
    - If the process runs out of time it can still be processed again.

10. Order or execution in the global chain.
    - Default is set to: 7

## Webpage  Settings

To review or adjust the configurations for the `webpage` operation, select `Edit` from the `Operations` menu.

In the `webpage` settings, you will see the following configuration options:

1. Label: 
    - Label for this Processor; which should be a unique machine-readable name
    - Can only contain lowercase letters, numbers, and underscores
    - We do not recommend changing this Label from the default `webpage`. 

2. Strawberry Runner Post Processor Plugin:
    - The `Post processor that extracts/generates Indexed Page Content from WACZ in an ADO` should be selected.
    - We do not recommend changing this Plugin selection.

3. Checkbox to mark this processor plugin as active 
    - We recommend keeping this checked as `active` at all times, but you may wish to temporarily disable this if you are performing certain types of administrative review tasks such as running large test ingests where you plan on deleting the ADOs before a final ingest.
    - If you accidentally uncheck this and need to re-trigger the `pager` (and corresponding nested `ocr` action), you can use Archipelago's [Find and Replace](find_and_replace.md) to first select a specific group of Digital Objects you wish to target for Post-Processing, then select the `Trigger Strawberrry Runners process/reprocess for Archipelago Digital Objects content item` from the [Find and Replace](find_and_replace.md) `Actions menu`.

4. ADO type(s) to limit this processor to:
    - A single ADO type or a comma delimited list of ado types that qualify to be Processed.
    - Leave empty to apply to all ADOs. If you do not provide any specific ADO types here, the processor will be applied for all ADOs with the JSON keys selected in the next step.
    - Default ADO type specified for this Processor: 'WebPage'

5. The expected and desired output of this processor:
    - Only option for this Processor is JSON output
    - If the output is just data and "One or more Files" is selected all data will be dumped into a file and handled as such.
    - Default is set to: `Data/Values that can be serialized to JSON`

6. Where and how the output will be used.
    - 'As Input for another processor Plugin' selection will only have an effect if another Processor is setup to consume this ouput.
    - Default selection is: 'In a Search API Document using the Strawberryfield Flavor Data Source (e.g used for HOCR highlight)'

7. The queue to use this processor:
    - The primary queue will be execute in realtime while the Secondary will be execute in background
    - Default selection is for the 'Secondary queue in background' The queue to use for this processor.

8. Checkbox option to 'Use NLP to extract entities from Text'
    - If checked Full text will be processed for Natural language Entity extraction using Polyglot
    - Default is have this option checked
    
9. The URL location of your NLP64 server.
    - Defaults to http://esmero-nlp:6400

10. Which method(NER) to use
    - The NER NLP method to use to extract Agents, Places and Sentiment.
    - Default selection: 'Polyglot (faster)'
    - Alternation selection: 'spaCy (more accurate)'

11. Timeout in seconds for this process.
    - Default is set to: 25
    - If the process runs out of time it can still be processed again.

12. Order or execution in the global chain.
    - Default is set to: 0

___

Return to the main [Strawberry Runners](strawberryrunners.md) or the [Archipelago Documentation main page](index.md).
