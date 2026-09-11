# KantanDB

KantanDB is an educational JSON document database implemented in multiple languages.

## Motivation

I want to build the same small database more than once. The Go version comes first, followed by a Rust port and perhaps versions in other languages. Keeping the behavior fixed should make the differences between the implementations easier to see and turn each port into a practical way to learn a language.

KantanDB is an experiment, not a production database.

## Database design

KantanDB groups JSON documents into named databases. The server assigns each document an immutable ID and tracks its current revision. Clients can create, read, replace, patch, and delete documents, with optional revision checks to prevent stale writes.

A database can define indexes over document fields. Queries support equality and ordered comparisons, while JSONPath allows queries beyond declared indexes. Results are paginated with cursors tied to the original query. Resource limits keep requests and query work bounded.

Documents and their index entries change together and persist across restarts. Document bodies are protected at rest, although names, IDs, indexed values, and record sizes remain visible.

## Go implementation

The current [Go prototype](https://github.com/kantandb/prototype) is a single-process HTTP service backed by Pebble. It uses UUIDv7 document IDs, ETags for revisions, synchronous batches for atomic durable writes, and striped locks to coordinate concurrent mutations and database deletion.

Document JSON is compressed with Zstandard and encrypted with AES-256-GCM before reaching Pebble. A required master key wraps a separate key for each database; derived keys protect documents and authenticated query cursors.

Declared indexes use JSON Pointer paths. Indexed queries scan ordered Pebble keys, while `QUERY /{database}` accepts RFC 9535 JSONPath. Simple paths use a matching index when possible; other paths scan encrypted documents in ID order. Request size, path complexity, scan work, document evaluation, and execution time are bounded.

## Rust implementation

TBD. Work starts after the Go design settles.
