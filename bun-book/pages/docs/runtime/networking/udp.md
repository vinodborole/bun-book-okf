---
type: Web Page
title: UDP - Bun
description: Use Bun's UDP API to implement services with advanced real-time requirements,
  such as voice chat.
resource: https://bun.sh/docs/runtime/networking/udp
timestamp: '2026-07-07T10:59:41.879776+00:00'
---

## Bind a UDP socket (`Bun.udpSocket()`)

To create a new (bound) UDP socket:
### Send a datagram

Specify the data to send, the destination port, and the destination address.`send` does not perform DNS
resolution, as it is intended for low-latency operations.
### Receive datagrams

When creating your socket, add a`data` callback to handle incoming packets:
server.ts

### Connections

UDP has no concept of a connection, but many UDP exchanges (especially as a client) involve only one peer. In that case you can connect the socket to that peer, which sets the destination address for every packet you send and restricts incoming packets to that peer.server.ts

### Send many packets at once using `sendMany()`

To send a large volume of packets without the overhead of a system call for each, batch them with `sendMany()`.
For an unconnected socket, `sendMany` takes an array as its only argument. Each set of three array elements describes a packet:
the data to send, the target port, and the target address.
server.ts

`sendMany` takes an array where each element is the data to send to the peer.
server.ts

`sendMany` returns the number of packets successfully sent. Like `send`, it only takes valid IP addresses
as destinations and does not perform DNS resolution.
### Handle backpressure

A packet you send may not fit into the operating system’s packet buffer. You can detect that this has happened when:- `send`returns- `false`
- `sendMany`returns a number smaller than the number of packets you specified. In this case, Bun calls the- `drain`socket handler once the socket becomes writable again:

### Socket options

UDP sockets support setting various socket options:### Multicast

Bun supports multicast operations for UDP sockets. Use`addMembership` and `dropMembership` to join and leave multicast groups:
`addSourceSpecificMembership` and `dropSourceSpecificMembership`:

# Citations

1. Source page: https://bun.sh/docs/runtime/networking/udp
