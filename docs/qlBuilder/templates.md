# Templates

Templates are a niche functionality. They are used in [create](commands.md#create) command to populate the initial script files and/or config file.

To create templates first create `C:\Users\<CURRENT USER>\qlbuilder_templates` folder. Inside it crate two more folder: `config` and `script`.

## Config templates

Inside the `config` folder are the config templates. The name of the files stored there is the config template name. 

When using the `create` command with template flag:

```
qlbuilder create my-project --config some-config-template
```

`qlBuilder` will create the default folder structure and will copy `some-config-template.yml` from `C:\Users\<CURRENT USER>\qlbuilder_templates\` folder into the new project folder and will overwrite the default `config.yml` file.

## Script templates

Inside the `script` folder are the config templates. The name of the folder there is the template name. Inside each folder create the required script files and `qlBuilder` will copy these files into the newly create project folder and will remove the default script files:

```
qlbuilder create my-project --script some-script-template
```

!!! info "Info"

    `create` command can be used with both `--config` and `--script` parameters at the same time
