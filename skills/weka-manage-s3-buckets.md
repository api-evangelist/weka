---
name: Manage WEKA S3 buckets and users
description: Stand up S3 on a WEKA cluster, create buckets and users, and manage S3 credentials via the REST API.
api: openapi/weka-openapi-original.json
operations: [login, getS3Cluster, getS3Buckets, createS3Bucket, getUsers, createUser, resetUserS3Credentials]
---

# Manage WEKA S3 buckets and users

## Steps

1. **Authenticate** — `POST /login` (`login`); use the bearer token on all calls.
2. **Check the S3 cluster** — `GET /s3` (`getS3Cluster`) to confirm the S3 service is configured.
3. **List buckets** — `GET /s3/buckets` (`getS3Buckets`).
4. **Create a bucket** — `POST /s3/buckets` (`createS3Bucket`) with the bucket name and backing filesystem.
5. **Create/confirm an S3 user** — `GET /users` (`getUsers`); if needed `POST /users` (`createUser`) with the `S3` role and an attached IAM policy.
6. **Issue S3 credentials** — `POST /users/{uid}/s3/resetCredentials` (`resetUserS3Credentials`) to generate/rotate the user's S3 access/secret keys.

## Rules

- Local S3 users require the S3 role plus an attached IAM policy (docs: additional-protocols/s3/s3-users-and-authentication).
- Changing a user's password does NOT rotate S3 API credentials — rotate explicitly via `resetUserS3Credentials`.
- Handle `403` as a missing role/policy, `404` as a missing bucket/user (errors/weka-problem-types.yml).
