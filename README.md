# Authenticator - OIDC

| Status                   |                      |
| ------------------------ |----------------------|
| Stability                | [beta]               |
| Distributions            | [contrib]            |

This extension implements a `configauth.ServerAuthenticator`, to be used in receivers inside the `auth` settings. The authenticator type has to be set to `oidc`.

## Configuration

```yaml
extensions:
  oidc:
    issuer_url: http://localhost:8080/auth/realms/opentelemetry
    issuer_ca_path: /etc/pki/tls/cert.pem
    audience: account
    username_claim: email

receivers:
  otlp:
    protocols:
      grpc:
        auth:
          authenticator: oidc

processors:

exporters:
  logging:
    logLevel: debug

service:
  extensions: [oidc]
  pipelines:
    traces:
      receivers: [otlp]
      processors: []
      exporters: [logging]
```
Firebase AppCheck is an example service that uses custom headers, doesn't provide an auto-discovery mechanism and has an `issuer_url` that isn't simply the base URL of the JWKS URL.
```yaml
extensions:
  oidc:
    issuer_url: https://firebaseappcheck.googleapis.com/<PROJECT_NUMBER>
    jwks_url: https://firebaseappcheck.googleapis.com/v1/jwks
    attribute: X-Firebase-AppCheck
    audience: projects/<PROJECT_NUMBER>
...
```
[beta]:https://github.com/open-telemetry/opentelemetry-collector#beta
[contrib]:https://github.com/open-telemetry/opentelemetry-collector-releases/tree/main/distributions/otelcol-contrib
