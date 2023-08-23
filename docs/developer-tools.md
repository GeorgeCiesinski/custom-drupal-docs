# Developer Tools

This document outlines the tools used to develop the sites. With some of the tools, I have also outlined some frequently used processes used during the development of the sites. 

## Command-line

There are many command-line tools which are used throughout the development of the sites. This section covers some of the built-in utilities which don't fall under other categories. 

### chmod

The `chmod` command is used frequently to change the permissions of directories and files in the Linux and Mac operating systems. 

#### Viewing and Understanding Permissions

To view the permissions of a directory or file, run the command `ls -l`. Alternatively, `ls -lh` also includes human readable file sizes in Mebibytes (as opposed to Megabytes). 

```title="View the permissions of files and directories in another directory"
ciesinsg@NB-00304-H14872 sites % ls -l default
total 168
-rw-r--r--@  1 ciesinsg  staff   9069 12 Jul 09:39 default.services.yml
-rw-r--r--@  1 ciesinsg  staff  35385 12 Jul 09:39 default.settings.php
drwxrwxr-x@ 10 ciesinsg  staff    320 24 Jul 15:56 files
-r--r--r--@  1 ciesinsg  staff  35997 13 Jul 09:09 settings.php
```

Each line shows the permissions for a file or a directory. The items that start with a `-` are files while the lines that start with `d` are directories. 

The next 9 characters are split into groups of three permissions, representing the `user`, `group`, and `other users` permissions in that order. The permissions are defined in a `read`, `write`, and `execute` order, where a dash means that permission is not granted to that user. 

```title="Permission Anatomy for Above Example"
#Directory Permission Example
d         rwx     rwx     r-x
directory
          owner | group | others
          all   | all   | Read and Execute

#File Permission Example
-         rw-              r--     r--
file
          owner          | group | others
          read and write | read  | read
```

- `r`: read
- `w`: write
- `x`: execute
- `-`: permission not granted

#### Changing Permissions

The chmod command can be used either with [symbolic](#symbolic-mode) mode or [numeric](#numeric-mode) mode. 

##### Symbolic Mode

Symbolic mode uses symbolic characters to add, remove or set permissions.

```title="Anatomy of symbolic chmod command"
chmod (users)(action)(permissions) (file or directory)
```

The users uses `ugoa` symbols where each means:

- `u`: `user` who owns the file or directory
- `g`: `group` user
- `o`: `other` users
- `a`: `all` of the above users

The actions available are: 

- `+`: Add permission
- `-`: Remove permission
- `=`: Set permission

The main permissions are:

- `r`: read
- `w`: write
- `x`: execute

```title="Add write permission for new_file.txt to all users"
chmod a+w new_file.txt
```

You can also chain permissions for different users with a comma:

``` title="Add read and write permissions for the owner, and read permissions for group and others"
chmod u=rw,go=r new_file.txt
```

**Note:** There are more permissions available at the [chmod manual](https://www.man7.org/linux/man-pages/man1/chmod.1.html) but these are less frequently used.

##### Numeric Mode

Numeric mode can be used as a chmod shorthand. It consists of three numbers representing the `user`, `group`, and `others` in that order. The numbers consists of one to four possible octal digits, derived by adding up the numerical representation of each permission.

- `read`: 4
- `write`: 2
- `execute`: 1

```title="All permissions for all users is represented by 777"
rwx rwx rwx
421 421 421
7   7   7
```

```title="Another example showing different permissions for each user, resulting in 756"
rwx r-x rw-
421 4-1 42-
7   5   6
```

Using this information, we can now demonstrate setting permissions using numeric mode. 

```title="Add write permission for new_file.txt to all users"
chmod 777 new_file.txt
```

You can also chain permissions for different users with a comma:

``` title="Add read and write permissions for the owner, and read permissions for group and others"
chmod 644 new_file.txt
```

The possible numerical combinations are: 

- `7`: All permissions
- `6`: Read and write
- `5`: Read and execute
- `4`: Read
- `3`: Write and execute
- `2`: Write
- `1`: Execute

Learn more about [Changing Permissions](https://www.man7.org/linux/man-pages/man1/chmod.1.html).

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

## MySQL

MySQL is the main database used in the Humber development and production environments. This section outlines some common use cases.

### Installation

You need to install two tools to use MySQL locally. 

1. MySQL Workbench

The [MySQL Workbench](https://dev.mysql.com/downloads/workbench/) is the software used to connect to the MySQL database. 

2. MySQL Community Server `8.0.xx`

The [MySQL Community Server](https://dev.mysql.com/downloads/mysql/) is the actual MySQL server you need to run locally. The MySQL Workbench can connect to this server. Currently, MySQL Workbench can only connect to MySQL Community Server `5.7.xx` or `8.0.xx`, so make sure you download an 8.0.xx version. 

### Server Setup

The following instructions are for setup on Mac. 

#### Add Root Connection

The root connection will be used to connect to the local server and write queries. This can be later used to create new users and databases.

1. Install the downloaded MySQL Community Server `8.0.xx`. During the installation it will ask you to set a password. Use the default password option and not legacy. Type in your password carefully as the software will not ask you to verify the password. 
2. Open System Settings and scroll to the bottom in the left panel. Select MySQL. In the right side of the window, click Initialize database and enter your password. Once this is complete, click "Start MySQL Server".
![MySQL Settings](assets/developer-tools/mysql-settings.png)
3. Open MySQL Workbench and click the + button.
![Add Connection](assets/developer-tools/workbench-add.png)
4. Fill in the connection information as per below:
![Connection Info](assets/developer-tools/connection-info.png)
1. Click "Test Connection". This should prompt a popup asking for your password. Fill it in, making sure to checkmark "Save Password" and click OK. 
![Connection Password](assets/developer-tools/connection-password.png)
1. If you see the below window, the connection is successful. In the Setup New Connection screen, you can now click OK and the connection will be saved. 
![Success](assets/developer-tools/success.png)

### Create New Database

Each local website requires a unique user and schema. In MySQL the schema is synomymous with database. The website is configured to use the created user to access the database. 

#### Create User

1. Connect to the root database setup in [Add Root Connection](#add-root-connection). 
2. Click Users and Privileges in the left side menu.
![users](assets/developer-tools/users.png)
3. Click Create User, and then fill out the username and password.
![create-user](assets/developer-tools/create-user.png)
4. Click Apply to save the new user.

**Note:** Make sure you set the host to localhost in order for the later steps to work.

#### Create Schema

1. Connect to the root database setup in [Add Root Connection](#add-root-connection). 
2. Click the Create New Schema button on the top toolbar.
![Create New Schema](assets/developer-tools/create-new-schema.png)
3. Name the new database, then select the Character Set `utf8mb4` and the Collation `utf8mb4_unicode_ci`. Click Apply, then Apply once again. 
![Configure Schema](assets/developer-tools/configure-schema.png)

#### Grant User Access to Schema

1. Connect to the root database setup in [Add Root Connection](#add-root-connection). 
2. In the Query tab, type the following SQL command where `itslocal` is replaced by the database, and `itsuser`@localhost is replaced by the username.

```sql
GRANT ALL PRIVILEGES ON itslocal TO itsuser@localhost;
```

3. Click CMD+Enter on Mac or Ctrl+Enter on Windows to run the query.

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