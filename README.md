# Humber Websites Docs

This repository houses the documentation for multiple Humber ITS Websites: 

- its-site
- its-cab
- its-gallery

## Using the Docs

### (Name)-Website-Documentation.pdf

These PDFs are the first place to start when looking at the documentation for a site. Each of the Humber ITS sites has a document that follows this name convention.

These documents contain all of the unique information and instructions that are only relevant to that particular site. If you see any section that advises you to look at `Drupal-Websites.pdf` this means that the information in that section applies to multiple sites. 

### Drupal-Websites.pdf

This document contains all of the information and instructions that apply to all of the Drupal sites. This may include information such as backing up and restoring Drupal sites, updating core or modules, etc. 

## View Documentation

# Maintainer

A maintainer is someone who periodically adds content to the site without changing any code or structures in the site. If you are a maintainer, it is recommended to download the files in this repository normally. If any period of time passes between updates, it is advisable to visit this page again to see if the documentation has been updated and download the newest version. 

# Developer

A developer is someone who updates core or module files, or changes the content types and structures of the website. It is recommended for developers to clone the repository locally and always check for updates using:

```
git fetch
git pull origin main
```

## Update Documents

### Raw Files

The Microsoft docx files are located in the `/Raw/` folder. This is where any changes can be made to the docs themselves. 

When adding content, use the various headers to organize the content in a similar way to the rest of the document.

Prior to saving any changes, go to the Table of Contents, click the down arrow beside the Table of Contents header, and click `Update Table`. In the pop-up, click `Update entire table`. This updates the TOC with all of the newly added headers, as well as any page changes the updates caused. If the changes are significant enough, increase the version of the document on the cover page. 

Finally, save a PDF version of the document in the `humber-sites-docs` root directory (not in `/Raw/`). This can be done by clicking File > Print and then the PDF button in the bottom left of the print dialog. Once this is done, add the files to git, commit, and push them up to origin. 