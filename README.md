# Humber Websites Docs

This repository houses the documentation for multiple Humber ITS Websites: 

* its-site
* its-cab
* Humber Drupal Theme

These docs are built using [MkDocs](https://www.mkdocs.org/) and the [Material Theme](https://squidfunk.github.io/mkdocs-material/). MkDocs is a static site generator that converts markdown files into beautiful documentation and makes the process of creating documentation very quick and easy.  

## Using the Docs

These docs need to be cloned and served locally before use.

### Cloning Repository

#### Requirements

* Github CLI or Git

#### Steps

<ol>
    <li>Click the <code><> Code</code> button and select <code>GitHub CLI</code>, then click the <strong>copy</strong> button.</li>
    <img src='img/clone.png'></img>
    <li><code>cd</code> into the directory where you want to clone this project using Terminal.</li>
    <li>Paste and run the command you copied in step 1.</li>
</ol>

### Local Setup

These steps are for first time setup after cloning this repository. 

#### Requirements

* Cloned Repository (See above)
* Python 3 (3.11.4 or later recommended)
* Pip3

#### Steps

**Note:** The following instructions are applicable on Mac and Linux, so Windows users may need to look up how to create and activate a virtual environment. You also may need to use `python` and `pip` instead of `python3` and `pip3`. This depends on your local setup and how the binaries were added to your path during Python setup. 

<ol>
    <li><code>cd</code> into the newly cloned repository using Terminal.</li>
    <li>Create a virtual environment using the command:</li>
    <code>python3 -m venv venv</code>
    <li>Activate the virtual environment using the command:</li>
    <code>source venv/bin/activate</code>
    <p>If this is done correctly, you should see <code>(venv)</code> in the Terminal.</p>
    <li>Install the requirements using the command:</li>
    <code>pip3 install -r requirements.txt</code>
    <li>In the terminal where you activated venv, run the command:</li> 
    <code>mkdocs serve</code>
    <p>This will generate the site and show you the link where you can access these docs.</p>
</ol>

#### Build

MkDocs can also build the production version of the documentation site. Use the command `mkdocs build` to build the production version. This will update the `site/` directory with the files and assets needed to open the site on the production server. 

### Pushing Updates

Updates can be safely pushed to the develop branch. This should be updated frequently and kept up-to-date with source material.
