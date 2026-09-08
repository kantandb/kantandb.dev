# KantanDB

KantanDB is a small learning project where I can build a document database and get more comfortable with Rust.

The first version will provide durable CRUD for JSON objects over HTTP. It will support named databases, server-generated UUIDv7 document IDs, revisions through ETags, JSON Merge Patch, and JSON Patch. SlateDB will handle storage using the local filesystem.

Once the basic database works, I plan to add equality and range queries over JSON fields. DuckDB will maintain a rebuildable index cache, while SlateDB remains the source of truth.

This is an experiment rather than a production database.
