# Development Environments

The Humber ITS Drupal sites are developed across multiple development environments to ensure it is easy and safe to update them in the future. 

Development is done in a local environment and a staging environment. The finished websites live in the production environment. 

Each environment has it's own instance of the website source code, the MySQL database, and Apache Server to serve the content.

## Local Environment

The local environment is the primary location where the site will be developed. Most changes happen locally and are later pushed to staging for testing.

## Staging Environment

The staging environment is a server space used to upload and test the development version of the sites. It functions similarly to the production server allowing developers to push code changes, test updates, and more. Using the development environment provides a safe method of testing changes before pushing them to production. 

[Making a Development Site](https://www.drupal.org/docs/user_guide/en/install-dev-making.html)

## Production Environment

The production environment is where the live site is hosted. 

It is not recommended to carry out core and module updates on the live/production version of the site because it could introduce breaking changes that are difficult to reverse. It may also result in an unexpected outage. Instead, it is recommended to set up a local development site, and only push changes to production after they've been thoroughly tested.

This development site is used for updates and major changes as well as for testing. Once the testing is complete, the changes can be pushed to the live site. 
