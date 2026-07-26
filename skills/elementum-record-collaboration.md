---
name: Collaborate on an Elementum record
description: Attach files and links, comment, add watchers, and relate records together on an existing Elementum record.
api: openapi/elementum-openapi-original.json
operations: [requestAccessToken, createRecordAttachment, createRecordLink, listRecordAttachments, downloadRecordAttachment, createRecordComment, listRecordComments, createRecordWatchers, createRecordRelatedItems]
---

# Collaborate on an Elementum record

Enrich a record with attachments, comments, watchers, and relationships.

## Prerequisites
- A Bearer token (see the Manage Elementum records skill, `requestAccessToken`).
- A target record: `{recordType}`, `{alias}`, and record `{id}`.

## Steps
1. **Attach a file** — `createRecordAttachment`: `POST /{recordType}/{alias}/{id}/attachments` (returns 202, processed asynchronously).
2. **Attach a link** — `createRecordLink`: `POST /{recordType}/{alias}/{id}/attachments/url-links`.
3. **List / download attachments** — `listRecordAttachments`: `GET .../attachments`; `downloadRecordAttachment`: `GET .../attachments/{attachmentId}/content`.
4. **Comment** — `createRecordComment`: `POST /{recordType}/{alias}/{id}/comment`; read with `listRecordComments`.
5. **Add a watcher** — `createRecordWatchers`: `POST /{recordType}/{alias}/{id}/watchers`.
6. **Relate records** — `createRecordRelatedItems`: `POST /{recordType}/{alias}/{id}/related-items`.

## Rules
- Attachment creation returns 202 — poll `listRecordAttachments` to confirm the file is available before downloading.
- List endpoints use Relay cursor pagination (`first`/`after`).
- Respect 403 (data-access policy) and 429 (rate limit) as in the records skill.
