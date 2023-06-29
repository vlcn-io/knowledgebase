---
title: "product vision"
---

- Client
	- crdts for local writes
		- but we still need server overrides
		- and strong consistency
		- and "optimistic only" tables / rows
		- What about CRDT fields that should be "set together"
		- JSON story (property table)
	- Reactivity
	- Functional Relational Programming
	- Rich text
	- [[converting relations to hierarchies|hierarchical fetch]]
- Multiple consistency models
	- CRDT / Causal
	- Strongly consistent, read-only, mirrored tables
	- Mirrored tables which allow client-side optimistic writes
	- Strongly consistent, read/write, mirrored tables
- Storage
	- What if the entire DB can't fit on device? How much do we put there? [Tiered storage?](https://rocksdb.org/blog/2022/11/09/time-aware-tiered-storage.html)
- Backend
	- multi-tenancy
	- subscriptions
	- row level security
	- sync a subset of the database
- Network
	- Client-server mainly
		- Server overrides
		- Server permissions / rejections
	- P2P never precluded by CRDT design decisions

# References
- https://riffle.systems/
- https://github.com/papers-we-love/papers-we-love/blob/main/design/out-of-the-tar-pit.pdf
- 