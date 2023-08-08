# Drupal Sites

## Introduction

This document contains information that is shared between all the Humber ITS Websites built using Drupal. 

This includes information about settings, core and module updates, backing up and restoring sites, content updates, or possible bugs. 

**Warning:** There are many links I have included across various sections of this documentation. Many of these links contain good information, but many also contain outdated information from older versions of Drupal. This is unfortunately the nature of open-source documentation. I have done my best to sift through this data and only include accurate and current information in this documentation. For that reason, the links are only intended to provide insight into changes from older versions of Drupal, or to reinforce certain points made in this documentation which I have confirmed is correct. 

### Requirements

The ITS website serves tens of thousands of Faculty Members, Staff and Students in a College environment. The below requirements will help to provide a positive and secure experience to all website users: 

1.	**Security** – The website should stay up to date with major security updates, and should include authentication for all users who modify the content of the site. 
2.	**Optimization** – All images, videos, and other assets should be optimized before usage on the site to ensure a fast browsing experience.
3.	**Accessibility** – Content should meet the accessibility requirements outlined in the AODA (Accessibility for Ontarians with Disabilities Act) 

### Guidelines

[HUMBER WEB ACCESSIBILITY COMPLIANCE (AODA)](https://humber.ca/tutorial/web-accessibility-compliance.html) - provide access to the WCAG Quick-Reference guide and AODA Compliance Reference, as well as various tools that assist in evaluating Web Accessibility. 

[Humber Interim Web Guidelines](https://humber.ca/brand/sites/default/files/publications/interim-web-guidelines.pdf
) - provides guidelines for standards Humber Websites should meet.

### Stack

The Humber ITS Drupal Stack uses:

* Linux OS
* Apache Server
* PostgreSQL
* PHP

## Development

This section is primarily for developers. It covers backing up and restoring both the Drupal Sites, and their corresponding databases. In addition to this, it also covers carrying out maintenance tasks, and important development concepts such as the `settings.php` file and other configuration files. 

For more information about developer tools, see the [Developer Tools](./developer-tools.md) documentation. 

### Version Control Using Git

The source code is backed up using [Git](./developer-tools.md#git). This is mostly used to back up snapshots of the site throughout development. For example, if the Drupal theme needs to be modified, you may branch off from the `develop` branch to create a `feature/detailed-name` branch using Git, and make your changes there. In the event something goes wrong and the theme completely breaks, you can simply checkout the `develop` branch to return to a working version.

**Note:** this is not a complete back-up of the site, but is instead used to create snapshots of the site throughout development. To see how to create a backup, see [Backup and Restore](backup-restore.md).

#### Gitignore

A gitignore is used to ensure sensitive files or unneeded files are not backed up using version control. For example, the `settings.php` file contains sensitive information including the database connection information, and should be backed up separately. For this reason it is included on the `.gitignore` so that it is ignored by Git. 

Learn more about [Setting up the .gitignore file](https://www.drupal.org/docs/user_guide/en/extend-git.html). 

### Settings.php

The `settings.php` file contains sensitive data about the website such as the database connection, trusted host, and configuration information. 
This file should NOT be uploaded to Github. Instead, it should be backed up in a secure location. 

The file is also read-only by default which is required to ensure the site is secure, however, there are many instances where the administrator may have to edit this file. The instructions to do this are below.

#### Location of the settings.php file

The settings file can be found in: `/project-folder/web/sites/default/`

#### Making changes to settings.php

The `settings.php` file is read-only by default which is a security precaution to ensure the site is not able to alter it in any way. In order to make changes, you must locally change the permission of the file to give yourself write access, make the required changes, and once again harden the permissions. This ensures that the file contents are only changed in an authorized way and not by malicious actors. 

1.	Open the terminal and `cd` into the [settings.php directory](#location-of-the-settingsphp-file). 
2.	Make the file editable:

    ```shell title="Add write permissions to settings.php"
    chmod a+w settings.php
    ```

3. Make the required changes using a text editor like vim or nano.
4. Harden the permissions after editing the file: 

    ```shell title="Add write permissions to settings.php"
    chmod 444 settings.php (Results in permissions -r-r--r--)
    ```    

#### Frequently Required settings.php Changes

##### Trusted-Host

The trusted host settings tell the website which hosts are authorized to access the site. This is an important security measure to ensure that fraudulent hosts cannot be used to access the site or create unauthorized clones.

This setting essentially tells the site which URL is allowed to access the site, and it affects both the production/live version of the site and the local development version. In the event that the host name changes, the trusted host settings must be modified to reflect this. 

The trusted host setting looks like this in settings.php: 

```shell title="Trusted-host settings" linenums="1"
$settings['trusted_host_patterns'] = [
  '^www\.its-cab\.test$',
  '^its-cab\.test$'
];
```

This is an array containing multiple comma-separated lines with the various host patterns. The standard is to indent each line with two spaces. The patterns themselves are defined using REGEX. 

##### Update Free Access

This setting is used during core updates. 

Todo:

* Complete this section

## Roles

Roles are a way to define users and establish a list of permissions the user has. Users can be assigned different roles, and roles can be edited to change the name or the list of permissions they have.

By default, Drupal has three roles: 

* [Anonymous Users](#anonymous-users)
* [Authenticated Users](#authenticated-users)
* [Administrator Users](#administrator-user)

### Users

Users are accounts with usernames and passwords. Drupal sites come with a User 1 account and all other users must be created by the admin, or by new users requesting accounts on the site.

Learn more about [Users](https://www.drupal.org/docs/user_guide/en/user-concept.html ).

### Configuring User Account Settings

It is possible to configure many aspects of user accounts including whether users should be able to request/open their own accounts, or whether an admin has to do this step manually. 

Learn more about [Configuring Users](https://www.drupal.org/docs/user_guide/en/config-user.html).

### User 1

The User 1 account is the first account created along with the site and is known as the diety account in Drupal. This is because it has significant administrative powers and can be used to create other admin accounts. It also has the power to perform any action on the site regardless of permissions.

### Anonymous Users

Anonymous users are any users who visit the site without logging in. This is defined as a role and the permissions for these users can be separately configured.

### Authenticated Users

Authenticated users are any users who have accounts and can sign in. Any user who can sign in, regardless of whether they've been assigned a role, is an authenticated user.

### Administrator User

Administrator users have access to all of the functions of Drupal. They are not only able to create new content, but can define content types, structures, and have administrative control over roles and other users. 

### Additional Roles

Various roles can be created on the site. These roles can be named anything you want, and the permissions for each role can be customized in many ways. For example, it is possible what content an account can see, edit, create and delete. There is much more to the permissions system, so to get a thorough understanding of it, it is necessary to dig into the official Drupal docs. 

To see the specific roles which were created on the various Drupal sites, please visit the **Roles** section in the Specific Documentation for that particular site.

Learn more about [Managing User Accounts](https://www.drupal.org/docs/user_guide/en/user-chapter.html).

#### Creating a New Role

Learn about Creating a New Role. 

#### Assigning Permissions to a Role

Once a role is created, the permissions must be defined. It can be helpful to look at other roles’ permissions to see how they are set up, and create the permissions based off this. In some cases, you may need to dig deeper about which permission to add to a role in case you are trying to achieve a specific purpose. 

**Note:** It is NOT recommended to add a permission to a role if it will only benefit a single user. This would grant that permission to every user who is assigned to that role. In that case, it would be beneficial to create a new unique role for that user. 

Learn more about [Assigning Permissions to a Role](https://www.drupal.org/docs/user_guide/en/user-permissions.html).

#### Changing a User's Role

Roles must be assigned to new users so they can enjoy the permissions of that role. Users with no assigned roles are simply authenticated users. In order to assign special permissions to those users, it is recommended to assign a role to them and not to modify the permissions of authenticated users. 

Learn more about [Changing a User's Role](https://www.drupal.org/docs/user_guide/en/user-roles.html).

## Modules

Modules extend functionality and add features. Installing the feature adds functionality, while uninstalling it removes it. Modules increase the time needed to generate a page, so it is important to only keep modules you use installed and remove the unused ones. 

In general, modules will not be added to sites frequently. They will only be added to create a new feature that may require a contributed module in order to function correctly. 

Most actions we perform on modules will be maintenance as new versions are released.

### Installing Modules

Learn about [Installing Modules](https://www.drupal.org/docs/extending-drupal/installing-modules).

### Using Composer to Manage Dependencies

All the Humber ITS Websites are builit with Composer. This makes adding and updating modules very simple. 

Learn more about [Managing Dependencies using Composer](https://www.drupal.org/docs/develop/using-composer/manage-dependencies#adding-modules).

### Considerations Before Updating

The website might flag some modules that are outdated, but care should be taken before updating any module. It is important to go over the various modules in use on the site and check the module Drupal pages to see if there are any special instructions for carrying out updates. 

Furthermore, it is important to make sure we are using the correct module for the core version. If using Drupal 10, only modules compatible with Drupal 10 should be used. If a new version of a module is built for Drupal 11, the core files should be updated first, and only if it is deemed appropriate to update the core version.

I will go into further detail about maintenance further in this document. 

### Shared Modules

Shared Modules are any modules that are used across most or all of the Humber ITS Drupal sites. 

#### Admin Toolbar

[https://www.drupal.org/project/admin_toolbar](https://www.drupal.org/project/admin_toolbar)

Improves the default toolbar and changes it into a drop-down menu with fast access to each administrative page. 
In the Extend menu, the following options are also enabled: 

* Admin Toolbar Content
* Admin Toolbar Extra Tools
* Admin Toolbar Search

#### R4032 Login

[https://www.drupal.org/project/r4032login](https://www.drupal.org/project/r4032login)

This module redirects anonymous users to login if the content they are trying to view requires a certain level of permissions. 

#### Pathauto

https://www.drupal.org/project/pathauto

This module is used to automatically generate unique URL’s for new content. Default Drupal behavior results in URL snippets like `node/1`, but PathAuto can be used to create templates so that URLs might instead look like `photo/2` or `change-request/143`.

Learn more about using [PathAuto](https://ostraining.com/blog/drupal/pathauto-2/).

#### Gin Theme

Gin theme is an administrator theme with a number of useful submodules. 

##### Gin Admin Theme

[https://www.drupal.org/project/gin](https://www.drupal.org/project/gin)

Beautiful Admin theme.

##### Gin Login

[https://www.drupal.org/project/gin_login](https://www.drupal.org/project/gin_login)

Improved login page.

##### Gin Toolbar

[https://www.drupal.org/project/gin_toolbar](https://www.drupal.org/project/gin_toolbar)

Helper module that brings Gin theme to the admin toolbar.

#### Field Group

[https://www.drupal.org/project/field_group](https://www.drupal.org/project/field_group)

Allows you to group fields in forms so that they are easier to organize.

#### Field Permissions

[https://www.drupal.org/project/field_permissions](https://www.drupal.org/project/field_permissions)

Provides field level permissions for different roles.

## Administration

### Admin Toolbar

The Admin toolbar has quick links to various Administrative features. It has the ability to link to content creation pages, define new content types and structures, change the appearance of the site, add or remove modules, administrate people, generate reports and display site status, change the configuration of the site, and much more. The Humber ITS Websites use the [Gin Admin Theme](#gin-theme) which changes the look of the Admin toolbar into something more modern.

It is vital to familiarize yourself with the toolbar. 

### One Time Login

You can restore a user's access to the site by generating a one time login and sending this to them. To do this, see the [One Time Login](./developer-tools.md#one-time-login) section of these docs. 
