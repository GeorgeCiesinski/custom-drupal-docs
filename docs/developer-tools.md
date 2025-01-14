# Developer Tools

This document outlines the tools used to develop the sites. With some of the tools, I have also outlined some frequently used processes used during the development of the sites. 

## Git

Git is a version control software which can be used to backup Drupal site directories. 

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

### Common Processes

#### Git Subtree

A git subtree is a way to include one Git repository inside of another. This is useful when a module or subdirectory of one project is used in other projects. In this case, we would create a subtree and push it to it's own repository. This can later be added in as a separate remote and pulled into other projects. The other major benefit is that when changes are made from different projects, those changes can be pushed to the subtree repository, and pulled into other projects, which makes maintenance easy. 

While git remembers your remote, it is unable to tell what subtrees you are using or track their changes independently of your project repo. For this reason, it is important to create a new subtree, pull changes, and push changes to it manually. 

Learn more about [Subtrees](https://youtu.be/JX5GPZjEWTo).
Learn even more about [Subtrees](https://youtu.be/sC1sfoCo5qY).

##### Creating a New Subtree

**Existing project:** This is the main project which the subtrees are located within.
**Subtree directory:** This is the folder containing the files you want to turn into a new subtree.
**Note:** Ignore this step if you are using an existing subtree.

1. Create a new repository on Github and copy the SSH link.
1. In the existing project, add the remote using an alias: `git remote add alias-name ssh-link`
1. Split the subtree directory into a separate branch named split: `git subtree split --prefix subtree-directory -b split --rejoin --squash`. The subtree directory is the path relative to your project root. It is good practice to rejoin and squash the commits so that the subtree doesn't contain your existing project history.
1. Push the subtree to the remove alias created earlier: `git push alias-name split:main`. Here, we are telling git to push the new "split" branch created in the last step to the remote repository with the alias "alias-name", and to push the split branch to this new repo's main branch. 

Upon completing these steps, the newly created git repository will contain the subtree files. 

##### Pulling Changes from Subtree

If the subtree you are using has had changes pushed to it, you can pull those changes into your own project. 

**Note:** To use the alias name, you must have added the repository link as an alias in [this section](#creating-a-new-subtree). Otherwise, use the git repository link in its place instead. 

1. cd into the project root as git sees it.
1. Pull the changes using `git subtree pull --prefix=subtree-directory --squash alias-name branch-name`

##### Pushing Changes to Subtree

If you have changes to push to the subtree, you can use the below process:

1. cd into the project root as git sees it.
2. Push the changes using `git subtree push --prefix=subtree-directory alias-name branch-name`

#### Tagging

Tagging is used to version certain commits so that they can be tracked and checked out if needed. 

##### Annotated Tags with Message

To standardize the process when working on the ITS sites, annotated tags with a message are recommended. While it is okay to use beta tags within a branch, most tagging should be done on the develop and main branches. This can be done after merging a feature into develop for instance: 

```
git checkout develop
git merge feature-branch
git tag -a v1.0.0 -m "Release version 1.0.0"
```

### Additional Info

Some more information can be found in these links. These are meant to get a better understanding of how Git can be used on a Drupal site, as well as best practices and the Git-flow method of creating branches. It should not be used in place of the other instructions in this document. 

[Building a Drupal Site with Git](https://www.drupal.org/docs/installing-drupal/building-a-drupal-site-with-git)

[Building a Drupal Site with Git](https://www.drupal.org/docs/installing-drupal/building-a-drupal-site-with-git)

[Successful Git Branching Model](https://nvie.com/posts/a-successful-git-branching-model/)


## Command-line

There are many command-line tools which are used throughout the development of the sites. This section covers some of the built-in utilities which don't fall under other categories. 

### Vim

Vim is a powerful terminal based text/script editor that is built into most unix based computers. You can open and edit files by using the command: 

```
vim filename
```

Vim is notorious for being pretty complex, which is an unfortunate side effect of how powerful it is. New users should look up [Vim Basics](https://www.redhat.com/sysadmin/beginners-guide-vim) or use an alternate text editor like nano if they prefer ease of use.

#### Modes

- **Normal Mode:** This mode allows you to give commands to the editor and carry out a number of functions.
- **Insert Mode:** This mode allows you to start inserting text into the file you are editing.
- **GUI Mode:**    This mode is only available in certain environments and gives you a graphical point and click interface to control the editor. 

## Apache

Apache server outputs the site locally to an IP address or domain. 

### Installing Apache

Install Apache using homebrew: `brew install httpd`

Learn more about [Installing Apache using Homebrew](https://tecadmin.net/install-apache-macos-homebrew/).

### Configuring Apache

The Apache configuration can be found in the directory `/opt/homebrew/etc/httpd/httpd.conf`.

The documentRoot is `/opt/homebrew/var/www`.

The default ports have been set in `/opt/homebrew/etc/httpd/httpd.conf` to `8080` and in `/opt/homebrew/etc/httpd/extra/httpd-ssl.conf` to `8443` so that httpd can run without sudo.

#### Testing Apache Configuration

With manually edited configuration files like this, it is easy to make mistakes. You can run the below command to check the configuration syntax: 

```
apachectl configtest
```

If all is well, it should output: `Syntax OK`

If there is some kind of a warning or error, it will instead output that error. An example of such an error is:

```
AH00557: httpd: apr_sockaddr_info_get() failed for (computer name)
```

There can be many different kinds of errors, so the best course of action is to copy the error and google it. To better be able to find the error, make sure you don't include some parts of the message that are unique to your setup, such as your computer name. 

### Using Apache

The server can be started and stopped using the command line. 

Start the Server: `brew services start httpd`

Stop the Server: `brew services stop httpd`

### Virtual Hosts

In order to use Apache to serve multiple sites, Virtual Hosts need to be enabled and configured.

#### Enable Virtual Hosts

1. Open `/opt/homebrew/etc/httpd/httpd.conf` using Vim
2. Change the Listen port to 80: `Listen 80`
3. Enable Virtual Hosts by uncommenting the line: `#Include /opt/homebrew/etc/httpd/extra/httpd-vhosts.conf`
4. Load the `mod_rewrite` module by uncommenting the line: `#LoadModule rewrite_module lib/httpd/modules/mod_rewrite.so`

**Note:** any web frameworks use it to enable “pretty URLs”, letting site visitors use URLs like `/posts/2021/some-post-title/` while translating them into URLs like `/index.php?p=697` for the back-end. 

5. Configure a server name to silence the warning that hostname cannot be determined: `ServerName www.example.com:8080` or `ServerName localhost:80`
6. Enable PHP by loading the PHP module: `LoadModule php_module /opt/homebrew/opt/php/lib/httpd/modules/libphp.so`
7. Add the below below the other include directives: 

```
# PHP settings
Include /opt/homebrew/etc/httpd/extra/httpd-php.conf
```

8. Open `/opt/homebrew/etc/httpd/extra/httpd-php.conf` with Vim and add the below PHP configuration: 

```
<IfModule php_module>
  <FilesMatch \.php$>
    SetHandler application/x-httpd-php
  </FilesMatch>

  <IfModule dir_module>
    DirectoryIndex index.html index.php
  </IfModule>
</IfModule>
```

Learn more about [Setting Up Virtual Hosts on macOS](https://www.git-tower.com/blog/apache-on-macos/).

#### Configuring Virtual Hosts

Now that Virtual Hosts have been enabled, you can create new configurations in the `/opt/homebrew/etc/httpd/extra/httpd-vhosts.conf` file. 

1. Open `/opt/homebrew/etc/httpd/extra/httpd-vhosts.conf` using Vim
2. Paste the below configuration for each website directory you want to host locally. Ensure you change the details as per the below explanation:

```
<VirtualHost *:80>
    ServerName its-site.test
    ServerAlias *.its-site.test
    DocumentRoot "/Users/ciesinsg/Documents/Repositories/its-site/web"

    <directory "/Users/ciesinsg/Documents/Repositories/its-site/web">
        Require all granted
        AllowOverride All
    </directory>
</VirtualHost>
```

**ServerName:** This is the root URL
**ServerAlias:** This is similar to the root URL but with a wildcard for the prefix
**DocumentRoot:** This is the web directory of the Drupal site
**Directory:** This is the same web directory as Document root

**Note:** The `.test` domain can be used to differentiate the sites from real existing domains and avoid conflicts when visiting the local sites. 

The other settings can be kept the same.

#### Defining a New Host

After [Configuring Virtual Hosts](#configuring-virtual-hosts), the final step required to be able to access the new site directory is to add the definition to the `/etc/hosts` file. 

1. Due to the `/etc/hosts` file permissions, open the file using Vim as a superuser: `sudo vim /etc/hosts`.
2. Add a new definition to the file: 

```
127.0.0.1       its.test
```

## Composer

In order to make maintenance easier, you can use [Composer](https://getcomposer.org/). 

#### Installing PHP

Homebrew is required in order to install PHP. Please visit the TechZone for help installing Homebrew. 

1. Search for and open “Terminal” in your app drawer. 
2. Run the command: `brew install php`
3. Test your PHP installation by running: `which php` -> which should output -> `/opt/homebrew/bin/php`

**Note:** These instructions are not guaranteed to work on Intel Macbooks and were created for Apple Silicon. 

#### Installing Composer

##### Requirements

1. Homebrew
2. PHP
3. Apple Mac with Apple Silicon chip (M1 or M2 chip)

**Notes:** 

1. It is recommended to install Composer in the `/usr/local/bin` directory as this directory is already included in the `$PATH`. This means that you can invoke composer in the Terminal without manually adding the composer install directory to `$PATH`. The steps to do this are outlined in the next section.
2. The steps after and including step 4 are taken from [these instructions](https://getcomposer.org/doc/). It is important to run the below steps 1-3 for installation on Apple Silicon (M1 & M2 chip) Macbooks because the /usr/local/bin folder is controlled by PAM by default, and simply following the instructions in the link will result in the Terminal blocking the command. 

##### Instructions

3. Search for and open **Terminal** in your app drawer. 
4. Change directories to `/usr/local/bin` by copying and pasting the following command into the Terminal: `cd /usr/local/bin`
5. Take ownership of the `/usr/local/bin` folder by copying and pasting the below command into the Terminal: 

```
sudo chown -R $(whoami) /usr/local/bin
```

If prompted for a password, enter your Microsoft 365 password.

4. Download install files running the command: 

```
php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');" 
```

5. Verify the download file integrity: 

```
php -r "if (hash_file('sha384', 'composer-setup.php') === 'e21205b207c3ff031906575712edab6f13eb0b361f2085f1f1237b7126d785e826a450292b6cfd1d64d92e6563bbde02') { echo 'Installer verified'; } else { echo 'Installer corrupt'; unlink('composer-setup.php'); } echo PHP_EOL;"
```

*Note:* If the Terminal outputs Installer verified, continue from step 6. If it instead outputs Installer corrupt, read the INSTALLER CORRUPT ERROR section below these instructions before continuing.

6. Install composer by running the command: `php composer-setup.php`

7. Delete the installer by running the command: `php -r "unlink('composer-setup.php');"`


8. Rename composer by running the command: `sudo mv composer.phar composer`

**Note:** This step is important for the Terminal to globally recognize commands starting with composer. Otherwise you will need to use `composer.phar` instead of `composer` which may result in unexpected bugs. 

9. Test the installation by running the command: `which composer ` -> which should output -> `/usr/local/bin/composer`

##### Installer Corrupt Error

It is very likely that you will get the error Installer corrupt when attempting Step 5 of this installation. The reason for this is that step 5 uses a hash to verify the integrity of the download, but the hash will change with each new version of composer. 

If this happens, start once again from Step 4. 

When you get to Step 5, visit this website https://getcomposer.org/download/ and copy & paste the line starting with `php -r "if (hash_file…` into the Terminal in place of the command listed in Step 5. This will ensure that the current and latest hash is used to verify the integrity of the file. 

Continue the remaining instructions until installation is complete.  

### Creating a New Website

Use the drupal/recommended-project-template to create a new project.

1. `cd` into your repository folder and create a new folder for the project with `mkdir project-name` where the project-directory is the name of the project.
2. Create a new project with `composer create-project drupal/recommended-project project-name`.
3. Add a [new Virtual Hosts configuration](#configuring-virtual-hosts) pointing at this new project directory.
4. Visit the URL you configured in Step 3 and follow the installation prompts.

Learn more about [Using Composer to Install Drupal](https://git.drupalcode.org/project/drupal/-/blob/11.x/core/INSTALL.txt).

## Drush

[Drush](https://www.drush.org/) provides command-line tools for Drupal Sites and is required to carry out some of the steps outlined further in these docs.

### Installing Drush

**Note:** If you cloned the existing site repositories, then it is likely that drush was automatically installed via the `composer.json` file in the site repository and does not need to be installed again. 

Drush can be installed using Composer: 

1. `cd` into the website root folder using Terminal.
2. Install drush using the command: `composer require --dev drush/drush`

### Drush Actions

There are a number of actions that can be performed using Drush. 

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

#### Updb

This is an alias for [updatedb](https://www.drush.org/11.x/commands/updatedb/). This essentially runs database updates like `update.php`.

```shell title="Drush updb"
drush updb
```

#### Watchdog

[Drush watchdog:show](https://www.drush.org/11.x/commands/watchdog_show/) outputs a list of errors that came up in Drupal. 

```shell title="Drush Watchdog"
drush watchdog:show
```

#### Uninstall Module

A common issue in Drupal is when you enable an outdated mod in the Extend menu and the site breaks down. In some cases it may display a `typeerror` or say the site cannot be reached right now. When this happens, it is not possible to go to the Extend menu and disable the newly enabled extension. In this case, you can use Drush to uninstall the module: 

```shell title="Drush Uninstall"
drush pm:uninstall module_name
```