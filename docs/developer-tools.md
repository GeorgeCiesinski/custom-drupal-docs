# Developer Tools

This document outlines the tools used to develop the sites. With some of the tools, I have also outlined some frequently used processes used during the development of the sites. 

## Command-line

There are many command-line tools which are used throughout the development of the sites. This section covers some of the built-in utilities which don't fall under other categories. 

### Tar

The [Tar](https://www.linux.org/docs/man1/tar.html) command creates tarball archives of files and directories while preserving file permissions. This is one of the tools used to create backups of the site directories. 

## Git

Git is a version control software and is used by the Humber ITS team to back up Drupal site directories. 

Learn more about [managing the repository using Git](https://www.drupal.org/docs/user_guide/en/extend-git.html).

### Requirements

1. Basic Git knowledge - Learn more about [becoming a git guru](https://www.atlassian.com/git/tutorials).

2. Basic understanding of Git-flow - It is extremely encouraged to maintain git-flow when pushing commits to the site repositories. Learn more about [git-flow](https://www.atlassian.com/git/tutorials/comparing-workflows/gitflow-workflow).

### Branch Types

**Main** - Permanent Branch

> This is the production branch and is a permanent branch. Only new versions should be pushed to the main branch. 

**Develop** - Permanent Branch

> This is the main development branch and is the second permanent branch. New features and hotfixes should branch off from the develop branch, and then back into it when complete. Release branches should also branch out from develop before being merged into main. 

> This branch only needs to be created once:

```shell title="Create develop branch and push to origin"
git checkout -b develop 
git push --set-upstream origin develop
```

**Feature**

> This is a new feature. It can be a new element, new logic, or any other code that results in a new feature being added to the project. 

```shell title="Create a new feature branch"
git checkout -b feature/descriptive-title
```

**Hotfix**

> This is essentially a bug fix. A hotfix branch would be created specifically to address a bug or issue. Like the feature branch, the changes can later be merged back into develop. 

```shell title="Create a new hotfix branch"
git checkout -b hotfix/descriptive-title
```

**Release**

> Unlike Feature or hotfix, a release branch is typically a child of develop and is merged into main. The purpose of the release branch is to do final testing and last-minute changes before creating a tagged release on the main branch. 

```shell title="Create a new release branch"
git checkout -b release/feature-name
```

### Additional Info

Some more information can be found in these links. These are meant to get a better understanding of how Git can be used on a Drupal site, as well as best practices and the Git-flow method of creating branches. It should not be used in place of the other instructions in this document. 

[Building a Drupal Site with Git](https://www.drupal.org/docs/installing-drupal/building-a-drupal-site-with-git)

[Building a Drupal Site with Git](https://www.drupal.org/docs/installing-drupal/building-a-drupal-site-with-git)

[Successful Git Branching Model](https://nvie.com/posts/a-successful-git-branching-model/)

## Composer

In order to make maintenance easier, all of the Humber ITS Drupal Sites were built using [Composer](https://getcomposer.org/). 

Learn more about [Using Composer to Install Drupal and Manage Dependencies](https://www.drupal.org/docs/develop/using-composer/manage-dependencies).

## Drush

[Drush](https://www.drush.org/) provides command-line tools for all of the Humber ITS Drupal Sites and is required to carry out some of the steps outlined further in these docs.

### Installing Drush

**Note:** If you cloned the existing site repositories, then it is likely that drush was automatically installed via the `composer.json` file in the site repository and does not need to be installed again. These instructions are mainly for any new Humber ITS Drupal sites. 

Drush can be installed using Composer: 

1. `cd` into the website root folder using Terminal.
2. Install drush using the command: `composer require --dev drush/drush`

### Drush Actions

There are a number of actions that can be performed using Drush. This section contains the actions which are commonly used in Humber ITS Drupal sites. 

#### Clear Cache

This is the quickest way to clear cache and is very useful when developing custom themes. Sometimes, changes to the theme do not reflect on the page due to outdated cache information. Run this command before refreshing the page to ensure the changes are rendered by the site:

```shell title="Clear cache using Drush"
drush cr
```

#### One Time Login

**Note:** drush is required in order to follow these steps.

If a user is unable to login for some reason, it is possible to send them a one-time login using drush uli. If successful, it will output a URL that the user can use to login and change their password. 

```shell title="Generate a one-time login link" linenums="1"
drush uli --name=username  --uri=website-url
```