# KantanDB

KantanDB is a small learning project where I can build a document database and get more comfortable with Rust.

The current prototype is a single-process HTTP service for durable named databases and JSON documents. It assigns UUIDv7 document IDs, tracks revisions with ETags, and supports document creation, reads, replacement, deletion, JSON Merge Patch, and JSON Patch. Conditional writes use `If-Match` to reject stale updates.

A database can define secondary indexes at creation time using JSON Pointer paths. Indexes are updated atomically with their documents. Queries support equality over JSON scalar values and ordered comparisons over numbers and strings. Database lists, document lists, and index queries are paginated; indexed queries use opaque cursors tied to the original query.

Documents, database metadata, index definitions, and index entries share a local ordered key-value store. Synchronous batches keep related changes atomic and durable, while striped locks coordinate concurrent writes and database deletion. The HTTP layer enforces request-size limits, validates names and documents, returns consistent JSON errors, and shuts down gracefully.

This is an experiment rather than a production database.
