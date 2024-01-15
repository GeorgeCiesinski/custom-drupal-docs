# Standards

Code standards are typically a good idea as it makes it easier to collaborate and to avoid certain bugs. This document contains various code standards for working on Drupal sites. 

It is especially recommended to follow standards when developing a Drupal site as much of the documentation and forum posts adhere to standards.

## Version Control

The source code is backed up using [Git](./developer-tools.md#git). This is mostly used to back up snapshots of the site throughout development. For example, if the Drupal theme needs to be modified, you may branch off from the `develop` branch to create a `feature/detailed-name` branch using Git, and make your changes there. In the event something goes wrong and the theme completely breaks, you can simply checkout the `develop` branch to return to a working version.

**Note:** this is not a complete back-up of the site, but is instead used to create snapshots of the site throughout development. To see how to create a backup, see [Backup and Restore](backup-restore.md).

### Gitignore

A gitignore is used to ensure sensitive files or unneeded files are not backed up using version control. For example, the `settings.php` file contains sensitive information including the database connection information, and should be backed up separately. For this reason it is included on the `.gitignore` so that it is ignored by Git. 

Learn more about [Setting up the .gitignore file](https://www.drupal.org/docs/user_guide/en/extend-git.html). 

## Humber Standard

### Web Accessibility Compliance

[HUMBER WEB ACCESSIBILITY COMPLIANCE (AODA)](https://humber.ca/tutorial/web-accessibility-compliance.html) - provide access to the WCAG Quick-Reference guide and AODA Compliance Reference, as well as various tools that assist in evaluating Web Accessibility. 

### Humber Interim Web Guidelines

[Humber Interim Web Guidelines](https://humber.ca/brand/sites/default/files/publications/interim-web-guidelines.pdf
) - provides guidelines for standards Humber Websites should meet.

## Code Standard

### Drupal

[Drupal Standards](https://www.drupal.org/docs/develop/standards) provides a general overview of the various code standards relevant to Drupal Sites, and provides links to specific docs about these standards.

#### CSS

[CSS](https://www.drupal.org/docs/develop/standards/css) provides a number of links discussing CSS standards, formatting guidelines, and more. 

**Note:** As per George Ciesinski, it is recommended to use SCSS instead of plain CSS as it provides a much better way to organize style code and maintain it in future updates. 

#### SCSS

SCSS can be useful for nesting style code within classes so that duplicate code can be easily avoided. It also helps with readability and provides other useful functions. 

##### Folder Organization

The easiest way to organize SCSS files within a theme is with the below folder structure:

```
.
└── theme-name/
    ├── css
    ├── fonts
    ├── images
    ├── js
    └── src/
        └── scss/
            ├── abstracts
            ├── base
            ├── components
            └── layout
```

**Abstracts** houses files containing mutually accessible styles which can be imported into other files using `@use "../abstracts" as *;`. Examples of style you might want to reuse throughout the site are `_colors.scss`, `_variables.scss`, `_breakpoints.scss` and more. 

To get this to work, it is necessary to create an `_index.scss` file in the `abstracts/` directory, and `@forward` each file you want to be able to `@use` in other files: 

```scss
@forward './breakpoints';
@forward './colors';
@forward './variables';
```

**Base** contains base styling for elements such as `html`, `ul`, headers, or any other style you want to apply to all elements across the site.

**Components** contains style code for specific components on the site. For example, you may create a `breadcrumb.scss` file in the components folder to house all of the style for the breadcrumb component.

**Layout** contains style code for specific pages or larger components which themselves contain components. An example for this might be navigation, forms, containers, or specific pages. 

##### Compiling SCSS

You can compile the SCSS files with any SCSS compiler. The easiest way is to use a custom script `watch.sh` which can be provided by George Ciesinski.

#### Javascript & Jquery

[Javascript Coding Standards](https://www.drupal.org/docs/develop/standards/javascript-coding-standards) provides a number of useful links to Javascript and JQuery coding standards, and most importantly, best practices. 

The best practices section is especially worth a read as it helps avoid certain coding practices which result in javascript loading incorrectly, or failing to load altogether on some pages.

#### PHP

[https://www.drupal.org/docs/develop/standards/php] contains numerous links including coding standards, API, and much more. 
