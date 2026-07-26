---
name: Publish an RDK app to the Liberty Global AppStore
description: Register a maintainer company and publish, update or withdraw an application's metadata in the Liberty Global AppStore Metadata Service (ASMS), through the AS3 maintainer perspective.
api: openapi/liberty-global-appstore-metadata-service-openapi.yml
operations: [createMaintainer, getMaintainer, listMaintainerApplications, createMaintainerApplication, getMaintainerApplication, replaceMaintainerApplication, deleteMaintainerApplication]
generated: '2026-07-25'
method: generated
---

# Publish an RDK app to the Liberty Global AppStore

This skill covers the **Maintainer** perspective of the AppStore Metadata Service — the developer-facing half of the API, reached through the **AS3 Proxy**. Use it to get a company registered and its application metadata into the store so set-top boxes can find it.

## Before you start

- **This is a self-hosted API.** The specification declares no `servers` block and Liberty Global runs no public endpoint. You are calling an operator's deployment of `daccloud/appstore-metadata-service`, or your own local stack. The provider's README documents `http://localhost:8081/as3` for this perspective. Substitute the operator's host.
- **Auth is HTTP Basic and is not in the spec.** The AS3 Proxy enforces it and also authorizes each maintainer to their own applications only. Credentials come from whoever runs the deployment. See `authentication/liberty-global-authentication.yml`.
- **There is no idempotency key.** Retrying a create is not safe in the "same key, same result" sense — it is safe only because a duplicate returns `409 Conflict`. See `conventions/liberty-global-conventions.yml`.

## Steps

1. **Register the maintainer company** — `createMaintainer` (`POST /maintainers`).
   Body: `code`, `name`, `address`, `homepage`, `email`. The `code` is the short opaque company identifier (e.g. `lgi`) and becomes the `{maintainerCode}` path segment for everything that follows. Choose it once; it is the primary key.
   - `201 Created` — registered. The response has no body.
   - `409 Conflict` — that code is taken. Do **not** retry with the same code; pick another or confirm you already own it with `getMaintainer`.
2. **Confirm the registration** — `getMaintainer` (`GET /maintainers/{maintainerCode}`). Returns the maintainer record. `403` means your credentials are not scoped to that company; `404` means it does not exist.
3. **Publish the application metadata** — `createMaintainerApplication` (`POST /maintainers/{maintainerCode}/apps`).
   The body is an `Application`: a `header` (id, version, name, description, icon, url, type, category, size, `ociImageUrl`, visibility flags, and a `localization[]` array of per-language name/description), a `requirements` block (`dependencies[]`, `platform` as architecture/variant/os, `hardware`, `features[]`), and a `maintainer` block.
   - Applications are identified by **id *and* version** — `demo.id.appl` at `2.2` is a different resource from the same id at `2.3`.
   - `type` is an OCI-style media range, e.g. `application/vnd.rdk-app.dac.lightning`. `ociImageUrl` points at the container image the bundle will be built from.
   - `201 Created` — published, no body. `409 Conflict` — that id+version already exists; use `replaceMaintainerApplication` instead. `400` — bad request. `401 Access denied` — note this operation returns **401**, not the 403 that most maintainer operations return; handle both.
4. **Read it back** — `getMaintainerApplication` (`GET /maintainers/{maintainerCode}/apps/{applicationId}`). Returns `MaintainerApplicationDetails`, the maintainer projection of the object. The set-top-box projection is a different shape; do not assume the fields match.
5. **Update in place** — `replaceMaintainerApplication` (`PUT /maintainers/{maintainerCode}/apps/{applicationId}`). This is a full replace, not a patch — send the whole `ApplicationForUpdate` body. Returns `204 No Content`. Being a PUT it is naturally idempotent, so it is the safe operation to retry.
6. **List what you have published** — `listMaintainerApplications` (`GET /maintainers/{maintainerCode}/apps`) with `offset` and `limit`. Read `meta.resultSet.total` to page; do not assume the first page is everything.
7. **Withdraw** — `deleteMaintainerApplication` (`DELETE /maintainers/{maintainerCode}/apps/{applicationId}`). Returns `204`. A `403` here means literally "the requestor is not allowed to delete the application" — a scoping problem, not a bad request.

## Error handling

Errors carry at most `{"message": "..."}` on `application/json`, and **most declared 4xx/5xx responses have no body at all**, so branch on the status code, not the payload. The API is not RFC 9457. Three documented numeric codes appear inside 400 descriptions: `100217` (platformName mandatory for native apps), `100231` (firmwareVer mandatory for native apps), `100237` (unsupported application type). Full map in `errors/liberty-global-problem-types.yml`.

## Do not assume

The service has no rate-limit contract, no request-id header, no versioning on the wire, and no webhook or callback — nothing tells you asynchronously that a bundle was built from your metadata. Poll the bundle service (see the fetch-an-application-bundle skill).
