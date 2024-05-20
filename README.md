Custom Drupal Docs

This repository contains custom Drupal documentation and links to official docs. 

## Motivation

I have been building Drupal sites for a couple years and have built several Drupal sites professionally. After coming from a background in Python and React, I was disappointed with the poor state of Drupal official docs. Sometimes is difficult to tell which documentation is for older or newer versions of Drupal. When you find documentation for the right version, you can't always be sure the documentation is up to date or accurate. There are many amazing modules, some of which completely lack documentation. 

My goal with these docs is to aggregate the helpful and up to date documentation where it exists, and to create my own custom documentation compiling the lessons I learned. These docs were created for my own use, but if there is interest in others using these docs, I may open this up to public contributions.

## MkDocs

These docs were built using [MkDocs](https://www.mkdocs.org/).

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

