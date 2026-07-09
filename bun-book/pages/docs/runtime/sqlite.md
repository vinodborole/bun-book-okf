---
type: Web Page
title: SQLite - Bun
description: Bun natively implements a high-performance SQLite3 driver.
resource: https://bun.sh/docs/runtime/sqlite
timestamp: '2026-07-09T12:17:04.216670+00:00'
---

[SQLite3](https://www.sqlite.org/)driver. To use it, import from the built-in

`bun:sqlite` module.
db.ts

[better-sqlite3](https://github.com/JoshuaWise/better-sqlite3)and its contributors for inspiring the API of

`bun:sqlite`.
Features include:
- Transactions
- Parameters (named & positional)
- Prepared statements
- Datatype conversions (`BLOB`becomes`Uint8Array`)
- Map query results to classes without an ORM - `query.as(MyClass)`
- The fastest performance of any SQLite driver for JavaScript
- `bigint`support
- Multi-query statements (for example `SELECT 1; SELECT 2;`) in a single call to`database.run(query)`

`bun:sqlite` module is roughly 3-6x faster than `better-sqlite3` and 8-9x faster than `deno.land/x/sqlite` for read queries. Each driver was benchmarked against the [Northwind Traders](https://github.com/jpwhite3/northwind-SQLite3/blob/46d5f8a64f396f87cd374d1600dbf521523980e8/Northwind_large.sqlite.zip)dataset. View and run the

[benchmark source](https://github.com/oven-sh/bun/tree/main/bench/sqlite).

## Database

To open or create a SQLite3 database:db.ts

db.ts

`readonly` mode:
db.ts

db.ts

### Strict mode

By default,`bun:sqlite` requires binding parameters to include the `$`, `:`, or `@` prefix, and does not throw an error if a parameter is missing.
To instead throw an error when a parameter is missing and allow binding without a prefix, set `strict: true` on the `Database` constructor:
db.ts

### Load via ES module import

You can also load a database with an import attribute.db.ts

db.ts

`.close(throwOnError: boolean = false)`

To close a database connection but allow existing queries to finish, call `.close(false)`:
db.ts

`.close(true)`:
db.ts

`close(false)` is called automatically when the database is garbage collected. It is safe to call multiple times but
has no effect after the first.`using` statement

The `using` statement closes the database connection when the block exits.
db.ts

`.serialize()`

`bun:sqlite` supports SQLite’s built-in mechanism for [serializing](https://www.sqlite.org/c3ref/serialize.html)and

[deserializing](https://www.sqlite.org/c3ref/deserialize.html)databases to and from memory.

db.ts

`.serialize()` calls [.](https://www.sqlite.org/c3ref/serialize.html)

`sqlite3_serialize``.query()`

Use the `db.query()` method on your `Database` instance to [prepare](https://www.sqlite.org/c3ref/prepare.html)a SQL query. The result is a

`Statement` instance that is cached on the `Database` instance. *The query is not executed.*

db.ts

**What does “cached” mean?**The caching refers to the

**compiled prepared statement**(the SQL bytecode), not the query results. When you call

`db.query()` with the same SQL string multiple times, Bun returns the same cached `Statement` object instead of recompiling the SQL.It is safe to reuse a cached statement with different parameter values:`.prepare()` instead of `.query()` when you want a fresh `Statement` instance that isn’t cached, for example if you’re dynamically generating SQL and don’t want to fill the cache with one-off queries.## WAL mode

SQLite supports[write-ahead log mode](https://www.sqlite.org/wal.html)(WAL), which dramatically improves performance, especially with many concurrent readers and a single writer. Enabling WAL mode is recommended for most applications. To enable WAL mode, run this pragma query at the beginning of your application:

db.ts

What is WAL mode?

What is WAL mode?

In WAL mode, writes to the database are written directly to a separate file called the “WAL file” (

`-wal`). A
shared-memory index file (`-shm`) is also created for read coordination. The WAL file is later integrated into the
main database file. Think of it as a buffer for pending writes. Refer to the [SQLite docs](https://www.sqlite.org/wal.html)for a more detailed overview.### WAL sidecar file cleanup

When using WAL mode with a file-based database, SQLite creates two sidecar files alongside your database: a write-ahead log (`-wal`) and a shared-memory index (`-shm`). Whether these files are automatically removed after `.close()` depends on your platform:
- **macOS**: Bun uses the system-provided SQLite, which Apple builds with persistent WAL enabled. The- `-wal`and- `-shm`files- **persist**after close. This is not a bug — it is how Apple configured the system SQLite.
- **Linux**and- **Windows**: Bun statically links its own SQLite build, which follows upstream defaults. The sidecar files are- **typically removed**after close when no other connections are open.

db.ts

## Statements

A`Statement` is a *prepared query*, which means it’s been parsed and compiled into an efficient binary form. It can be executed multiple times. Create a statement with the

`.query` method on your `Database` instance.
db.ts

`?1`) or named (`$param` or `:param` or `@param`).
db.ts

`Statement` can be executed with several different methods, each returning the results in a different form.
### Binding values

To bind values to a statement, pass an object to the`.all()`, `.get()`, `.run()`, or `.values()` method.
db.ts

db.ts

`strict: true` lets you bind values without prefixes

By default, the `$`, `:`, and `@` prefixes are **included**when binding values to named parameters. To bind without these prefixes, use the

`strict` option in the `Database` constructor.
db.ts

`.all()`

Use `.all()` to run a query and get back the results as an array of objects.
db.ts

[and repeatedly calls](https://www.sqlite.org/capi3ref.html#sqlite3_reset)

`sqlite3_reset`[until it returns](https://www.sqlite.org/capi3ref.html#sqlite3_step)

`sqlite3_step``SQLITE_DONE`.
`.get()`

Use `.get()` to run a query and get back the first result as an object.
db.ts

[followed by](https://www.sqlite.org/capi3ref.html#sqlite3_reset)

`sqlite3_reset`[until it no longer returns](https://www.sqlite.org/capi3ref.html#sqlite3_step)

`sqlite3_step``SQLITE_ROW`. If the query returns no rows, `null` is returned.
`.run()`

Use `.run()` to run a query and get back an object with execution metadata. This is useful for schema-modifying queries (such as `CREATE TABLE`) or bulk write operations.
db.ts

[and calls](https://www.sqlite.org/capi3ref.html#sqlite3_reset)

`sqlite3_reset`[once. Stepping through all the rows is not necessary when you don’t care about the results. The](https://www.sqlite.org/capi3ref.html#sqlite3_step)

`sqlite3_step``lastInsertRowid` property is the ID of the last row inserted into the database. The `changes` property is the number of rows affected by the query.
`.as(Class)` - Map query results to a class

Use `.as(Class)` to run a query and get back the results as instances of a class. The class’s methods, getters, and setters are available on each row.
db.ts

`Object.create` than `new`: the class’s prototype is assigned to the object, so its methods, getters, and setters work.
The database columns are set as properties on the class instance.
`.iterate()` (`@@iterator`)

Use `.iterate()` to run a query and incrementally return results. This is useful for large result sets that you want to process one row at a time without loading all the results into memory.
db.ts

`@@iterator` protocol:
db.ts

`.values()`

Use `values()` to run a query and get back all results as an array of arrays.
db.ts

[and repeatedly calls](https://www.sqlite.org/capi3ref.html#sqlite3_reset)

`sqlite3_reset`[until it returns](https://www.sqlite.org/capi3ref.html#sqlite3_step)

`sqlite3_step``SQLITE_DONE`.
`.finalize()`

Use `.finalize()` to destroy a `Statement` and free any resources associated with it. Once finalized, a `Statement` cannot be executed again. Typically, the garbage collector does this for you, but explicit finalization may be useful in performance-sensitive applications.
db.ts

`.toString()`

Calling `toString()` on a `Statement` instance prints the expanded SQL query. This is useful for debugging.
db.ts

[. The parameters are expanded using the most recently bound values.](https://www.sqlite.org/capi3ref.html#sqlite3_expanded_sql)

`sqlite3_expanded_sql`## Parameters

Queries can contain parameters. These can be numerical (`?1`) or named (`$param` or `:param` or `@param`). Bind values to these parameters when executing the query:
query.ts

db.ts

## Integers

SQLite supports signed 64-bit integers, but JavaScript only supports signed 52-bit integers or arbitrary-precision integers with`bigint`.
`bigint` input is supported everywhere, but by default `bun:sqlite` returns integers as `number` types. If you need to handle integers larger than 2^53, set the `safeIntegers` option to `true` when creating a `Database` instance. This also validates that `bigint` values passed to `bun:sqlite` do not exceed 64 bits.
`safeIntegers: true`

When `safeIntegers` is `true`, `bun:sqlite` returns integers as `bigint` types:
db.ts

`safeIntegers` is `true`, `bun:sqlite` throws an error if a `bigint` value in a bound parameter exceeds 64 bits:
db.ts

`safeIntegers: false` (default)

When `safeIntegers` is `false`, `bun:sqlite` returns integers as `number` types and truncates any bits beyond 53:
db.ts

## Transactions

A transaction executes multiple queries*atomically*: either all of them succeed or none do. Create a transaction with the

`db.transaction()` method:
db.ts

`db.transaction()` returns a new function (`insertCats`) that *wraps*the function that executes the queries. To execute the transaction, call this function. Arguments are passed through to the wrapped function, and the wrapped function’s return value is returned by the transaction function. The wrapped function also has access to the

`this` context as defined where the transaction is executed.
db.ts

[begins](https://www.sqlite.org/lang_transaction.html)a transaction when

`insertCats` is called and commits it when the wrapped function returns. If an exception is thrown, the transaction is rolled back. The exception propagates as usual; it is not caught.
**Nested transactions**— Transaction functions can be called from inside other transaction functions. When doing so, the inner transaction becomes a

[savepoint](https://www.sqlite.org/lang_savepoint.html).

View nested transaction example

View nested transaction example

db.ts

`deferred`, `immediate`, and `exclusive` versions.
`.loadExtension()`

To load a [SQLite extension](https://www.sqlite.org/loadext.html), call

`.loadExtension(name)` on your `Database` instance:
db.ts

**macOS users**By default, macOS ships with Apple’s proprietary build of SQLite, which doesn’t support extensions. To use extensions, install a vanilla build of SQLite.

terminal

`bun:sqlite` to the new build, call `Database.setCustomSQLite(path)` before creating any `Database` instances. (On other operating systems, this is a no-op.) Pass a path to the SQLite `.dylib` file, *not*the executable. With recent versions of Homebrew this is something like

`/opt/homebrew/Cellar/sqlite/<version>/libsqlite3.dylib`.db.ts

`.fileControl(cmd: number, value: any)`

To use the advanced `sqlite3_file_control` API, call `.fileControl(cmd, value)` on your `Database` instance. See [WAL sidecar file cleanup](#wal-sidecar-file-cleanup)for a practical example.

db.ts

`value` can be:
- `number`
- `TypedArray`
- `undefined`or- `null`

## Reference

Type Reference

### Datatypes

| JavaScript type | SQLite type | 
|---|---|
| `string` | `TEXT` | 
| `number` | `INTEGER`or`DECIMAL` | 
| `boolean` | `INTEGER`(1 or 0) | 
| `Uint8Array` | `BLOB` | 
| `Buffer` | `BLOB` | 
| `bigint` | `INTEGER` | 
| `null` | `NULL` |

# Citations

1. Source page: https://bun.sh/docs/runtime/sqlite
