# Commands

## List of the available commands and their description

### file

The runbook file location

!!! example "Example"

    `automatiqal --file path\to\runbook.yaml`

    `automatiqal -f path\to\runbook.yaml`

!!! note "Non local files"

    If the `file` argument value is in url format, then Automatiqal CLI will attempt to download the runbook

### variables

The variables file location

!!! example "Example"

    `automatiqal --file path\to\runbook.yaml --variables path\to\variables.yaml`

    `automatiqal --file path\to\runbook.yaml -v path\to\variables.yaml`

### global (flag)

Use global variables file as values source. The file **should** be located at user's home folder and should be names `.automatiqal`

!!! example "Example"

    `automatiqal --file path\to\runbook.yaml --global`

    `automatiqal --file path\to\runbook.yaml -g`

### env (flag)

Use system/user environment variables file as values source

!!! example "Example"

    `automatiqal --file path\to\runbook.yaml --env`

    `automatiqal --file path\to\runbook.yaml -e`

### inline

Provide variables and their values directly in the command, that runs the runbook

!!! example "Example"

    `automatiqal --file path\to\runbook.yaml --inline "my-variable=value123; another-variable = 456"`

    `automatiqal --file path\to\runbook.yaml -i "my-variable=value123; another-variable = 456"`

### json (flag)

Indicates that the provided runbook is in JSON format (and not YAML)

!!! example "Example"

    `automatiqal --file path\to\runbook.json --json`

### output

Saves the raw result into the provided file. The result is always returned as JSON

!!! example "Example"

    `automatiqal --file path\to\runbook.yaml --output path\to\some-file.json`

    `automatiqal --file path\to\runbook.yaml -o path\to\some-file.json`

### raw (flag)

Display the raw result directly in the console/terminal. When this flag is used then all other console/terminal messages are suspended

!!! example "Example"

    `automatiqal --file path\to\runbook.yaml --raw`

!!! note "Further processing"

    Use this command if the result have to be processed outside Automatiqal

### summary

Saves the summary output in a file. This do **not** prevent the summary to be displayed on the terminal

!!! example "Example"

    `automatiqal --file path\to\runbook.yaml --summary path\to\some-file.txt`

    `automatiqal --file path\to\runbook.yaml -s path\to\some-file.txt`

### connect

Test if connection with Qlik can be established from the provided `environment` configuration. **No tasks are ran**

!!! example "Example"

    `automatiqal --connect`

### sample

Generate sample runbook and variables files in the **current folder**

!!! example "Example"

    `automatiqal --sample`

### help

Show list of the available commands

!!! example "Example"

    `automatiqal --help`
