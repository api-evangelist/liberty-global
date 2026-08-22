# Liberty Global (liberty-global)

<!-- API-EVANGELIST-PROVENANCE:BEGIN -->
> ### About this repository
>
> **This is not our API.** This repository is an independent, third-party profile of a company's
> **publicly available** API surface, maintained by [API Evangelist](https://apievangelist.com).
> API Evangelist does not operate, host, resell, or support this company's APIs, and is not
> affiliated with or endorsed by the company unless stated on the profile.
>
> **Where the information came from.** Everything here is assembled from material a member of the
> public can reach with a browser and no credentials — the company's own website, developer portal
> and documentation, the specifications it publishes for public use (OpenAPI, AsyncAPI, JSON Schema,
> `apis.json`, `llms.txt` and similar), its public repositories, and its public status, pricing and
> changelog pages. **Nothing here is obtained by breaching a system, defeating an access control, or
> using credentials of any kind.**
>
> **The rating is an independent assessment.** The Kin Score and Agent Readiness rating are
> independently calculated scores of a company's *public* API artifacts, produced by API Evangelist
> against a published rubric. They are not certifications, endorsements, security assessments, or
> audits, and they score published artifacts — not the quality, safety, or security of the software.
>
> **Corrections, re-scores, and removal are free.** No partnership, contract, or purchase is
> required, and you do not need to justify the request.
>
> - **Something wrong?** Open an issue on this repository, or email
>   [info@apievangelist.com](mailto:info@apievangelist.com).
> - **Published something new?** Ask for a re-score and we will re-run the rating.
> - **Want the listing taken down?** Say so and we will honor it. The profile is reduced to your
>   company name, a factual description, and a link to your own site, and the company is recorded as
>   **unrated** — never scored zero for having asked.
>
> **Response times.** Acknowledgement within **one business day**; removal or restriction within
> **two business days**; corrections and re-scores within **five business days**.
>
> **Not from the company, and here with a question?** You are welcome here — we would rather be the
> front line and point you the right way than have a good report go nowhere. What this repository
> can answer is narrow, though, so it is worth knowing who you are actually looking for:
>
> - **A question about how the API works, an account, billing, or a bug in the service** — that is
>   the company's own support, not us. We profile this API; we do not operate it and cannot see
>   your account.
> - **A bug in an open-source project we only catalog** — file it on that project's own repository.
>   This has happened with a real and correct bug report that reached us instead of the people who
>   could fix it, which helped nobody.
> - **Anything about this listing itself** — the description, the tags, the rating, a missing or
>   wrong artifact — is ours. Open an issue here.
> - **Not sure, or something general about API Evangelist or APIs.io** — open an issue on the
>   [APIs.io Inbox](https://github.com/api-search/inbox) and we will route it.
>
> **This repository contains no software, and we will never ask you to download anything.** There is
> no build, release, installer, or binary here — only text and machine-readable API descriptions, so
> there is nothing here that can be "corrupt" or need "repairing". Any issue, comment, or email
> claiming otherwise and offering a download link is not from us and is hostile. Do not follow the
> link; it is a lure. Report it to GitHub and, if you like, tell us at
> [info@apievangelist.com](mailto:info@apievangelist.com) so we can take it down.
>
> **On a security or compliance team?** Email
> [info@apievangelist.com](mailto:info@apievangelist.com) with *security* in the subject line and
> you will get a person, not a form. We will tell you exactly which public URLs this profile was
> built from so your team can see the same surface we did, and we will take the listing down on
> request while you work through it.
>
> Full detail: **[Where this data comes from](https://apievangelist.com/about/where-our-data-comes-from)**
<!-- API-EVANGELIST-PROVENANCE:END -->

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
