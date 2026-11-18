# Page metadata

Title: KantanDB: Go implementation

Description: Notes on the current Go implementation.

# Site header

## Brand

[KantanDB](./)

Accessible label: KantanDB home

## Main navigation

- [Database design](./)
- [Go implementation](./go.html) (current page)
- [Server source ↗](https://github.com/kantandb/server-go)

## Go

The Go server implements the current database design as one HTTP service backed by a local Pebble store.

[Browse the source →](https://github.com/kantandb/server-go)

# Main article

## 01 HTTP interface

The service creates named databases and accepts document CRUD requests. It generates UUIDv7 document IDs and exposes revisions as ETags. Replacement, patch, and deletion can use `If-Match` to reject a stale revision.

Patches support JSON Merge Patch and JSON Patch. Routes use Go's `net/http` package and live under `/db`.

The HTTP layer validates names and documents, limits body size, uses a consistent JSON error format. See the [OpenAPI 3.2 specs](https://github.com/kantandb/server-go/blob/main/openapi.yaml) for the complete interface.

## 02 Storage

Pebble holds every logical database in one ordered local store. Documents, database metadata, index definitions, and index entries occupy separate key ranges. Synchronous batches update a document and its indexes atomically and durably.

Concurrent document writes coordinated by striped locks.

## 03 Encryption

The server requires a base64-encoded 256-bit master key in a file outside the data directory. It derives a store wrapping key and uses it to protect a random key for each database.

Document JSON is compressed with Zstandard, then encrypted and authenticated with AES-256-GCM. Per-document keys are derived with HKDF-SHA256. Cursor keys are derived separately.

## 04 Indeces

Indeces are declared when a database is created. Each one maps a JSON Pointer path to ordered Pebble keys containing the encoded field value and document ID.

## 05 JSONPath queries

`QUERY /db/{database}` accepts RFC 9535 JSONPath in a JSON request body. A document matches when any scalar selected by the path satisfies the comparison.

A simple path uses a matching declared index when it is safe to do so. Other paths scan documents in ID order.

## 06 Bulk transfer

`GET /bulk/{database}` streams documents as NDJSON from a consistent Pebble snapshot. `POST /bulk/{database}` imports them with new UUIDv7 IDs and revisions, rebuilds the target database's index entries, and commits atomically.

Operators can limit transfer size, document count, Pebble batch size, duration, and concurrency.

# Page navigation

- [← Back to database design](./)
- [View source ↗](https://github.com/kantandb/server-go)

# Footer

KantanDB

Go server · [Apache 2.0](https://github.com/kantandb/server-go/blob/main/LICENSE)
