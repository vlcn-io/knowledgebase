Every write to the database receives a corresponding database version. The database version is a continuously increasing 64 bit int. It increase on every transaction. All writes in the same transaction receive the same db version.

- Inserting a row associates all columns in that row with the next db version
- Updating a column associates that column with the next db version
- Deleting a row records a sentinel for that row with the next db version

This mapping of mutations to database versions allows for efficient incremental updates. A node just asks another node for all changes since the db version it last received.

When met with [[row level security]], however, this solution to incrementalism is not enough.