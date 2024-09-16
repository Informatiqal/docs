# Loops

!!! note ""

    `Automatiqal v0.4+`

Task can be executed multiple times by using `loop` property. For example if the task is to create 3 tags then the following task can be written:

```yaml
- name: Create tags
  operation: tag.create
  loop:
    values:
      - Some name 1
      - Some name 2
      - Some name 3
  details:
    - name: { { item } }
```

`Automatiqal` will loop through the values, defined in the `loop` property and will run the same operation but replacing `{{ item }}` with the currently looped value.

## Values

`loop` property can contain either:

- array of mixed `string`, `numbers` or `boolean`
- array of objects in format `key: value`, where `value` can be mix of `string`, `number` or `boolean`

Examples:

```yaml
loop:
  values:
    - some name 1
    - 12345
    - true
```

```yaml
loop:
  values:
    - key: value
    - key: 12345
    - key: true
```

The following is **NOT allowed** since it will mix primitive values and objects:

```yaml
loop:
  values:
    - some name 1
    - 12345
    - key: some value
```

## Accessing values

- `{{ item }}` - will be replaced with the current loop value
- `{{ index }}` - will be replaced with the current index value (First element will be `1`)
- `{{ item.<key> }}` - when using objects in the `loop` property it will be replaced with the `<key>` value of the currently looped element. For example:

```yaml
- name: Create tags
  operation: tag.create
  loop:
    values:
      - name: Some name 1
        another: something 4
      - name: Some name 2
        another: something 5
      - name: Some name 3
        another: something 6
  details:
    - name: { { item.name } }
```

While looping `Automatiqal` will use the value for the `name` property to replace `item.name` with.

## Context

The loop syntax can be used anywhere in the task. And if object with multiple keys are used then different keys can be used for different purpose.

For example - if we have to publish multiple apps and each app should be published in a different stream then our task can look like:

```yaml
- name: Publish apps
  operation: app.publish
  filter: id eq {{ item.appId }}
  loop:
    values:
      - appId: 11111111-1111-1111-1111-11111111111
        appNewName: New app name
        stream: Some stream
      - appId: 22222222-2222-2222-2222-22222222222
        appNewName: Another new app name
        stream: Another stream
  details:
    stream: { { item.stream } }
    name: { { item.appNewName } }
```

## Task results

Since tasks with a `loop` are ran multiple times then the result of these will be the combined result from all the runs.

In our example above if we want to perform another task, that us using the result from `Publish apps` as a source, then this task will be performed on both published apps.

## Concurrency

By default the loops are executed in sequence. There is a task option that can overwrite this behavior and run the loops in parallel:

```yaml
- name: Create some tags
  operation: tag.create
  loop:
    values:
      - Some tag 1
      - Some tag 2
      - Some tag 3
  details:
    - name: { { item } }
  options:
    loopParallel: true
```

When `loopParallel` options is explicitly set to `true` then the loops will be ran in parallel.

## External files

Loop values can be provided by loading external files:

```yaml
- name: Create some tags
  operation: tag.create
  loop:
    location: c:\some\path\to\file.csv
    format: csv
  details:
    - name: { { item } }
  options:
    loopParallel: true
```

The example above will read the specified csv file and will loop through it's rows. If the csv have multiple columns then the `{{ item.<key> }}` syntax can be used.

### Formats

At the moment only 3 file formats are supported: `csv`, `json` and `yaml`

#### CSV files

- can have multiple columns
- should be comma separated
- UTF-8 encoded

#### JSON files

- contains json array
- UTF-8 encoded

#### YAML files
  
  - contains array
  - UTF-8 encoded
