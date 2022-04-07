# Posts

A post is the basic, standalone implementation of a piece of shared content. It is simply a blob of text, limited to 1024 characters. Content type may be `text` or [`gfm`](https://github.github.com/gfm/). Posts may be nested arbitrarily deep forum-style by including a reference to its parent.

Clients SHOULD implement rich text composition and rendering, including links, @-mentions, topics, inline images, videos, and site previews. Clients SHOULD render annotations alongside the original post, and hide the post and annotations if deleted. Servers MUST implement deletions as soft-deletions by setting the `deleted` timestamp and redacting the `body` of the post.

## Record

```
{
  body: str,
  content_type: str,
  deleted: timestamp,
  parent_type: post|account|server,
  parent_id: str,
}
```

## Events

- `post/create {gid, body, parent_type, parent_id}`
- `post/delete {gid}`

# Annotations

An annotation is an addendum to a post. This is specified in lieu of updates, to avoid confusing or malicious edits while providing a platform for the original author to draw attention to new information or corrections to the original post.

## Record Schema

`{post_id, body: str, content_type: str}`

## Events

- `annotation/create {gid, post_id, body, content_type}`

# Tags

Tags are strings prefixed by `#`, and SHOULD be extracted by clients from the body of posts, annotations, and account bodies and published.

## Record Schema

`{handle: str, record_type: post|annotation|account, record_id: uuid}`

## Events

- `tag/create {gid, handle, record_type, record_id}`

# Mentions

A mention is a reference to an account, including a mapping between handle (since they may be non-unique) and the account's public key. It is a client's responsibility to extract mentions from content and publish them.

Mentions SHOULD be implemented on the client by determining the mapping between an account's handle and public key at post composition time. The handle should be embedded in the post, and mapped to a key in the `mentions` list.

## Record Schema

`{handle: str, target: str, record_type: post|annotation|account, record_id: uuid}`

## Events

- `mention/create {gid, handle, target, record_type, record_id}`
