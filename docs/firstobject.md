# Your First Digital Object [![Google Groups](https://img.icons8.com/wired/32/000000/google-groups.png)](https://groups.google.com/forum/#!forum/archipelago-commons)


You followed every [Deployment step](https://github.com/esmero/archipelago-deployment/blob/8.x-1.0-beta1/README.md) and you have now a local ``Archipelago`` instance. Great!

So what now? It is time to give your new Repository a try and we feel the best way is to start by ingest a simple Digital Object. 

Note: This guide will assume Archipelago is running on `http://localhost:8001`, so if you wizardly deployed everthing in a different location, please replace all `URIs` with your own setup while following this guide.

## Requirements
 - Running Archipelago (http://localhost:8001)
 - 20 minutes of your time.
 - Open Mind.

## Welcome!

Start by opening `http://locahost:8001` in your favourite Web Browser.

![Welcome](../imgs/Welcome.jpg)

Your Demo deployment will have a fancy Home page with some banners and a small explanation of what Archipelago is and can do. Feel free to read through that now or later.

Click on `Log in` and use your `demo` credentials from them deployment guide.

- User: demo
- pass: demo 

(or whatever password you decided was easy for you to remember during the deployment phase)

![Log In](../imgs/Log-in.jpg)

Press the `Log in` button.

![Logged In](../imgs/Logged-in.jpg)

Great, welcome `demo` user! This users has limited credentials and uses the same global Theme as any anonymous user would. Still, `demo` can create content, so let's use those super powers and give that a try.

You will see a new `Menu item` on the top, black, navigation bar named `Add Content`. Click it!

## Adding Content

### Brief Background

As you already know Archipelago is build on `Drupal 8/9`, a very extensible `CMS`. In practice that means you have (at least) the same functionality any Drupal deployment has and that is also true for Content Managment. 

Drupal ships by default with a very flexible `Content Entity Type` named `Node`. `Nodes` are used for creating Articles and simple Pages but also in Archipelago as `Digital Objects`. 
Drupal has a pretty tight integration with `Nodes` and that means you get a lot of fun and useful functionality by default by using them.

<details><summary>Want to know more about Entities?</summary>
<div>

An `Article` and a `Digital Object` are both of type `Nodes`, but each one represents a different `Content Type`. `Content Types` are also named `Bundles`. 
An individual Content, like "Black and White photograph of a kind Dog" is named a `Content Entity` or more specific in this case a `Node`.

What have `Article` and `Digital Object` Content types in common and what puts the apart?

- Each Content Type or Bundle has a set of `Base Fields` and also user configurable set of `Fields` attached (or bundled together).
  - E.g `Article` has a title, a Body and the option to add an image.
  - `Digital Object` has a title but also a special, very flexible one named `Strawberry Field` (more about that later).
- Fields are where you put your data into and also where your data comes from when you expose it to the world.
  - `Nodes`, as any other Content entity have Base Fields (which means you can't remove or configure them) that are used all over the place. Good examples are the `title` and also the owner, named `uid` (you!).
  - Other Fields, specific to a Content Type, can be added and configured per Bundle. 
  - A `Field Widget` is used to input data into a Field.
  - Each field can have a `Field Formatter` that allows you to setup how it is displayed to the World.
  - A set of `Field Formatters` (the way you want to show your content formatted to the world) is named a `Display Mode`. You can have many, create new ones and remove them, but only use one at the time.
  - A set of `Field Widgets` (the way you want to Create and Edit a `Node`) is named a `Form Mode`. You can also have many, create new ones but only use one at the time.
  
- Each Content Type can have different Permissions (using the build in `User Roles` system).
- Each Content Type can have one or more `Display Modes`. In Practice this means `Display Modes` are attached to `Content Types`.
  - Each display modes can have its own Permissions 
- Each Content Type can have one or more `Form Modes`. In Practice this means `Form Modes` are attached to `Content Types`.
  - Each `Form Mode` can have its own Permissions.

There is of course a lot more to Nodes, Content Types, Formatters, Widgets and in general [Content Entities](https://www.drupal.org/docs/user_guide/en/planning-data-types.html) but this is a good start to understand what will happen next.

</div>
</details>

![Add Content](../imgs/Add-content.jpg)

Here you see all the `Content Types` defined by default in Archipelago. Yes, you got it right, go on and click on `Digital Object` to get your first Digital Object Node.

## That first Digital Object

### My Metadata

What you see here is a `Form Mode` in action, with the Title field exposed as a simple text input (`Field Widget`), a Multi Step Web Form that will ingest metadata into a field of type Strawberry Field (where all the magic happens) 
attached to that field using a `Webform Field Widget`, an editorial/advanced Block on the right side and a Workflow/state drop down (`Save as`) at the bottom. 

![Ingest Step1](../imgs/MyMetadata_1.jpg)

Fill the form out. We recommend to use similar values as the ones shown in the screen capture to make following the tutorial easier. 

<details><summary>Why does this look different than Repository X?</summary>
<div>

We assume you come from a world where repositories define different Content types and the shape, the fields and values (Schema) are fixed and set by someone else or at least quite complicated to configure. This is where Archipelago differs and starts to propose its own style. You noticed that there is a single Content Type named `Digital Object` and you have here a single Web Form. So how does this allows you to have Images, sequences, Videos, audio, 3D images, etc? 

There are many ways of answering that, archipelago works under the idea of an (or many) Open Schema(s), and that notion permeates the whole environment. Practical answers, simplest way to explain this based on this demo is:

- The `Digital Object` is a generic container for any shape of metadata. Metadata is generated either via this Webform based widget, manually (no worries, power-user need only) or via APIs and can take because of that any shape to express your needs of Digital Objects. You really don't need many types of Digital Objects (trust us!). If you ever need them, those are easy to setup.
- The Strawberry field Field Widget allows you to attach any `Webform`, built using the [`Webform Module`](https://www.drupal.org/docs/8/modules/webform) and Webforms can be setup in almost infinite ways. Any field, combo, style can be used. Multi Step, single step. We made sure they always only touch/modify data they know how to touch, so even a single input element webform would ensure any previous metadata to persist even if not readable by itself (See the potential?). And Each Webform can be also quite smart!
- The Strawberry field Field Widget will take all your Webform input, process any uploaded files, generate a JSON representation, enrich and complement it with Archipelago specific data and save it for you inside the `Strawberry field`.

We will come back to this later. 

</div>
</details>

Make sure you select `Photograph` as `Media Type` and all the fields with a red `*` are filled up. Press `Next: Linked data`. That will load the next step inline. 

Note: Don't press `Save`, Don't press `Preview`. Why? You are not ready yet!

### Linked Data

![Ingest Step2](../imgs/ingest-step2.jpg)

As the name of this step suggests; on this page you will be adding all your Linked Data elements. This Step showcases some of the Autocomplete Linked Data Webform elements we built for Archipelago. We truly believe in Wikidata as an open, honest, source of Linked Open Data and also one where you can contribute back. But we also have LoC autocompletes and Getty.

Click: `Next: Collections`

### Collections

![Ingest Step3](../imgs/inges-step-3_Collections.jpg)

Since this is our first digital object we do not yet have a Digital Object *Collection* for which `My First Digital Object` could be a member of. In other words, you can leave `Collection Membership` blank and click `Next: Upload Files`.

### Upload Files

![Ingest Step4](../imgs/ingest-step-4_Upload-files.jpg)

This step is pretty straight forward: Click `Choose Files` to open your file selector window and choose which file you would like to ingest. After you've done this, click `Save Metadata`. 

Two things to note: The `Save Metadata` button simply persists the metadata in the current webform session. The actual ingest of the Object happens when you click `Save` (see the **Complete** step below). Secondly, each "upload field" has a set of allowed file types (such as .jpg, .tiff, .png) and in this demo there are also file size restrictions which are explained in the Upload Field's description.

From here, Steps 5 & 6 (hotspots & Preview) are skipped and you are directed to the **Complete** step.

### Complete

![Ingest Step7](../imgs/ingest-step-7_Complete.jpg)

You made it! You added your Metadata, Linked Data, Uploaded your files and now you're ready to save! First, give your object a preview by clicking the `Preview` button directly to the right of the `Save` button.

Does everything look alright? Great! Click the `Back to content editing` button in the top left corner to go through with saving your object.

![Preview-page-screenshot](../imgs/ingest-documentation_preview-page.png)

Now you are back to where you began, and you are ready to save your First Digital Object! **Be sure to switch your "Save As" status from *Draft* to *Publish*.** Once you hit `Save` you should see a few green push notifications indicating your object has been created. 

Like this:
![Ingest Step7 Saved](../imgs/ingest-step-7_Complete-save.jpg)

Congratulations on creating your first digital object!
