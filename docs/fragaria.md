---
title: Fragaria Redirects Module
tags:
  - Fragaria
  - Redirects
---

# Fragaria Redirects Module

Archipelago's Fragaria Redirect Module is a Drupal 9/10 module that provides dynamic redirect routes matched against existing Search API field values. This module will also provide future Unique IDs (API) integrations and PURLs.

### Prerequisites

Before proceeding with the following configuration steps, you need to first create the [Strawberry Key Name Provider and Solr Field](strawberry_key_name_providers.md) that corresponds to the Field that will be matched against the variable part of the route exists.

In other words, if your Digital Objects have a field and value such as:

```JSON
	"legacy_PID": [
		"mylegacyrepo:1234"
	],	
```

You need to make sure the values from the `legacy_PID` JSON key are indexed (as a Solr/Search API Field) and ready for use as part of the Fragaria Redirects configuration. 

!!! note "Best Practice"

    Your new Solr field should to be of field type **"String"** for a perfect match and best results. Using "Full Text" or a related variant Solr field type will allow for a partial match, which might lead multiple original URLs redirecting to the first match in Archipelago.

## Fragaria Redirect Entity Configuration

1. Navigate to `/admin/config/archipelago/fragariaredirect`.

2. Select the `Add a Redirect Config` button.

3. Enter a label for the Fragaria Redirect Entity you are configuring.

4. Enter the Prefix (that follows your domain) for the Redirect Route.

    - Do not use `node/` or `do/` as a Prefix. Even if these will technically work (redirect), using either of these Prefix paths will override your existing Paths defined by Drupal and Archipelago.

5. If applicable, enter the the Suffixes (that follow the prefix + the variable part) for the Redirect Route.

    - Enter one by line. This configuration option is not required.
    - Check/uncheck the option to `Instead of fixed Prefixes add a single {catch_all} variable suffix at the end` as needed. Checking this will disable any entered static suffixes.

6. Select the Search API Index where the Field that will be matched against the variable part of the route exists.

7. Select the Search API Field that will be matched against the variable part of the route.

8. If applicable, add static prefixes for to the variable part/argument of the path.

    - Enter one by line. This is useful when the variable part of the ROUTE does not match 1:1 the actual indexed data. e.g the route is /oldrepo/1 and the indexed value is "namespace:1". In that case add "namespace:" here.

9. Add static suffixes for to the variable part/argument of the path.

    - Enter one by line. This is useful when the variable part of the ROUTE does not match 1:1 the actual indexed data. e.g the route is /oldrepo/namespace:1 and the indexed value is "namespace:1-page". In that case add "-page" here.

10. Select the Type of HTTP redirect to perform.

    - Option 1: Temporary Redirect (forced GET)
    - Option 2: Permanent Redirect

11. Lastly, select the box next to `Is this Fragaria Redirect Route active?` to set your Redirect to `active`, and **Save** your configuration.

### Example Redirects Configuration

![Fragaria Redirects Configuration Example](images/fragaria_redirects_config_example.png)

The above example configuration would enable a Temporary redirect from a legacy repository site with a URL of _https://mylegacyrepo.edu//mylegacyrepo/object/mylegacyrepo:1234_ to your new Archipelago PURL of _https://mynewarchipelagorepo.edu/do/mynewADOUUID_.

___

Thank you for reading! Please contact us on our [Archipelago Commons Google Group](https://groups.google.com/forum/#!forum/archipelago-commons) with any questions or feedback.

Return to the [Archipelago Documentation main page](index.md).
