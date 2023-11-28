As you know I maintain an extension that allows multi-writer SQLite. The approach I've taken is one that is pretty complicated and requires all nodes to run the same algorithm.

There are other, simpler, approaches which (in a client-server setup) allow the server to be mostly agnostic to how conflict resolutions happens. In these approaches the user can use anything they'd like on the backend (postgres, sqlite, mongo, etc.).

One of those simpler models is the idea of "rebasing" changes on a client.

At a high level:

1. The client maintains a log of mutations that it has made locally
2. The client writes local database modifications to a separate file, similar to a WAL

When the server sends data to the client, the client throws away the file from (2), effectively re-setting its database state to what it was before the client made any local changes.

Then the client applies the changes sent by the server to its state, modifying the actual DB file.

Finally, the client re-plays the mutation log from (1) to get to the new server + local state. If any client-side mutations were incorporated in the update sent by server, the client starts at that position in the log rather than the start of the log.

---

I'm thinking it should be possible to write a VFS that re-directs xRead and xWrite requests. Such that all local writes go to a custom file rather than the main db file. xRead would be updated to read from the local write file if the requested range was updated locally, from the normal file if not.


![image](https://github.com/rhashimoto/wa-sqlite/assets/1009003/b9b3ac0e-ee19-4211-90fa-2735ac4e9d81)



Does hooking in at the VFS layer sound at all reasonable to you to implement this? I've never ventured into VFS territory so I don't know what gotchas may be lurking there. My assumption is that `xRead` and `xWrite` are always reading and writing full pages.