# Plugins

## Built-in

3 built-in plugins are available out of the box:

- `http` - send the notification data to a specified url (POST by default)
- `file` - write the notification data into a file (`json`. Each notification on a new line)
- `echo` - just logs the notification data into the `Publiqate` logs

## Community

There is a [dedicated repo](https://github.com/Informatiqal/publiqate-plugins) where community plugins will be hosted. We'll be keep adding plugins there but everyone is free to contribute.

- [SMTP](https://github.com/Informatiqal/publiqate-plugins/blob/main/plugins/smtp/README.md) - send emails. Using EJS template engine for the email body
- [S3](https://github.com/Informatiqal/publiqate-plugins/blob/main/plugins/s3/README.md) - store the notification data to a S3 bucket

## Custom plugins

`Publiqate` can load custom build plugins. The plugins should export a single JS function (`implementation`) and `meta` object. `Publiqate` will pass the data to the function and use the `meta` object to identify the plugin. The plugins can utilize whatever packages are needed and to be build to a ESM package (ideally into a single file).

The very basic plugin:

```js
export const meta = {
  author: "Author name",
  version: "0.1.0", // just for a reference
  name: "echo", // this is the important part
};

export function implementation(callback, notification, logger) {
  // do something with the notification data
}
```

3 arguments will be passed to the implementation function:

- `callback` - the part of the config callback that is associated with that notification
- `notification` - the actual notification data
- `logger` - logger instance. Write anything to the log file with it (instance of [Winston logger](https://github.com/winstonjs/winston))

`notification` argument have the following structure:

```js
{
  config: {}, // full notification config (from the config file)
  environment: {}, // Qlik env details (from the config file)
  data: [], // the notification data
  entities: [] // full Qlik entities data that raised the notification
}
```

`notification.data` have the same structure for all notifications:

```json
"data": [
    {
        "changeType": 2,
        "objectType": "ExecutionResult",
        "objectID": "a5852393-6d0c-4842-9ddf-3c49b1bd3446",
        "changedProperties": [
            "modifiedDate",
            "stopTime",
            "duration"
        ],
        "engineID": "",
        "engineType": "",
        "originatorNodeID": "6b3a6fa8-6f3d-4211-9823-75976a88623d",
        "originatorHostName": "some-host-name.com",
        "originatorContextID": null,
        "createdDate": "2024-11-19T07:37:20.031Z",
        "modifiedDate": "2024-11-19T07:37:20.156Z",
        "schemaPath": "ExternalChangeInfo"
    }
]
```

`notification.entities` property structure depends on what entity has triggered the notification. List of all entities and their structure can be seen on Qlik's [Repository API reference page](https://help.qlik.com/en-US/sense-developer/May2024/APIs/RepositoryServiceAPI/index.html#Methods)