# Development Environments

Drupal sites are developed across multiple development environments to ensure it is easy and safe to update them in the future. 

Development is done in a local environment and a staging environment. The finished websites live in the production environment. 

Each environment has it's own instance of the website source code, the MySQL database, and Apache Server to serve the content.

## Local Environment

The local environment is the primary location where the site will be developed. Most changes happen locally and are later pushed to staging for testing. 

### Exporting Config

When the local version of the site is ready to be pushed into staging or production, you must first export the config which will become necessary during the deployment process. 

1) Run Drush config export. This creates a config directory and config files in the `/web/sites/default/files/config_--xyz` folder. The folder name will include a randomly generated hash so it will be different in every site.

```shell title="Drush Config Export Process"
drush cex
```

This process must be done on first deployment, and every time updates are deployed. 

## Staging Environment

The staging environment is a server space used to upload and test the development version of the sites. It functions similarly to the production server allowing developers to push code changes, test updates, and more. Using the development environment provides a safe method of testing changes before pushing them to production. 

[Making a Development Site](https://www.drupal.org/docs/user_guide/en/install-dev-making.html)

### Deployment

#### First Deployment

The source code can be deployed to the staging or production servers by cloning the Git repository. Sites typically contain a `.gitignore` file which tells Git to ignore `/web/sites/default/files` directory and `settings.php`, so a few additional steps are required during deployment. 

1) [Import the Database backup](database.md#importing-data) to the server database. Ensure that you import the `57` version of the dump into SQL 5.7 databases. 
   
2) cd into the site directory, and clone the Git repository into the current `.` directory: 

```shell title="Clone the repository into the current directory using ."
git clone repository_url .
```

3) Use an SFTP app such as Cyberduck to drag the `/web/sites/default/files` folder and `settings.php` file into `/web/sites/default`.
   
4) Alter the `settings.php` file to update the database connection information at the bottom of the file. 
   
5) Run the drush deployment process below: 

```shell title="Drush Deployment Process"
composer install --no-dev
drush deploy
drush cache:rebuild
```

6) While still in the site root, change the permissions of files and folders to the below: 

```shell title="Drupal File and Folder Permissions"
chmod 775 -R *
chmod 777 -R web/sites/default/files
```

**Note:** The permissions instructions are contrary to Drupal documentation because the Server team has configured the security from their end and require Drupal sites to use the above hex permissions.

Learn more about the deployment process [here](https://drupal.stackexchange.com/questions/254407/order-of-drush-commands-for-automated-deployment). 

#### Pushing Updates

Changes made locally need to be pushed to the remote repository in order to be pulled into staging and production environments. Before pushing the changes, update the Drupal Config using `drush cex` which exports the configuration to a folder in `/files`. 

#### Deploying Updates

In order to push updates to a site in staging, follow the below process.

**Note:** Before starting, it is recommended to create a [Full Backup](backup-restore.md#creating-a-full-backup) of the staging or production environment.

1) First, enable maintenance mode and clear cache. Then reset the repository and pull the changes from the active branch (develop or main). 

```shell title="Pulling Changes"
drush state:set system.maintenance_mode 1
drush cache:rebuild
git reset --hard
git pull
```

2) Optional: Make any changes required in `settings.php` or the config if needed prior to deploying.

3) Deploy the changes and disable maintenance mode.

```shell title="Deploying Changes"
composer install --no-dev
drush deploy
drush state:set system.maintenance_mode 0
drush cache:rebuild
```

4) Test the staging or production site to ensure that it is working correctly. If you get an error that you do not have permission to access the site or a file, you must set the permissions once more: 

```shell title="Drupal File and Folder Permissions"
chmod 775 -R *
chmod 777 -R web/sites/default/files
```

### Using Load Balancer

Load Balancers can cause some unexpected behavior in Drupal sites such as some models thinking the site uses `http` instead of `https`. Fixing this requires configuration changes to be made in the `settings.php` file.

Learn more about [Using Load Balancers](https://www.drupal.org/node/425990). 

Learn more about [Drupal and Reverse Proxies](https://medium.com/@lmakarov/drupal-8-and-reverse-proxies-the-base-url-drama-c5553cbc9a3e). 

## Production Environment

The production environment is where the live site is hosted. 

It is not recommended to carry out core and module updates on the live/production version of the site because it could introduce breaking changes that are difficult to reverse. It may also result in an unexpected outage. Instead, it is recommended to set up a local development site, and only push changes to production after they've been thoroughly tested.

This development site is used for updates and major changes as well as for testing. Once the testing is complete, the changes can be pushed to the live site. 
