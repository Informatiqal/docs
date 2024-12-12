# Enigma mixins

This package contains set ot [enigma.js](https://github.com/qlik-oss/enigma.js/) mixins. Enigma.js documentation describes mixins as:

> The mixin concept allows you to add or override QIX Engine API functionality. A mixin is basically a JavaScript object describing which types it modifies, and a list of functions for extending and overriding the API for those types.

`enigma-mixin` adds few mixin on `global`, `doc` and `object` enigma.js objects.

For the specific mixin have a look at the dedicated pages:

- [global](./global.md)
- [doc](./doc.md)
- [object](./object.md)

## Installation

The installation is very simple:

```bash
npm install enigma-mixin
```

## Usage

```js
import { globalMixin, docMixin, objectMixin } from "enigma-mixin";

const session = enigma.create({
  schema,
  mixins: [...docMixin, ...globalMixin, ...objectMixin],
  url: `wss://sense-engine-url`,
  createSocket: (url) => new WebSocket(url),
});
```

As we can see mixins are loaded/defined as a session property. There is no need to attach all types of mixin. Attach whatever type is required.
