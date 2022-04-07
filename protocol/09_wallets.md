Use https://webln.dev/#/

# Wallets

Schema: `{type: bitcoin, address: str}`

## Creation

Topic: `wallet/create`
Payload: `{type: bitcoin, address: str}`

## Delete

Topic: `wallet/delete`
Payload: `{str: str}`


# Monetization

Identities may specify a bitcoin wallet in order to receive payments for content produced.

Payments are federated rather than truly decentralized; node operators collect micropayments from clients (batched to keep transaction fees low) based on content served and use that liquidity to cover costs and pay content producers. This has the effect of throttling social media addiction, because you're paying actual money. This payment strategy should have a free tier, and cost different amounts based on serving heavy content or requesting unavailable content.

Payouts should be disbursed on a delayed time frame, based on replication votes for a given identity.
