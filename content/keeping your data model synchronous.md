---
title: "keeping your data model synchronous"
---

A problem that is introduced once an app moves from having all data in-memory to having to page some data to disk is that all access to that data suddenly becomes async. The app's entire data model, even if there is an ORM'ed version of it, would likely be completely infected with async as we can't always know when we'll need to wait and go to disk.

[[fully synchronous reactivity]]
[[data model maturity curve]]