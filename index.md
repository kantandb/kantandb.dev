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

KantanDB groups JSON documents into named databases. A document is always a JSON object.

## Indexes and queries

A database may define indexes over fields used often in queries. JSONPath handles queries that do not fit a declared index.

## Persistence

A successful write survives a restart. A document and its index entries change together, so a query does not see half of an update.

## Encryption

Document bodies are encrypted before they reach storage.

# Implementations

## Go

Status: Current

The Go version uses net/http to serve requests, Pebble for storage, and AES-256-GCM to encrypt documents.

## Rust

Status: Planned

Work on Rust port will start soon.

# Footer

KantanDB

Source on [GitHub](https://github.com/kantandb/kantandb.dev)
