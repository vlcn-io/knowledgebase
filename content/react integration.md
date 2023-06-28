---
title: "react integration"
---

cr-sqlite is integrated to React via `useQuery` hooks. [docs](https://vlcn.io/docs/js/react)

# Main considerations:
- opening IndexedDB transactions is expensive. Hence we must gather all `useQuery` requests in a given render pass and issue them in a single transaction
- `useQuery` must be reactive. We use the `tables_used` vtab to extract tables hit by a query to enable re-running them. We could get smarter and do an [[inverted database]] and even [[incremental updates to cached queries]] to enable [[fully synchronous reactivity]].
- [[waterfalling & flickering]]
- The [[wasm tax]] looks pretty high. Maybe it is better to gather small queries into a large query rather than doing many tiny queries. This could change when [[Moving to OPFS]] as in that situation we're going over a worker boundary but we can batch returns into the same tick.

# Other Improvements
- Have the [[DB cache]] live longer than a single pass through the event loop? Maybe not given we cache in the reactive layer anyhow.

# Future
- [[Moving to OPFS]] will require a re-work of the react integration. We can presumably batch all returned data into the same event loop tick in this situation, however, which may actually be a beneift

# Structured Data
- Drizzle allows us to pull [[converting relations to hierarchies|hierarchical data]] rather easily: https://orm.drizzle.team/docs/rqb
	- We could use that to
		- Enable larger queries to deal with the [[wasm tax]]
		- Enable the GraphQL like vision https://twitter.com/schickling/status/1599076832107630594
		- Bring the [vanilla fetch](https://github.com/tantaman/vanilla-fetch) idea to Vulcan