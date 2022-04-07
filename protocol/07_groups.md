# Groups

## Record Schema

`{name: str, private: bool}`

## Events

- `group/create {gid, name, private}`

# Memberships

## Record Schema

`{group_id: str, account_id: str}`

## Events

- `group/invite {gid: str, account_id: str}`
- `group/join {gid: str}`
- `group/leave {gid: str}`
