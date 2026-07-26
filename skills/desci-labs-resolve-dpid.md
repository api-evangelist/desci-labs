---
name: Resolve a dPID to a research object
description: Resolve a DeSci Labs decentralized persistent identifier (dPID) to its full research-object history, manifest metadata, and IPFS data tree using the public dPID Resolver API.
api: openapi/desci-labs-dpid-resolver-openapi.json
base_url: https://beta.dpid.org/api
authentication: none (public read API)
operations:
- GET /v2/resolve/dpid/{dpid}/{versionIx}
- GET /v2/resolve/{path}
- GET /v2/data/dpid/{dpid}
- GET /v2/data/dpid/{dpid}/*
- GET /v2/query/history/{id}
---

# Resolve a dPID to a research object

Use the public dPID Resolver (`https://beta.dpid.org/api`, testnet `https://dev-beta.dpid.org/api`). No authentication is required. Errors come back as `application/json` with an `ErrorResponse` envelope (`error`, `details`, `params`, `path`).

## Steps

1. **Resolve the identifier.** Call `GET /v2/resolve/dpid/{dpid}/{versionIx}` to get the full research-object history and metadata for a specific version, or `GET /v2/resolve/{path}` for flexible path/format resolution. Passing `format=raw` returns a `302` redirect to the underlying IPFS gateway.
2. **Read the version history.** Call `GET /v2/query/history/{id}` for the ordered `versions[]` (each `DpidVersion` has `index`, `cid`, `time`, `resolveUrl`).
3. **List the data tree.** Call `GET /v2/data/dpid/{dpid}` to get the root IPFS folder tree (`IpfsEntry` nodes with `name`, `path`, `cid`, `size`, `type`, `children`).
4. **Descend into a component.** Call `GET /v2/data/dpid/{dpid}/*` with the desired sub-path to fetch a specific component; a missing path returns `404 Path not found`.

## Rules
- A malformed dPID returns `400 Invalid dpid` — validate before retrying.
- Resolver reads are idempotent; see `conventions/desci-labs-conventions.yml`. There is no write path here — publishing goes through `@desci-labs/nodes-lib` against the Nodes backend.
