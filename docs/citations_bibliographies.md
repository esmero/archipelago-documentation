---
title: Citations and Bibliographies Guide
tags:
  - Citations
  - Bibliographies
  - citeproc-php
  - Citation Style Language
  - CSL
---

# Citations and Bibliographies Guide

As part of the [format_strawberryfield module](https://github.com/esmero/format_strawberryfield), we provide [citeproc-php](https://github.com/seboettg/citeproc-php) as a Twig extension and field formatter to generate citations and bibliographies using [Citation Style Language (CSL)](http://citationstyles.org/). To start using the library, please ensure that the dependencies are installed first.

??? important "Install citeproc-php dependencies"

    From the deployment server:
    
    ```shell
    docker exec -ti esmero-php bash -c 'drush archipelago-download-citeproc-dependencies'
    ```
    
    You'll see the following if the dependencies have not been previously installed:
    
    ```shell
    ! [NOTE] Citeproc-php dependencies are not present. Downloading and extracting them now.
    
    https://github.com/citation-style-language/styles/tarball/v1.0.2 [200 OK]
    
                                                                                    
     [OK] Files have been downloaded into /tmp/citeproc-styles.tar.gz directory.    
                                                                                    
    
    https://github.com/citation-style-language/locales/tarball/v1.0.2 [200 OK]
    
                                                                                    
     [OK] Files have been downloaded into /tmp/citeproc-locales.tar.gz directory.   
    ```

    If the dependencies were previously installed, and something went wrong, you can install them again:
    
    ```shell
     ! [NOTE] Citeproc-php dependencies are present.                                
    
     Download and extract libraries again?:
      [0] Cancel
      [1] Yes
    ```

## Usage

### Twig Filter

1. Familiarize yourself Twig Templates in Archipelago:
    * [Twig Templates in Archipelago](metadatatwigs.md)
    * [Working with Twig in Archipelago](workingtwigs.md)
2. Select a style or styles from the [Citation Styles Reference table](citation_styles.md).
3. If language support is needed provide the locale code from the [Citation Locales Reference table](citation_locales.md). Otherwise, provide a blank string `''` for the default of `English US` for the locale argument.
4. Familiarize yourself with the [Citation Schema Reference](citation_schema.md) for structuring a bibliography.
3. Use the below example as guide to build a bibliography in a Twig Template:

    !!! example "Twig Bibliography Example"
 
        ```html+twig hl_lines="1 25 26 36 37"
        {# First we structure the metadata to be processed by the filter. #}
        {% if data.creator|length > 0 or data.creator_lod|length > 0 %}
          {% if data.creator_lod %}
            {% if data.creator_lod is iterable %}
              {% set authors %}
                [
       	          {% for creator in data.creator_lod %}
       	            {% set name_array = creator.name_label | split(',') %}
                    {% set family_name = name_array[0] %}
                    {% set given_name = name_array[1] %}
                    {
                      "family": "{{ family_name }}",
                      "given": "{{ given_name }}"
                    }
                    {{ not loop.last ? ',' : '' }}
       	          {% endfor %}
                ]
              {% endset %}
            {% else %}
       	      {{ data.creator }}
            {% endif %} 
          {% endif %}
        {% endif %}

        {# Below is the structure of a basic input item. See the schema referenced above for more advanced usage. #}
        {# You'll notice that it's an array, which means you can pass multiple items. #}
        {% set book %}
          [{
            "author": {{ authors }},
            "id": "{{ data.label }}",
            "title": "{{ data.label }}",
            "type": "{{ data.type }}"
          }]
        {% endset %}

        {# Now we pass the data to the filter with the appropriate locale and style(s). #}
        {{ book | bibliography('en', ['chicago-fullnote-bibliography','modern-language-association']) }}

        ```

### Field Formatter

1. Familiarize yourself with [Strawberryfield Formatters](strawberry-field.md).
1. Familiarize yourself Twig Templates in Archipelago:
    * [Twig Templates in Archipelago](metadatatwigs.md)
    * [Working with Twig in Archipelago](workingtwigs.md)
2. Select a style or styles from the [Citation Styles Reference table](citation_styles.md).
3. If language support is needed provide the locale code from the [Citation Locales Reference table](citation_locales.md). Otherwise, provide a blank string `''` for the default of `English US` for the locale argument.
4. Familiarize yourself with the [Citation Schema Reference](citation_schema.md) for structuring a bibliography.
2. Create a Twig Template to generate the JSON data for the formatter:

    !!! example "JSON Data Template for Bibliography Field Formatter"
 
        ```html+twig hl_lines="1 3 27 28 39"
        {# Use this example to generate data for the Bibliography Field Formatter #}

        {# First we structure the metadata to be processed by the formatter. #}
        {% if data.creator|length > 0 or data.creator_lod|length > 0 %}
       	  {% if data.creator_lod %}
       	    {% if data.creator_lod is iterable %}
              {% set authors %}
                [
       	          {% for creator in data.creator_lod %}
       	            {% set name_array = creator.name_label | split(',') %}
                    {% set family_name = name_array[0] %}
                    {% set given_name = name_array[1] %}
                      {
                        "family": "{{ family_name }}",
                        "given": "{{ given_name }}"
                      }
                    {{ not loop.last ? ',' : '' }}
       	          {% endfor %}
                ]
              {% endset %}
       	    {% else %}
       	      {{ data.creator }}
            {% endif %} 
       	  {% endif %}
        {% endif %}

        {# Below is the structure of a basic input item. See the schema referenced above for more advanced usage. #}
        {# You'll notice that it's an array, which means you can pass multiple items. #}
        {% set book %}
          [{
            "author": {{ authors }},
            "id": "{{ data.label }}",
            "title": "{{ data.label }}",
            "type": "{{ data.type }}",
            "locale": "en"
          }]
        {% endset %}

        {# Now we output the JSON set above to pass to the formatter. #}
        {{ book }}
        ```

4. Save the template as `JSON` for the `Primary mime type this Twig Template entity will generate as output.`
5. Create a new Display Suite `Content (Digital Object) - 🍓 Strawberry (Descriptive Metadata source)` or use an existing one.
6. Enable the field under `Manage display` for the appropriate `View mode` and save.
7. Select the `Strawberry Field Simple Citation Formatter` for the enabled Field's Formatter.
8. Select the `Operations` (small gear to the right of the field) and complete the form:
    * The metadata Twig template created in step 2.
    * The citation style(s) you'd like to display.
    * The key for the locale, e.g. the `locale` key set in the above example.
9. Press `Update`, and then `Save` at the bottom.
