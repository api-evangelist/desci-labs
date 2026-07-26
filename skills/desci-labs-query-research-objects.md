---
name: Query and page through research objects
description: List and search DeSci Labs research objects (dPIDs) across the registry — paginated listing, per-owner lookup, and reverse lookup from a Ceramic stream id — via the public dPID Resolver API.
api: openapi/desci-labs-dpid-resolver-openapi.json
base_url: https://beta.dpid.org/api
authentication: none (public read API)
operations:
- GET /v2/query/dpids
- GET /v2/query/objects
- GET /v2/query/owner/{id}
- GET /v2/query/reverse/{id}
- POST /v2/query/history
---

# Query and page through research objects

The dPID Resolver query surface enumerates the registry. No authentication required. Base URL `https://beta.dpid.org/api`.

## Steps

1. **Page the registry.** Call `GET /v2/query/dpids` with `page` (1-based), `size` (max 100), and `sort` (`desc` = newest first). Set `history=true` to include each object's full version history and `metadata=true` to resolve manifest metadata (`authors`, `title`, `license`); narrow with `fields=` (comma-separated). The response is a `DpidListResponse` (`dpids`, `pagination`).
2. **Enumerate raw objects.** Call `GET /v2/query/objects` for all research objects when you do not need the paginated dPID view.
3. **Filter by owner.** Call `GET /v2/query/owner/{id}` to get every research object for an owner id.
4. **Reverse lookup.** Call `GET /v2/query/reverse/{id}` to find the dPID for a Ceramic stream id.
5. **Batch history.** Call `POST /v2/query/history` with multiple ids to fetch the version history of many objects in one request.

## Rules
- Respect `size` max of 100; page with the returned `pagination` object.
- See `data-model/desci-labs-data-model.yml` for `DpidQueryResult` / `DpidVersion` shapes and `conventions/desci-labs-conventions.yml` for the pagination contract.
