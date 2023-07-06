---
title: "tinybase"
priority: "p3"
---

#project

- Needs to only ingest synced row on sync
- Needs to only saved modified rows on persist

- Loads just the changed row(s) after sync events?
- Saves just the changed state to DB on interval?
- Paging into memory:
	- Specify a query to load? Regular OO model but with queries specified for connections?
	- TinyBase query system that maps to SQL queries if needed?