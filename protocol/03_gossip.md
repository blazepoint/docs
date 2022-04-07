# Gossip

Gossip in Blazepoint is primarily accomplished using client hints - that is, depending on how a user interacts with certain content, a client may publish replication requests to servers it is connected to. The idea here is that the network topology ought to match the topology of the actual social network. This enables interesting emergent behavior with regards to scaling and moderation.

Client hints are complemented by some direct collaboration between servers, allowing for more efficient use of bandwidth and access to more complete and up-to-date content (in contrast to e.g. Scuttlebutt). Servers SHOULD NOT gossip between themselves, except to fulfill client requests.

# Votes

A `vote` is a request by a user to replicate and otherwise prioritize certain content. Votes MAY be applied to any record type. Score is a float ranging from -1 to 1. If a user votes multiple times, the record MUST be upserted. Clients MAY choose what constitutes a vote; they may have explicit vote functionality, or implement votes based on likes, follows, blocks, etc.

If content is voted up, that vote is shared with all of a client's connected servers. Those servers SHOULD then make an effort to acquire that content and its context (related posts, comments, accounts) and serve that content to its own clients as appropriate.

Content's overall score SHOULD affect how likely it and content from the originating account is to be recommended to other users in `feeds`. Negative scores SHOULD outweigh positive scores, so clients should be careful to only register a down vote in the case of block/mute type actions rather than mere downvotes. The goal is to prioritize content reach rather than appeal via community-driven moderation.

## Record Schema

`{record_type: str, record_id: str, score: float}`

## Events

- `vote/create {gid, record_id, record_type, score}`
- `vote/delete {gid}`

# Servers

A client MAY connect with one or more servers (ideally two or more). Clients MAY allow a user to specify whether a server should be published to, or only read from. Clients SHOULD make a good effort to publish events to all publishable servers, queueing them up to be re-tried in case of network or server failure.

Servers SHOULD implement support for peer gossip (otherwise a server would only be able to share content directly published with it). This support MUST be indicated at `/info` by including `gossip: "all"` in the features object.

When a client connects to a new server, it SHOULD notify it of other known servers in the network using the `server/recommend` topic. The server MUST then request the full server info from each recommended server's `/info` endpoint to fill in name and description. A server's record should have no id, and MUST be identified by its `url`. If a server moves, a new server record is created.

Servers MUST publish a list of servers via the `/request` endpoint as record type `server`, but MAY omit peers from the list at their discretion.

Whenever a server receives an event it SHOULD check for missing context. This can be done either eagerly, or on demand when the content is requested. Which strategy is implemented depends on the use case and size of data. Context may include either basic account data, or topic-specific context like comments, votes, etc. If any context is missing, the server SHOULD request it from known peers using the `/request` endpoint.

On occasion, servers MAY push events to other servers directly using the `/publish` endpoint, for example when propagating an `account/delete` request. Receiving servers SHOULD validate the `Authorization` header and compare the provided public key to the final key in each event's `path`, rejecting events that do not have a topic that should be aggressively gossiped, or that did not originate with the sender.

Servers MAY delete infrequently requested content to keep storage low, or eagerly replicate popular content. To allow for batching and high cardinality broadcasting, eager replication should be implemented asynchronously.

## Record Schema

`{url: str, name: str, description: str}`

## Events

- `server/recommend {servers: [{url}]}`
