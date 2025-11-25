# Mappa delle Fonti per Tesi: WebSocket Moderno

## RFC 6455 - WebSocket Protocol Specification

### RFC 6455 - The WebSocket Protocol (Official Standard)
**Ente**: IETF  
**Anno**: 2011 (published), 2024+ (maintained)  
**Tipo**: RFC Standard ufficiale  
**Link**: https://datatracker.ietf.org/doc/html/rfc6455  
**Affidabilità**: ⭐⭐⭐⭐⭐ Massima (IETF Standard)

**Takeaway chiave**:
- WebSocket Protocol enables **two-way, full-duplex communication** over single TCP connection
- HTTP upgrade mechanism: initial HTTP handshake, then protocol switch (101 Switching Protocols)
- Handshake headers: `Upgrade: websocket`, `Connection: Upgrade`, `Sec-WebSocket-Key`, `Sec-WebSocket-Version: 13`
- Server response: `Sec-WebSocket-Accept` derived from client key + magic string SHA-1 hash
- Frames protocol: minimal overhead, supports text and binary messages
- **Designed for interactive messaging** traffic patterns (unlike standard HTTP)
- Design works over HTTP ports 80/443 (ws:// and wss://), supports proxies/firewalls
- Stateful connection maintained by both client and server

**Applicabilità tesi**: 
- Fonte primaria WebSocket protocol, handshake mechanism, frame format, RFC base[1][2][3][4][5][265]

---

### WebSocket Protocol Overview (Tech Reference)
**Fonti**: RFC 6455 detailed docs, InterSystems IRIS documentation  
**Anno**: 2025  
**Tipo**: Technical documentation  
**Link**: https://docs.intersystems.com/irisforhealthlatest/csp/docbook/DocBook.UI.Page.cls?KEY=AWEBSOCKETS  
**Affidabilità**: ⭐⭐⭐⭐ Alta

**Takeaway chiave**:
- WebSocket handshake phase: client sends HTTP GET upgrade request, server sends 101 response
- Protocol upgrade: HTTP (http://) → WebSocket (ws://) after successful handshake
- Full-duplex channel enables concurrent independent communication (client ↔ server)
- Minimal framing per message (low overhead)
- WebSocket server class implementation: OnPreServer(), Server(), OnPostServer() callbacks
- Connection states: 0 (not established), 1 (established, communication possible), 2 (closing handshake), 3 (closed)
- SharedConnection property: dedicated connection vs shared connection pool (async WebSocket)
- Binary data support: BinaryData property for non-UTF-8 encoded frames

**Applicabilità tesi**: 
- Implementation details WebSocket, server lifecycle, connection states[265]

---

## WebSocket vs SSE vs HTTP/2 Push Comparison

### WebSockets vs Server-Sent Events: Key Differences
**Fonte**: Ably, FreeCodeCamp  
**Anno**: 2025  
**Tipo**: Technical comparison guide  
**Link**: https://ably.com/blog/websockets-vs-sse  
**Affidabilità**: ⭐⭐⭐⭐ Alta (Real-time platform expertise)

**Comparison Matrix**:

| Feature | WebSockets | Server-Sent Events (SSE) |
|---------|-----------|------------------------|
| **Communication** | Full-duplex (two-way) | One-way (server→client only) |
| **Data types** | Binary + UTF-8 text | UTF-8 text only |
| **Connections per browser** | Limited by server resources | Limited by browser (6 max) |
| **Polyfillable** | No (needs WebSocket fallback) | Yes (JavaScript polyfill) |
| **Enterprise firewalls** | May have issues (custom protocol) | Seamless (built on HTTP) |
| **Use cases** | Chat, gaming, collaboration, multiplayer | Stock tickers, news updates, read-only real-time |

**When use WebSockets**:
- Multiplayer collaboration (shared state)
- Chat applications (bidirectional messaging)
- Real-time games (low latency, high bandwidth)
- Cases where upward communication essential

**When use SSE**:
- One-way server→client streaming (simpler)
- Read-only dashboards (stocks, metrics)
- News/notification feeds
- When firewall compatibility critical
- **Can extend with AJAX for upward channel** (REST POST)

**Performance note**: WebSockets perform best in simulated scenarios, but SSE simpler for read-only use cases[266][142]

---

### HTTP/2 vs WebSockets: Complementary, Not Replacement
**Fonte**: OpenIllumi, Stack Overflow  
**Anno**: 2025  
**Tipo**: Technical analysis  
**Link**: https://openillumi.com/en/en-http2-websockets-distinction/  
**Affidabilità**: ⭐⭐⭐⭐ Alta

**Takeaway chiave**:
- **Misconception**: HTTP/2 Server Push makes WebSockets obsolete
- **Reality**: They are complementary for different scenarios

**HTTP/2 features**:
- Multiplexing: multiple streams over single TCP connection
- Server Push: proactively send resources to client
- Binary protocol with minimal framing
- Similar to WebSocket benefits, but different purpose

**Key differences**:
- **HTTP/2 Push is NOT enforceable**: intermediaries (proxies, routers) can ignore pushes
- **HTTP/2 connections timeout**: close after inactivity (not designed long-lived)
- **WebSocket API exposure**: browser JavaScript can use for real-time bidirectional communication
- **HTTP/2 for resource loading**: Server Push optimizes asset delivery, NOT application real-time communication

**Current limitation**: HTTP/2 lacks browser JavaScript API for direct binary data frame consumption
- For UTF-8 text streaming: use SSE (not HTTP/2 Push)
- For bidirectional real-time: WebSocket necessary

**Conclusion**: HTTP/2 + SSE can handle some WebSocket use cases, but WebSockets remain necessary for:
- Binary data push from server to JS client
- Bidirectional real-time communication
- Interactive messaging patterns[267][268]

---

## WebSocket Scalability & Architecture

### WebSocket Architecture: Sticky Sessions vs Sharding
**Fonte**: Ably  
**Anno**: 2025  
**Tipo**: Architecture best practices  
**Link**: https://ably.com/topic/websocket-architecture-best-practices  
**Affidabilità**: ⭐⭐⭐⭐ Alta (Real-time platform production)

**Takeaway chiave**:

**1. Sticky Sessions** (session affinity):
- Load balancer tracks client → same server per reconnection
- Client doesn't need to know sharding scheme
- **Pros**: Easy horizontal scale, session continuity, simple for clients
- **Cons**: Load balancer single point of failure, poor load distribution if unbalanced, state sharing overhead

**2. Sharding** (explicit partitioning):
- Clients assigned to specific shard/server partition
- Deterministic routing: hash(client_id) → shard
- **Pros**: Decentralized (no central load balancer dependency), balanced load
- **Cons**: Complex client-side routing logic, resharding expensive when scaling

**Best practice**: Hybrid approach
- Non-sticky sessions with state sharing (Redis/shared backend)
- Allows flexible client migration, survives server failures
- Load balancer doesn't become bottleneck
- Client reconnects to any available server (state recoverable)

**Comparison pros/cons**:

| Aspect | Sticky | Sharding | Hybrid |
|--------|--------|----------|--------|
| Scalability | Easy add/remove | Need reshard | Easy (stateless) |
| Load distribution | Can be poor | Balanced | Balanced |
| SPOF | Load balancer | None | None |
| Complexity | Low | High | Medium |
| State sharing | Optional | Implicit | Explicit |

---

### WebSocket Scaling at Production Scale
**Fonte**: Websocket.org  
**Anno**: 2024  
**Tipo**: Best practices guide  
**Link**: https://websocket.org/guides/websockets-at-scale/  
**Affidabilität**: ⭐⭐⭐⭐ Alta

**Takeaway chiave**:
- Non-sticky load balancing preferred (despite misconception WebSockets require sticky)
- Implement shared connection state layer (Redis, Memcached)
- Allows server failure recovery without client awareness
- Stateful long-lived connections require careful orchestration
- Horizontal scaling needs pub/sub messaging layer (Redis, Kafka, NATS)
- Connection management: track active connections per server, circuit breakers

---

### Kafka as WebSocket Backbone
**Fonte**: Ably  
**Anno**: 2024  
**Tipo**: Architecture pattern  
**Link**: https://ably.com/blog/scaling-kafka-with-websockets  
**Affidabilität**: ⭐⭐⭐⭐ Alta

**Takeaway chiave**:
- **Problem**: Kafka designed for high throughput data, not connection management
- **Kafka limitations**: 2000 concurrent connections per broker typical
- **Solution**: Decouple WebSocket connection layer from Kafka data processing

**Architecture**:
```
Clients ← WebSocket Layer (Connection Management) ← Kafka (Data Throughput)
```

**Example**: 50,000+ concurrent connections
- Naive approach: 25+ Kafka brokers (2k connections per broker)
- Smart approach: 5 Kafka brokers (for data throughput) + WebSocket tier (for connections)
- **Result**: 5x cost reduction, better resource utilization

**Benefits**:
- **Full-duplex communication**: WebSocket bidirectional
- **Single long-lived connections**: reduces setup/teardown costs
- **Independent scaling**: WebSocket tier scales connections, Kafka scales throughput
- **Complex subscriptions**: pub/sub model aligns client interests with data routing

**Example use case**: Gaming leaderboard
- Players submit scores → Kafka score-ingestion topic
- Aggregation service updates scores, pushes leaderboard → Kafka leaderboard-update topic
- WebSocket tier delivers leaderboard updates to subscribed players
- 50k+ players simultaneously, Kafka backend handles throughput

---

## WebSocket Backpressure & Flow Control

### Backpressure Management in WebSockets
**Fonte**: Skyline Codes, Haroldadmin  
**Anno**: 2024-2025  
**Tipo**: Advanced patterns guide  
**Link**: https://skylinecodes.substack.com/p/backpressure-in-websocket-streams  
**Affidabilität**: ⭐⭐⭐⭐ Alta

**Takeaway chiave**:
- **Problem**: Asynchronous non-blocking I/O can queue data faster than network transmits
- **Root cause**: `ws.send()` returns instantly (copied to OS TCP send buffer), but network transmission is slow
- **Speed mismatch**: server CPU fast, network bandwidth limited, client processing slowest
- **Result**: data accumulation in buffers, memory pressure, potential crash

**Detection**:
- **Client-side**: `WebSocket.bufferedAmount` property (bytes queued, not yet sent)
  - Monitor threshold (e.g., >64KB indicates backpressure)
  - Throttle outgoing messages if exceeded
- **Server-side**: 
  - `ws.send()` return value: `false` indicates TCP buffer full
  - `drain` event (Node.js `ws` library): buffer space available again
  - Monitor per-client buffer size

**Code Example** (Node.js with `ws`):
```javascript
const MAX_BUFFER = 1024 * 1024; // 1MB per client
let isPaused = false;

function sendWithBackpressure(data) {
  if (ws.readyState === WebSocket.OPEN) {
    const ok = ws.send(data);
    if (!ok) {
      // TCP buffer full, pause sending
      isPaused = true;
      console.warn('Backpressure detected, pausing');
    }
  }
}

// Resume when drain event fires
ws._socket.on('drain', () => {
  isPaused = false;
  console.log('Buffer drained, resuming sends');
  // Resume processing messages from queue
});
```

**Management strategies**:
1. **Control producer rate** (ideal): slow down server when client can't keep up
2. **Reactive Streams**: explicit flow control (request N items)
3. **Application-level queue**: buffer messages per client, pause when threshold exceeded
4. **WebSocketStream (experimental)**: browser Streams API with built-in backpressure
5. **Bounded buffering + dropping**: drop old messages when buffer full (not recommended for critical data)

**Applicabilità tesi**: 
- Production WebSocket reliability, backpressure handling, scalability patterns[269][270][271][272][273][275]

---

## WebSocket Security

### WebSocket Authentication Best Practices
**Fonte**: Ably, Heroku, MDN, Invicti  
**Anno**: 2024-2025  
**Tipo**: Security guides  
**Link**: https://ably.com/blog/websocket-authentication  
**Affidabilità**: ⭐⭐⭐⭐ Alta

**Takeaway chiave**:

**Authentication Challenge**:
- WebSocket handshake uses HTTP (100% compatible)
- But browser WebSocket API **doesn't allow arbitrary HTTP headers** (including `Authorization`)
- HTTP cookies ARE sent automatically (but CSRF risks)

**Authentication Options**:

**1. Cookies** (automatic):
- Sent with WebSocket upgrade handshake
- **Pro**: Simple, automatic
- **Con**: CSRF vulnerability if not protected, limited to same-site

**2. URL Query Parameters** (?token=xyz):
- Token in connection URL
- **Pro**: Works with all clients
- **Con**: Visible in logs, URL history, security risk

**3. Sec-WebSocket-Protocol Header** (workaround):
- Client specifies token in `protocols` array
- `new WebSocket(url, ["token_here"])`
- **Pro**: Clever workaround, uses subprotocol field
- **Con**: May get logged plaintext, not standardized use

**4. In-band authentication** (post-connection message):
- Authenticate AFTER connection established
- First message contains credentials
- **Pro**: Flexible token refresh, can send new token later
- **Con**: Window of unauthenticated connection briefly

**5. Authorization Header (via reverse proxy)**:
- Reverse proxy extracts header, adds cookie before upstream
- **Pro**: Transparent to browser client
- **Con**: Requires proxy infrastructure

**Best practice**:
- Combine: handshake origin + cookies + in-band token validation
- Enable token refresh mechanism (resend token after expiry)
- Validate `Origin` header for CSRF protection (but spoof-able by non-browser clients)

---

### WebSocket Origin Header Validation
**Fonte**: Heroku, MDN, MDN Developers  
**Anno**: 2024-2025  
**Tipo**: Security documentation  
**Link**: https://devcenter.heroku.com/articles/websocket-security  
**Affidabilität**: ⭐⭐⭐⭐ Alta

**Takeaway chiave**:
- **Origin header**: Sent by ALL browsers with WebSocket handshake
- **Purpose**: Identify source of WebSocket request for CSRF prevention
- **Limitations**: 
  - Non-browser clients can easily spoof
  - Not reliable standalone for authentication

**Protection against Cross-Site WebSocket Hijacking (CSWH)**:
```javascript
// Server-side validation
const origin = request.headers['origin'];
const allowedOrigins = ['https://example.com', 'https://app.example.com'];

if (!allowedOrigins.includes(origin)) {
  response.writeHead(403, 'Forbidden');
  response.end();
}
```

**Combined with authentication**:
- Origin header checks (advisory, CSRF prevention)
- Cookie/token validation (true authentication)
- Per-message authorization checks (GraphQL field-level auth)

---

### WebSocket Security Best Practices Checklist (OWASP)
**Fonte**: Invicti, OWASP, MDN  
**Anno**: 2024-2025  
**Tipo**: Security checklist  
**Link**: https://www.invicti.com/blog/web-security/websocket-security-best-practices  
**Affidabilität**: ⭐⭐⭐⭐ Alta

**Checklist**:

✅ **Always use wss:// (WebSocket Secure)** - TLS encryption in transit  
✅ **Validate Origin header** - CSRF prevention (check allowlist)  
✅ **Authenticate at handshake** - Cookies or token in URL/header  
✅ **Per-message authorization** - Check permissions before processing  
✅ **Token refresh** - Expiry handling, in-band token refresh  
✅ **Rate limiting** - Messages per second per client  
✅ **Input validation** - Sanitize all received data (JSON parsing, injection prevention)  
✅ **Connection timeouts** - Idle connection closing, heartbeat monitoring  
✅ **Error handling** - Never leak sensitive info in error messages  
✅ **Logging & monitoring** - Track connection anomalies, failed auth attempts  

---

## Socket.IO - Practical WebSocket Implementation

### Socket.IO: Features and Reliability Layer
**Ente**: Socket.IO official  
**Anno**: 2024  
**Tipo**: Official documentation  
**Link**: https://socket.io/docs/v4/  
**Affidabilità**: ⭐⭐⭐⭐⭐ Massima (Official docs)

**Takeaway chiave**:
- **WebSocket wrapper** with reliability enhancements
- Open-source, Node.js implementation

**Features**:

**1. HTTP long-polling fallback**:
- Automatic fallback if WebSocket unavailable
- Handles misconfigured proxies, enterprise firewalls
- Critical when 97%+ browsers support WebSocket but edge cases exist

**2. Automatic reconnection**:
- Heartbeat mechanism: periodic connection health check
- Client auto-reconnects on disconnect with exponential backoff
- Prevents server overwhelm from simultaneous reconnection storms
- Example: retry delays 1s, 2s, 4s, 8s...

**3. Packet buffering**:
- Messages queued during disconnection
- Auto-sent upon reconnection
- Prevents message loss during temporary outages

**4. Acknowledgements** (RPC pattern):
```javascript
// Sender
socket.emit("hello", "world", (response) => {
  console.log(response); // "got it"
});

// Receiver
socket.on("hello", (arg, callback) => {
  console.log(arg); // "world"
  callback("got it");
});

// With timeout
socket.timeout(5000).emit("hello", "world", (err, response) => {
  if (err) console.log("No response in 5s");
});
```

**5. Multiplexing** (namespaces):
- Separate communication channels on same connection
- Example: `/chat`, `/notifications`, `/admin`

**Applicabilità tesi**: 
- Production WebSocket implementation, reliability patterns, reconnection strategies[281][284]

---

### Socket.IO Reconnection Strategy
**Fonte**: Socket.IO docs  
**Anno**: 2024  
**Tipo**: Official documentation  
**Link**: https://socket.io/how-to/build-a-basic-client  
**Affidabilità**: ⭐⭐⭐⭐⭐ Massima

**Takeaway chiave**:
- On connection close, automatically retry with delay
- Default exponential backoff: prevents thundering herd on server restart
- Example: initial delay 2s, then exponential
- Configurable: `reconnectionDelay`, `reconnectionDelayMax`

**Implementation**:
```javascript
#onClose(reason) {
  setTimeout(() => this.#open(), this.#opts.reconnectionDelay);
}

// With exponential backoff
reconnectionDelay = Math.min(
  2000 * Math.pow(2, attempt),
  30000 // max 30s
) + Math.random() * 1000; // jitter
```

---

## Apollo GraphQL Subscriptions over WebSocket

### GraphQL Subscriptions with Apollo Server
**Ente**: Apollo GraphQL  
**Anno**: 2025  
**Tipo**: Official documentation  
**Link**: https://www.apollographql.com/docs/react/data/subscriptions  
**Affidabilità**: ⭐⭐⭐⭐⭐ Massima (Official)

**Takeaway chiave**:
- GraphQL subscriptions over WebSocket for real-time server→client updates
- Apollo Server: built-in subscription support
- Apollo Client: `GraphQLWsLink` for WebSocket transport

**Server-side setup**:
```javascript
import { ApolloServer } from 'apollo-server';
import { createServer } from 'http';
import { SubscriptionServer } from 'subscriptions-transport-ws';

const server = new ApolloServer({ schema });
const httpServer = createServer();

server.listen().then(({ url }) => {
  new SubscriptionServer({
    execute,
    subscribe,
    schema,
  }, {
    server: httpServer,
    path: '/graphql',
  });
});
```

**Client-side setup**:
```javascript
import { GraphQLWsLink } from "@apollo/client/link/subscriptions";
import { createClient } from "graphql-ws";

const wsLink = new GraphQLWsLink(
  createClient({
    url: "ws://localhost:4000/subscriptions",
  })
);

const client = new ApolloClient({
  link: wsLink,
  cache: new InMemoryCache(),
});
```

**Subscription query**:
```graphql
subscription OnMessageSent {
  messageSent {
    id
    content
    author
  }
}
```

**Applicabilità tesi**: 
- GraphQL real-time pattern, Apollo subscriptions implementation[282][285]

---

### GraphQL WebSocket Integration
**Fonte**: VideoSDK  
**Anno**: 2024  
**Tipo**: Integration guide  
**Link**: https://www.videosdk.live/developer-hub/websocket/graphql-websocket  
**Affidabilità**: ⭐⭐⭐⭐ Alta

**Takeaway chiave**:
- `subscriptions-transport-ws` package for GraphQL + WebSocket
- Split link routing: subscriptions → WebSocket, queries/mutations → HTTP
- Front-end React with `useSubscription` hook

**Architecture**:
- Queries/Mutations: HTTP POST to Apollo Server
- Subscriptions: WebSocket to same server
- Apollo Client auto-routes based on operation type

---

## WebSocket Backpressure & Flow Control (Technical)

### Backpressure in WebSocket Streams (Flow Control)
**Fonte**: Haroldadmin, websockets library documentation  
**Anno**: 2024-2025  
**Tipo**: Advanced technical guides  
**Link**: https://blog.haroldadmin.com/posts/backpressure-with-websockets-and-web-streams  
**Affidabilität**: ⭐⭐⭐ Media-Alta

**Takeaway chiave**:
- **Native WebSocket API**: NO backpressure support (limitation)
- **Web Streams API**: Has built-in backpressure
- **Solution**: Combine WebSocketStream (experimental) with Streams API
  - `writer.write()` returns Promise that resolves when data sent
  - `await writer.write()` enforces backpressure
- **Current state**: WebSocketStream in draft, browser support limited
- **Workaround**: Application-level buffering + `bufferedAmount` monitoring

---

## When to Use WebSocket vs Alternatives

### Decision Framework: WebSocket vs Polling vs SSE

| Use Case | Best Choice | Rationale |
|----------|-------------|-----------|
| **Chat/messaging** | WebSocket | Bidirectional, low latency |
| **Gaming** | WebSocket | Continuous state sync, multiplayer |
| **Collaboration** | WebSocket | Real-time mutual updates |
| **Stock tickers** | SSE | One-way, simple, firewall-friendly |
| **News feeds** | SSE | One-way streaming, read-only |
| **Device status** | Polling + SSE | Polling simple, SSE for high-frequency |
| **Notifications** | SSE + push | One-way, retry automatic, push for mobile |
| **API updates** | REST polling | Simple, caching-friendly |
| **Low latency critical** | WebSocket | Minimum overhead |
| **Firewall strict** | SSE | HTTP-based, seamless passage |

**Polling**:
- ✅ Simple, stateless, easy cache
- ❌ High latency (polling interval), server load (frequent requests)
- Use: Low frequency updates, best-effort acceptable

**SSE (Server-Sent Events)**:
- ✅ One-way, simple, HTTP-based, automatic reconnect
- ❌ UTF-8 only, limited concurrent connections per browser
- Use: Read-only real-time streaming

**WebSocket**:
- ✅ Full-duplex, bidirectional, binary support, interactive messaging
- ❌ Stateful, complex backpressure, firewall issues, more overhead
- Use: Multiplayer, chat, collaborative editing, gaming

---

## Summary Affidabilità Fonti

| Categoria | Affidabilità | Fonti |
|-----------|--------------|-------|
| **RFC 6455** | ⭐⭐⭐⭐⭐ Massima | IETF standard |
| **Apollo Docs** | ⭐⭐⭐⭐⭐ Massima | Official vendor |
| **Socket.IO** | ⭐⭐⭐⭐⭐ Massima | Official library |
| **Real-time platforms** | ⭐⭐⭐⭐ Alta | Ably, production expertise |
| **Security docs** | ⭐⭐⭐⭐ Alta | Heroku, MDN, OWASP |
| **Architecture patterns** | ⭐⭐⭐⭐ Alta | Ably, Websocket.org |
| **Technical guides** | ⭐⭐⭐ Media-Alta | Engineering blogs |

---

## Note per Tesi

### Cosa coperto:
✅ RFC 6455 WebSocket protocol specification  
✅ WebSocket vs SSE vs HTTP/2 Push comparison  
✅ Sticky sessions vs sharding vs hybrid scalability  
✅ Kafka/NATS as WebSocket backbone  
✅ Backpressure flow control detection/management  
✅ Security: origin validation, authentication options, CSWH prevention  
✅ Socket.IO reliability patterns  
✅ Apollo GraphQL subscriptions  
✅ Decision framework: when WebSocket vs alternatives  

### Criticità WebSocket (balanced view):
- **Stateful complexity**: unlike REST statelessness
- **Backpressure subtle**: needs careful management
- **Authentication awkward**: browser API limitation
- **Firewall issues**: custom protocol may block
- **Memory intensive**: per-connection resources

### Quando WebSocket is overkill:
- Simple notifications (polling sufficient)
- One-way streaming (SSE better fit)
- Low-frequency updates (REST polling acceptable)
- High firewall restrictions (SSE more compatible)

### Quando WebSocket necessary:
✅ Bidirectional communication  
✅ Low-latency real-time required  
✅ Interactive multiplayer/chat  
✅ Continuous state synchronization  
✅ Binary data transmission  
