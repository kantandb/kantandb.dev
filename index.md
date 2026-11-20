# Page metadata

Title: KantanDB

Description: A small JSON document database.

# Site header

## Brand

[KantanDB](./)

Accessible label: KantanDB home

## Main navigation

- [Database design](#design) (current page)
- [Go implementation](./go.html)

## KantanDB

A small JSON document database.

# Main article: Database design

Anchor: `design`

## Databases and documents

KantanDB uses JSON objects as documents and groups documents into named databases.

## Indices and queries

KantanDB uses `jq` as a query language and can define indices over often used fields. When index is not awailable the queryplanner fails back to full scan.

## Persistence

A successful write survives a restart and a document changes are hapening atomically with indices updates.

## Encryption

Document bodies are compressed and encrypted before they reach storage.

# Implementations

## Go

Status: Current

The Go version uses net/http to serve requests, Pebble for storage, and AES-256-GCM to encrypt documents. Supported output formats are JSON and stream NDJSON.

## Rust

Status: Planned

Work on Rust port will start soon.

# Footer

KantanDB

Source on [GitHub](https://github.com/kantandb/kantandb.dev)
