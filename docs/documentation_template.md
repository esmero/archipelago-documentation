---
title: Example Documentation Page Title
tags:
  - Documentation
  - Contributing
  - Examples
---

# Example Documentation Page Title

A brief overview of the specific functionality or workflow area that will be covered in your documentation.

## Steps/Guides

1. Step one example.
2. Step two example with images:

    ![New Documentation Image](images/strawberries_color.png)

3. Step three example with Details section:

    ??? info "Click to open this Details Section"
 
        More Details in a List

        * Apples
        * Oranges
        * Peaches
        * Pumpkins

4. Step four example with a simple code block:

    ```html+twig
    {% for image in attribute(data, 'as:image')|slice(0,1) %}
     {% if image["flv:exif"] %}
       {% set height = image["flv:exif"].ImageHeight%}
     {% else  %}
       {% set width = 1200 %}
     {% endif %}
       {% set imageurl =  IIIFserverurl ~ image['url']|replace({'s3://':''})|replace({'private://':''})|url_encode %}
    <a href="{{ nodeurl }}" title="{{ data.label }}" alt="{{ data.label }}">
    <img src="{{ imageurl }}/pct:5,5,90,30/,400/0/default.jpg" class="d-block.w-auto" alt="{{ image.name }}" height="400px" style="min-width:1200px">
    </a> 
    {% endfor %}
    ```

5. Step five example with a code block that has a title and highlighted lines:

    ```html+twig title="HTML in a TWIG template" hl_lines="8 9 10"
    {% for image in attribute(data, 'as:image')|slice(0,1) %}
     {% if image["flv:exif"] %}
       {% set height = image["flv:exif"].ImageHeight%}
     {% else  %}
       {% set width = 1200 %}
     {% endif %}
       {% set imageurl =  IIIFserverurl ~ image['url']|replace({'s3://':''})|replace({'private://':''})|url_encode %}
    <a href="{{ nodeurl }}" title="{{ data.label }}" alt="{{ data.label }}">
    <img src="{{ imageurl }}/pct:5,5,90,30/,400/0/default.jpg" class="d-block.w-auto" alt="{{ image.name }}" height="400px" style="min-width:1200px">
    </a> 
    {% endfor %}
    ```

6. Last Step Example. 

Congratulations! 🎉
