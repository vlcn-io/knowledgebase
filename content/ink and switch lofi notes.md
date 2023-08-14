---
title: "ink and switch lofi notes"
---

https://www.inkandswitch.com/local-first/

> In this article we propose “local-first software”: a set of principles for software that enables both collaboration _and_ ownership for users

> “local-first software”: a set of principles for software that enables both collaboration _and_ ownership for users. Local-first ideals include the ability to work offline and collaborate across multiple devices, while also improving the security, privacy, long-term preservation, and user control of data.



- Software, in the 90s and early 2000's, was local.
- Local-first is _strictly harder_ / software on hard-mode
	- Harder then 90s local
	- Harder than cloud
- 90s concept of local just can't work today
	- Self Sync
	- Collab
	- Different device constraints but same app on all


We've been surrounded by "local" software
- Mail client
- File explorer
- Orig iTunes
- Orig Office

And the apple ecosystem:
- iPhoto
- iTunes of today?

How did they handle self sync and collab? Through the unit of files.

Does the file unit make sense in today's world?
- Issue tracking system. No. Each issue a file? Each comment? Private comments and threads?
- iPhoto -- I don't have to deal with file management. I.e., what to place on cloud vs on device. iPhoto does it.

The migration to the cloud made sense. It was an exodus from the "final-final" copies of docs. The use driven management of "where to store a thing."

But with the migration to the cloud we lost:
- Offline capability
- Data ownership / preservation

Local-first is all the benefits of the cloud:
- Available on any device (if network)
- Not thinking about storage (remember diskmon?)
- Sharing at fine grained levels, not just at the file

we started local. We got devices, we got networked. We saw the problems (final-final, disk full, where to put a thing, can only share at file level... JIRA over excel? hmm). Exodus to cloud made sense. Now we see the problems of the cloud:
- Where'd my data go?
- Where'd the product go?
- What's the storage format? How can I exit? Export to other apps?
- My data isn't my data. It is behind a wall.
- Innovation is stifled
- No internet no go? Wut?
- Latency :|


Thought experiment: JIRA over Excel file?
That excel file needs:
- Row level security
- Sync
- Merging
- Finding peers (or facilitated by server)
- Access control
- Auth

Much is said about "end user programming" and how "excel enables this" but could we ever build a cloud like app out of excel? One with sharing and collaboration and permissions and such?

But how do we go local and keep the advantages of the cloud?
First, what does the cloud give us:
- Fine grained (row level, column level) access control
- No thinking about storage
- Single repository of all information, no final-final
- Collaboration on the same item(s)
- Auth

Local-first software runs even if the service shuts down. What does this mean?
- Copies of data on own device(s)
- Software runs w/o network on available local set
- Sync protocol so we can get new sync providers?
	- but what if there is a required cloud component of server authoritative data?
	- can this be encoded in the protocol? Service logic is made available for new services to spin up when original spins down?

We can rabbit-hole on storage first.

