# Schema

Great little addition is the availability of [YAML schema](https://github.com/Informatiqal/automatiqal-cli-schema). The schema greatly helps when writing runbooks.

## VSCode

First make sure you have [YAML (by Red Hat)](https://marketplace.visualstudio.com/items?itemName=redhat.vscode-yaml) extension installed. This extension will give YAML language support in VSCode.

After that there are two ways to use the schema: inline (local) and as user setting (global).

### **Inline**

Inline method is applied to each individual yaml runbook file.

!!! note

    If `--sample` or `-s` were used to generate sample files then the schema path will be automatically added

Add the following line to the top of the runbook yaml file:

`# yaml-language-server: $schema=https://github.com/Informatiqal/automatiqal-cli-schema/blob/main/schemas/runbook.json?raw=true`

Or if the schema is downloaded locally:

`# yaml-language-server: $schema=c:\path\to\runbook.json`

### **User settings**

- `Ctrl + Shift + p`
- search for `Preferences: Open User Settings`
- search for `schema`
- click on `JSON`
- click on `Edit in settings.json` (`JSON: Schemas` section)
- add new entry

  ```json
  {
    "fileMatch": ["/*.something.yaml"],
    "url": "https://github.com/Informatiqal/automatiqal-cli-schema/blob/main/schemas/runbook.json?raw=true"
  }
  ```

  `*.something.yaml` - the schema in this case will be applied to all files that have `something.yaml` in their name. Replace `something` with whatever you want.
