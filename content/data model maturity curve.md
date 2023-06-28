Data models in UI applications go through a maturity curve.

1. They start as fully in-memory with some lazy persist of the entire blob over the network
2. Fully in-memory with the blob split up into chunks that can go over the network
	1. These chunks are specific to the application data format
3. Fully in-memory, chunked networking
4. Fully in-memory with some lazy local persist to disk + lazy persist over network
	1. This is to facilitate faster initial startup
5. 