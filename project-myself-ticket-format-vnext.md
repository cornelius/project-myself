# Project MySelf Ticket Format Specification

This document specifies the format of tickets used to access data in
[Project MySelf](https://github.com/cornelius/project-myself).

*This is work in progress. It documents a current state, but it will evolve.*

Tickets contain the information which is needed to encrypt and decrypt data on
the client. The server only sees encrypted data and can not access the content.
Tickets are stored on the client and need to be kept safe there. They can be
exchanged between clients to allow multiple clients to access the same data
shared through the server.

Data is organized in "buckets" on the server. Each bucket contains a series of
data items. Access is managed per bucket. Each ticket gives access to one
bucket.

A ticket looks like this:

```json
{
  "name": "My Data",
  "bucket_id": "hdze7dhded",
  "key": "DhdjjD7DJHJKJDKkjhze77=="
}
```

`name` contains a user-chosen name for the data in the bucket.

`bucket_id` is the id of the bucket on the server.

`key` is the key used to encrypt and decrypt the data. It has to be kept secret,
so the ticket should not be accessible by anyone else than the right user.
