---
type: Web Page
title: SQL - Bun
description: Bun provides native bindings for working with SQL databases through a
  unified Promise-based API that supports PostgreSQL, MySQL, and SQLite.
resource: https://bun.sh/docs/runtime/sql
timestamp: '2026-07-07T10:59:41.879776+00:00'
---

db.ts

### Features

- Tagged template literals to protect against SQL injection
- Transactions
- Named & positional parameters
- Connection pooling
- `BigInt`support
- SASL (SCRAM-SHA-256), MD5, and Clear Text authentication
- Connection timeouts
- Returning rows as data objects, arrays of arrays, or Buffer
- Binary protocol support makes it faster
- TLS support (and auth mode)
- Automatic configuration with environment variables

## Database Support

`Bun.SQL` provides a unified API for multiple database systems:
### PostgreSQL

PostgreSQL is used when:- The connection string doesn’t match SQLite or MySQL patterns (it’s the fallback adapter)
- The connection string explicitly uses `postgres://`or`postgresql://`protocols
- No connection string is provided and environment variables point to PostgreSQL

db.ts

### MySQL

MySQL support is built into`Bun.SQL`, with the same tagged template literal interface, and is compatible with MySQL 5.7+ and MySQL 8.0+:
db.ts

MySQL Connection String Formats

MySQL Connection String Formats

MySQL accepts various URL formats for connection strings:

MySQL-Specific Features

MySQL-Specific Features

MySQL databases support:

- **Prepared statements**: Automatically created for parameterized queries with statement caching
- **Binary protocol**: For better performance with prepared statements and accurate type handling
- **Multiple result sets**: Support for stored procedures returning multiple result sets
- **Authentication plugins**: Support for mysql_native_password, caching_sha2_password (MySQL 8.0 default), and sha256_password
- **SSL/TLS connections**: Configurable SSL modes similar to PostgreSQL
- **Connection attributes**: Client information sent to server for monitoring
- **Query pipelining**: Execute multiple prepared statements without waiting for responses

### SQLite

SQLite support is built into`Bun.SQL`, with the same tagged template literal interface:
SQLite Connection String Formats

SQLite Connection String Formats

SQLite accepts various URL formats for connection strings:

Simple filenames without a protocol (like 

`"myapp.db"`) require explicitly specifying `{ adapter: "sqlite" }` to avoid ambiguity with PostgreSQL.SQLite-Specific Options

SQLite-Specific Options

SQLite databases support additional configuration options:Query parameters in the URL are parsed to set these options:

- `?mode=ro`→- `readonly: true`
- `?mode=rw`→- `readonly: false, create: false`
- `?mode=rwc`→- `readonly: false, create: true`(default)

## Inserting data

Pass JavaScript values directly to the SQL template literal; Bun handles the escaping.### Bulk Insert

You can also pass an array of objects, which Bun expands into an`INSERT INTO ... VALUES ...` statement.
### Picking columns to insert

Use`sql(object, ...string)` to pick which columns to insert. Each column must be defined on the object.
## Query Results

By default, Bun’s SQL client returns query results as arrays of objects, where each object represents a row with column names as keys. Two other formats are available.`sql``.values()` format

The `sql``.values()` method returns each row as an array of values, in the same order as the columns in your query.
`sql``.values()` is useful when a query returns duplicate column names. With objects (the default), the last column wins because the column name is the key. With `sql``.values()`, every column is present in the array, so you can read duplicates by index.
`sql``.raw()` format

The `.raw()` method returns rows as arrays of `Buffer` objects. Use it for binary data or for performance.
## SQL Fragments

Bun can build queries dynamically from runtime conditions without risking SQL injection.### Dynamic Table Names

To reference tables or schemas dynamically, use the`sql()` helper, which escapes them:
### Conditional Queries

Use the`sql()` helper to build queries with conditional clauses:
### Dynamic columns in updates

Use`sql(object, ...string)` to pick which columns to update. Each column must be defined on the object. If you don’t list any columns, all keys on the object are used.
### Dynamic values and `where in`

Value lists can also be created dynamically, for `WHERE IN` queries. You can also pass an array of objects and name the key to build the list from.
`sql.array` helper

The `sql.array` helper creates PostgreSQL array literals from JavaScript arrays:
`sql.array` is PostgreSQL-only. Multi-dimensional arrays and NULL elements may not be supported yet.`sql``.simple()`

The PostgreSQL wire protocol supports two types of queries: “simple” and “extended”. Simple queries can contain multiple statements but don’t support parameters, while extended queries (the default) support parameters but only allow one statement.
To run multiple statements in a single query, use `sql``.simple()`:
`${value}`). If you need parameters, split your query into separate statements.
### Queries in files

`sql.file` reads a query from a file and executes it. If the file uses placeholders like `$1` and `$2`, you can pass parameters to the query. Without parameters, the file can contain multiple commands.
### Unsafe Queries

`sql.unsafe` executes raw SQL strings. Use it with caution: it does not escape user input. Without parameters, the string can contain more than one command.
### Execute and Cancelling Queries

Queries are lazy: they only start executing when awaited or run with`.execute()`.
To cancel a running query, call `cancel()` on the query object.
## Database Environment Variables

You can configure`sql` connection parameters with environment variables. The client checks them in order of precedence and detects the database type from the connection string format.
### Automatic Database Detection

When you use`Bun.sql()` without arguments, or `new SQL()` with a connection string, Bun detects the adapter from the URL format:
#### MySQL Auto-Detection

MySQL is selected when the connection string matches these patterns:- `mysql://...`- MySQL protocol URLs
- `mysql2://...`- MySQL2 protocol URLs (compatibility alias)

#### SQLite Auto-Detection

SQLite is selected when the connection string matches these patterns:- `:memory:`- In-memory database
- `sqlite://...`- SQLite protocol URLs
- `sqlite:...`- SQLite protocol without slashes
- `file://...`- File protocol URLs
- `file:...`- File protocol without slashes

#### PostgreSQL Auto-Detection

PostgreSQL is the default for connection strings that don’t match MySQL or SQLite patterns:### MySQL Environment Variables

MySQL connections can be configured with environment variables:| Environment Variable | Default Value | Description | 
|---|---|---|
| `MYSQL_HOST` | `localhost` | Database host | 
| `MYSQL_PORT` | `3306` | Database port | 
| `MYSQL_USER` | `root` | Database user | 
| `MYSQL_PASSWORD` | (empty) | Database password | 
| `MYSQL_DATABASE` | `mysql` | Database name | 
| `MYSQL_URL` | (empty) | Primary connection URL for MySQL | 
| `TLS_MYSQL_DATABASE_URL` | (empty) | SSL/TLS-enabled connection URL | 

### PostgreSQL Environment Variables

These environment variables define the PostgreSQL connection:| Environment Variable | Description | 
|---|---|
| `POSTGRES_URL` | Primary connection URL for PostgreSQL | 
| `DATABASE_URL` | Alternative connection URL (auto-detected) | 
| `PGURL` | Alternative connection URL | 
| `PG_URL` | Alternative connection URL | 
| `TLS_POSTGRES_DATABASE_URL` | SSL/TLS-enabled connection URL | 
| `TLS_DATABASE_URL` | Alternative SSL/TLS-enabled connection URL | 

| Environment Variable | Fallback Variables | Default Value | Description | 
|---|---|---|---|
| `PGHOST` | - | `localhost` | Database host | 
| `PGPORT` | - | `5432` | Database port | 
| `PGUSERNAME` | `PGUSER`,`USER`,`USERNAME` | `postgres` | Database user | 
| `PGPASSWORD` | - | (empty) | Database password | 
| `PGDATABASE` | - | username | Database name | 

### SQLite Environment Variables

SQLite connections can be configured with`DATABASE_URL` when it contains a SQLite-compatible URL:
**Note:**PostgreSQL-specific environment variables such as

`POSTGRES_URL` and `PGHOST` are ignored when using SQLite.
## Runtime Preconnection

Bun can preconnect to PostgreSQL at startup, before your application code runs, so the first query doesn’t pay the connection latency.`--sql-preconnect` flag establishes a PostgreSQL connection at startup using your configured environment variables. If the connection fails, the error is handled without crashing your application.
## Connection Options

You can configure the connection manually by passing options to the`SQL` constructor. Options vary by adapter:
### MySQL Options

### PostgreSQL Options

### SQLite Options

SQLite Connection Notes

SQLite Connection Notes

- **Connection Pooling**: SQLite doesn’t use connection pooling as it’s a file-based database. Each- `SQL`instance represents a single connection.
- **Transactions**: SQLite supports nested transactions through savepoints, similar to PostgreSQL.
- **Concurrent Access**: SQLite handles concurrent access through file locking. Use WAL mode for better concurrency.
- **Memory Databases**: Using- `:memory:`creates a temporary database that exists only for the connection lifetime.

## Dynamic passwords

For alternative authentication schemes such as access tokens, or databases with rotating passwords, set`password` to a synchronous or asynchronous function. Bun calls it at connection time to resolve the password.
## SQLite-Specific Features

### Query Execution

SQLite executes queries synchronously, unlike PostgreSQL, which uses asynchronous I/O. The API still returns Promises:### SQLite Pragmas

Use`PRAGMA` statements to configure SQLite behavior:
### Data Type Differences

SQLite has a more flexible type system than PostgreSQL:## Transactions

To start a new transaction, use`sql.begin`. This method works for both PostgreSQL and SQLite. For PostgreSQL, it reserves a dedicated connection from the pool. For SQLite, it begins a transaction on the single connection.
The `BEGIN` command is sent automatically, including any optional configurations you specify. If an error occurs during the transaction, Bun issues a `ROLLBACK`.
### Basic Transactions

### Savepoints

Savepoints create intermediate checkpoints within a transaction, so part of it can roll back without aborting the whole thing.### Distributed Transactions

Two-Phase Commit (2PC) is a distributed transaction protocol: in phase 1 the coordinator prepares each node, making sure its data is written and ready to commit, and in phase 2 the nodes commit or roll back based on the coordinator’s decision. In PostgreSQL and MySQL, distributed transactions persist beyond their original session, so privileged users or coordinators can commit or roll them back later. PostgreSQL implements them as prepared transactions; MySQL uses XA Transactions. An uncaught exception during the distributed transaction rolls back all changes. Otherwise, you can commit or roll back the transaction later.## Authentication

Bun supports SCRAM-SHA-256 (SASL), MD5, and Clear Text authentication. SASL is recommended for better security. See Postgres SASL Authentication.### SSL Modes Overview

PostgreSQL’s SSL/TLS modes control whether a secure connection is required and how much certificate verification is performed.| SSL Mode | Description | 
|---|---|
| `disable` | No SSL/TLS used. Connections fail if server requires SSL. | 
| `prefer` | Tries SSL first, falls back to non-SSL if SSL fails. Default mode if none specified. | 
| `require` | Requires SSL without certificate verification. Fails if SSL cannot be established. | 
| `verify-ca` | Verifies server certificate is signed by trusted CA. Fails if verification fails. | 
| `verify-full` | Most secure mode. Verifies certificate and hostname match. Protects against untrusted certificates and MITM attacks. | 

### Using With Connection Strings

You can also set the SSL mode in the connection string:## Connection Pooling

Bun’s SQL client manages a connection pool: database connections are reused across queries instead of being opened and closed for each one, and the pool caps the number of concurrent connections.## Reserved Connections

`sql.reserve()` takes a connection from the pool and returns a client that wraps it, so you can run queries on an isolated connection.
## Prepared Statements

By default, Bun’s SQL client creates named prepared statements for queries it can infer are static, which is faster. To disable this, set`prepare: false` in the connection options:
`prepare: false` is set:
Queries still use the “extended” protocol, but run as unnamed prepared statements. An unnamed prepared statement lasts only until the next Parse statement specifying the unnamed statement as destination is issued.
- Parameter binding is still safe against SQL injection
- Each query is parsed and planned from scratch by the server
- Queries are not pipelined

`prepare: false` when:
- Using PGBouncer in transaction mode (though since PGBouncer 1.21.0, protocol-level named prepared statements are supported when configured properly)
- Debugging query execution plans
- Working with dynamic SQL where query plans need to be regenerated frequently
- Only one command per query is supported (unless you use `sql``.simple()`)

## Error Handling

The client provides typed errors for different failure scenarios. Errors are database-specific and extend a base error class:### Error Classes

PostgreSQL-Specific Error Codes

PostgreSQL-Specific Error Codes

### PostgreSQL Connection Errors

| Connection Errors | Description | 
|---|---|
| `ERR_POSTGRES_CONNECTION_CLOSED` | An established connection was terminated | 
| `ERR_POSTGRES_CONNECTION_FAILED` | Connection was accepted but closed before the handshake completed (e.g. the server is still starting up). Retried with backoff until `connectionTimeout`while queries are waiting. Note: errors the server sends during startup, like`57P03`, surface as`ERR_POSTGRES_SERVER_ERROR` | 
| `ERR_POSTGRES_CONNECTION_REFUSED` | Connection was refused because nothing is listening at the address. Fails immediately and is not retried | 
| `ERR_POSTGRES_CONNECTION_TIMEOUT` | Failed to establish connection within timeout period | 
| `ERR_POSTGRES_IDLE_TIMEOUT` | Connection closed due to inactivity | 
| `ERR_POSTGRES_LIFETIME_TIMEOUT` | Connection exceeded maximum lifetime | 
| `ERR_POSTGRES_TLS_NOT_AVAILABLE` | SSL/TLS connection not available | 
| `ERR_POSTGRES_TLS_UPGRADE_FAILED` | Failed to upgrade connection to SSL/TLS | 

### Authentication Errors

| Authentication Errors | Description | 
|---|---|
| `ERR_POSTGRES_AUTHENTICATION_FAILED_PBKDF2` | Password authentication failed | 
| `ERR_POSTGRES_UNKNOWN_AUTHENTICATION_METHOD` | Server requested unknown auth method | 
| `ERR_POSTGRES_UNSUPPORTED_AUTHENTICATION_METHOD` | Server requested unsupported auth method | 
| `ERR_POSTGRES_INVALID_SERVER_KEY` | Invalid server key during authentication | 
| `ERR_POSTGRES_INVALID_SERVER_SIGNATURE` | Invalid server signature | 
| `ERR_POSTGRES_SASL_SIGNATURE_INVALID_BASE64` | Invalid SASL signature encoding | 
| `ERR_POSTGRES_SASL_SIGNATURE_MISMATCH` | SASL signature verification failed | 

### Query Errors

| Query Errors | Description | 
|---|---|
| `ERR_POSTGRES_SYNTAX_ERROR` | Invalid SQL syntax (extends `SyntaxError`) | 
| `ERR_POSTGRES_SERVER_ERROR` | General error from PostgreSQL server | 
| `ERR_POSTGRES_INVALID_QUERY_BINDING` | Invalid parameter binding | 
| `ERR_POSTGRES_QUERY_CANCELLED` | Query was cancelled | 
| `ERR_POSTGRES_NOT_TAGGED_CALL` | Query was called without a tagged call | 

### Data Type Errors

| Data Type Errors | Description | 
|---|---|
| `ERR_POSTGRES_INVALID_BINARY_DATA` | Invalid binary data format | 
| `ERR_POSTGRES_INVALID_BYTE_SEQUENCE` | Invalid byte sequence | 
| `ERR_POSTGRES_INVALID_BYTE_SEQUENCE_FOR_ENCODING` | Encoding error | 
| `ERR_POSTGRES_INVALID_CHARACTER` | Invalid character in data | 
| `ERR_POSTGRES_OVERFLOW` | Numeric overflow | 
| `ERR_POSTGRES_UNSUPPORTED_BYTEA_FORMAT` | Unsupported binary format | 
| `ERR_POSTGRES_UNSUPPORTED_INTEGER_SIZE` | Integer size not supported | 
| `ERR_POSTGRES_MULTIDIMENSIONAL_ARRAY_NOT_SUPPORTED_YET` | Multidimensional arrays not supported | 
| `ERR_POSTGRES_NULLS_IN_ARRAY_NOT_SUPPORTED_YET` | NULL values in arrays not supported | 

### Protocol Errors

| Protocol Errors | Description | 
|---|---|
| `ERR_POSTGRES_EXPECTED_REQUEST` | Expected client request | 
| `ERR_POSTGRES_EXPECTED_STATEMENT` | Expected prepared statement | 
| `ERR_POSTGRES_INVALID_BACKEND_KEY_DATA` | Invalid backend key data | 
| `ERR_POSTGRES_INVALID_MESSAGE` | Invalid protocol message | 
| `ERR_POSTGRES_INVALID_MESSAGE_LENGTH` | Invalid message length | 
| `ERR_POSTGRES_UNEXPECTED_MESSAGE` | Unexpected message type | 

### Transaction Errors

| Transaction Errors | Description | 
|---|---|
| `ERR_POSTGRES_UNSAFE_TRANSACTION` | Unsafe transaction operation detected | 
| `ERR_POSTGRES_INVALID_TRANSACTION_STATE` | Invalid transaction state | 

### SQLite-Specific Errors

SQLite errors carry SQLite’s standard error codes and numbers:Common SQLite Error Codes

Common SQLite Error Codes

| Error Code | errno | Description | 
|---|---|---|
| `SQLITE_CONSTRAINT` | 19 | Constraint violation (UNIQUE, CHECK, NOT NULL, etc.) | 
| `SQLITE_BUSY` | 5 | Database is locked | 
| `SQLITE_LOCKED` | 6 | Table in the database is locked | 
| `SQLITE_READONLY` | 8 | Attempt to write to a readonly database | 
| `SQLITE_IOERR` | 10 | Disk I/O error | 
| `SQLITE_CORRUPT` | 11 | Database disk image is malformed | 
| `SQLITE_FULL` | 13 | Database or disk is full | 
| `SQLITE_CANTOPEN` | 14 | Unable to open database file | 
| `SQLITE_PROTOCOL` | 15 | Database lock protocol error | 
| `SQLITE_SCHEMA` | 17 | Database schema has changed | 
| `SQLITE_TOOBIG` | 18 | String or BLOB exceeds size limit | 
| `SQLITE_MISMATCH` | 20 | Data type mismatch | 
| `SQLITE_MISUSE` | 21 | Library used incorrectly | 
| `SQLITE_AUTH` | 23 | Authorization denied | 

## Numbers and BigInt

Numbers that exceed the range of a 53-bit integer are returned as strings:## BigInt Instead of Strings

To get large numbers as`BigInt` instead of strings, set the `bigint` option to `true` when creating the SQL client:
## Roadmap

Things we haven’t finished yet:- Connection preloading with the `--db-preconnect`Bun CLI flag
- Column name transforms (for example, `snake_case`to`camelCase`). This is mostly blocked on a unicode-aware implementation of changing the case in C++ using WebKit’s`WTF::String`.
- Column type transforms

## Database-Specific Features

#### Authentication Methods

MySQL supports multiple authentication plugins that are automatically negotiated:- `mysql_native_password`
- `caching_sha2_password`
- `sha256_password`

#### Prepared Statements & Performance

MySQL uses server-side prepared statements for all parameterized queries:#### Multiple Result Sets

MySQL can return multiple result sets from multi-statement queries:#### Character Sets & Collations

`Bun.SQL` uses the `utf8mb4` character set for MySQL connections, which covers all of Unicode, including emoji.
#### Connection Attributes

Bun sends client information to MySQL for monitoring:#### Type Handling

MySQL types are converted to JavaScript types:| MySQL Type | JavaScript Type | Notes | 
|---|---|---|
| INT, TINYINT, MEDIUMINT | number | Within safe integer range | 
| BIGINT | string, number or BigInt | number if the value fits in i32/u32, otherwise string or BigInt depending on the `bigint`option | 
| DECIMAL, NUMERIC | string | To preserve precision | 
| FLOAT, DOUBLE | number | |
| DATE | Date | JavaScript Date object | 
| DATETIME, TIMESTAMP | Date | Decoded as UTC (see note below); `0000-00-00`becomes an Invalid Date | 
| TIME | number | Total of microseconds | 
| YEAR | number | |
| CHAR, VARCHAR, VARSTRING, STRING | string | |
| TINY TEXT, MEDIUM TEXT, TEXT, LONG TEXT | string | |
| TINY BLOB, MEDIUM BLOB, BLOB, LONG BLOB | string | BLOB types are aliases for the TEXT types | 
| JSON | object/array | Automatically parsed | 
| BIT(1) | boolean | BIT(1) in MySQL | 
| GEOMETRY | string | Geometry data | 

`DATETIME` and `TIMESTAMP` values have no timezone on the wire, so Bun reads them back as **UTC**— the

`Date` you get has the same UTC wall-clock that was stored, regardless of the machine’s timezone. This matches how values are written (a bound `Date` stores its UTC components). The same applies to PostgreSQL’s `timestamp` (without time zone); `timestamptz` carries an explicit offset and is unaffected.
#### Differences from PostgreSQL

The API is unified, but behavior differs:- **Parameter placeholders**: MySQL uses- `?`internally but Bun converts- `$1, $2`style automatically
- **RETURNING clause**: MySQL doesn’t support RETURNING; use- `result.lastInsertRowid`or a separate SELECT
- **Array types**: MySQL doesn’t have native array types like PostgreSQL

### MySQL-Specific Features

We haven’t implemented`LOAD DATA INFILE` support yet.
### PostgreSQL-Specific Features

We haven’t implemented these yet:- `COPY`support
- `LISTEN`support
- `NOTIFY`support

- GSSAPI authentication
- `SCRAM-SHA-256-PLUS`support
- Point & PostGIS types
- All the multi-dimensional integer array types (only a couple of the types are supported)

## Common Patterns & Best Practices

### Working with MySQL Result Sets

### MySQL Error Handling

### Performance Tips for MySQL

- **Use connection pooling**: Set appropriate- `max`pool size based on your workload
- **Enable prepared statements**: They’re enabled by default and improve performance
- **Use transactions for bulk operations**: Group related queries in transactions
- **Index properly**: MySQL relies heavily on indexes for query performance
- **Use**: It’s set by default and handles all Unicode characters- `utf8mb4`charset

## Frequently Asked Questions

Why is this `Bun.sql` and not `Bun.postgres`?

Why is this `Bun.sql` and not `Bun.postgres`?

The plan was to add more database drivers. The unified API now supports PostgreSQL, MySQL, and SQLite.

How do I know which database adapter is being used?

How do I know which database adapter is being used?

The adapter is automatically detected from the connection string:

- URLs starting with `mysql://`or`mysql2://`use MySQL
- URLs matching SQLite patterns (`:memory:`,`sqlite://`,`file://`) use SQLite
- Everything else defaults to PostgreSQL

Are MySQL stored procedures supported?

Are MySQL stored procedures supported?

Yes, stored procedures are supported, including OUT parameters and multiple result sets:

Can I use MySQL-specific SQL syntax?

Can I use MySQL-specific SQL syntax?

Yes, you can use any MySQL-specific syntax:

## Why not just use an existing library?

You can use npm packages like postgres.js, pg, and node-postgres in Bun too. They’re great options. Two reasons why:- We think it’s simpler for developers to have a database driver built into Bun. The time you spend library shopping is time you could be building your app.
- We use some JavaScriptCore engine internals to create objects faster, in ways that would be difficult to implement in a library.

# Citations

1. Source page: https://bun.sh/docs/runtime/sql
