# OnError

Each task can have a `try ... catch` workflow defined.

The `onError` block is just a list of tasks to be performed if `Automatiqal` encounter an error during the task execution.

For example if the task tries to update an app that do not exists then `Automatiqal` will throw an error (assuming that `options.allowZero` is either not present or set to `false`). If we want to capture the error and perform an extra operation before exit then `onError` block can be defined where the `onError` tasks are specified.

In the example below the `Update app (false positive)` task will raise an error. The `onError` block will "capture" the error and perform an extra task (`Delete imported app`) and will exit.

```yaml
- name: Update app (false positive)
  operation: app.update
  filter: name eq 'Non existing application'
  details:
    name: New name
  onError:
    tasks:
      - name: Delete imported app
        operation: app.remove
        source: Import application
```

## Continue

Once all tasks from `onError` block are completed the execution of the runbook is stopped and the whole process will exit with an error state.

If there is need to continue after error is thrown we can use `continue` operation to tell `Automatiqal` to not exit, once all `onError` tasks are complete, but to "exit" from the `onError` block and continue with the next task.

```yaml
- name: Update app (false positive)
  operation: app.update
  filter: name eq 'Non existing application'
  details:
    name: New name
  onError:
    tasks:
      - name: Delete imported app
        operation: app.remove
        source: Import application
      - name: Do not exit and continue
        operation: continue
- name: Some other task
  ...
```

In the example above `Automatiqal` will perform `Delete imported app` task and because there is `operation: continue` will continue with `Some other task` after this.

!!! note "Fun fact"

    Nested `Task -> OnError -> Task -> OnError ...` is not against the rules :wink:
