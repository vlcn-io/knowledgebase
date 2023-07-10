---
title: "streaming reactivity"
---

From Jerome (https://discord.com/channels/989870439897653248/989870440585494530/1128026067819561084):

basically, to support _any_ queries (even very complex ones), we have to check if anything was upserted or deleted on every change. even if the change is "inserting a new row", it's possible a negative constraint in the query removes a row from the result set.

so what I'm doing is:
- modify the query to prepend all primary keys for all tables mentioned in the query, we also re-alias all selected columns like: `col_0`, `col_1`, ... (you'll see why later)
- store the query results in a temporary table via `INSERT INTO ... SELECT ...` (this is possibly problematic since it goes away with the connection and I'm using a connection pool)
- the temporary table has an extra column called `__corro_rowid` which is basically the position of the resulting row. it's an auto-incrementing integer
- we stream back the rows with their `__corro_rowid`, but not the extra primary keys for all tables we added as columns. effectively the client gets a stream of `{rowid: i64, cells: Vec<SqliteValue>}`
- now, when there's a change (any change), we have to prepare 2 queries, which is a bit of bummer...
- the first query is to detect any upsertions: 
```sql
INSERT INTO <temp-table> (__corro_pk_table1_pk1, ..., col_0, ...)
  SELECT * FROM (<original query w/ added where clause to only fetch rows related to the current change> EXCEPT SELECT ... FROM <templ-table>) WHERE 1
  ON CONFLICT (__corro_pk_table1_pk1, <list of primary key columns>)
    DO UPDATE SET
       col_0 = excluded.col_0, ...
RETURNING *
```
- the second query is to detect any deletes: 
```sql
DELETE FROM <temp-table>
  WHERE (__corro_pk_table1_pk1, <list of primary key columns>) IN (SELECT __corro_pk_table1_pk1, <list of primary key columns> FROM <temp-table> EXCEPT <original query w/ added where clause to only fetch rows related to the current change>)) RETURNING *
```