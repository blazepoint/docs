# Accounts

An account is a basic representation of a user's interaction with the Blazepoint network. A server MUST implement account registration and privacy controls.

An account is identified by its public key, which MUST also be it record id. Signatures, public key and encodings are done according to the [Schnorr signatures standard for the curve secp256k1](https://bips.xyz/340).

A client MUST store an account's full event history as well as synchronization state, etc. This may be persisted using a third-party service, or directly on the user's device, but in any case an account's data MUST be encrypted at rest using a password supplied by the user.

An account may specify `sharing` settings as follows:

- `none` means content should not be displayed except to mutual followers (account page is still public)
- `local` means content should not be gossiped to other servers
- `network` means content may be gossiped to the broader Blazepoint network

Server and client implementations SHOULD respect account sharing preferences.

## Record Schema

`{key: str, handle: str, avatar: url, description: str, sharing: none|local|network}`

## Events

- `account/create {handle, avatar, description, sharing}`
- `account/update {handle, avatar, description, sharing}`
- `account/delete {}`

Servers SHOULD honor account deletion requests. The purpose of this endpoint is to remove sensitive information from the system, not to re-write event history. Since Blazepoint implementations need to be able to handle missing data anyway, the simplest implementation here would be to delete all events and records published by the account, other than the account deletion request itself, which would be agressively propagated through the system (see ["server push"](/protocol/03_gossip.md#server-push) for more details).

# Instances

An `instance` identifies a client/account pair, e.g. "Jim's Desktop". An instance is automatically created when an account sends its first message with a given instance. Servers and clients MAY support renaming this instance, and displaying it where relevant.

## Record Schema

`{identifier: str, name: str}`

## Events

- `instance/update {identifier, name}`
