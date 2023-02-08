# Filter/source

When working with existing entities in QMC (apps, streams, reload tasks, virtual proxies etc.) we can use `filter`/`source` task property to define over which object(s) the task will be performed.

!!! warning

    the eligible tasks can have either `filter` or `source` property but not both!

## Filter

`Filter` property syntax follows [Qlik Sense REST API filtering](https://help.qlik.com/en-US/sense-developer/November2022/Subsystems/RepositoryServiceAPI/Content/Sense_RepositoryServiceAPI/RepositoryServiceAPI-Filtering.htm) syntax rules.

| Operator | Description           | String | Int | Enum | GUID | Char | DateTime |
| -------- | --------------------- | ------ | --- | ---- | ---- | ---- | -------- |
| eq       | Equal                 | x      | x   | x    | x    | x    | x        |
| ne       | Not equal             | x      | x   | x    | x    | x    | x        |
| gt       | Greater than          | -      | x   | x    | -    | x    | x        |
| ge       | Greater than or equal | -      | x   | x    | -    | x    | x        |
| lt       | Less than             | -      | x   | x    | -    | x    | x        |
| le       | Less than or equal    | -      | x   | x    | -    | x    | x        |
| sw       | Starts with           | x      | -   | -    | -    | -    | -        |
| ew       | Ends with             | x      | -   | -    | -    | -    | -        |
| so       | Substring of          | x      | -   | -    | -    | -    | -        |

For example:

- if the task will update apps (`app.update` operation) and the apps are identified by their common name prefix and are published then the filter will be:

  `name sw 'some prefix' and publishTime ne '1753-01-01T00:00:00.000Z'`

!!! note

    When working with GUID there is no need for `'` around it

## Source

The task entities can be sourced from the result from another task by passing the task name in the `source` property.

```yaml
- name: Get all apps
  operation: app.get
  filter: name sw 'some prefix' and publishTime ne '1753-01-01T00:00:00.000Z'
- name: Add new tag
  operation: app.update
  source: Get all apps
  details: ...
```

As we can see the `source` of the second task is the name of the first task (`Get all apps`). This way `Add new tag` task will be performed on the apps that are returned from a pervious task.

!!! note

    Any previous task can be used as source. Not only the immediate ones

!!! note

    `Automatiqal` will perform initial check if the tasks, used as a source, are the same type as the current task. So we can't perform `app.remove` over source that returns `Stream` entities (for example)
