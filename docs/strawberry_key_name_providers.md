---
title: Strawberry Key Name Providers
tags:
  - Strawberry Key Name Providers
  - Solr
  - Facets
  - Search
---

# Strawberry Key Name Providers, Solr Field, and Facet Configuration

For an overview of how Strawberry Key Name Providers fit within the context of the rest of Archipelago, please see the [Drupal and JSON](metadatainarchipelago.md#drupal-and-json) section in our [Metadata in Archipelago](metadatainarchipelago.md) overview documentation.

In order to expose the Strawberry Field JSON keys (and values) for Archipelago Digital Objects (ADOs) to Search/Solr, Views, and Facets, we need to make use of a plugin system called *Strawberry Key Name Providers*. The following guide covers
- Configuring first the *Strawberry Key Name Providers*
- Then configuring the corresponding Solr Fields necessary for Search and Views exposure
- Finally, the configuration of Facets and placement of Facet blocks on your theme as needed.

## Creating a Strawberry Key Name Provider

1. First, we'll start with an example of a Strawberry Field JSON key that we would like to expose:

    !!! example "date_created_edtf"
    
        ```json hl_lines="8-13"
        ...
        "subject_wikidata": [
            {
                "uri": "http:\/\/www.wikidata.org\/entity\/Q55488",
                "label": "railway station"
            }
        ],
        "date_created_edtf": {
            "date_to": "",
            "date_free": "2016~\/2017~",
            "date_from": "",
            "date_type": "date_free"
        },
        "date_created_free": null,
        ...
        ``` 

2. Next, we are going to create a new *Strawberry Key Name Provider* by going to `Administration > Structure > Strawberry Key Name Providers`, pressing the `+ Add Strawberry Key Name Provider` button, filling in the fields as follows, and saving:
    * `Label`: `Date Created EDTF`
    * `Strawberry Key Name Provider Plugin`: `JmesPath Strawberry Field Key Name Provider`
    * `One or more comma separated valid JMESPaths`: `date_created_edtf.date_free`
    *  Confirm that the value for `Exposed Strawberry Field Property` (under the `One or more comma separated valid JMESPaths` field) is set to `date_created_edtf_date_free`. This is the `Strawberry Field Property` that will hold the data coming from the JMESPath Query when evaluated against and ADO's JSON and will be visible as a *Strawberry Field Property* to Drupal and the Search API. When doing this in a production environment, you might want to change the automatically generated value and assign a simpler one to remember. You can always do this by pressing `Edit`. But for the purpose of this documentation please keep `date_created_edtf_date_free`.
    * `Is Date?`: `☑`

### Strawberry Key Name Provider Plugins

You'll notice that there are four plugins, each with different options, available for different use cases. Below you'll find each plugin with examples from the providers that come with a default deployment.

#### Entity Reference JmesPath Strawberry Field Key Name Provider

!!! example "ismemberof"

    **One or more comma separated valid JMESPaths**: `ismemberof`

    **Entity type**: `node`

#### Flavor/Embedded JSON Service Strawberry Field Key Name Provider

!!! example "hoCR Service"

    **Source JSON Key used to read the Service/Flavour**: `ap:hocr`

#### JmesPath Strawberry Field Key Name Provider

!!! example "Subject Labels"

    **One or more comma separated valid JMESPaths**: `subject_loc[*].label, subject_wikidata[*].label, subject_lcnaf_geographic_names[*].label,subject_temporal[*].label, subject_lcgft_terms[*].label, term_aat_getty[*].label, pubmed_mesh[*].label`

#### JSONLD Strawberry Field Key Name Provider

!!! note "Best Practice"

    As in the example below, if there are a group of flat and unique keys that you want to expose, we recommend creating one provider with this plugin and using a list of keys instead of creating multiple providers. This Provider will also auto assign Lists of Properties from an external JSON-LD ontology/vocabulary (e.g Schema.org). It uses direct access approach, e.g. `type` will get all values for any JSON Key named `type` at any hierarchy level (across the whole JSON document) and it will also use the same exact name (`type`) for the `Exposed Strawberry Field Property`.

!!! example "schema.org"

    **Additional keys separated by commas**: `ismemberof,type,hocr,city,category,country,state,display_name,author,license`

## Creating a Solr Field

1. Go to `Administration > Configuration > Search and metadata > Search API > Drupal Content to Solr 9 > Fields`.
2. Press the `Add fields` button.
3. Search for the field created above (expand the `🍓 Strawberry (Descriptive Metadata source) (field_descriptive_metadata)`, e.g. for the key mapped above, look for `field_descriptive_metadata:date_created_edtf_date_free`.
4. Scroll down after adding to make sure the `Type` for the field is correct (`date` for the example in this guide).
5. Reindex Solr.
    1. Go to `Administration > Configuration > Search and metadata > Search API` and click on the link to the index for your Drupal data.
    2. Press the `Queue all items for reindexing` button.
    3. Let cron reindex or press the `Index now` button.

## Creating a Facet

1. Go to `Administration > Configuration > Search and metadata > Facets`.
2. Press the `+ Add facet` button.
3. Select your facet settings. For the example in this guide, we'll select the following:
    * `Facet source`: `View Solr search content, display Page`
    * `Field`: `🍓 Strawberry (Descriptive Metadata source) >> date_created_edtf_date_free (field_descriptive_metadata:date_created_edtf_date_free)`
    * `Name`: `🍓 Strawberry (Descriptive Metadata source) >> date_created_edtf_date_free`
4. Save.
5. Continue with the facet configuration by pressing `Edit` for the facet we just created and adjusting the many options available as needed. For the example in this guide, we'll adjust the below from the default settings:
    * `Facet settings`
        * `☑` `Date item processor`
            * `Date display`
                * `🔘` `Actual date with granularity`
            * `Granularity`
                * `🔘` `Year`
        * `URL alias`: `sbf_date_created_edtf`
6. Save.

## Creating a Block for the Facet

1. Go to `Administration > Structure > Block layout`.
2. Select the appropriate theme. For the example in this guide, we'll select `Archipelago Base Theme`.
3. Press the `Place block` button next to the appropriate region. For the example in this guide, we'll be placing the block in the `Sidebar second` region.
4. Select your facet from the list. For the example in this guide, we'll select `🍓 Strawberry (Descriptive Metadata source) >> date_created_edtf_date_free`
5. Press the `Place block` button next to the facet. Once the block is added, you can drag and drop it to change its position among the existing blocks and saving.

___

Thank you for reading! Please contact us on our [Archipelago Commons Google Group](https://groups.google.com/forum/#!forum/archipelago-commons) with any questions or feedback.

Return to the [Archipelago Documentation main page](index.md).
