# Subscriptions

A subscription registers an account's interest in following an account or topic.

## Record Schema

`{record_type: account|topic, record_id: uuid}`

## Events

- `subscription/create {gid, record_type, record_id}`
- `subscription/delete {gid}`
