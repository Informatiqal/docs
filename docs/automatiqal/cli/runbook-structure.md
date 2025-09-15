# Runbook structure

The runbook have 3 main parts:

- [Header](#header)
- [Environment](#environment)
- [Tasks](#tasks)

## Header

```yaml
name: Sample run book
description: Short description
trace: error # optional log level
edition: windows # optional QS edition. Only QSEoW is supported at the moment
```

## Environment

In this section is defined how and where to connect to QS.

Check out [Authentication](./authentication.md) section for more information on the available authentication methods.

The code block below shows how to establish connection using JWT token

```yaml
environment:
  host: my-sense-instance.com
  port: 443
  proxy: jwt
  authentication:
    token: ${jwt_token}
```

## Tasks

This section includes all the operations that the runbook will perform. The tasks are **executed in sequence**

The runbook below have 7 tasks:

- `Create custom property` - create new custom property for `Stream` objects and defines 3 values
- `Create tag` - create new tag with 3 values
- `Create stream` - creates new stream and applies 2 custom property value and 2 tag values
- `Update stream` - update the stream, that that was create in `Create stream` task by adding one additional custom property value and replace the existing tags in new tag

```yaml
tasks:
  - name: Create custom property
    description: create new custom property with 3 values and applied to streams objects
    operation: customProperty.create
    details:
      name: NewCustomProperty
      choiceValues:
        - value 1
        - value 2
        - value 3
      objectTypes:
        - Stream
  - name: Create Tags
    description: create 3 new tags
    operation: tag.createMany
    details:
      names:
        - Some Tag 1
        - Some Tag 2
        - Some Tag 3
  - name: Create stream
    description: create new stream and apply 2 CP values and 2 tags
    operation: stream.create
    details:
      name: New Stream
      customProperties:
        - NewCustomProperty=value 1
        - NewCustomProperty=value 2
      tags:
        - Some Tag 1
        - Some Tag 2
  - name: Update stream
    description: >
      update the created stream by appending new 
      custom property value
      and overwriting the existing tags
    operation: stream.update
    filter: name eq 'New Stream'
    options:
      appendCustomProps: true
      appendTags: false
    details:
      tags:
        - Some Tag 3
      customProperties:
        - NewCustomProperty=value 3
```
