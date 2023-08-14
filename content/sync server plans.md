---
title: "sync server plans"
---

Starting from our websocket sync server -- https://github.com/vlcn-io/cr-sqlite/pull/316

- Create native inbound and outbound stream abstractions as documented in https://github.com/vlcn-io/cr-sqlite/issues/38
- create server + rest api for strongly consistent stuff
	- expose rest API as vtab?
	- rest api 