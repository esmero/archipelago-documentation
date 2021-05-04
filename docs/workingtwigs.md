# Working with Twig in Archipelago

The following information can also be found in this Presentation from the "Twig Templates and Archipelago" Spring 2021 Workshop:
- [Twig Templates and Archipelago](https://docs.google.com/presentation/d/1qgYYESR1eMWuPXkpFwLnFiWXVZAf9Lhhaafq6Z3E138/edit?usp=sharing)

## Prequisites (with food analogy)

1. Know your Data/Metadata. What do I have? :thinking:
	- What do I have in my Fridge? Do I have Tofu? Do I have Peppermint :candy:? One Bunch?
2. Know your final desired output Document: MODS, HTML, GEOJSON, etc. 
	- What are you going to cook :cook:? Do you have a picture of the Curry :curry:? Have you ever had Curry :stew:?
3. Know your Twig Basics
	- How to cut and dice :knife:, steam and sauté :bowl_with_spoon:
4. Do not be afraid
	- You can’t get burned :fire: here and Ingredients :strawberry: do not expire!

#### Note about the Examples:
_All examples shown below are using the following JSON snipped from [Laddie the dog running in the garden, Bronx, N.Y., undated [c. 1910-1918?]](https://archipelago.nyc/do/17355bdb-d784-4037-96fe-5c160296e639)._

<details><summary>Click to view image of the JSON snippet.</summary>
  <span>

![LaddieJSON](../imgs/LaddieJSON.png)

</span>
</details> 

<details><summary>Click to view this snippet as JSON.</summary>
  <span>

```json
{
    "type": "Photograph",
    "label": "Laddie the dog running in the garden, Bronx, N.Y., undated [c. 1910-1918?]",
    "owner": "New-York Historical Society, 170 Central Park West, New York, NY 10024, 212-873-3400.",
    "rights": "This digital image may be used for educational or scholarly purposes without restriction. Commercial and other uses of the item are prohibited without prior written permission from the New-York Historical Society. For more information, please visit the New-York Historical Society's Rights and Reproductions Department web page at http:\/\/www.nyhistory.org\/about\/rights-reproductions",
    "language": [
        "English"
    ],
    "documents": [],
    "publisher": "",
    "ismemberof": "111",
    "creator_lod": [
        {
            "name_uri": "",
            "role_uri": "http:\/\/id.loc.gov\/vocabulary\/relators\/pht",
            "agent_type": "personal",
            "name_label": "Stonebridge, George Ehler",
            "role_label": "Photographer"
        }
    ],
    "description": "George Ehler Stonebridge (d. 1941) was an amateur photographer who lived and worked in the Bronx, New York.",
    "subject_loc": [
        {
            "uri": "http:\/\/id.loc.gov\/authorities\/subjects\/sh85038796",
            "label": "Dogs"
        }
    ],
    "date_created": "1910-01-01"
}
```

</span>
</details>  	

## First: Know Your Data
Understanding the basic structure of your JSON data. 

1. Single JSON Value.

	![KnowYourData1](../imgs/KnowYourData1.png)
	- For `"type": "Photograph"`
		- "type" = JSON Key or Property
		- "Photograph" = Single JSON Value (string)

2. Multiple JSON Values (Array of Enumeration of **Strings**)

	![KnowYourData2](../imgs/KnowYourData2.png)
	- For `"language": ["English","Spanish"]`
		- "language" = JSON Key or Property
		- "["English","Spanish"]" = Multiple JSON Values (Array of Enumeration of **Strings**)

3. Multiple JSON Values	(Array of Enumeration of **Objects**)

	![KnowYourData3](../imgs/KnowYourData3.png)
	- For `"subject_loc":[{"uri":"http://..","label":"Dogs"},{"uri":"http://..","label":"Pets"}]`
		- "subject_loc" = JSON Key or Property
		- [{"uri":"http://..","label":"Dogs"},{"uri":"http://..","label":"Pets"}] =
			- **Object** with two JSON Keys. Each one with a single Value
			- Multiple JSON Values (Array of Enumeration of **Objects**)

## Getting Started with the Twig Language in Archipelago

- **Data** is known as **Context** in Twig Lingo.

- **All your** JSON Strawberryfield Metadata is accessible inside a Variable named _data_ in your twig template. 

- You can access the _values_ by using `data DOT Property (attribute) Name`.
	- In the Laddie the Dog example shown above:
		- `data.type` will contain "Photograph"
		- `data.language` will contain [ "English" ]
		- `data.language[0]` will contain "English" 
			- 0 means first entry in an Array or Enumeration
		- `data.subject_loc` will contain [{ "uri":"http…","label": "Dog" }]
		- `data.subject_loc.uri` will contain `"http://id.loc.gov/…"`
		- `data.subject_loc.label` will contain "Dog"

#### Twig Statements and Printing 
- https://twig.symfony.com/doc/3.x/templates.html
- Simple examples using Printing Statements
	- Single JSON Value Example:
		- Twig template: 
		 ```twig
		 Hello I am a {{ data.type }} and very happy to meet you
		 ````
		- Rendered output:
			- `Hello I am a Photograph and very happy to meet you`
	- Multiple JSON Values Example:
		- Twig template:
		```twig
		Hello I was classified as "{{ data.subject_loc.label }}" and very happy to meet you
		````
		- Rendered Output:
			- `Hello I was classified as "Dogs" and very happy to meet you`

#### Twig Statements and Executing
- https://twig.symfony.com/doc/3.x/tags/if.html
- Rendered Output based upon different Twig `conditionals`, `operators`, `tests`, `assignments`, and `filters`

- Example 1:
	- Twig template:
		```twig
		{% if data.subject_loc is defined %}
		Hey I have a Subject Key
		{% else %}
		Ups no Subject Key
		{% endif %}
		```
	- Rendered Output:
		- `Hey I have a Subject Key`
	- Conditionals, Operator, and Test usage:
		- **If/else** are conditionals
		- **is** is an operator
		- **defined** is a test	

- Example 2:
	- Twig template:
		```twig
		`{% for key, subject in data.subject_loc %}
		* Subject {{ subject.label }} found at position {{ key }}
		{% endfor %}`
		````
	- Rendered Output:
		- * Subject Dogs found at position 0`
	- Loop usage:	
		- **for** is a loop
		- Inside the loop you have access to **key**, **subject**

- Example 3
	- Twig template:
		```twig
		`{% for subject in data.subject_loc %}
		{% set label_lowercase = subject.label|lower %}
		My lower case Subject is {{ label_lowercase }}
		{% endfor %}`
		````
	- Rendered Output:
		- `My lower case Subject is dogs`
	- Assignment, Filter, and Loop uage:	
		- **set** is an assignment 
		- | is a pipe, used after a value to apply a filter.
		- **lower** is a filter
		- Inside the loop you have have access to **subject** and **label_lowercase**

- Example 4:
	- Twig template: 
	```twig
	{% for subject in data.subject_loc %}
	  {% set label_lowercase = subject.label|lower %}
	My lower case Subject is {{ label_lowercase }}
	{% endfor %}
	{# 
	 This won’t display because it was assigned inside 
	 The For Loop
	#}
	{{ label_lowercase }}
	```
	- Rendered Output:
		- `My lower case Subject is dogs`

### Full Examples for Common Uses Cases:

**Use case #1:** I have multiple LoD Subjects and want to display them in my page as a clickable ordered list but i’m a safe/careful person.

**Twig Example for Use Case #1:**
```twig
{% if data.subject_loc is iterable and data.subject_loc is not empty %}
<h2>My Subjects</h2>
<ul>
   {% for subject in data.subject_loc %}
   <li>
      <a href=”{{ subject.uri }}” title=”{{ subject.label|capitalize }}” target=”_blank”>
      {{ subject.label }}
      </a>
   </li> 
   {% endfor %}
</ul>
{% endif %}
```
**Use case #2:** I have sometimes a publication date. I want to show it in beautiful human readable language.

**Twig Example for Use Case #2:**
```twig
{% if data.date_published is not empty %}
<h2>Date {{ data.label }} was published:</h2>
<p>
{{ data.date_published|date("F jS \\o\\f Y \\a\\t g:ia") }}
</p>
{% endif %}
```
**Use Case #3 (Full Curry):** {# May 4th 2021 @dpino: I have sometimes a user provided creation date. I want to show it in beautiful human readable language but fallback to automatic date if absent. I also want in the last case to show it was either “created” or “updated”. #}
```json
    "as:generator": {
        "type": "Update",
        "actor": {
            "url": "https:\/\/archipelago.nyc\/form\/descriptive-metadata",
            "name": "descriptive_metadata",
            "type": "Service"
        },
        "endTime": "2021-03-17T13:24:01-04:00",
        "summary": "Generator",
        "@context": "https:\/\/www.w3.org\/ns\/activitystreams"
    }
```

**Twig Example for Use Case #3:**
```twig
{% if data.date_created is not empty %}
<h2>Date {{ data.label }} was created:</h2>
<p>
  {{ data.date_created|date("F jS \\o\\f Y \\a\\t g:ia") }}
</p>
{% else %}
<h2>Date {{ data.label }} was {{ attribute(data, 'as:generator').type|lower }}d  in this repository:</h2>
<p>
  {{  attribute(data, 'as:generator').endTime|date("F jS \\o\\f Y \\a\\t g:ia") }}
</p>
{% endif %}
```

## And now it's your turn!

We hope you found the information presented here to be helpful in getting started working with Twigs in Archipelago. Click here to return to the main [Twigs in Archipelago documentation](../docs/metadatatwigs.md). Happy Twigging!

---

Thank you for reading! Please contact us on our [Archipelago Commons Google Group](https://groups.google.com/forum/#!forum/archipelago-commons) with any questions or feedback.

Return to the [Archipelago Documentation main page](../README.md).


