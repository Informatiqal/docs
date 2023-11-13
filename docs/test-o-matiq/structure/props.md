# Props

`Props` is an optional property that is used for two (for now) reasons:

## Selections

This section is used to define selections that can be reused across the test suite without the need to write the same selections over and over again.

!!! warning

    Its important to mention that selections defined here are not performed unless called from some of the test suites.

```json
...
props: {
    selections: {
        DepartmentAndStatus: [
          {
            field: "Department",
            values: ["Production", "Sales", "Operations"],
          },
          {
            field: "Status",
            values: ["Pending", "Complete"],
          },
        ],
        ProductionLastYear: [
            {
                field: "Department",
                values: ["Production"]
            },
            {
                field: "Year",
                values: ["2023"]
            }
        ]
    }
...
}
```

In the example above we've defined two placeholder selections:

- `DepartmentAndStatus` - this selection will select in two fields `Department` and `Status`. In `Department` field it will select three values - `Production`, `Sales` and `Operations` and in `Status` field will select `Pending` and `Complete`

- `ProductionLastYear` - this selection will select `Production` in `Department` field and `2023` in `Year` field

!!! warning

    Selections are performed in "order or appearance" (from top to bottom) and in sequence (second selection will be performed once the first one is applied, third after the second is applied etc.)

## Variables (session)

This sections is for defining set of session variables. As the name suggests these variables will not be preserved in the app and will be destroyed once the session is closed (aka the tests suite is complete).

The purpose of these variables can be different but if there is a need for a variable during the test run then this is the place to define them.

The variables are defined as simple string property - the property name will be used as variable name and the property value will be the variable value:

```json
...
props: {
    variables: {
        SomeName: "sum(Amount)",
        AnotherName: "count(ID)",
    }
...
}
```
