---
name: Provision a WEKA filesystem with a directory quota
description: Create a WEKA filesystem, then set a directory quota on it, using the REST API.
api: openapi/weka-openapi-original.json
operations: [login, createFileSystem, getFileSystem, resolvePath, putQuota, listQuotas]
---

# Provision a WEKA filesystem with a directory quota

## Steps

1. **Authenticate** — `POST /login` (`login`); use the returned bearer token on all calls.
2. **Create the filesystem** — `POST /fileSystems` (`createFileSystem`) with the filesystem name, group, and capacity. Capture the returned `uid`.
3. **Verify** — `GET /fileSystems/{uid}` (`getFileSystem`).
4. **Resolve the target directory to an inode** — `GET /fileSystems/{uid}/resolvePath` (`resolvePath`) with the directory path to obtain its `inode_id`.
5. **Set the quota** — `PUT /fileSystems/{uid}/quota/{inode_id}` (`putQuota`) with `soft_limit_bytes`, `hard_limit_bytes`, and optional `grace_seconds`.
6. **Confirm** — `GET /fileSystems/{uid}/quota` (`listQuotas`).

## Rules

- Reference the equivalent CLI for parameter semantics (docs: weka-rest-api-and-equivalent-cli-commands).
- Quotas are keyed by `inode_id`, not path — always resolve the path first.
- Mutations are not idempotent by key; re-read via `listQuotas`/`getQuota` to reconcile after a failure rather than blindly retrying (conventions/weka-conventions.yml).
