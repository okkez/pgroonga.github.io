---
title: "@@ operator for non jsonb types"
layout: en
---

# `@@` operator for non `jsonb` types

## Summary

`@@` operator performs full text search with query. Its syntax is similar to syntax that is used in Web search engine. For example, you can use OR search by `KEYWORD1 OR KEYWORD2` in query.

## Syntax

```sql
column @@ query
```

`column` is a column to be searched.

`query` is a query for full text search. It's `text` type.

[Groonga's query syntax](http://groonga.org/docs/reference/grn_expr/query_syntax.html) is used in `query`.

## Usage

Here are sample schema and data for examples:

```sql
CREATE TABLE memos (
  id integer,
  content text
);

CREATE INDEX pgroonga_content_index ON memos USING pgroonga (content);
```

```sql
INSERT INTO memos VALUES (1, 'PostgreSQL is a relational database management system.');
INSERT INTO memos VALUES (2, 'Groonga is a fast full text search engine that supports all languages.');
INSERT INTO memos VALUES (3, 'PGroonga is a PostgreSQL extension that uses Groonga as index.');
INSERT INTO memos VALUES (4, 'There is groonga command.');
```

You can perform full text search with multiple keywords by `@@` operator like `KEYWORD1 KEYWORD2`. You can also do OR search by `KEYWORD1 OR KEYWORD2`:

```sql
SELECT * FROM memos WHERE content @@ 'PGroonga OR PostgreSQL';
--  id |                            content                             
-- ----+----------------------------------------------------------------
--   3 | PGroonga is a PostgreSQL extension that uses Groonga as index.
--   1 | PostgreSQL is a relational database management system.
-- (2 rows)
```

See [Groonga document](http://groonga.org/docs/reference/grn_expr/query_syntax.html) for query syntax details.

Note that you can't use syntax that starts with `COLUMN_NAME:` like `COLUMN_NAME:@KEYWORD`. It's disabled in PGroonga.

You can't use `COLUMN_NAME:^VALUE` for prefix search. You need to use `VALUE*` for prefix search.

## Sequential scan

TODO: Describe about `SET search_path = "$user",public,pgroonga,pg_catalog;`.

## See also

  * [`%%` operator](match.html)

  * [Groonga's query syntax](http://groonga.org/docs/reference/grn_expr/query_syntax.html)
