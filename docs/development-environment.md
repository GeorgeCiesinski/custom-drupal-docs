# Development Environment

The development environment is a server space used to upload and test the development version of the sites. It functions similarly to the production server allowing developers to push code changes, test updates, and more.

Using the development environment provides a safe method of testing changes before pushing them to production.

## Server

The server used as the development environment is the Marketing Staging Environment (OWL).

**Url:** `dlh-mnfs01.humber.org`

**Alias:** `alias=owl.humber.org`

### Development Root

The GRMC team has provided access to the directory `/var/www/websites/departments/itsweb/its-new` which is accessible at [https://staging1.humber.ca/its-new/](https://staging1.humber.ca/its-new/).

New directories can be created within to house multiple sites. Those sites can then be accessed by appending the directory name to the above link. 

### Accessing Using SSH

**Note:** In order to access the Development Environment, you must be granted access by the GRMC team. Reach out to Lora Nasim to request access. 

The server can be accessed through SSH using the command:

```shell title="Accessing Owl using SSH"
ssh -l your_username dlh-mnfs01.humber.org
```

The server will prompt you to enter your Humber password.

You must then change directories to the [Development Root](#development-root) using the command:

```shell title="Change directories to Development Root"
cd /var/www/websites/departments/itsweb/its-new
```

### Uploading a Site to Development Root

Todo
