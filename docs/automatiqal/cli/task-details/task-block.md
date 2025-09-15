# Task

Each task can be "split" into two sections - meta and details

## Meta

```yaml
- name: Create custom property
  description: create new custom property with 3 values and applied to streams objects
  operation: customProperty.create
```

- `name` - user defined.
- `description` - (optional) short description of the task
- `operation` - valid [operation](./operations-list.md)
- `skip` - (optional) `boolean`. If `false` then the task will not be executed. Default value is `false`

!!! Warning

    `name` should be unique for the whole runbook! Including the [onError](../on-error-block.md) tasks

## Details

Details properties depends on the [operation](./operations-list.md)

!!! note

    Check out [schema](../schema.md) section for more info how to use the available JSON schema to help with details properties

Few examples:

### `customProperty.create`

```yaml
details:
  name: NewCustomProperty
  choiceValues:
    - value 1
    - value 2
    - value 3
  objectTypes:
    - Stream
```

### `app.publish`

```yaml
details:
  stream: New Stream
  name: Something New (Published)
```

### `virtualProxy.create`

```yaml
details:
  description: New VP
  prefix: new
  sessionCookieHeaderName: X-Qlik-Session-New
  authenticationMethod: HeaderDynamicUserDirectory
  loadBalancingServerNodes:
    - ${host}
  websocketCrossOriginWhiteList:
    - ${host}
  headerAuthenticationHeaderName: hdr-usr
  headerAuthenticationDynamicUserDirectory: $ud\\$id
  hasSecureAttributeHttp: false
  hasSecureAttributeHttps: true
  sameSiteAttributeHttps: None
  extendedSecurityEnvironment: true
```

### `dataConnection.create`

```yaml
details:
  name: Test data connection
  connectionString: CUSTOM CONNECT TO "provider=QvRestConnector.exe;url=https://localhost/qrs/app/full;timeout=900;method=GET;autoDetectResponseType=true;keyGenerationStrategy=0;authSchema=ntlm;skipServerCertificateValidation=true;useCertificate=No;certificateStoreLocation=LocalMachine;certificateStoreName=My;trustedLocations=qrs-proxy%2https://localhost:4244;queryParameters=xrfkey%20000000000000000;addMissingQueryParametersToFinalRequest=false;queryHeaders=X-Qlik-XrfKey%20000000000000000%1User-Agent%2Windows;PaginationType=None;"
```

### `externalTask.addTriggerMany`

```yaml
details:
  - name: Weekly trigger
    repeat: Weekly
    startDate: 2022-02-07T12:30:00.000
    daysOfWeek:
      - Monday
      - Wednesday
      - Sunday
  - name: Monthly trigger
    repeat: Monthly
    enabled: false
    startDate: 2022-02-07T13:30:00.000
    daysOfMonth: [1, 5, 10]
```
