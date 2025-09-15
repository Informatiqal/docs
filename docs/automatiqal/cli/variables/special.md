# Reserved variables

There are few variables that can be used out of the box and there is no need for them to be defined anywhere.

## GUID

Random generated UUID/GUID identifier.

Syntax: `${GUID}`

Data example: `123e4567e89b12d3a456426614174000`

!!! note

    Some Qlik REST API values have restrictions to not accept specific values. To avoid any issues the returned UUID is without the `-`.

## RANDOM

Random generated alpha-numeric 20 character string. All letters are upper case.

Syntax: `${RANDOM}`

Data example: `4QA7JHDRW5JIA6EXWW1E`

## TODAY

Todays date in `YYYYMMDD` format.

Syntax: `${TODAY}`

Data example: `20230208`

## NOW

The current timestamp in `YYYYMMDDHHmmss` format.

Syntax: `${TODAY}`

Data example: `20230208191723`

## INCREMENT & DECREMENT

At the beginning of the runbook the increment value will be set to `1`. Each instance of `${INCREMENT}` will increase the value with `+1`. And each instance of `${DECREMENT}` will add `-1` to the value.

Syntax: `${INCREMENT}` / `${DECREMENT}`

!!! note

    If `${DECREMENT}` instances are more than `${INCREMENT} ones then negative numbers will appear
