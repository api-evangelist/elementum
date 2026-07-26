---
name: Provision Elementum users and groups
description: Create and manage organization users and groups, and manage group membership and email domains, via the Elementum REST API.
api: openapi/elementum-openapi-original.json
operations: [requestAccessToken, listUsers, createUser, getUser, deactivateUser, listGroups, createGroup, addUsersToGroup, listGroupUsers, removeUsersFromGroup, addDomainsToGroup]
---

# Provision Elementum users and groups

Manage organization identity: users, groups, membership, and group email domains.

## Prerequisites
- A Bearer token from `requestAccessToken` with a service account that has user/group admin permissions.

## Steps
1. **List / create users** — `listUsers`: `GET /users`; `createUser`: `POST /users`.
2. **Read / deactivate a user** — `getUser`: `GET /users/{userId}`; `deactivateUser`: `POST /users/{userId}/deactivate`.
3. **List / create groups** — `listGroups`: `GET /groups`; `createGroup`: `POST /groups`.
4. **Manage membership** — `addUsersToGroup`: `POST /groups/{groupId}/users`; `listGroupUsers`: `GET /groups/{groupId}/users`; `removeUsersFromGroup`: `DELETE /groups/{groupId}/users`.
5. **Scope by email domain** — `addDomainsToGroup`: `POST /groups/{groupId}/domains`.

## Rules
- Groups carry roles, data access, notifications, and approvals — assign users to groups rather than granting access per user.
- A 403 indicates the calling service account lacks the required admin role.
- Handle 429 with backoff; paginate list endpoints with Relay cursors.
