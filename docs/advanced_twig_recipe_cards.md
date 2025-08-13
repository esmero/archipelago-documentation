---
title: Advanced Twig Recipe Cards
tags:
  - Twig
  - Twig Filters
  - Twig Functions
  - Twig Templates
  - Examples
  - JSON
---

# Advanced Twig Recipe Cards

The Twig Recipe Cards below reference more advanced Metadata transformation, display, or other use cases/needs you may have in your own Archipelago repository. Please review our [Twig Recipe Cards for Common Use Cases](twig_recipe_cards.md) before diving into these more advanced Twig recipe cards.

## Twig Macros

Archipelago's default Object Description Metadata Display (Twig) template includes 4 different helpful Macros that enable you to use a standard approach for outputting different metadata elements in the same way without the repeating all of the same Twig logic over many lines for every metadata element you want to reference--you can instead call a onetime-defined Macro in a single line in your Twig template for different elements.

To use any of the following Twig Macros, you **need to first place a defined Macro at the top of the Twig template you are using for your specific output**, then call the Macro in the template below that as needed for particular Metadata Elements.

When calling the Macro in your Twig template, follow the patterns shown in the "Example Macro Usage for a Metadata Element" below each Main Twig Recipe Card section.

### Default Twig Macro 1 - Simple HTML Output 

**Main Twig Recipe Card for Default Twig Macro #1:**

```twig
{%- macro html_output(html_title, source_datum, list_wrapper = "p", markdown = false) -%} 
  {# -- Macro note: if markdown is selected no wrapper will be used -#} 
  {%- if source_datum and source_datum|length > 0 -%} 
    {%- if html_title|length > 0 -%} 
<h3>{{html_title}}</h3> 
    {%- endif -%} 
	{%- if source_datum is iterable -%} 
      {%- for source_data in source_datum -%} 
        {%- if markdown -%} 
		  {{ source_data|markdown_2_html }} 
        {%- else -%} 
          <{{list_wrapper}}>{{ source_data|striptags }}</{{list_wrapper}}> 
        {%- endif -%} 
      {%- endfor -%} 
	{%- else -%} 
      {%- if markdown -%} 
          {{ source_datum|markdown_2_html }} 
      {%- else -%} 
		  <{{list_wrapper}}>{{ source_datum|striptags }}</{{list_wrapper}}> 
      {%- endif -%} 
    {%- endif -%} 
  {%- endif -%} 
{%- endmacro html_output -%} 
```

This Macro enables you to define an `html_title` variable to be used for the heading  display of a metadata element defined as `source_datum` (notation of `data.element` when you call the Macro), then checks the metadata element's value for iterability (multiple versus single values), and lastly checks whether the output should be shown using a `list_wrapper` (defined as the `<p>` tag in the Macro itself) or using Markdown syntax. 

If you specify "markdown = true" when you call this Macro, no `list_wrapper` will be used on the display output. The value(s) for the defined element will be passed through the 'markdown_2_html' Twig filter before being displayed. 

If you specify "markdown = "false" when you call this Macro, the value(s) for the defined element will be passed through the '|striptags' Twig filter before being displayed.

**Example Macro Usage for a Metadata Element**

```twig
{{ _self.html_output("Title", data.label) }}
```

This Twig snippet will use "Title" for the heading  display of the `data.label` metadata element.

![Macro 1 Output Example](images/Macro1_OutputExample.png)


**Second Example Macro Usage for a different Metadata Element**

```twig
{{ _self.html_output('Description', data.description, '', true) }} 
```

This Twig snippet will use "Description" for the heading  display of the `data.description` metadata element, and will use the 'markdown_2_html' Twig filter against the value(s) contained in element.
  
![Macro 1b Output Example](images/Macro1b_OutputExample.png)


### Default Twig Macro 2 - Simple HTML Output as List 

**Main Twig Recipe Card for Default Twig Macro #2:**

```twig
{%- macro html_output_list(html_title, source_datum, markdown = false) -%} 
  {# -- Macro note: if markdown is selected no wrapper will be used -#} 
  {%- if source_datum and source_datum|length > 0 -%} 
    {%- if html_title|length > 0 -%} 
<h3>{{html_title}}</h3> 
    {%- endif -%} 
  <ul> 
	{%- if source_datum is iterable -%} 
	  {%- for source_data in source_datum -%} 
        {%- if markdown -%} 
	      <li>{{ source_data|markdown_2_html }}</li> 
        {%- else -%} 
          <li>{{ source_data }}</li> 
        {%- endif -%} 
	  {%- endfor -%} 
	{%- else -%} 
      {%- if markdown -%} 
        <li>{{ source_datum|markdown_2_html }}</li> 
      {%- else -%} 
		<li>{{ source_datum }}</li> 
      {%- endif -%} 
    {%- endif -%} 
  </ul> 
  {%- endif -%} 
{%- endmacro html_output_list -%} 
```
This Macro enables you to define an `html_title` variable to be used for the heading  display of a metadata element defined as `source_datum` (notation of `data.element` when you call the Macro), then checks the metadata element's value for iterability (multiple versus single values), and lastly checks whether the output should display values with or without using Markdown syntax. 
  
If you specify "markdown = true" when you call this Macro, the value(s) for the defined element will be passed through the 'markdown_2_html' Twig filter before being displayed.

**Example Macro Usage for a Metadata Element**

```twig
{{ _self.html_output_list('Publisher', data.publisher) }} 
```

This Twig snippet will use "Publisher" for the heading  display of the `data.publisher` metadata element.
  
![Macro 2 Output Example](images/Macro2_OutputExample.png)  
  

### Default Twig Macro 3 - HTML Output Search (Non-LoD Elements)

This Macro requires you to set a variable for a `search_facet`. See this guide to learn about [Strawberry Key Name Providers, Solr Field, and Facet Configuration](strawberry_key_name_providers.md). You need to complete the full Strawberry Key Name Provider -> Solr Field -> Facet  configuration for a specified metadata element (JSON Key) before you can effectively use this Macro.

**Main Twig Recipe Card for Default Twig Macro #3:**

```twig  

{%- macro html_output_search(html_title, source_datum, search_facet) -%} 
  {%- if source_datum and source_datum|length > 0 -%} 
    {%- if html_title|length > 0 -%} 
<h3>{{html_title}}</h3> 
    {%- endif -%} 
  <ul> 
	{%- if source_datum is iterable -%} 
	  {%- for source_data in source_datum -%} 
	  <li><a href="/search?search_api_fulltext=&f%5B0%5D={{search_facet}}%3A{{ source_data|url_encode }}" target="_blank" >{{ source_data }}</a></li> 
	  {%- endfor -%} 
	  {%- else -%} 
	  <li><a href="/search?search_api_fulltext=&f%5B0%5D={{search_facet}}%3A{{ source_datum|url_encode }}" target="_blank" >{{ source_datum }}</a></li> 
	  {%- endif -%} 
  </ul> 
  {%- endif -%} 
{%- endmacro html_output_search -%}
```

This Macro enables you to define an `html_title` variable to be used for the heading display of a metadata element defined as `source_datum` (notation of `data.element` when you call the Macro), define another variable of `search_facet` to use for the internal search facets URL pattern, then checks the metadata element's value for iterability (multiple versus single values).

**Example Macro Usage for a Metadata Element**

```twig
{{ _self.html_output_search("Local Subjects", data.subjects_local, "descriptive_metadata_subjects") }}
```

This Twig snippet will use "Local Subjects" for the heading  display of the `data.subjects_local` metadata element. 

![Macro 3 Output Example](images/Macro3_OutputExample.png)

The search generated will use the defined facet variable of "descriptive_metadata_subjects" in the URL pattern:
'~/search?search_api_fulltext=&f%5B0%5D=descriptive_metadata_subjects%3ADogs'

### Default Twig Macro 4 - HTML Output Search (LoD Elements with Labels and URIs) 

This Macro requires you to set a variable for a `search_facet`. See this guide to learn about [Strawberry Key Name Providers, Solr Field, and Facet Configuration](strawberry_key_name_providers.md). You need to complete the full Strawberry Key Name Provider -> Solr Field -> Facet  configuration for a specified metadata element (JSON Key) before you can effectively use this Macro.

**Main Twig Recipe Card for Default Twig Macro #4:**

```twig
{# Use for LoD elements with URI and label values#} 
{%- macro html_output_search_lod(html_title, source_datum, search_facet) -%} 
  {%- if source_datum and source_datum|length > 0 -%} 
    {%- if html_title|length > 0 -%} 
<h3>{{html_title}}</h3> 
    {%- endif -%} 
  <ul> 
	  {%- for source_data in source_datum -%} 
	  <li><a href="/search?search_api_fulltext=&f%5B0%5D={{search_facet}}%3A{{ source_data.label|url_encode }}" target="_blank" >{{ source_data.label }}</a></li> 
	  {%- endfor -%} 
  </ul> 
  {%- endif -%} 
{%- endmacro html_output_search_lod -%} 
```

This Macro enables you to define an `html_title` variable to be used for the heading display of a metadata element defined as `source_datum` (notation of `data.element` when you call the Macro), define another variable of `search_facet` to use for the internal search facets URL pattern, then checks the metadata element's value for iterability (multiple versus single values).

**Example Macro Usage for a Metadata Element**

```twig
{{ _self.html_output_search_lod("Library of Congress Subjects", data.subject_loc, "descriptive_metadata_subjects") }} 
```

This Twig snippet will use "Library of Congress Subjects" for the heading  display of the `data.subject_loc` metadata element--specifically for the `data.subject_loc.label` value(s).

![Macro 4 Output Example](images/Macro4_OutputExample.png)

The search generated will use the defined facet variable of "descriptive_metadata_subjects" in the URL pattern:
'~/search?search_api_fulltext=&f%5B0%5D=descriptive_metadata_subjects%3ASuper%20Cool%20Dogs'

## Calling Drupal Views Within Twig Templates

_Recipe coming, check back soon!_
___

Thank you for reading! Please contact us on our [Archipelago Commons Google Group](https://groups.google.com/forum/#!forum/archipelago-commons) with any questions or feedback.

Return to the [Archipelago Documentation main page](index.md).
