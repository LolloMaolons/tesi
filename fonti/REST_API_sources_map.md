# Mappa delle Fonti per Tesi: REST API Moderno

## Fondamenti Teorici REST

### Dissertazione di Roy Fielding - REST Architectural Style (2000)
**Autore**: Roy Thomas Fielding  
**Anno**: 2000  
**Tipo**: Tesi di dottorato (UC Irvine)  
**Link**: https://www.ics.uci.edu/~fielding/pubs/dissertation/fielding_dissertation.pdf  
**Affidabilità**: ⭐⭐⭐⭐⭐ Massima (Fonte originale, 11000+ citazioni)

**Takeaway chiave**:
- Definisce REST come stile architetturale per sistemi distribuiti di ipermedia, non come protocollo
- Enfatizza 6 vincoli: client-server, statelessness, cache, uniform interface, layered system, code-on-demand
- Giustificazione teorica per scalabilità, generalità, indipendenza deployment, latenza e sicurezza
- Base fondativa del Web moderno e HTTP/1.1 design
- **Critica essenziale**: Fielding distingue tra "REST-style" (effettivamente RESTful) e "REST API" (spesso RPC-style su HTTP)

**Applicabilità tesi**: 
- Sezione teorica fondamentale, source primario REST principles
- Differenza tra true REST (HATEOAS) vs pratica comune (CRUD+JSON)
- Principi guida scalabilità e evoluzione API[185][186][187]

---

### REST Architectural Style - Chapter 5 Fielding (Web version)
**Autore**: Roy Thomas Fielding  
**Anno**: 2000  
**Tipo**: Capitolo dissertazione online  
**Link**: https://www.ics.uci.edu/~fielding/pub\s/dissertation/rest_arch_style.htm  
**Affidabilità**: ⭐⭐⭐⭐⭐ Massima

**Takeaway chiave**:
- Confronto diretto con C2 architectural style (event-based, asynchronous)
- REST emphasizes: scalability component interactions, generality interfaces, independent deployment, intermediary components
- REST pull-style vs C2 nominally push-based asynchronous (WebSocket-like)
- REST manca supporto nativo state notifications (reason WebSocket per real-time)
- Constraints rationale: generic resource interface, stateless interactions, intrinsic caching support

**Applicabilità tesi**: 
- Fondamento comparazione REST vs WebSocket, event-driven patterns
- Spiegazione perché REST non adatto real-time, necessità WebSocket[186]

---

### REST Controversy: "Most RESTful APIs Aren't Really RESTful" (2025)
**Autore**: Florian Krämer  
**Anno**: 2025  
**Tipo**: Technical analysis  
**Link**: https://florian-kraemer.net/software-architecture/2025/07/07/Most-RESTful-APIs-are-not-really-RESTful.html  
**Affidabilità**: ⭐⭐⭐⭐ Alta (Industry expert, based on Fielding's work)

**Takeaway chiave**:
- **Misconcezione critica**: REST ≠ CRUD su HTTP con JSON (questa è RPC-style)
- Fielding's 2008 blog post: "If the engine of application state is not being driven by hypermedia, then it cannot be RESTful"
- 6 regole vere REST: protocol-independent, standard protocol evolution, generic resource interface, HATEOAS
- **Comune** misconcezioni: REST=CRUD, resource=entity, NO verbs in URIs - sono design decision, NON REST requirements
- **Pragmatismo**: seguire POST's Law ("Be conservative in what you do, liberal in what you accept")
- Consiglio: "Be pragmatic. Avoid dogmatists. Se API facile imparare e hard misuse, rest non importa"

**Applicabilità tesi**:
- Sezione critica su implementazione reale vs teoria, why HATEOAS non adopted
- Disconnection tra REST specs e industry practice (REST is CRUD, not hypermedia-driven)
- Balanced view pragmatismo vs purity[189]

---

### REST API Tutorial - What is REST (RESTfulAPI.net)
**Fonte**: RESTfulAPI.net  
**Anno**: 2025  
**Tipo**: Online tutorial  
**Link**: https://restfulapi.net  
**Affidabilità**: ⭐⭐⭐⭐ Alta (Industry reference)

**Takeaway chiave**:
- REST = Representational State Transfer, not protocol but architectural style
- Can be implemented in many ways, not limited to HTTP
- Guiding principles must be satisfied if service called "RESTful"
- REST API (RESTful API) = Web API conforming REST style
- Distinction REST style vs REST API implementation

**Applicabilità tesi**: 
- Definizioni clear REST, differenza stile vs implementazione[192]

---

## Richardson Maturity Model - REST Maturity Levels

### Richardson Maturity Model Overview
**Autore**: Leonard Richardson  
**Anno**: 2008 (proposto), 2025 (updated)  
**Tipo**: Architectural model framework  
**Link**: https://restfulapi.net/richardson-maturity-model/  
**Affidabilità**: ⭐⭐⭐⭐⭐ Massima (Industry standard evaluation framework)

**4 Livelli Maturity**:

**Level 0 - Swamp of POX (Plain Old XML)**
- Single URI, POST per tutte azioni
- RPC-style, not RESTful
- Circa 50% APIs pubbliche

**Level 1 - Resources**
- Separate URIs per risorse
- Resources identified by URIs
- Ma non sfrutta HTTP methods (GET per tutto)

**Level 2 - HTTP Verbs**
- Proper HTTP methods: GET (read), POST (create), PUT (update), DELETE (delete)
- Resources identified by URIs
- **Status quo majority APIs** (approx 90% public APIs)
- Sfrutta HTTP status codes: 200, 201, 204, 400, 404, 500

**Level 3 - Hypermedia Controls (HATEOAS)**
- HATEOAS: Hypermedia As Engine Of Application State
- Responses include navigable links (self, related, next, actions)
- Clients discover endpoints dynamically, no hardcoding URIs
- **True REST per Fielding**
- Rare in production (<5% APIs)

**Applicabilità tesi**:
- Framework evaluate REST maturity, positioning APIs
- Most production APIs Level 2, benefits/complexity Level 3
- HATEOAS adoption barriers explanation[223][224][229]

---

### HATEOAS: Implementation and Maturity (2024-2025)
**Fonti**: DigitalAPI.ai, Vincent Racca, Stack Overflow discussion  
**Anno**: 2024-2025  
**Tipo**: Technical articles + discussion  
**Link**: https://www.digitalapi.ai/blogs/what-is-hateoas  
**Affidabilità**: ⭐⭐⭐ Media-Alta (Practitioner analysis)

**Takeaway chiave**:
- HATEOAS Level 3 maturity: embedded navigation links in responses
- Instead external documentation, clients receive actionable links
- Links include relations (rel attribute), methods (GET, POST, DELETE)
- Strategic impact: reduce version churn, decouple frontends, support workflows
- **Implementation barriers**: added complexity, client sophistication, performance concerns, framework support
- **Real-world adoption low**: simple RPC-style + versioning preferred by most teams
- **Best for**: public APIs, platform APIs, long-lived workflows evolving over time

**Applicabilità tesi**:
- Sezione HATEOAS, why Level 3 rarely adopted
- Cost-benefit analysis, quando usare vs pragmatic Level 2
- Gap tra teoria Fielding e industry practice[190][226]

---

## Standard e Specifiche HTTP

### RFC 9110 - HTTP Semantics
**Ente**: IETF  
**Anno**: 2022  
**Tipo**: Standard IETF RFC (HTTP/1.1, 2, 3 unified)  
**Link**: https://datatracker.ietf.org/doc/html/rfc9110  
**Affidabilità**: ⭐⭐⭐⭐⭐ Massima (Standard ufficiale)

**Takeaway chiave**:
- Consolidates RFC 7231, 7232, 7233, 7235 HTTP/1.1 semantics
- Covers HTTP methods, status codes, header fields, content negotiation
- Core REST transport layer specification
- Methoid semantics: GET (safe, idempotent), POST (unsafe, non-idempotent), PUT (idempotent), DELETE (idempotent), PATCH (non-idempotent)
- Status code categories: 2xx success, 3xx redirection, 4xx client error, 5xx server error

**Applicabilità tesi**: 
- Fondamento HTTP semantics per REST, idempotenza, safety[18]

---

### RFC 9111 - HTTP Caching
**Ente**: IETF  
**Anno**: 2022  
**Tipo**: Standard IETF RFC  
**Link**: https://datatracker.ietf.org/doc/html/rfc9111  
**Affidabilità**: ⭐⭐⭐⭐⭐ Massima (Standard ufficiale)

**Takeaway chiave**:
- Defines HTTP cache behavior, cache directives, validation mechanisms
- Cache-Control header: max-age, no-cache, no-store, public, private, must-revalidate
- ETag (Entity Tag) per strong/weak validation
- Last-Modified header per timestamp-based validation
- Conditional requests: If-None-Match (ETag), If-Modified-Since, If-Match
- **304 Not Modified** response saves bandwidth
- REST APIs native caching advantages vs GraphQL single-endpoint caching challenges

**Applicabilità tesi**: 
- Sezione caching REST, strategia ETags, conditional requests
- Vantaggio competitivo REST su GraphQL per cacheability[228][231]

---

### RFC 9700 - OAuth 2.0 Security Best Current Practice (2025)
**Ente**: IETF  
**Anno**: 2025  
**Tipo**: Standard IETF BCP (Best Current Practice)  
**Link**: https://datatracker.ietf.org/doc/rfc9700/  
**Affidabilità**: ⭐⭐⭐⭐⭐ Massima (Standard IETF aggiornato)

**Takeaway chiave**:
- Updates e extends RFC 6749, 6750 (OAuth 2.0 core specs)
- Threat model aggiornato per modern threats
- Security advisories per token handling, PKCE, mutual TLS
- Grant types: Authorization Code, Client Credentials, Device Flow, Refresh Token
- **PKCE** obbligatorio per public clients (mobile, SPA)
- Token binding, proof-of-possession techniques
- Zero-trust architecture integration

**Applicabilità tesi**:
- Sicurezza REST moderna, OAuth2 best practices, PKCE requirement[214]

---

## Microsoft REST API Guidelines

### Microsoft REST API Guidelines - Official (GitHub)
**Ente**: Microsoft  
**Anno**: 2016 (original), 2024-2025 (maintained)  
**Tipo**: Industry best practices guide  
**Link**: https://github.com/microsoft/api-guidelines  
**Affidabilità**: ⭐⭐⭐⭐⭐ Massima (Enterprise standard)

**Takeaway chiave**:
- Comprehensive design guidance per Microsoft services consistency
- URL structure: /api/v{version}/resource-type/resource-id
- HTTP methods proper usage: GET (read-safe-idempotent), POST (create), PATCH (update fields), PUT (replace)
- Error responses: consistent error schema with error code, message, details
- Status codes: 200 OK, 201 Created, 204 No Content, 400 Bad Request, 401 Unauthorized, 403 Forbidden, 404 Not Found, 429 Too Many Requests, 5xx Server
- Throttling/Rate limiting: Retry-After header, 429 status
- Versioning: URI path (/v1, /v2) preferred, header versioning alternative
- Content negotiation: Accept, Content-Type headers (application/json primary)
- Pagination: skip, top (limit) query parameters, next links

**Applicabilità tesi**:
- Gold standard REST API design practices, large-scale deployment
- Guidance su versioning, error handling, pagination[196][197]

---

### Microsoft Azure REST API Best Practices (2025)
**Ente**: Microsoft Azure  
**Anno**: 2025  
**Tipo**: Official documentation  
**Link**: https://learn.microsoft.com/en-us/azure/architecture/best-practices/api-design  
**Affidabilità**: ⭐⭐⭐⭐⭐ Massima (Official guidance)

**Takeaway chiave**:
- **RESTful principles**: platform independence (HTTP standard), loose coupling (standard protocols), statelessness
- Richardson Maturity Model: Level 0 (single URI, POST) → Level 3 (HATEOAS)
- Most commercial APIs Level 2, Level 3 complexity trade-off
- **OpenAPI Initiative**: standardize REST API descriptions, Swagger→OpenAPI
- Contract-first approach: design API contract first, implement second
- Resource URIs: /users, /users/{userId}, /users/{userId}/orders
- HTTP methods semantics: GET (read), POST (create), PUT (replace), PATCH (update fields)
- Status codes: 2xx success, 3xx redirection, 4xx client error, 5xx server
- Authentication: OAuth2, OIDC, Azure AD
- Rate limiting, throttling, logging

**Applicabilità tesi**:
- Comprehensive REST design guidance, alignment maturity model
- Justification Level 2 mainstream adoption[195]

---

## OpenAPI Specification

### OpenAPI Specification - Official
**Ente**: OpenAPI Initiative (Linux Foundation)  
**Anno**: 2023+ (regularly updated)  
**Tipo**: Industry standard specification  
**Link**: https://swagger.io/specification/  
**Affidabilità**: ⭐⭐⭐⭐⭐ Massima (Standard de-facto)

**Takeaway chiave**:
- Machine-readable REST API description format (formerly Swagger)
- YAML/JSON formats, both human and machine readable
- Specifies: endpoints (paths), operations, parameters, request/response schemas, authentication
- Supports: code generation (Swagger Codegen), documentation (Swagger UI), testing, API governance
- Security schemes: apiKey, HTTP (Basic, Bearer, etc), OAuth2, OpenID Connect
- Schema definitions: reusable models with $ref references
- Contract-first API development enabler

**Applicabilità tesi**:
- OpenAPI standard per REST API design, documentation automation[199][202]

---

### Designing REST APIs with OpenAPI: Top-Down Approach (2024)
**Autore**: SACode  
**Anno**: 2024  
**Tipo**: Technical guide  
**Link**: https://dev.to/sacode/designing-restful-apis-with-openapi-a-top-down-approach-b33  
**Affidabilità**: ⭐⭐⭐ Media-Alta (Practitioner guide)

**Takeaway chiave**:
- OpenAPI design approach: specification first, implementation second
- Path definitions: /api/v1/users/{userId}, parameters definition
- Request/response schemas: $ref component schemas
- Data types, validation rules per field
- Code generation benefits: server skeletons, client SDKs
- Living documentation: specification is always current

**Applicabilità tesi**:
- OpenAPI-first design practice, documentation-as-code pattern[199]

---

## REST API Design Best Practices

### REST API Best Practices 2025
**Fonte**: Hevo Data  
**Anno**: 2025  
**Tipo**: Best practices guide  
**Link**: https://hevodata.com/learn/rest-api-best-practices/  
**Affidabilità**: ⭐⭐⭐⭐ Alta (Industry guide)

**Takeaway chiave**:
- **Nouns over Verbs**: /users/{id}, non /getUser (HTTP method already has verb)
- **JSON sempre**: parse universale, framework support diffuso
- **Plurals**: /users, /products (non /user, /product)
- **Versioning**: URI path /v1, /v2 (clarity, caching benefit)
- **Caching**: Cache-Control headers, reduce database queries, local memory caching
- **Compression**: gzip, brotli reduce transfer size
- **Rate limiting**: Throttling per client, quota management
- **Security**: validate/sanitize inputs, HTTPS obbligatorio, OAuth2, RBAC, encoding responses
- **Error handling**: consistent error schema, descriptive messages
- **Pagination**: skip/top parameters, next links

**Applicabilità tesi**:
- Practical REST API design checklist, proven patterns[200]

---

## REST Idempotency

### Idempotency in REST API Design
**Autori**: Priyanshu Kumawat  
**Anno**: 2023  
**Tipo**: Technical article  
**Link**: https://dev.to/priyanshu_kumawat/idempotency-in-a-rest-api-design-and-why-do-we-need-it-3l0  
**Affidabilità**: ⭐⭐⭐⭐ Alta (Technical depth)

**Takeaway chiave**:
- **Idempotency**: operazione multiple volte = same result as single
- **Safe methods**: GET, HEAD, OPTIONS, TRACE (non cambiano state)
- **Idempotent**: GET, HEAD, OPTIONS, TRACE, PUT, DELETE (ma non POST, PATCH)
- **POST non idempotent**: N requests = N resources (create 3 volte = 3 records)
- **PUT idempotent**: replace resource, send twice = same final state
- **DELETE idempotent**: delete once remove, delete again 404 (same final state no resource)
- **PATCH non idempotent**: partial update operations (increment field twice = wrong value)
- **Idempotency key pattern**: UUID client-generated per retry detection
  - Client send same key per retry, server check se processed già
  - Prevent duplicate orders, duplicate payments (financial systems)
- **Server-side idempotency**: request lookup cache, check key, return cached result se exists

**Applicabilità tesi**:
- Sezione idempotency importance REST, retry safety, distributed systems
- Pattern Idempotency Key per reliability[205][206][207][208]

---

### Designing Robust APIs with Idempotency - Stripe
**Autore**: Stripe Engineering  
**Anno**: 2017  
**Tipo**: Industry case study  
**Link**: https://stripe.com/blog/idempotency  
**Affidabilità**: ⭐⭐⭐⭐⭐ Alta (Industry leader payment APIs)

**Takeaway chiave**:
- Idempotency solves distributed failure ambiguity
- Client can safely retry any error without duplicating side effects
- Idempotency Key header: client UUID per request
- Server-side: store key → result mapping
- POST payment creation example: send key twice = same response, paid once
- Essential per payment systems, financial transactions
- Best practice: all POST/PATCH operations idempotent key support

**Applicabilità tesi**:
- Financial/transactional APIs, real-world idempotency requirement[208]

---

### HTTP Methods: POST vs PUT vs PATCH
**Fonte**: Stack Overflow, Treblle, DEV Community  
**Anno**: 2019-2025  
**Tipo**: Community discussion + guides  
**Link**: https://stackoverflow.com/questions/54863759/rest-api-when-to-use-post-put-patch-and-delete  
**Affidabilità**: ⭐⭐⭐⭐ Alta (Consensus community)

**Takeaway chiave**:
- **POST**: create new resource, non idempotent (repeated = multiple resources)
- **PUT**: replace entire resource, idempotent (repeated = same final state)
- **PATCH**: partial update, conditional idempotency
  - Idempotent if: applying same patch twice = same result (replace operation)
  - Non idempotent if: patch contains relative operations (increment, append)
- **DELETE**: remove resource, idempotent (repeated = 404, same final state)
- **GET**: safe (non-modifying), idempotent

**Applicabilità tesi**:
- Semantica corretta HTTP methods, quando usare quale[204][207]

---

## Caching e Conditional Requests

### HTTP Caching with Conditional Requests and ETags
**Fonte**: Azion, Zuplo, IETF RFC  
**Anno**: 2025 (Azion), 2025 (Zuplo)  
**Tipo**: Technical guides + standard  
**Link**: https://www.azion.com/en/learning/performance/what-are-conditional-requests/  
**Affidabilità**: ⭐⭐⭐⭐ Alta (CDN provider + API platform)

**Takeaway chiave**:
- **Conditional requests**: HTTP headers check se resource changed
- **ETag (Entity Tag)**: unique resource version identifier (fingerprint)
  - **Strong ETags**: byte-for-byte equality (for exact match)
  - **Weak ETags**: semantic equivalence (for dynamic content)
- **Validation headers**:
  - If-None-Match: send request se ETag differs (client has stale)
  - If-Modified-Since: send request se timestamp newer
  - If-Match: precondition for updates (avoid lost update problem)
- **304 Not Modified**: server response se resource unchanged (no body)
- **Flow**: 1) client GET with ETag stored 2) client send If-None-Match 3) server check 4) 304 if unchanged
- **Benefits**: bandwidth savings, reduced server load, faster response
- **Edge case**: strong ETags resource-intensive compute

**Applicabilità tesi**:
- Caching strategy REST nativo, ETags implementation[209][212]

---

### REST API Optimization with ETags - Zuplo
**Fonte**: Zuplo  
**Anno**: 2025  
**Tipo**: Technical guide  
**Link**: https://zuplo.com/learning-center/optimizing-rest-apis-with-conditional-requests-and-etags  
**Affidabilità**: ⭐⭐⭐⭐ Alta (API gateway platform)

**Takeaway chiave**:
- **ETag generation strategies**:
  - Strong ETags: hash content (SHA-256) per exact match
  - Weak ETags: "W/..." notation per semantic equivalence
- **Best practices**:
  - Generate on server, never accept client ETags (security)
  - Combine with Cache-Control directives
  - Use rowversion (DB timestamp) in SQL Server
  - Separate service layer per ETag generation logic
- **Implementation patterns**: Flask (Blueprint.etag), Express (auto-generate), API Gateway (edge computing)
- **Common issues**: weak ETags not widely implemented, test thoroughly client compatibility

**Applicabilità tesi**:
- ETag implementation details, generation strategies[212]

---

## API Versioning

### API Versioning Strategies - xMatters (2025)
**Fonte**: xMatters  
**Anno**: 2025  
**Tipo**: Comprehensive guide  
**Link**: https://www.xmatters.com/blog/api-versioning-strategies  
**Affidabilità**: ⭐⭐⭐⭐ Alta (Enterprise platform)

**Takeaway chiave**:
- **4 Main Strategies**:

**1. URI Versioning** (/api/v1/products)
- Pros: Simple, cacheable (different URIs = different cache keys), widely used (Facebook, Twitter, Airbnb)
- Cons: Large codebase footprint, URL clutter, need route logic

**2. Query Parameter** (?version=1)
- Pros: Straightforward, easy default latest version
- Cons: Difficult routing, cache confusion

**3. Header Versioning** (Accept-version: 1.0)
- Pros: Doesn't clutter URIs
- Cons: Requires custom headers, hard to test browser

**4. Content Negotiation** (Accept: application/vnd.xm.device+json;version=1)
- Pros: Granular resource-level versioning, small footprint
- Cons: Less accessible, HTTP headers required, harder browser testing

**Best Practice**: URI versioning for public APIs (clarity), header for backend services

**Applicabilità tesi**:
- Versioning strategies comparison, trade-offs adoption[210][213]

---

### Best Practices API Versioning - Ambassador (2024)
**Fonte**: Ambassador  
**Anno**: 2024  
**Tipo**: Technical guide  
**Link**: https://www.getambassador.io/blog/api-versioning-best-practices  
**Affidabilità**: ⭐⭐⭐ Media-Alta (API platform)

**Takeaway chiave**:
- **MAJOR.MINOR.PATCH** versioning internal
- **Release schedule**: inform consumers upcoming changes, provide migration window
- **Deprecation**: mark endpoints @deprecated with sunset date, monitor usage
- **Backward compatibility**: maintain old versions min 6-12 months during migration
- **Consumer communication**: changelog, deprecation notices, migration guides
- **Hybrid approach possible**: URI major version + header minor/patch

**Applicabilità tesi**:
- Versioning lifecycle management, deprecation strategy[213]

---

## Sicurezza REST API

### OAuth2 vs OpenID Connect - Differences 2024
**Fonte**: Endgrate  
**Anno**: 2024  
**Tipo**: Protocol comparison guide  
**Link**: https://endgrate.com/blog/oauth-vs-openid-connect-key-differences-2024  
**Affidabilità**: ⭐⭐⭐⭐ Alta (Identity integration platform)

**Takeaway chiave**:
- **OAuth 2.0**: Authorization (cosa access), delegated access
  - Grant types: Authorization Code, Client Credentials, Device Flow, Refresh Token
  - Token: access token (bearer)
  - Use: API access without password sharing
  
- **OpenID Connect**: Authentication (chi sei), extension OAuth2
  - Adds: ID token (JWT, user info), UserInfo endpoint
  - Use: user login, SSO, federated identity
  
- **Flows per scenario**:
  - Server-side web app: Authorization Code Flow
  - SPA/Mobile: Authorization Code Flow + PKCE
  - M2M: Client Credentials Flow

- **Security**: HTTPS mandatory, PKCE for public clients, verify signatures, short token expiry

**Applicabilità tesi**:
- OAuth2 vs OIDC distinction, when use quale[214][215][217]

---

### API Security Best Practices
**Fonte**: BrowserStack, LoginRadius, KVY Technology  
**Anno**: 2024-2025  
**Tipo**: Security guides  
**Link**: https://www.browserstack.com/guide/rest-api-design-principles-and-best-practices  
**Affidabilità**: ⭐⭐⭐⭐ Alta (Security-focused guides)

**Takeaway chiave**:
- **HTTPS obbligatorio**: encryption in transit, prevent MITM attacks
- **Input validation**: strict schema (Joi, JSON Schema), whitelist allowed characters
- **RBAC (Role-Based Access Control)**: granular permissions per role
- **Rate limiting**: throttle per client, prevent DDoS
- **API key rotation**: expose key = immediate invalidation
- **SQL injection prevention**: parameterized queries, ORM frameworks
- **XSS prevention**: encode output, CSP headers
- **CORS**: restrict origins, whitelist partners
- **Logging/monitoring**: detect anomalies, audit trail

**Applicabilità tesi**:
- REST security best practices, common vulnerabilities[203][220]

---

## REST API Performance e Payload Optimization

### REST API Overfetching/Underfetching Solutions (2025)
**Autore**: Ritik Sahu  
**Anno**: 2025  
**Tipo**: Technical article  
**Link**: https://www.linkedin.com/pulse/designing-efficient-apis-dont-overfetch-underfetch-ritik-sahu-vrafc  
**Affidabilità**: ⭐⭐⭐⭐ Alta (Senior engineer analysis)

**Takeaway chiave**:
- **Overfetching**: API return più dati non usati, inflated payloads, frontend rendering slow
  - Soluzione: dynamic serializer fields, query parameter filtering (?fields=id,name)

- **Underfetching**: API non return tutti dati, client must multiple requests, latency multiplied
  - Soluzione: select_related/prefetch_related (ORM), aggregation gateway, composite endpoints

- **Techniques avoid both**:
  1. Dynamic Serializer Fields (return only requested fields)
  2. GraphQL (precise fetching, but complexity)
  3. ORM optimization (select_related, prefetch_related, N+1 prevention)
  4. Modular endpoints per use-case (/users/profile, /users/summary)
  5. API Gateway aggregation (merge data cross-services)
  6. Versioned consumer-specific APIs (/v1/mobile/users vs /v1/web/users)
  7. Response shaping query params (?include=email,name)
  8. Field-level usage monitoring (analytics guide optimization)
  9. Content negotiation (PDF, CSV, JSON formats)

**Applicabilità tesi**:
- REST overfetching/underfetching vs GraphQL advantage
- Optimization strategies REST keep performance[218]

---

### API Performance Optimization - Zuplo (2025)
**Fonte**: Zuplo  
**Anno**: 2025  
**Tipo**: Performance guide  
**Link**: https://zuplo.com/learning-center/increase-api-performance  
**Affidabilità**: ⭐⭐⭐⭐ Alta (API gateway platform)

**Takeaway chiave**:
- **6 Key strategies**:
  1. **Caching**: frequent responses cached, reduce backend load
  2. **Payload reduction**: remove unnecessary fields, compress (gzip, brotli)
  3. **Rate limiting**: throttle traffic, prevent overload
  4. **Regional endpoints**: serve geographically close, lower latency
  5. **Efficient authentication**: Lambda authorizers (serverless), minimal latency
  6. **Performance monitoring**: track latency p50/p99, error rates

- **Compression**:
  - GZIP: text data, widely compatible
  - Brotli: 17-25% better than GZIP, modern apps
  - Protocol Buffers: 3-10x smaller JSON payloads (trade-off: binary, need client library)

- **Pagination**: limit data per request, avoid massive responses

**Applicabilità tesi**:
- REST performance tuning, scaling large APIs[221]

---

## REST vs GraphQL Trade-offs

### REST vs GraphQL vs WebSockets Comparison (2024)
**Fonte**: Appwrite  
**Anno**: 2024  
**Tipo**: Technical comparison  
**Link**: https://appwrite.io/blog/post/rest-vs-graphql-websockets-which-is-best-for-your-app  
**Affidabilità**: ⭐⭐⭐ Media-Alta (API platform perspective)

**Takeaway chiave**:

| Feature | REST | GraphQL | WebSocket |
|---------|------|---------|-----------|
| Communication | Request-Response | Request-Response | Full-Duplex |
| Real-Time | No | Yes (subscriptions) | Yes |
| Data Transfer | Individual requests | Precise queries | Continuous |
| Complexity | Low | Medium | High |
| Latency | High (real-time) | Medium | Low |
| Use Case | CRUD, simple | Complex data, mobile | Live updates |

**REST Pros**: Simple, widely used, caching native, stateless  
**REST Cons**: Over/underfetching, no real-time, multiple requests needed

**GraphQL Pros**: Flexible queries, bandwidth efficient (no over-fetch)  
**GraphQL Cons**: Complex implementation, N+1 queries, single endpoint caching hard

**WebSocket Pros**: Real-time, low latency, bidirectional  
**WebSocket Cons**: Stateful (scaling hard), complex, resource-intensive

**Applicabilità tesi**:
- Matrix comparison REST vs altri, when use quale[219]

---

### REST vs GraphQL Deep Dive - API7 (2025)
**Fonte**: API7.ai  
**Anno**: 2025  
**Tipo**: Technical analysis  
**Link**: https://api7.ai/pt/learning-center/api-101/graphql-vs-rest  
**Affidabilità**: ⭐⭐⭐⭐ Alta (API platform)

**Takeaway chiave**:
- **Architectural philosophies**:
  - REST: resource-centric, multiple endpoints per resource
  - GraphQL: query language, single endpoint, client-driven data shape

- **Data fetching efficiency**:
  - REST: underfetching (multiple requests for related data), overfetching (unwanted fields)
  - GraphQL: precise fetching, single query gets exactly needed data

- **Schema and type system**:
  - REST: external standards (OpenAPI) for contracts
  - GraphQL: built-in strongly-typed schema, self-documenting

- **Client flexibility vs server control**:
  - REST: server predefined endpoints, fixed response structure
  - GraphQL: client dictates data shape, more flexible

- **Use cases alignment**:
  - REST: simple CRUD, caching-critical, clear resource boundaries
  - GraphQL: complex data relationships, mobile (bandwidth), rapid UI iteration

- **Hybrid approaches**: REST backend services + GraphQL aggregation layer (BFF pattern)

**Applicabilità tesi**:
- Comprehensive REST vs GraphQL, when choose REST[230]

---

### REST vs GraphQL Trade-offs Framework (2011-2025)
**Fonte**: Tradeoffs.dev  
**Anno**: 2025  
**Tipo**: Technical framework  
**Link**: https://tradeoffs.dev/article/The_tradeoffs_between_using_a_RESTful_vs_GraphQL_API_for_your_application.html  
**Affidabilità**: ⭐⭐⭐ Media-Alta (Trade-off analysis)

**Takeaway chiave**:
- **Flexibility vs Simplicity**:
  - REST: easy learn/implement, simple CRUD
  - GraphQL: steep learning curve, powerful for complex

- **Performance vs Network overhead**:
  - REST: native caching, scalability, multiple requests (latency)
  - GraphQL: optimized responses, fewer requests (high per-query complexity)

- **Evolution vs Stability**:
  - REST: mature, stable contracts, breaking changes need versioning
  - GraphQL: easier evolve schema, runtime detection, rapid prototyping

- **Tooling vs Support**:
  - REST: established tools, framework support everywhere
  - GraphQL: growing ecosystem, GitHub/Shopify adoption increasing

**Applicabilità tesi**:
- REST strengths positioning vs GraphQL, decision framework[227]

---

## REST HTTP Modern

### What GraphQL Users Should Know About HTTP and REST (2025)
**Fonte**: WunderGraph  
**Anno**: 2025  
**Tipo**: Technical article  
**Link**: https://wundergraph.com/blog/what_every_graphql_user_should_know_about_http_and_rest  
**Affidabilità**: ⭐⭐⭐⭐ Alta (GraphQL/REST platform)

**Takeaway chiave**:
- **Stateless REST constraint**: each request contains all info needed, no server-side session state
- **Scalability benefit**: stateless = horizontal scaling (any server can handle request)
- **WebSocket challenge**: stateful connections (subscriptions), violates REST stateless
  - WebSocket auth via message (no HTTP headers), complicates auth/scaling
  - Sticky sessions required, connection affinity needed

- **REST natural caching**: URI-based cache keys, CDN-friendly
- **GraphQL caching challenge**: single endpoint (/graphql), all queries same URL, cache-busting hard

**Applicabilità tesi**:
- REST statelessness advantage, why WebSocket violates REST principles
- Scalability implications[222]

---

## REST Security Comprehensive

### Comprehensive REST API Security Guide (2025)
**Fonte**: Practical DevSecOps  
**Anno**: 2025  
**Tipo**: Security guide  
**Link**: https://www.practical-devsecops.com/what-is-rest-api-security/  
**Affidabilità**: ⭐⭐⭐⭐ Alta (DevSecOps expert)

**Takeaway chiave**:
- **6 REST Principles**: Uniform Interface, Client-Server Separation, Statelessness, Cacheability, Layered System, Code-on-Demand
- **Common vulnerabilities**:
  - Injection attacks (SQL injection)
  - Broken authentication (weak password, token exposure)
  - Broken authorization (escalation)
  - Excessive data exposure (over-fetching)
  - Rate-limiting gaps (DDoS)
  - Security misconfiguration
  - Lack encryption (HTTP vs HTTPS)

- **JWT vs OAuth2**:
  - JWT: compact token format (used within OAuth2)
  - OAuth2: full delegation framework for access management
  - Use JWT within OAuth2 as token type

- **Algorithm per security**:
  - HMAC: message authentication
  - RSA/ECC: public-key encryption
  - SHA-256: hashing
  - JWT/OAuth2: specific signature algorithms

**Applicabilità tesi**:
- REST security principles, vulnerability types, JWT vs OAuth2 distinction[220]

---

## Summary Affidabilità Fonti

### Categoria Affidabilità: MASSIMA (⭐⭐⭐⭐⭐)
- **IETF RFCs** (9110, 9111, 9700): Standard protokol ufficiali
- **Fielding Dissertation** (2000): Fonte primaria REST definition
- **Microsoft Guidelines**: Enterprise standard
- **OpenAPI Initiative**: Industry standard de-facto
- **RFC 7234**: Standard HTTP/1.1 caching

### Categoria Affidabilità: ALTA (⭐⭐⭐⭐)
- **Microsoft Azure**: Official documentation
- **Industry platforms** (Zuplo, Stripe, Ably): Production expertise
- **Best practices guides**: Consensus industry (xMatters, Ambassador, Hevo)
- **Technical articles**: Authoritative authors (Krämer, Sahu, etc)

### Categoria Affidabilità: MEDIA-ALTA (⭐⭐⭐)
- **Developer community** (DEV, LinkedIn): Practitioner perspectives
- **API platforms**: Vendor-specific best practices
- **Technical comparisons**: Analysis frameworks

---

## Note per Tesi

### Cosa coperto:
✅ Principi REST teorici (Fielding, Richardson Maturity Model)  
✅ Standard HTTP (RFC 9110, 9111, 9700)  
✅ Industry guidelines (Microsoft, OpenAPI)  
✅ Design practices (versioning, caching, security)  
✅ REST limiti (over/underfetching, caching challenges)  
✅ Comparazione REST vs GraphQL vs WebSocket  
✅ HATEOAS adoption barriers  
✅ Idempotency patterns per reliability  
✅ OAuth2/OIDC modern security  

### Criticità REST (per balanced view):
- **HATEOAS adoption LOW** (<5% production APIs) - complexity barrier
- **Over/underfetching** - motivates GraphQL adoption
- **Single endpoint limitation** - caching challenges vs REST (Level 2 standard)
- **Statelessness scalability** vs WebSocket stateful connections
- **Gap teoria (Fielding true REST)** vs practice (CRUD+JSON Level 2)

### Quando scegliere REST:
✅ Simple CRUD operations  
✅ Strong caching requirements (CDN-friendly)  
✅ Resource-centered, clear boundaries  
✅ Public APIs (clarity, versioning)  
✅ High-traffic, need horizontal scaling (stateless)  
✅ Standardized ecosystem (tools, docs, frameworks)  

### Quando considerare alternative:
❌ Complex nested data relationships → GraphQL
❌ Real-time bidirectional → WebSocket
❌ AI context management → MCP
❌ Multiple data sources aggregation → GraphQL federation + REST backend
