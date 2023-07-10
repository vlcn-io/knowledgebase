---
title: "Beyond CRDTs, the next frontier of local-first"
---
#presentation

CRDTs (or some other convergence mechanism) is only the first frontier.

What follows:
1. Permissions
3. Bottomless replication due to size constraints
4. Moving out of memory and async vs sync browser quirkiness
5. Multi-tenant DBs
6. Query based sync
7. Perf? The "functional relational" is a separate concern from local-first...

What follows re-cast:
	- Bottomless replication (change devices examples)
	- Auth
	- Sharing (variation on multi-tenancy of many users in 1 db)
	- Better RX?
	- Local networking?
	- App delivery? (dweb tomorrow example)
	- Intent?
	- what is network delayed vs what is immediate? Is the immediate data really complete? Mixing consistency models
	- Local-first may not actually simplify development at all. It just looks simple at the outset.

Some good thoughts in this vein are forming in [[row level security]], [[client defined query based sync]], [[incrementalism]], [[bloom filters for row level security]], [[inverted database]]