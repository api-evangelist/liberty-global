# Liberty Global (liberty-global)

Liberty Global is a London-headquartered converged connectivity group that owns and operates fixed-broadband and mobile networks across Europe, mostly through joint ventures rather than under its own brand — Virgin Media O2 in the United Kingdom (50/50 with Telefonica), VodafoneZiggo in the Netherlands (50/50 with Vodafone Group), Telenet in Belgium, and Virgin Media Ireland — serving roughly 80 million connections over fibre-rich broadband and 5G. It sits in the value chain as a network owner and holding company, not as a developer-facing platform.

**APIs.json:** [https://raw.githubusercontent.com/api-evangelist/liberty-global/refs/heads/main/apis.yml](https://raw.githubusercontent.com/api-evangelist/liberty-global/refs/heads/main/apis.yml)

## The Honest API Posture

There is no first-party developer portal. Every developer hostname probed on 2026-07-25 failed to resolve — `developer.libertyglobal.com`, `developers.libertyglobal.com`, `docs.libertyglobal.com`, `api.libertyglobal.com`, `opengateway.libertyglobal.com`, `developers.opengateway.libertyglobal.com` — and `/developer`, `/developers`, `/api` and `/opengateway` on the corporate site all return HTTP 404.

The sharpest artifact is one Liberty Global publishes about itself: its own GitHub organisation profile lists `https://developer.libertyglobal.com` as its blog and `developers@libertyglobal.com` as its contact. That hostname is NXDOMAIN. The company still points the world at a developer programme that no longer exists.

## CAMARA and GSMA Open Gateway

Liberty Global is a genuine GSMA Open Gateway participant — part of the initiative since its launch at MWC 2023 — and its February 2024 Network-as-a-Service framework with AWS explicitly commits to CAMARA standard APIs. But nothing is callable from the parent. Every CAMARA API with real evidence behind it ships through an operating joint venture:

- **SIM Swap** — Virgin Media O2 (UK) and VodafoneZiggo (NL)
- **Number Verification** — VodafoneZiggo (NL)
- **KYC Age Verification** — Virgin Media O2 (UK), commercially launched with BT, EE and Vodafone on 2025-09-23 under the UK Online Safety Act
- **KYC Tenure** — Virgin Media O2 (UK), same launch
- **KYC Match** — Virgin Media O2 (UK), announced for end of 2025

Liberty Global does not appear on Aduna's operator roster; its JV parents Telefonica and Vodafone do. Any aggregator reach is inherited through the partner, not held directly. No TM Forum Open API conformance, no public NEF/SCEF surface, no network-slicing or edge/MEC API, no webhooks, no AsyncAPI, no public Postman workspace, and no first-party API SDKs were found. CIBA appears nowhere.

## APIs

The only OpenAPI definitions Liberty Global itself publishes are three Apache-2.0 RDK set-top-box App Store component specs in its public GitHub organisation, copyright Liberty Global Technology Services BV. All three were fetched verbatim and parse as valid OpenAPI 3.0. None declares a `servers` block or a security scheme — they describe operator-hosted services, not a consumable public API product.

### AppStore Metadata Service API

The ASMS (AppStore Metadata Service) REST API — the MAS API in RDK. Manages application metadata and maintainer records for the RDK-based set-top box app store.

- **Human URL:** [https://github.com/LibertyGlobal/appstore-metadata-service](https://github.com/LibertyGlobal/appstore-metadata-service)
- **OpenAPI:** [openapi/liberty-global-appstore-metadata-service-openapi.yml](openapi/liberty-global-appstore-metadata-service-openapi.yml) — OpenAPI 3.0.0, version 0.7.0
- **Paths:** `/apps`, `/apps/{applicationId}`, `/maintainers`, `/maintainers/{maintainerCode}`, `/maintainers/{maintainerCode}/apps`, `/maintainers/{maintainerCode}/apps/{applicationId}`

### AppStore Bundle Service API

Handles the application bundle generation process by interacting with the Bundle Generator and Bundle Cryptor services.

- **Human URL:** [https://github.com/LibertyGlobal/appstore-bundle-service](https://github.com/LibertyGlobal/appstore-bundle-service)
- **OpenAPI:** [openapi/liberty-global-appstore-bundle-service-openapi.yml](openapi/liberty-global-appstore-bundle-service-openapi.yml) — OpenAPI 3.0.1, version 0.0.1

### AppStore Caching Service API

Acts as a caching proxy in front of the AppStore Bundle Service, serving generated application bundles.

- **Human URL:** [https://github.com/LibertyGlobal/appstore-caching-service](https://github.com/LibertyGlobal/appstore-caching-service)
- **OpenAPI:** [openapi/liberty-global-appstore-caching-service-openapi.yml](openapi/liberty-global-appstore-caching-service-openapi.yml) — OpenAPI 3.0.1, version 0.0.1

## Tags

- Telecommunications
- United Kingdom
- Broadband
- Fixed Broadband
- Mobile Network Operator
- Network APIs
- CAMARA
- Open Gateway
- 5G
- Europe
- Set-Top Box
- RDK

## Links

- [Website](https://www.libertyglobal.com/)
- [GitHub Organization](https://github.com/LibertyGlobal)
- [LinkedIn](https://www.linkedin.com/company/liberty-global)
- [News](https://www.libertyglobal.com/news/)

## Timestamps

- **Created:** 2026-07-25
- **Modified:** 2026-07-25
