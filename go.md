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

## Go

The Go server implementation on top of Pebble.

# Main article: Implementation details

## HTTP interface

The service creates named databases and accepts document CRUD requests. It generates UUIDv7 document IDs and exposes revisions as ETags. Replacement, patch, and deletion can use `If-Match` to reject a stale revision.

Patches support JSON Merge Patch and JSON Patch. Routes use Go's `net/http` package and live under `/db`.

See the [OpenAPI 3.2 specs](https://github.com/kantandb/server-go/blob/main/openapi.yaml) for the complete interface.

## Storage

Pebble holds every logical database in one ordered local store. Documents, database metadata, index definitions, and index entries occupy separate key ranges. Synchronous batches update a document and its indexes atomically and durably.

Concurrent document writes coordinated by striped locks.

## Encryption

The server requires a base64-encoded 256-bit master key in a file outside the data directory. It derives a store wrapping key and uses it to protect a random key for each database.

Document JSON is compressed with Zstandard, then encrypted and authenticated with AES-256-GCM. Per-document keys are derived with HKDF-SHA256. Cursor keys are derived separately.

## Indices

Indices are declared when a database is created. Each one maps a JSON Pointer path to ordered Pebble keys containing the encoded field value and document ID.

## jq queries

`QUERY /db/{database}` accepts a raw jq queries as plain text. The server uses gojq to filter and transform documents from one Pebble snapshot.

The default response is a JSON array with `application/x-ndjson` switching to NDJSON streams.

`Limit` and `Cursor` headers allows for pagination.

## Bulk transfer

Export done with '.' over `QUERY /db/{database}` and `Accept: application/x-ndjson` for streams.

Same stream can be imported with `POST /db/{database}` and `Content-Type: application/x-ndjson`, but wrapper IDs and revisions are disarded and import assigns new UUIDv7 IDs. Raw NSJSON without IDs and revision wrappers are suppoted.

# Footer

KantanDB

Source on [GitHub](https://github.com/kantandb/kantandb.dev)
