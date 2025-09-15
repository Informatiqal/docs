# Inline

Variables and their values can be specified directly in the command that starts the runbook. This is done by using `--inline` or `-i` arguments.

Example:

- `automatiqal --file runbook.yaml --inline host=my-sense-instance.com;virtual-proxy=jwt`
- `automatiqal --file runbook.yaml -i host=my-sense-instance.com;virtual-proxy=jwt`

!!! warning

    Specifying variables like this will keep the command in the commands history. Please try and do not pass sensitive (passwords, api keys, tokens etc.) data with this method.

!!! note

    For PowerShell please wrap the variables into double quotes. For example:

    `automatiqal --file runbook.yaml -i "host=my-sense-instance.com;virtual-proxy=jwt"`
