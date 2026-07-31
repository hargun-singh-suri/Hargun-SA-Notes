# API Gateway — Notes

## The Problem It Solves

- **Example system:** a video sharing/streaming platform. Starts as **one service** handling frontend, user profiles, subscriptions, notifications, video storage/streaming, comments, and security (auth/authz).
- Over time, this single service becomes too big/complex → split into multiple services (one per purpose), applying the **organizational scalability** principle we learned earlier.
- **New problem this creates:**
  1. The single API is now split into multiple APIs (one per service) — the client (browser) now needs to know the internal structure of your system and call different services for different tasks.
     - Example: loading a video page requires calling the Frontend Service, Video Service, AND Comments Service separately.
  2. Each service now has to **reimplement its own security/auth logic** — duplication + performance overhead.

## What Is an API Gateway?

- An **API management service** sitting between the client and the collection of backend services.
- Uses the **API composition** pattern: combines multiple internal service APIs into **one single external API** the client calls.
- Result: client makes **one request** to the API Gateway, which handles calling the right internal services.

## Benefits of an API Gateway

### 1. Internal Changes Stay Transparent to Clients
- You can restructure internal services (e.g., splitting Frontend Service into desktop vs mobile variants, or Video Service into high-res vs low-res) without the client ever noticing — the external API contract stays the same.

### 2. Centralized Security
- Authentication, authorization, and SSL termination all happen in **one place** — the gateway.
- Malicious/impersonation attempts can be blocked right at the gateway, before reaching internal services.
- Can enforce **role/permission-based access** (e.g., who can view private videos, delete/upload videos).
- Can implement **rate limiting** here to block denial-of-service attacks.

### 3. Improved Performance
- **Saves overhead:** authentication happens once at the gateway instead of being repeated at every service.
- **Request routing:** instead of the client making 3 separate calls (Frontend, Video, Comments services), it makes **one call** to the gateway, which routes to all needed services and aggregates the responses into one.
- **Caching:** the gateway can cache static content/responses, returning them immediately without hitting backend services — reduces response time.

### 4. Monitoring & Alerting
- Since all traffic passes through one place, you get real-time visibility into traffic patterns and load.
- Enables alerts for sudden traffic drops/spikes — improves **observability** and **availability**.

### 5. Protocol Translation
- Externally you might expose a REST API using JSON, while internally different services may use different RPC technologies, formats, or even legacy protocols (like HTTP 1 with XML).
- The gateway can also translate protocols for **external partners** (e.g., an ad company or video-hosting partner) whose systems only support specific protocols — without forcing changes to your internal services.

## Quality Attributes Gained

- **Security** (centralized auth, rate limiting)
- **Performance** (routing + caching)
- **Availability** (via monitoring/alerting → better observability → faster incident response)

## Best Practices & Anti-Patterns

### 1. Keep Business Logic OUT of the API Gateway
- The gateway's job is **composition and routing** — not making business decisions.
- If you start adding business logic and make the gateway "too smart," you recreate the original problem: one bloated, unmanageable service doing everything — exactly what splitting into services was meant to avoid.

### 2. Watch Out for Single Point of Failure
- Since all traffic goes through the gateway, it can become a single point of failure.
- **Scalability/availability fix:** deploy multiple gateway instances behind a load balancer (straightforward).
- **Bigger risk:** a bad release or bug in the gateway itself can take down the **entire system** for all clients — so gateway deployments need extra caution to avoid human error.

### 3. Accept the Small Performance Overhead — Don't Bypass the Gateway
- Adding a gateway does introduce a small amount of latency (one more hop).
- Overall, the benefits outweigh this cost — but teams may be tempted to **bypass the gateway** for "optimization," which is an anti-pattern.
- **Why bypassing is bad:** if only the gateway calls a service externally, that service team can freely make internal API changes and just update the gateway. But if external clients call the service directly, the team must coordinate changes across every external client — reintroducing the tight coupling problem the gateway was meant to solve in the first place.

## Quick Recap

| Benefit | What it Solves |
|---|---|
| API Composition | One API for the client, regardless of internal service count |
| Centralized Security | No duplicated auth logic across services |
| Request Routing + Caching | Fewer client calls, faster responses |
| Monitoring | Real-time visibility, faster alerting |
| Protocol Translation | Bridges different internal/external protocols |

| Anti-Pattern | Why to Avoid |
|---|---|
| Business logic in gateway | Recreates the original "one bloated service" problem |
| No redundancy for gateway | Becomes a single point of failure for the whole system |
| Bypassing the gateway | Reintroduces tight coupling between services and external clients |

**Key takeaway:** An API Gateway sits between clients and your backend services, composing multiple internal APIs into one external API. It centralizes security, improves performance via routing/caching, and enables monitoring and protocol translation — but it must stay a thin routing/composition layer (no business logic), needs redundancy to avoid being a single point of failure, and should never be bypassed by external clients calling services directly.
