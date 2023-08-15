---

title: Using the JSON Patch Metadata for Archipelago Digital Objects VBO action
tags:

- Advanced Batch Find and Replace

- JSON Patch Find and Replace

- Search and Replace

- JSON Patch

- JSON Pointer 

---

# JSON Patch Find and Replace

Enables you to carry out advanced [JSON Patch](https://jsonpatch.com) operations within your metadata.

!!! note

    Please refer to the main [Find and Replace documentation page](find_and_replace.md) for a general overview of [where to find within your Archipelago](find_and_replace.md#where-to-find), a [general overview of default options](find_and_replace.md#main-page-overview) and [important notes and workflow recommendations](find_and_replace.md#important-notes--workflow-recommendations).

## What is a JSON Patch and when to use it?

Before we dive into the mechanics of doing JSON Patching Batch in Archipelago we need to learn what a **JSON Patch** is and of course when applying this action is useful, possible (or not).

A JSON Patch is a `JSON` Document containing precise operations that can heavily modify the structure and values of an existing JSON Document, in this case the RAW JSON found inside a strawberryfield of our ADOs. 

The operations available for modifications of an JSON document are:

- “add”
- “remove”
- “replace”,
- “move”
- “copy”

And there is also one (very important) used to check/validate the existence of values/keys:

- “test”

Even if you can have multiple operations in a single JSON Patch Document, they are always applied as a whole. Means if any of those fail nothing will be applied and in concrete, in our VBO action, no change will be done to your ADO. This in combination with the “test” operation gives you a lot of safety and a way of discerning/skipping completely a complex set of operations on e.g. a failed ”test”.

JSON Patch uses `JSON Pointers` as arguments in all of these operations to target a specific key/value of your JSON.

Using the following JSON Snippet as example:

```JSON
"subject_lcgft_terms": [
    {
        "uri": "http:\/\/id.loc.gov\/authorities\/genreForms\/gf2017027249",
        "label": "Photographs"
    }
]
```

These JSON Pointers will resolve in the following manner:

- `/subject_lcgft_terms` 

```JSON
[
    {
        "uri": "http:\/\/id.loc.gov\/authorities\/genreForms\/gf2017027249",
        "label": "Photographs"
    }
]
```

- `/subject_lcgft_terms/0`
 
```JSON
{
    "uri": "http:\/\/id.loc.gov\/authorities\/genreForms\/gf2017027249",
    "label": "Photographs"
}
```

- `/subject_lcgft_terms/0/label`

```
Photographs
```

AS you can see a JSON Pointer is very precise , allowing you to target complete structures and values but it *does not allow* Wildcard Operations. Means you can not "search" or do loosy comparissons. This very fact limits many times the use case. E.g. if you have a list of terms like this:

```JSON
"terms": [
	"term1",
	"term2",
	"term3"
]
```

and you want to "test" for the existence of `"term3"` before applying a change, you would need to know exactly at what position inside the `terms Array` (Starting from 0) it will/should be found. And that might not be consistent across every ADO. 

So how do we use these pointers in an operation inside a JSON Patch document? Using the same fake "terms" JSON snippet the previously listed, operations are written like this:

## add

```JSON
{ "op": "add", "path": "/terms/0", "value": "term_another" }
```

This will add before the first term (in this case "term1" ) "term_another" as a value.

??? info "At the end"

    you can use a dash (`-`) (e.g. "/terms/-") instead of the numeric position of an array entry to denote that the "value" needs to be added at the end. This is needed specially for empty lists. You can not target `0` position on an empty array.

## remove

```JSON
{ "op": "remove", "path": "/terms/1"}
```

This will remove the second term (in this case "term2" )

## replace

```JSON
{ "op": "replace", "path": "/terms/1", "value": "term_again" }
```
This will remove the second term (in this case "term2" ) and put in its place "term_again" as a value. Basically two operationes, “remove“ and “add “ in a single step

## move

```JSON
{ "op": "move", "from": "/terms", "path": "/terms_somewhere_else"}
```

This will copy all values inside top JSON key `terms` into a new top JSON key named `terms_somewhere_else` and remove then the old `terms` key.

## copy

```JSON
{ "op": "copy", "from": "/terms", "path": "/terms_somewhere_else"}
```

Similar to “move“, it will copy values inside top JSON key `terms` into a new top JSON key named `terms_somewhere_else` without removing then the old `terms` key or its content!

## test

```JSON
{ "op": "test", "path": "/terms/0", "value": "term1"}
```

Finally, “test“ will check if on position `0` of terms there is a value of "term1". If not, the test will fail. If a single test fails the whole JSON Patch will be cancelled. Tests can not be concatenated via OR bolean operators. So they always act individually. Two tests with one failing is a failed JSON Patch.

There is more of course! The Complete documentation can be found [here](https://jsonpatch.com)

So. When to use JSON Patch? There are a few general rules/suggestions:

- When you can generate one or more precise “test“ operations. If you need to test against data that might exists but its position is not clear, a single JSON Patch will not do the trick. That said you could run the same JSON Patch document against the same set of ADOs more than once modifying slightly the “test“ to cover all variations (check for value at positition `0`, then again at position `1`, etc)
- When you need to use data that is already inside the JSON Document and move it around. Sometimes `fixing` a value, in the sense of putting a static replacement, is not what you need. Other `Actions` [documented](find_and_replace.md) will allow you to replace one value with another fixed one. But JSON Patch will allow you to use existing data inside your target JSON and move it/copy it.
- When you operate on multiple JSON Keys and can target / match complete values. You can not replace a single word inside a value via a JSON Patch. But you can apply multiple precise operations in a single pass.
- When you need to change data types. e.g. convert a single value into an array.
- When you need safety. The fact that a single failed “test“ will cancel a whole JSON Patch allows you to operate with safety and ensure the final destination will always be a JSON

Now that we know what it is and when we should do it, we can make a small exercise. The goal:

- For all ADO's with json key with value `photograph` that are member of collection with Node ID `16`
    - convert the `description` key from a single value into an array.
    - add an extra LOD entry
    - Copy a Value into a new Key

## Step 1: Select the ADOs to be Modified

As with every other VBO action described in our documentation, start by selecting the ADOs you want to modify using the exposed Search Field(s) and/or Facets present in the Search and Replace `View` found at `/search-and-replace` . 

Once you have filtered down the list to manageable size, containing at least the ADOs you plan on modifying (but for JSON Patch operation could be more too because you can “test“ and match ), press either on `Select / deselect all results in this view` to pass all the result (this includes all pages, not only the currently visible one) or go selectively one by one by checking on the `toggle` found beside each ADO's title. You will see how the number in `Selected 0 item in this view` increases. Now press `Apply to select Items`. We will use for this example `Laddie the dog running in the garden, Bronx, N.Y., undated [c. 1910-1918?]`, an ADO we provide in our DEMO sets. 

Redundant to say, Batch Actions are intended to be used when a modification needs to be applied to more than one item, implying there is a pattern. For single ADOs you can always do this faster directly via the **EDIT** tab.

??? note "Keep your JSON Patches (and friends) around"

    Since JSON Patching involves writing a, sometimes complex, JSON Document, please keep around an Application (or Text File) where you can copy/paste and save your JSON Patches for resuse or future references. Archipelago will not store nor remember between runs the JSON Patch document you submitted. It is also very useful to copy and have at hand the `RAW JSON` of one of the ADOs you plan to modify as a reference/aid while building the given JSON Patch document.

## Step 2: JSON Patch Metadata for Archipelago Digital Objects Action Configuration

The default config JSON Patch form will contain an example JSON Patch set of Commands (Document)

![Default JSON Patch Command](images/json_patch_config_default.jpg)

We are going to replace this one with a valid JSON document. Notice that it does not require a root `{}` Object wrapper and it is really a list (or array) of operations. 

A bit of repetition but needed to explain the JSON Patch document. You can see on the following Note box how we copyed the RAW JSON of one ADO to be Patched to have a reference while building the JSON PATCH. for the sake of brevity we remove here the longer Image File technical Metadata.

??? info "RAW JSON of `Laddie the dog running in the garden, Bronx, N.Y., undated [c. 1910-1918?]`"

    ```JSON title="Laddie the dog running in the garden, Bronx, N.Y., undated [c. 1910-1918?] | before Patching"
    {
        "note": "",
        "type": "Photograph",
        "viaf": "",
        "label": "Laddie the dog running in the garden, Bronx, N.Y., undated [c. 1910-1918?]",
        "model": [],
        "owner": "New-York Historical Society, 170 Central Park West, New York, NY 10024, 212-873-3400.",
        "audios": [],
        "images": [
            26
        ],
        "models": [],
        "rights": "This digital image may be used for educational or scholarly purposes without restriction. Commercial and other uses of the item are prohibited without prior written permission from the New-York Historical Society. For more information, please visit the New-York Historical Society's Rights and Reproductions Department web page at [http:\/\/www.nyhistory.org\/about\/rights-reproductions](http:\/\/www.nyhistory.org\/about\/rights-reproductions).",
        "videos": [],
        "creator": "",
        "ap:tasks": {
            "ap:sortfiles": "index"
        },
        "duration": "",
        "ispartof": [],
        "language": "English",
        "documents": [],
        "edm_agent": "",
        "publisher": "",
        "ismemberof": [
            16
        ],
        "creator_lod": [
            {
                "name_uri": "",
                "role_uri": "http:\/\/id.loc.gov\/vocabulary\/relators\/pht",
                "agent_type": "personal",
                "name_label": "Stonebridge, George Ehler",
                "role_label": "Photographer"
            }
        ],
        "description": "George Ehler Stonebridge (d. 1941) was an amateur photographer who lived and worked in the Bronx, New York. He left little record of himself, but an invaluable one of his surroundings and interests. Stonebridge lived at several locations in the Bronx with his wife Bella, and their three children Grace, George, and William. He worked at the Northern Gaslight Company, although the position he held is unknown. In addition to taking photographs, Stonebridge wrote poetry and prose about his love of the Bronx, his children, and in honor of military victories. Some of Stonebridge's photographs appeared in local papers. In 1898, he was an authorized reporter and photographer for the North Side News; in 1905 he was an authorized reporter for the Bronx Borough Record and Times, and probably took photographs for that paper as well. Stonebridge was fascinated with the subject of military preparedness. Training rituals and staged battles were one of his favorite photographic subjects. His 1898 poem, \"Remember the Maine,\" celebrates the United States' victory in the Spanish-American War. He was especially proud of soldiers from the Bronx, and photographed historical tablets throughout the Borough commemorating previous military victories. Stonebridge also used his photographs to illustrate lectures. In 1907, he gave several lectures on \"The Training of War,\" using colored lantern slides.",
        "interviewee": "",
        "interviewer": "",
        "pubmed_mesh": null,
        "sequence_id": "",
        "subject_loc": [
            {
                "uri": "http:\/\/id.loc.gov\/authorities\/subjects\/sh85038796",
                "label": "Dogs"
            }
        ],
        "website_url": "",
        "as:generator": {
            "type": "Update",
            "actor": {
                "url": "http:\/\/localhost:8001\/form\/descriptive-metadata",
                "name": "descriptive_metadata",
                "type": "Service"
            },
            "endTime": "2022-12-05T09:19:37-05:00",
            "summary": "Generator",
            "@context": "https:\/\/www.w3.org\/ns\/activitystreams"
        },
        "date_created": "1910-01-01",
        "issue_number": null,
        "date_published": "",
        "subjects_local": null,
        "term_aat_getty": "",
        "ap:entitymapping": {
            "entity:file": [
                "model",
                "audios",
                "images",
                "videos",
                "documents",
                "upload_associated_warcs"
            ],
            "entity:node": [
                "ispartof",
                "ismemberof"
            ]
        },
        "europeana_agents": "",
        "europeana_places": "",
        "local_identifier": "nyhs_PR066_6136",
        "subject_wikidata": [
            {
                "uri": "http:\/\/www.wikidata.org\/entity\/Q3381576",
                "label": "black-and-white photography"
            },
            {
                "uri": "http:\/\/www.wikidata.org\/entity\/Q60",
                "label": "New York City"
            }
        ],
        "date_created_edtf": "",
        "date_created_free": null,
        "date_embargo_lift": null,
        "physical_location": null,
        "related_item_note": null,
        "rights_statements": "In Copyright - Educational Use Permitted",
        "europeana_concepts": "",
        "geographic_location": {
            "lat": "40.8466508",
            "lng": "-73.8785937",
            "city": "New York",
            "state": "New York",
            "value": "The Bronx, Bronx County, New York, United States",
            "county": "",
            "osm_id": "9691916",
            "country": "United States",
            "category": "boundary",
            "locality": "",
            "osm_type": "relation",
            "postcode": "",
            "country_code": "us",
            "display_name": "The Bronx, Bronx County, New York, United States",
            "neighbourhood": "Bronx County",
            "state_district": ""
        },
        "note_publishinginfo": null,
        "subject_lcgft_terms": [
            {
                "uri": "http:\/\/id.loc.gov\/authorities\/genreForms\/gf2017027249",
                "label": "Photographs"
            }
        ],
        "upload_associated_warcs": [],
        "physical_description_extent": "",
        "subject_lcnaf_personal_names": "",
        "subject_lcnaf_corporate_names": "",
        "subjects_local_personal_names": "",
        "related_item_host_location_url": null,
        "subject_lcnaf_geographic_names": [
            {
                "uri": "http:\/\/id.loc.gov\/authorities\/names\/n81059724",
                "label": "Bronx (New York, N.Y.)"
            },
            {
                "uri": "http:\/\/id.loc.gov\/authorities\/names\/n79007751",
                "label": "New York (N.Y.)"
            }
        ],
        "related_item_host_display_label": null,
        "related_item_host_local_identifier": null,
        "related_item_host_title_info_title": "",
        "related_item_host_type_of_resource": null,
        "physical_description_note_condition": null
    }
    ```
    
    **Based on our own plan:**
    
    1. Check first if of type `photograph` and `memberof` Node ID `16`
    
    ```JSON
    { "op": "test", "path": "/type", "value": "Photograph"},
    { "op": "test", "path": "/ismemberof", "value": [16]}
    ```
    
    Notice that to be really sure we also match the data type, an `array` with a single value `16` of type integer (makes sense since the Operation is also a JSON and will be evaluated in the same way as the source RAW JSON). This is a precise match. if the ADO belongs to multiple Collections it will fail of course.
    
    2. Now we are going to convert `description` key from a single value into an array.
    
    ```JSON
    {"op": "add","path": "/temp_description_array","value": []},
    {"op": "move","from": "/description","path": "/temp_description_array/-"},
    {"op": "move","from": "/temp_description_array","path": "/description"},
    ```
    
    This is a multi step operation. Given that JSON Patch can not "cast" types and depends on a given datatype to be present before, e.g. adding a new value to it, we use here a temporary key.
    Notice that you can not “add“ or “move“ to e.g. the position `0` because the destination array is indeed empty (that will fail). But by using the `dash` you can command it to put it at the end, which on an empty list is also at the beginning (we are starting to understand this!). 
    
    3. And the extra LOD entry for wikidata, in this case the Q Item for [collie](https://www.wikidata.org/wiki/Q1196071) (type of herding dog)
    
    ```JSON
    { "op": "add", "path": "/subject_wikidata/-", "value": {
            "uri": "https://www.wikidata.org/wiki/Q1196071",
            "label": "collie"
        }
    }
    ```

4. Finally we are going to copy the `geographic_location.state` value and put it into `subjects_local`:

```JSON
{"op": "remove", "path": "/subjects_local"},
{"op": "add", "path": "/subjects_local", "value": []},
{"op": "copy", "from": "/geographic_location/state", "path": "/subjects_local/-"}
```

Why so many operations? Because initially ` "subjects_local"` had a value of `null` and it is not suited to generate/add to it as a multivalued key because of that. So we need to remove it first, recreate it as an empty list and then we can copy. **Pro Note:** you could partially rewrite this as `replace` operation!

??? info "what if subjects_local has data?"

    You will lose it! you can add a “test“ or move data instead of recreating. Many choices.

The final JSON Patch will look like this. Copy it into the Configuration JSON Patch commands form field: 

```JSON
[
    {"op": "test", "path": "/type", "value": "Photograph"},
    {"op": "test", "path": "/ismemberof", "value": [16]},
    {"op": "add","path": "/temp_description_array","value": []},
    {"op": "move","from": "/description","path": "/temp_description_array/-"},
    {"op": "move","from": "/temp_description_array","path": "/description"},
    {"op": "add","path": "/subject_wikidata/-","value": 
        {
        "uri": "https://www.wikidata.org/wiki/Q1196071",
         "label": "collie"
         }
    },
    {"op": "remove", "path": "/subjects_local"},
    {"op": "add", "path": "/subjects_local", "value": []},
    {"op": "copy", "from": "/geographic_location/state", "path": "/subjects_local/-"}
]
```

??? note "The inverse process online"

    Now that you know (or are starting to understand) the manual process you can also try this [online tool](https://json-patch-builder-online.github.io) that allows you, based on a source and a destination JSON, generate the needed JSON Patch to mutate one JSON into the another. The logic might not be always what you need and most likely it will not take in account that you actually need to move values and will prefer to fix values to be added via an add operation


## Step 3: Run the JSON Patch Action in simulation mode

Ready. Now **check** the "only simulate and debug affected JSON" checkbox. We want to see if we did well but not yet modify any ADOs. Press `Apply` button. You will get another confirmation screen. Press `Execute Action`.

It will run quick (on this example) and you will redirected back on the original Drupal Views of Step 1. If your source `ADO` is actually the one from our Demo Collection you might see a `diff`, something very similar to this:

``` title="Text based diff after a successfull Patch"
129d129 < "description": "George Ehler Stonebridge (d. 1941) was an amateur photographer who lived and worked in the Bronx, New York. He left little record of himself, but an invaluable one of his surroundings and interests. Stonebridge lived at several locations in the Bronx with his wife Bella, and their three children Grace, George, and William. He worked at the Northern Gaslight Company, although the position he held is unknown. In addition to taking photographs, Stonebridge wrote poetry and prose about his love of the Bronx, his children, and in honor of military victories. Some of Stonebridge's photographs appeared in local papers. In 1898, he was an authorized reporter and photographer for the North Side News; in 1905 he was an authorized reporter for the Bronx Borough Record and Times, and probably took photographs for that paper as well. Stonebridge was fascinated with the subject of military preparedness. Training rituals and staged battles were one of his favorite photographic subjects. His 1898 poem, \"Remember the Maine,\" celebrates the United States' victory in the Spanish-American War. He was especially proud of soldiers from the Bronx, and photographed historical tablets throughout the Borough commemorating previous military victories. Stonebridge also used his photographs to illustrate lectures. In 1907, he gave several lectures on \"The Training of War,\" using colored lantern slides.", 155d154 < "subjects_local": null, 182a180,183 > }, > { > "uri": "https:\/\/www.wikidata.org\/wiki\/Q1196071", > "label": "collie" 236c238,244 < "physical_description_note_condition": null --- > "physical_description_note_condition": null, > "description": [ > "George Ehler Stonebridge (d. 1941) was an amateur photographer who lived and worked in the Bronx, New York. He left little record of himself, but an invaluable one of his surroundings and interests. Stonebridge lived at several locations in the Bronx with his wife Bella, and their three children Grace, George, and William. He worked at the Northern Gaslight Company, although the position he held is unknown. In addition to taking photographs, Stonebridge wrote poetry and prose about his love of the Bronx, his children, and in honor of military victories. Some of Stonebridge's photographs appeared in local papers. In 1898, he was an authorized reporter and photographer for the North Side News; in 1905 he was an authorized reporter for the Bronx Borough Record and Times, and probably took photographs for that paper as well. Stonebridge was fascinated with the subject of military preparedness. Training rituals and staged battles were one of his favorite photographic subjects. His 1898 poem, \"Remember the Maine,\" celebrates the United States' victory in the Spanish-American War. He was especially proud of soldiers from the Bronx, and photographed historical tablets throughout the Borough commemorating previous military victories. Stonebridge also used his photographs to illustrate lectures. In 1907, he gave several lectures on \"The Training of War,\" using colored lantern slides." > ], > "subjects_local": [ > "New York" > ]
```

Which means your Patch would have been applied!

In case something went wrong, e.g. any of the operations did not match your source data, you will see a `WARNING` like this

```
Patch could not be applied for Laddie the dog running in the garden, Bronx, N.Y., undated [c. 1910-1918?]
```

## Step 4: Run the JSON Patch Action but for real!

Now that actual patching. Repeat from Step 1 to Step 3 but keep "only simulate and debug affected JSON" **unchecked** and follow the steps again. You ADO will be modified and you will get almost no notifications except of an action completed notice (in soothing green). If you check Laddie's ADO RAW json (expand in the same resulting view) it will look now like this

```JSON title="Laddie the dog running in the garden, Bronx, N.Y., undated [c. 1910-1918?] JSON after patching" hl_lines="37 38 39 65 66 67 95 96 97 98"
{ 
    "note": "",
    "type": "Photograph",
    "viaf": "",
    "label": "Laddie the dog running in the garden, Bronx, N.Y., undated [c. 1910-1918?]",
    "model": [],
    "owner": "New-York Historical Society, 170 Central Park West, New York, NY 10024, 212-873-3400.",
    "audios": [],
    "images": [
        26
    ],
    "models": [],
    "rights": "This digital image may be used for educational or scholarly purposes without restriction. Commercial and other uses of the item are prohibited without prior written permission from the New-York Historical Society. For more information, please visit the New-York Historical Society's Rights and Reproductions Department web page at [http:\/\/www.nyhistory.org\/about\/rights-reproductions](http:\/\/www.nyhistory.org\/about\/rights-reproductions).",
    "videos": [],
    "creator": "",
    "ap:tasks": {
        "ap:sortfiles": "index"
    },
    "duration": "",
    "ispartof": [],
    "language": "English",
    "documents": [],
    "edm_agent": "",
    "publisher": "",
    "ismemberof": [
        16
    ],
    "creator_lod": [
        {
            "name_uri": "",
            "role_uri": "http:\/\/id.loc.gov\/vocabulary\/relators\/pht",
            "agent_type": "personal",
            "name_label": "Stonebridge, George Ehler",
            "role_label": "Photographer"
        }
    ],
    "description": [
        "George Ehler Stonebridge (d. 1941) was an amateur photographer who lived and worked in the Bronx, New York. He left little record of himself, but an invaluable one of his surroundings and interests. Stonebridge lived at several locations in the Bronx with his wife Bella, and their three children Grace, George, and William. He worked at the Northern Gaslight Company, although the position he held is unknown. In addition to taking photographs, Stonebridge wrote poetry and prose about his love of the Bronx, his children, and in honor of military victories. Some of Stonebridge's photographs appeared in local papers. In 1898, he was an authorized reporter and photographer for the North Side News; in 1905 he was an authorized reporter for the Bronx Borough Record and Times, and probably took photographs for that paper as well. Stonebridge was fascinated with the subject of military preparedness. Training rituals and staged battles were one of his favorite photographic subjects. His 1898 poem, \"Remember the Maine,\" celebrates the United States' victory in the Spanish-American War. He was especially proud of soldiers from the Bronx, and photographed historical tablets throughout the Borough commemorating previous military victories. Stonebridge also used his photographs to illustrate lectures. In 1907, he gave several lectures on \"The Training of War,\" using colored lantern slides."
    ],
    "interviewee": "",
    "interviewer": "",
    "pubmed_mesh": null,
    "sequence_id": "",
    "subject_loc": [
        {
            "uri": "http:\/\/id.loc.gov\/authorities\/subjects\/sh85038796",
            "label": "Dogs"
        }
    ],
    "website_url": "",
    "as:generator": {
        "type": "Update",
        "actor": {
            "url": "http:\/\/localhost:8001\/form\/descriptive-metadata",
            "name": "descriptive_metadata",
            "type": "Service"
        },
        "endTime": "2022-12-05T09:19:37-05:00",
        "summary": "Generator",
        "@context": "https:\/\/www.w3.org\/ns\/activitystreams"
    },
    "date_created": "1910-01-01",
    "issue_number": null,
    "date_published": "",
    "subjects_local": [
        "New York"
    ],
    "term_aat_getty": "",
    "ap:entitymapping": {
        "entity:file": [
            "model",
            "audios",
            "images",
            "videos",
            "documents",
            "upload_associated_warcs"
        ],
        "entity:node": [
            "ispartof",
            "ismemberof"
        ]
    },
    "europeana_agents": "",
    "europeana_places": "",
    "local_identifier": "nyhs_PR066_6136",
    "subject_wikidata": [
        {
            "uri": "http:\/\/www.wikidata.org\/entity\/Q3381576",
            "label": "black-and-white photography"
        },
        {
            "uri": "http:\/\/www.wikidata.org\/entity\/Q60",
            "label": "New York City"
        },
        {
            "uri": "https:\/\/www.wikidata.org\/wiki\/Q1196071",
            "label": "collie"
        }
    ],
    "date_created_edtf": "",
    "date_created_free": null,
    "date_embargo_lift": null,
    "physical_location": null,
    "related_item_note": null,
    "rights_statements": "In Copyright - Educational Use Permitted",
    "europeana_concepts": "",
    "geographic_location": {
        "lat": "40.8466508",
        "lng": "-73.8785937",
        "city": "New York",
        "state": "New York",
        "value": "The Bronx, Bronx County, New York, United States",
        "county": "",
        "osm_id": "9691916",
        "country": "United States",
        "category": "boundary",
        "locality": "",
        "osm_type": "relation",
        "postcode": "",
        "country_code": "us",
        "display_name": "The Bronx, Bronx County, New York, United States",
        "neighbourhood": "Bronx County",
        "state_district": ""
    },
    "note_publishinginfo": null,
    "subject_lcgft_terms": [
        {
            "uri": "http:\/\/id.loc.gov\/authorities\/genreForms\/gf2017027249",
            "label": "Photographs"
        }
    ],
    "upload_associated_warcs": [],
    "physical_description_extent": "",
    "subject_lcnaf_personal_names": "",
    "subject_lcnaf_corporate_names": "",
    "subjects_local_personal_names": "",
    "related_item_host_location_url": null,
    "subject_lcnaf_geographic_names": [
        {
            "uri": "http:\/\/id.loc.gov\/authorities\/names\/n81059724",
            "label": "Bronx (New York, N.Y.)"
        },
        {
            "uri": "http:\/\/id.loc.gov\/authorities\/names\/n79007751",
            "label": "New York (N.Y.)"
        }
    ],
    "related_item_host_display_label": null,
    "related_item_host_local_identifier": null,
    "related_item_host_title_info_title": "",
    "related_item_host_type_of_resource": null,
    "physical_description_note_condition": null
}
```

That is all. Again, keep your **JSON Patches** safe in a text document, test/try simple things first, look for **patterns**, look for *No-Nos* that can become "tests" to avoid touching ADOs that do not need to be updated and always remember that the destination type (single value, array or object) of an existing Key might affect your complex logic. Happy Patching!

___

Thank you for reading! Please contact us on our [Archipelago Commons Google Group](https://groups.google.com/forum/#!forum/archipelago-commons) with any questions or feedback.

Return to the main [Find and Replace documentation page](https://github.com/esmero/archipelago-documentation/blob/1.0.0/docs/find_and_replace.md) or the [Archipelago Documentation main page](https://github.com/esmero/archipelago-documentation/blob/1.0.0/docs/index.md).
