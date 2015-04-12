# Project MySelf Data Format Specification

This document specifies the data format used by clients of Project MySelf. Data
is stored on a server using the HTTP API, the format of the content is defined
here. This includes the format of the encrypted payload data.

## General structure

Data is organized in buckets. Each bucket contains a series of items, which
contain data. The item consists of an outer format, which is readable by client
and server, and an inner format, which is used by the encrypted payload content.
The inner format is only readable by the client.

## Outer format

The outer format is a container for the actual data. The purpose of the outer
format is to provide minimal structure to facilitate syncing of data between
client and server. It doesn't contain any information related to the content of
the actual data.

Here is an example for an item:

```json
{
  "item_id": "319231477",
  "parent_id": "580188393",
  "content": "-----BEGIN PGP MESSAGE-----\nVersion: GnuPG v2.0.22 (GNU/Linux)\n\
njA0EAwMCZVm4mqXSrK/TySPF9uKEzklUMHh3q9UC4MqhdcOW02H/bnmtzrCvXy0h\n6/bLdQ==\n=Gpol\n-----END PGP MESSAGE-----\n"
}
```

The `item_id` identifies the item within the bucket. Its value is an unique id.
The item id is used as an argument when retrieving items via the API.

The `parent_id` contains the `item_id` of the previous item, which was written
to the bucket, where the items are stored. If the item is the first one in the
bucket, the value is an empty string `""`. This linkage establishes a
well-defined order of the items.

Having the items linked by the `parent_id` entries allows to establish an
effective syncing protocol between client and server to only transmit data,
which is not present on the client yet.

Items are never deleted. If their data should be changed or removed, this is
done by writing a new item with new data or a command to treat the data of the
old item as removed.

The `content` contains the encrypted user data. The format of the encrypted
content is described in the section about the inner format.

## Inner format

The inner format specifies the format of the content of the encrypted data. How
the data is encrypted is up to the client.

Information needed to decrypt the data is stored by the client in a ticket.
There is a separate ticket for each bucket. The ticket needs to be exchanged
between clients in a secure way, if they should access the same bucket. Content
of and exchange of tickets are not part of this specification.

### New data

An item will commonly contain a new data item. Here is an example of the inner
item of such an entry:

```json
{
  "id": "a72b0ed3dd34bf735490e8291a01cdef",
  "written_at": "2015-04-12T19:33:59Z",
  "data": "42"
}
```

The `id` is a unique id for the inner item. It is not related to the `item_id`
of the outer format.

`written_at` contains the date and time when the item was written. It is
formatted according to ISO and times should be in UTC.

`data` contains the actual content of the item. This can be a simple value or
more structured data. Its interpretation is up to the client.

### Modifiying data

Items written to the server are immutable. They are never deleted or modified.
If the data stored as the payload should be changed a new item has to be
written containing the information, which overrides the old value.

If previous items (defined by the linkage of the outer item) with the same id
(from the inner item) exist, their data is overwritten with the data of the last
item with the same id.

If the value of the inner item is empty `""`, the item is considered to be
deleted.

### Tags

Items can be tagged to mark them as special data. Clients interprete data
depending on the tag.

An example for a tagged data item is:

```json
{
  "id": "0c72547e2ff1ca6f66435d0c0b9fe298",
  "written_at": "2015-04-12T19:53:29Z",
  "tag": "title",
  "data": "Example title"
}
```

This example sets the title of the data in the bucket to "Example title".
The `title` tag is the only tag right which has a defined meaning in the
current specification.

Tagged data can be modified by writing a new item with a new value for the given
tag. The latest value for a given tag in the linked list of outer items defines
the current value of the tag.
