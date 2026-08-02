---
name: Authenticate and inspect a WEKA cluster
description: Log in to a WEKA (NeuralMesh) cluster, obtain a bearer token, and read cluster status, filesystems, and alerts.
api: openapi/weka-openapi-original.json
operations: [login, refreshToken, whoAmI, getClusterStatus, getFileSystems, getAlerts]
---

# Authenticate and inspect a WEKA cluster

Use the WEKA REST API (served at `https://<cluster>:14000/api/v2`) to sign in and read cluster state.

## Steps

1. **Log in** — `POST /login` (`login`) with `{ "username", "password" }`. The response returns an `access_token` (valid 5 minutes by default) and a `refresh_token`.
2. **Set the header** — send `Authorization: Bearer <access_token>` on every subsequent call.
3. **Refresh when needed** — when the access token expires, `POST /login/refresh` (`refreshToken`) with the refresh token to get a new access token. Do not re-send credentials.
4. **Confirm identity** — `GET /users/whoami` (`whoAmI`) to verify the authenticated principal and role.
5. **Read cluster status** — `GET /cluster` (`getClusterStatus`).
6. **List filesystems** — `GET /fileSystems` (`getFileSystems`).
7. **Check alerts** — `GET /alerts` (`getAlerts`).

## Rules

- Access tokens are short-lived (5 min default); prefer a long-lived local-user API token for automation (generated via GUI/CLI).
- On `401`, obtain a fresh token before retrying (see errors/weka-problem-types.yml).
- No idempotency-key contract exists; reads are safe to retry, mutations are not (conventions/weka-conventions.yml).
