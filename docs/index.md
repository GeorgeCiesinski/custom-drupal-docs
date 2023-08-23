# Welcome

These docs are for Drupal websites built for Humber ITS. 



## Using these Docs

These documents provide information for Users (content editors) and Developers working on these sites. The information has been separated into **specific documentation** for each site, and general **Drupal Sites** documentation.

The recommended method of using the documentation is to check the specific documentation for the website, and then to check the Drupal Sites documentation. 

Alternatively, you can use the search feature to search across all of the pages.

In addition to general and specific site documentation, there are additional links in the navbar for various other useful sections. There, you can find information on backing up and restoring the database, performing maintenance, connecting to the developer environment, using the various developer tools, and a glossary. 

### Specific Documentation

These docs contain unique information for each site which is not relevant to other sites. Some examples of this are unique roles, modules, or structures that are used by one site but not the others. This documentation is most useful to regular users such as content editors, as it contains instructions how to create the specific content used on each site.

The specific documentation for each site can be found by clicking the corresponding site name at the top of the page.

### Drupal Sites Documentation

This is the documentation for all of the information that is relevant to every site. This includes general information about developing, administering and maintaining Drupal sites.

The [Drupal Sites](./drupal-sites.md) documentation can also be accessed by clicking the Drupal Sites tab at the top of the page.

## Development Information

### Start Date

The design & build process for the ITS websites started on April 2023. 

### Team

* **Lora Nasim** - Associate Director
* **George Ciesinski** – Web Developer
* **Michael Boadu** – Marketing and Communications Coordinator 

## Documentation

These docs were built using MkDocs and the Material for MkDocs theme.

### MkDocs

For MkDocs documentation visit [mkdocs.org](https://www.mkdocs.org).

For Material for MkDocs documentation visit [squidfunk.github.io](https://squidfunk.github.io/mkdocs-material/getting-started/).

### Commands

The below commands are usable in the virtual environment this project is installed in. 

* `mkdocs new [dir-name]` - Create a new project.
* `mkdocs serve` - Start the live-reloading docs server.
* `mkdocs build` - Build the documentation site.
* `mkdocs -h` - Print help message and exit.

### Project layout

    mkdocs.yml    # The configuration file.
    docs/
        index.md  # The documentation homepage.
        ...       # Other markdown pages, images and other files.
