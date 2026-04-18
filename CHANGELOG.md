# Changelog

## v0.2.0

### Breaking

- The chart no longer ships a default `authentication.fallback-admin.secret`.
  The configmap template now `fail`s render if the secret is unset OR equals
  the placeholder `"changeme"`. Stops new deployments from accidentally
  exposing the mail server with publicly-known credentials. Existing
  deployments that pass an explicit secret (or env-var reference like
  `"%{env:VAR}%"`) are unaffected — `helm upgrade` succeeds normally.

  **Migration:** if you currently rely on the chart default, add to your
  values:
  ```yaml
  config:
    authentication:
      fallback-admin:
        user: "admin"
        secret: "<your-strong-password>"
  ```
  Or use an env-var reference plus `envFrom` to source from a Secret.

### Added

- New optional `templates/networkpolicy.yaml` gated on `networkPolicy.enabled`
  (default `false` for backwards compatibility). When enabled, restricts
  ingress to the public mail protocol ports plus user-supplied management
  API rules. See `values.yaml` for the schema.

## v0.1.1 and earlier

See git history.
