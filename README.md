The Dark docs are built using Docusaurus.
If you're trying to do something beyond the scope of this README, check out their [docs](https://docusaurus.io/).

# What's In This Document

* [Get Started in 5 Minutes](#get-started-in-5-minutes)
* [Project Structure](#project-structure)
* [Editing Content](#editing-content)
* [Adding Content](#adding-content)
* [How CI Auto-deploys](#how-ci-auto-deploys)
* [Publishing Changes Manually](#publishing-changes-manually)

# Get Started in 5 Minutes

1. Make sure all the dependencies for the website are installed:

```sh
# Install dependencies
$ cd website
$ yarn
```
2. Run your dev server:

```sh
# Start watching the site for changes
$ yarn start
```

## Project Structure

There are two main branches:

* master
* gh-pages

The website is hosted from `gh-pages`, but everything there is auto-generated from `master`.
When we want to make changes, we create a new branch off `master` with the format `username/my-change` and make as many commits as we need to.
Then, we create a new pull request from that branch with `master` as the base. When the pull request is merged, CircleCI will automatically
deploy the changes from `master` to the website (it runs a script against the source files on `master` and deploys the generated website to `gh-pages`).

### Directory Structure

The project file structure in `master` is

```
docs/
  README.md -- this file
  .circleci/
    config.yml -- used to autodeploy via circle ci
  docs/ -- individual markdown documentation pages
    changelog.md
    getting-started.md
    assets/ -- images that can be referenced in the doc pages
    ...
  website/
    package.json -- helper scripts
    siteConfig.js -- core site configuration
    sidebars.json -- sidebar sections and pages
    blog/ -- we don't use this yet but might want to for the changelog. See [this](https://docusaurus.io/docs/en/adding-blog).
    core/
      Footer.js -- a customizable footer for the website. Required by docusaurus
    pages/ -- additional docs pages not currently linked from anywhere
      en/
        help.js -- help page
        users.js -- users of this project page
    static/
      css/
      img/
    node_modules/
  .gitignore -- ignores autogenerated files that shouldn't sync via git
```

# Editing Content

## Editing an existing docs page

Edit docs by navigating to `docs/` and editing the corresponding document:

`docs/doc-to-be-edited.md`

```markdown
---
id: page-needs-edit
title: This Doc Needs To Be Edited
---

Edit me...
```

For more information about docs, click [here](https://docusaurus.io/docs/en/navigation)

# Adding Content

## Adding a new docs page to an existing sidebar

1. Create the doc as a new markdown file in `/docs`, example `docs/newly-created-doc.md`:

```md
---
id: newly-created-doc
title: This Doc Needs To Be Edited
---

My new content here..
```

2. Refer to that doc's ID in an existing sidebar in `website/sidebar.json`:

```javascript
// Add newly-created-doc to the Getting Started category of docs
{
  "docs": {
    "Getting Started": [
      "quick-start",
      "newly-created-doc" // new doc here
    ],
    ...
  },
  ...
}
```

For more information about adding new docs, click [here](https://docusaurus.io/docs/en/navigation)

## Adding items to your site's top navigation bar

1. Add links to docs, custom pages or external links by editing the headerLinks field of `website/siteConfig.js`:

`website/siteConfig.js`
```javascript
{
  headerLinks: [
    ...
    /* you can add docs */
    { doc: 'my-examples', label: 'Examples' },
    /* you can add custom pages */
    { page: 'help', label: 'Help' },
    /* you can add external links */
    { href: 'https://github.com/facebook/docusaurus', label: 'GitHub' },
    ...
  ],
  ...
}
```

For more information about the navigation bar, click [here](https://docusaurus.io/docs/en/navigation)

## Adding custom pages

1. Docusaurus uses React components to build pages. The components are saved as .js files in `website/pages/en`:
2. If you want your page to show up in your navigation header, you will need to update `website/siteConfig.js` to add to the `headerLinks` element:

`website/siteConfig.js`
```javascript
{
  headerLinks: [
    ...
    { page: 'my-new-custom-page', label: 'My New Custom Page' },
    ...
  ],
  ...
}
```

For more information about custom pages, click [here](https://docusaurus.io/docs/en/custom-pages).

## Checking formatting

We run some tools to ensure that the docs are consistently formatted and to
find common errors. If you run `yarn format` you should pass the linter.

`markdownlint` is run automatically in CI - you can run it
locally as `yarn run lint`.

# How CI Auto-deploys

The `.circleci/config.yml` file describes the CircleCI configuration.
It watches for commits/merges into the `master` branch, runs a script to generate the contents of
`gh-pages`, and pushes `gh-pages` to github.

# Publishing Changes Manually

**NOTE: You shouldn't need to do this because CircleCI runs this automatically.**

On the commandline (remember to replace `<YOUR USERNAME>` with your github username):

```sh
$ cd website
$ GIT_USER=<YOUR USERNAME> CURRENT_BRANCH=master yarn publish-gh-pages
```

If you're using ssh instead of https, replace `GIT_USER=<YOUR USERNAME>` with `USE_SSH=true`.
