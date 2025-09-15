# Multiple environments

The default behavior of the runbook is "one runbook one environment". Usually this is more than enough but there are cases where the workflow requires multiple environments. To achieve this we have to make changes the `environment` section and the tasks.

## Environment section

In the `environment` section we can pass array (multiple) environments. For each environment we have to specify two new properties::

- name - unique (user defined) name of the cluster
- default - `boolean`. Only one environment can be set as default `true`. For each task we can specify against which environment it should be ran. But this can make the runbook a bit too descriptive. Because of this the task environment property is set as optional. If `environment` is not specified for the task(s) then the task will be ran against the environment which is set as `default: true`. For all other environments here we have to explicitly set `default: false`.

```yaml
environment:
  - host: some-sense-cluster.com
    name: Cluster 1
    default: true
    ...
  - host: another-sense-cluster.com
    name: Another cluster
    default: false
    ...
```

## Tasks

If multiple environments are to be used then we can specify `environment` property for the tasks. This property will indicate which environment config to be used when executing the task.

In the example below we have 3 tasks:

- `Get some streams` will be ran against `Cluster 1` config
- `Get some apps` will be ran against `Another cluster` config
- `Get some other apps` - this task is missing the `environment` property. This will make it run against the environment config for which `default` is set to `true`. In the example below this will be `Cluster 1`

```yaml
environment:
  - host: some-sense-cluster.com
    name: Cluster 1
    default: true
    ...
  - host: another-sense-cluster.com
    name: Another cluster
    default: false
    ...
tasks:
  - name: Get some streams
    operation: stream.get
    filter: name sw 'something'
    environment: Cluster 1

  - name: Get some apps
    operation: app.get
    filter: name sw 'something'
    environment: Another cluster

  - name: Get some other apps
    operation: app.get
    filter: name so 'another-apps'
    # no environment property. It will be ran against the env with "default: true"
```
