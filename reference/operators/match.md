---
title: "%% operator"
layout: en
---

# `%%` operator

## Summary

`%%` operator performs full text search by one keyword.

## Syntax

```sql
column %% keyword
```

`column` is a column to be searched.

`keyword` is a keyword for full text search. It's `text` type.

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

You can perform full text search with one keyword by `%%` operator:

```sql
SELECT * FROM memos WHERE content %% 'engine';
--  id |                                content                                 
-- ----+------------------------------------------------------------------------
--   2 | Groonga is a fast full text search engine that supports all languages.
-- (1 row)
```

If you want to perform full text search with multiple keywords or AND/OR search, use [`@@` operator](query.html).

## See also

  * [`@@` operator](query.html)
