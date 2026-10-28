# Docker Compose environment files

Each service that uses deployment-specific values has a service-local
`.env.example`. Compose reads a service directory's `.env` for interpolation;
it does not pass every value to containers unless the Compose file also lists
that file under `env_file:` or maps the values under `environment:`. Authentik
and Paperless intentionally use `.env` for both purposes.

Before deploying a service, run these commands from its directory:

```powershell
Copy-Item .env.example .env
# Edit .env and replace every change_me value with the required local value.
docker compose config --quiet
docker compose up -d
```

Use unique, securely generated values for passwords, application signing keys,
database passwords, VPN keys, Cloudflare tokens, Gotify tokens, and OIDC client
secrets. Do not commit `.env`; `.gitignore` excludes it while allowing the
sanitized `.env.example` templates. Relative bind-mount paths are resolved from
the service Compose file's directory. Host paths and public hostnames in a
template must match the local deployment.

Paperless OIDC configuration is provided as a single-line JSON value in
`paperless/.env`. Replace its example client ID, client secret, and Authentik
discovery URL with the values for the deployed provider. Keep the secret only
in local `.env`.

Every service directory has a `.env.example`; values with safe defaults are
shown for clarity, while required credentials and secrets use clearly marked
placeholders that must be replaced before deployment.
