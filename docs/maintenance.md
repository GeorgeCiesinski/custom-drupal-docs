# Maintenance



## Core and Module Update

Drupal core and the various modules the site uses are frequently updated to address security issues and bugs that come up over time. In many cases, it is beneficial to update these components to ensure that the Humber site is secure and working correctly.

### How to tell there's updates?

Updates can be checked using the Admin toolbar on the website by visiting the Available Updates section. 

`Reports` > `Available Updates`

This will show a screen like this which shows if Drupal core and modules are up to date:

![Available Updates](assets/drupal-sites/updates.png)

If any of these components do not show `Up to date` and a green check mark, those components may have updates.

### Should I update?

Updates should be made if they address a security flaw or a bug that may affect the website. You can check this by visiting the pages of the Drupal core version or the module project pages and reading the release notes. Any significant changes will be listed there, and a decision can be made based on this information.

Learn more about [Updating Core Software](https://www.drupal.org/docs/user_guide/en/install-composer.html).

Learn more about [Updating Modules](https://www.drupal.org/docs/user_guide/en/security-update-module.html).

Learn more about [Updating Themes](https://www.drupal.org/docs/user_guide/en/security-update-theme.html).

### Testing Updates

Updates should be made on the development website and tested thoroughly before pushing to production. This would help with catching any incompatibilities or issues that may come up from the updates.  