# Admin UI and API

## Admin UI

At the moment the admin UI is very basic. It allows to:

- verify config - verify if the available config file is a valid
- reload config - reloads the config to pick up any changes that are made

## API

Under the hood the admin UI is using couple of API endpoints - `/api/reload-config` and `/api/verify-config`.

The API endpoints can be used without the admin UI. The restriction is that the request should send the cookie specified in the config (`general -> admin -> cookie -> name/value`)
