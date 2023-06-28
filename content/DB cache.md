---
title: "DB cache"
---

The JS integration currently makes use of a DB Cache. This cache only serves to collapse duplicate queries into a single query. Any other data caching is done within the reactivity layer (currently the `useQuery` hook. see [[react integration]])