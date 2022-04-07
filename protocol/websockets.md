Servers MAY implement a websocket version of the protocol, signalling policy support with `{websocket: "all"}`.

Clients can send 3 types of messages as follows:

- `["publish", event]`, used to publish events.
- `["request", query]`, used to request records and subscribe to new updates.
- `["close", id]`, used to stop previous subscriptions.

Servers can send 2 types of messages as follows:

- `["response", response]`, used to send responses requested by clients.
- `["error", error]`, used to send error messages or other things to clients.
