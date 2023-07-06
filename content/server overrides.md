---
title: "server overrides"
priority: "p0"
---

#project

"server overrides" or [[server authoritative]] data is the means to allow the server to overrule clients.

Users can implement this in user-space by processing the changeset log before applying it and throwing out changes they do not agree with. Of course the client passing bad changes will be diverged at this point.

So if they throw out changes, they must tell the client to roll back the given rows or apply a given state for those rows.

Roll-back:
1. Send the server versions of those rows that the client must take and apply
	1. Apply these outside the merge path? Or make a "override" path that applies them without bumping any versions? 
	2. Or bump the server versions where server did an override? dos problems?
	3. Server responds with "overrule" with cols and version to roll back to? And the client's db version that was overruled?