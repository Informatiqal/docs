# Commands

## appDetails

> Alias: appdetails

Print information about the application (file size, publish time, stream etc.)

**Parameters:**

- environment name (mandatory)
- `--output` (optional) - the output will be saved into the provided file path

**Examples:**

```
qlbuilder appDetails [env]
```

```
qlbuilder appDetails [env] --output C:\some\path\output.txt
```

## build

Combine the tab script (from `src` folder) files into one and saves it into `dist` folder. Technically this is an internal command and there wont be much need to be ran manually

**Examples:**

```
qlbuilder build
```

## checkScript

> Alias: checkscript

Checks the local script for syntax errors against Qlik session app.

**Parameters:**

- environment name (mandatory)

**Examples:**

```
qlbuilder checkScript [env]
```

## create

Create new project structure in the current directory

**Parameters:**

- `-t, --tasks` - (optional) - create `.vscode` folder with `tasks.json` and `settings.json` files. Check [vscode](#vscode) command for more info
- `-c, --config` - (optional) replace the default `config.yml` with the specified template config
- `-s, --script` - (optional) replace the default files inside `src` folder with the files from the specified template folder

!!! info "Templates"

    Check out the [templates](../templates) page for more info about templates

## cred

List all credentials stored in `C:\Users\<CURRENT USER>\.qlbuilder.yml`.
If the yml file is encrypted then decryption password will be required.

**Examples:**

```
qlbuilder cred
```

## decrypt

Decrypts `C:\Users\<CURRENT USER>\.qlbuilder.yml` and overrides it with the decrypted content.

**Parameters:**

- `--view` (optional) - decrypts the file without overriding its content and show the decrypted content in te console
- `-p, --password` (optional) - provide the required password and skip the user input

!!! warning "WARNING"

    Use only if really needed. Providing the password like that will expose the password in the shell history

**Examples:**

```
qlbuilder decrypt
```

```
qlbuilder decrypt --password asd
```

```
qlbuilder decrypt --view
```
 
## download

Downloads the configured qvf

**Parameters:**

- environment name (mandatory)
- `--nd, --nodata` (optional) - default is `true`. Download the qvf without data
- `-p, --path` (optional) - default is the current folder. Provide path to a folder where the qvf will be saved

**Examples:**

```
qlbuilder download [env]
```

```
qlbuilder download [env] --nodata false
```

```
qlbuilder download [env] --path C:\path\to\some\folder
```

## encrypt

Encrypts `C:\Users\<USERNAME>\.qlBuilder.yml`. Password will be entered in the prompt.

**Parameters:**

- `-p, --password` (optional) - provide the required password and skip the user input

!!! warning "WARNING"

    Use only if really needed. Providing the password like that will expose the password in the shell history

**Examples:**

```
qlbuilder encrypt
```

```
qlbuilder encrypt --password asd
```

## getScript

> Alias: getscript

Get the remote script, splits it into files (tabs) and save the files inside `scr` folder. The order of the tabs is maintained

**Parameters:**

- environment name (mandatory)
- `-y` (optional) - By default the command will ask for explicit confirmation before remove the current files and replace them with the server version. This parameter skips the confirmation requirement

**Examples:**

```
qlbuilder getScript [env]
```

```
qlbuilder getScript [env] -y
```

## help

Prints the general help or specific command help.

**Examples:**

```
qlbuilder --help
```

```
qlbuilder getScript --help
```

```
qlbuilder download --help
```

## reload

> Alias: reload

Triggers reload of the requested app as if `Reload` button is pressed in Qlik's script editor. The reload progress is displayed in the console.

**Parameters:**

- environment name (mandatory)
- `-ro, --reload-output` - (optional) saves the reload log into the provided file location
- `-roo, --reload-output-overwrite` - (optional) similar to `reload-output` but the file name will always be `[appId].txt`. This way only the last reload log will be available

**Examples**

```
qlbuilder reload [env]
```

```
qlbuilder reload [env] -ro C:\path\to\some_log.txt
```

```
qlbuilder reload [env] -r0o C:\path\to\folder
```

## section

Manage script sections - add, remove and move script tab files without the need to manually re-name and re-order. Extra users input will be needed depending on the section action

**Examples:**

```
qlbuilder add
```

```
qlbuilder move
```

```
qlbuilder remove
```

## setScript

> Alias: setscript

Combines all `qvs` files  `src` folder, saves the output into `dist` folder, as a single file (`LoadScript.qvs`), and sets the content back to the Qlik app

**Parameters:**

- environment name (mandatory)

**Examples:**

```
qlbuilder setScript [env]
```

## tables

> Alias: fields

Print details for the tables and fields in the app

**Parameters:**

- environment name (mandatory)
- `--output` - (optional) is provided will also save the output to the specified file. If the file extension is `.md` then the command will apply extra styling and makes the output Markdown compatible

**Examples:**

```
qlbuilder tables [env]
```

```
qlbuilder tables [env] --output C:\path\to\locaiton\tables.txt
```


## templates

List the available config and script templates. (Check out the [templates](../templates) documentation for more info about templates)

**Examples**

```
qlbuilder templates
```

## vscode

Creates `.vscode` folder with pre-defined `tasks.json` and `settings.json` files

**Examples:**

```
qlbuilder vscode
```

## watch

> Alias: Watch

Starts `watch` mode. In this mode `qlBuilder` listens for file changes inside `src` folder. When change is detected, by default, `qlBuilder` will auto check the generated script for errors (against a session app). In this mode the console is "dynamic" and can accept commands directly - set script, reload, check syntax and clear the console.

**Parameters:**

- environment name (mandatory)
- `-r, --reload` - (optional) - after each file change reload the app
- `-s, --set` - (optional) - after each file change set the script back to Qlik
- `-d, --disable` - (optional) - disable the default behaviour and do not check the app for script errors after each file change

**Examples:**

```
qlbuilder watch [env]
```

```
qlbuilder watch --set [env]
```
