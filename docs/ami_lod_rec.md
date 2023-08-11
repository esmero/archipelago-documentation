---
title: Using AMI's Linked Data Reconciliation
tags:
  - AMI
  - Archipelago Multi Importer
---

# Using AMI's Linked Data Reconciliation

Archipelago Multi Importer (AMI)'s Linked Data Reconciliation tool can be used to enrich your metadata with Linked Data (LoD). Using this tool, you can map values from your topical/subject metadata elements to your preferred LoD vocabulary source. These mappings can then be transformed via a corresponding Metadata Display (Twig) template to process the values into JSON-formatted metadata for your specified AMI set.

*The aim of this tool is to automize as much of the reconciliation process as feasible within Archipelago. Please be aware that data reconciliation will still be in part a manual and potentially time intensive process.*

### Important Note: Preliminary / Pre-requisite AMI Set Configuration

In order to Reconciliate an AMI Set, you will need to have selected the 'Template' or 'Custom' data transformation approach (then also, via 'Template' for your Digital Object or Collection types) during [Step 3 : Data Transformation](AMIviaSpreadsheets.md#step-3-data-transformation-selections) of your AMI Set configuration.

Your source spreadsheet will also need to contain at least one column containing terms/names (values) you want to reconcile against an LoD Authority Source. Multiple values should be separated by '|@|'.

## Step 1: Select the AMI Set you will be working with.

From the main AMI Sets List page, click on your AMI Set's Name, or select the 'Edit' option from the Operations menu on the right-hand side of the Sets list.

![AMI Sets List](images/ami/AMIsetsList.jpg)

## Step 2: Reconcile LoD Tab

Navigate to the Reconcile LoD tab.

![Reconcile LoD Tab](images/ami/AMIreconcileLodTab.jpg)

## Step 3: LoD Reconciling Selections

From the list of columns from your spreadsheet source, select which columns you want to reconcile against LoD providers. 

Under the LoD Sources section, select how your chosen Columns will be LoD reconciled.
- LoD reconcile options will be on the left, LoD Authority Sources will be on the right.
- Example: 'local_subjects' will be mapped to 'LoC subjects (LCSH)'
	
![AMI LoD Sources](images/ami/AMI_LodSources.jpg)
	
??? info "Full list of potential LoD Authority Sources"

    - LoC subjects(LCSH)
    - LoC Name Authority File (LCNAF)
    - LoC Genre/Form Terms (LCGFT)
    - LoC Thesaurus of Graphic Materials (TGN)
    - LoC MARC List for Geographic Areas
    - LoC Relators Vocabulary (Roles)
    - LoC MADS RDF by type:
 	 - Corporate Name
 	 - Personal Name
 	 - Family Name
 	 - Topic
 	 - Genre Form
	 - Geographic
 	 - Temporal
 	 - Extraterrestrial Area
    - VIAF
    - Getty aat Fuzzy / Terms / Exact Label Match
    - Wikidata Q Items

To preview the values contained in the column(s) you selected, click the 'Inspect cleaned/split up column values' button. 

	
![AMI LoD](images/ami/AMIreconcileValuePreview.jpg)

***Tip***: *This preview step provides you with the opportunity to return to your AMI Set source CSV and make any necessary label/term corrections such as outliers and formatting errors before processing. This can be done multiple times until your source set is fully prepared. If using this workflow, you will tick the 'Re-process only adding new terms/LoD Authority Sources' processing option after replacing your updated source CSV (see screenshot below)*


When ready, there are multiple processing options to select from depending on your current need/workflow.
 - To process immediately, select 'Process LoD from Source'
 - To enqueue the batch process, select 'Enqueue but do not process Batch in real time.
 - To add new data (i.e. terms, LoD Authority Sources) to existing reconciliation (e.g after replacing 
   source CSV data), select 'Re-process only adding new terms/LoD Authority Sources

![Process LoD](images/processLoD.jpg)

*Important note: if you have previously run LoD Reconciliation for your AMI set, this action will overwrite any manually corrected LoD on your Processed CSV. Please make sure you have a backup if unsure.*

Depending on the size of your AMI Set, the Reconciliation processing may take a few minutes. 
	
![AMI LoD Reconciling Success](images/ami/AMIbatchReconcile.jpg)

When the process is finished, you will see a brief confirmation message.
	
![AMI LoD Reconciling Success](images/ami/AMI_LodRecSuccess.jpg)

## Step 4: Edit Reconciled LoD

Open the 'Edit Reconciled LoD' tab.

You will see a table (form) containing:
- Your Original term values (labels)
- The CSV Column Header/Key from the source spreadsheet where the value is found
- A Checked option you can use to denote that an LoD mapping has been reviewed/revisioned
- The Linked Data Label and URL pairing selected during the LoD reconciliation process

![AMI Edit Reconciled LoD](images/ami/AMIeditReconciledLoD.jpg)

The results table will show 10 original terms and mappings per page. You can advance through the pages using the page numbers and navigational arrows above and below the table.

## Step 5: Review and Edit your Reconciled LoD Mappings

Review the LoD reconciliation mappings, to make sure the best terms were selected for your metadata.

- To revise and select a different term mapping, begin by typing in the 'Label' box in the corresponding LoD lookup element. (You can type directly over an incorrect term or within an empty cell if no value was mapped/identified.)
- Select your preferred term and URL pairing from the list.
- You can also add or remove multiple mappings using the +/- buttons beside the LoD lookup element.
- If desired, click the Checked option to mark that the term was reviewed/revisioned.

As you advance through your review process, it is recommended that you use the 'Save Current LoD Page' at the bottom of each results page as you work. This will preserve the corrections you may have made and update the LoD Reconciled data for your AMI Set within the editing form.

![AMI Edit Reconciled LoD](images/ami/AMIreconcileSaveCurrentPageSuccess.jpg)

When you have finished editing/reviewing your data, you must select 'Save all LoD back to CSV File' or else your LoD selections will not be preserved.

![AMI Edit Reconciled LoD](images/ami/AMIreconcileSaveAllSuccess.jpg)


## Step 6: AMI Set Review and Twig (Metadata Display) Preparation

You will now need to make sure that the Metadata Display (Twig) Template you selected to use during your initial AMI Set configuration is setup to Process your LoD mapped Label and URL selections into your Digital Objects and Collections JSON metadata.

For every JSON key/element in your metadata that you need to process the LoD Reconciled data into, you need to specify in your Template that data for this element will be read from the 'Processed Data' LoD information. 

In the following example Twig snippet, the "subject_loc" JSON key will map corresponding values from the 'Processed Data' (data.lod) LoD information into a newly created Digital Object/Collection during the AMI Set Processing.

```shell
"subject_loc": {{ data_lod.mods_subject_topic.loc_subjects_thing|json_encode|raw }},
```
 
- "subject_loc" = destination JSON Key or Property for your LoD values
- data.lod = directs the Twig template to source from the Processed Data LoD information (instead of the original AMI Source CSV accessed via 'data.xx_property_name')
- mods.subject.topic = the Column header in the original AMI Source CSV
- loc_subjects_thing = the Column containing the Label and URI/L pairs in the Processed Data LoD editable Table/Form (and reference CSV)

The same general pattern can be adapted to apply to different mapping scenarios (original CSV source columns to Reconciled LoD Sources) as needed.

??? info "Full list of Column Options => Corresponding LoD Sources"

    - 'loc_subjects_thing' => LoC subjects(LCSH)
    - 'loc_names_thing' => LoC Name Authority File (LCNAF)
    - 'loc_genreForms_thing' => LoC Genre/Form Terms (LCGFT)
    - 'loc_graphicMaterials_thing' => LoC Thesaurus of Graphic Materials (TGN)
    - 'loc_geographicAreas_thing' => LoC MARC List for Geographic Areas
    - 'loc_relators_thing' => LoC Relators Vocabulary (Roles)
    - 'loc_rdftype_CorporateName' => LoC MADS RDF by type: Corporate Name
    - 'loc_rdftype_PersonalName' => LoC MADS RDF by type: Personal Name
    - 'loc_rdftype_FamilyName' => LoC MADS RDF by type: Family Name
    - 'loc_rdftype_Topic' => LoC MADS RDF by type: Topic
    - 'loc_rdftype_GenreForm' =>  LoC MADS RDF by type: Genre Form
    - 'loc_rdftype_Geographic' => LoC MADS RDF by type: Geographic
    - 'loc_rdftype_Temporal' =>  LoC MADS RDF by type: Temporal
    - 'loc_rdftype_ExtraterrestrialArea' => LoC MADS RDF by type: Extraterrestrial Area :space_invader:
    - 'viaf_subjects_thing' => VIAF
    - 'getty_aat_fuzzy' => Getty aat Fuzzy
    - 'getty_aat_terms' => Getty aat Terms
    - 'getty_aat_exact' => Getty aat Exact Label Match
    - 'wikidata_subjects_thing' => Wikidata Q Items


## Next Steps 

To proceed with Processing your AMI Set, [click here](AMIviaSpreadsheets.md#step-7-ami-set-processing) to be directed to the main [Ingesting Digital Objects via Spreadsheets](AMIviaSpreadsheets.md).
___

Thank you for reading! Please contact us on our [Archipelago Commons Google Group](https://groups.google.com/forum/#!forum/archipelago-commons) with any questions or feedback.

Return to the [Archipelago Documentation main page](index.md).

