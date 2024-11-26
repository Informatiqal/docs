# Publiqate

`Publiqate` is an extendable notification handler for Qlik Sense (QSEoW).

QSEoW have [Notification API](https://help.qlik.com/en-US/sense-developer/May2024/Subsystems/RepositoryServiceAPI/Content/Sense_RepositoryServiceAPI/RepositoryServiceAPI-Notification-Create-Change-Subscription.htm) which can call specific URL when specified events are raised (like tasks failures, node/services down, any entity creation/deletion/update etc).

`Publiqate` abstracts the notifications registration and maintenance. It allows you to describe the notifications in yaml file and it will take care of the rest.

## Plugins

Few built-in and official plugins are available. For the list of available plugins and how to write custom plugin have a look a the [plugins page](./plugins.md)

## Config

Config is separated in 4 sections:

### General

Config the general behavior of `Publiqate`

```yaml
general:
  port: # on which port Publiqate will receive the notifications
  uri: "192.168.137.1"
  certs: # path to pem certificates if we need the qlik -> Publiqate comms to be https. Only valid certificates! If the certificate is not valid for some reason (self-signed for example) Qlik is not sending the notification!
  logLevel: # log levels: debug, info, error, warning, crit. Default is info
  vars: # check the "Config variables" section
  admin:
    port: # onl which port the admin UI to be started
    cookie: # name and value of a cookie to be accepted by the admin api endpoints
      name: # cookie name
      value: # cookie value
    certs: # path to pem certificates for the admin UI
```

### Qlik

Multiple Qlik instances can be defined. When `Publiqate` starts it will connect to all of them and will create the required notifications.

```yaml
qlik:
  - name: # name of the qlik environment'
    certs: # path to qlik's certificates
    host: # machine name of the central node
    userName: # which user to use when communicating with Qlik. Default sa_scheduler
    userDir: # above user's directory. Default INTERNAL
```

### Plugins (optional)

Define list with plugins to be loaded.

```yaml
plugins:
  - c:\path\to\plugin\index.js
  - c:\path\to\another\plugin\index.js
```

### Notifications

The "main" part where the actual notifications are defined.

```yaml
notifications:
  - type: ExecutionResult
    environment: # name of the Qlik environment
    name: # name of the notification
    id: # unique ID of the notification
    filter:
    condition:
    changeType:
    propertyName:
    options:
      disableCors: # if true the notification endpoint can be accessed from everywhere. Default is false
      whitelist: # list of hosts that can access the notification endpoint. Default is the Qlik central node machine name
        - my-link.com
        - 192.168.0.1
      enabled: # true or false. If false then when Qlik sends the notification nothing will be triggered here
      getEntityDetails: # if true Publiqate will retrieve full entity details from Qlik
    callbacks: # what to do when notification is received. Multiple callbacks can be triggered for single notification.
      - type: http # the name of the plugin
        details: # details associated with the plugin
          method: post
          url: http://localhost:3000
      - type: file # another plugin
        details:
          path: c:\some\file.json
```

## Config variables

It is possible to use variables file to store any sensitive values (passwords, api keys, secrets etc). The variables files location is specified in the `general.vars` property.

```txt
user_name=SomeUser
user_password=my-secret-password
```

```yaml
general:
...
vars: c:\path\to\variables.txt
...
callbacks:
  - type: smtp
    details:
      user: ${user_name}
      password: ${user_password}
```

In the config file variables are defined with `${...}`. From the above example the content of `${user_name}` will be replaced with the respective value from the `variables.txt` file.

Variables can be used for any property of the config file.

## Installation

You can either run:

```bash
npm install @informatiqal/publiqate
```

to install it in whatever location is needed OR clone the repo and run `npm install` and then `npm run build` from the folder where the clone is. The result code will be into `dist` folder.

## Windows service

In the near future there will be a section here that describes how to install the package as a Windows service.
