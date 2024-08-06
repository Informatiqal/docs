# Import

Tasks can also be defined in external files and imported into the current runbook. The basic syntax is:

```yaml
- task:
  ...
  - import: path\to\some\file.yaml
  ...
```

Inside the external file we can define any set of tasks:

```yaml
# yaml-language-server: $schema=https://raw.githubusercontent.com/Informatiqal/automatiqal-cli-schema/main/schemas/tasks_only.json

- name: Get about
  operation: about.get
- name: List apps
  operation: app.getAll
```

The package will:

- load the main runbook file
- loop through the tasks and find `import` tasks
- loads the `import` file and replace the defined `import` tasks with the content of the loaded file
  - this loop will continue until no `import` task is found. This means that `import of import of import ...` is a valid scenario
- continue from here as usual

!!! warning

    Once all external files are loaded the resulted runbook should have unique names of tasks!

## Variables

Once all external files are loaded the defined variables will be replaced.

Since the external files can be templates and used in different runbooks we cant change the variables names to match the current runbook. For this reason when loading external files we can provide mapping of current runbook variables to external file variables.

Main runbook:

```yaml
- import:
    path: ./external-file.yaml
    vars:
      - blah: "TEMP"
      - blah1: "TEMP 123"
      - temp: "${host}"
```

external-file.yaml

```yaml
- name: Get some app
  operation: app.get
  filter: name eq '${meh}'
- name: Get another app
  operation: app.get
  filter: name sw '${meh1}'
- name: Get node
  operation: node.get
  filter: hostName sw '${temp}'e
```

In the example above we a passing additional `vars` property when loading external file. In this property we can define our mapping.

The example above will load `external-file.yaml` and will:

- replace `blah` variable with the string `TEMP`
- replace `blah1` variable with the string `TEMP 123`
- replace `temp` variable with the content of the `host` variable from the main runbook.
