---
title: "inverted database"
---

#project

The reactivity problem is a matter of answering the question:
> For a given write, what queries are invalidated by that write?

In as fine a grained way as possible. I.e., only invalidating the exact queries that dependended on the data written rather than invalidating a large blast radius.

One step to solving this is noticing that queries specify ranges:

`SELECT * FROM foo WHERE x > ? AND x < ?`

And points:

`SELECT * FROM foo WHERE x = ?`

and that we can "index the queries" themselves by indexing the ranges and/or points they are requesting.

An R-Tree is ideal for this. We could use SQLite in-memory and it's R-Tree to index our queries if the [[wasm tax]] isn't too high.

# Joins

# References
- https://github.com/ccorcos/tuple-database#Reactivity
- https://github.com/wotbrew/relic#reactive-programming