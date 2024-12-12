# Global

Global mixin are available on the `global` enigma.js object.

```js
import { globalMixin } from "enigma-mixin";

const session = enigma.create({
  schema,
  mixins: [...globalMixin],
  url: `wss://sense-engine-url`,
  createSocket: (url) => new WebSocket(url),
});

const global = await session.open(); // mixin available once the session is established
```

## Mixin

### mGetReloadProgress

This mixin gets the reload progress in human readable form - similar to what Qlik outputs when an app is being reloaded.

Usage:

```js
const doc = await global.openDoc("some-doc-id");

// init the mixin
const reloadProgress = global.mGetReloadProgress();

// prepare the emitter
reloadProgress.emitter.on("progress", (msg) => {
  // here we will receive the reload messages
  // do whatever you have to do with them :)
  console.log(msg);
});

// prepare the on error emitter
// errors will be emitted by Qlik if configureReload is set correctly (see below)
reloadProgress.emitter.on("error", (errorMessage) => {
  // any script errors can be handled here
  // FYI on progress will also receive the errors messages
  // just so we can have the complete reload log in one place
  console.log(errorMessage);
});

// optional
// if qUseErrorData is true then onError event will be triggered if the reload is failing
// https://help.qlik.com/en-US/sense-developer/November2023/Subsystems/EngineJSONAPI/Content/service-global-configurereload.htm
await global.configureReload(true, true, false);

const reloadApp = await new Promise(function (resolve, reject) {
  // or doc.doReloadEx
  // DO NOT "await" doReload or doReloadEx
  // if await them then pooling for reload progress messaged
  // will be started after the app finished reloading
  doc.doReload().then((r) => {
    // give it another few ms to make sure we have all the messages
    setTimeout(function () {
      // stop the mixin from pooling for new messages.
      // The app reload is already complete at this point
      reloadProgress.stop();
      // resolve the main promise
      resolve(r);
    }, 300);
  });

  // see below for available (optional) options that can be passed
  reloadProgress.start();
});
```

When initializing the mixin we can pass specific `qRequestId`. If this parameter is not passed then the default value of `-1` is used.

```js
const reloadProgress = global.mGetReloadProgress(123);
```

`start()` function accept few options. **All of these are optional**

- `poolInterval` - `number` how often to pool for new reload messages. Default is `200` (ms),
- `skipTransientMessages` - `boolean` Qlik returns two type of messages - persistent and transient. Persistent messages are the ones that stating which table is started loading, how many rows are loaded at the end of each table etc. Transient messages are the ones that display how many rows were loaded so far. For example if we are loading table with 1M rows then transient messages will be like: `5025`, `120124`, `500003` ... `1000000`. To ignore these messages set this option to `true`. Default is `false`
- `includeTimeStamp` - `boolean` include the current timestamp for each message. This is the timestamp when the message is received and not when the message was send. Default is `false`
- `trimLeadingMessage` - `boolean` some of the default messages have a leading space. Setting this option to `true` will remove the leading space. Default is `false`

  ```js
  reloadProgress.start({
    poolInterval: 300,
    skipTransientMessages: true,
    includeTimeStamp: false,
    trimLeadingMessage: true,
  });
  ```
