# Test-O-Matiq

## What it is

Data testing is usually boring and time consuming job and also is lacking when developing BI apps (Qlik included). For this reason we are introducing `Test-O-Matiq`. It is a Node.js package that allows to run user specified data tests against Qlik apps.

`Test-O-Matiq` might not be popular by itself. The main consumer will be [Test-O-Matiq CLI](/test-o-matiq-cli). The CLI will wrap this package and have the ability to define the test suites in YAML/JSON files which then will be ran by the CLI.

Tests can be splint in two areas:

### Meta

Test the overall status of an app - general "health" of the app without going into to much details about the data

#### Data model

Test the overall view of the data model

- Fields - list of fields to be present in the app
- Tables - list of tables to be present in the app
- Synthetic keys - synthetic keys are allowed or not in the app
- Always one selected - list of fields, for which `qOneAndOnlyOne` property should be present

#### Objects

Although not strictly data test but it will be good to have the ability to test if specific objects (viz, sheet etc) are present in the app

#### Fields

List of fields to exists and the count of their distinct values is matching an expected number

#### Tables

List of tables to exists and the count of their rows is matching an expected number

#### Variables

- Exists - variables to exists in the app
- DoNotExists - variables to not exists in the app

#### Data connections

Check if specific data connections are available/visible from the app/user

### Data

This is where the real data tests are happening.

This section allows us to specify list of test cases to be executed in sequence. Each test case have two main sections:

#### Selections (optional)

This sections contains set of selections to be made before the tests are ran.

#### Tests

The actual tests to be performed.

!!!note

    Tests are ran in "order of appearance"

At the moment two types of tests can be specified:

- Scalar - result of "one line" expressions is compared with user defined expected result. Any Qlik expression can be specified here (ones including variables, master items, alternate states etc.). Think of this type as a "textbox"/KPI validation.
- List - check for specific values presence in fields

!!! Note

    Once the project stabilize a third test type is planned - table. Build data table from user defined dimensions and expressions and compare the result with the expected values

### Example

```js
{
  description: "Description goes here",
  version: "0.0.1",
  props: {
    // pre-define selections that can be (re)used in the tests
    selections: {
      "Family class": [
        {
          field: "Family Class Desc",
          values: ["Kia Hubs", "Nissan Hubs"],
        },
      ],
      States: [
        {
          field: "state_name",
          values: ["California", "Virginia"],
        },
        // access previously defined selections
        {
          byName: ["Family class"],
        },
      ],
    },
    // pre-define session variables that can be (re)used in the results
    variables: {
      test: "=sum(0.45)",
      test1: "sum(123)",
    },
  },
  spec: {
    meta: {
      DataModel: {
        Table: ["Customer Good Sales DataFinalFamilyClass"],
        Field: ["Family Class", "Family Class Desc"],
        SyntheticKeys: true,
        AlwaysOneSelected: ["Year"],
      },
      Object: ["MEAjCJ"],
      Field: [{ name: "Family Class", count: 34 }],
      Table: [{ name: "Customer Good Sales DataFinalFamilyClass1", count: 33 }],
      Variable: ["SomeVariable"],
      DataConnections: ["Rest API connection", "Documents"],
    },
    data: {
      "Tests group 1": {
        properties: {
          clearAllBeforeEach: true,
        },
        // define selections that will be made prior each test execution
        selections: [
          {
            field: "Product Group Desc",
            values: ["Produce"],
          },
        ],
        tests: [
          {
            // scalar test example. aka expression/kpi style of test
            name: "Test case 1",
            type: "scalar",
            // test specific selections
            // applied after the test suite selections
            selections: [
              {
                // use selections that are defined in the top props section
                byName: ["States"],
              },
            ],
            details: {
              // use normal Qlik expression definition
              expression:
                "round(Sum([Sales Margin Amount])/Sum([Sales Amount]), 0.01)",
              result: "=$(test)", // sum(0.45)
              operator: "=",
            },
          },
          {
            // list test example - check if specific field values are present
            name: "List test",
            type: "list",
            selections: [],
            details: {
              name: "Product Group Desc",
              values: ["Produce", "Deli"],
              operation: "present",
            },
          },
        ],
      },
    },
  },
};
```
