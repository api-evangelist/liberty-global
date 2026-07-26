---
name: Discover AppStore applications from a set-top box
description: Search the Liberty Global AppStore Metadata Service catalogue and read one application's details through the read-only STB perspective, including the offset/limit paging contract.
api: openapi/liberty-global-appstore-metadata-service-openapi.yml
operations: [listApplications, getApplicationDetails]
generated: '2026-07-25'
method: generated
---

# Discover AppStore applications from a set-top box

This skill covers the **STB** perspective — the read-only half of the AppStore Metadata Service, reached through the **ASMS Proxy**, which deliberately exposes only fetch operations to devices. Two operations, and both are safe to call repeatedly.

## Before you start

- Self-hosted API, no public endpoint, no `servers` block in the specification. The provider's README documents `http://localhost:8082/asms` for this perspective in a local stack; an operator deployment substitutes its own host.
- HTTP Basic auth at the proxy tier, not declared in the spec. The README notes this component "should be providing unique authentication/authorization capabilities in future versions" — treat STB-side auth as unfinished and confirm with the operator.
- Both operations are `connected`/`read` in `agentic-access/liberty-global-agentic-access.yml`. Nothing here mutates state.

## Steps

1. **Search the catalogue** — `listApplications` (`GET /apps`).
   All filters are flat query parameters and all are optional:
   - `name`, `description` — pattern matches, not exact.
   - `version` — **defaults to the literal string `latest`** if you omit it. If you want every version, you have to know that and say otherwise.
   - `type` — an OCI-style media range, e.g. `application/vnd.rdk-app.dac.lightning`.
   - `platform` — an `architecture:[version]:[os]` triple, e.g. `arm:v7:linux`. Send the device's real platform or you will get applications it cannot run.
   - `category` — a closed enum (`Category`), e.g. `application`.
   - `maintainerName` — filter by publishing company.
   - `offset`, `limit` — paging.
2. **Page correctly.** The response is a `StbApplicationsList`: `applications[]` plus `meta.resultSet` with `count` (items in this page), `offset` (items skipped), `limit` (page size) and `total` (items matching the filter). Loop while `offset + count < total`. There is no cursor and no `Link` header.
3. **Read one application** — `getApplicationDetails` (`GET /apps/{applicationId}`).
   `{applicationId}` carries the **id:version** form — the pair identifies the resource, not the id alone.
   Returns `StbApplicationDetails`. This is a different shape from the list item (`StbApplicationHeader`) and a different shape again from what a maintainer sees; there is no single canonical Application representation on the wire.
4. **Handle the documented 400s.** `getApplicationDetails` returns `400` with three named causes: `100217` platformName is mandatory for native apps, `100231` firmwareVer is mandatory for native apps, `100237` unsupported application type. For native apps you must supply platform and firmware context — the request will not succeed without it.
5. **Then go get the bundle.** Metadata tells you an application exists and gives you its `ociImageUrl`; it does not give you the installable artifact. Use the fetch-an-application-bundle skill.

## Error handling

`403 Access denied` and `404 Not Found` are declared without bodies. The only typed error body is the shared `{"message": "..."}` `ErrorResponse` on the `default` response. Branch on status codes. See `errors/liberty-global-problem-types.yml`.
