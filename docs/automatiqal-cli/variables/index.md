# Variables

There are two type of variables that can be setup:

- [Static](#static)
- [Runtime](#runtime)

## **Static**

To define static variable just wrap the variable name into `${ }`. For example `${something}` is a variable named `something`.

```yaml
name: Sample run book
edition: windows
environment:
  host: ${host}
  port: 443
  proxy: ${virtual-proxy}
  authentication:
    token: ${jwt_token}
```

In the example above 3 variables are defined: `host`, `virtual-proxy` and `jwt_token`

### Definitions

There are few ways to define static variables:

- [dedicated file](./dedicated.md)
- [environment](./environment.md)
- [inline](./inline.md)
- [global](global.md)

All methods can be passed into a single command. If more than one method is used and a variable is found in multiple methods then the following priority is used (high to low):

1. inline
2. dedicated file
3. environment
4. global

!!! note

    Before the runbook is ran `Automatiqal` will check the defined variables and if it cant find it in the provided variables definitions it will exit immediately.

## **Runtime**

These variables have an limited use case but there are some workflows where the variable value is unknown until some of the previous task is ran. For example - creating of a virtual proxy and attaching it.

Behind the scenes creating virtual proxy is a 2 step process:

- creating virtual proxy
- attach the virtual proxy to a proxy

If the VP is not attached to a proxy service then its not operational.

The step that attaches the VP to a proxy requires the VP id and the id is unknown until the VP is actually created.

The following sample shows how to create VP and attach it to proxy

```yaml
- name: Create VP
  operation: virtualProxy.create
  details:
    prefix: test
    description: My new virtual proxy
    sessionCookieHeaderName: X-Qlik-Session-Test
    hasSecureAttributeHttps: true
    sameSiteAttributeHttps: None
    websocketCrossOriginWhiteList:
      - localhost
      - localhost:3000
- name: Add VP to proxy
  operation: proxy.update
  filter: Node eq 'Central'
  details:
    virtualProxies: [$${Create VP}]
```

The important bit is on the last row - `$${Create VP}`.

`$${..}` is an example of runtime variable. This will instruct `Automatiqal` to use the IDs, returned from `Create VP` task to populate the values.

By default the `ID` values will be extracted. If another property need to be passed then we can extend the syntax a bit:

```yaml
Create VP#websocketCrossOriginWhiteList
```

In this example `websocketCrossOriginWhiteList` property will be extracted from `Create VP` task result
