# Archipelago Contribution Guide [![Google Groups](https://img.icons8.com/wired/32/000000/google-groups.png)](https://groups.google.com/forum/#!forum/archipelago-commons)


``Archipelago`` welcomes, appreciates, and recognizes any and all type of contribution. This includes input on all use cases and needs, questions or answers, documentation, DevOps, and configurations.  It also includes more general ideas, thoughts and even future dreams! Of course we also invite you to contribute PHP code, including fixes and new features. We want to be embracing, helpful, kind and open. We encourage discussions and always respect anyone's opinions, language, gender, style, backgrounds, origins and destinations if those come from the same root values of respect we state here. We follow and support conflict resolution using nothing more than basic common sense and value diversity in all its shapes, forms, colors, epoches, numbers and kinds, with or without labels, including inbetweens and evolving. We always assume we can do better and that you have done a lot. Under this very basic social framework, this is how we hope you can contribute:

## Where the wild things live

Archipelago has 5 active Github repositories

- [Strawberryfield](https://github.com/esmero/strawberryfield/tree/8.x-1.0-beta1)
  What? Code: Deals with Metadata Storage and Exposure to Drupal. Events/Subscribers that trigger when Content is modified. The way Internal pieces of JSON are exposed to the rest of the ecosystem. Core to all what is Archipeleago as a concept. One of its Drupal forms is a Field.
- [Webform Strawberryfield](https://github.com/esmero/webform_strawberryfield/tree/8.x-1.0-beta1)
  What? Code: Deals with UI based Ingest of Content using Webforms. How Files and Media is attached to the JSON, tech md is extracted and any interaction that happens during the edit and ingest process via a form. In its Drupal form it provides a Field Widget for `Strawberryfield`.
- [Format Strawberryfield](https://github.com/esmero/format_strawberryfield/tree/8.x-1.0-beta1)
  What? Code:Deals with exposing and transforming the JSON when navigating the site. What is displayed, how its displayed. Provides templating, Metadata Display entities via Twig and direct file downloads. In its Drupal from it provides many Field Formatters for Strawberryfield` and a Content Entity for the twig templates.
- [Archipelago Deployment](https://github.com/esmero/archipelago-deployment/tree/8.x-1.0-beta1)
  What? DevOps: Docker-compose deployment strategy, including a full skeletong project with persistance folders for Min.io, DB, Solr, Cantaloupe and Drupal 8. Includes Initial deployment configurations, what modules are enabled, how things look in Drupal 8 and some scripts plus the deployment documentation for both OSX and Linux.
- [Archipelago Documentation](https://github.com/esmero/archipelago-documentation/tree/8.x-1.0-beta1)
  What? Documentation: This guide and whatever we manage to write to explain Archipelago goes here.

## Questions, answers and in transit between both ends.

For now we have a single Community interaction Channel, our [google group](https://groups.google.com/forum/#!forum/archipelago-commons). That is the best place to ask questions and make suggestions that are not specific to a single module and/or you think can contribute to a larger community. Discussions work best in this forum (not excluding github of course), and our official announcements are posted there too.


## Documentation Workflow

Documentation is an evolving effort, and we need help. This guide lives in github in [Archipelago Documentation](https://github.com/esmero/archipelago-documentation/tree/8.x-1.0-beta1). Documentation and Development Worklfow both work the same way, so keep reading!

## Development Workflow

Start by reading open ISSUES (so you don't end up redoing what someone else is already working on) and looking at our [Roadmap for version 8.x-1.0](https://github.com/esmero/archipelago-deployment/issues/5). If the solution to your problem is not there or of there is an unchecked element in the roadmap, this is a great opportunity to help by creating a new ISSUE.

So next you will start by opening an Github ISSUE in any of the 5 GitHub repositories. Which repository will depend on what it is you are trying to do.
Please be concise with the title of your ISSUE so that it is easy to understand. Use Markdown to explain the What, How, etc of your contribution. Note: Even if something related is already in the works, if for example there could already be an open issue or you are in doubt, you can still contribute. Just add your comments on any open ISSUE. Or if you think you want to contribute with a totally different perspective, feel free to open a new ISSUE anyway. We can always discuss next steps starting from there. Every Community has its rhythm and style and our style is just beginning to develop: we are still figuring out what works best for everyone.

Once you are done and you feel comfortable working to make a change yourself, take note of the `ISSUE number` (lets name it `#issuenumber`).

The Gist is:
- Fork the GitHub repository where you created the ISSUE, decide which Branch you want to change.
- Make a new Branch out of that one and name it ISSUE-#issuenumber. E.g if #issuenumber is 6, name your branch ISSUE-6.
- Make changes in that branch and send a pull request.

As a best practice, we encourage pull requests to discuss/fix existing code, new code and documention changes.

For the full step by step workflow, we will use [Archipelago Documentation](https://github.com/esmero/archipelago-documentation/tree/8.x-1.0-beta1) and the `8.x-1.0-beta1` branch as example. The same applies to any of the other repositories, just change the remote urls and use the most current Branch name.

### Example: Setup Archipelago Documentation GitHub Repository
Fork the [Archipelago Documentation Upstream](https://github.com/esmero/archipelago-documentation/fork) source repository to your own personal Github account (e.g YOU). Copy the URL of your Archipelago Documentation fork (you will need it for the `git clone` command below).

```Shell
$ git clone https://github.com/YOU/archipelago-documentation
$ cd archipelago-documentation
$ git checkout 8.x-1.0-beta1
```

### Set up git remote as ``upstream``
```Shell
$ git remote add upstream https://github.com/esmero/archipelago-documentation
$ git fetch upstream
$ git merge upstream/8.x-1.0-beta1
...
```

### Create your ISSUE branch
Before making changes, make sure you create a branch using the ISSUE number you created for these contributions.

```Shell
$ git checkout -b ISSUE-6
```

### Do some Cleanup and test locally
After your code changes, make sure

- If modifying `PHP`, run `phpcs --standard=Drupal yourchanged.file.php`. We use (or try our best) to use Drupal 8 Coding standards
- If modifying a `MARKDOWN` file, make sure it renders Ok (you can use Textmate, Atom, Textile, etc to preview) and that links are not broken.
- If writing large pieces of Code, add PHP Tests. We can help if you don't know how (tell us in the ISSUE). Test will be enforced starting with the first stable release, long conversation of why!
- If modifying `PHP`, please test your changes live on your local instance of Archipelago. All non Documentation modules are already there inside `web/modules/contrib/`


### Commit changes
After verification, commit your changes. This is [very good post](https://chris.beams.io/posts/git-commit/) on how to write commit messages.
```Shell
$ git commit -am 'Fix that Strawberry'
```

### Push to the branch
Push your locally committed changes to the remote origin (your fork)
```Shell
$ git push origin ISSUE-6
```

### Create a Pull Request
Pull requests can be created via GitHub. [This document](https://help.github.com/articles/creating-a-pull-request/) explains in detail how to create one. After your Pull Request gets peer reviewed and approved, it can be merged. Discussion can happen and peers can ask you for modifications, fixes or more information on how to test. We will be respectful. You will be given credit for all your contributions and shown appreciation. There is no wrong and never too little. There could be a too much!
