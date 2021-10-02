# Archipelago Contribution Guide

`Archipelago` welcomes, appreciates, and recognizes any and all types of contribution. This includes input on all use cases and needs, questions or answers, documentation, DevOps, and configurations. We also welcome general ideas, thoughts, and even dreams for the future of our repository! Of course, we also invite you to contribute PHP code, including fixes and new features.

We will be helpful, kind, and open. We encourage discussions and always respect one another's opinions, language, gender, style, backgrounds, origins, and destinations, provided they come from the same root values of respect, as stated here. We support conflict resolution using nothing more than basic common sense. We value diversity in all its shapes, forms, colors, epoches, numbers, and kinds, with or without labels, including in-between and evolving. We always assume we can do better and that you have done a lot. Under this very basic social framework, this is how we hope you can contribute:

## Where The Wild Things Live

Archipelago has 5 active GitHub repositories

- [Strawberryfield](https://github.com/esmero/strawberryfield)
  What? Code: Deals with metadata storage and exposure to Drupal. Events/Subscribers that trigger when content is modified. The way internal pieces of JSON are exposed to the rest of the ecosystem. Core to all of what Archipeleago is as a concept. One of its Drupal forms is a Field.
- [Webform Strawberryfield](https://github.com/esmero/webform_strawberryfield)
  What? Code: Deals with UI based ingest of content using webforms. How files and media are attached to JSON, how tech md is extracted, and any interaction that happens during the edit and ingest processes via a form. In its Drupal form it provides a Field Widget for `Strawberryfield`.
- [Format Strawberryfield](https://github.com/esmero/format_strawberryfield)
  What? Code: Deals with exposing and transforming the JSON when navigating the site. What is displayed, how it is displayed. Provides templating, metadata display entities via Twig, and direct file downloads. In its Drupal form it provides many field formatters for Strawberryfield` and a content entity for the Twig templates.
- [Archipelago Deployment](https://github.com/esmero/archipelago-deployment)
  What? DevOps: Docker-compose deployment strategy, including a full skeleton project with persistence folders for Min.io, DB, Solr, Cantaloupe and Drupal 8. Includes initial deployment configurations, which modules are enabled, how things look in Drupal 8 and some scripts plus the deployment documentation for both OSX and Linux.
- [Archipelago Documentation](https://github.com/esmero/archipelago-documentation)
  What? Documentation: This guide and whatever we manage to write to explain Archipelago goes here.

## Questions, Answers and In Transit Between Both Ends

We host a community interaction channel, our [google group](https://groups.google.com/forum/#!forum/archipelago-commons). This is the best place to ask questions and make suggestions that are not specific to a single module, and/or if you would like to contribute to a larger conversation within our community. Discussions work best in this forum (not excluding GitHub of course), and our official announcements are posted there too.


## Documentation Workflow

Documentation is an evolving effort, and we need help. This guide lives in GitHub in [Archipelago Documentation](https://github.com/esmero/archipelago-documentation). Documentation and Development Worklfow both work the same way, so keep reading!

## Development Workflow

Start by reading open ISSUES (so you don't end up redoing what someone else is already working on) and looking at our [Roadmap for version 1.0.0-RC2](https://github.com/esmero/archipelago-deployment/issues/103). If the solution to your problem is not there or if there is an unchecked element in the roadmap, this is a great opportunity to help by creating a new ISSUE.

Next, start by opening an GitHub ISSUE in any of the 5 GitHub repositories, depending on what it is you are trying to do.

Please be concise with the title of your ISSUE so that it is easy to understand. Use Markdown to explain the what, how, etc, of your contribution. Note: Even if something related is already in the works, you can still contribute. Just add your comments on any open ISSUE. Or, if you think you want to contribute with a totally different perspective, feel free to open a new ISSUE anyway. We can always discuss next steps starting from there. Every community has its rhythm and style and our style is just beginning to develop. We are still figuring out what works best for everyone.

Once you are done and you feel comfortable working to make a change yourself, take note of the `ISSUE number` (lets name it `#issuenumber`).

The gist is:
- Fork the GitHub repository where you created the ISSUE, decide which branch you want to change.
- Make a new branch out of that one and name it ISSUE-#issuenumber. E.g if #issuenumber is 6, name your branch ISSUE-6.
- Make changes in that branch and send a pull request.

As a best practice, we encourage pull requests to discuss/fix existing code, new code, and documention changes.

For the full step-by-step workflow, we will use [Archipelago Documentation](https://github.com/esmero/archipelago-documentation) and the `1.0.0-RC3` branch as example. The same applies to any of the other repositories: just change the remote urls and use the most current branch name.

### Example: Set Up Archipelago Documentation GitHub Repository
Fork the [Archipelago Documentation Upstream](https://github.com/esmero/archipelago-documentation/fork) source repository to your own personal GitHub account (e.g. YOU). Copy the URL of your Archipelago Documentation fork (you will need it for the `git clone` command below).

```Shell
$ git clone https://github.com/YOU/archipelago-documentation
$ cd archipelago-documentation
$ git checkout 1.0.0-RC3
```

### Set Up Git Remote As ``upstream``
```Shell
$ git remote add upstream https://github.com/esmero/archipelago-documentation
$ git fetch upstream
$ git merge upstream/1.0.0-RC3
...
```

### Create Your ISSUE Branch
Before making changes, make sure you create a branch using the ISSUE number you created for these contributions.

```Shell
$ git checkout -b ISSUE-6
```

### Do Some Clean Up and Test Locally
After your code changes, make sure

- If modifying `PHP`, run `phpcs --standard=Drupal yourchanged.file.php`. We (try our best to) use Drupal 8 coding standards.
- If modifying a `MARKDOWN` file, make sure it renders well (you can use Textmate, Atom, Textile, etc to preview) and that links are not broken.
- If writing large pieces of code, add PHP Tests. We can help if you don't know how (tell us in the ISSUE). Tests will be enforced starting with the first stable release.
- If modifying `PHP`, please test your changes live on your local instance of Archipelago. All non-documentation modules are already inside `web/modules/contrib/`.


### Commit Changes
After verification, commit your changes. This is [very good post](https://chris.beams.io/posts/git-commit/) on how to write commit messages.
```Shell
$ git commit -am 'Fix that Strawberry'
```

### Push To The Branch
Push your locally committed changes to the remote origin (your fork)
```Shell
$ git push origin ISSUE-6
```

### Create A Pull Request
Pull requests can be created via GitHub. [This document](https://help.github.com/articles/creating-a-pull-request/) explains in detail how to create one. After your Pull Request gets peer reviewed and approved, it can be merged. Discussion can happen and peers can ask you for modifications, fixes or more information on how to test. We will be respectful. You will be given credit for all your contributions and shown appreciation. There is no wrong and never too little. There could never be too much!

---

Thank you for reading! Please contact us on our [Archipelago Commons Google Group](https://groups.google.com/forum/#!forum/archipelago-commons) with any questions or feedback.

Return to the [Archipelago Documentation main page](index.md).
