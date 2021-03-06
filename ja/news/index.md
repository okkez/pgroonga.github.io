---
title: おしらせ
layout: ja
---

# おしらせ

## 1.0.1: 2015-12-29 {#version-1-0-1}

### 改良

  * Accepted `none` as none value for tokenizer and normalizer. [groonga-dev,03664] [Reported by Naoki Takami]

  * Supported `CREATE DATABASE TABLESPACE`. [Suggested by Hiroyuki Sato]

### 修正

  * Fixed a bug that sequential scan doesn't work for regular expression search.  [Reported by Hiroyuki Sato]

### 感謝

  * Naoki Takami

  * Hiroyuki Sato

## 1.0.0: 2015-10-29 {#version-1-0-0}

It's the first major release!!!

You need to run `DROP EXTENSION pgroonga CASCADE`, upgrade PGroonga binary, run `CREATE EXTENSION pgroonga` and create indexes again to upgrade to 1.0.0 from older versions.

### 改良

  * Propagated double initialization on failing to open database. [groonga-dev,03528] [Reported by Naoki Takami]

  * Supported index only scan.

  * Supported regular expression search by `@~`. [groonga-dev,03563] [Reported by Hiroaki Tachikawa]

  * [Windows] Bundled MeCab.

  * Made `LIKE` with index outputs `LIKE` with sequential scan compatible outputs. It's implemented by using recheck feature provided by PostgreSQL. It means that `LIKE` is slower than `%%` operator and `@@` operator.

  * Supported `ILIKE` with index.

### 感謝

  * Naoki Takami

  * Hiroaki Tachikawa

## 0.9.0: 2015-09-29 {#version-0-9-0}

You can update to 0.9.0 from 0.8.0 by override install and executing the following SQL:

```sql
ALTER EXTENSION pgroonga UPDATE;
```

You don't need to re-create `pgroonga` indexes.

### 改良

  * Supported `jsonb`. You can use `@>` operator like GIN index for `jsonb`. `@>` operator works like GIN index for `jsonb`. It's compatible. You can also use `@@` operator. It's PGroonga original operator. It's more flexible.

## 0.8.0: 2015-09-01

You can update to 0.8.0 from 0.7.0 by override install. You don't need to re-create `pgroonga` indexes.

### 改良

  * Reduced needless loop on `VACUUM`.
  * Reduced temporary memory usage.
  * `pgroonga.log_path`: Added a variable that changes path for Groonga log.
  * incompatible: Changed the path for Groonga log to the database directory by default. You can change it by `pgroonga.log_path`.
  * `pgroonga.log_type`: Added a variable that changes how to log.
  * Supported `TRUNCATE`-ed table. [GitHub:#1] [Reported by Hiroki Nakamura]
  * `pgroonga.snippet_html()`: Added a function that generates snippet HTML. [groonga-dev,03398] [Reported by Hiroki Nakamura]
  * Supported Ubuntu 14.04 LTS (Trusty Tahr). [Suggested by Yokoda Toshiaki]
  * `pgroonga.lock_timeout`: Added a variable that changes the number of lock retries. [groonga-dev,03419] [Suggested by Naoki Takami]

### 感謝

  * Hiroki Nakamura
  * Yokoda Toshiaki
  * Naoki Takami

## 0.7.0: 2015-07-10

You can update to 0.6.0 from 0.5.0 by override install. You don't need to re-create `pgroonga` indexes.

### 改良

  * incompatible: Changed to use Groonga's default logger. Messages for PGroonga is logged to `pgroonga.log` in database directory instead of PostgreSQL's log path.
  * `pgroonga.log_level`: Added a variable that changes log level.

### 修正

  * Fixed a bug that lexicon tables for dropped indexes aren't removed on `VACUUM`.

## 0.6.0: 2015-05-29

You can update to 0.6.0 from 0.5.0 by override install. You don't need to re-create `pgroonga` indexes.

### 改良

  * `pgroonga.score()`: Supported HOT update on PostgreSQL 9.3.
  * Supported log messages from Groonga.
  * Stopped to try opening Groonga database when Groonga database path doesn't exist.
  * Supported Debian GNU/Linux Jessie.

### 修正

  * Fixed a bug that large block number in ctid is overflowed.

## 0.5.0: 2015-04-29

You can't upgrade to 0.5.0 from 0.4.0 without re-creating `pgroonga` index. You need to re-install PGroonga:

```sql
DROP EXTENSION pgroonga CASCADE;
CREATE EXTENSION pgroonga;
-- Create your pgroonga indexes again.
```

### 改良

  * `pgroonga.score()`: Supported HOT update.
  * Supported Ubuntu 15.04 Vivid Vervet.
  * Supported Windows.

### 変更

  * `pgroonga.score()`: Required primary key.

## 0.4.0: 2015-03-29

You can't upgrade to 0.4.0 from 0.3.0 without re-creating `pgroonga` index. You need to re-install PGroonga:

```sql
DROP EXTENSION pgroonga CASCADE;
CREATE EXTENSION pgroonga;
-- Create your pgroonga indexes again.
```

### 改良

  * Supported `column LIKE '%keyword'` as a short cut of `column @@ 'keyword'`.
  * Supported range search with multi-column index.
  * Added PGroonga setup script on Travis CI. Add the following line to `install` section in your `.travis.yml`:

        curl --silent --location https://github.com/pgroonga/pgroonga/raw/master/data/travis/setup.sh | sh

  * Added `pgroonga.table_name()` that returns table name in Groonga.
  * Added `pgroonga.command()` that executes Groonga command line.
  * Added `pgroonga.score()` that returns search score in Groonga.
  * Supported `timestamp` type.
  * Supported `timestamp with time zone` type.
  * Supported `varchar[]` type.
  * Supported full-text search for `text[]` type.
  * Supported full-text search by index and other search by index in one `SELECT`.
  * Added Yum repositories for CentOS 5 and 6.

### 変更

  * Dropped `text == text` search by index. Use 4096 bytes or smaller `varchar` instead.
  * Dropped PostgreSQL 9.2 support.

## 0.3.0: 2015-02-09

You can't upgrade to 0.3.0 from 0.2.0 without re-creating `pgroonga` index. You need to re-install PGroonga:

```sql
DROP EXTENSION pgroonga CASCADE;
CREATE EXTENSION pgroonga;
-- Create your pgroonga indexes again.
```

### 改良

  * Supported encoding
  * Supported customizing tokenizer and normalizer by `WITH` such as:

    ```sql
    CREATE INDEX pgroonga_index
              ON table
           USING pgroonga (column)
            WITH (tokenizer='TokenMecab',
                  normalizer='NormalizerAuto');
    ```

  * Reduced needless locks.
  * Supported column compression by LZ4.
  * Supported non full-text search index such as text, numbers and timestamp.

### 変更

  * Changed database file base name to `pgrn` from `grn`.

## 0.2.0: 2015-01-29

The first release!!!
