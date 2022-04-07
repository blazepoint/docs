To avoid transplanting the value of one server's users into another server's community, feeds should only include votes published directly to it (path = 1), rather than inherited from other servers.

## `/search`

A global search endpoint. Accepts a POST with a JSON payload:

Schema: `{query: str, only: [accounts|posts|topics|comments|groups]?}`

## `/feed`

A websocket endpoint that retrieves posts and comments relevant to the current user. Users may specify a list of weights to incorporate into recommendations based on past interactions.

Schema: `{after: str?, weights: {[account_id]: {count: int, score: float}}}`

