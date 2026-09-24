# SignalMap evidence — shop.example.com

Scan `ing_0001` · lens `attack_surface`

## How to use this

- Use only the evidence in this package. Nothing here may be supplemented from memory, from the web, or from what is typical of applications like this one.
- Name no endpoint, route, parameter or host that does not appear in this package. If the answer needs one that is not here, say that it is not here.
- Cite evidence ids for every conclusion that rests on one.
- Distinguish observed from inferred from unknown, and state the unknowns rather than filling them. An absence in this package is not evidence of absence in the target.
- Produce hypotheses and the evidence that would test them, never findings and never payloads. SignalMap tested nothing.

## Target

Read a capture of traffic an authorised operator drove through the application while signed in. SignalMap itself sent no request. Every relationship means 'observed in this capture', never 'reachable in general'. Credential, cookie and personal-data values were redacted before anything was stored.

Entities: 1 ai_service, 1 api_service, 5 asset, 1 auth_mechanism, 1 auth_route, 18 endpoint, 1 exposure, 4 external_host, 2 form, 1 graphql_operation, 1 host, 1 library, 1 package, 13 page, 19 parameter, 2 role, 15 route, 4 runtime_config, 3 script, 8 secret_candidate, 13 security_control, 6 source_file, 1 source_map, 1 sse_endpoint, 2 technology, 1 websocket

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

## Attack surface

Entry points are ranked by reading interest, not by risk. A candidate input names the class of question a parameter invites (an object reference asks about per-object authorization, a redirect target about validation) - it is a reading of the name and observed value shape, never a finding. SignalMap sent only GET and tested none of this.

### Entry points
- PUT shop.example.com/api/account/address — BOTH · → 200 · sensitive path · inputs: object_reference  `ev_0098, ev_0156`
- DELETE shop.example.com/api/admin/users/{id} — BOTH · → 204 · sensitive path · inputs: object_reference  `ev_0119, ev_0160`
- POST shop.example.com/api/export — BOTH · → 201 · sensitive path · inputs: object_reference  `ev_0100, ev_0163`
- GET shop.example.com/api/billing/invoices — RUNTIME_ONLY · → 401 · sensitive path  `ev_0161`
- GET shop.example.com/api/internal/debug — BOTH · → 403 · sensitive path  `ev_0104, ev_0164`
- PATCH shop.example.com/api/account/preferences — RUNTIME_ONLY · → 200 · sensitive path  `ev_0157`
- POST shop.example.com/api/admin/flags — BOTH · → 200 · sensitive path  `ev_0115, ev_0158`
- GET shop.example.com/api/admin/users — BOTH · → 200 · sensitive path  `ev_0113, ev_0159, ev_0227`
- GET shop.example.com/api/config/internal — BOTH · → 200 · sensitive path  `ev_0121, ev_0162, ev_0228`
- GET shop.example.com/api/orders/{id} — RUNTIME_ONLY · → 200 · inputs: object_reference  `ev_0166`
- GET shop.example.com/api/profile — BOTH · → 200 · sensitive path  `ev_0094, ev_0167`
- GET shop.example.com/api/reports/export/raw — BOTH · → 200 · sensitive path  `ev_0117, ev_0168`
- POST shop.example.com/api/session — RUNTIME_ONLY · → 200 · inputs: credential  `ev_0169`
- GET shop.example.com/api/admin/audit — STATIC_ONLY · sensitive path  `ev_0123`
- GET shop.example.com/api/invoices/pending — STATIC_ONLY · sensitive path  `ev_0102`
- GET shop.example.com/api/orders — BOTH · → 200  `ev_0096, ev_0165`
- POST shop.example.com/graphql — RUNTIME_ONLY · → 200  `ev_0170`
- GET shop.example.com/static/app.js.map — RUNTIME_ONLY · → 200  `ev_0171`

### Input candidates — a reading of name and value shape, never a claim
#### object reference (4)
- `account_id` on shop.example.com/api/account/address [body]
- `id` on shop.example.com/api/admin/users/{id} [path]
- `account_id` (integer) on shop.example.com/api/export [body]
- `id` (integer) on shop.example.com/api/orders/{id} [path]
#### credential (1)
- `password` on shop.example.com/api/session [body]

### Endpoints that refused the anonymous scan
- 401 shop.example.com/api/billing/invoices
- 403 shop.example.com/api/internal/debug

### External services the application's own code calls
- api.stripe.com — affinity high

### Could not be verified without a session
- endpoints referenced in code but never observed in use (2) — the application names these; nothing was seen calling them, so whether they are reachable, and by whom, is untested
- endpoints that refused the anonymous scan (401/403) (2) — these need a session the scan did not have; what they guard is exactly what an authenticated re-scan would reveal

## What this scan did not see

Where a collector reports PARTIAL or FAILED, every count drawn from it is a floor rather than a total, and something absent here may simply never have been looked at.

- browser is NOT_CONFIGURED: disabled by configuration
- well-known is NOT_CONFIGURED: disabled by configuration
- openapi is NOT_CONFIGURED: disabled by configuration
