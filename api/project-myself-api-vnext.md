# Project MySelf API Specification

This specification defines an HTTP API for storing private data on a server
using client-side encryption. It supports clients only writing data and clients
reading and synchronizing data. Data is separated into buckets on the server.
Each bucket contains a list of items. Access to buckets is controlled by client-
side generated and managed keys.

The scope of this specification is the communication between clients and server
via JSON over HTTP. It includes the outer format of the actual data, which is
carrying the user data as payload in encrypted form. The inner format of the
data is opaque to the client-server API. To make it possible for clients to
interoperate there is a separate specification of the full [data
format](https://github.com/cornelius/project-myself/blob/master/project-myself-data-format-vnext.md).

For more info about Project MySelf see the
[README](https://github.com/cornelius/project-myself/blob/master/README.md).

## Clients

There are two different types of clients, admin clients and user clients. Admin
clients can manage buckets and clients, but don't have access to the data. User
clients have access to the data, managed per bucket. Clients have to be
registered by other clients to gain access to the server. The first admin client
gains access to the server at setup time.

## Authentication

All access to the server requires authentication. The only exceptions are the
calls to register new clients.

Clients authenticate by basic HTTP authentication using a client id and password
provided by the server on registration of the client.

There are two levels of access, admin access and user access.

## Return codes

The server uses standard HTTP return codes. The server returns 200 on success
and 400 on error. The body of an error response contains an error message as
text.



## Admin


### Registering first admin client

`POST /admin/register/<pin>` _(no authentication)_

#### Parameters

`pin`: four-digit identification number, obtained from operator of server

#### Return

Admin client id and password

#### Description

On startup the server prints out the URL of the call required to register the
first admin client, so that the operator of the server can transfer it to the
location, where the first admin client is ran. The call includes a PIN, which
makes the call unique.

The first call to a freshly started server needs to be the admin client
registration call including the correct PIN. All other calls are ignored until
an admin client is registered. If an incorrect PIN is provided the server is
shut down.


### List all clients

`GET /admin/clients` _(admin authentication)_

#### Return

List of all registered clients including if they are an admin or user client


### Remove access for client

`DELETE /admin/clients/<client-id>` _(admin authentication)_

#### Parameters

`client-id`: Id of client whose access should be removed


### List all registration tokens

`GET /admin/tokens` _(admin authentication)_

#### Return

List of all tokens including if they have been used or not


### Delete token

`DELETE /admin/tokens/<token>` _(admin authentication)_

#### Parameter

`token`: Token to be deleted


### List buckets

`GET /admin/buckets` _(admin authentication)_

#### Return

List of all buckets which exist on the server including some information about
each bucket


### Delete bucket

`DELETE /admin/buckets/<bucket-id` _(admin authentication)_

#### Paremeter

`bucket-id`: Id of bucket to be deleted



## Register client


### Create registration token

`POST /token` _(admin or user authentication)_

#### Return

Registration token

#### Description

A client can register other clients by creating a token and passing this token
to the other client. The other client then has to use the `Register new user
client` call to get access to the server.

Tokens should expire after 15 minutes.

All registration tokens can be tracked via the admin API.


### Register new user client

`POST /register/<token>` _(no authentication)_

#### Parameter

`token`: Token obtained by `Create registration token` call

#### Return

User client id and password

#### Description

The client is registered and gets the credentials it has to use for
authentication in subsequent calls.

All registered clients can be tracked via the admin API.



## Server status

### Ping

`GET /ping` _(user authentication)_

#### Return

`pong`



## Bucket creation

### Create new bucket

`POST /data` _(user authentication)_

#### Return

Id of new bucket



## Access bucket items


### Create new item

`POST /data/<bucket-id>` _(user authentication)_

#### Parameters

`bucket-id` Id of bucket to which data should be written

#### Body

Encrypted content (inner item) wrapped in a container handled by the server
(outer item).

```json
{
  "id": <item-id>,
  "parent-id": <item-id>,
  "content": <encrypted content>
}
```

#### Return

Id of created item


### Get all items from a bucket

`GET /data/<bucket-id>` _(user authentication)_

#### Parameters

`bucket-id`: Id of bucket from which data is read

#### Return

List of all items of a bucket:

```json
[
  {
    "id": ...
    ...
  },
  {
    "id": ...
    ...
  }
]
```


### Get new items from a bucket

`GET /data/<bucket-id>?parent=<parent-id>` _(user authentication)_

#### Parameters

`parent-id`: Id of parent item of the first item to read

#### Return

List of all items following item identified by `parent-id`:

```json
[
  {
    "id": <item-id>,
    "parent-id": <parent-id>,
    ...
  },
  {
    "id": ...
    "parent-id": <item-id>
    ...
  }
]
```


### Get one item from a bucket

`GET /data/<bucket-id>/<item-id>` _(user authentication)_

#### Parameters

`bucket-id`: Id of bucket from which data is read
`item-id`: Id of item which is read

#### Return

Requested item

```json
{
  "id": <item-id>,
  "parent-id": <item-id>,
  "content": <encrypted content>
}
```
