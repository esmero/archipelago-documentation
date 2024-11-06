# Archipelago Glossary of Terms

This list provides quick reference points for some of the most common Archipelago terminology you are likely to encounter.

If you are not seeing a term listed here, try using the 'Search' found above to look for your term across all documentation pages.

### ADOs : Archipelago Digital Objects and Collections

Abbreviation of Archipelago Digital Object. Can be used to refer to ADO Collections/Compounds/Creative Work Series Parents as well.

Essentially, any Content type that uses a 'Strawberryfield' (see reference below) is an ADO.

### ADO to View Mode Mapping

This refers to the 'ADO to View Mode Mapping' form found at `/admin/config/archipelago/viewmode_mapping`. This form is used to determine the 'Display Mode' used for an ADO based on the object's `type`.

See also the 'Display Mode' and `type` references below.

### AMI: Archipelago Multi Importer

Archipelago's Module for module for batch/bulk/mass ingests of Archipelago digital objects (ADOs) and collections. AMI also enables you to perform batch administrative actions, such as updating, patching/revising, or deleting digital objects and collections.

Related documentation: [Archipelago Multi-Importer (AMI)](ami_index.md).

### AMI Sets

This refers to the special AMI custom entities that hold an Ingest or Update Strategy, a source data CSV with data/metadata, and possibly a CSV for Processed Data coming from Archipelago's [Linked Data Reconciliation](ami_lod_rec.md).

Related documentation: [Archipelago Multi-Importer (AMI) - AMI Set Entity](ami_index.md#ami-set-entity).

### AMI Reports / Reports Tab

The AMI Reports tab contains information related to the last Processing operation performed for an AMI Set.

Related documentation: [Step 10: Review your newly created Digital Objects directly or via AMI Set Report](AMIviaSpreadsheets.md/#step-10-review-your-newly-created-digital-objects-directly-or-via-ami-set-report).

### AMI Update

AMI's Update Operations can be used to Update, Replace, or Append metadata values or files for existing Digital Objects and Collections found in your Archipelago. You can prepare and use AMI Update Sets in different ways using one of three functional operations, depending on your update needs.

Related documentation: [AMI Update Sets](ami_update.md).

### `ap:entitymapping` (JSON key)

This key is used to provide structural mapping hints for Archipelago, such as whether an ADO uses `images` or `ispartof`. 

Related documentation: [Metadata in Archipelago - The ap:entitymapping key](metadatainarchipelago.md#the-apentitymapping-key)

### `as:{as_file_type}` (JSON key)

This key is used to provide filetype mapping information for Archipelago, and will be automatically generated depending on the type of file(s) associated with an ADO. 

Related documentation: [Metadata in Archipelago - The as:{as_file_type} key](metadatainarchipelago.md#the-asas_file_type-keys)

### Compound Object

See 'Digital Object Collection / Compound Object / Creative Work Series' below.

### Creative Work Series

See 'Digital Object Collection / Compound Object / Creative Work Series' below.

### Data Transformation Selections

The data transformation approach used in an AMI Set that determines how your source data will be transformed into ADOs upon AMI Set Processing.

Related documentation: [Step 3. Data Transformation Selections](AMIviaSpreadsheets.md/#step-3-data-transformation-selections).

### Digital Object

This is the Drupal Content Type that refers to Digital Objects in Archipelago. 

This is the Content Type used for standalone/non-hierarchically structured or Child Objects in Parent-Child Relationships.

For most Archipelago repositories, this is the most common Drupal Content Type used.

More context can be found here: [Metadata in Archipelago - Drupal and JSON](metadatainarchipelago.md#drupal-and-json)

### Digital Object Collection / Compound Object / Creative Work Series

This is the Drupal Content Type that refers to Digital Object Collections or Parent Compound Objects / Creative Work Series in Archipelago. 

This is Content Type used for Parent Objects in Parent-Child Relationships (such as Book to Page, where Book = Parent and Page = Child). 

More context can be found here: [Metadata in Archipelago - Drupal and JSON](metadatainarchipelago.md#drupal-and-json)

### Display Mode / View Mode

A Display Mode is a configureable page layout, using Formatters and other options, for the general display of an ADO in Archipelago. You can define Display Modes for different `types` of Digital Objects and Collections. 

Display Mode and View Mode can be used interchangeably.

Related documentation: [Primer on Display Modes](webformsasinput.md). 

### Find and Replace

Archipelago's Advanced Batch Find and Replace functionality provides different ways for you to efficiently Find/Search and Replace metadata values found in the raw JSON of your Digital Objects and Collections.

Related documentation: [Advanced Batch Find and Replace](find_and_replace.md)

### Formatters / Strawberryfield Formatters

See 'Strawberryfield Formatters' below.

### Fragaria Redirects

Archipelago's Fragaria Redirect Module is a special module that provides dynamic redirect routes matched against existing Search API field values.

Related documentation: [Fragaria Redirects Module](fragaria.md). 

### `ismemberof` (JSON key)

This key, in default Archipelago configurations, is used for Collection Membership.

This key can contain multiple values if appropriate for your repository's Collections structure.

For ingested ADOs, this key will contain 'node id' integers for corresponding Collection(s).

In AMI sets, this key can contain integers to connect to an object's corresponding row in the same spreadsheet/CSV, or a UUID for an already ingested Collection object.

This key should never contain string values for Collection labels/titles.

See also `ap:entitymapping` key above.

### `ispartof` (JSON key)

This key, in default Archipelago configurations, is used for Parent-Child Object Relationships (so a Child ADO would reference the Parent ADO in `ispartof`).

This key should contain single values only, due to the nature of the Parent-Child Object Relationship.

For ingested ADOs, this key will contain 'node id' integers for a corresponding Parent Object.

In AMI sets, this key can contain integers to connect to an object's corresponding row in the same spreadsheet/CSV, or a UUID for an already ingested Parent object.

This key should never contain string values for Parent Object labels/titles.

See also `ap:entitymapping` key above.

### `label` (JSON key)

This key is used for the title of the Digital Object or Collection. This key is **required** for all ADOs. 

Related documentation: [Metadata in Archipelago - The label key](metadatainarchipelago.md#the-label-key)

### Linked Data Reconciliation / LoD Reconciliation

AMI's (Archipelago Multi Importer's) Linked Data Reconciliation tool can be used to enrich your metadata with Linked Data (LoD). Using this tool, you can map values from your topical/subject metadata elements to your preferred LoD vocabulary source. These mappings can then be transformed via a corresponding Metadata Display (Twig) template to process the values into JSON-formatted metadata for your specified AMI set.

Related documentation: [Linked Data Reconciliation](ami_lod_rec.md).

### Metadata API Module

Archipelago's Metadata API Module is a special Strawberryfield Formatter Module that implements Metadata API Endpoints and uses Views and Metadata Display Entities as a configuration option to provide dynamic APIs based on Metadata present in each Archipelago Digital Object (or Node) that contains a Strawberryfield type of field (JSON).

Related documentation: [Metadata API Module](open_api.md)

### Metadata Display Entities / Twig Templates

Metadata Display Entities are particular Twig Templates used in Archipelago to transform or 'cast' JSON metadata/data into different displays and outputs.

Related documentation: [Twig Templates and Archipelago](metadatatwigs.md)

### Metadata Display Preview

Archipelago's Metadata Display Preview is a very handy tool for your repository toolkit that enables you to preview the output of your Metadata Display (Twig) Templates (found at `/metadatadisplay/list`).

Related documentation: [Metadata Display Preview](metadata_display_preview.md)

### Metadata Display Usage

Archipelago's Metadata Display Usage tab is an extension of Metadata Display Preview that provides a convenient way for you to check where a Metadata Display Template is currently being used across your Archipelago. 

Related documentation: [Metadata Display Usage](metadata_display_usage.md)

### Process Tab / AMI Set Processing

The Process Tab contains configuration options for selecting outcomes related to the Processing of your AMI Set.

Related documentation: [Step 7: AMI Set Processing](AMIviaSpreadsheets.md/#step-7-ami-set-processings).

### Search and Replace

See 'Find and Replace' above.

### Search and Solr

Refers to the integrations of related (Archipelago, Drupal) Search and Solr Index components in Archipelago.

Related documentation: [Search & Solr Overview](search-solr-index.md)

### Strawberryfield (or Strawberry Field)

The flexible, extensible JSON blob Field attached to every Archipelago Digital Object / Collection / Compound Object / Creative Work Series.

The Strawberryfield contains all of the data and metadata associated with ADOs in a single Field (per ADO).

In default Archipelagos, can be viewed in the 'Raw JSON' display beneath every ADO when logged in as an authenticated user.

Related documentation: [Metadata in Archipelago](metadatainarchipelago.md).

### Strawberry Flavour

A 'Strawberry flavour' refers to a post-processing related document, such as a generated HOCR solr document or new filetype created by Archipelago such as a WACZ file generated from an original WARC file.

### Strawberryfield Formatter

Strawberryfield Formatters are Archipelago specific Field Formatters that can transform Strawberryfield JSON data into different formats for display, including in IIIF Manifests and/or different Viewers.

More information found at: [Strawberryfield Formatters](strawberryfield-formatters.md)

### Strawberry Keyname Provider

Strawberry Keyname Providers feed the values from your desired JSON keys to Solr fields. They are a crucial part of the Search and Solr setup in Archipelago.

Related Documentation: [Search & Solr Overview - Strawberry Keyname Providers](search_solr_index.md#2-strawberry-keyname-providers)

### Strawberryfield Modules

Strawberryfield Modules are the Archipelago specific modules at the heart of our system.

- [Strawberryfield](strawberryfields.md)
    - [Github Repository](https://github.com/esmero/strawberryfield)
    - [Related documentation](strawberryfields.md)
- Format Strawberryfield
    - [Github Repository](https://github.com/esmero/format_strawberryfield)
    - [Related documentation](strawberryfield-formatters.md)
- Webform Strawberryfield
    - [Github Repository](https://github.com/esmero/webform_strawberryfield)
    - [Related documentation](webforms.md)
- Strawberry Runners
    - [Github Repository](https://github.com/esmero/strawberry_runners)
    - [Related documentation](strawberryrunners.md)
- Archipelago Multi-Importer (AMI)
    - [Github Repository](https://github.com/esmero/ami)
    - [Related documentation](ami_index.md)
- Fragaria (Redirects)
    - [Github Repository](https://github.com/esmero/fragaria)
    - [Related Documentation](fragaria.md)

Related documentation: [Strawberryfields Forever](strawberryfields.md).

### Strawberry Runners / SBR

Archipelago's Strawberry Runners (SBR) module provides provides a set of post-processing capabilities for the JSON based metadata, files and entities that comprise your Archipelago Digital Objects (ADOs). 

Related documentation: [Strawberry Runners Post-Processing Configuration](strawberryrunners.md).

### Twig Templates / Metadata Display Entities

See 'Metadata Display Entities' above.

### `type` (JSON key)

This key is used for the Digital Object or Digital Object Collection/semantic Type, such as 'Photograph' or 'Collection'. This key is **required** for all ADOs.

Related documentation: [Metadata in Archipelago - The type key](metadatainarchipelago.md#the-type-key)

### View Mode / Display Mode

See 'Display Mode' entry above.

### Webforms

The 'Webform Strawberryfield' module provides Drupal Webform ( == awesome piece of code) integrations for StrawberryField, so you can really have control over your Metadata ingests entered and edited via webforms.

Related documentation: [Webforms in Archipelago](webforms.md)

___

Thank you for reading! Please contact us on our [Archipelago Commons Google Group](https://groups.google.com/forum/#!forum/archipelago-commons) with any questions or feedback.

Return to the [Archipelago Documentation main page](index.md).


