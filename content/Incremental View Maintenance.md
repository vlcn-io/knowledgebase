---
title: "Incremental View Maintenance"
priority: "p1"
---

#project

Most apps are read, not write, heavy. Incremental view maintenance lets us shift work to deal with this reality. We can do _more work_ on write in order to _save time_ on read.

The Noria project shows it best:

![[/attachments/Pasted image 20230628100342.png]]
> Noria automatically keeps cached results up-to-date as the underlying data, stored in persistent _base tables_, change. Noria uses partially-stateful data-flow to reduce memory overhead, and supports dynamic, runtime data-flow and query change.

# Implementing
- First we need to map writes to impacted queries / views via something like an [[inverted database]].
- Potentially we would do [[differential data flow]] against the new write for the given query / view