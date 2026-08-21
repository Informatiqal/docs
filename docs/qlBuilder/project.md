# Projects

Projects can live in any folder. And the general rule is: one project, one Qlik app.

## Create project

To create a project run:

```
qlbuilder my-project-name
```

This will create a new folder (`my-project-name`) into the current folder.

## Project folder structure

Once the project folder is created it will have the following structure:

```
.
├── dist/
│   └── LoadScript.qvs
├── src/
│   └── 1--Main.qvs
├── .gitignore
├── config.yml
└── README.md
```

The main entities here are:

- `dist` - where the final script is stored. `LoadScript.qvs` will be generated automatically
- `src` - this is the "main" folder. Each file in this folder represents script tab in Qlik. Each file naming convention is:
    - number representing the position of the tab in the main script
    - separator (`--`)
    - name of the tab in the script
    - extension (`.qvs`)

!!! info

    Files order and numbering can be managed with [section](./commands/#section) command

- `config.yml` - configuration for this project (see the next section)

## Setup project

`create` command will create a boilerplate `config.yml` outlining the possible configuration options.

This boilerplate file contains list of named environments. Each item in the list contains four main properties:

- `name` - used defined environment name
- `host` - url/machine name to which connection will be made
- `appId` - the ID of the managed app
- `authentication` what type of authentication will be used to establish connection

!!! warning "Important"

    The `name` property is important. Its value will be used to match a respective credentials in the [global config](index/#global-config)

### Authentication properties

Inside the `authentication` section the `type` property specifies the type of the authentication. The possible values are:

    - `winform`
    - `certificate`
    - `jwt`
    - `saas`

Additionally if connection is established to QSE (on-prem) and virtual proxy is used here we can specify the cookie name that the virtual proxy uses. Via `sessionHeaderName` optional property

### Other/optional properties

- `secure` - default is `true`. Connection to be established via `https (true)` or `http (false)`
- `trustAllCerts` - default is `true`. To ignore (`true`) all certificate errors (like self-signed certificates) or not (`false`) when establishing connection.

### Examples

!!! info "Important"

    To get the actual auth information (user/pass, jwt etc.) the `name` property is matched to the same value in the global config (`C:\Users\<CURRENT USER>\.qlbuilder.yml`)

#### Certificates (direct engine connection)

```yaml
- name: qse-certificates
  # for direct engine connection port should be specified.
  # The default engine port is 4747 (4848 for QS Desktop)
  host: my-qs-engine-host:4747
  appId: 12345678-1234-1234-1234-12345678901
  authentication:
    type: certificates
```

#### Virtual proxy and cookie name:

```yaml
- name: jwt-connection
  host: my-qs-host/virtual-proxy-prefix
  appId: 12345678-1234-1234-1234-12345678901
  authentication:
    type: jwt
    sessionHeaderName: X-Qlik-Session-JWT
```

#### Windows authentication

```yaml
- name: winform
  host: my-qs-host
  appId: 12345678-1234-1234-1234-12345678901
  authentication:
    type: winform
```

#### SaaS

```yaml
- name: saas
  host: my-tenant-url
  appId: 12345678-1234-1234-1234-12345678901
  authentication:
    type: saas
```
