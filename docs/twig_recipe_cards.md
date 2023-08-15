---
title: Twig Recipe Cards for Common Use Cases
tags:
  - Twig
  - Twig Filters
  - Twig Functions
  - Twig Templates
  - Examples
  - JSON
---

# Twig Recipe Cards for Common Use Cases

The Twig Recipe Cards below reference common Metadata transformation, display, or other use cases/needs you may have in your own Archipelago repository.

### Getting Started Working with Twig in Archipelago

We recommend reading through our main [Metadata Display Preview](metadata_display_preview.md) and [Twigs in Archipelago documentation](metadatatwigs.md) overview guides, and also our [Working with Twig](workingtwigs.md) primer before diving into applying any of these recipes in your own Archipelago.

## AMI Ingest Template Adaptations -- Common Use Cases and Twig Recipe Cards:

**Use Case #1:** I used [AMI LoD Reconciliation](ami_lod_rec.md) to reconciliate the values in my AMI Set Source CSV `mods_subject_topic` column against both LCSH and Wikidata. I would like to map the reconciliated values into the Archipelago default `subject_loc` and `subject_wikidata` JSON keys.

**Twig Recipe Card for Use Case #1:**

```twig
  {#- LCSH -#}
    {% if data.mods_subject_topic|length > 0 %}
    "subject_loc": {{ data_lod.mods_subject_topic.loc_subjects_thing|json_encode|raw }},
    {% endif %}
  {#- Wikidata -#}     
    {% set subject_wikidata = [] %}
    {% for source, reconciliated in data_lod %}
      {% if (('subject' in source) or ('genre' in source)) and reconciliated.wikidata_subjects_thing and reconciliated.wikidata_subjects_thing|length > 0 %}
         {% set subject_wikidata = subject_wikidata|merge(reconciliated.wikidata_subjects_thing) %}
      {% endif %}
    {% endfor %}  
```

**Use Case #2:** I have both columns containing a `mods_subject_authority_lcsh_topic` (labels) and corresponding `mods_subject_authority_lcsh_valueuri` (URIs) data in my AMI Set Source Data CSV that I would like to pair and map into the Archipelago default `subject_loc` JSON key.

**Twig Recipe Card for Use Case #2:**

```twig
{%- if data['mods_subject_authority_lcsh_topic'] is defined and not empty -%}
    {% set subjects = data["mods_subject_authority_lcsh_topic"] is iterable ? data["mods_subject_authority_lcsh_topic"] : data["mods_subject_authority_lcsh_topic"]|split('|@|') %} 
    {% set subject_uris = data["mods_subject_authority_lcsh_valueuri"] is defined ? data["mods_subject_authority_lcsh_valueuri"] : '' %} 
    {% set subject_uris_list = subject_uris is iterable ? subject_uris : subject_uris|split('|@|') %}
    "subject_loc": [
        {% for subject in subjects %}
        {
            "uri": {{ subject_uris_list[loop.index0]|default('')|json_encode|raw }},
            "label": {{ subject|json_encode|raw }}
        }
        {{ not loop.last ? ',' : '' }}
        {% endfor %}
    ],
{%- endif -%}
```

**Use case #3:** I have `dc.creator` and `dc.contributor` columns in my AMI Set Source Data CSV with simple JSON-encoded values (e.g. source column cells contain `["Name 1, Name 2"]`) that I would like to map to the Archipelago default `creator_lod` JSON key.

**Twig Recipe Card for Use Case #3:**

```twig  
{% if data['dc.creator']|length > 0 or data['dc.contributor']|length > 0 %}
    {% set total_creators = (data["dc.creator"]|length) + (data["dc.contributor"]|length) %}
    {% set current_creator = 0 %}                
    "creator_lod": [
        {% for creator in data["dc.creator"] %}
            {% set current_creator = current_creator + 1 %}
            {% set creator_source = data["dc.creator"][loop.index0] %}
        {
            "name_uri": null,
            "agent_type": null,
            "name_label": {{ creator|json_encode|raw }},
            "role_label": "Creator",
            "role_uri": "http://id.loc.gov/vocabulary/relators/cre"
        }
        {{ current_creator != total_creators ? ',' : '' }}
        {% endfor %}
        {% for creator in data["dc.contributor"] %}
            {% set current_creator = current_creator + 1 %}
            {% set creator_source = data["dc.contributor"][loop.index0] %}
        {
            "name_uri": null,
            "agent_type": null,
            "name_label": {{ creator|json_encode|raw }},
            "role_label": "Contributor",
            "role_uri": "http://id.loc.gov/vocabulary/relators/ctb"
        }
        {{ current_creator != total_creators ? ',' : '' }}
        {% endfor %}   
 ],
{% endif %}  
```
**Use Case #4:** I have a mix of different columns containing Creator/Contributor/Other-Role-Types Name values with or without corresponding URI values that I would like to map to the default Archipelago `creator_lod` JSON key.

**Twig Recipe Card for Use Case #4:**

??? info "Click to view the full Recipe Card"
     
    ```twig
    {#- START Names from LoD and MODS CSV with/without URIS. -#} 
        {# Updated August 26th 2022, by Diego Pino. New checks/logic for mods_name_type_role_composed_or_more_namepart
        - Check first IF for a given namepart there is already reconciliaton. 
        - IF not i check if there is a matching valueuri, 
        - If not leave the URL empty and use the value in the namepart (label) only?
        - Only check/use mods_name_corporate/personal_namepart field IF there are no other fields
        - That specify Roles. Since normally in ISLANDORA that field (no role) is a Catch all names one
        - And in that case USE creator as the default ROLE
        #}
        {%~ set creator_lod = [] -%} 
        {# Used to keep track of parts after the type (corporate, etc) that are no roles
         but authority properties. Add more if you find them #}
        {%  set roles_that_are_no_roles = ['authority_naf','authority_marcrelator',''] %}
        {# Used to keep track of the ones that are reconciled already #}
        {%- set name_has_creator_lod = [] -%}
        {%- for key,value in data_lod -%}
          {%- if key starts with 'mods_name_' and key ends with '_namepart' -%}
          {# If there is mods_name_SOMETHING_namepart in data_lod we keep track so we 
          do not try afterwards to use that Sources KEY from the CSV.
          #}
            {%- set name_has_creator_lod = name_has_creator_lod|merge([key]) -%}
          {# Now we remove 'mods_name_' and '_namepart' #}
            {%- set name_type_and_role = key|replace({'mods_name_':'', '_namepart':''}) -%}
          {# We will only target personal or corporate. If any of those are missing we skip? #}
            {% set name_type = null %}
            {%- if name_type_and_role starts with 'personal_' -%}
               {% set name_type = 'personal' %}
            {%- elseif name_type_and_role starts with 'corporate_' -%}
          	   {%- set name_type = 'corporate' -%}
            {%- endif -%}
            {%- if name_type is not empty -%}
            {#- Now we remove 'type', e.g 'corporate_' -#}
              {%- set name_role = name_type_and_role|replace({(name_type ~ '_'):''}) -%}
              {# in case the name_role contains one of roles_that_are_no_roles, e.g
              something like `creator_authority_marcrelator` we remove that #}
              {% for role_that_is_no_role in roles_that_are_no_roles %}
                 {%- set name_role = name_role|replace({(role_that_is_no_role):''}) -%}
              {% endfor %}
              {# After removing all what can not be a role if we end with an empty #}
              {% if name_role|trim|length == 0 %}
                 {%- set name_role = "creator" %}
              {% else %}
                {%- set name_role = name_role|replace({'\\/':'//' , '_':' '})|trim -%}
              {% endif %}
            {#- we iterate over all possible vocabularies and fetch the reconciliated names from them (if any) -#}
              {%- for approach, names in value -%} 
            {#- if there are actually name pairs (name and uri) that were reconciliated we use them -#}
                {%- if names|length > 0 -%}
                  {#- we call the ami_lod_reconcile twig extension with the role label using the LoC Relators endpoint in english and get 1 result -#}
                  {%- set role_uri = ami_lod_reconcile(name_role|lower|capitalize,'loc;relators;thing','en',1) -%}
                  {#- for each found name pair in a list of possible LoD reconciliated elements we generate the final structure that goes into "creator_lod" json key -#}
                  {%- for name in names -%} 
                    {%- set creator_lod = creator_lod|merge([{'role_label': name_role|lower|capitalize, 'role_uri': role_uri[0].uri, "agent_type": name_type, "name_label": name.label, "name_uri": name.uri}]) -%}     
                 {%- endfor -%}
                {%- endif -%}
              {%- endfor -%}
            {% endif -%}
          {%- endif -%} 
        {%- endfor -%}
        {# Now go for the RAW CSV data for names #}
        {%- for key,value in data -%}
        {# here we skip values previoulsy fetched from LoD and stored in name_has_creator_lod #}
          {%- if key not in name_has_creator_lod and key starts with 'mods_name_' and key ends with '_namepart' -%}
          {# If there is mods_name_SOMETHING_namepart in data_lod we keep track so we 
          do not try afterwards to use that Sources KEY from the CSV.
          #}
            {%- set name_has_creator_lod = name_has_creator_lod|merge([key]) -%}
          {# Now we remove 'mods_name_' and '_namepart' #}
            {%- set name_type_and_role = key|replace({'mods_name_':'', '_namepart':''}) -%}
          {# We will only target personal or corporate. If any of those are missing we skip? #}
            {%- set name_type = null -%}
            {%- if name_type_and_role starts with 'personal_' -%}
               {%- set name_type = 'personal' -%}
            {%- elseif name_type_and_role starts with 'corporate_' -%}
          	   {%- set name_type = 'corporate' -%}
            {%- endif -%}
            {% if name_type is not empty %}
            {# Now we remove 'type', e.g 'corporate_' #}
              {%- set name_role = name_type_and_role|replace({(name_type ~ '_'):''}) -%}
              {# in case the name_role contains one of roles_that_are_no_roles, e.g
              something like `creator_authority_marcrelator` we remove that #}
              {% for role_that_is_no_role in roles_that_are_no_roles %}
                 {%- set name_role = name_role|replace({(role_that_is_no_role):''}) -%}
              {% endfor %}
              {# After removing all what can not be a role if we end with an empty #}
              {% if name_role|trim|length == 0 %}
                 {%- set name_role = "creator" %}
              {% else %}
                {%- set name_role = name_role|replace({'\\/':'//' , '_':' '})|trim -%}
              {% endif %}
              {# Now we check if there is a corresponding _valueuri for this #}
              {% set name_uris = [] %}
              {%- if data[('mods_name_' ~ name_type_and_role ~ '_valueuri')] is not empty 
              and data[('mods_name_' ~ name_type_and_role ~ '_valueuri')] != '' -%}
                {%- set name_uris = data[('mods_name_' ~ name_type_and_role ~ '_valueuri')]|split('|@|') -%}
              {%- endif -%}
              {%- set role_uri = ami_lod_reconcile(name_role|lower|capitalize,'loc;relators;thing','en',1) -%}
            {#- we split and iterate over the value of the mods_name key -#}
            {# NOTE. THIS IS TARGETING Anything after the year 1000, or 2000 #}
              {%- for index,name in value|replace({'|@|1':', 1', '|@|2':', 2', '|@|-':', -'})|split('|@|') -%}
                {%- if name is not empty and name|trim != '' -%}
                  {%- set name_uri = null -%}
                  {# Here we can check if one of the names IS not a name (e.g a year? #}
                  {#- we call the ami_lod_reconcile twig extension with the role label using the LoC Relators endpoint in english and get 1 result -#}
                  {%- if name_uris[index] is defined and name_uris[index] is not empty -%}
                     {%- set name_uri = name_uris[index] -%}
                  {%- endif -%}
                  {%- set creator_lod = creator_lod|merge([{'role_label': name_role|lower|capitalize, 'role_uri': role_uri[0].uri, "agent_type": name_type, "name_label": name, "name_uri": name_uri}]) -%}
                {%- endif -%}
              {%- endfor -%}
            {%- endif -%}
          {%- endif -%}
        {%- endfor ~%}
        {# Use reduce filter + other logic for depulicating #}
    	{% set creator_lod = creator_lod|reduce((unique, item) => item in unique ? unique : unique|merge([item]), []) %}
        "creator_lod": {{ creator_lod|json_encode|raw -}},
        {#- END Names from LoD and MODS CSV with/without URIS. -#}
    ```

**Use Case #5:** I have geographic location information that I would like to reconciliate against Nominatim and map into the default Archipelago 'geographic_location' key. I have AMI Source Data CSVs which contain values/labels and some which contain coordinates.

**Twig Recipe Card for Use Case #5 with variation notes:**

```twig
{#- <-- Geographic Info and terms:
  Includes options for geographic info for:
  - Nominatim lookup by value/label
  - Nominatim lookup by coordinates 
  -#}
    {#- use value for Nominatim search -#}
    {% if data.mods_subject_geographic|length > 0 %}
      {% set nominatim_from_label = ami_lod_reconcile(data.mods_subject_geographic,'nominatim;thing;search','en') -%}
    "geographic_location": {{ nominatim_from_label|json_encode|raw }},
    {% endif %}
    {#- use coordinates for Nominatim search, if provided -#}
    {% if data.mods_subject_cartographics_coordinates|length > 0 %}
      {% set nominatim_from_coordinates = ami_lod_reconcile(data.mods_subject_cartographics_coordinates,'nominatim;thing;reverse','en') -%}
    "geographic_location": {{ nominatim_from_coordinates|json_encode|raw }},
    {% endif %}
{#- Geographic Info and terms --> #}  
```

**Use Case #6:** I have date values in a `dc.date` column that contain instances of 'circa' or 'Circa' where I would like to replace with the EDTF-friendly '~' instead and map to the Archipelago default 'date_created_edtf' JSON key.

**Twig Recipe Card for Use Case #6:** 

```twig
    {% if data['dc.date'] is defined %}
    	{% set datecleaned = data['dc.date']|replace({"circa ":"~", "Circa ":"~"}) %}
    	"date_created_edtf": {
        "date_to": "",
        "date_free": {{ datecleaned|json_encode|raw }},
        "date_from": "",
        "date_type": "date_free"
        },        
    {% endif %}
```

_More recipe cards will be added over time. Please see our [Archipelago Contribution Guide](giveortake.md) to learn about contributing your own recipe card or other documentation._

___

Thank you for reading! Please contact us on our [Archipelago Commons Google Group](https://groups.google.com/forum/#!forum/archipelago-commons) with any questions or feedback.

Return to the [Archipelago Documentation main page](index.md).