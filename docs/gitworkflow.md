# Managing, sheltering, pruning and nurturing your own custom Archipelago 

Now that you have your base Archipelago Live Deployment running (Do you? If not, [go back!](../README.md)) you may be wondering about things like:

1. What happens when I need to update to the next release?
2. How do I keep my Drupal and Modules updated in between releases?
3. Can I add Drupal Modules?
2. Will a new release overwrite all my customizations?
3. What things are safe to customize?
4. How do I keep my very own things in Version Control and safe from others?
5. And many (many) other similar questions

## 1. Keep your Archipelago under Version Control via Github

`Archipelagos` are living beings. They evolve and become beautiful, closer and closer to your needs. Because of that `resetting` 
your particularities on every `Archipelago` code release is not a good idea nor even recommended. 
What you want is to keep your own `Drupal Settings` safe and be able to restore all in case something goes wrong. 
Your facets, your themes, your Solr fields, your own modules and all their configurations. 

The ones we ship with every Release will `reset` your Archipelago's settings to Factory defaults if applied `wildly`.

This is where `Github` comes in place.

### Basic steps

Prerequisites:
- Have an Archipelago Deployment Live instance running
- Have Terminal access to your Live Instance
- Have a Github account
- Have a personal Github [Access Token Created](https://docs.github.com/en/github/authenticating-to-github/keeping-your-account-and-data-secure/creating-a-personal-access-token)
- Run `git config --global --edit` on your Live Instance and Set your user name/email/etc 
  -  Note: Opens in `Vi`! in case of emergency/panic press `ESC` and type `:x` to escape and/or run away in terror. To edit Press `i` and uncomment the lines. Once Done press `ESC` and type `:x` to save.

### 1.1 Start by creating a Git Fork
Let's fork https://github.com/esmero/archipelago-deployment-live under **your own Account** via the web. 
Happy Note: Since 2021 keeping also forked branches in sync with the origin can be done via the UI directly.

### 1.2 Connect your Live instance terminal. 
Move to your repository's base folder, and let's start by adding your New Fork as a secondary Git `Origin`.
Replace in this command `yourOwnAccount` with (guess what?) **your own account**.

```Shell
git remote add upstream https://github.com/yourOwnAccount/archipelago-deployment-live
````

Now check if you have two remotes (`origin` => This repository, `upstream` => your own fork):

```Shell
git remote -v
````

You will see this:

```Shell
origin	https://github.com/esmero/archipelago-deployment-live (fetch)
origin	https://github.com/esmero/archipelago-deployment-live (push)
upstream	https://github.com/yourOwnAccount/archipelago-deployment-live (fetch)
upstream	https://github.com/yourOwnAccount/archipelago-deployment-live (push)
```

Good!

### 1.3 Now let's create from your current Live Instance a new Branch.
We will push this branch into your Fork and it will be all yours to maintain.
Please replace `yourOwnOrg` with any Name you want for this. We like to keep the current Branch name in place after your personal
prefix.

```Shell
git checkout -b yourOwnOrg-1.0.0-RC3
```

Good, you now have a new `local` branch named `yourOwnOrg-1.0.0-RC3` and it's time to decide what we are going to push into Github.

### 1.4 Push the Basics.
By default our deployment strategy (this repository) ignores a few files you want to have in Github. 
Also, there are things like the Installed Drupal Modules and PHP Libraries (the Source Code), the Database, Caches and also your Secrets (`.env` file) and your drupal `settings.php`) file.
You **FOR SURE** do **not** want to have these in Github and are better suited for a private Backup Storage.

Let's start by `push`ing what you have (no commits, your new `yourOwnOrg-1.0.0-RC3` as it is) to your new Fork. From there on we can add new Commits and files.

```Shell
git push upstream yourOwnOrg-1.0.0-RC3
```

And Git will respond with the following. Use your `yourOwnAccount` personal Github Access Token as password.
```Shell
Username for 'https://github.com': yourOwnAccount
Password for 'https://yourOwnAccount@github.com': 
Total 0 (delta 0), reused 0 (delta 0)
remote: 
remote: Create a pull request for 'yourOwnOrg-1.0.0-RC3' on GitHub by visiting:
remote:      https://github.com/yourOwnAccount/archipelago-deployment-live/pull/new/yourOwnOrg-1.0.0-RC3
remote: 
To https://github.com/yourOwnAccount/archipelago-deployment-live
 * [new branch]      yourOwnOrg-1.0.0-RC3 -> yourOwnOrg-1.0.0-RC3
 ```

### 1.5 First Commit
Right now this new Branch (go and check it out at https://github.com/yourOwnAccount/archipelago-deployment-live/tree/yourOwnOrg-1.0.0-RC3)
will not differ at all from 1.0.0-RC3. That is ok. To make your Branch unique, what we want is to "commit" our changes. How do we do this?

Let's add our `composer.json` and `composer.lock` to our change list. Both of these files are quite personal and as you add more Drupal Modules, dependencies or Upgrade your Archipelgo and/or Drupal Core and Modules all of these corresponding files will change. See the `-f`?
Because our base deployment ignores that file and you want it, we "Force" add it. 
_Note: At this stage `composer.lock` won't be added at all because it's still the same as before. So you can only "add" files that have changes._

```Shell
git add drupal/composer.json 
git add -f drupal/composer.lock
```

Now we can see what is new and will be committed by executing:

```Shell
git status
```

You may see something like this:
```Shell
On branch yourOwnOrg-1.0.0-RC3 
Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
	new file:   drupal/composer.json

Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
	modified:   drupal/scripts/archipelago/deploy.sh
	modified:   drupal/scripts/archipelago/update_deployed.sh

Untracked files:
  (use "git add <file>..." to include in what will be committed)
	deploy/ec2-docker/docker-compose.yml
	drupal/.editorconfig
	drupal/.gitattributes
```

If you do not want to add each `Changes not staged for commit` individually (WE recommend you only commit what you need, be warned and take caution) you can also issue an `git add .` which means add all.

```Shell
git add drupal/scripts/archipelago/deploy.sh
git add drupal/scripts/archipelago/update_deployed.sh
git add deploy/ec2-docker/docker-compose.yml
```

In this case we are also committing `docker-compose.yml` which you may have customized and modified to your domain (See [Install Guide Step 3](https://github.com/esmero/archipelago-deployment-live/blob/1.0.0-RC3/README.md#step-3-setup-your-enviromental-variables-for-dockerservices)) `deploy.sh` and `update_deployed.sh` scripts.
If you ever need to avoid tracking at all certain files you can edit `.gitignore` file and add more patterns to it (look at it, it's fun!).

```Shell
git commit -m "Fresh Install of Archipelago for yourOwnOrg"
```

If you had your email/user account setup correctly (see [Prerequisites](https://github.com/esmero/archipelago-deployment-live/blob/1.0.0-RC3/docs/gitworkflow.md#basic-steps)) you will see:

```Shell
Fresh Install of Archipelago yourOwnOrg
 4 files changed, 360 insertions(+), 46 deletions(-)
 create mode 100644 deploy/ec2-docker/docker-compose.yml
 create mode 100644 drupal/composer.json
```

And now finally you can push this back to your Fork:

```Shell
git push upstream yourOwnOrg-1.0.0-RC3
```

And Git will respond with the following. Use your `yourOwnAccount` personal Github Access Token as password.

```Shell

Username for 'https://github.com': yourOwnAccount
Password for 'https://yourOwnAccount@github.com': 
Enumerating objects: 18, done.
Counting objects: 100% (18/18), done.
Compressing objects: 100% (10/10), done.
Writing objects: 100% (10/10), 2.26 KiB | 2.26 MiB/s, done.
Total 10 (delta 5), reused 0 (delta 0)
remote: Resolving deltas: 100% (5/5), completed with 5 local objects.
To https://github.com/yourOwnAccount/archipelago-deployment-live
   d9fa835..3427ce5  yourOwnOrg-1.0.0-RC3 -> yourOwnOrg-1.0.0-RC3
```

And done.

## 2. Keeping your Archipelago Modules Updated during releases

Releases in Archipelago are a bit different to other OSS projects. When a Release is done (let's say 1.0.0-RC2) we freeze the current release branches in every module we provide, package the release and inmediatelly start with a new Release Cycle (6 months long normally) by creating in each repository a new Set of Branches (for this example 1.0.0-RC3). All new commits, fixes, improvements, features now will ALWAYS go into the Open/on-going new cycle branches (for this example 1.0.0-RC3) and once we are done we do all over again: We freeze ((or this example 1.0.0-RC3) and a new release cycle starts with fresh new "WIP" branches (for this example 1.1.0). 

Some Modules like AMI or Strawberry Runners have their independent Version but are anyways release together. E.g for `1.0.0-RC3` both AMI and Strawberry Runners are 0.2.0. Why? because work started later than the core Archipelago and also because they are not really CORE. So what happens with `main` branches? In our project `main` branches are never experimental. They are always a 1:1 with the latest stable release. So `main` will contain a full commit of 1.0.0-RC2 until we freeze 1.0.0-RC3 when `main` gets that code. Over and over. Nice right?

### Which modules belong to Archipelago and follow a release cycle?

The following modules are the ones we update on every release:

- https://github.com/esmero/strawberryfield -> Composer name: `strawberryfield/strawberryfield`
- https://github.com/esmero/format_strawberryfield -> Composer name: `strawberryfield/format_strawberryfield`
- https://github.com/esmero/webform_strawberryfield -> Composer name: `strawberryfield/webform_strawberryfield`
- https://github.com/esmero/ami -> Composer name: `archipelago/ami`
- https://github.com/esmero/strawberry_runners    -> Composer name: `strawberryfield/strawberry_runners`

We also update macro modules that are meant for deployment like this [Repository](https://github.com/esmero/archipelago-deployment-live) and https://github.com/esmero/archipelago-deployment

To keep your Archipelago up to date, specially once you `go custom` as described in this Documentation, the process is quite simple. 
E.g to fetch latest `1.0.0-RC3` updates during the `1.0.0-RC3` release cycle run:

```Shell
docker exec -ti esmero-php bash -c "composer require strawberryfield/strawberryfield:dev-1.0.0-RC3 strawberryfield/format_strawberryfield:dev-1.0.0-RC3 strawberryfield/webform_strawberryfield:dev-1.0.0-RC3 archipelago/ami:0.2.0.x-dev strawberryfield/strawberry_runners:0.2.0.x-dev strawberryfield/strawberry_runners:0.2.0.x-dev archipelago/archipelago_subtheme:dev-1.0.0-RC3 -W"
```
And then run any Database updates that may be needed:

```Shell
docker exec -ti esmero-php bash -c "drush updatedb"
```
This will bring all the new code and all (except if there are BUGS! )should work as expected.

_Note:_ Archipelago really tries hard to be as backwards compatible as possible and rarely you will see a non documented or non dealt with deprecation. 

_Note 2:_ We of course recommend running always Stable (frozen) release but since code is plastic and fixes will go into a WIP open branch you should be safe enough to move all modules together. 

You can run these commands any time you need and while the release is open you will _get always_ the latest code (even if its always the same branch). Please follow/subscribe to each Module's Github to be aware of changes/issues and improvements.

## 3. Keeping your Archipelago's Drupal Contributed Modules and Core updated

### 3.1 Contributed Modules.
To keep your Archipelago's Drupal up to date check your Drupal at https://yoursite.org/admin/modules/update. Make sure you check mostly (yes mostly, no need to overreact) for Security Updates. Not every Drupal contributed module (project) keeps backwards compatibility and we try to test every version we ship (as in this repositorie's composer.lock files) before releasing. Once you detected a major change/requirement, visit the Project's Changelog Website and take some time reading it. If you feel confident its not going to break all, copy the suggested `composer command`. E.g if you visit https://www.drupal.org/project/google_api_client/releases/8.x-3.2 you will see that the update is suggested as:
```
Install with Composer: $ composer require 'drupal/google_api_client:^3.2'
```

Using the same module as an example, before doing any final updates, check your current running version (take note in case you need to revert)

```Shell
docker exec -ti esmero-php bash -c "composer info 'drupal/google_api_client"
```

**Keep the version around**.

Now let's update, which means using the suggested command translated to our own Docker world like this (notice the `-W`)

```Shell
docker exec -ti esmero-php bash -c "composer require 'drupal/google_api_client:^3.2 -W"
```

And then run any Database updates that may be needed:

```Shell
docker exec -ti esmero-php bash -c "drush updatedb"
```

This will update that module. Test your website. Depending on what you update you want to focus first on the functionality it provides and then create/edit/delete a fictuous Digital Object to ensure it did not added any errors to your most beloved Digital Objects workflows.

If you see errors, you feel its not acting as it should you can revert by doing:

```Shell
docker exec -ti esmero-php bash -c "composer require 'drupal/google_api_client:^VERSION_YOU_KEPT_AROUND -W"
```

And then run any Database updates that may be needed:

```Shell
docker exec -ti esmero-php bash -c "drush updatedb"
```

If this happens** we encourage you to please** 👏 share your finding with our community/slack/Github ISSUE here.

### 3.1 Drupal Core inside the same major version:

Quite similar to a contributed module but normally involves at least 3 dependencies and of course larger changes.

#### Exact Version
E.g. Inside a same major version (E.g inside Drupal 9) If you are currently running Drupal `9.0.1` and you want to update to an exact latest (as i write `9.2.4`)

```Shell
docker exec -ti esmero-php bash -c "composer require drupal/core:9.2.4 drupal/core-dev:9.2.4 drupal/core-composer-scaffold:9.2.4 drupal/core-project-message:9.2.4 --update-with-dependencies"
```

Or under Drupal 8, if you are currently running Drupal `8.9.14` and you want to update to an exact latest (as i write `8.9.18`)

```Shell
docker exec -ti esmero-php bash -c "composer require drupal/core-dev:8.9.18 drupal/core:8.9.18 drupal/core-composer-scaffold:8.9.18 --update-with-dependencies"
```

And then for both cases run any Database updates that may be needed:

```Shell
docker exec -ti esmero-php bash -c "drush updatedb"
```

#### Alternative Major Version

if you want to only remember a `single command` and want to be sure to also get all extra packages run for Drupal 9

```Shell
docker exec -ti esmero-php bash -c "composer require drupal/core-dev:^9 drupal/core:^9 drupal/core-composer-scaffold:^9 drupal/core-project-message:^9 -W"
```
Or for Drupal 8
```Shell
docker exec -ti esmero-php bash -c "composer require drupal/core-dev:^8 drupal/core:^8 drupal/core-composer-scaffold:^8 drupal/core-project-message:^8 -W"
```

And then for both cases run any Database updates that may be needed:

```Shell
docker exec -ti esmero-php bash -c "drush updatedb"
```

This will always get you the latest `Drupal` and `dependencies` allowed by your composer.json

### 3.2 Drupal Core between major versions:

Since mayor versions may bring larger deprecations, contributed modules will stay behind and the world (and your database may collapse) we _really_ recommend to do some tests first (locally) or follow one of our guides. We at archipelago will always document a larger version update. Currently, [Drupal 8 to Drupal 9 Update is documented in detail here](upgradeFromD8ToD9.md)

---

Thank you for reading! Please contact us on our [Archipelago Commons Google Group](https://groups.google.com/forum/#!forum/archipelago-commons) with any questions or feedback, or open an ISSUE in this [Archipelago Deployment Live Repository](https://github.com/esmero/archipelago-deployment-live/).

Return to [Archipelago Live Deployment](../README.md).
