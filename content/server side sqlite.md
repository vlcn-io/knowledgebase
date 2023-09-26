
> I'd be curious to hear how your experience with Turso & D1 is. Turso looks interesting since it's a fully managed solution but IIRC it doesn't really do interactive transactions. It also seems like it suffers from the N+1 query issue since the application and data aren't colocated. I looked at D1 briefly when they first came out but I haven't taken a lot since they did their rewrite.
> 
> Let me know if you have any other questions or if I can help out at all.

I should turn this into a blog post but if you're willing to bear with me...
# Turso

Turso has the simplest experience in terms of being "push-button" ready.

The big drawback (which you allude to), however, is that Turso is setting up SQLite much like a traditional DB. You interact with it over HTTP rather than it being embedded in-process, throwing out many of the advantages of SQLite. Turso is working on "embedded replicas" which'll let you run SQLite in-process again. I believe a preview of this just landed. This should also fix the interactive transaction story.

The other drawback to Turso is that you can't programmatically spin up new DBs. E.g., if you do  want to create a new DB each time a user is registered or does something. That's currently impossible but also something they're working on fixing.

A big bonus of Turso, besides being simple, is that they're improving the write concurrency story of SQLite. I think based on Hekaton?

The last drawback of Turso is config & extensions. You can't just treat it exactly like SQLite and load whatever extensions you want. If you need extensions added, they currently must be on the turso allow list and built into their release. Probably doesn't matter to most people.

Small drawback -- Turso requires you to use custom bindings rather than any old SQLite bindings you like.

# D1

I'm not a fan of D1 at all. I've only had it described to me by one of their VPs but my understanding is:

1. DB instances are currently tied to a single "durable object"
2. DBs cannot be shared between durable objects
3. You can't use whatever SQLite bindings you want and you have to use their custom wrapper APIs
4. Extensions can't be loaded into D1. Loading a SQLite extension requires adding it to the global D1 build for all D1 users.

I can't come up with any positives for D1... It feels like a product solving all the wrong problems to me.

# LiteFS & Fly

LiteFS is my personal favorite.

Pros:
1. Can use any SQLite bindings and (I think?) any SQLite3 version 
2. Can load arbitrary extensions
3. DB is embedded into the application process as expected
4. Read replicas are simple to set up


The things preventing fly from knocking it out of the park:
1. Getting set up with LiteFS is a lot of configuration work rather than being as simple as Turso.
2. Write forwarding is not transparent and, if you can't use the litefs proxy, takes some effort to get working. Ideally I could use my DB as normal and write transactions would automatically forward.

An unknown is if SQLite's write concurrency is good enough for use on the server side and LiteFS makes no improvements here.


