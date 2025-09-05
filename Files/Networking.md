# Networking

#### 1. What are the valid top level types of JSON ?

<details>
  <summary>Solution</summary>

**Short answer:** JSON values can be any valid JSON *value* — object, array, string, number, boolean or null. In practice APIs most commonly use **object (dictionary)** or **array** as the top-level value.

**Details**

* According to the JSON specification, a JSON *text* is a serialized JSON *value*. A *value* can be:

  * Object (e.g. `{"name":"Alice"}`)
  * Array (e.g. `[1,2,3]`)
  * String (e.g. `"hello"`)
  * Number (e.g. `42` or `3.14`)
  * Boolean (`true` / `false`)
  * `null`

* **Why people say "object or array":** many REST APIs and libraries expect an object or array at the top-level because it maps cleanly to records or lists. Some tools, middleware or older parsers historically assumed object/array and rejected other fragments.

* **iOS specifics:**

  * `JSONSerialization.jsonObject(with:options:)` will parse top-level *objects* and *arrays* by default. To accept top-level strings/numbers/booleans/null you must pass `.allowFragments`.
  * `JSONDecoder` / `Codable` can decode any top-level JSON value as long as your `Decodable` target matches the JSON structure (e.g., you can decode into `String`, `Int`, `[MyModel]`, or `MyModel`).

**Examples**

```json
// object
{ "id": 1, "name": "Alice" }

// array
[{"id":1}, {"id":2}]

// string (valid JSON value)
"just a string"

// number (valid JSON value)
123
```

**Best practice:** Use an object (`{ ... }`) for responses that represent resources and arrays (`[ ... ]`) for collections — it's the most interoperable and clear approach for APIs.

</details>

#### 2. What is HTTP?

<details>
  <summary>Solution</summary>

**Short answer:** HTTP (Hypertext Transfer Protocol) is an application-layer protocol used for client/server communication on the web. In iOS apps the client (the app) issues HTTP requests to servers (APIs) and receives HTTP responses.

**Detailed explanation**

* **Request/response model:** a client sends a request (method, URL, headers, optional body) and the server returns a response (status code, headers, optional body).
* **Stateless:** each request is independent; servers shouldn’t rely on previous requests to hold conversation state (state can be layered with cookies, tokens, etc.).
* **Common HTTP elements**

  * **URL** (scheme `http`/`https`) — identifies resource.
  * **Method** — action to perform (GET, POST, etc.).
  * **Headers** — metadata (e.g., `Content-Type`, `Accept`, `Authorization`).
  * **Status code** — numeric result from server (200, 404, 500, …).
  * **Body** — payload (JSON, HTML, binary, etc.).
* **Security & transport**

  * Use **HTTPS** (HTTP over TLS) to encrypt traffic and protect integrity.
  * TLS certificates, OCSP, and proper TLS configuration are important for production apps.

**Example (curl):**

```bash
curl -X GET "https://api.example.com/users/1" -H "Accept: application/json"
```

**iOS usage**

* Use `URLSession` (or third-party libraries) to make requests.
* Example (simplified):

```swift
let url = URL(string: "https://api.example.com/users/1")!
let task = URLSession.shared.dataTask(with: url) { data, response, error in
  // handle response
}
task.resume()
```

**Notes**

* HTTP versions matter: HTTP/1.1 is widespread; HTTP/2 improves performance (multiplexing); HTTP/3 (QUIC) is emerging.
* Design APIs carefully around semantics (idempotency, caching, versioning).

</details>

#### 3. What is `CRUD` ?

<details>
  <summary>Solution</summary>

**Short answer:** CRUD stands for **Create, Read, Update, Delete** — the basic operations you perform on persistent data.

**Detailed explanation & mapping to HTTP**

* **Create** → usually mapped to `POST` (server creates a new resource). Response often `201 Created` with `Location` header pointing to new resource.
* **Read** → `GET` (retrieve a resource or list). Safe (does not modify server state) and typically cacheable.
* **Update** → `PUT` or `PATCH`:

  * `PUT` — replace a resource (idempotent: repeating the same `PUT` has same effect).
  * `PATCH` — apply partial modifications (not necessarily idempotent unless designed so).
* **Delete** → `DELETE` — remove resource (should be idempotent: deleting an already-deleted resource returns same result).

**Example**

* `POST /items` → create item
* `GET /items/123` → read item 123
* `PUT /items/123` → update/replace item 123
* `DELETE /items/123` → delete item 123

**Practical considerations**

* Idempotency: repeated `PUT`/`DELETE` calls should not produce unexpected side-effects.
* Use appropriate status codes (201, 200, 204, 400, 404, etc.) to convey result.

</details>

#### 4. Name some HTTP methods ?

<details>
  <summary>Solution</summary>

**Common HTTP methods**

* `GET` — retrieve resource (safe).
* `POST` — submit data/create resource.
* `PUT` — replace resource (idempotent).
* `PATCH` — partially update resource.
* `DELETE` — remove resource.
* `HEAD` — like `GET` but only headers (no body).
* `OPTIONS` — query server-supported methods or CORS preflight.
* `CONNECT`, `TRACE` — less commonly used; for proxies/debugging.

**Notes**

* Some people mistakenly use `UPDATE`; **there is no standard `UPDATE` method** — use `PUT` or `PATCH`.
* Choose methods according to semantics (e.g., `GET` should not change server state).
* Be mindful of caching rules that differ per method (`GET` responses can be cached).

</details>

#### 5. What is a status code and give an example ?

<details>
  <summary>Solution</summary>

**Short answer:** A status code is a 3-digit number sent in an HTTP response that indicates the result of the request. It groups responses into classes and gives clients a quick, standard way to determine success or the type of error.

**Status code classes**

* **1xx** Informational — provisional responses.
* **2xx** Success — request succeeded (e.g., `200 OK`, `201 Created`, `204 No Content`).
* **3xx** Redirection — further action is needed (e.g., `301 Moved Permanently`, `302 Found`).
* **4xx** Client error — the request is invalid or unauthorized (e.g., `400 Bad Request`, `401 Unauthorized`, `403 Forbidden`, `404 Not Found`, `429 Too Many Requests`).
* **5xx** Server error — server failed to fulfill a valid request (e.g., `500 Internal Server Error`, `503 Service Unavailable`).

**Example**

* `500 Internal Server Error` — indicates an unexpected server failure; something went wrong on server side (not the client’s request).

**iOS tip**

* When using `URLSession`, the response can be cast to `HTTPURLResponse` and you can check `statusCode`:

```swift
if let http = response as? HTTPURLResponse {
  switch http.statusCode {
  case 200:
    // OK
  case 401:
    // handle auth
  default:
    // other handling
  }
}
```

**Best practice:** Use the most specific and meaningful status code to help clients react correctly.

</details>

#### 6. What are two ways in which web content is formatted for delivery to a client ?

<details>
  <summary>Solution</summary>

**Two common formats**

* **JSON (JavaScript Object Notation)** — lightweight, human-readable, modern standard for REST APIs. Easy to parse and map to objects (`Codable` in Swift).
* **XML (eXtensible Markup Language)** — older, more verbose; used in SOAP APIs and some legacy systems. Supports schemas, namespaces and more complex document structures.

**Other formats to be aware of**

* **HTML** — web pages.
* **Protocol buffers (protobuf)**, **MessagePack**, **CBOR** — binary formats used for efficiency/gRPC or constrained environments.
* **CSV**, **Plain text**, **multipart/form-data** for file uploads.

**Why JSON is popular**

* Simpler and lighter than XML.
* Native mapping to common data structures (objects/arrays).
* Good ecosystem support in mobile/web languages.

**Example Content-Type headers**

* `Content-Type: application/json`
* `Content-Type: application/xml`

</details>

#### 7. Explain RESTFul APIs ?

<details>
  <summary>Solution</summary>

**Short definition:** REST (Representational State Transfer) is an architectural style for designing networked applications that treat server-side data as *resources* accessed and manipulated using a uniform interface (HTTP verbs).

**Core principles / constraints of REST**

* **Client–Server:** separation of concerns (UI and data/services).
* **Stateless:** each request contains all necessary information; server does not rely on stored session state.
* **Cacheable:** server responses must indicate cacheability to improve performance.
* **Uniform Interface:** consistent way to interact with resources (use of HTTP methods, resource URIs, standard status codes).
* **Layered System:** architecture can be composed of layers (proxies, gateways) without clients needing to know.
* **Code on demand (optional):** servers can send executable code (rare in practice).

**Typical REST practices**

* **Resources** expressed as URIs (e.g., `/users/123/orders`).
* **Use HTTP verbs**: `GET` to retrieve, `POST` to create, `PUT/PATCH` to update, `DELETE` to remove.
* **Use status codes** to indicate outcome (200, 201, 204, 400, 404, 500).
* **Stateless authentication** via tokens (Bearer JWT) or API keys.
* **Hypermedia (HATEOAS)** — including links in responses is REST’s purist idea, but it’s infrequently implemented in many APIs.

**Practical considerations**

* **Versioning:** include API version in the URL (`/v1/…`) or headers to manage breaking changes.
* **Pagination & filtering:** for list endpoints use paging params (`?page=2&limit=50`) or cursor-based pagination.
* **Idempotency:** design endpoints so that safe/idempotent properties are clear (repeating `PUT` should be safe).
* **Documentation:** provide clear docs and examples (OpenAPI/Swagger).

**Example**

```
POST /v1/users            -> create user (201 Created)
GET  /v1/users/123        -> read user 123 (200 OK)
PUT  /v1/users/123        -> replace user 123 (200/204)
PATCH /v1/users/123       -> modify user 123 (200/204)
DELETE /v1/users/123      -> delete user 123 (204 No Content)
```

</details>

#### 8. What is a MIME type ?

<details>
  <summary>Solution</summary>

**Short answer:** MIME type (aka media type or Content-Type) is a standardized identifier that describes the format of a resource’s data so clients and servers know how to interpret it.

**Format**

* `type/subtype` with optional parameters (e.g., `application/json; charset=utf-8`).

**Common examples**

* `application/json` — JSON payloads
* `text/html; charset=utf-8` — HTML pages
* `image/png` — PNG images
* `multipart/form-data; boundary=...` — file upload forms
* `application/octet-stream` — generic binary data
* vendor-specific: `application/vnd.mycompany.v1+json`

**HTTP headers**

* **`Content-Type`** — the MIME type of the response or request body.
* **`Accept`** — what formats the client can accept (e.g., `Accept: application/json`).

**iOS usage**

```swift
var request = URLRequest(url: url)
request.setValue("application/json", forHTTPHeaderField: "Content-Type")
request.setValue("application/json", forHTTPHeaderField: "Accept")
```

**Why it matters:** Proper MIME types enable correct parsing, rendering and negotiation between client and server.

</details>

#### 9. What's the difference between Websockets and HTTP ?

<details>
  <summary>Solution</summary>

**High-level difference**

* **HTTP** is a stateless, request–response protocol: client initiates request and server responds, connection may be short-lived (especially with HTTP/1.1).
* **WebSocket** is a protocol providing **persistent, full-duplex, bi-directional** communication over a single TCP connection. After an initial HTTP-based handshake, both client and server can send messages at any time.

**Key points**

* **Handshake:** A WebSocket connection starts with an HTTP `Upgrade` request; if the server accepts, the connection is upgraded and a persistent socket is established (`ws://` or secure `wss://`).
* **Directionality:** HTTP — client → server; server → client only as a response. WebSocket — both can push messages spontaneously.
* **Use cases:** real-time chat, live feeds (stocks/sports), multiplayer games, collaborative editing, telemetry.
* **Alternatives:** Long polling (client repeatedly polls), Server-Sent Events (SSE — server-to-client one-way streaming), gRPC streaming.
* **Security:** Use `wss://` for encrypted WebSocket (TLS). Ensure authentication and authorization for sockets too.

**iOS example (URLSessionWebSocketTask)**

```swift
let url = URL(string: "wss://example.com/socket")!
let task = URLSession.shared.webSocketTask(with: url)
task.resume()

// receive
task.receive { result in
  switch result {
  case .failure(let err): print("WS receive error", err)
  case .success(.string(let text)): print("Got text:", text)
  case .success(.data(let data)):  print("Got data:", data)
  @unknown default: break
  }
}

// send
task.send(.string("hello")) { error in
  if let e = error { print("Send error", e) }
}
```

**When to choose:**

* Use **HTTP (REST/HTTP2)** for standard request/response APIs, caching, and when server push is not necessary.
* Use **WebSocket** for low-latency, real-time two-way communication where the server needs to push data frequently.

**Practical caveats**

* Persistent connections consume resources (sockets) on servers; plan scaling and reconnection strategies.
* Implement heartbeat/ping-pong and reconnect backoff for robustness.

</details>
