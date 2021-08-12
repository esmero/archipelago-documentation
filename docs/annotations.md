# Annotations in Archipelago

Archipelago extends [Annotorius](https://github.com/recogito/annotorious) to provide W3C-compliants WebAnnotations for Digital Objects. These Annotations can be added per image (when multiple), edited, and saved/discarded using the regular Edit mode (bonus: temp storage that persists when you log out and come back in your session). Archipelago also exposes a full API for WebAnnotations, that keeps track of which Images (referenced in the Strawberryfield) were annotated and create inside the Digital Object's JSON the W3C valid entries.

### Enabling Annotations
1. Navigate to Admin --> Structure --> Content types --> Digital Object --> Manage Display and select the "Digital Object Full view" mode. `https://yoursite.org/admin/structure/types/manage/digital_object/display/digital_object_viewmode_fullitem`
	![annotations step 1](../imgs/annotations_step1.jpg)
2. On the “Fragola” row, click on the small gear icon on the far right, which will be open the configurations for this display type. Select the “Enable loading/editing of W3C webAnnotations” option. 	
	![annotations step 2](../imgs/annotations_step2.jpg)
   - Learn more about the JSON format of WebAnnotations here: https://www.w3.org/TR/annotation-model/#index-of-json-keys.
3. Select whether you would like to use the Rectangular or Polygon (freehand drawing) tool for your annotation style.
4. Select the 'Update' button and Save your settings.
You are now ready to get started adding Annotations!

### Adding and saving Annotations
1. Navigate to the Image-based Digital Object you would like to apply Annotations to.
2. To add a new Annotation, select and hold the Shift key. Click and then drag to apply either a Rectangular box or multi-point Polygon box.
3. Double click to exit the Annotation drawing mode.
4. Enter text for your Annotation in the 
	![annotations edit](../imgs/annotations_edit.jpg)
5. Click "Ok" when you are done.
6. To save your Annotation (or Annotations if you created multiple), navigate to the main content "Edit" tab, where you will see a message about Unsaved Web Annotation Changes.
	![annotations edit save](../imgs/annotations_edit_save.jpg)
7. Select "Save" to preserve your Annotation(s). They will now become part of your Digital Object's JSON, found under the `ap:annotationCollection` key.
	- Pressing the "Discard" button will discard only the unsaved Annotations, and will reload the page.

### Editing and Deleting Annotations
1. Navigate to the Image-based Digital Object you would whose Annotation(s) you want to edit or delete.
2. Click within the Annotation and select the downwards arrow in the upper right-hand corner of the pop-up window.
3. Select either the "Edit" option and Edit the Annotation as desired; Or select the "Delete" option.
	![annotations edit delete](../imgs/annotations_edit_delete.jpg)
4. To preserve your editing or deleting actions, navigate to the main content "Edit" tab, where you will see a message about Unsaved Web Annotation Changes.
	![annotations edit delete save](../imgs/annotations_edit_delete_save.jpg)
5. Select "Save" to preserve your Annotation(s) edits or deletions. Pressing the "Discard" button will discard only the unsaved Annotations changes, and will reload the page.
	
---

Thank you for reading! Please contact us on our [Archipelago Commons Google Group](https://groups.google.com/forum/#!forum/archipelago-commons) with any questions or feedback.

Return to the [Archipelago Documentation main page](../README.md).
