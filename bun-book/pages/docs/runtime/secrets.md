---
type: Web Page
title: Secrets - Bun
description: Use Bun's Secrets API to store and retrieve sensitive credentials securely
resource: https://bun.sh/docs/runtime/secrets
timestamp: '2026-07-09T12:17:04.216670+00:00'
---

index.ts

## Overview

`Bun.secrets` provides a cross-platform API for managing sensitive credentials that CLI tools and development applications typically store in plaintext files like `~/.npmrc`, `~/.aws/credentials`, or `.env`. It uses:
- **macOS**: Keychain Services
- **Linux**: libsecret (GNOME Keyring, KWallet, and other secret service daemons)
- **Windows**: Windows Credential Manager

This API is mostly useful for local development tools. We may later add a 

`provider` option for production deployment
secrets.## API

`Bun.secrets.get(options)`

Retrieve a stored credential.
**Parameters:**

- `options.service`(string, required) - The service or application name
- `options.name`(string, required) - The username or account identifier

**Returns:**

- `Promise<string | null>`- The stored password, or- `null`if not found

`Bun.secrets.set(options)`

Store or update a credential.
**Parameters:**

- `options.service`(string, required) - The service or application name
- `options.name`(string, required) - The username or account identifier
- `options.value`(string, required) - The password or secret to store

**Notes:**

- If a credential already exists for the given service/name combination, it is replaced
- The stored value is encrypted by the operating system

`Bun.secrets.delete(options)`

Delete a stored credential.
**Parameters:**

- `options.service`(string, required) - The service or application name
- `options.name`(string, required) - The username or account identifier

**Returns:**

- `Promise<boolean>`-- `true`if a credential was deleted,- `false`if not found

## Examples

### Storing CLI Tool Credentials

### Migrating from Plaintext Config Files

### Error Handling

### Updating Credentials

## Platform Behavior

### macOS (Keychain)

- Credentials are stored in the user’s login keychain
- The keychain may prompt for access permission on first use
- Credentials persist across system restarts
- Accessible by the user who stored them

### Linux (libsecret)

- Requires a secret service daemon such as GNOME Keyring or KWallet
- Credentials are stored in the default collection
- May prompt for unlock if the keyring is locked
- The secret service must be running

### Windows (Credential Manager)

- Credentials are stored in Windows Credential Manager
- Visible in Control Panel → Credential Manager → Windows Credentials
- Persisted with the `CRED_PERSIST_ENTERPRISE`flag, so they’re scoped per user
- Encrypted using Windows Data Protection API

## Security Considerations

- **Encryption**: Credentials are encrypted by the operating system’s credential manager
- **Access Control**: Only the user who stored the credential can retrieve it
- **No Plain Text**: Passwords are never stored in plain text
- **Memory Safety**: Bun zeros out password memory after use
- **Process Isolation**: Credentials are isolated per user account

## Limitations

- Maximum password length varies by platform (typically 2048-4096 bytes)
- Keep `service`and`name`reasonably short (under 256 characters)
- Some special characters may need escaping depending on the platform
- Requires appropriate system services:
- Linux: Secret service daemon must be running
- macOS: Keychain Access must be available
- Windows: Credential Manager service must be enabled
 

## Comparison with Environment Variables

Unlike environment variables,`Bun.secrets`:
- ✅ Encrypts credentials at rest (thanks to the operating system)
- ✅ Avoids exposing secrets in process memory dumps (memory is zeroed after it’s no longer needed)
- ✅ Survives application restarts
- ✅ Can be updated without restarting the application
- ✅ Provides user-level access control
- ❌ Requires OS credential service
- ❌ Not very useful for deployment secrets (use environment variables in production)

## Best Practices

- 
**Use descriptive service names**: Match the tool or application name If you’re building a CLI for external use, use a UTI (Uniform Type Identifier) for the service name.
- 
**Credentials-only**: Don’t store application configuration in this API This API is slow; keep non-secret settings in a config file.
- 
**Use for local development tools**:- ✅ CLI tools (gh, npm, docker, kubectl)
- ✅ Local development servers
- ✅ Personal API keys for testing
- ❌ Production servers (use proper secret management)

# Citations

1. Source page: https://bun.sh/docs/runtime/secrets
