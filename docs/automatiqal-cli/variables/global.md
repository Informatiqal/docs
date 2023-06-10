# Global

Variables can be defined into a global file as well. If `-g` or `--global` argument is passed then `Automatiqal` will try and load variables from `C:\Users\<USERNAME>\.automatiqal`.

Example:

- `automatiqal --file runbook.yaml --global`
- `automatiqal --file runbook.yaml -g`

!!! note "Structure"

    The structure of the config file is the same as the [dedicated variables file](./dedicated.md)
