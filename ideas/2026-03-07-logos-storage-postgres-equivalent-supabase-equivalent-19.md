---
id: 19
title: "Logos Storage → Postgres-equivalent → Supabase-equivalent"
status: approved
author: "deca12x"
created: "2026-03-07"
source_issue: 19
---

# Logos Storage → Postgres-equivalent → Supabase-equivalent

## Description
TLDR: make Logos usable as an app backend, not only as a storage substrate.

Today Logos Storage is low-level: great for storing blobs/files, but not enough for most app builders. Most teams need a higher abstraction layer: structured data, queries, indexes, auth-aware access, APIs, and developer tooling.

This would let developers use Logos for normal product building, not just for storage primitives.

Core features could include:
- tables / collections / typed schemas
- CRUD operations
- indexes
- query language
- REST and/or GraphQL API generation
- auth and permissions
- realtime subscriptions
- admin dashboard
- SDKs
- migrations
- local dev emulator

## Social Problem & Logos Stack Solution
Any B2C company needs to keep track of users, user profiles, preferences, and connect those to Most useful software is not built from raw storage primitives.
Any B2C or B2B app needs to keep track of users, profiles, permissions, settings, application state, relationships between entities, and often analytics or event data. That requires structured data access, not just file storage.

Today, many builders default to centralized stacks because they are easy:
- Postgres for structured data
- Supabase/Firebase for backend services
- REST/GraphQL APIs
- auth and row-level permissions
- dashboards and SDKs

The social problem is that decentralized or alternative stacks often remain inaccessible to ordinary developers. If the Logos stack stays too low-level, only highly technical teams will use it. That sharply limits adoption.

In other words, the missing piece is not only infrastructure, but developer usability.
If Logos wants broad real-world adoption, developers need an abstraction layer that matches how modern apps are actually built.

## Previous Attempts & Hypotheses
1. Supabase / Firebase
These platforms define the modern developer baseline:
- managed database
- authentication
- API generation
- realtime
- dashboards
They succeed because they dramatically simplify backend development. However they are centralized infrastructure and do not provide data sovereignty or decentralization guarantees.

2. Polybase DB
Polybase is a decentralized database that exposes a developer interface similar to Firebase: collections, queries, and SDKs. Internally it uses blockchain verification and zk-proofs for integrity. The goal is to combine verifiable data with a familiar database abstraction. Adoption has remained limited, largely due to ecosystem maturity and operational complexity.

3. Ceramic + ComposeDB
Ceramic provides a decentralized data network based on append-only streams. ComposeDB adds a GraphQL database layer on top of Ceramic so developers can define schemas and query structured data. The system enables composable shared data across applications, but requires running indexing nodes and understanding the underlying stream model, which raises the complexity for typical app developers.

## Functional Specifications
The system should provide a **structured data layer and developer interface on top of Logos Storage**.

## Data Model
Developers must be able to define structured schemas.
Example entities:
```
User
Profile
Post
Message
Event
```

Requirements:
* typed fields
* unique identifiers
* relationships between entities
* schema versioning

The model may be relational, document-based, or hybrid, but must support structured queries.

---

## Query Layer
Applications must be able to retrieve data efficiently.
Minimum query capabilities:
* filtering
* sorting
* pagination
* lookups by indexed fields
* relationship traversal

Example queries:
```
get users where reputation > 100
get posts by user_id
get events near location
```

Indexes should be supported to make these queries efficient.

---

## API Layer
Applications should not need to directly interact with storage primitives.
The platform should automatically expose APIs such as:
```
GET /users
POST /posts
GET /posts?author=123
```
Supported interfaces:
* REST
* optionally GraphQL
---

## Identity and Access Control
Modern applications require user-level permissions.
The system should support:
* wallet-based identity
* email/password authentication
* OAuth providers
Access policies must allow:
* collection-level permissions
* record-level permissions

---

## Developer Experience

To compete with existing backend platforms, the system should include:

* web dashboard
* schema editor
* data explorer
* logs and metrics
* migrations

SDKs should be available for common languages (e.g. TypeScript).

## Risks & Deployment Strategy
## Key Risks

**1. Database semantics on top of storage**

Object storage systems are not naturally queryable databases.

Challenges include:

* indexing
* consistency
* efficient querying

The architecture must introduce a structured state layer that maps database records to storage objects.

---

**2. Query performance**

Without indexing infrastructure, query performance may be insufficient for typical applications.

Possible mitigation:

* dedicated indexing services
* query caches
* materialized views

---

**3. Complexity of decentralized systems**

Fully decentralized query execution can introduce latency and operational complexity.

A pragmatic architecture may separate concerns:

```
storage layer → decentralized
query/index layer → distributed services
API layer → developer interface
```

---

**4. Developer adoption**

The largest risk is usability.

If developers must understand Logos internals to build applications, adoption will remain limited.

Success should be measured by whether a developer can:

```
create project
define schema
deploy backend
query data
```