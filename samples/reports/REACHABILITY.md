# shop.example.com — reachability by role

Ingest `ing_0001`.

> **A traffic-derived relationship means “observed in this capture”, not “reachable in general”.** SignalMap surfaces hypotheses and the evidence needed to test them. It never declares a vulnerability, and nothing below has been tested.

## Cross-role surface

### `user` against `admin`

**2 endpoints reached by `user` that only `admin` is offered.** Each is a broken-access-control *hypothesis*: the lower-privileged session requested it, and nothing that session was served ever names it.

| endpoint | methods | statuses | response | evidence |
| --- | --- | --- | --- | --- |
| `shop.example.com/api/admin/users` | GET | 200 | 779 B | `burp_000019` |
| `shop.example.com/api/config/internal` | GET | 200 | 248 B | `burp_000020` |

**Unknown for each:** `ROLE_BOUNDARY_UNKNOWN` — whether the application authorised the response. A 200 here can be an error page, an empty tenant-scoped result, or a denial the client rendered; the capture cannot tell which.

**Next:** replay each request as `user` and as `admin` and compare the bodies. Equal bodies mean the boundary is not enforced; a denial or an empty result means it is.

## Roles captured

| role | requests | endpoints reached | endpoints offered | URLs fetched |
| --- | ---: | ---: | ---: | ---: |
| `admin` | 10 | 7 | 6 | 10 |
| `user` | 20 | 13 | 8 | 19 |

### `admin` reached 7 endpoint(s)

- `POST shop.example.com/api/admin/flags` — 200, 1 sighting(s) `burp_000029`
- `GET shop.example.com/api/admin/users` — 200, 1 sighting(s) `burp_000028`
- `DELETE shop.example.com/api/admin/users/{id}` — 204, 1 sighting(s) `burp_000030`
- `GET shop.example.com/api/config/internal` — 200, 1 sighting(s) `burp_000032`
- `GET shop.example.com/api/profile` — 200, 1 sighting(s) `burp_000033`
- `GET shop.example.com/api/reports/export/raw` — 200, 1 sighting(s) `burp_000031`
- `POST shop.example.com/api/session` — 200, 1 sighting(s) `burp_000027`

### `user` reached 13 endpoint(s)

- `PUT shop.example.com/api/account/address` — 200, 1 sighting(s) `burp_000011`
- `PATCH shop.example.com/api/account/preferences` — 200, 1 sighting(s) `burp_000012`
- `GET shop.example.com/api/admin/users` — 200, 1 sighting(s) `burp_000019`
- `GET shop.example.com/api/billing/invoices` — 401, 1 sighting(s) `burp_000018`
- `GET shop.example.com/api/config/internal` — 200, 1 sighting(s) `burp_000020`
- `POST shop.example.com/api/export` — 201, 1 sighting(s) `burp_000010`
- `GET shop.example.com/api/internal/debug` — 403, 1 sighting(s) `burp_000017`
- `GET shop.example.com/api/orders` — 200, 1 sighting(s) `burp_000008`
- `GET shop.example.com/api/orders/{id}` — 200, 1 sighting(s) `burp_000009`
- `GET shop.example.com/api/profile` — 200, 1 sighting(s) `burp_000007`
- `POST shop.example.com/api/session` — 200, 1 sighting(s) `burp_000006`
- `POST shop.example.com/graphql` — 200, 1 sighting(s) `burp_000013`
- `GET shop.example.com/static/app.js.map` — 200, 1 sighting(s) `burp_000004`

## State-changing requests

7 endpoint/method pair(s) that changed state in this capture. No passive scan can observe one of these, and no SignalMap command will ever send one.

- `DELETE shop.example.com/api/admin/users/{id}` — as `admin`
- `PATCH shop.example.com/api/account/preferences` — as `user`
- `POST shop.example.com/api/admin/flags` — as `admin`
- `POST shop.example.com/api/export` — as `user`
- `POST shop.example.com/api/session` — as `admin`, `user`
- `POST shop.example.com/graphql` — as `user`
- `PUT shop.example.com/api/account/address` — as `user`

## What this comparison can miss

`offered` is read from the endpoint paths written as literals in the pages and bundles each identity was served. Three consequences follow, and all of them make this list **shorter** than the truth rather than longer:

- A path assembled at runtime — a template literal, a concatenation, a value out of configuration — is not recovered, so an endpoint offered only that way is in nobody's `offered` set and can never be reported as crossed.
- An endpoint neither interface names is in neither set, so reaching it is not a crossing here even though it may be the most interesting request in the capture. Read `ATTACK_SURFACE.md` for those.
- A capture only shows where somebody browsed. An interface nobody opened was never served, so nothing it names is counted as offered to anyone.

The reverse error is rarer but possible: two identities served the same bundle share its endpoints, so a genuine boundary enforced purely server-side, behind a shared frontend, will not appear as a difference.

## Evidence

Every `burp_` id resolves to one line in `evidences/exchanges.jsonl`, which names the content-addressed blobs under `evidences/blobs/` holding that exchange's redacted request and response.
