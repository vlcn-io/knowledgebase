90s: User, data, compute colocated
Cloud: data & compute colocated. User separated.
Map-Reduce: Data distributed, compute moved to data, user separated.
"Edge": Data moved to compute. Data could be moved to user.

[[Beyond CRDTs, The Next Frontier of Local-First]]

The next local first question:
How do I get my data where I need it, when I need it?

Cloud answered this:
Re-query the central service each time. Queries driven by current view of the user. Anything already on device is just a cache.

Current work to improve the answer:
Pre-fetch views the user may go to. Still full queries, still treat responses as cache.
Move more data to servers near user for lower latency

Local-First:
Can we take the cloud approach?
- Your dataset is more expansive than your current view.
- In LoFi, On-device is not just a cache. Could re-query all then merge.
- Can we do incrementally?