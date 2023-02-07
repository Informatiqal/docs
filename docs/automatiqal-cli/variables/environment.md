# Environment

If `-e` or `--env` argument is provided `Automatiqal` will parse the available environment variables and will search for the variable names specified in the runbook.

For the values to be extracted the environment variables have to set prior to the start of the runbook.

Example:

- `automatiqal --file runbook.yaml --env`
- `automatiqal --file runbook.yaml -e`
