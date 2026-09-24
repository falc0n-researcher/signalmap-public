# SignalMap evidence — shop.example.com

Scan `ing_0001` · lens `methodology`

## How to use this

- Use only the evidence in this package. Nothing here may be supplemented from memory, from the web, or from what is typical of applications like this one.
- Name no endpoint, route, parameter or host that does not appear in this package. If the answer needs one that is not here, say that it is not here.
- Cite evidence ids for every conclusion that rests on one.
- Distinguish observed from inferred from unknown, and state the unknowns rather than filling them. An absence in this package is not evidence of absence in the target.
- Produce hypotheses and the evidence that would test them, never findings and never payloads. SignalMap tested nothing.

## Target

Read a capture of traffic an authorised operator drove through the application while signed in. SignalMap itself sent no request. Every relationship means 'observed in this capture', never 'reachable in general'. Credential, cookie and personal-data values were redacted before anything was stored.

Entities: 1 ai_service, 1 api_service, 5 asset, 1 auth_mechanism, 1 auth_route, 18 endpoint, 1 exposure, 4 external_host, 2 form, 1 graphql_operation, 1 host, 1 library, 1 package, 13 page, 19 parameter, 2 role, 15 route, 4 runtime_config, 3 script, 8 secret_candidate, 13 security_control, 6 source_file, 1 source_map, 1 sse_endpoint, 2 technology, 1 websocket

## Architecture

### route (15)
- cdn.example.net/  `ev_0140, ev_0143`
- shop.example.com/ — /  `ev_0006`
- shop.example.com/admin — /admin  `ev_0008`
- shop.example.com/api  `ev_0139, ev_0142`
- shop.example.com/api/admin/users — /api/admin/users  `ev_0010`
- shop.example.com/api/config/internal — /api/config/internal  `ev_0018`
- shop.example.com/api/orders — /api/orders  `ev_0026`
- shop.example.com/api/orders/9001 — /api/orders/{id}  `ev_0024`
- shop.example.com/api/profile — /api/profile  `ev_0030`
- shop.example.com/api/reports/export/raw — /api/reports/export/raw  `ev_0028`
- shop.example.com/login — /login  `ev_0012`
- shop.example.com/static/admin.chunk.js — /static/admin.chunk.js  `ev_0014`
- shop.example.com/static/app.js — /static/app.js  `ev_0016`
- shop.example.com/static/app.js.map — /static/app.js.map  `ev_0020`
- shop.example.com/static/vendor.js — /static/vendor.js  `ev_0022`

### endpoint (18)
- shop.example.com/api/account/address — PUT · /api/account/address  `ev_0098, ev_0156`
    - uses_parameter shop.example.com/api/account/address?account_id=
    - uses_parameter shop.example.com/api/account/address?line1=
    - uses_parameter shop.example.com/api/account/address?postcode=
- shop.example.com/api/account/preferences — PATCH · /api/account/preferences  `ev_0157`
    - uses_parameter shop.example.com/api/account/preferences?currency=
    - uses_parameter shop.example.com/api/account/preferences?marketing=
- shop.example.com/api/admin/audit — GET · /api/admin/audit  `ev_0123`
    - requires_role admin
- shop.example.com/api/admin/flags — POST · /api/admin/flags  `ev_0115, ev_0158`
    - uses_parameter shop.example.com/api/admin/flags?enabled=
    - uses_parameter shop.example.com/api/admin/flags?flag=
    - requires_role admin
- shop.example.com/api/admin/users — GET · /api/admin/users  `ev_0113, ev_0159, ev_0227`
    - requires_role admin
- shop.example.com/api/admin/users/{id} — DELETE · /api/admin/users/{id}  `ev_0119, ev_0160`
    - uses_parameter shop.example.com/api/admin/users/{id}?id=
    - requires_role admin
- shop.example.com/api/billing/invoices — GET · /api/billing/invoices  `ev_0161`
- shop.example.com/api/config/internal — GET · /api/config/internal  `ev_0121, ev_0162, ev_0228`
    - requires_role admin
- shop.example.com/api/export — POST · /api/export  `ev_0100, ev_0163`
    - uses_parameter shop.example.com/api/export?account_id=
    - uses_parameter shop.example.com/api/export?card=
    - uses_parameter shop.example.com/api/export?format=
- shop.example.com/api/internal/debug — GET · /api/internal/debug  `ev_0104, ev_0164`
- shop.example.com/api/invoices/pending — GET · /api/invoices/pending  `ev_0102`
- shop.example.com/api/orders — GET · /api/orders  `ev_0096, ev_0165`
    - uses_parameter shop.example.com/api/orders?page=
    - uses_parameter shop.example.com/api/orders?status=
- shop.example.com/api/orders/{id} — GET · /api/orders/{id}  `ev_0166`
    - uses_parameter shop.example.com/api/orders/{id}?id=
- shop.example.com/api/profile — GET · /api/profile  `ev_0094, ev_0167`
- shop.example.com/api/reports/export/raw — GET · /api/reports/export/raw  `ev_0117, ev_0168`
    - requires_role admin
- shop.example.com/api/session — POST · /api/session  `ev_0169`
    - uses_parameter shop.example.com/api/session?password=[REDACTED]
    - uses_parameter shop.example.com/api/session?username=
- shop.example.com/graphql — POST · /graphql  `ev_0170`
    - uses_parameter shop.example.com/graphql?operationName=
    - uses_parameter shop.example.com/graphql?query=
    - uses_parameter shop.example.com/graphql?variables.term=
- shop.example.com/static/app.js.map — GET · /static/app.js.map  `ev_0171`

### parameter (19)
- shop.example.com/api/account/address?account_id= — body  `ev_0146`
- shop.example.com/api/account/address?line1= — body  `ev_0147, ev_0194`
- shop.example.com/api/account/address?postcode= — body  `ev_0148, ev_0195`
- shop.example.com/api/account/preferences?currency= — body  `ev_0196`
- shop.example.com/api/account/preferences?marketing= — body  `ev_0197`
- shop.example.com/api/admin/flags?enabled= — body  `ev_0198`
- shop.example.com/api/admin/flags?flag= — body  `ev_0199`
- shop.example.com/api/admin/users/{id}?id= — path  `ev_0149`
- shop.example.com/api/export?account_id= — body  `ev_0200`
- shop.example.com/api/export?card= — body  `ev_0201`
- shop.example.com/api/export?format= — body  `ev_0202`
- shop.example.com/api/orders/{id}?id= — path  `ev_0152`
- shop.example.com/api/orders?page= — query  `ev_0150, ev_0203`
- shop.example.com/api/orders?status= — query  `ev_0151, ev_0204`
- shop.example.com/api/session?password=[REDACTED] — body  `ev_0205`
- shop.example.com/api/session?username= — body  `ev_0206`
- shop.example.com/graphql?operationName= — body  `ev_0207`
- shop.example.com/graphql?query= — body  `ev_0208`
- shop.example.com/graphql?variables.term= — body  `ev_0209`

### script (3)
- https://shop.example.com/static/admin.chunk.js  `ev_0111`
    - references shop.example.com/api/admin/users
    - references shop.example.com/api/admin/flags
    - references shop.example.com/api/reports/export/raw
    - references shop.example.com/api/admin/users/{id}
- https://shop.example.com/static/app.js  `ev_0092`
    - references shop.example.com/api/profile
    - references shop.example.com/api/orders
    - references shop.example.com/api/account/address
    - references shop.example.com/api/export
- https://shop.example.com/static/vendor.js  `ev_0091`

### auth_route (1)
- https://shop.example.com/login  `ev_0136`

### auth_mechanism (1)
- jwt  `ev_0137`

### technology (2)
- Express  `ev_0033`
- nginx  `ev_0032`

### library (1)
- jQuery@3.6.0 — version banner  `ev_0124`
    - ships jquery

### external_host (4)
- api.openai.com  `ev_0050, ev_0055, ev_0210`
- api.stripe.com  `ev_0051, ev_0056`
- cdn.example.net  `ev_0089, ev_0211`
- metrics.example.net  `ev_0052, ev_0057, ev_0212`

### runtime_config (4)
- https://shop.example.com/#__CONFIG__  `ev_0138`
    - configures https://shop.example.com/
- https://shop.example.com/login#__CONFIG__  `ev_0141`
    - configures https://shop.example.com/login
- https://shop.example.com/static/app.js#CONFIG  `ev_0144`
    - configures https://shop.example.com/static/app.js
- https://shop.example.com/static/app.js#ROUTES  `ev_0145`
    - configures https://shop.example.com/static/app.js

### source_map (1)
- https://shop.example.com/static/app.js.map  `ev_0126`
    - reconstructs src/index.ts
    - reconstructs src/api/profile.ts
    - reconstructs src/api/orders.ts
    - reconstructs src/api/admin.ts

### form (2)
- https://shop.example.com/ — POST  `ev_0071, ev_0074`
- https://shop.example.com/login — POST  `ev_0078`

## Signals — attention, never verdicts

- **2 endpoint(s) reached by 'user' that only 'admin' is offered** — GET shop.example.com/api/admin/users → 200; GET shop.example.com/api/config/internal → 200
    - why: two captured identities disagree about this endpoint: the lower-privileged one requested it, and nothing that identity's interface was served ever names it. That is where a broken access control would look exactly like this
    - unknown: ROLE_BOUNDARY_UNKNOWN
- **1 model API the application refers to** — api.openai.com/v1/chat/completions — OpenAI
    - why: the client-side code names a model API, and a credential for one of those providers appears in the same shipped code. Together those say the key reaches the browser
    - unknown: AUTHORIZATION_UNKNOWN
- **4 parameters that read as object references (IDOR candidates)** — account_id on shop.example.com/api/account/address, id on shop.example.com/api/admin/users/{id}, account_id (integer) on shop.example.com/api/export, id (integer) on shop.example.com/api/orders/{id}
    - why: an id a client supplies is where a missing per-object authorization check shows up - a reading of the parameter's name and value shape, never a claim
- **2 endpoints refused an anonymous request (401/403)** — 401 shop.example.com/api/billing/invoices, 403 shop.example.com/api/internal/debug
    - why: the endpoint itself refused the scan for lack of credentials - the clearest evidence of an authorization boundary, and the surface an authenticated session would open
    - unknown: AUTHENTICATED_STATE_NOT_OBSERVED
- **35 entities mapped while holding your session** — 15 route(s), 18 endpoint(s)
    - why: this scan ran signed in, so the model describes the application a signed-in user sees - which part of it needs the session is the one thing a single run cannot say
    - unknown: UNAUTHENTICATED_STATE_NOT_OBSERVED, AUTHORIZATION_UNKNOWN
- **1 model-provider key in shipped code** — https://shop.example.com/static/app.js#f113eebd68b4 — OpenAI
    - why: a OpenAI credential format appears in code the browser downloads. Unlike most credentials this one is metered and needs no access to the application to spend: whoever holds it can bill the owner directly
- **2 endpoints referenced in source but never observed in use** — shop.example.com/api/admin/audit, shop.example.com/api/invoices/pending
    - why: the application's own code names these, and nothing was seen calling them during this scan - the disagreement is the interesting part
    - unknown: AUTHORIZATION_UNKNOWN, RUNTIME_UNKNOWN
- **1 parameter that read as credentials carried in a URL** — password on shop.example.com/api/session
    - why: a token or key in a query string ends up in logs, history and referers - a reading of the parameter's name and value shape, never a claim
- **1 collector did not see everything** — browser is NOT_CONFIGURED: disabled by configuration
    - why: every count in this scan is a floor rather than a total, and a signal that did not fire may only mean nothing looked
- **6 endpoints called at runtime with no source reference** — shop.example.com/api/account/preferences, shop.example.com/api/billing/invoices, shop.example.com/api/orders/{id}, shop.example.com/api/session, shop.example.com/graphql, shop.example.com/static/app.js.map
    - why: something requested these while the page ran, and no bundle this scan read mentions them - the caller is code that was not collected
- **7 secret candidates in shipped code** — values whose shape matches a known credential format, or whose name says so Some of these formats are publishable by design and are protected by an origin or referrer restriction rather than by secrecy - check the restriction, not the exposure.
    - why: shape is not proof - many are publishable keys - but a credential format in client code is worth a human deciding about
- **1 published source map** — the original source of 1 bundle is recoverable
    - why: a source map turns a minified bundle back into readable code, including paths and dependency names the build intended to hide
- **No identity providers, 1 authentication route** — https://shop.example.com/login
    - why: how an application delegates identity shapes everything behind it
    - unknown: AUTHENTICATED_STATE_NOT_OBSERVED
- **4 configuration blocks served to the browser** — the application hands its own settings to anyone who loads the page
    - why: configuration names services, identifiers and endpoints that no crawl of the rendered page would reach

## What is not known

- **AUTH_PROVIDER_UNKNOWN** auth_route:https://shop.example.com/login — which identity provider stands behind this authentication surface; a login route exists and no provider was identified
- **AUTHORIZATION_UNKNOWN** endpoint:shop.example.com/api/account/address — SignalMap sends GET only and never calls a discovered endpoint
- **AUTHORIZATION_UNKNOWN** endpoint:shop.example.com/api/account/preferences — SignalMap sends GET only and never calls a discovered endpoint
- **AUTHORIZATION_UNKNOWN** endpoint:shop.example.com/api/admin/audit — SignalMap sends GET only and never calls a discovered endpoint
- **RUNTIME_UNKNOWN** endpoint:shop.example.com/api/admin/audit — api_state is STATIC_ONLY
- **AUTHORIZATION_UNKNOWN** endpoint:shop.example.com/api/admin/flags — SignalMap sends GET only and never calls a discovered endpoint
- **AUTHORIZATION_UNKNOWN** endpoint:shop.example.com/api/admin/users — SignalMap sends GET only and never calls a discovered endpoint
- **ROLE_BOUNDARY_UNKNOWN** endpoint:shop.example.com/api/admin/users — reached by user, offered only to admin
- **AUTHORIZATION_UNKNOWN** endpoint:shop.example.com/api/admin/users/{id} — SignalMap sends GET only and never calls a discovered endpoint
- **AUTHORIZATION_UNKNOWN** endpoint:shop.example.com/api/billing/invoices — SignalMap sends GET only and never calls a discovered endpoint
- **AUTHORIZATION_UNKNOWN** endpoint:shop.example.com/api/config/internal — SignalMap sends GET only and never calls a discovered endpoint
- **ROLE_BOUNDARY_UNKNOWN** endpoint:shop.example.com/api/config/internal — reached by user, offered only to admin
- **AUTHORIZATION_UNKNOWN** endpoint:shop.example.com/api/export — SignalMap sends GET only and never calls a discovered endpoint
- **AUTHORIZATION_UNKNOWN** endpoint:shop.example.com/api/internal/debug — SignalMap sends GET only and never calls a discovered endpoint
- **API_PURPOSE_UNKNOWN** endpoint:shop.example.com/api/internal/debug — what this endpoint is for; its path names no recognisable resource
- **AUTHORIZATION_UNKNOWN** endpoint:shop.example.com/api/invoices/pending — SignalMap sends GET only and never calls a discovered endpoint
- **RUNTIME_UNKNOWN** endpoint:shop.example.com/api/invoices/pending — api_state is STATIC_ONLY
- **AUTHORIZATION_UNKNOWN** endpoint:shop.example.com/api/orders — SignalMap sends GET only and never calls a discovered endpoint
- **AUTHORIZATION_UNKNOWN** endpoint:shop.example.com/api/orders/{id} — SignalMap sends GET only and never calls a discovered endpoint
- **AUTHORIZATION_UNKNOWN** endpoint:shop.example.com/api/profile — SignalMap sends GET only and never calls a discovered endpoint
- **AUTHORIZATION_UNKNOWN** endpoint:shop.example.com/api/reports/export/raw — SignalMap sends GET only and never calls a discovered endpoint
- **AUTHORIZATION_UNKNOWN** endpoint:shop.example.com/api/session — SignalMap sends GET only and never calls a discovered endpoint
- **AUTHORIZATION_UNKNOWN** endpoint:shop.example.com/graphql — SignalMap sends GET only and never calls a discovered endpoint
- **AUTHORIZATION_UNKNOWN** endpoint:shop.example.com/static/app.js.map — SignalMap sends GET only and never calls a discovered endpoint
- **API_PURPOSE_UNKNOWN** endpoint:shop.example.com/static/app.js.map — what this endpoint is for; its path names no recognisable resource
- **OWNERSHIP_UNKNOWN** external_host:api.openai.com — who operates this third-party host
- **THIRD_PARTY_PURPOSE_UNKNOWN** external_host:api.openai.com — what the application uses this third party for
- **OWNERSHIP_UNKNOWN** external_host:cdn.example.net — who operates this third-party host
- **THIRD_PARTY_PURPOSE_UNKNOWN** external_host:cdn.example.net — what the application uses this third party for
- **OWNERSHIP_UNKNOWN** external_host:metrics.example.net — who operates this third-party host
- **THIRD_PARTY_PURPOSE_UNKNOWN** external_host:metrics.example.net — what the application uses this third party for
- **ROUTE_RUNTIME_NOT_OBSERVED** route:cdn.example.net/ — what this route does when rendered; it was declared or crawled, never navigated
- **ROUTE_RUNTIME_NOT_OBSERVED** route:shop.example.com/ — what this route does when rendered; it was declared or crawled, never navigated
- **ROUTE_RUNTIME_NOT_OBSERVED** route:shop.example.com/admin — what this route does when rendered; it was declared or crawled, never navigated
- **ROUTE_RUNTIME_NOT_OBSERVED** route:shop.example.com/api — what this route does when rendered; it was declared or crawled, never navigated
- **ROUTE_RUNTIME_NOT_OBSERVED** route:shop.example.com/api/admin/users — what this route does when rendered; it was declared or crawled, never navigated
- **ROUTE_RUNTIME_NOT_OBSERVED** route:shop.example.com/api/config/internal — what this route does when rendered; it was declared or crawled, never navigated
- **ROUTE_RUNTIME_NOT_OBSERVED** route:shop.example.com/api/orders — what this route does when rendered; it was declared or crawled, never navigated
- **ROUTE_RUNTIME_NOT_OBSERVED** route:shop.example.com/api/orders/9001 — what this route does when rendered; it was declared or crawled, never navigated
- **ROUTE_RUNTIME_NOT_OBSERVED** route:shop.example.com/api/profile — what this route does when rendered; it was declared or crawled, never navigated
- **ROUTE_RUNTIME_NOT_OBSERVED** route:shop.example.com/api/reports/export/raw — what this route does when rendered; it was declared or crawled, never navigated
- **ROUTE_RUNTIME_NOT_OBSERVED** route:shop.example.com/login — what this route does when rendered; it was declared or crawled, never navigated
- **ROUTE_RUNTIME_NOT_OBSERVED** route:shop.example.com/static/admin.chunk.js — what this route does when rendered; it was declared or crawled, never navigated
- **ROUTE_RUNTIME_NOT_OBSERVED** route:shop.example.com/static/app.js — what this route does when rendered; it was declared or crawled, never navigated
- **ROUTE_RUNTIME_NOT_OBSERVED** route:shop.example.com/static/app.js.map — what this route does when rendered; it was declared or crawled, never navigated
- **ROUTE_RUNTIME_NOT_OBSERVED** route:shop.example.com/static/vendor.js — what this route does when rendered; it was declared or crawled, never navigated
- **JS_ROLE_UNKNOWN** script:https://shop.example.com/static/app.js — what part of the application this script is; nothing in its name says
- **VERSION_UNKNOWN** technology:express — which version of this component shipped
- **VERSION_CONFIDENCE_LOW** technology:nginx — one source, no version banner from a second
- **UNAUTHENTICATED_STATE_NOT_OBSERVED** host:shop.example.com — every request carried the session the operator supplied

## What this scan did not see

Where a collector reports PARTIAL or FAILED, every count drawn from it is a floor rather than a total, and something absent here may simply never have been looked at.

- browser is NOT_CONFIGURED: disabled by configuration
- well-known is NOT_CONFIGURED: disabled by configuration
- openapi is NOT_CONFIGURED: disabled by configuration
