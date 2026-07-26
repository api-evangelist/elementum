---
name: Manage Elementum records
description: Authenticate, then search, read, create, and update records in an Elementum app, element, or task using the REST API v1.
api: openapi/elementum-openapi-original.json
operations: [requestAccessToken, recordList, getRecord, createRecord, updateRecord]
---

# Manage Elementum records

Operate on records inside an Elementum Object (an App, Element, or Task) via the v1 REST API.

## Prerequisites
- A Client ID and Client Secret (User Settings > OAuth > Create New Token > API Access).
- Know your `recordType` (`apps`, `elements`, `tasks`, or `transactions`) and the Object `alias` (namespace or UUID).
- Base URL `https://api.elementum.io/v1` (EU: `https://api.eu.elementum.io/v1`).

## Steps
1. **Get a token** — `requestAccessToken`: `POST /oauth/token` with HTTP Basic Auth (`client_id:client_secret`) and body `grant_type=client_credentials`. Use the returned Bearer token in `Authorization: Bearer <token>`. Tokens expire after 24 hours.
2. **Search records** — `recordList`: `GET /{recordType}/{alias}`. Page with Relay cursors (`first`+`after` forward, `last`+`before` back). Narrow with an RSQL `filter`, e.g. `Status==Open;Priority==High` (`;`=AND, `,`=OR; URL-encode).
3. **Read one** — `getRecord`: `GET /{recordType}/{alias}/{id}`.
4. **Create** — `createRecord`: `POST /{recordType}/{alias}` (returns 201).
5. **Update** — `updateRecord`: `PUT /{recordType}/{alias}/{id}`.

## Rules
- A 401 means the token is missing/expired/wrong — re-request one. A 403 means the service account lacks a data-access policy for this Object.
- On 429 (rate limited), back off and retry.
- Errors are plain HTTP status codes (see errors/elementum-problem-types.yml). No idempotency key exists, so guard against duplicate creates yourself.
