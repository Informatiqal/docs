# Task options

In contrast with the task details, options are common for all [operations](./operations-list.md). Some of them might not make sense for specific entities and `Automatiqal` will ignore them, even if they are provided.

!!! note

    Options property is optional and each option itself is optional

- `multiple` (`true/false`) - by default `Automatiqal` throw an error if its about to perform operation on more than one entity ([`filter`/`source`](./filter-source.md)). Provide `true` to explicitly grant the task "permission" to operate on more than one on multiple objects.
- `allowZero` (`true/false`) - `Automatiqal` will throw an error if the task is about to be performed on 0 entities
- `customPropertyOperations` ([add/remove/set](#addremoveset)) - when updating an entity and passing custom property values, how to act on these values
- `tagOperations` ([add/remove/set](#addremoveset)) - when updating an entity and passing tag values
- `whitelistOperation` ([add/remove/set](#addremoveset)) - when updating virtual proxy and passing whitelist values
- `virtualProxiesOperation` ([add/remove/set](#addremoveset)) - when updating proxy and passing virtual proxy values

## Add/Remove/Set

- `add` - add the provided values to the already existing values (aka append)
- `remove` - remove the provided values from the entity
- `set` - overwrite the existing values with the provided ones
