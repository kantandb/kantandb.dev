# Page metadata

Title: KantanDB

Description: A small JSON document database.

# Site header

## Brand

[KantanDB](./)

Accessible label: KantanDB home

## Main navigation

- [Database design](#design)
- [Go implementation](./go.html)
- [Source ↗](https://github.com/kantandb/kantandb.dev)

## KantanDB

A small JSON document database.

# Main article: Database design

Anchor: `design`

These rules apply to every implementation.

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

The Go version is a single-process HTTP service built with net/http, storage on top of Pebble, and AES-256-GCM encryption.

[Implementation notes →](./go.html)

## Rust

Status: Planned

Work on Rust port will start soon.

# Footer

KantanDB

Source and notes on [GitHub](https://github.com/kantandb/kantandb.dev).
