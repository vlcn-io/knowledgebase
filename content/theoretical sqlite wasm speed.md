---
title: "theoretical sqlite wasm speed"
---

https://gist.github.com/tantaman/28f1fc4cb7ccc0012bf3bd8ccca76eed

- [[tables used]] is deathly slow.
- preparing queries is slow.
- thread

TODO:
- with OPFS enabled -- how long those same tests take
- with Roy's lock_mode_exclusive + shared access handle approach
	- can we WAL mode this?
- 
