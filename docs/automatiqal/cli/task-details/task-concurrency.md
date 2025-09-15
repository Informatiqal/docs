# Concurrency and parallel options

Few concurrency task options are available when operation is performed on multiple entities (apps, streams etc.).

## Parallel

By default the operation will be performed in sequence for all task entities. To alter this behavior and run the operation in parallel use:

```yaml
...
options:
    parallel: true
...
```

!!! note "Default"

    If `parallel` is not specified explicitly then its value is `false`

## Concurrent

The `concurrent` option implements "rolling N" processing. The value for `concurrent` specifies the max items that can be processed at the same time.

```yaml
...
options:
    parallel:
        concurrent: 5
...
```

In the example above if we have to process 10 entities then the process will start with the first 5 in parallel and once one of them is processed then the 6th one will be started. The process will continue until all 10 are processed.

### Delay

  `delay` (in seconds) is an optional property that can be used to have a time gap once N-th item was processed:

```yaml
...
options:
    parallel:
        concurrent: 5
        breakCount: 10
        delay: 2
...
```

In the example above once the 10th item is processed the process will stop for 2 seconds.

!!! note

    With `breakCount` specified all items will be split into batches of N items (`breakCount`) and for each batch there will be no more than X (`concurrent`) items being processed.

## Batch

Entities can be processed in batches with `batch` option. The option specifies how many items should be in each batch. Once the items are batched then batches are executed in sequence but the items inside each batch are processed in parallel.

```yaml
...
options:
    parallel:
        batch: 5
...
```

Using the example above and having to process 10 entities. The process will start with the first 5 and when all of them are processed the process will pick the next batch (5) and will process them as well.

!!! note

    By design all entities in each batch are processed in parallel

### Delay

  `delay` (in seconds) is an optional property that can be used to have a time gap between each batch. Without it each batch will be started right after the previous batch is complete.

  ```yaml
...
options:
    parallel:
        batch: 5
        delay: 1
...
```

In the example above there will be 1 second delay between each batch.

## Sequence

To emulate sequence processing (one after another) we can use `batch` option and set it to 1:

```yaml
...
options:
    parallel:
        batch: 1
...
```
