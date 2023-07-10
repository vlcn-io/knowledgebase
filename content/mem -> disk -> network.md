---
title: "mem -> disk -> network"
---

We're issuing SQL.

There's three levels of data availability:
- Memory
- Disk
- Network

One way to approach this is to try to make it all transparent to the dev:
```
- issue a synchronous query
- get empty result back
- subscription then keeps them up to date with whatever flows in from disk or network
```

Of course we have transition problems here if we want to wait for data or be aware of loading states.

> Do we ever know if we have the complete / final set of data for a query? We could know of some finality with respect to a delta with the server... "final as of last sync" or some such. Finality with respect to when _that specific query_ was synced... if we do client defined query sync.

```ts
const x = useQuery(...);
```

Your `useQuery` hook already exposes these states! Excluding network! Via the return value:

```ts
return {
  data: <-- memory resident data,
  loading: <-- loading status
}
```

We can extend the loading status to indicate network & disk and with what finality the network is. If the subscription is hydrated we often never need to hit disk.