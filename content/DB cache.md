---
title: "DB cache"
priority: "p1"
---

#project

The JS integration currently makes use of a DB Cache. This cache only serves to collapse duplicate queries, made in the same event loop tick, into a single query. Any other data caching is done within the reactivity layer (currently the `useQuery` hook. see [[react integration]])

We could be smarter and cache here indefinitely / until invalidate. Why do this? Well because many components might be issuing the exact same query and the `useQuery` cache is not shared between components.

A smarter DB Cache also can be a resting place for [[fully synchronous reactivity]].