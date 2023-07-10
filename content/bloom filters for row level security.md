Bloom filters don't so much implement the security mechanism as they enable tracking dirty bits so we can still incrementally send updates after privacy invalidations rather than requiring full re-pulls.

![[row level security#^d9509b]]

So how would a bloom filter better integrate row level security and [[incrementalism]]?

The idea would be that each time a permission rule denies a read we'd record the primary keys of the denied row in a bloom filter.

> hm.. is this a bloom filter _per client_ ? :/ Is that tenable?

When permissions change, exposing a new set of available data, we'd pass the primary keys of this data through the bloom filter. All keys possibly in the set of denied reads would be enqueued to be sent to the client.

> ok, but the question is: how do we efficiently compute the set of newly available data?

I think we need to pair with query based sync and understanding the "active set" of queries the client wants to sync (i.e., [[client defined query based sync]]). Looking back at the [[user promoted to god example]], we wouldn't ever want to sync that user _all data on the site_.