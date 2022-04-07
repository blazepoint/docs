A server MAY implement limits on user registration. This should be described in its policy as follows:

```
{
  registration: {approval: bool, limit: int?}
}
```

If `approval` is `true`, accounts will be unable to `publish` any messages subsequent to its initial creation until approved by the server. The server MUST return an error if denying a publish request with the code `account/status` and a helpful error message.

Regardless of whether `limit` is set (though it's a nice hint for targeted community size), a server MAY not allow an account to `publish` with the error code `account/denied`.
