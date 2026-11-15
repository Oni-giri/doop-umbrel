# Doop for Umbrel

Umbrel Community App Store packaging for [Doop](https://github.com/kgoedecke/doop).
The image builds from upstream tag `v0.14.0` and runs Doop's production Node
server with Chromium and PostgreSQL.

## Release

Push a matching `v<upstream-version>` tag to this repository to run the
publishing workflow, or dispatch it with an upstream ref and image tag. The
workflow builds upstream's production Dockerfile and publishes
`ghcr.io/oni-giri/doop-umbrel:<upstream-version>`. The app-store image tag and
manifest version should match.

## Configuration

The pre-start hook creates and retains random database, auth-signing and
credential-encryption secrets under `${APP_DATA_DIR}/secrets`; preserve that
directory when backing up or restoring the app. Database and canvas data are
persisted in the app's data directory. Doop needs an absolute public origin for
OAuth and integrations that use redirect URLs, so `exports.sh` derives
`BETTER_AUTH_URL` from the Umbrel-provided `DEVICE_DOMAIN_NAME`, `APP_DOMAIN`,
`APP_PROXY_PORT` and `APP_HIDDEN_SERVICE`. That covers local, remote-access and
Tor launches without editing the package; override `BETTER_AUTH_URL` in
`docker-compose.yml` only to pin a different canonical URL.

Optional provider variables such as `ANTHROPIC_API_KEY`, SMTP settings and
OAuth credentials can be added to the web service environment in
`docker-compose.yml` when needed. Never commit real credentials.
