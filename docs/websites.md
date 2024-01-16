# Websites

## CAB

This document contains information specific to the Humber ITS - CAB Website. This includes modules, settings, roles and structures unique to the CAB Website.

Check out the [Drupal Sites documentation](./drupal-sites.md) for information that is shared across all of the Drupal Sites. 

### Source Code

[CAB Website Sourcecode](https://github.com/Humber-ITS/ITS-cab).

### Roles

The CAB site uses a number of unique roles in addition to the Admin role which are not present in the other sites. 

#### Chair

The chair role is specifically for the CAB chair and lets them view and edit any change request. This gives them the ability to alter several chair-only fields on the change request so that the status or success of the request can be tracked. Users with this role can also access restricted areas of the site such as the overview. 

#### Change Requester

The change requester role is for any users who need to request new change requests. Users with this role can only see and edit their own change requests and have limited access to the rest of the site. 

### Modules

The CAB site uses a number of unique modules which are outlined here. 

### Content Types

The CAB site uses the below custom content types:

#### Change Request

The change request content type contains many fields which are required to detail new change requests. 

The fields are broken up into [field group](#field-group) categories so that the fields can be grouped into bite sized chunks. 

The field groups used in change requests are: 

* **Basic Information** - Ownership, summary, dates, categories
* **Rationale & Scope** - Proposal and reasoning
* **Design** - Design process and individuals involved
* **Risk** - Risk assessment and reasoning
* **Release** - Release steps and required resources
* **Backout** - Backout plan in case of issues
* **Communication** - Communication plan
* **Review** - Change request status and review process

All of the sections except for **Review** are filled out by the [Change Requester](#change-requester). Once the change request is submitted, the [Chair](#chair) can edit the **Review** section to update the status of the change request. 

### Views

Views are a way to display content types and are part of Drupal core. With views, it is possible to show many different types of data and content using many different types of formats. In addition to the Drupal Core views, the CAB site uses some [contributed modules](#modules) which expand the functionality of views and allow additional format types. I have listed the views used on the CAB website below: 

#### Calendar

The calendar view takes all change requests and places them on a calendar. It makes use of the [Calendar View](#calendar-view) module. Clicking the item links will take you to the specific change request. 

#### Change Requests

The Change Requests view takes all change requests and lists them in a table by **Authored On** date. It also exposes filters to the user so they can filter the results by author, change requester, and/or by date. Clicking the item links will take you to the specific change request. 

#### Overview

The overview page takes all change requests and aggregates several change request fields into charts. There is also an exposed filter to search by date range. 

## ITS Website

This document contains information specific to the Humber ITS Website. This includes modules, settings, roles and structures unique to the main ITS Website.

Check out the [Drupal Sites documentation](./drupal-sites.md) for information that is shared across all of the Drupal Sites. 

### Source Code

[ITS Website Sourcecode](https://github.com/Humber-ITS/ITS-site).

### Modules

The below modules are unique to the ITS site. In addition to these, check out [Drupal Sites Modules](drupal-sites.md#modules) for all of the modules ITS shares with other Humber Drupal Sites.

## Directory

The Humber Directory is an existing site located at [https://humber.ca/directory/](https://humber.ca/directory/#). This site was developed by Perceptible and is hosted on `zandee.humber.org`. There is no staging environment setup for this site, so all changes must be made carefully in the production environment, or tested before hand on a local environment.

### Editing or Adding Departments

The department list is managed in the `perceptible_handler_filter_directory.inc` file within the site directory. This can be found at the below location and edited with a text editor.

```
sites > all > modules > custom > perceptible > views > perceptible_handler_filter_directory.inc
```

### Adding Employees to the Directory

Employee information is managed in Active Directory. 
