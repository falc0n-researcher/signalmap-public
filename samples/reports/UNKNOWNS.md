# shop.example.com — what is not known

Ingest `ing_0001`.

> **Attention, not severity.** Nothing here is a vulnerability claim. This model was built from captured traffic that an authorised operator drove through the application: every request in it was made by them, not by SignalMap, which sent nothing. Each relationship means *observed in this capture*, never *reachable in general*, and no credential, cookie value or personal datum was stored - values were redacted before anything was written.

## What is not known (50)

- **AUTHORIZATION_UNKNOWN** (18) — whether this endpoint enforces per-object authorization; nothing was requested, so nothing can be said
    - endpoint:shop.example.com/api/account/address, endpoint:shop.example.com/api/account/preferences, endpoint:shop.example.com/api/admin/audit, endpoint:shop.example.com/api/admin/flags, endpoint:shop.example.com/api/admin/users …
- **ROUTE_RUNTIME_NOT_OBSERVED** (15) — what this route does when rendered; it was declared or crawled, never navigated
    - route:cdn.example.net/, route:shop.example.com/, route:shop.example.com/admin, route:shop.example.com/api, route:shop.example.com/api/admin/users …
- **OWNERSHIP_UNKNOWN** (3) — who operates this third-party host
    - external_host:api.openai.com, external_host:cdn.example.net, external_host:metrics.example.net
- **THIRD_PARTY_PURPOSE_UNKNOWN** (3) — what the application uses this third party for
    - external_host:api.openai.com, external_host:cdn.example.net, external_host:metrics.example.net
- **RUNTIME_UNKNOWN** (2) — whether this is reachable at runtime; only static evidence exists
    - endpoint:shop.example.com/api/admin/audit, endpoint:shop.example.com/api/invoices/pending
- **ROLE_BOUNDARY_UNKNOWN** (2) — whether the application authorized this: a lower-privileged session reached an endpoint that only the higher-privileged interface offers, and nothing here says whether the response was permitted or leaked
    - endpoint:shop.example.com/api/admin/users, endpoint:shop.example.com/api/config/internal
- **API_PURPOSE_UNKNOWN** (2) — what this endpoint is for; its path names no recognisable resource
    - endpoint:shop.example.com/api/internal/debug, endpoint:shop.example.com/static/app.js.map
- **AUTH_PROVIDER_UNKNOWN** (1) — which identity provider stands behind this authentication surface; a login route exists and no provider was identified
    - auth_route:https://shop.example.com/login
- **JS_ROLE_UNKNOWN** (1) — what part of the application this script is; nothing in its name says
    - script:https://shop.example.com/static/app.js
- **VERSION_UNKNOWN** (1) — which version of this component shipped
    - technology:express
- **VERSION_CONFIDENCE_LOW** (1) — the version rests on one weak observation; a second source would settle it
    - technology:nginx
- **UNAUTHENTICATED_STATE_NOT_OBSERVED** (1) — what this application looks like signed out; this scan ran with a session throughout, so nothing here distinguishes the two
    - host:shop.example.com

## Blind spots

Where a collector stopped short, every count above is a floor and something absent may simply never have been looked at:

- browser is NOT_CONFIGURED: disabled by configuration
- well-known is NOT_CONFIGURED: disabled by configuration
- openapi is NOT_CONFIGURED: disabled by configuration

## Evidence

Every `ev_` id above resolves to an observation in `evidences/scan.json` and, where a body was stored, to a content-addressed blob under `evidences/blobs/`. `signalmap evidence <host> <id> --show` prints it.
