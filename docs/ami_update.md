---
title: Using AMI's Update Operations
tags:
  - AMI
  - Archipelago Multi Importer
  - AMI Update Sets
  - Update
  - Replace
  - Append
---

# Using AMI's Update Operations

Archipelago Multi Importer (AMI)'s Update Operations can be used to Update, Replace, or Append metadata values or files for existing Digital Objects and Collections found in your Archipelago. You can prepare and use AMI Update Sets in different ways using one of three functional operations, depending on your update needs. This guide will provide a general overview of the three main functions and how each operation may be useful.

### Important Notes: Preliminary / Pre-requisites

1. You need to have existing Digital Objects and Collections (ADOs) in your Archipelago to work with. You should have a prepared AMI Udpate Set CSV that contains at least the following columns/headers:
- **node_uuid**
    - required
    - should contain the values for any existing ADOs you wish to update
    - needs to be a 1:1 match; AMI Update functionality cannot be used to update node_uuid values
- **label**
    - required
    - contains the corresponding label (title) for your existing ADOs
    - can contain modifications for the label (title) values
- **type**
    - optional, only required if using the Custom (Expert Mode) approach for your AMI set configuration or targetting changes for 'type' mappings for your ADOs
    - contains the corresponding type values for your existing ADOs
    - can contain modifications for the type values    
- **additional metadata elements you wish to Update, Replace, or Append values for, such as "subjects"**
    - optional
    - follow recommendations and practices described in more below detail
- **files**
    - optional
    - follow recommendations and practices described in more below detail

2. You should be familiar with the basic mechanics of AMI Set Configuration noted in [Steps 1-6, here](AMIviaSpreadsheets.md#step-1-plugin-selection).

3. For all Update operations, it is strongly recommended to create a small test batch CSV referencing one to two/three ADOs to execute your desired Update actions across before running your larger Update Sets.

!!! warning "Caution with Templates for Data Transformation"

    If you are planning to use the 'Template' or 'Custom' data transformation approach during [Step 3 : Data Transformation](ami_spreadsheet_overview/#step-3-data-transformation-selections) of your AMI Update Set configuration, you will need to have prepared your corresponding AMI Ingest Template to account for the specific Update actions you have planned. 
    
    It is important to keep in mind that all of the metadata elements for your existing ADOs metadata may not necessarily be present in your AMI Update Set Source CSV. For example, you may have only prepared your AMI Update Set Source CSV to contain a limited number of headers/columns, such as only those required (node_uuid, label) and one or two metadata elements you wish to update (such as "subjects"). If you choose to pass your AMI Update Set through a Twig template, the output after Processing your AMI Update Set may overwrite your existing data if you do not have all of the necessary logic/checks in place to preserve the existing metadata if desired.
   
    In other words. Imagine your twig template contains this statement:
    ```shell
      "subjects": {{ data.subjects|json_encode|raw }}, 
    ```  
    
    Independently of IF your CSV contains "subjects" as a header/column, the Twig template will still output an empty "subjects", which, when using the "Replace" mode will wipe out any existing "subjects" in your ADO.
    
    During any update operation (independently of the functional operation chosen) and IF you are using/passing your CSV through a template, AMI will provide an extra Context key for you to reference in your Twig Template. You can always reference 'dataOriginal.subjects' -- all dataOriginal.xx keys contain the values of the existing metadata for your ADOs. This allows you to make "smart" templates that check IF a certain key/values exists, compare the unmodified (and to be modified) Digital Object/Collection with the new data passed, then generate the desired ouput. 

## Update Set Processing Options

Beginning from [Step 7, Process/ing](AMIviaSpreadsheets#step-7-ami-set-processing) of your AMI Set Configruation, select the Update operation that best corresponds to your targetted Update scenario.

![AMI Update Processing Step](images/ami_update_processing_step.png)

![AMI Update Type Options](images/ami_update_type_options.jpg)

### 1. Normal Update Operation 

The **Normal** Update Operation 'will update a complete existing ADO's configured target field with new JSON Content'. This will replace everything in an ADO with new processed data. The Normal update operation is powerful and can essentially overwrite your whole JSON object record if not paired with a template that has all the extra checks logic needed to existing data (see note of 'Caution with Templates for Data Transformation' above). 
- If the "Do not touch existing files" option is checked, then existing files will be left untouched. It is recommended to keep this option checked if you are targetting only Metadata updates.
- It is also recommended to only use the Normal Update approach if you need to re-process most of the metadata fields for ADOs.

### 2. Replace Update Operation

The **Replace** Update Operation Replace 'will replace JSON keys found in an ADO's configured target field with new JSON content. Not provided ones (fields/JSON keys) will be kept'. If the processed data contains a JSON key that is already in the ADO’s metadata to be updated, the values in the AMI Update Set CSV will be used, replacing completely the values found in the existing ADO.

- The Replace Update operation and setup I find myself using most often for metadata updates is for a scenario and setup something like this:
I notice a missing or incorrectly processed field in my original AMI Set
I create an AMI set that contains only the node_uuid + type + label (as required for all AMI sets -- type only required if using the Custom (Expert Mode) approach for your AMI set configuration) --and a column for the new field and values.
If the values are singular, no need to JSON-encode the values in the CSV
If the values are multiple, I JSON-encode as ["value1","value2"]
I use the Direct data transformation approach on the AMI configuration
I use the Replace update operation and keep 'Do not touch existing files' checked.
With this setup, the new field value(s) are added to the existing JSON for the impacted ADOs.


### 3. **Append** Update Operation

Append. If the processed data contains a key that is already in the ADO’s metadata to be updated, attempts will be made to match the “source” type (array, complex object) to add to it. So it you had 2 values in a key, and your processed data contains a single value, the result will have 3 values (and then it will try to deduplicate too). If the Source data did not contain a key present in the processed data, then it will be added.

The Append operation can be useful, but it can be a bit tricky because of the single value / array issue. (edited) 


