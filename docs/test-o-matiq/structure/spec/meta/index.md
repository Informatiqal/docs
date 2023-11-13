# Meta

Meta tests are optional and as their name suggests these tests are not about the data of the app itself but more about the app in general.

## Data model

Test the overall view of the data model. All of the tests in the list below are optional:

- Fields - list of fields to be present in the app
- Tables - list of tables to be present in the app
- SyntheticKeys - synthetic keys are allowed or not in the app
- AlwaysOneSelected - list of fields, for which `qOneAndOnlyOne` property should be true

```json
{
...
    spec: {
        meta: {
            DataModel: {
                Field: ["Year", "OrderDate", "SalesAmount", "OrderId"],
                Table: ["MasterCalendar", "OrderTransactions"],
                SyntheticKeys: false,
                AlwaysOneSelected: ["Year"]
            }
        }
    }
...
}
```

## VizObject

Check if specific objects (viz, sheet etc) are present in the app

```json
{
...
    spec: {
        meta: {
            VizObject: ["MEAjCJ"]
        }
    }
...
}
```

## Field

List of fields to exists and the count of their distinct values is matching an expected number

```json
{
...
    spec: {
        meta: {
            Field: [
                {
                    name: "Year",
                    count: 3,
                },
                {
                    name: "OrderId",
                    count: 10,
                }
            ]
        }
    }
...
}
```

## Table

List of tables to exists and the count of their rows is matching an expected number

```json
{
...
    spec: {
        meta: {
            Table: [
                {
                    name: "OrderTransactions",
                    count: 100
                },
                {
                    name: "MasterCalendar",
                    count: 36
                }
            ]
        }
    }
...
}
```

## Variable

- Exists - variables to exists in the app
- DoNotExists - variables to not exists in the app

```json
{
...
    spec: {
        meta: {
            Exists: ["vPrevYear", "vLastYear"],
            DoNotExists: ["vTest1"],
        }
    }
...
}
```

## DataConnection

Check if specific data connections are available/visible from the app/user

```json
{
...
    spec: {
        meta: {
            DataConnections: [
                "Database connection 1",
                "REST API data connection 1",
            ],
        }
    }
...
}
```

## MasterItem

Similar to data connections check, just check if specific master items are present in the app (dimensions, measures and visualizations)

```json
{
...
    spec: {
        meta: {
            MasterItem: {
                dimensions: ["Color by Metric", "Budget Period"],
                measures: ["Central Margin %"],
                visualizations: ["Sales $ by State", "Sales vs Margin by Product"]
            },
        }
    }
...
}
```
