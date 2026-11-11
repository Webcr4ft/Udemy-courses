# WebSocket vs. HTTP

> **TL;DR:** **HTTP** is request-response: the client asks, the server answers, and the server can't speak first. **WebSocket** keeps one connection open so both sides can send messages at any time. Use HTTP for normal pages and APIs. Use WebSocket for live, real-time features.

---

## The Simple Idea

Both are ways for a client (like a browser or phone app) to talk to a server. The difference is **who can start the conversation and how long the connection stays open**.

| | HTTP | WebSocket |
|---|---|---|
| **Think of it as** | Sending letters or texts one at a time | A phone call that stays connected |
| **Analogy** | You ask a question, get an answer, and hang up. To ask again, you must call again. | You call once and stay on the line. Either person can speak whenever they want. |
| **Who starts messages** | Only the client | Both client and server |

---

## What Is HTTP?

HTTP (Hypertext Transfer Protocol) is the protocol used for communication between a client, such as a web browser, and a server.

### How it works

1. The client sends an HTTP request.
2. The server processes the request and returns an HTTP response.
3. If the client needs more data, it sends another request.

### Key points

- The client always starts the communication.
- The server only responds. It cannot send data on its own.
- Commonly used to load web pages and exchange data through APIs.

**Example:** A news website periodically asks the server for the latest headlines. This repeated asking is called **HTTP polling**.

```
Client  --- request --->  Server
Client  <-- response ---  Server

Client  --- request --->  Server   (asks again later)
Client  <-- response ---  Server
```

---

## What Is WebSocket?

WebSocket is a communication protocol that lets a client and server exchange data over a **persistent connection**. Once connected, both sides can send messages independently, without creating a new HTTP request for each message.

### How it works

1. The client starts a WebSocket **handshake** with the server.
2. After the handshake succeeds, both sides can send messages over the open connection.
3. The connection stays open until either side closes it or it is interrupted.

### Key points

- Supports two-way (bidirectional) communication.
- The connection stays open instead of reconnecting each time.
- Useful for apps that need frequent, real-time data exchange.

**Example:** A chat app. The server pushes new messages to connected clients almost instantly, without the clients repeatedly asking "any new messages?"

```
Client  --- handshake --->  Server
Client  <-- accepted -----  Server

Client  <--> messages <-->  Server   (either side, any time)
Client  <--> messages <-->  Server

Connection stays open until closed
```

---

## Main Differences, One by One

### 1. Communication
- **HTTP:** request-response. One side asks, the other answers.
- **WebSocket:** bidirectional. Both sides can send messages.

### 2. Connection
- **HTTP:** connections can be reused, but the pattern is still request then response.
- **WebSocket:** one persistent connection that stays open.

### 3. Who sends data
- **HTTP:** the client starts every exchange.
- **WebSocket:** both sides can send messages.

### 4. Server updates
- **HTTP:** the server can't push updates by itself. You need polling or other techniques.
- **WebSocket:** the server can send messages directly to the client.

### 5. Overhead
- **HTTP:** repeated requests add overhead, because each one carries headers.
- **WebSocket:** lower overhead for frequent messages, because the connection is already open.

### 6. Real-time support
- **HTTP:** needs extra techniques for frequent updates.
- **WebSocket:** built for real-time communication.

### 7. Data types
- **HTTP:** JSON, HTML, files, and more.
- **WebSocket:** text and binary data.

### 8. Protocol
- **HTTP:** HTTP / HTTPS
- **WebSocket:** WS / WSS

### 9. Connection setup
- **HTTP:** a simple request and response.
- **WebSocket:** a handshake, followed by WebSocket communication.

### 10. Resource usage
- **HTTP:** depends on request patterns and connection reuse.
- **WebSocket:** needs resources to keep every connection open.

### 11. Security
- **HTTP:** HTTPS encrypts communication.
- **WebSocket:** WSS encrypts communication.

### 12. Common uses
- **HTTP:** web pages, REST APIs, file transfers.
- **WebSocket:** chat apps, online games, live notifications.

---

## Quick Comparison Table

| Feature | HTTP | WebSocket |
|---|---|---|
| **Communication** | Request-response | Bidirectional |
| **Connection** | Connections can be reused | Persistent connection |
| **Data transfer** | Client initiates requests | Both sides send messages |
| **Server updates** | Requires polling or other techniques | Direct server-to-client messages |
| **Overhead** | Repeated requests add overhead | Lower for frequent messages |
| **Real-time support** | Needs extra techniques | Supports real-time communication |
| **Data types** | JSON, HTML, files, and more | Text and binary data |
| **Protocol** | HTTP / HTTPS | WS / WSS |
| **Connection setup** | Request and response | Handshake, then WebSocket |
| **Resource usage** | Depends on request patterns | Resources needed for open connections |
| **Security** | HTTPS | WSS |
| **Common uses** | Web pages, REST APIs, file transfers | Chat apps, online games, live notifications |

---

## Important Points the Article Leaves Out

- **WebSocket starts as HTTP.** The handshake is an HTTP request with an "Upgrade: websocket" header. If the server agrees, the connection switches from HTTP to WebSocket on the same connection.
- **Other options exist for live updates.**
  - **Short polling:** the client asks every few seconds. Simple but wasteful.
  - **Long polling:** the client asks, and the server holds the request open until it has news. Better, but still repeated.
  - **Server-Sent Events (SSE):** the server pushes updates to the client over HTTP, one direction only. Good for live feeds and notifications.
  - **WebSocket:** two-way, best when both sides send often.
- **If you only need server to client updates, SSE may be simpler.** It works over normal HTTP, reconnects automatically, and is easier to scale.
- **Scaling WebSocket is harder.** Every connected user holds an open connection. With 100,000 users, that is 100,000 open connections, which uses memory and needs careful server design.
- **Load balancers need sticky behavior or shared state.** If a user connects to server A and a message arrives at server B, B must know how to reach that user. Teams often use Redis Pub/Sub or a message broker to connect servers. This links back to your earlier topics on load balancers and Pub-Sub.
- **WebSocket connections can drop.** Mobile networks, sleeping phones, and proxies can cut them. Good apps add **heartbeats** (small "ping/pong" messages) and **automatic reconnection**.
- **HTTP is stateless, WebSocket is stateful.** Each HTTP request stands alone. A WebSocket connection remembers who is connected, so the server has to manage that state.
- **Caching works with HTTP, not WebSocket.** Browsers and CDNs cache HTTP responses well. WebSocket messages are not cached.
- **HTTP tooling is richer.** Status codes, REST conventions, and standard tools make HTTP easier to test and debug. WebSocket gives you free-form messages, so you design your own message format.
- **Authentication needs care.** Authenticate during the handshake (for example with a token), and re-check when needed, because the connection can live for hours.
- **Always use WSS in production**, just as you use HTTPS, so data is encrypted.
- **HTTP/2 and HTTP/3 reduce the gap.** They allow many requests over one connection and are faster than old HTTP/1.1, but they are still request-response by design.
- **Don't use WebSocket for everything.** If an app loads data once or updates rarely, plain HTTP is simpler, cheaper, and easier to scale.

---

## When to Use Which

### Use HTTP when
- You are loading web pages.
- You are building REST APIs.
- You are transferring files.
- Data is requested occasionally, not constantly.
- You want simple, cacheable, easy-to-scale communication.

### Use WebSocket when
- You need live chat or messaging.
- You are building online games.
- You need live notifications or collaborative editing.
- You need live dashboards, stock prices, or sports scores.
- Both client and server need to send data frequently.

### Use both when
- Your app loads pages and normal data over HTTP, and uses a WebSocket only for the live parts, such as the chat window or notification feed. This is how most real apps work.

---

## Real-Life Examples

| Application | Likely choice | Why |
|---|---|---|
| Loading a blog or news article | HTTP | One request, one response |
| Login and signup | HTTP | Occasional, simple request-response |
| File download | HTTP | Standard transfer |
| WhatsApp-style chat | WebSocket | Instant two-way messages |
| Multiplayer game | WebSocket | Constant fast updates both ways |
| Live trading prices | WebSocket (or SSE) | Frequent server updates |
| Delivery tracking map | WebSocket or SSE | Server pushes location updates |
| Notification bell | SSE or WebSocket | Server pushes events |

---

## Final Summary

| Question | Answer |
|---|---|
| Client asks, server answers, now and then? | **HTTP** |
| Server needs to push data instantly? | **WebSocket** (or SSE) |
| Both sides send often in real time? | **WebSocket** |
| Server-to-client updates only? | **SSE** may be simpler |
| Not sure? | Start with **HTTP** |
| Large real app? | Usually **both** |
