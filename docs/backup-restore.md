# Backup and Restore

## Site Maintenance

The Site Maintenance message informs regular users the site is under maintenance. Authorized users can maintain the site and update content during this time. 

[Enabling and Disabling Maintenance Mode](https://www.drupal.org/docs/user_guide/en/extend-maintenance.html)

## Backups

It is important to back up the website frequently to prevent data loss. A backup consists of the site's source code, and a MySQL dump of the database. 

It is an especially good idea to carry out a full backup when:

* Updating or Upgrading
* Migrating, copying, moving, or replacing files or the whole site
* When there has been significant content added

Backups should also be tested periodically to ensure it is possible to restore the site from the backups.

### Backup Conventions

In order to stay consistent, backups should be placed into separate folders for each site. 

Each separate backup should be placed in its own folder with the name <code>YYYY-MM-DD version</code>.

This folder should contain the MySQL dump, and the source code for the site.

```title="Folder Structure"
/cab backups                    # Contains all cab backups
    /2023-11-14 v0.0.1          # Contains specific backup files
        2023-11-14-cab.sql      # MySQL dump
        cab.tar.gz              # Site root folder (source code)
```

```title="Version Name"
0.0.x   # PATCH contains bug fixes, or small changes
0.x.0   # MINOR change contains new features or branch merges
x.0.0   # Major version signifies public release state

/*
Note on release state: 

The major version 0 is for unreleased development versions of the site. Once the site is released, the version changes to 1. If enough minor changes and patches occur, the major version can change with the next push to production. 
*/
```

### Creating a Full Backup

Creating a full backup is a multi-step process:

1. [Turn off Cron Jobs](#turn-off-cron-jobs)
1. [Clear Cache](#clear-cache)
1. [Loosen Permissions](#loosen-permissions)
1. [Back up Site Files](#backup-site-files)
1. [Back up Database](#backup-database)
1. [Turn on Cron Jobs](#turn-on-cron-jobs)
1. [Harden Permissions](#harden-permissions)

**Note:**  

#### Turn off Cron Jobs

It is highly recommended to turn off CRON jobs before making a Full Backup as the CRON jobs can be resource intensive. 

1. Select <code>Configuration</code> > <code>System</code> > <code>CRON</code> within the Admin toolbar.

![Navigate to CRON](assets/backup-restore/cron-menu.png){ width="400" }

2. Change **Run cron every** to **Never**.

![Navigate to CRON](assets/backup-restore/run-cron-never.png){ width="500" }

#### Clear Cache

It is a good practice to clear or rebuild caches when moving a site from one host to another. This can also be useful when installing new modules or themes as it is often a first step in troubleshooting. It is recommended to clear the cache frequently throughout development. 

**Note:** Sites may slow down a bit after being cleared as the cache slowly fills back up. This is a normal side-effect of clearing the cache. 

##### Clearing Cache using Drush

The quickest and easiest way to clear the cache is to use Drush. The steps to do this are described in [Developer-tools/Drush](developer-tools.md#clear-cache).

##### Steps to Clear Cache Manually

1. Select <code>Configuration</code> > <code>Development</code> > <code>Performance</code> within the Admin toolbar.

![Navigate to Performance](assets/backup-restore/cache-menu.png){ width="400" }

2. Click **Clear all caches**.

![Clear all Caches](assets/backup-restore/clear-cache.png){ width="400" }

#### Backup Site Files

The source code can be backed up using the CLI:

1. You can [use tar](developer-tools.md#gzip--tar) to back up the entire [root folder](glossary.md#root-folder). 

```title="Zip project directory using tar"
tar -czvf YYYY-MM-DD-project.tar.gz cab

# Replace the YYYY-MM-DD with the date, and project with the project name
```

2. Move the resulting file into the backup folder described in [Backup Conventions](#backup-conventions).

#### Backup Database

The database can be backed up using MySQLWorkbench. To back up the database:

1. Open the MySQLWorkbench app and connect to the database.
2. In the menu, click **Server**, then click **Data Export**.

![Data Export](assets/backup-restore/data-export.png){ width="400" }

3. Under "Tables to Export" checkmark the database name. Under "Export Options" select **Export to Self-Contained File**

![Start Export](assets/backup-restore/start-export.png){ width="600" }

4. In the space beside **Export to Self-Contained File**, feel free to change the directory, and rename the file as per [Backup Conventions](#backup-conventions). Then click Start Export.

5. Move the resulting file into the backup folder described in [Backup Conventions](#backup-conventions).

#### Turn on Cron Jobs

Once the database has been backed up, Cron jobs can and should be enabled once again. To do this, simply repeat the steps to [Turn off Cron Jobs](#turn-off-cron-jobs) but instead, change **Run cron every** to **3 hours**.

## Development Site

It is not recommended to carry out core and module updates on the live/production version of the site because it could introduce breaking changes that are difficult to reverse. It may also result in an unexpected outage. Instead, it is recommended to set up a local development site which is essentially a clone of the live/production server. The process of setting up a local development site can also be used to verify the integrity of the backup files. 

This development site is used for updates and major changes as well as for testing. Once the testing is complete, the changes can be pushed to the live site. 

[Making a Development Site](https://www.drupal.org/docs/user_guide/en/install-dev-making.html)

## Restore

This section is about restoring the website from a backup.

## Pushing Website

This section focuses on pushing the website to the staging server. 

### Using Rsync to Transfer Website

Rsync is a command-line utility used to transfer files and directories. It can also be used over SSH making it ideal to transfer the site via SSH. 

#### Pushing from Local to Staging

### Removing Website from Staging

**Warning:** This action is very dangerous and should be used carefully. It should not be used in production without reaching out to the lead developer as it first grants ALL linux permissions to all users and groups, and then deletes the directory and its contents. It should only be done if there is a significant enough issue to purge the directory and reupload it.

1. Use Terminal to cd into the Staging directory. 

2. Grant 777 permissions to the directory and its contents. This step is required as certain Drupal files are setup with limited permissions and prevent recursive removal if the permissions aren't changed.

```shell title="Grant 777 permissions to directory recursively"
chmod -R 777 its-cab
```

3. Remove the directory and its contents recursively. 

```shell
rm -rf its-cab
```