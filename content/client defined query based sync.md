---
title: "client defined query based sync"
---

How much data does the client device need?

- Whole database replication may be untenable for situations where the user's devices have different storage profiles
- Whole database replication is a non-starter for traditional application architectures and all data for all users in the same db
- Whole db replication doesn't make sense when sharing is not at the DB level

Another issue is what data to prioritize for sync / fetch and when. Blindly syncing the DB without input from the app dev may lead to a bad experience.

Think about [[user promoted to god example]]. We don't want to block all other syncs as the entire set of what they now have access to streams in.

So what is client defined query based sync? And why is it possible harder or different in a local-first architecture?

[[blurring the client-server boundary might be a mistake]]

# What it is

Traditional client-server apps will have the client request some set of data from the server for each view. E.g., either via a Rest endpoint for the view or GraphQL.

This ensures each view only fetches what it needs.

We can take the same approach with the local-first model but maybe additionally supply data to continue loading into the background.

The issue with these client defined queries and our current model, however, is as follows:
1. [[incrementalism]]. We'd need to know how to incrementally update these queries as the underlying data changes.
2. [[row level security]]. We'd need to know how to update these queries as the set of visible data to the user changes
3. Merge metadata. The results of these queries, when querying CRRs, need to include merge metadata.
4. Mixing models. Ideally the user can mix strongly and eventually consistent data.

If we re-cast everything in this model... we no longer just wire dbs directly together over the sync protocol.

# Getting Started

What is the simplest way to start supporting this model?

- We'd need the inverted DB to support subscriptions
- Each `useQuery` can set up a subscription with the backend
	- Of course multiplexed over a single connection
- LiteFS issue... we just know of DB change not what changed. You could get "what" via a db_version query and map that what to subscribers.
- how do we resume a userQuery? Need to know the db_version it last left off at + user's bloom filter of restricted reads.
	- Run the query only grabbing rows past the given db_version?
		- but need to get "now visible due to privacy" rows which could be in the past
	- Run the full query, returning all rows?
	- Run the full query
		- Return rows that are > db_version
		- Return rows that are < db_version but possibly in the bloom filter

Devs should get control over how this process works.

Synchronous queries that resolve immediately based on hydrated subscriptions followed by data from disk followed by data from network...

Or should we just make everything explicit to the dev instead of trying to hide it?

[[mem -> disk -> network]]

Queries to DB then with eventual data over the network is a new model. E.g., `SELECT * FROM foo JOIN bar`. Maybe no data exists locally for `bar` yet.



This is the root of our problem and a problem traditional apps easily solve by forcing the client to specify queries to be made of the server.


---
scratch:
A confluence of problems make this architecture a requirement.

1. Incrementally fetching changed data
2. Row level security and incrementally returning what the user can now see
	1. the [[user promoted to god example]]
3. - 