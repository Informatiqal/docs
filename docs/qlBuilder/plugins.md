# Plugins

`qlBuilder` supports writing and loading custom build plugins. The plugins are side-loaded and can extend `qlBuilder` functionality with extra commands.

In essence plugins are ESM JS files that expose a function, which is then loaded during `qlBuilder` startup. Have a look at the [developing plugin](plugins/developing.md) page for more info.

## Internal
Internally all core [commands](commands.md) codebase is implemented as a plugin.

## Community

More info to follow

## Loading plugins

!!! warning "Important"

    Do not load anything that you don't trust or know it's origin!

The list with the available plugins is read from `C:\Users\<CURRENT USER>\qlBuilder\plugins.yaml` ( Create the file if do not exists).

The structure of the file is simple:

```yaml
plugins:
  - X:\path\to\some\plugin\index.js
  - X:\path\to\another\plugin.js
```
