# Developing plugins

As mentioned the custom plugins are simple ESM JS files that are loaded during `qlbuilder` start-up phase.

The custom plugin have to export two entities:

- [meta object](./#meta-object)
- [action function](./#action-function)

!!! info "More info 1"

    How the plugin is built (plain JS, rollup, vite etc.) is not in the scope of this documentation

!!! info "More info 2"

    How the plugin is distributed (published to public/private npm, copy-paste etc.) is not in the scope of this documentation

!!! info "More info 3"

    Considering the idea to have a dedicated repo for **public** community plugins. Still undecided at this point

!!! info "More info 4"

    For any issues and questions please feel free to open and [issue](https://github.com/Informatiqal/qlbuilder/issues) in the repo

## Meta object

The meta object have the following structure:

```typescript
PluginMeta {
  command: {
    name: string;
    description?: string;
    aliases?: string[];
    argument?: string;
    options?: {
      flag: string;
      description?: string;
      defaultValue?: string | string[] | boolean;
    }[];
  };
  options?: {
    requireConnection?: boolean;
    requireEnv?: boolean;
    requireApp?: boolean;
    configFile?: string;
  };
}
```

Only `command.name` is the required option and if the others are not provided `qlBuilder` will populate them with a default values.

But for clarity it is recommended to explicitly provide as many of the properties as possible.

#### Command

For more information about the command section please refer to [commander.js](https://github.com/tj/commander.js/) documentation

#### Options

This section defines few properties which guide `qlBuilder` on what the arguments for the `action` function should include.

- `requireConnection` - (default `true`) if `true` connection will be established and `global` object (from `enigma.js`) will be provided. (`requireEnv` must be `true`)
- `requireApp` - (default `true`) if `true` the required app will be open and the app handle will be provided (`requireEnv` must be `true`)
- `requireEnv` - (default `true`) if `true` the command will have to have an environment as an argument
- `configFile` - (default `config.yml`) allows to specify which config file to be used

## Action function

The command is first registered from the `meta` object. When the custom command is invoked `qlBuilder` will do the needful first (some checks, establish connection etc.) and then will call the plugin `action` function. The call will include the object below as an argument:

```typescript
interface PluginArguments<T> {
    environment: IConfig | undefined; // undefined when requireEnv is false
    command: {
        argument: string | undefined; // undefined if no argument is needed/provided
        options: T; // the values for the options defined in the meta object (if any)
    };
    engine: {
        global: AnyFunction; // enigma.js global object
        app: AnyFunction; // enigma.js app object
        session: AnyFunction; // enigma.js session object
        auth: Auth; // the auth info used for establishing connection
        enigmaInstance: typeof Engine; // the whole enigma.js instance
    };

    tools: {
        build(): string; // trigger build command to generate LoadScript.qvs
        spinner: typeof Spin; // console spinner instance
        print: typeof Print; // custom console.log instance
        generateXrfkey(): string; // helper function to generate xrfkey strings
        uuid(): string; // helper function to generate uuid strings
    };
}
```

Very simple `action` function:

```javascript
export async function action(arg) {
    const print = new args.tools.print();
    const spin = new args.tools.spinner("Starting ...", "arc");

    spin.start();

    print.ok("Hello, from my custom command!");

    spin.stop();
}
```

!!! info "More info"

    All core commands are implemented as plugins so you can use them as more advanced examples. Have a look at the [qlBuilder repo](https://github.com/Informatiqal/qlbuilder/tree/master/src/commands)

## Example

The below plugin will register new command (`testing`) which accepts one argument (`-s, --someFlag`) and upon execution will print `Something` in the console.

```javascript
export const meta = {
    command: {
        name: "testing",
        description: "Command description goes here",
        aliases: [],
        options: [
            {
                flag: "-s --someFlag",
                description: "Flag description goes here",
                defaultValue: "Hello",
            },
        ],
    },
    options: {
        requireConnection: false,
        requireEnv: false,
        requireApp: false,
    },
};

export async function action(arg) {
    console.log("Something");
}
```
