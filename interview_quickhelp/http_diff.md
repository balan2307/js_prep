# HTTP/1.1 vs HTTP/2 vs HTTP/3

## HTTP/1.1
- One request at a time per TCP connection — requests queue up sequentially
- To achieve parallelism, browser opens up to **6 TCP connections per domain** — but each connection is expensive (TCP + TLS handshake before any data flows)
- Headers are **plain text, repeated on every request** — cookies, user-agent, accept headers resent every time, wasteful
- **Head-of-line blocking** — within a connection, slow request blocks everything behind it
- Workarounds: concatenate JS, sprite images, domain sharding — all hacks around protocol limits

## HTTP/2
- **Multiplexing** — multiple requests truly parallel over a single TCP connection. No need for 6 connections or domain sharding hacks.
- **HPACK header compression** — headers sent once in full, only changed headers sent after. Saves significant bandwidth.
- Binary framing — data sent as binary (not plain text), faster to parse
- Stream prioritization — tell browser which resources matter more

### ❌ Still had problems:
- **TCP head-of-line blocking** — HTTP/2 runs multiple streams over one TCP connection. TCP guarantees ordered packet delivery at connection level. If one packet is lost, TCP halts ALL streams waiting for retransmission — even streams that don't need that packet. HTTP/2 can't fix this since it sits above TCP.
- **Slow connection setup** — TCP handshake + TLS handshake = 2-3 round trips before data flows
- **Connection tied to IP** — switch from WiFi to mobile → IP changes → TCP connection drops → everything restarts

## HTTP/3
- Replaces TCP with **QUIC** (built on UDP). QUIC reimplements reliability per stream, not per connection.
- **No head-of-line blocking** — lost packet only blocks its own stream, other streams keep flowing unaffected
- **0-RTT / 1-RTT connection setup** — TLS is built into QUIC. New connection = 1 round trip. Repeat visit = 0 round trips (reuses previous session keys). vs HTTP/2's 2-3 round trips.
- **Connection migration** — QUIC identifies connections by a connection ID, not IP+port. Switch from WiFi to mobile → IP changes → QUIC connection survives seamlessly

### Why QUIC is built on UDP and not a new TCP?
TCP is baked into OS kernels and network hardware worldwide — changing it takes decades. UDP is already everywhere. QUIC runs in userspace on top of UDP, so it can be updated like normal software.

## Summary Table

| | HTTP/1.1 | HTTP/2 | HTTP/3 |
|---|---|---|---|
| Parallelism | 6 TCP conns per domain | Multiplexing / 1 TCP | Multiplexing / QUIC |
| HOL Blocking | Per connection | TCP level (all streams) | None (per stream) |
| Headers | Plain text, repeated | Compressed (HPACK) | Compressed |
| Setup Speed | Slow (per conn) | 2-3 RTT | 0-1 RTT |
| Mobile | Poor | Okay | Great |

## Key Insight
> **Stream** = one request-response pair (e.g. GET /api/users).  
> **Packet** = small chunk of data TCP sends over the wire. Each packet carries a stream_id inside so HTTP/2 knows which stream it belongs to — but TCP doesn't look inside, it just sees sequence numbers.  
>
> HTTP/2 solved HTTP-level HOL blocking (no more 6 connections needed).  
> HTTP/3 solved TCP-level HOL blocking — QUIC tracks ordering per stream, not per connection.

## Interview Answer (1 min)
HTTP/1.1 handled one request per TCP connection, so browsers opened 6 parallel connections per domain — expensive. Headers repeated every request. Head-of-line blocking meant one slow request blocked everything.

HTTP/2 fixed parallelism with multiplexing over one TCP connection and HPACK header compression. But TCP head-of-line blocking remained — one lost packet halts all streams since TCP guarantees ordered delivery at connection level, not stream level.

HTTP/3 replaced TCP with QUIC (built on UDP). QUIC tracks reliability per stream — lost packet only blocks its own stream. TLS is built in, so connection setup drops to 0-1 RTT. And connections use IDs not IPs, so they survive network switches.
