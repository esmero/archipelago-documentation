---
title: Twig Extensions
tags:
  - Twig
  - Twig Filters
  - Twig Functions
  - Twig Templates
  - Examples
  - JSON
  - Markdown
  - HTML
---

# Twig Extensions

One advantage of Drupal's integration of the [Twig](https://www.drupal.org/docs/theming-drupal/twig-in-drupal) template engine is the availability of extensions (filters and functions).

## Default Twig Extensions from Symfony

The [Symfony](https://symfony.com/) PHP framework, which is integrated into Drupal Core, provides extensions, which we use in our default templates:

* [Twig Filters from Symfony](https://twig.symfony.com/doc/2.x/filters/index.html)
* [Twig Functions from Symfony](https://twig.symfony.com/doc/2.x/functions/index.html)

## Default Twig Extensions from Drupal

Additionally, we have some very handy Drupal-specific extensions:

* [Twig Filters from Drupal](https://www.drupal.org/docs/theming-drupal/twig-in-drupal/filters-modifying-variables-in-twig-templates#s-drupal-specific-filters)
* [Twig Functions from Drupal](https://www.drupal.org/docs/theming-drupal/twig-in-drupal/functions-in-twig-templates)

## Default Twig Extensions from Archipelago

Finally, we have a growing list of extensions that apply to our own specific use cases:

### Twig Filters from Archipelago

!!! example "edtf_2_human_date"

    The `edtf_2_human_date` filter takes an EDTF date and an optional language code (defaults to English), and converts it to a human-readable format using the [EDTF PHP](https://github.com/ProfessionalWiki/EDTF) library. The list of language codes is available [here](https://github.com/ProfessionalWiki/EDTF/tree/master/i18n).

    Let's start with the following metadata fragment:
    ```json title="Metadata Fragment" hl_lines="5"
    ...
    "subject_wikidata": "",
    "date_created_edtf": {
        "date_to": "",
        "date_free": "~1899",
        "date_from": "",
        "date_type": "date_free"
    },
    "date_created_free": null,
    ...
    ```

    Then we pass the `date_free` field through the `trim` filter (as a precaution, in case there's any accidental whitespace), and then we finally hand off the field to our `edtf_2_human_date` filter:
    ```html+twig title="edtf_2_human_date" hl_lines="1 3"
    {{ data.date_created_edtf.date_free|trim|edtf_2_human_date('en') }}
    
    {# Output: Circa 1899 #}
    ```
    
!!! example "html_2_markdown"

    The `html_2_markdown` filter, as the name suggests, converts HTML to Markdown.

    We start with this string of HTML:
    ```html+twig title="HTML string"
    {% set html_string = "
      <ul>
        <li>One thing</li>
        <li>Another thing</li>
        <li>The last thing</li>
      </ul>
    " %}
    ```

    Then we pass it to the filter:
    ```html+twig title="html_2_markdown" hl_lines="1 3-7"
    {{ html_string | html_2_markdown }}
    
    {# Output:
      - One thing
      - Another thing
      - The last thing
    #}
    ```

!!! example "markdown_2_html"

    The `markdown_2_html` filter, as the name suggests, is the reverse of the above and converts Markdown to HTML.

    We start with this string of Markdown:
    ```html+twig title="Markdown string"
    {% set markdown_string = "
      - One thing
      - Another thing
      - The last thing
    " %}
    ```

    Then we pass it to the filter:
    ```html+twig title="markdown_2_html" hl_lines="1 3-7"
    {{ markdown_string | markdown_2_html }}
    
    {# Output:
      <ul>
        <li>One thing</li>
        <li>Another thing</li>
        <li>The last thing</li>
      </ul>
    #}
    ```

!!! example "sbf_json_decode"

    The `sbf_json_decode` filter decodes a JSON-encoded string.

    We start with this string of JSON string:
    ```html+twig title="JSON string"
    {% set json_string = "
      {
        \"date_to\": \"\",
        \"date_free\": \"~1899\",
        \"date_from\": \"\",
        \"date_type\": \"date_free\"
      }
    " %}
    ```

    Then we pass it to the filter:
    ```html+twig title="sbf_json_decode" hl_lines="1 3-6"
    {% json_decoded = json_string | sbf_json_decode %}
    
    {{ json_decoded.date_free }}
    {# Output:
      ~1899
    #}
    ```

### Twig Functions from Archipelago

!!! example "clipboard_copy"

    The `clipboard_copy` function generates a copy/paste HTML element based on the provided CSS class for the element(s) we'd like to copy.

    We start with this element that we'd like to be copied:
    ```html+twig title="HTML example" hl_lines="2"
    <div class="csl-bib-body-container chicago-fullnote-bibliography">
      <div class="csl-bib-body">
        <div class="csl-entry">
          New York Botanical Garden. “Descriptive Guide to the Grounds, Buildings and Collections.”
        </div>
      </div>
    </div>
    ```

    Then we pass it to the function:
    ```html+twig title="clipboard_copy" hl_lines="1"
    {{ clipboard_copy('csl-bib-body','','Copy Bibliography Entry') }}
    ```
    
    As you can see above, the `clipboard_copy` function takes three arguments:

    * a CSS class for the element to be copied
    * an optional CSS class (the default is `clipboard-copy-button`) for the copy button
    * optional text (the default is `Copy to Clipboard`) for the copy button

    The result for this example looks as follows:

    ![Copy/Paste Element](images/copy_paste_element.png)

!!! example "sbf_entity_ids_by_label"

    The `sbf_entity_ids_by_label` function, as the name suggests, provides a Drupal entity ID for the following Drupal entity types:

    * node
    * taxonomy_term
    * group
    * user 

    If we start with the user entity `jsonapi`, we can do the following:
    ```html+twig title="sbf_entity_ids_by_label" hl_lines="1 4 8"
    {% set jsonapi_user_ids=sbf_entity_ids_by_label('jsonapi','user','') %}

    {% for jsonapi_user_id in jsonapi_user_ids %}
      {{ jsonapi_user_id }}
    {% endfor %}

    {# Output:
      3
    #}
    ```

    As you can see above, the `sbf_entity_ids_by_label` function takes three arguments:

    * the entity label
    * the entity type (see above for supported types)
    * an optional entity bundle

    We then loop through the returned result, which is an array of IDs (in this case, just a single one).

!!! example "sbf_search_api"

    The `sbf_search_api` function executes a search API query against a specified index.

    ```html+twig title="sbf_search_api" hl_lines="1"
    {% set search_results=sbf_search_api('default_solr_index','strawberry',[],{'status':1},[]) %}
    {% set labels=search_results['results']['13']['fields']['label_2'] %}
    <ul>
      {% for label in labels %}
        <li>{{ label }}</li>
      {% endfor %}
    </ul>
    ```

    As you can see above, the `sbf_search_api` function takes eight arguments:

    * The machine name of the Search API index to search against (string)
    * A full text term to search (string)
    * An array of full text fields to search the term against. If empty all will be used. (array)
    * The fields => filters to match against (associative array)
    * The fields to facet (array)
    * The fields to sort against (associative array)
    * Offset for the results (int)

    For this example we end up with the following output:

    * JPEG File Interchange Format
    * Organic farming--United States
    * Strawberries
    * Strawberry Field at Thorpes Organic Family Farm
    * organic agriculture
    * strawberries
