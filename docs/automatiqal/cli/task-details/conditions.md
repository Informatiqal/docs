# Conditions

Apart from `skip` property, which have to be set explicitly, tasks can be skipped based on condition. `when` condition can be set for tasks and based on the evaluation of that condition then task will be skipped or not.

The result of each condition sets `skip` property of the task. So even if `skip` was not explicitly set if the task have `when` condition then `skip` property will be set (based on the result of the `when` condition)

!!! note "Priority"

    `skip` have higher priority and its evaluated before `when` conditions

!!! note "Default"

    If `skip` property is not defined then its auto added and set to `false`

!!! note "Loop"

    `when` condition is evaluated before the task is ran. This means that any defined `loop` will be ran post `when` evaluation and not for each element

## Syntax

### Operators

Syntax of `when` is the same as the one used in `filter` property.

The following operators can be used to compare items:

- `eq` - equal `==`
- `ne` - not equal `!=`
- `gt` - grater than `>`
- `ge` - greater than or equal `>=`
- `lt` - less than `<`
- `le` - less than or equal `<=`
- `sw` - string starts with
- `ew` - string ends with
- `so` - string is substring of

Multiple conditions can be combined with `and` and `or` operators. Brackets are also respected.

```yaml
- name: Some task
  ...
  when: 1 > 0 and ((10 * 2) > 10)
```

### Properties

Result from previous tasks can be used:

- Logical - few special properties can be checked:
    - `length` - compare the length of the returned data

        ```yaml
        - name: Some task
          operation: app.get
          filter: some filter here

        - name: Another task
          operation: app.delete
          source: Some task
          when: $${Some task|length} > 2
        ```

        In the example above, the second task will be ran only if `Some task` is returning more than 2 entries

    - `skip` - compare the `skip` property of the previous task

        ```yaml
        - name: Some task
          operation: app.get
          filter: some filter here
          skip: true

        - name: Another task
          operation: app.delete
          source: Some task
          when: $${Some task|skip} eq false
        ```

        In the example above, the second task will be ran only if `Some task` is being skipped.`

- Property - conditions can be checked against specific property, returned from previous task. Since tasks can return multiple entities (multiple apps, streams, users, tasks etc.) when checking for specific property we should have in mind that.
    - any - the default behavior will be to check if ANY of the returned values satisfies the condition

        ```yaml
        - name: Some task
          operation: app.get
          filter: some filter here
          skip: true

        - name: Another task
          operation: app.delete
          source: Some task
          when: $${Some task#name} sw "Something"
        ```

        In the above example the `when` condition will be evaluated as `true` as long as at least one of the apps, returned from `Some task` task, have a name that starts with `Something`

    - all - if we want to check if all entities are satisfying the condition then we have to use `!` when specifying the property:

        ```yaml
        - name: Some task
          operation: app.get
          filter: some filter here
          skip: true

        - name: Another task
          operation: app.delete
          source: Some task
          when: $${Some task#name!} sw "Something" //! is important here
        ```

        In the above example the `when` condition will be `true` if the name of ALL returned apps starts with `Something`
