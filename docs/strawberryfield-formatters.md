# Strawberryfield Formatters

This documentation will give a brief overview of Archipelago's [Strawberryfield Formatters](https://github.com/esmero/format_strawberryfield) and how they work using the default View mode `Digital Object Full View` as an example.

## At a glance
When taking a look at your [First Digital Object](https://github.com/esmero/archipelago-documentation/blob/8.x-1.0-beta2/docs/firstobject.md) note that multiple formatters are working together to create this `Display` ( or `View mode`). Since "*My First Digital Object*" is a `Photograph` the `Display` being used is `Digital Object Full View` which, by default, uses formatters to:

- (**Red**) Create the image viewer where users can zoom in, zoom out, fullscreen and rotate all the images associated with the ADO.
- (**Blue**) Display the `Object Description` and `Type of Resource`.
- (**Green**) Display the Raw JSON Metadata and IIIF Presentation Manifest.

![Image Viewer](../imgs/strawberryfield-formatters/01_my-first-digital-object.jpg)


## Under the hood

When editing an ADO, at the top of the Webform page there is a tab titled `Manage display` which will take us to where all the Formatters live. **Take note** that the `DISPLAY SETTINGS` show we are using the *Default* View mode.

![Display Settings](../imgs/strawberryfield-formatters/02_managedisplay.jpg)

Once the page loads the `Default` View mode is automatically selected. However, because we are editing an object with the `Media type` `Photograph`, we need to actually need to edit the View mode `Digital Object Full View` since it is the *Default* View mode for this `Media type`.

<details><summary>How to find which View mode is Default per Media type</summary>
<div>

The **ADO Type to View mode Mapping** page tells the ADOs which View mode to use by default per Media type. This page can be accessed at */admin/config/archipelago/viewmode_mapping*.

</div>
</details></p>

![Selecting Digital Object Full View](../imgs/strawberryfield-formatters/03_default-managedisplay.jpg)

There are two sections in `Manage display` for `Digital Object Full View`: 1) **Content** and 2) **Disabled**. Moving a field into **Content** means this formatter will be used to the display the ADO in some way. The formatters moved to **Disabled** are inactive and are subsequently not being used for displaying the ADO.

There are four fields named `🍓Strawberry` and each one is a copy of the field `🍓Strawberry (Descriptive Metadata source)`. Since the names of the fields do not imply their function, they have been named *Strawberry* in four different ways (Italiano, Deutsch, Diné Bizaad, and English) in order to organize and help users visually remember which field is doing what for the `Display`.

Recall *My First Digital Object* at beginning of this document where there were 3 sections highlighted in **Red**, **Blue**, and **Green**.

- In **Red** (`🍓Fragola`) there is the [Strawberry Field Formatter for IIIF media](../formatter-for-iiif-media.md) which takes the image stored in S3 to display the photograph with the image viewer.
- In **Blue** (`🍓Erdbeere`) there is the [Strawberry Field Formatter for Custom Metadata Templates](../formatter-for-custom-metadata-templates.md) which displays the raw JSON metadata using configurable Twig templates. In this example, the default Twig template uses the JSON key `type` to display the `Type of Resource`.
- In **Green** (`🍓Strawberry (Descriptive Metadata)`) there is the [Strawberry Default Formatter](../strawberry-default-formatter.md) which is used to display the Raw JSON Metadata.

![Strawberry Fields](../imgs/strawberryfield-formatters/04_strawberryfields.jpg)

## At the end of the day
The decision for how your metadata is displayed is totally in your control.

Under the `WIDGET` column, there is a quick description/overview of what the formatter is doing.

![Widget](../imgs/strawberryfield-formatters/05_widget.jpg)

And by clicking on the gear icon under the `OPERATIONS` column, all of the options for configuring the formatter are revealed. To use `🍓Fragola` as an example (the Formatter for IIIF media), we can choose which JSON Key is being used to fetch the IIIF Media URLs (found inside the raw JSON being played with `Strawberry Default Formatter`), the maximum height and width of the viewer, etc.

![Fragola](../imgs/strawberryfield-formatters/06_fragola.jpg)

And then with `🍓Erdbeere` (the Formatter for Custom Metadata Templates) there is the option, among many others, to configure which Twig template the formatter will use for displaying your Metadata.

![Erdbeere](../imgs/strawberryfield-formatters/07_erdbeere.jpg)

Stay tuned for more in-depth documentation on all thirteen (13!) formatters shipped with Archipelago.
