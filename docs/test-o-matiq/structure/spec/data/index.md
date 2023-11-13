# Data

`Data` section is separated into three sub-sections:

## Selections

This section is optional and in depends on the specific case but here we can specify list of selections that should be applied **before** each test is ran.

!!! note

    Before each test `clearAll` is called! To change this behavior checkout the `Options` section below

In general the selections here are defined in the same manner as the [global placeholder selections](../../props.md#selections).

There are couple of additions though:

### Bookmarks

Here we can add bookmarks as part of our selections list by providing the bookmarks names:

```js
selections: [
    ...
    {
        bookmark: "Bookmark name here"
    }
    ...
]
```

### ByName

We can also reference the global placeholder selections (defined in the [global placeholder section](../../props.md#selections)) and they will be applied:

```js
selections: [
    ...
    {
        byName: "Name of selection specified in the props section"
    }
    ...
]
```

### State

For each selection alternate state can be defined. This includes the placeholder selections. When defining placeholder selections is not possible to set state. State for placeholder selections can be specified when these selections are referenced (aka actually used)

!!! note

    If no state is specified then the default ($) state will be used

```js
selections: [
    ...
    {
        byName: "Name of selection specified in the props section",
        state: "Alternate state 1"
    },
    {
        field: "Department",
        values: ["Production", "Sales", "Operations"],
        state: "Another Alternate state"
    },
    {
        field: "Status",
        values: ["Pending", "Complete"],
        state: "$" // if we want to be explicit
    }
    ...
]
```

!!! note

    Its not possible to provide state when using bookmarks

## Options

Small optional section used to set some test suite options:

- `skip` - if true then the whole test suite will not be ran
- `clearAllBeforeEach` - default is `true`. If set to `false` then `clearAll()` will not be called before each test is executed

## Tests

This section is the actual part where developers might spend most of their time. In this section all data tests are defined.

Each test can be logically split into three sections:

### Generic

- name - short description of the test
- description - optional field to provide more information about the test
- type - `scalar` or `list` (more on this below)
- skip - default `true`. If set to `false` the test will not be ran

### Selections

Define test specific selections that will be made before the data tests is evaluated. This sections is defined in the same way as test suite [selections](#selections)

### Details

And at the end all is about this section.

At the moment two test types are supported:

#### Scalar

This the very basic and possibly most used test type. It's just a single expression. Check out the example below.

- expression - It can be any Qlik expression. The value in this field is passed as it is to Qlik engine to be evaluated
- result - what should be the expected result. This can also be an expression
- operator - one of `<`, `>`, `>=`, `<=`, `==`, `!=`, `=`, `<>`. The result of `expression` will be compared with `result` based on the operator.
- state - optional field to define in which state the expression will be evaluated

#### List

This test type is to validate if values in specific field are present or missing after some selections are made. Will agree that this is not very "interesting" test scenario but it can be useful in some cases

- fieldName - name of the field
- values - array of values
- operator - one of `present` or `missing`
- state - optional field to define in which state the list will be evaluated

## Full example

```js
data: {
    "Test suite 1": {
        // test suite wide selections (made before each test)
        selections: [
            {
                field: "Product Group Desc",
                values: ["Deli"],
            }
        ],
        tests: [
        {
            name: "Test expression 2",
            type: "scalar",
            // test specific selections. 
            // Made after the test suite selections are performed
            selections: [
            {
                field: "Product Group Desc",
                values: ["Produce"],
            },
            ],
            details: {
                expression: "round([Margin %] * 100, 0.1)",
                operator: "==",
                result: 43.6,
            },
        },
        {
            name: "Test expression 2 (State)",
            type: "scalar",
            // Selections can be made in specific alternate state
            selections: [
            {
                field: "Product Group Desc",
                values: ["Frozen Foods"],
                state: "Test",
            },
            ],
            details: {
                expression: "round([Margin %] * 100, 0.01)",
                operator: "==",
                result: 44.89,
            },
        },
        {
            name: "Test expression 3",
            type: "scalar",
            skip: true,
            details: {
                expression: "sum(1001)",
                operator: ">=",
                result: 900,
            },
        },
        ],
    },
},
```
