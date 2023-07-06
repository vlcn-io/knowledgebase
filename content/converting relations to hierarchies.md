---
title: "converting relations to hierarchies"
priority: "p1"
---

#blog

Applications generally need their data in a hierarchical format. E.g., a UI is a tree where data is passed down the tree to hydrate components.

Relation are of course great -- they let us view the data in any tree like structure we want whereas if we start with a tree we're stuck in that singular format.

We just need a better way to turn relations into trees. SQL is great in that it maps from relations to relations so we can continue using the same computation primitives against query results as we use against base tables but it was not made to pull a hierarchy.

# Options
- SQL messaging via JSON group bys:  https://twitter.com/schickling/status/1599076832107630594
- Drizzle [relation queries](https://orm.drizzle.team/docs/rqb)
- `useQuery` at each component to pull the inherently flat data at a given level of the tree
	- This leads to waterfalling and flickering problems in the case where the storage API is async. E.g., your pokemon game example