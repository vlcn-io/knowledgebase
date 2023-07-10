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
const q = schema.sql<_autoinject from eslint_>`SELECT * FROM x join y ...`;
```

`q` should have full type information about what was selected and the types of those things. As a branded type?
Ideally we can do inline autocomplete in the SQL string. Do GraphQL plugins do this in GraphQL frags that we can get implementation tips from?

# V0
1. Walk the AST https://github.com/itsdouges/typescript-transformer-handbook , https://github.com/microsoft/TypeScript/wiki/Using-the-Compiler-API
	1. find
	   ```ts
	   SCHEMA.sql<NAME>`sql`
	   ```
2. Resolve SCHEMA to the thing imported and applied (js file with content property)
3. Used that as schema type source
4. Generate types from tagged literal contents
5. Output to typing file in `__generated__` dir as sibling to current file named NAME.

# V1
An initial bare-bones version:
1. Syntax highlighting provided by some other pkg
	1. e.g, https://github.com/thebearingedge/vscode-sql-lit
2. Generate types via your own codegen, stick them into after tag `<>` via eslint.
	1. Can write this step in Rust? Given you'll need the parser for RX anyway.
3. Completions come later

Even simpler start?
```ts
// Watch SQL files and generate Schema.d.ts files of the same name?
// Then they can be imported and used.
// Ts TS Plugin (or ESLint plugin or whatever..) will scan for their usage
// so we can generate types
const q = schema.sql<UserType>`...`
```

Can we do TS Source Transform rather than ESLint?
https://github.com/Microsoft/TypeScript/issues/29993
[[ts ast]]
[[ts transform plugin]]

---

# Inspiration
https://github.com/Quramy/ts-graphql-plugin
ts-graphpl-plugin config:

```json
{
  "compilerOptions": {
    "module": "commonjs",
    "target": "es5",
    "plugins": [
      {
        "name": "ts-graphql-plugin",
        "schema": "path-or-url-to-your-schema.graphql",
        "tag": "gql"
      }
    ]
  }
}
```

We can take:
- sql files
- or js files with a sql member exported?
	- `import * as foo from 'thing';` and access the configured property?

CLI commands, bundler integrations.

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