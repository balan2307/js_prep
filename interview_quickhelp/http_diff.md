# HTTP/1.1 vs HTTP/2 vs HTTP/3

## HTTP/1.1
- One request at a time per TCP connection
- Browser opens up to **6 parallel TCP connections** per domain for parallelism
- Each connection = expensive (TCP handshake + TLS handshake)
- Headers are **plain text, repeated on every request** (wasteful)
- Head-of-line blocking — slow request blocks everything behind it

## HTTP/2
- **Multiplexing** — multiple requests in parallel over a single TCP connection
- **HPACK header compression** — headers sent once, only diffs sent after
- Binary framing — faster to parse than plain text
- Stream prioritization
- ❌ Still has **TCP head-of-line blocking** — one lost packet blocks all streams
- ❌ Slow connection setup — TCP + TLS = 2-3 round trips
- ❌ Connection tied to IP — mobile network switch kills connection

## HTTP/3
- Replaces TCP with **QUIC** (built on UDP)
- **No head-of-line blocking** — lost packet only blocks its own stream
- **0-RTT / 1-RTT** — TLS built into QUIC, data flows almost immediately
- **Connection migration** — uses connection ID, survives IP changes (WiFi → mobile)

## Summary Table

| | HTTP/1.1 | HTTP/2 | HTTP/3 |
|---|---|---|---|
| Parallelism | Multiple TCP conns | Multiplexing / 1 TCP | Multiplexing / QUIC |
| HOL Blocking | Per connection | TCP level | None |
| Headers | Plain text, repeated | Compressed (HPACK) | Compressed |
| Setup Speed | Slow (per conn) | 2-3 RTT | 0-1 RTT |
| Mobile | Poor | Okay | Great |

## Key Insight
> HTTP/2 solved HTTP-level HOL blocking but TCP itself still blocked.  
> HTTP/3 solved this by replacing TCP with QUIC — reliability per stream, not per connection.
