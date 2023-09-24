# Things to come

A (lots) of things are planned to be added:

- state - ability to define at what Qlik state the test will be ran (or selections to be made)
- deviations - atm the results are compared strictly (A === B). Option to add %, +/- etc variations between the results
- table - new test type. Specify table (dimensions, measures and values) inside the test and compare with Qlik table
- other test types?
- multiple results expectations - compare one expression (for example) with multiple conditions/results. For example:
`sum(Sales)` should be greater than `100` but less than `1000`
- multi-app testing - run test against more than one app to compare the data between the data layers
- regression testing - compare the run times and values between multiple test runs. This will help to identify potential issues that can lead to higher response times in the dashboard
- for the CLI - detailed test output + potential UI (`HTML`) output generation that can be displayed on a web page (or GitHub/BitBucket page). The output itself should be in a format that can be loaded back in Qlik itself
- pass expressions and data that are defined externally
- define the tests in non YAML/JSON format (for example Excel). If this is implemented will be way in the future and probably functionality of the CLI that will convert the external data into YAML/JSON
