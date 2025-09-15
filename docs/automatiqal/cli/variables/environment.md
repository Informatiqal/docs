# Environment

If `-e` or `--env` argument is provided `Automatiqal` will parse the available environment variables and will search for the variable names specified in the runbook.

For the values to be extracted the environment variables have to set prior to the start of the runbook.

## Example

- `automatiqal --file runbook.yaml --env`
- `automatiqal --file runbook.yaml -e`

## Setting environment variable

Setting environment variable for the current PowerShell session is done like this:

```ps
$env:qlik_host="qlik-sense-host"
```

The above command will create new session environment variable called `qlik_host` and its value will be `qlik-sense-host`.

Once this is set if we start the runbook from the same terminal session then we can use `${qlik_host}` in our Automatiqal runbook.

!!! note "Permanent environment variables"

    Permanent environment variables are set from Windows Control Panel as a system wide or user specific ones
