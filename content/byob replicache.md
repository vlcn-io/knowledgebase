---
title: byob replicache
---
# CVR Thoughts
- We can do incremental via `create sequence` to use for `rowversion` and paginate on row version
	- Cons:
		- privacy invalidations and query changes become hard
			- These are hard because the `rowversion` may not change on things that are now visible. e.g., `SELECT * FROM todo JOIN filter ON filter.id = uid WHERE filter.status = todo.status`
				- Changing a filter changes what is returned by the query without changing the query data and rowversions
				- Thus our incremental strategy will never pick it up since it filters on `rowversion > x`
- We can allow the frontend to control incrementalism
	- This means that the CVR becomes an ever-growing list of IDs.
	- IDs of all the things sent from all incremental queries combined.
	- Cons:
		- How do we detect a delete?
			- We can use a tombstone
			- We can scan _the entire cvr_ to find no longer present ids
	- Pros:
		- Queries can change and still be incremental by checking cvr / sent set
		- Privacy can change and still be incremental
- Frontend control incrementalism w/o an ever growing id list?
	- Still no worko. We'd have to check the entire history of CVRs to find a delete not just the most recent prior cvr.

# Cookie Thoughts
- Super annoying to have to set a cookie and out-of-band sync state
- Should be able to use the data returned from `pull` and attach that to the next args to `pull`

# Poke
- IDs in Poke to adjust next call to pull to fetch those ids...
- Getting updated data due to a mutation isn't the easiest thing in the world
	- Mutations must return impacted IDs
	- Those are poked thru
	- Then we re-pull with (hopefully) params related to that poke to limit ourselves to the required data


Initialization pull paginated on row-version?
But how would we ever re-initialize in the face of privacy changes?
It can request a full re-init that is filtered by CVR? "full pull"

So pull types:
1. Incremental initialization, ordered by row version
2. Full-pull that diffs against CVR
	1. This handles query changes or privacy changes
3. Micro-pull that is in response to a poke
4. Full nuking pull that ignores CVR too?

# Pull
- Let me pass args to pull not thru query params or cookie

Why do I need to load the prior CVR? Why not just load the full set of sent items?
It is prior and the DB should have my data since I put it...

Prior CVR for--
- many tabs syncing?
- network hiccup <-- this.
	- If we record all as being groovy then we'll think we sent to the client when we did not and future sends won't work
	- Now deletions... this is a problem


> recombining would certainly make things a bit simpler.

> but what page of the incremental sync they are currently on

Page is a bit of a fluid target. Two pages may be nearly identical (order by created DESC vs order by modified DESC) so it seems to me you'd want to share CVR information such that the second query doesn't re-sync things already sent by the first.

# Rails
- Custom key format? E.g., comments.

# Locks
- sync lock? Write-free pull?

# Comment Sync
- rowversion seq for comment
- fetch by rowversion in ever increasing order

Interval pull

# Problems of Client Driven Fetch for Repliear
- Need to indicate how much, for a given query, to over-fetch
	- So scroll experience is good
- Need to merge in all past CVRs, even for different queries, so we have the complete picture of what exists on the client
	- Can do with current approach
	- Can do with recursive query
- Filter change events can be problematic since it requires hitting server too
	- for things not yet synced. And flash of `0` to `n` results to `n + y` as more pages come in


CVR PG:
- Issue sync is hella slow at the start, slowly speeds up...
- Each table is slow at start until the full table is synced? B/c less that is synced the more that needs to be churned?
	- Can throw in your cursor trick + a final full pull?
	- Comment sync slows down the more that is synced?

Do initial seeding with `rowversion` global stuff...
Then later seeding with no cursors?


Next up:
3. test modifying shiznit
4. Query split across client and server
5. frontend railsified / updated for new key prefixes?
6. speed up queries?

- Poke channels
- Rails on frontend
- Demo video 4 million w/ sorted source, after and limit

---
- seq, rowversion
	- cursor for initial sync
- client id, query -> pull for client
- recursive query conversion

- poke
- push

- Replace mutators
- rails comment put... is it broken given our key requirements? nonstandard key?
- Store cvr in db so db reset nukes it
	- cvr_meta: order, client_version, client_group_id?
		- pk on `client_group_id` and `order`?

comments:
http://localhost:5173/?iss=11734-0
https://repliear.herokuapp.com/d/Hc0uXUhTnH?iss=15074-0

- use driven query..

Do we need to serialize on client group?

Pull is mostly read only except for CVR calculation.

1. Pull comes in
2. We read client group
3. We pull data

We do the lock to keep cvr version monotonic? We can also solve client group problems by getting tabs to coordinate sync. E.g., a web lock.

Delete culling questions.

---
SELECT * FROM cvr_entry WHERE tbl = 3 AND client_group_id = '1a1134ff-7995-41d7-8748-d5fc58beca83';

SELECT * FROM cvr_entry WHERE tbl = 3 AND client_group_id = '1a1134ff-7995-41d7-8748-d5fc58beca83';


What next:
1. Sync Query driven by UI
	1. query name and params sent over from client synced thru replicache
		1. client group id as a part of this?
2. Remove locking on backend? The only thing moving is the cvr version...
	1. Can this not be an auto-increment col on our cvr table?

# Overview
- Start with CVR and that we can use CVR history
- Rails
	- Can't use it for comments
		- because key prefix
- Mutations
	- 2 args, second arg doens't go to backend
- Pull
	- Serialization and lock errors
	- Multiple client groups pulling? Why not coordinate with a lock?

# Feedback more
- Indices for comments thing
- Range removal
- Comment index ``

# Today
- Simplify repliear as much as possible
- Add current materialite changes
- Put materialite in rails
- Rails with dynamic key prefix?
- Feedback notes / discussions
- Diagrams in discussions