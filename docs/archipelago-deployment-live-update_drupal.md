---
title: Updating Drupal Core in Archipelago
tags:
  - Archipelago-deployment
  - Archipelago-deployment-live
  - DevOps
  - Drupal
  - Drupal Core
---

# Updating Drupal Core on a live Archipelago Site

Drupal, the project, [puts out new core releases on a regular schedule](https://www.drupal.org/about/core/policies/core-release-cycles/schedule). Your Archipelago site needs to apply the security updates and possibly minor releases between major core updates. Major core updates will typically coincide with an updated Archipelago stable release. 

Updating core is done via composer.

## Steps/Guides

1. First you will want to verify Drupal core itself will update and the dependencies. To so, you run the following command. 
```docker exec -ti esmero-php bash -c "composer update "drupal/core-*:^9" --with-all-dependencies --dry-run```
The ```--dry-run``` flag will allow you to see what will be updated. Once you review the updates and ready to go with the full update, you will run the same command without the ```dry-run``` flag.
2. Update core and the dependencies
```docker exec -ti esmero-php bash -c "composer update "drupal/core-*:^9" --with-all-dependencies"```
3. Sometimes the database needs to be updated after the update of the files. The following command will prompt you to type yes if there are updates to be done, or will not return anything if no updates are needed.
```docker exec -ti esmero-php bash -c "drush updatedb"```
4. Clear the Drupal cache
```docker exec -ti esmero-php bash -c "drush cache:rebuild"```

Occassionaly there will be other Drupal modules that Archipelago uses and they need to be updated at the same time you run a core update. This is an example of updating Drupal Webform which was required for moving to Drupal 9.5.x.

1. Use composer to update a module to a specific release. 
```docker exec -ti esmero-php bash -c "composer update "drupal/webform:6.1.4```
2. Check for database updates and apply them if necessary
```docker exec -ti esmero-php bash -c "drush updatedb"```
3. Clear the cache again
```docker exec -ti esmero-php bash -c "drush cache:rebuild"```
