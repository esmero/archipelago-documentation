# Ingesting New Digital Objects and Collections using Spreadsheets or Google Sheets

Ingesting Only Digital Objects or Both Digital Objects and Collections uses similar processes, with a few key differences. [Click here](/docs/AMIviaSpreadsheets.md#ingesting-both-new-digital-objects-and-collections-in-the-same-spreadsheet) to jump to the [Ingesting Both New Digital Objects and Collections section](/docs/AMIviaSpreadsheets.md#ingesting-both-new-digital-objects-and-collections-in-the-same-spreadsheet) of this guide page.

## Ingesting _Only_ New Digital Objects

From either the main Content page or the AMI Sets List page, select the 'Start an AMI set' button to begin.

#### Step 1: Plugin Selection
Select the Plugin type you will be using from the dropdown menu.
  - Google Sheets Importer
  - Spreadsheet Importer  (if using local CSV file)

    ![Plugin Importer Select](/imgs/ami/PluginImporterSelect.jpg)

_*The `Remote JSON API Importer` and additional remote import source options (for other repository systems) will be covered in separate tutorials following future releases._

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

#### Step 5: ZIP upload

Provide an optional ZIP file containing your assets.
  - You may choose to upload a ZIP file containing all or some of the corresponding files specified in your csv/spreadsheet.
  - The file upload size restrictions specified in your Archipelago instance will apply here (512MB maximum by default). 
 
 ![AMI Step 5 Zip](/imgs/ami/AMIstep5zip.jpg) 
  
  - Please note, when creating your ZIP file (in particular, within an OSX environment): only select the folders and files needed, not the top/enclosing folder they are in. 
   
<details><summary>Click to view screenshot of example ZIP file creation in OSX</summary>
<span>
	
 ![Example ZIP OSX](/imgs/ami/ExampleZipOSX.jpg)
	
</span>
</details>  

#### Step 6: AMI Set Confirmation

You will now see a message letting you know that 'Your source data was saved and is available as a CSV at `linktotheAMIgenerated.csv`

The message will also let you know that your New AMI Set was created and provide a link to the AMI Set page.

  ![AMI Step 6](/imgs/ami/AMIStep6.jpg)

#### Step 7: AMI Set Processing

Your newly created AMI Set will now need to be Processed.

If you clicked on the 'see it here' link in Step 6, you will be brought to the AMI Set page for review. From this page you can review the JSON configuration for your set (determined by your selections in the preceeding steps).

  ![AMI Set Admin Review](/imgs/ami/AMIsetAdminReview.jpg)

To Process this set, navigate to the `Process` tab. You will have two options related to the Processing outcome for your AMI Set. 
 - Option 1: Select the "Desired ADOS Statuses After Process" for both Digital Objects (and Digital Object Collections, if also part of your ingest).
	- The Statuses you have available will reflect the publication workflow/moderation states (such as Draft, Published, Archived/Unpublished) setup in your Archipelago instance, and the permissions associated your user account.	
 - Option 2: You may choose to check the option to 'Enqueue but do not process Batch' to place the AMI set in the Queue to Process. *For the AMI version shipped with Archipelago 1.0.0-RC2, the option to 'Enqueue' for scheduled/future Processing should only be used if the Queue operations for your Archipelago instance are configured. Please return to this page for updated Enqueueing instructions that will accompany future releases.*
 - Select `Confirm` to continue. 
	
  ![AMI Admin Set Process](/imgs/ami/AMIsetAdminProcessRC2.jpg)

You may also select `Process` from the `Operations` menu for the AMI set from the main `AMI sets` page.

  ![AMI Sets Admin Process Operations Menu](/imgs/ami/AMISetAdminProcessOpsMenu.jpg)
  
If you chose to 'Confirm" and Process your AMI Set immediately, proceed to Step 9: Processing and ADO Creation.   

#### Step 8: Queue Manager (if not Processing immediately)

If you chose to place your AMI set in the Queue to Process in step 7, you may need to navigate to the Queue Manager found at `/admin/config/system/queue-ui`. (Be sure to select the `Queue Manager` under the System section, not the `Queue Manager for Hydroponic Service` under the Archipelago section). If the Queue operations for your Archipelago instance have been configured, you can simply leave your AMI Set in the Queue for Processing on the preconfigured schedule.

  ![AMI Queue Manager](/imgs/ami/AMIqueueMgr.jpg)

To Process your AMI Set immediately from the Queue Manager page, select the checkbox next to the 'AMI Digital Object Ingester Queue Worker'. Keep the `Action` menu set to `Batch Process` and click the `Apply to selected items` button.

  ![AMI Queue Manager Batch](/imgs/ami/AMIqueueMgrBatchProcess.jpg)

#### Step 9: Processing and ADO Creation

Your AMI set will now be Processed. You can follow the set's progress through the `Processing queues` loading screen.

  ![AMI Processing Queues](/imgs/ami/AMIprocessingQueue.jpg)

After your AMI set is Processed, you will receive confirmation messages letting you know your Digital Objects were successfully created. 

  ![AMI Set Succes](/imgs/ami/AMIsetSuccessRC2.jpg)
  
From this message, you can click on each ADO title to review the new created Digital Object (or Collection) if you wish. Or, you may proceed to step 10.  

#### Step 10: Review your newly created Digital Objects

Return to the main Content page found at `/admin/content` and review your newly created Digital Objects. After ensuring that files and metadata elements were mapped correctly, you may choose to change the Status for your Digital Objects to 'Published'. Celebrate your AMI success with a fresh coffee, tea, or cookie!


## Ingesting _Both_ New Digital Objects and Collections (in the same spreadsheet)

From either the main Content page or the AMI Sets List page, select the 'Start an AMI set' button to begin.

#### Steps 1: Plugin Selection & Step 2: Operation and Spreadsheet Source Selection
Follow the same instructions found above for Ingesting New Digital Objects.

#### Step 3: Data Transformation Selections
To import Digital Objects and Digital Object Collections at the same time/from same spreadsheet source, you will need to select the `Custom (Expert Mode)` option for your data transformation approach.
  - Custom (Expert Mode)
    - Provides very granular custom data transformation and mapping options

You will then need to 'Select your Custom Data Transformation and Mapping Options' for each of your Digital Object types.

- For Collection objects:
 - Select either the Direct or Template (and corresponding JSON template) option for your data transformation approach.
 - Select the destination Fields and Bundles for `Strawberry (Descriptive Metadata source) for Digital Object Collection`
 - You may also wish to Select which columns contain filenames, entities or URLS where files can be fetched from. For most Collection objects, you will either leave unselected or choose `images` if you are uploading a thumbnail image for your Collection.

![AMI Custom Mode Collections](/imgs/ami/AMIcustomModeCollections.jpg)

- For **each** Digital Object type in your spreadsheet source:
  - You will also need to select either the Direct or Template (and corresponding JSON template) option for your data transformation approach.
  - Then select which columns contain filenames, entities or URLS where files can be fetched from. Select what columns correspond to the Digital Object types found in your spreadsheet source.
  - Then Select the destination Fields and Bundles for `Strawberry (Descriptive Metadata source) for Digital Object`
  - For example, for Book type Digital Objects, you would select the following options (as depicted in this screenshot):

  ![AMI Custom Digital Object Mappings](/imgs/ami/AMIcustomDigitalObjectMappings.jpg)  

- If using Sheet 2 of the Demo AMI Ingest set (found above):
   - Follow the instructions for mapping the Digital Object Collection (found in row 2), using the Template Data transformation approach with the AMI 1.0.0-RC1 JSON Template. Also select the destination Fields and Bundles for `Strawberry (Descriptive Metadata source) for Digital Object Collection`. Leave the file source fetching unselected.
   - For the Book, Photograph, and Conversation (Oral History) Digital Object types, choose the same the Template Data transformation approach with the AMI 1.0.0-RC1 JSON Template.
   - For Book object types, select `images` and `documents` for file source fetching.
   - For Photograph object types, select `images` for file source fetching.
   - For Conversation (Oral History) object types, select `images`, `documents`, and `audio` for file source fetching.

#### Step 4: Global ADO Mappings

Select your global ADO mappings.
  - Even if empty (no values), select `node_uuid`.
  - In order to make sure that Digital Objects containing the corresponding UUID or spreadsheet row number for any corresponding Collections, make sure `ismemberof` is also selected in the ADO Parent Columns.
  - By default, the option to automatically assigns UUIDs is selected. If you have existing UUIds, unselect this option.
  - Select the corresponding Columns for the Required ADO mappings.
  - If using Sheet 2 of the Demo AMI Ingest set (found above):
    - Select both `ismemberof` and `node_uuid` for ADO Parent columns
    - Keep 'Automatically assign UUID' checked
    - Do not select any column for 'Sequence'
    - Select the `label` column for ADO Label

    ![AMI Step 4 Global ADO Mappings](/imgs/ami/AMIstep4GlobalADOmappings.jpg)

#### Step 5-10:

Follow the same instructions found in Steps 5-10 above. As part of step 10, make sure your Digital Objects were ingested into the corresponding Collections you mapped them to in your spreadsheet source. Please note, you will need to Publish the Digital Objects before the Objects will appear in the Collection's View page (whether accessed as a logged-in Admin user or Anonymous/Public user). Celebrate your next AMI success with another fresh coffee, tea, or cookie!

---

Thank you for reading! Please contact us on our [Archipelago Commons Google Group](https://groups.google.com/forum/#!forum/archipelago-commons) with any questions or feedback.

Return to the [Archipelago Documentation main page](../README.md).

