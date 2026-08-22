# Main

The main purpose of `Automatiqal` is to help Qlik admin/support teams with the deployment, maintenance, configuration and administration of their Qlik instances.

!!! note

    At the moment only Qlik Sense on Windows is supported but SaaS integration is under development

## JS package

The main lift and shift done by the JS package [docs](./package/index.md).

## CLI

`Automatiqal CLI` ([docs](./cli/index.md)) is a wrapper around the JS package that can execute runbooks defined in YAML/JSON files.

Dedicated JSON schema can be used to help with the runbook structure.

Few key features:

- support majority of the available Qlik Repository API endpoints
- execute against multiple QS environments
- dynamic runbooks - define and use variables from files, global or inline
- loops based on external files
- define concurrency when processing entries
- import tasks from external files
- `when` conditions
