---
title: "type safe sql"
priority: "p1"
---

#project

People keep writing crappy query builders in order to get type safety in their language of choice.

We can get it by parsing:
1. The schema file(s)
2. The SQL queries themselves

Usage looks like:

```ts
const schema = ingest(schema_files...);
const q = schema.sql`SELECT * FROM x join y ...`;
```

`q` should have full type information about what was selected and the types of those things.
Ideally we can do inline autocomplete in the SQL string. Do GraphQL plugins do this in GraphQL frags that we can get implementation tips from?

# Implementation References
- LSP Plugin Reference: https://github.com/microsoft/TypeScript/wiki/Writing-a-Language-Service-Plugin
- https://github.com/Microsoft/typescript-template-language-service-decorator - high level / the decorator framework for tagged literals
- https://github.com/mjbvz/vscode-lit-html
- https://github.com/Microsoft/vscode/issues/41113 - high level overview
- https://github.com/joe-re/sql-language-server - abandoned LSP for SQL. Requires a SQLite db file :/ rather than just taking schema file(s) as input
- https://github.com/codeschool/sqlite-parser - sqlite -> AST
- https://github.com/styled-components/typescript-styled-plugin - recent impl

# References
- https://discord.com/channels/989870439897653248/989870440585494530/1125443906675409010
- [[TreeQL]] - https://github.com/tantaman/composed-sql