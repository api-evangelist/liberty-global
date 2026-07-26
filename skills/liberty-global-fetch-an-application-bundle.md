---
name: Fetch a generated application bundle
description: Retrieve an RDK/DAC application bundle through the Liberty Global AppStore Caching Service and Bundle Service, including the 202-then-poll generation contract and the two-hop 404 semantics.
api: openapi/liberty-global-appstore-caching-service-openapi.yml
also_uses: openapi/liberty-global-appstore-bundle-service-openapi.yml
operations: []
operations_note: Neither service declares an operationId; both expose a single GET on a five-segment composite path.
generated: '2026-07-25'
method: generated
---

# Fetch a generated application bundle

Metadata (ASMS) says an application exists. This skill gets you the installable artifact. Two services are involved and they are a chain, not alternatives.

## The chain

```
client → AppStore Caching Service → AppStore Bundle Service → AppStore Metadata Service
                                          ↓ (AMQP)
                              Bundle Generator + Bundle Cryptor
```

Only the first three links are specified in OpenAPI. The generator and cryptor are driven over RabbitMQ queues (`bundlegen-service-requests`, `bundlegen-service-status`, `bundlecrypt-service-requests`, `bundlecrypt-service-status`) whose names the README publishes but whose messages are not specified anywhere.

## Before you start

- Self-hosted. Neither specification declares a `servers` block; the READMEs document `http://localhost:8080` for both services in a local run.
- Neither service declares a `securityScheme` and neither README shows an `Authorization` header on these calls. Treat them as internal services the operator fronts.
- **Neither operation has an `operationId`**, so they cannot be referenced by id from tooling, Arazzo workflows or generated clients. Address them by method and path.

## Steps

1. **Ask the caching service** — `GET /{appId}/{appVersion}/{platformName}/{firmwareVersion}/{appBundleName}`.
   All five segments are required path parameters and together they are the identity of the bundle. `appVersion` and `firmwareVersion` matter: a bundle is built for a specific app version against a specific firmware.
   - `200` — the bundle is cached; the body is the artifact stream.
   - `202` — not cached yet; the bundle service is generating it. **Poll the same URL.** There is no callback, no webhook and no status endpoint.
   - `404` — "Application not found in AppStore Bundle Service".
   - `400` / `500` — these two carry a real body: the shared `ErrorResponse`, `{"message": "..."}`. The caching service is the only one of the three services that specifies an explicit 500 with a body.
2. **If you are calling the bundle service directly** — `GET /applications/{appId}/{appVersion}/{platformName}/{firmwareVersion}/{appBundleName}`. Same five-segment key, prefixed with `/applications`.
   - `202` — generation in progress; poll.
   - `404` — **"Application not found in AppStore Metadata Service"**. Read that literally: the bundle service resolved your request against ASMS and ASMS has no such application. A 404 here is a *metadata* problem, not a bundle problem — go back and check with `getApplicationDetails`.
   - `400` — `ErrorResponse` body.
3. **Back off between polls.** There is no rate-limit contract, no `Retry-After`, and no documented generation time. Use your own bounded exponential backoff and a hard timeout; nothing in the API will tell you a generation has failed permanently.

## Do not assume

There is no way to list bundles, cancel a generation, or subscribe to a completion event. The only affordance is GET-and-poll on a composite key you already know. If you do not know the `appBundleName`, the API cannot tell you it.

See `conventions/liberty-global-conventions.yml` for the async semantics and `errors/liberty-global-problem-types.yml` for the full status map.
