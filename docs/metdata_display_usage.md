---
title: Metadata-Display-Usage
tags:
  - Metadata Display
  - Twig Template
  - Usage
---

# Metadata Display Usage

Archipelago's Metadata Display Usage tab is an extension of [Metadata Display Preview](metadata_display_preview.md) that provides a convenient way for you to check where a Metadata Display Template is currently being used across your Archipelago. You can review the direct and indirect usage of each Metadata Display Template (found at `/metadatadisplay/list`) across your whole system, and quickly jump to corresponding usage areas to make adjustments if needed. 

## Step-by-Step

1. Navigate to the Metadata Display list at `/admin/content/metadatadisplay/list` (or through the admin menu via  `Manage > Content > Metadata Displays`). From the main Metadata Display List page, you can access all of the different display, rendering, and processing templates found in your Archipelago. You can also quickly review the 'In use' status from this page.

    ![Metadata Display List with Usage](images/metadata_display_list_with_usage.png)

2. Click on the Template Name or select 'Edit' for the Template you wish to review and navigate to the 'Usage' tab.

3. On the Usage tab for every Template in your Archipelago, you will be able to see the 'Label' and 'How' (direct or indirect) usage information for the following four areas across your whole system:
    - Exposed Metadata Display Entities (*such as an exposed Dublin Core XML endpoint*)
    - AMI sets (*[Archipelago Multi Importer](ami_index.md)*)
    - Entity View Modes (*[Primer on View/Display Modes](webformsasinput.md)*)
    - Views (*Templates can be used by Views for rendering outputs, amongst other uses*)
  
    ![Metadata Display Usage Tab](images/mdp_usage_tab.png)

    ![Metadata Display Usage Tab View Section](images/mdp_usage_tab_view_section.png)   

4. For each usage section noted above, you will be able to navigate to any corresponding Exposed Metadata Display Entity, AMI Set, Entity View Mode, or View to make changes to the template usage.

!!! note "Before Making Any Changes"

    Before diving into Metadata Display Usage review and making changes to what/where tempaltes are used, we recommend reading our [Twigs in Archipelago documentation](metadatatwigs.md) overview guide and also our [Working with Twig](workingtwigs.md) primer.

___

Thank you for reading! Please contact us on our [Archipelago Commons Google Group](https://groups.google.com/forum/#!forum/archipelago-commons) with any questions or feedback.

Return to the [Archipelago Documentation main page](index.md).
