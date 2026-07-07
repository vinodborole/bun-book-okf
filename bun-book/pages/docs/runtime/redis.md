---
type: Web Page
title: Redis - Bun
description: Use Bun's native Redis client with a Promise-based API
resource: https://bun.sh/docs/runtime/redis
timestamp: '2026-07-07T10:59:41.879776+00:00'
---

Bun’s Redis client supports Redis server versions 7.2 and up.

redis.ts

## Getting Started

To use the Redis client, you first need to create a connection:redis.ts

- `REDIS_URL`
- `VALKEY_URL`
- If not set, defaults to `"redis://localhost:6379"`

### Connection Lifecycle

The Redis client automatically handles connections in the background:redis.ts

redis.ts

## Basic Operations

### String Operations

redis.ts

### Numeric Operations

redis.ts

### Hash Operations

redis.ts

### Set Operations

redis.ts

## Pub/Sub

Bun provides native bindings for the Redis Pub/Sub protocol, added in Bun 1.2.23.### Basic Usage

Create a publisher in`publisher.ts`:
publisher.ts

`subscriber.ts`:
subscriber.ts

terminal

terminal

Subscribing takes over the 

`RedisClient` connection: a client with
subscriptions can only call `RedisClient.prototype.subscribe()`. To send other
commands to Redis, create a separate connection with `.duplicate()`:redis.ts

### Publishing

Publish messages with the`publish()` method:
redis.ts

### Subscriptions

Subscribe to channels with the`.subscribe()` method:
redis.ts

`.unsubscribe()` method:
redis.ts

## Advanced Usage

### Command Execution and Pipelining

The client automatically pipelines commands, improving performance by sending multiple commands in a batch and processing responses as they arrive.redis.ts

`enableAutoPipelining` option to `false`:
redis.ts

### Raw Commands

Use the`send` method to run any Redis command, including ones without a dedicated method. The first argument is the command name, and the second is an array of string arguments.
redis.ts

### Connection Events

You can register handlers for connection events:redis.ts

### Connection Status and Monitoring

redis.ts

### Type Conversion

The client automatically converts Redis responses to JavaScript values:- Integer responses are returned as JavaScript numbers
- Bulk strings are returned as JavaScript strings
- Simple strings are returned as JavaScript strings
- Null bulk strings are returned as `null`
- Array responses are returned as JavaScript arrays
- Error responses throw JavaScript errors with appropriate error codes
- Boolean responses (RESP3) are returned as JavaScript booleans
- Map responses (RESP3) are returned as JavaScript objects
- Set responses (RESP3) are returned as JavaScript arrays

- `EXISTS`returns a boolean instead of a number (1 becomes true, 0 becomes false)
- `SISMEMBER`returns a boolean (1 becomes true, 0 becomes false)

- `AUTH`
- `INFO`
- `QUIT`
- `EXEC`
- `MULTI`
- `WATCH`
- `SCRIPT`
- `SELECT`
- `CLUSTER`
- `DISCARD`
- `UNWATCH`
- `PIPELINE`
- `SUBSCRIBE`
- `UNSUBSCRIBE`
- `UNPSUBSCRIBE`

## Connection Options

When creating a client, you can pass options to configure the connection:redis.ts

### Reconnection Behavior

When a connection is lost, the client automatically attempts to reconnect with exponential backoff:- The client starts with a small delay (50ms) and doubles it with each attempt
- Reconnection delay is capped at 2000ms (2 seconds)
- The client attempts to reconnect up to `maxRetries`times (default: 10)
- Commands executed during disconnection are:
- Queued if `enableOfflineQueue`is true (default)
- Rejected immediately if `enableOfflineQueue`is false
 
- Queued if 

## Supported URL Formats

The Redis client supports various URL formats:redis.ts

## Error Handling

The Redis client throws typed errors for different scenarios:redis.ts

- `ERR_REDIS_CONNECTION_CLOSED`- Connection to the server was closed
- `ERR_REDIS_AUTHENTICATION_FAILED`- Failed to authenticate with the server
- `ERR_REDIS_INVALID_RESPONSE`- Received an invalid response from the server

## Example Use Cases

### Caching

redis.ts

### Rate Limiting

redis.ts

### Session Storage

redis.ts

## Implementation Notes

Bun’s Redis client is implemented in Rust and uses the Redis Serialization Protocol (RESP3). It reconnects automatically with exponential backoff and pipelines commands, so multiple commands can be sent without waiting for replies to previous ones.## Limitations and Future Plans

Limitations we plan to address in future versions:- Transactions (MULTI/EXEC) must be done through raw commands

- Redis Sentinel
- Redis Cluster

# Citations

1. Source page: https://bun.sh/docs/runtime/redis
