# API

## Exported methods and properties

```javascript
import { sse, server } from "@informatiqal/qlik-sse";

OR

import * as whatever from "@informatiqal/qlik-sse";

// whatever.see
// whatever.server

```

### `sse`

A namespace for easy access to types, all types in [SSE.proto](https://github.com/Informatiqal/qlik-sse/blob/master/assets/SSE.proto) are accessible from this namespace:

```js
import { sse } from "@informatiqal/qlik-sse";

console.log(sse.FunctionType.AGGREGATION); // 1
```

### `server(options)`

- `options` <[Object]>
    - `identifier` <[string]> Identifier for this SSE plugin.
    - `version` <[string]> Version number of the SSE.
    - `allowScript` <[boolean]> **OPTIONAL** Whether to allow script evaluation. Defaults to `false`.
    - `disableBuildIn` <[boolean]> **OPTIONAL** Disable/enable loading of all build-in functions. Default is `false`
    - `ssl` <[Object]> **OPTIONAL**
        - `root` <[string]> - path to `root_cert.pem`
        - `cert` <[string]> - path to `sse_server_cert.pem`
        - `key` <[string]> - path to `sse_server_key.pem`

- returns: <[Server](#server)>

Creates a new [Server](#server) instance.

```js
import { server } from "@informatiqal/qlik-sse";

const server = server({
  identifier: 'abc',
  allowScript: true,
  disableBuildIn: false,
  ssl: {
    root: "path/to/root.pem",
    cert: "path/to/sse_server_cert.pem",
    key: "path/to/sse_server_key.pem",
  }
});
```

## `Server`

### `server.start(options)`

- `options` <[Object]>
    - `port` <[number]> Port to run the server on. Defaults to `50051`.

Starts the server.

### `server.close()`

Stops the server.

### `server.addFunction(fn, config)`

- `fn` <[function] ([Request](#request))>
- `config` <[Object]>
    - `functionType` <[FunctionType]> Type of function
    - `returnType` <[DataType]> Type of data this function is expected to return
    - `params` <[Array]<[Object]>>
    - `name` <[string]>
    - `dataType`: <[DataType]>
    - `tableDescription` <[TableDescription]> Description of the returned table when function is called from load script using the `extension` clause.

Register a function which can be called from an expression.

```js
const fn = (request) => {/* do stuff */};
server.addFunction(fn, {
  functionType: q.sse.FunctionType.TENSOR,
  returnType: q.sse.DataType.NUMERIC,
  params: [{
    name: 'first',
    dataType: q.sse.DataType.NUMERIC
  }]
})
```

### `server.listAllFunctions()`

Metadata about all registered functions (enabled and disabled)

```js
[
 {
    name: "ListAllFunctions",
    description: "Build-in function that returns information about the available functions",
    functionType: 1,
    returnType: 0,
    params: [{
        name: "Table",
        dataType: 0,
      }],
    functionId: 1002,
    enabled: true,
  },
  ...
]
```

### `server.disableFunction(functionId)`

- `functionId` <[number]> - id of the function to disable

```javascript
s.disableFunction(1001);
```

Disable specific function. The function is identified by its `id`.

!!!note

        Once the function is disabled and if the function is called from Qlik then the package will responds with an error and it will close the connection. This will make the Qlik script to fail.

### `server.enableFunction(functionId)`

- `functionId` <[number]> - id of the function to enable

```javascript
s.enableFunction(1001);
```

Enable specific function. The function is identified by its `id`.

### `server.removeFunction(functionId)`

- `functionId` <[number]> - id of the function to remove

```javascript
s.removeFunction(1001);
```

Removes specific function.  The function is identified by its `id`.

!!!note

    Once the function is removed then it cant be called from Qlik (aka reload fails) and will not be listed in `listAllFunctions`

## `Request`

### `request.on(event, fn)`

- `event` <[string]> Name of event to listen to. Possible values: `data`.
- `fn` <[function] ([BundledRows])>

```js
request.on('data', (bundle) => {/* deal with bundle*/})
```

### `request.write(bundle)`

- `bundle` <[BundledRows]>

Writes data back to Qlik Engine.

```js
request.on('data', (bundle) => {
  const returnBundle = {/* */};
  request.write(returnBundle);
});
```

[Array]: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array "Array"
[boolean]: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Data_structures#Boolean_type "Boolean"
[function]: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Function "Function"
[number]: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Data_structures#Number_type "Number"
[Object]: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object "Object"
[string]: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Data_structures#String_type "String"
