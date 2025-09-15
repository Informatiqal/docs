# Authentication

Below are examples of the available authentication methods.

## Certificates

```yaml
environment:
  host: my-sense-instance.com
  port: 4242
  authentication:
    cert: path\to\client.pem
    key: path\to\client_key.pem
    user_dir: SOME_USER_DIR
    user_name: SOME_USER_ID
```

!!! warning "Important"

    When authenticating with certificates the port should be 4242.

!!! warning

    Keep certificates in a safe place!

## JWT

```yaml
environment:
  host: my-sense-instance.com
  port: 443
  proxy: jwt
  authentication:
    token: eyJhbGciOiJSUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1...
```

## Header

```yaml
environment:
  host: my-sense-instance.com
  port: 443
  proxy: hdr
  authentication:
    header: header-name
    user: USER_DIR\USER_ID
```

## Ticket

```yaml
environment:
  host: my-sense-instance.com
  port: 443
  proxy: hdr
  authentication:
    ticket: ticket-value
```
