# Motivation

`qlBuilder` is a CLI tool which is ran from the command prompt. The tool allows Qlik Sense developers to write their Qlik scripts locally and to communicate with Qlik instance to:

- set the reload script into configured Qlik app
- reload app (the reload is performed on the Qlik instance itself)
- check for syntax errors while developing without the need to save the whole app (the syntax check is performed against temporary session app)
- download Qlik app(s) with or without data
- and more

one project = one app

# Installation

> npm install -g qlbuilder

Once the global package is installed `qlbuilder` command can be used from CMD/PowerShell.

The next steps are:

- [create and setup your first project](./project)
- setup the global config

# Global config

All authentication information is kept outside the project folders inside `C:\Users\<CURRENT USER>\.qlbuilder.yml` file.

The structure of the file is simple and contains the Qlik environment name and the credentials used to connect to it. For example:

!!! info "Important"

    The environment names shuold be unique

!!! info "Info"

    The file can be encrypted. Have a look at [encrypt](./commands/#encrypt) and [decrpyt](./commands/#decrypt) commands

```yaml
dev:
  QLIK_USER: DOMAIN\my-dev-user
  QLIK_PASSWORD: my-dev-password
prod:
  QLIK_USER: DOMAIN\my-prod-user
  QLIK_PASSWORD: my-prod-password
dev_jwt:
  QLIK_TOKEN: eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...
prod_cert:
  QLIK_CERTS: c:\path\to\cert\folder
  QLIK_USER: DOMAIN\UserName
```

The above example defines 4 Qlik environments and 3 different types of authentication - two Windows, one with JWT and one with certificates.
