How might we implement row level security for the CRDT database?

The big issue is that when privacy changes it can cause a bunch of new data to be visible that never had a write made to it. Given our [[incrementalism]] is based on "stuff being written", we need to somehow identify that this new data (which didn't receive a write) is available for the user. ^d9509b

Think of it like this:
![[user promoted to god example]]

My latest thinking is around using [[bloom filters for row level security]] + [[client defined query based sync]].

[[why not encryption for permissions]]