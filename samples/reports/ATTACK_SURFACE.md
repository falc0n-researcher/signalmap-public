# shop.example.com — attack surface

Ingest `ing_0001`.

> **Attention, not severity.** Nothing here is a vulnerability claim. This model was built from captured traffic that an authorised operator drove through the application: every request in it was made by them, not by SignalMap, which sent nothing. Each relationship means *observed in this capture*, never *reachable in general*, and no credential, cookie value or personal datum was stored - values were redacted before anything was written.

## Attack surface

### Entry points (ranked by reading interest, not risk)

- PUT shop.example.com/api/account/address — BOTH · → 200 · sensitive path · inputs: object_reference `ev_0098, ev_0156`
- DELETE shop.example.com/api/admin/users/{id} — BOTH · → 204 · sensitive path · inputs: object_reference `ev_0119, ev_0160`
- POST shop.example.com/api/export — BOTH · → 201 · sensitive path · inputs: object_reference `ev_0100, ev_0163`
- PATCH shop.example.com/api/account/preferences — RUNTIME_ONLY · → 200 · sensitive path `ev_0157`
- POST shop.example.com/api/admin/flags — BOTH · → 200 · sensitive path `ev_0115, ev_0158`
- GET shop.example.com/api/admin/users — BOTH · → 200 · sensitive path `ev_0113, ev_0159, ev_0227`
- GET shop.example.com/api/billing/invoices — RUNTIME_ONLY · → 401 · sensitive path `ev_0161`
- GET shop.example.com/api/config/internal — BOTH · → 200 · sensitive path `ev_0121, ev_0162, ev_0228`
- GET shop.example.com/api/internal/debug — BOTH · → 403 · sensitive path `ev_0104, ev_0164`
- GET shop.example.com/api/orders/{id} — RUNTIME_ONLY · → 200 · inputs: object_reference `ev_0166`
- GET shop.example.com/api/profile — BOTH · → 200 · sensitive path `ev_0094, ev_0167`
- GET shop.example.com/api/reports/export/raw — BOTH · → 200 · sensitive path `ev_0117, ev_0168`
- POST shop.example.com/api/session — RUNTIME_ONLY · → 200 · inputs: credential `ev_0169`
- GET shop.example.com/api/admin/audit — STATIC_ONLY · sensitive path `ev_0123`
- GET shop.example.com/api/invoices/pending — STATIC_ONLY · sensitive path `ev_0102`
- GET shop.example.com/api/orders — BOTH · → 200 `ev_0096, ev_0165`
- POST shop.example.com/graphql — RUNTIME_ONLY · → 200 `ev_0170`
- GET shop.example.com/static/app.js.map — RUNTIME_ONLY · → 200 `ev_0171`

### Input candidates

A reading of each parameter's name and observed value shape - never a claim, and no value was recorded.

**object reference** (4)
- `account_id` on shop.example.com/api/account/address
- `id` on shop.example.com/api/admin/users/{id}
- `account_id` (integer) on shop.example.com/api/export
- `id` (integer) on shop.example.com/api/orders/{id}

**credential** (1)
- `password` on shop.example.com/api/session

### Refused the anonymous scan (an authorization boundary)

- 401 shop.example.com/api/billing/invoices `ev_0161`
- 403 shop.example.com/api/internal/debug `ev_0104, ev_0164`

### Could not be verified without a session

- 2 endpoint(s) named in code but never observed in use — reachability and who may reach them is untested
- 2 endpoint(s) that refused the anonymous scan — a session would reveal what they guard
- the authenticated application itself: every observation is from an anonymous client, and SignalMap never signs in

## Blind spots

Where a collector stopped short, every count above is a floor and something absent may simply never have been looked at:

- browser is NOT_CONFIGURED: disabled by configuration
- well-known is NOT_CONFIGURED: disabled by configuration
- openapi is NOT_CONFIGURED: disabled by configuration

## Evidence

Every `ev_` id above resolves to an observation in `evidences/scan.json` and, where a body was stored, to a content-addressed blob under `evidences/blobs/`. `signalmap evidence <host> <id> --show` prints it.
