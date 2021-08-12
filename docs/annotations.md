# Annotations in Archipelago

Archipelago extends [Annotorius](https://github.com/recogito/annotorious) to provide W3C-compliants WebAnnotations for Digital Objects. These Annotations can be added per image (when multiple), edited, and saved/discarded using the regular Edit mode (bonus: temp storage that persists when you log out and come back in your session). Archipelago also exposes a full API for WebAnnotations, that keeps track of which Images (referenced in the Strawberryfield) were annotated and creates the W3C valid entries inside the Digital Object's JSON.

### Enabling Annotations
1. Navigate to `https://yoursite.org/admin/structure/types/manage/digital_object/display/digital_object_viewmode_fullitem`
   (Admin --> Structure --> Content types --> Digital Object --> Manage Display and select the "Digital Object Full view" mode. 
	![annotations step 1](../imgs/annotations_step1.jpg)
2. On the “Fragola” row, click on the small gear icon on the far right, which will be open the configurations for this display type. Select the “Enable loading/editing of W3C webAnnotations” option. 	
	![annotations step 2](../imgs/annotations_step2.jpg)
   - *Learn more about the JSON format of WebAnnotations here: https://www.w3.org/TR/annotation-model/#index-of-json-keys.
3. Select whether you would like to use the Rectangular or Polygon (freehand drawing) tool for your annotation style.
4. Select the 'Update' button and Save your settings.

You are now ready to get started adding annotations!

### Adding and saving Annotations
1. Navigate to the image-based Digital Object you would like to apply annotations to.
	- _Please note: the Digital Object type must be setup to display the image file(s) using the Open SeaDragon viewer. Stay tuned for updates announcing web annotation integration for Mirador 3._
3. To add a new annotation, select and hold the Shift key. Click and then drag to apply either a Rectangular box or multi-point Polygon shape.
4. Double click to exit the annotation drawing mode.
5. Enter the text for your annotation in the pop-up window.
	![annotations edit delete save](../imgs/annotations_edit_delete_save.jpg)
5. Click the "Ok" button when you are ready.
6. To save your annotation (or annotations if you created multiple), navigate to the main content "Edit" tab, where you will see a message about Unsaved Web Annotation Changes.
	![annotations edit save](../imgs/annotations_edit_save.jpg)
7. Select "Save" to preserve your Annotation(s). They will now become part of your Digital Object's JSON, found under the `ap:annotationCollection` key.
	- Pressing the "Discard" button will discard only the unsaved Annotations, and will reload the page.

### Editing and Deleting Annotations
1. Navigate to the Image-based Digital Object you would whose Annotation(s) you want to edit or delete.
2. Click within the Annotation and select the downwards arrow in the upper right-hand corner of the pop-up window.
3. Select either the "Edit" option and Edit the Annotation as desired; Or select the "Delete" option.
	![annotations edit delete](../imgs/annotations_edit_delete.jpg)
4. To preserve your editing or deleting actions, navigate to the main content "Edit" tab, where you will see a message about Unsaved Web Annotation Changes. (see screenshot in Step 6 of the Adding and saving Annotations instructions above)
5. Select "Save" to preserve your Annotation(s) edits or deletions. Pressing the "Discard" button will discard only the unsaved Annotations changes, and will reload the page.
	
---

Thank you for reading! Please contact us on our [Archipelago Commons Google Group](https://groups.google.com/forum/#!forum/archipelago-commons) with any questions or feedback.

Return to the [Archipelago Documentation main page](../README.md).
