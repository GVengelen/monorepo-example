This monorepo is forked fro [this repository](https://github.com/alexeagleson/monorepo-example) by Alex Eagleson. The contents of this Readme are not about setting up monorepos, they will cover how you can incorperate changesets and ArgoCD in you workflow. If you're interested in setting up monorepos you can check out Alex' video on Youtube

{% youtube McEc5yX6kf4 %}

## Table of contentents
- Setting up Changesets
- Adding Changesets to Github Actions
- Adding Argocd to your cluster
- Configuring Argocd Image updater

## Monorepos
One method of grouping code from multiple projects into one is called a [monorepo](https://en.wikipedia.org/wiki/Monorepo).  A _monorepo_ is simply the practice of placing multiple different projects that are related in some way into the same repository.

The biggest benefit is that you do not need to worry about version mismatch issues between the different pieces of your project.  If you update an API route in the server of your monorepo, that commit will be associated with the version of the front end that consumes it.  With two different repositories you could find yourself in a situation where your v1.2 front-end is asking for data from your v1.1 backend that somebody forgot to push the latest update for.

Another big benefit is the ability to import and share code and modules between projects.  Sharing types between the back-end and front-end is a common use case.  Your can define the shape of the data on your server and have the front-end consume it in a typesafe way.  

## Integrating Changesets

This section is for maintainers who want to use Changesets to manage versioning and releases. If you’re contributing, see [adding a changeset](https://github.com/changesets/changesets/blob/main/docs/adding-a-changeset.md) for contributor instructions.

The typical workflow with Changesets looks like this:

```
+-------------------+      +---------------------+      +---------------------+
| 1. Add changeset  | ---> | 2. Prepare release  | ---> | 3. Publish packages |
+-------------------+      +---------------------+      +---------------------+
```

We'll automate Steps 2 and 3 later in this tutorial

### Installing Changesets

To get started, install the Changesets CLI and initialize it in your repository:

```shell
yarn add @changesets/cli -W && yarn changeset init
```
or
```shell
npx @changesets/cli init
```

### Creating a Changeset

Whenever you make a change that should be released, create a changeset:

```shell
yarn changeset
```
or
```shell
npx @changesets/cli
```

> Tip: You can also use `changeset add` to create a changeset, but running the CLI without a subcommand works too.

### Releasing New Versions

When you’re ready to cut a release, run the following to update versions and changelogs:

```shell
yarn changeset version
```
or
```shell
npx @changesets/cli version
```

This command will:

- Collect all pending changesets
- Bump package versions according to semantic versioning
- Update changelogs with details from each changeset

At this stage, review the generated changelogs and version bumps to ensure everything looks correct. Make any edits if needed.

### Publishing Packages

Once you’re satisfied with the changes, publish your packages to npm:

```shell
yarn changeset publish
```
or
```shell
npx @changesets/cli publish
```

This will publish any packages that have a new version compared to what’s on npm.

```
+-------------------+      +---------------------+      +---------------------+
|  Add changeset    | ---> |  Version & changelog| ---> |   Publish to npm    |
+-------------------+      +---------------------+      +---------------------+
```

