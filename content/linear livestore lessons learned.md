---
title: linear livestore lessons learned
---
- Static query definitions are nice for:
	1. Single preparation of the query
	2. Re-use across components
	3. For [[materialite]] for sharing part of a data stream and forking the query
1. `useComponentState` is an interesting alternative to ORMs and [[post facto relational]]
	1. It is a functional way of managing state without a domain model
	2. It hides sql-ness from devs
	3. Does turn things into very much a ui driven key-value store
2. Given most reactivity control is taken away from react via computed signals
	1. We can ensure all state updates are made and committed before re-rendering
	2. No partial state updates on renderw

How might we fix the Materialite filter update ergonomics to be a derived or computed signal?
How might we share identical parts of a query stream?
	Static queries.. maybe. Still need bind params to modify the query.
	`useQuery` hook looks for structural sharing among active queries? Pipes them off those sharing points? Cleanup becomes a bitch.