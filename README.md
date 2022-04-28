Blazepoint is a protocol for building interoperable decentralized social media systems.

Blazepoint does not use blockchains, p2p networking, or DHTs. Instead, it combines the concept of cryptographic identity with a simple client/server gossip protocol to align network topology with the topology of the actual social network.

If we're successful, the result will be a system that supports community-driven moderation, content monetization through the lightning network, and safe communities that do not become echo chambers.

# Features

- Anonymity and cryptographic identity, multi-device support
- Topics, @mentions, follow/mute/block, likes, comments
- Multiple front-ends, server discovery
- Edits (via annotation rather than update-in-place)
- Search engine accessibility
- Private messages; private and public groups; marketplaces
- Monetization for content creators and node operators

# How it works

The basic idea is that clients are in control of network replication, while servers do the heavy lifting. Clients indicate content that they think should be replicated from one server to the next by "voting" for it (this may be automatically inferred by client software from upvotes, follows, replies, etc). That vote then gets published to all servers the client is connected to who then request that content from all known peers and publish it to their followers.

It's important that Blazepoint is only a protocol; multiple implementations should be able to exist and interoperate. This makes it possible not only to make different design decisions, but also to support multiple social media use cases using a single cryptographic identity. As I see it, there are four main use cases for social media:

- Forums are content-focused and support deeply nested, long-running conversations. An example of this is something like Reddit.
- Groups are more event-focused; groups of people cohere not by common interest, but by occasional participation in a real-life or virtual event. Facebook has gotten this one right.
- Chat is ideally for very small groups of friends, ideally no more than 10. Chat is not a good default interface for large community participation, since it is not searchable or browsable by topic, and it is unstructured, causing multiple conversations to interleave confusingly.
- Marketplaces have no need for social cohesion, except maybe geographical or topical. Facebook and Craigslist get this right.

Twitter is a hybrid between forums, groups, and chat, but sacrifices focus for reach. Blazepoint will never be able to compete on reach, since Twitter's scale does require a centralized architecture, but it should bring a much better balance between focus and reach.

# Prior Art

Blazepoint is similar to [ActivityPub](https://www.w3.org/TR/activitypub/), but takes node operators out of the driver's seat. Replication is not a firehose, it is implemented selectively by listening to user recommendations. On Mastodon, the "network" tab is worthless because content there has nothing to do with the instance you're on. At the same time, the instance you're using doesn't publish content from other instances you're interested in. It's the worst mix of Reddit and Twitter.

Blazepoint is similar to [Scuttlebutt](https://ssbc.github.io/scuttlebutt-protocol-guide/), but is less technically sophisticated. This is a good thing, because many of the limitations of Scuttlebutt (lack of multi-device support, content deletion, intensive storage requirements) come from its technical sophistication.

Blazepoint is most similar to [Nostr](https://github.com/fiatjaf/nostr). I agree with almost everything said on their readme about the problems with current social networks and how the solution should work. The two projects are similar enough that I hope they can be merged, or at least learn from one another over time.

The difference is worth explaining. One way to articulate it is that Blazepoint includes a more robust (I think) vision for moderation, content replication, and monetization. A more theoretical explanation is that while Nostr is meant to be a naive event-stream protocol, where social is just one application of many (see Citadel Dispatch #63), Blazepoint is a social protocol akin to ActivityPub. It is designed to support many different social use-cases, but makes some trade-offs to support focus and scaling on that particular domain.

The implication of this is that Blazepoint could in theory be built on top of Nostr as a second-layer. However, the Nostr spec makes few enough assumptions/prescriptions, that Blazepoint will likely profit from a more opinionated foundation.

# Notes

- [Blur/block questionable images](https://github.com/infinitered/nsfwjs)
- Spam
  - It's easy to create accounts; new accounts should have a probationary period.
  - It's easy to connect to many servers at once to broadcast stuff.
    - Servers should be able to control who they let subscribe vs contribute
  - A small fee for posting should exist to discourage spam
  - Prevent image uploads or links for new accounts
- Potentially implement multi-tenancy so spinning up a hosted node is easy
- Add marketplace where the node takes a deposit then disburses when the goods are delivered.
- Read endpoints should be implemented using websockets.
  - [Postgres Pub/Sub](https://webapp.io/blog/postgres-is-the-answer/)
- Server owner is exposed to liability due to content as well as commerce.
  - Potentially support messages with encrypted contents that other users can request access to
- Implement cap on users subscribed to or registered with (?) a server, to encourage many servers, lower incentive for various attacks.
- Read this on sockets: http://www.adama-lang.org/blog/woe-of-websocket/
- Charge for heavy/risky content. Text is free, links are risky, images and videos are risky and heavy. Text-only spam has a much higher cost/benefit ratio
- What happens if a client generates duplicate uuids for records? Can't just use a unique constraint because it may only get gossiped later.
- Figure out mnemonic seed words and derivation path, or at least how to share accounts between devices https://github.com/fiatjaf/nostr/blob/master/nips/06.md
- Users may attach a comment to a server and persist it to multiple competing servers. These comments can be interacted with as usual, and shown on a server profile page.
- More aggressive moderation can be implemented on a server basis by designating certain accounts as moderators who can block content or users with impunity for a given server.
- Multi-device user/password sign in
  - Any time after creating an account the user may specify a password for logging in from other devices.
  - The user's private key would be hashed with the password and a random salt and saved to their account.
  - An attempted sign in from another device would:
    - retrieve users that match the given user name
    - attempt to decrypt each match client-side so the password is not shared with the server
    - copy the private key to the new device
    - request a copy of all exiting user data
- What happens when a server receives events out of order?
- Do something like https://rsslay.fiatjaf.com/ to turn RSS feeds into accounts
- Use [DID](https://www.w3.org/TR/did-core/)s?
- Specify how a user can `server/join`
  - Invoicing workflow for up-front payment
  - Servers may ask peers for account reputation
  - Servers may require the account to have content already shared to it
  - Other conditions/workflows?
- Social key recovery
- https://t.me/nostr_protocol/13868
- Use https://p2panda.org/ as the base protocol
- Specify joining a server
- Don't get into Silk-Road-type trouble https://freeross.org/
- [Keep following private](https://twitter.com/doctorow/status/1494151525748424717)
