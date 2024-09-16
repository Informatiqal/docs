# Concurrency and parallel options

Few concurrency task options are available when operation is performed on multiple entities (apps, streams etc.).

## Parallel

By default the operation will be performed in parallel for all task entities. To alter this behavior and run the operation in sequence use:

```yaml
...
options:
    parallel: false
...
```

!!! note "Default"

    If `parallel` is not specified explicitly then its value is `true`

## Concurrency

When `parallel` is `true` we can control how many entities are being processed at the same time by providing `concurrency` option value. This value will make that no more than N number of entities are processed at the same time.

```yaml
...
options:
    parallel: true
    concurrency: 5
...
```

In the example above if we have to process 10 entities then the process will start with the first 5 in parallel and once one of them is processed the 6th one will be started. The process will continue until all 10 are processed. `Rolling N`

## Batch

When `parallel` is `true` we can process the entities in batch mode with `batch` option.

```yaml
...
options:
    parallel: true
    batch: 5
...
```

Using the example above and having to process 10 entities. The process will start with the first 5 and when all of them are processed the process will pick the next batch (5) and will process them as well.

!!! note

    By design all entities in each batch are processed in parallel
