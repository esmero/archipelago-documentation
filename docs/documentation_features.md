---
title: Additional Documentation Features
tags:
  - Documentation
  - Contributing
  - Examples
---

# Additional Documentation Features

Below are some examples of features that are currently in use on the site. To explore more visit the [Material for MkDocs documentation](https://squidfunk.github.io/mkdocs-material/reference).

## Examples

### Images

Images are located in the `docs/images` folder. You can add new ones there and link to them by relative path. For example, if you added `strawberries_color.png`, you would embed it like so:

!!! example "Image"

    === "Markup"
    
        ```markdown
        ![New Documentation Image](images/strawberries_color.png)
        ```

    === "Result"
    
        ![New Documentation Image](images/strawberries_color.png)

### Admonitions

!!! example "Question Admonition"

    === "Markup"
    
        ```markdown
        ??? question "What is a collapsible admonition?"
         
            This is a collapsible admonition. It can have a title, and it collapses so as not to interrupt the flow the of the document, but it provides useful information as needed.
        ```
    
    === "Result"
    
        ??? question "What is a collapsible admonition?"
         
            This is a collapsible admonition. It can have a title, and it collapses so as not to interrupt the flow the of the document, but it provides useful information as needed.
    
You can read more about admonitions with further examples in the [Material for MkDocs documentation](https://squidfunk.github.io/mkdocs-material/reference/admonitions/).

### Code blocks

!!! example "Code block with title and highlighted lines"

    === "Markup"
    
        ````markdown
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
        ````
    
    === "Result"
    
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

### Quirks

Because of the use of front matter (the block of YAML at the top that contains settings and data for the file) the markup for a horizontal rule is restricted. To create one you have to use the following:

!!! example "Horizontal Rule"

    === "Markup"
    
        ```markdown
        ___
        ```
    
    === "Result"
    
        ___
    
!!! info
    The above are underscore (`_`) characters, as opposed to hyphens (`-`).

___

Some of the documentation that is [automatically deployed from the repos](documentation_technical.md) have special comments that are converted to theme-specific elements via script.


!!! example "Front Matter"

    === "Deployment Repo with Front Matter"
    
        ```markdown
        <!--documentation
        ---
        title: "Adding Demo Archipelago Digital Objects (ADOs) to your Repository"
        tags:
          - Archipelago Digital Objects
          - Demo Content
        ---
        documentation-->
        ```
    
    === "Documentation Repo with Front Matter"
    
        ```markdown
        ---
        title: "Adding Demo Archipelago Digital Objects (ADOs) to your Repository"
        tags:
          - Archipelago Digital Objects
          - Demo Content
        ---
        ```

!!! example "Switching Elements"

    === "Deployment Repo with Theme-specific Markup"
    
        ````markdown
        <!--switch_below
        
        ??? info "OSX (macOS)/x86-64"
        
            ```shell
            cp docker-compose-osx.yml docker-compose.yml
            ```
        
        ??? info "Linux/x86-64/AMD64"
        
            ```shell
            cp docker-compose-linux.yml docker-compose.yml
            ```
        
        ??? info "OSX (macOS)/Linux/ARM64"
        
            ```shell
            cp docker-compose-arm64.yml docker-compose.yml
            ```
        
        switch_below-->
        
        ___
        
        OSX (macOS)/x86-64:
        
        ```shell
        cp docker-compose-osx.yml docker-compose.yml
        ```
        
        ___
        
        Linux/x86-64/AMD64:
        
        ```shell
        cp docker-compose-linux.yml docker-compose.yml
        ```
        
        ___
        
        OSX (macOS)/Linux/ARM64:
        
        ```shell
        cp docker-compose-arm64.yml docker-compose.yml
        ```
        
        ___
        
        <!--switch_above
        switch_above-->
        ````
    
    === "Documentation Repo with Theme-specific Markup"
    
        ````markdown
        ??? info "OSX (macOS)/x86-64"
        
            ```shell
            cp docker-compose-osx.yml docker-compose.yml
            ```
        
        ??? info "Linux/x86-64/AMD64"
        
            ```shell
            cp docker-compose-linux.yml docker-compose.yml
            ```
        
        ??? info "OSX (macOS)/Linux/ARM64"
        
            ```shell
            cp docker-compose-arm64.yml docker-compose.yml
            ```
        
        <!--repo_docs
        
        ___
        
        OSX (macOS)/x86-64:
        
        ```shell
        cp docker-compose-osx.yml docker-compose.yml
        ```
        
        ___
        
        Linux/x86-64/AMD64:
        
        ```shell
        cp docker-compose-linux.yml docker-compose.yml
        ```
        
        ___
        
        OSX (macOS)/Linux/ARM64:
        
        ```shell
        cp docker-compose-arm64.yml docker-compose.yml
        ```
        
        ___
        
        repo_docs-->
        ````
