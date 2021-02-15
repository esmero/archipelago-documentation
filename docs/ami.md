# Archipelago Multi-Importer

[Archipelago Multi-Importer (AMI)](https://github.com/esmero/ami) is a module for batch/bulk/mass ingests of Archipelago digital objects and collections. AMI also enables you to perform batch administrative actions, such as updating, patching/revising, or deleting digital objects and collections.

### *Work-in-Progress Notes*

*Please be aware that the version of AMI shipped with Archipelago 1.0.0-RC1 provides much of the core batch functions useful for getting started working with large amounts of content, but is not the full or final version of this module. This documentation page will be updated with successive AMI and Archipelago releases.*

## Getting started with AMI

You can access AMI through the `AMI Sets` tab on the main Content page found at `/admin/content`

  ![Content AMI Sets](/imgs/ami/ContentAMIsets.jpg)

or directly at `/amiset/list`

  ![AMI Sets List](/imgs/ami/AMIsetsList.jpg)

### Google Sheets API Congifuration

If you plan on using the Google Sheets Importer option, you will need to:
  * [Configure the Google Sheets API](/docs/googleapi.md)

## Spreadsheet Formatting Overview

There are multiple ways a spreadsheet/CSV file can be structured to work with AMI, depending on the data transformation and mapping you will be using.

<details><summary>Click to view Basic Requirements and Recommended Options</summary>
<span>

- For most standard AMI ingests, each Row of your spreadsheet/CSV will correspond to a single Digital Object or Collection.
- Columns in your spreadsheet/CSV can be mapped to different data (files) and metadata elements (label, description, subjects, etc.).

- It is recommended that different types of files are placed into separate columns--"images", "documents", "models", "videos", "audios".
  - Filepaths can point to remote files, to existing files within your docker container, and s3 (or other storage type/location that is accessible to Archipelago).
      - Example path for existing file within docker container:
      `/var/www/html/d8content/myAMIimage.jpg`
      - Example s3 path:
      `s3://myAMIuploads/myAMIdocument.pdf`
      - Example remote filepath:
      `https://dogsaregreat.edu/dogs.tiff`
  - **Multiple files can be placed in a single cell, separated by a semicolon ( ; ).** You can also place multiple files into separate columns (of the same filetype), but this may result in a large and cumbersome spreadsheet.
  - For Digital Objects comprised of multiple types of files, such as an Oral History Interview with an audio file and a PDF transcript file, you can place different file types within different corresponding columns for the same Row.
  - It is recommended that filepaths are copied/stored as plain (non hyperlinked) formatted text.

- **Every spreadsheet/CSV file should contain the following Columns:**
  - `node_uuid`
    - this can be empty
    - if empty, Archipelago will automatically generate UUIDs
    - can be used with existing UUIDs during migrations
  - `type`
    - the Digital Object or Digital Object Collection Type, such as 'Photograph' or 'Collection'
  - `label`
    - the title of the Digital Object or Collection

- **Recommended Columns:**
  - Files as defined above
    - `images`, `audios`, `documents`, etc.
  - `ismemberof` and/or `ispartof` (and/or whatever predicate corresponds with the relationship you are mapping)
    - these columns can be used to connect related objects using the object-to-object relationship that matches your needs
    - these columns can hold 3 types of values
      - empty (no value)
      - an integer to connect an object to another object's corresponding row in the same spreadsheet/CSV
        * Ex: Row 2 corresponds to a Digital Object Collection; for a Digital Object corresponding to Row 3, the 'ismemberof' column contains a value of '2'. The Digital Object in Row 3 would be ingested as a member of the Digital Object Collection in Row 2.
      - a UUID to connect with an already ingested object
  - Metadata - for all the rich, detailed information associated with your Digital Objects and Collections
    - Every Column header will become a JSON Key and each cell a JSON value for that Key
    - You can use direct JSON snippets such as:
    ````
    [{"uri": "http://id.loc.gov/authorities/subjects/sh95008857","label": "Digital libraries"}]
    ````

    - If you have an advanced twig template with the necessary logic, you can place data in cells that can be parsed and structured in various ways (such as multiple values separated by semicolons split accordingly, capitalization of values based on defined patterns, etc.)

    </span>
    </details>

### Example Spreadsheet/CSV    

This spreadsheet can be used to import a small set of Digital Objects using the same assets part of the [Two-Step Demo content ingest guide](https://github.com/esmero/archipelago-deployment/blob/1.0.0-RC1/docs/democontent.md) (These objects can also be found in the [Archipelago Deployment repository](https://github.com/esmero/archipelago-deployment)).

* https://docs.google.com/spreadsheets/d/1PEhWPXrVDVvt8T1qFBbW71EyobOKT7-7eDFUwfyyxHQ/edit?usp=sharing
* To use this spreadsheet, you can either download as a CSV file to use with the Spreadsheet Importer, or make a copy of this using your [configured Google Sheets API account](/docs/googleapi.md).

### Example JSON template

This JSON template can be used during the Data Transformation (step 3) of your AMI Import. This particular template corresponds with the metadata elements found in the Default Descriptive Metadata and Default Digital Object Collection webforms shipped with Archipelago 1.0.0-RC1.

  <details><summary>Click to view the example 1.0.0-RC1 AMI JSON template</summary>
  <span>

To use this template, copy and paste the JSON below directly into a new Metadata Display, found here for a local`http://localhost:8001/metadatadisplay/list` or `http://yoursite.org/metadatadisplay/list`. Select `JSON` as the 'Primary mime type this Twig Template entity will generate as output' for this new Metadata Display.

```json_encode
  {
      "type": {{ data.type|json_encode|raw }},
      "label": {{ data.label|json_encode|raw }},
      "issue_number": {{ data.issue_number|json_encode|raw }},
      "interviewee": {{ data.interviewee|json_encode|raw }},
      "interviewer": {{ data.interviewer|json_encode|raw }},
      "duration": {{ data.duration|json_encode|raw }},
      "website_url": {{ data.website_url|json_encode|raw }},
      "description": {{ data.description|json_encode|raw }},
      "date_created": {{ data.date_created|json_encode|raw }},
      "creator": {{ data.creator|json_encode|raw }},
      "creator_lod": {{ data.creator_lod|json_encode|raw }},
      "publisher": {{ data.publisher|json_encode|raw }},
      "language": {{ data.language|json_encode|raw }},
      "ismemberof": [],
      "owner": {{ data.owner|json_encode|raw }},
      "local_identifier": {{ local_identifier|json_encode|raw }},
      "date_published": {{ data.date_published|json_encode|raw }},
      "rights_statements": {{ data.rights_statements|json_encode|raw }},
      "rights": {{ data.rights|json_encode|raw }},
      "subject_loc": {{ data.subject_loc|json_encode|raw }},
      "subject_lcnaf_personal_names": {{ data.subject_lcnaf_personal_names|json_encode|raw }},
      "subject_lcnaf_corporate_names": {{ data.subject_lcnaf_corporate_names|json_encode|raw }},
      "subject_lcnaf_geographic_names": {{ data.subject_lcnaf_geographic_names|json_encode|raw }},
      "subject_lcgft_terms": {{ data.subject_lcgft_terms|json_encode|raw }},
      "subject_wikidata": {{ data.subject_wikidata|json_encode|raw }},
      "edm_agent": {{ data.edm_agent|json_encode|raw }},
      "term_aat_getty": {{ data.term_aat_getty|json_encode|raw }},
      "geographic_location": {{ data.geographic_location|json_encode|raw }},
      "subjects_local_personal_names": {{ data.subjects_local_personal_names|json_encode|raw }},
      "subjects_local": {{ data.subjects_locals|json_encode|raw }},
      "audios": [],
      "images": [],
      "models": [],
      "videos": [],
      "documents": [],
      "as:generator": {
          "type": "Create",
          "actor": {
              "url": {{ setURL|json_encode|raw }},
              "name": "ami",
              "type": "Service"
          },
          "endTime": "{{"now"|date("c")}}",
          "summary": "Generator",
          "@context": "https:\/\/www.w3.org\/ns\/activitystreams"
      },
      "upload_associated_warcs": []
  }
```

</span>
</details>  

## Ingesting New Digital Objects using Spreadsheets or Google Sheets

<details><summary>Click to view Instructions</summary>
<span>

From either the main Content page or the AMI Sets List page, select the 'Start an AMI set' button to begin.

#### Step 1: Plugin Selection
Select the Plugin type you will be using from the dropdown menu.
  - Google Sheets Importer
  - Spreadsheet Importer  (if using local CSV file)

    ![Plugin Importer Select](/imgs/ami/PluginImporterSelect.jpg)

_*The `Remote JSON API Importer` option will be covered in a separate tutorial._

#### Step 2: Operation and Spreadsheet Source Selection
Select 'Create New ADOs' as the Operation you would like to perform.

- If using Google Sheets Importer:
  - Enter the ID of your Google Sheet
  - Enter the Cell Range for your Google Sheet

    ![Google Sheets Importer](/imgs/ami/AMIstep2googlesheets.jpg)

- If using Spreadsheet Importer:
  - Select 'Choose File' to upload the CSV you will be using.

    ![Google Sheets Importer](/imgs/ami/AMIstep2spreadsheet.jpg)  

#### Step 3: Data Transformation Selections
Select the data transformation approach--how your source data will be transformed into ADO (Archipelago Digital Object) Metadata.

- You will have 3 options for your data transformation approach:
  1. Direct
    - Columns from your spreadsheet source will be cast directly to ADO metadata (JSON), without transformation/further processing (only intended for use with simple data strings).
  2. Custom (Expert Mode)
    - Provides very granular custom data transformation and mapping options
    - Needs to be used if importing Digital Objects and Digital Object Collections at the same time/from same spreadsheet source (see separate instructions below).
  3. Template
    - Columns from your spreadsheet source will be cast to ADO metadata (JSON) using a Twig template setup for JSON output.

- You will also need to Select which columns contain filenames, entities or URLS where files can be fetched from. Select what columns correspond to the Digital Object types found in your spreadsheet source.

- Lastly, for this step, you will need to select the destination Fields and Bundles for your New ADOs. If your spreadsheet source only contains Digital Objects, select `Strawberry (Descriptive Metadata source) for Digital Object`

  - If using Sheet 1 of the Demo AMI Ingest set (found above):
   - Select `Template` and use the AMI Ingest JSON template that corresponds with your metadata elements.
   - Select `images`, `documents`, and `audios` for the file source/fetching.

   ![AMI Step 3 Data Transformation Mappings](/imgs/ami/AMIstep3JSONtemplateDataTransformation.jpg)

#### Step 4: Global ADO Mappings

Select your global ADO mappings.
  - Even if empty (no values), select `node_uuid` and any relationship predicate columns (such as `ismemberof`).
  - By default, the option to automatically assigns UUIDs is selected. If you have existing UUIds, unselect this option.
  - Select the corresponding Columns for the Required ADO mappings.
  - If using Sheet 1 of the Demo AMI Ingest set (found above):
    - Select both `ismemberof` and `node_uuid` for ADO Parent columns
    - Keep 'Automatically assign UUID' checked
    - Do not select any column for 'Sequence'
    - Select the `label` column for ADO Label

    ![AMI Step 4 Global ADO Mappings](/imgs/ami/AMIstep4GlobalADOmappings.jpg)

#### Step 5: _Work-in-Progress ZIP upload_

_*Though this step to 'Provide an optional ZIP file containing your assets' is available on the AMI 1.0.0-RC1 form, this functionality is not currently available. This ZIP upload option will be part of a future release, along with a corresponding documentation update._

#### Step 6: AMI Set Confirmation

You will now see a message letting you know that 'Your source data was saved and is available as a CSV at `linktotheAMIgenerated.csv`

The message will also let you know that your New AMI Set was created and provide a link to the AMI Set page.

  ![AMI Step 6](/imgs/ami/AMIstep6.jpg)

#### Step 7: AMI Set Processing

Your newly created AMI Set will now need to Processed.

If you clicked on the 'see it here' link in Step 6, you will be brought to the AMI Set page for review. From this page you can review the JSON configuration for your set (determined by your selections in the preceeding steps).

  ![AMI Set Admin Review](/imgs/ami/AMIsetAdminReview.jpg)

To Process this set, navigate to the `Process` tab. Uncheck the option to 'Enqueue but do not process Batch' to immediately place the AMI set in the Queue to Process. Select `Confirm` to continue. *For the AMI version shipped with Archipelago 1.0.0-RC1, the option to 'Enqueue' for scheduled/future Processing should not be used, as the Queue operations are not yet configured. Please return to this page for updated Enqueueing instructions that will accompany future releases.*

  ![AMI Admin Set Process](/imgs/ami/AMIsetAdminProcess.jpg)

You may also select `Process` from the `Operations` menu for the AMI set from the main `AMI sets` page.

  ![AMI Sets Admin Process Operations Menu](/imgs/ami/AMISetAdminProcessOpsMenu.jpg)

#### Step 8: Queue Manager

After you have placed your AMI set in the Queue to Process, navigate to the Queue Manager found at `/admin/config/system/queue-ui`. (Be sure to select the `Queue Manager` under the System section, not the `Queue Manager for Hydroponic Service` under the Archipelago section).

  ![AMI Queue Manager](/imgs/ami/AMIqueueMgr.jpg)

From the Queue Manager page, select the checkbox next to the 'AMI Digital Object Ingester Queue Worker'. Keep the `Action` menu set to `Batch Process` and click the `Apply to selected items` button.

  ![AMI Queue Manager Batch](/imgs/ami/AMIqueueMgrBatchProcess.jpg)

#### Step 9: Processing and ADO Creation

Your AMI set will now be Processed. You can follow the set's progress through the `Processing queues` loading screen.

  ![AMI Processing Queues](/imgs/ami/AMIprocessingQueue.jpg)

After your AMI set is Processed, you will receive confirmation messages letting you know your Digital Objects were successfully created.

  ![AMI Set Succes](/imgs/ami/AMIsetSuccess.jpg)

#### Step 10: Review your newly created Digital Objects

Return to the main Content page found at `/admin/content` and review your newly created Digital Objects. After ensuring that files and metadata elements were mapped correctly, change the 'Status' for your Digital Objects to 'Published'. Celebrate your AMI success with a fresh coffee, tea, or cookie!

</span>
</details>  

## Ingesting New Digital Objects and Collections using Spreadsheets or Google Sheets

  <details><summary>Click to view instructions</summary>
  <span>
  <br>

  *These instructions are still Enqueued. Please stay tuned for AMI documentation updates being released early February 2021.*

  </span>
  </details>

---

Thank you for reading! Please contact us on our [Archipelago Commons Google Group](https://groups.google.com/forum/#!forum/archipelago-commons) with any questions or feedback.

Return to the [Archipelago Documentation main page](../README.md).
