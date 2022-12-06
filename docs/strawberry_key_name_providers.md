---
title: Strawberry Key Name Providers
tags:
  - Strawberry Key Name Providers
  - Contributing
  - Examples
---

# Strawberry Key Name Providers

For a contextual overview see [Drupal and JSON](metadatainarchipelago.md#drupal-and-json). The following will guide you through mapping a Strawberryfield JSON key to a Strawberry Key Name Provider

## Create a Facet

1. First we are going to create or identify a JSON key to be faceted. For this example, we'll use `date_created_edtf`:

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

2. Next, we are going to create a new *Strawberry Key Name Provider* by going to `Administration > Structure > Strawberry Key Name Providers`, press the `+ Add Strawberry Key Name Provider` button, fill in the fields as follows, and save:

    * `Label`: `Date Created EDTF`
    * `Strawberry Key Name Provider Plugin`: `JmesPath Strawberry Field Key Name Provider`
    * `One or more comma separated valid JMESPaths`: `date_created_edtf.date_free`
    * `Is Date?`: `☑`

3. Then we add the Solr field for indexing by going to `Administration > Configuration > Search and metadata > Search API > Drupal Content to Solr 8 > Fields`, pressing the `Add fields` button, and searching for the field (expand the `🍓 Strawberry (Descriptive Metadata source) (field_descriptive_metadata)` to find and add `field_descriptive_metadata:date_created_edtf_date_free`. Scroll down after adding to make sure the `Type` for the field is correct (`date` in this case). Reindex.

4. Now we'll add the facet by going to `Administration > Configuration > Search and metadata > Facets`, selecting the following options, and saving:

    * `Facet source`: `View Solr search content, display Page`
    * `Field`: `🍓 Strawberry (Descriptive Metadata source) >> date_created_edtf_date_free (field_descriptive_metadata:date_created_edtf_date_free)`
    * `Name`: `🍓 Strawberry (Descriptive Metadata source) >> date_created_edtf_date_free`

5. Continuing with the facet configuration, we select the following from the list of many options, by pressing `Edit` for the facet we just created, updating some of the default settings, and saving:

    * `Facet settings`
        * `☑` `Date item processor`
            * `Date display`
                * `🔘` `Actual date with granularity`
            * `Granularity`
                * `🔘` `Year`
        * `URL alias`: `sbf_date_created_edtf`

6. Finally, we'll add the facet as a block by going to `Administration > Structure > Block layout`, selecting the `Archipelago Base Theme`, pressing the `Place block` button next to `Sidebar second`, selecting the `🍓 Strawberry (Descriptive Metadata source) >> date_created_edtf_date_free`, and pressing the `Place block` button again. Once the block is added, you can drag and drop it to change its position among the existing blocks and saving.

