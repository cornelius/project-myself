# Project MySelf API Specification

This specification defines an API for storing private data on a server using
client-side encryption. It supports clients only writing data and clients
reading and synchronizing data. Data is separated into buckets on the server.
Each bucket contains a list of items. Access to buckets is controlled by client-
side generated and managed keys.

For more info about Project MySelf see the
[README](https://github.com/cornelius/project-myself/blob/master/README.md).

## Clients

There are two different types of clients, admin clients and user clients. Admin
clients can manage buckets and clients, but don't have access to the data. User
clients have access to the data, managed per bucket. Clients have to be
registered by other clients to gain access to the server. The first admin client
gains access to the server at setup time.

## Authentication

All access to the server requires authentication. The only exception is the
very first access, which gives access to the first admin client.

Clients authenticate by basic HTTP authentication using a client id and password
provided by the server on registration of the client.

There are two levels of access, admin access and user access.

## Return codes

The server uses standard HTTP return codes. The server returns 200 on success
and 400 on error. The body of an error response contains an error message as
text.

## Registering first admin client

`POST /admin/register/<pin>` _(no authentication)_

### Parameters

`pin`: four-digit identification number, obtained from operator of server

### Return

Admin client id and password

### Description

On startup the server prints out the URL of the call required to register the
first admin client, so that the operator of the server can transfer it to the
location, where the first admin client is ran. The call includes a PIN, which
makes the call unique.

The first call to a freshly started server needs to be the admin client
registration call including the correct PIN. All other calls are ignored until
an admin client is registered. If an incorrect PIN is provided the server is
shut down.
