---
title: DataCite Integration
tags:
  - DataCite
  - DOI
  - Fragaria
---

# DataCite Integration

Archipelago's 1.6.0+ release (December 2025) features DataCite Integration, including UI interfaces, DataCite Schema V4 compliance, stable reporting, and test and production environment. Archipelago's DataCite functionality is integrated into the Fragaria Module and RAW JSON workflows.

This documentation page is a work-in-progress and will be updated in the upcoming weeks to provide more detailed information related to this functionality area in the upcoming weeks.

??? note "DataCite Fabrica Account Required"

    To use Archipelago's DataCity Integration, your institution will need to have established DataCite Fabrica account credentials. Archipelago will not provide test account credentials for you to use. 
    
    You can find more information about establshing a DataCite Fabrica account at: https://doi.datacite.org.


### Configuration Form

You can find the DataCite DOI Integration Settings Configuration Form:

- Through the `Manage` menu > `Configuration` > `Archipelago` > `DataCite Services`
- Directly at `/admin/config/archipelago/fragaria/DataCite` 

![DataCite Config Form](images/DataCite_config_form.png)

Please note that your account credentials will be not show the saved password after your initial configuration form save.

### Corresponding Metadata Display Template

The default Archipelago 1.6.0+ release includes a custom Twig template with DataCite V4 mappings for default Archipelago instances. This default template includes all required DataCite elements, plus a few optional elements.

For default Archipelago instances, you can find this template at ~`/metadatadisplay/20`.

### doi_DataCite_action Key

For AMI Set Ingests and Updates, you can use the specialty `doi_DataCite_action` key in your AMI Set CSV and specify one of the following listed operations.

First, add a column to your AMI Set CSV named exactly `doi_DataCite_action`. Then apply one of the following operational values in your corresponding CSV row below the `doi_DataCite_action` column:

- literally nothing (empty), or a `0`, or `null`: if used to update existing Objects, any previous DOIs will stay put. Has no effect/no minting will happen, no change to existing DOIs will happen for that row
- `draft` (written as a word in the cell): if no previous DOI was minted (new ingest/updated Object without DOI), a new Draft DOI will be minted
- `register` (written as a word in the cell): if no previous DOI was minted (new ingest/updated Object without DOI), a new registered DOI will be minted and cannot be longer deleted; if a previous DOI exists, it can transition from its current state to "registered" (draft to registered if the ADO ), (findable to registered only if the ADO ends unpublished after the operation)
- `publis`h (written as a word in the cell). I no previous DOI was minted (new ingest/updated Object without DOI), a new Findable DOI will be minted, can not be longer deleted
- `delete` (written as a word in the cell). I a previous DOI was minted and its current state is draft, it will be deleted.

### DataCite Reporting



___

Thank you for reading! Please contact us on our [Archipelago Commons Google Group](https://groups.google.com/forum/#!forum/archipelago-commons) with any questions or feedback.

Return to the [Archipelago Documentation main page](index.md).
