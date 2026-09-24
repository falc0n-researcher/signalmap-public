# shop.example.com — reconnaissance brief

Ingest `ing_0001`.

> **Attention, not severity.** Nothing here is a vulnerability claim. This model was built from captured traffic that an authorised operator drove through the application: every request in it was made by them, not by SignalMap, which sent nothing. Each relationship means *observed in this capture*, never *reachable in general*, and no credential, cookie value or personal datum was stored - values were redacted before anything was written.

## Application

`shop.example.com` is a single-page application with 15 routes, 18 API endpoints and 3 scripts, built on Express, nginx.

The browser rendered 0 of the 15 routes; 16 of the 18 endpoints were observed being called.

## Shape

- **15** routes
- **18** API endpoints
- **19** parameters
- **3** scripts
- **1** auth route
- **2** technologies
- **1** library
- **4** third-party hosts
- **1** source map
- **2** forms

## Architecture

- **Frontend** — shop.example.com
- **Payment** — api.stripe.com

## Routes (15)

- cdn.example.net/ — declared `ev_0140, ev_0143`
- shop.example.com/ — declared `ev_0006`
- shop.example.com/admin — declared `ev_0008`
- shop.example.com/api — declared `ev_0139, ev_0142`
- shop.example.com/api/admin/users — declared `ev_0010`
- shop.example.com/api/config/internal — declared `ev_0018`
- shop.example.com/api/orders — declared `ev_0026`
- shop.example.com/api/orders/9001 — declared `ev_0024`
- …and 7 more — `signalmap query <host> "what routes exist?"`

## APIs (18)

10 both, 6 runtime only, 2 static only.

- PUT shop.example.com/api/account/address — both · parameters: account_id, line1, postcode `ev_0098, ev_0156`
- POST shop.example.com/api/admin/flags — both · parameters: enabled, flag `ev_0115, ev_0158`
- GET shop.example.com/api/admin/users — both `ev_0113, ev_0159, ev_0227`
- DELETE shop.example.com/api/admin/users/{id} — both · parameters: id `ev_0119, ev_0160`
- GET shop.example.com/api/config/internal — both `ev_0121, ev_0162, ev_0228`
- POST shop.example.com/api/export — both · parameters: account_id, card, format `ev_0100, ev_0163`
- GET shop.example.com/api/internal/debug — both `ev_0104, ev_0164`
- GET shop.example.com/api/orders — both · parameters: page, status `ev_0096, ev_0165`
- …and 10 more

## JavaScript (3 scripts)

2 first-party, 1 library; 1 libraries identified; 1 source maps; 0 workers.

| Script | Ownership | Role | Runtime | Map |
| --- | --- | --- | --- | --- |
| admin.chunk.js | first-party | route-chunk | no | no |
| app.js | first-party | unknown | no | yes |

Libraries: jQuery@3.6.0.

## Authentication

- Identity providers: none identified (`AUTH_PROVIDER_UNKNOWN`)
- Authentication routes: https://shop.example.com/login
- Mechanisms: jwt
- Authenticated state: **not observed** — SignalMap never signs in.

## Third parties (4)

- api.stripe.com — payment, affinity high
- api.openai.com — unknown purpose, affinity unknown
- cdn.example.net — unknown purpose, affinity unknown
- metrics.example.net — unknown purpose, affinity unknown

## Technology (2)

| Product | Version | Confidence | Identified by |
| --- | --- | --- | --- |
| Express | — | none | headers |
| nginx | 1.25.3 | medium | headers |

## Worth attention first

1. **2 endpoint(s) reached by 'user' that only 'admin' is offered** — GET shop.example.com/api/admin/users → 200; GET shop.example.com/api/config/internal → 200
    - why it stands out: two captured identities disagree about this endpoint: the lower-privileged one requested it, and nothing that identity's interface was served ever names it. That is where a broken access control would look exactly like this
    - ranked by: coverage incomplete, crossed role boundary, runtime evidence, unknown authorization, sensitive workflow · confidence high
    - still unknown: ROLE_BOUNDARY_UNKNOWN
    - where to look: replay each request as the lower role and as the higher one and compare: equal bodies mean the boundary is not enforced, a denial or an empty result means it is. SignalMap has not tested this and makes no claim
    - evidence: ev_0113, ev_0159, ev_0121, ev_0162
2. **1 model API the application refers to** — api.openai.com/v1/chat/completions — OpenAI
    - why it stands out: the client-side code names a model API, and a credential for one of those providers appears in the same shipped code. Together those say the key reaches the browser
    - ranked by: coverage incomplete, runtime evidence, new third party, sensitive workflow · confidence high
    - still unknown: AUTHORIZATION_UNKNOWN
    - where to look: check whether the request is made from the browser or proxied. Providers named: OpenAI
    - evidence: ev_0155
3. **4 parameters that read as object references (IDOR candidates)** — account_id on shop.example.com/api/account/address, id on shop.example.com/api/admin/users/{id}, account_id (integer) on shop.example.com/api/export, id (integer) on shop.example.com/api/orders/{id}
    - why it stands out: an id a client supplies is where a missing per-object authorization check shows up - a reading of the parameter's name and value shape, never a claim
    - ranked by: coverage incomplete, unknown authorization, high application affinity, sensitive workflow · confidence medium
    - where to look: confirm the check that should bound this input, with an authorized test
    - evidence: ev_0146, ev_0149, ev_0200, ev_0152
4. **2 endpoints refused an anonymous request (401/403)** — 401 shop.example.com/api/billing/invoices, 403 shop.example.com/api/internal/debug
    - why it stands out: the endpoint itself refused the scan for lack of credentials - the clearest evidence of an authorization boundary, and the surface an authenticated session would open
    - ranked by: coverage incomplete, unknown authorization, new authentication boundary, sensitive workflow · confidence high
    - still unknown: AUTHENTICATED_STATE_NOT_OBSERVED
    - where to look: re-scan with a session for one of these to see what it guards
    - evidence: ev_0161, ev_0104, ev_0164
5. **35 entities mapped while holding your session** — 15 route(s), 18 endpoint(s)
    - why it stands out: this scan ran signed in, so the model describes the application a signed-in user sees - which part of it needs the session is the one thing a single run cannot say
    - ranked by: coverage incomplete, unknown authorization, sensitive workflow · confidence high
    - still unknown: UNAUTHENTICATED_STATE_NOT_OBSERVED, AUTHORIZATION_UNKNOWN
    - where to look: scan again without --auth-storage-state and diff the two: what only appears in this one is the authenticated surface
6. **1 model-provider key in shipped code** — https://shop.example.com/static/app.js#f113eebd68b4 — OpenAI
    - why it stands out: a OpenAI credential format appears in code the browser downloads. Unlike most credentials this one is metered and needs no access to the application to spend: whoever holds it can bill the owner directly
    - ranked by: coverage incomplete, sensitive workflow, runtime evidence · confidence medium
    - where to look: confirm whether each is a real key or a placeholder, and whether the model call belongs on the server. SignalMap has not validated any of them against any service
    - evidence: ev_0213
7. **2 endpoints referenced in source but never observed in use** — shop.example.com/api/admin/audit, shop.example.com/api/invoices/pending
    - why it stands out: the application's own code names these, and nothing was seen calling them during this scan - the disagreement is the interesting part
    - ranked by: coverage incomplete, unknown authorization, high application affinity, sensitive workflow · confidence high
    - still unknown: AUTHORIZATION_UNKNOWN, RUNTIME_UNKNOWN
    - where to look: confirm whether these are reachable, and by whom
    - evidence: ev_0123, ev_0102
8. **1 parameter that read as credentials carried in a URL** — password on shop.example.com/api/session
    - why it stands out: a token or key in a query string ends up in logs, history and referers - a reading of the parameter's name and value shape, never a claim
    - ranked by: coverage incomplete, high application affinity · confidence medium
    - where to look: confirm the check that should bound this input, with an authorized test
    - evidence: ev_0205
- …and 6 more — see `signalmap signals`.

## What changed

Only one scan exists for this host, so nothing can be compared yet. A second scan gives this section a timeline.

## Impact — applicability, never a verdict

3 components, 2 with an observed version. No advisory feed was supplied.


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

## Where to investigate

1. 2 endpoint(s) reached by 'user' that only 'admin' is offered
    - path: shop.example.com/api/admin/users → shop.example.com/api/config/internal
    - settle first: ROLE_BOUNDARY_UNKNOWN
    - replay each request as the lower role and as the higher one and compare: equal bodies mean the boundary is not enforced, a denial or an empty result means it is. SignalMap has not tested this and makes no claim
2. 1 model API the application refers to
    - path: api.openai.com/v1/chat/completions
    - settle first: AUTHORIZATION_UNKNOWN
    - check whether the request is made from the browser or proxied. Providers named: OpenAI
3. 4 parameters that read as object references (IDOR candidates)
    - path: shop.example.com/api/account/address?account_id= → shop.example.com/api/admin/users/{id}?id= → shop.example.com/api/export?account_id= → shop.example.com/api/orders/{id}?id=
    - confirm the check that should bound this input, with an authorized test
4. 2 endpoints refused an anonymous request (401/403)
    - path: shop.example.com/api/billing/invoices → shop.example.com/api/internal/debug
    - settle first: AUTHENTICATED_STATE_NOT_OBSERVED
    - re-scan with a session for one of these to see what it guards
5. 35 entities mapped while holding your session
    - path: shop.example.com/ → shop.example.com/admin → shop.example.com/api/admin/users → shop.example.com/login → shop.example.com/static/admin.chunk.js
    - settle first: UNAUTHENTICATED_STATE_NOT_OBSERVED, AUTHORIZATION_UNKNOWN
    - scan again without --auth-storage-state and diff the two: what only appears in this one is the authenticated surface
6. 1 model-provider key in shipped code
    - path: https://shop.example.com/static/app.js#f113eebd68b4
    - confirm whether each is a real key or a placeholder, and whether the model call belongs on the server. SignalMap has not validated any of them against any service
7. 2 endpoints referenced in source but never observed in use
    - path: shop.example.com/api/admin/audit → shop.example.com/api/invoices/pending
    - settle first: AUTHORIZATION_UNKNOWN, RUNTIME_UNKNOWN
    - confirm whether these are reachable, and by whom
8. 1 parameter that read as credentials carried in a URL
    - path: shop.example.com/api/session?password=[REDACTED]
    - confirm the check that should bound this input, with an authorized test
9. 1 collector did not see everything
    - raise the budget and re-scan before treating any absence as real
10. 6 endpoints called at runtime with no source reference
    - path: shop.example.com/api/account/preferences → shop.example.com/api/billing/invoices → shop.example.com/api/orders/{id} → shop.example.com/api/session → shop.example.com/graphql
    - find what issues these calls; a chunk may not have been fetched
11. 7 secret candidates in shipped code
    - path: https://shop.example.com/api/config/internal#0a8adf155f0c → https://shop.example.com/api/config/internal#f0568ff8e7d0 → https://shop.example.com/api/session#6f889376de58 → https://shop.example.com/static/app.js#0e0c2062ef04 → https://shop.example.com/static/app.js#3a576da218ce
    - confirm whether each is publishable or was never meant to ship
12. 1 published source map
    - path: https://shop.example.com/static/app.js.map
    - read the reconstructed tree for structure the bundle hides

Record what you conclude: `signalmap annotate <host> <entity> --status interesting|validated|dismissed --note ...`.

## Blind spots

Where a collector stopped short, every count above is a floor and something absent may simply never have been looked at:

- browser is NOT_CONFIGURED: disabled by configuration
- well-known is NOT_CONFIGURED: disabled by configuration
- openapi is NOT_CONFIGURED: disabled by configuration

## Evidence

Every `ev_` id above resolves to an observation in `evidences/scan.json` and, where a body was stored, to a content-addressed blob under `evidences/blobs/`. `signalmap evidence <host> <id> --show` prints it.
