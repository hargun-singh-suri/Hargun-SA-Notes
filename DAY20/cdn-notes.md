# CDN (Content Delivery Network) — Notes

## The Problem: Latency from Physical Distance

- Even with multiple data centers + Global Server Load Balancing, there's still real latency caused by:
  1. **Physical distance** between user and server.
  2. **Multiple network hops** between routers along the way.

### Worked Example: User in Brazil, Server in US East
Loading a homepage with 10 assets (images, JS, CSS), assuming ~200ms latency to the US:

| Step | Latency |
|---|---|
| TCP 3-way handshake (3 trips) | 600ms |
| HTTP request/response for HTML page | 400ms |
| Request for 10 assets to reach server | 200ms |
| Loading all 10 assets | 2000ms |
| **Total** | **~3.2 seconds** |

- Per a **Google Analytics study (March 2016):** 53% of mobile users abandon a site if it takes longer than 3 seconds to load. This ~3.2s result is a real problem.
- Replicating the *service* across more data centers doesn't fully help — because it's not business logic that needs to be closer to users, it's **static content** (images, HTML, JS, CSS, videos).

## What Is a CDN?

- **CDN (Content Delivery Network)** = a globally distributed network of servers placed strategically to speed up content delivery to end users.
- Originally built to solve the **"World Wide Wait"** problem — bad user experience from slow connections/overloaded servers.
- Works by **caching your content** on **edge servers** located at various **Points of Presence** — physically closer to users and better positioned in the network.
- Can deliver: webpages, images, text, CSS, JS, and both live and on-demand video streams.
- Used across almost every digital-facing industry: e-commerce, banks, SaaS, media/streaming, social media.

## Benefits of Using a CDN

1. **Faster page loads** — content served from nearby edge servers instead of your origin server.
2. **Improved availability** — since most content comes from the CDN (not your servers directly), issues/slowness in your own system are less noticeable to users.
3. **Improved security** — helps mitigate DDoS attacks, since malicious traffic gets distributed across the CDN's large server network instead of hitting your system directly.
4. **Extra speed techniques:** CDN servers use optimized/faster hard drives, and reduce bandwidth via compression (Gzip) and JS minification.

### Worked Example With CDN
Same Brazil user, now hitting a nearby CDN edge server (~50ms latency):

| Step | Latency |
|---|---|
| TCP 3-way handshake | 150ms |
| HTTP request/response for HTML page | 100ms |
| Requesting + loading all assets | 550ms |
| **Total** | **~800ms (under 1 second)** |

- Huge improvement — from ~3.2s down to under 1 second, purely by caching content closer to the user.

## 2 CDN Integration Strategies

### 1. Pull Strategy
- You tell the CDN provider which content to cache, and how often to invalidate it (**TTL — Time To Live** per asset/type).
- **First request** for an asset → CDN has nothing cached yet, so it fetches from your servers (populates the cache).
- **Subsequent requests** → served directly from the edge server — no round trip to your system.
- **When TTL expires:** CDN checks with your server if the asset changed.
  - If unchanged → CDN just refreshes the expiration timer, serves the same cached copy.
  - If changed → your server sends the new version, CDN updates its cache and serves the new one.

**Pros:**
- Low maintenance — once configured, the CDN provider handles everything going forward.

**Cons:**
- The very first user requesting an uncached asset experiences higher latency (cache miss).
- If all assets share the same TTL, they may all expire simultaneously → a spike of refresh requests hitting your servers at once.
- Your system still needs to maintain reasonably high availability — if an asset expires and your system is down when the CDN tries to pull the new version, users get errors.

### 2. Push Strategy
- You manually/automatically upload/publish content directly to the CDN.
- Whenever content changes, **you're responsible** for re-publishing the new version to edge servers.
- Some CDNs support this natively; others simulate it by setting a very long (effectively infinite) TTL — and you manually **purge** the cache when you want to force a refresh from your servers.

**Pros:**
- If content doesn't change often, push once and traffic goes straight to edge servers from then on.
- Greatly reduces traffic to your own system, and reduces the burden of maintaining high availability — even if your system goes down temporarily, users still get content from the CDN, unaffected.

**Cons:**
- If content changes frequently, you must actively keep republishing — otherwise users get **stale/out-of-date content**.

## Quick Recap

| Strategy | Best For | Trade-off |
|---|---|---|
| Pull | Content that changes occasionally, low maintenance desired | First-request latency, possible simultaneous expiry spikes |
| Push | Content that rarely changes | Must manually republish on every change, or content goes stale |

**Key takeaway:** A CDN caches your static content on globally distributed edge servers close to users, cutting page load times dramatically (in the example, from ~3.2s down to under 1s) while also improving availability and security. Choose Pull for low-maintenance caching of content that changes occasionally, or Push for content that rarely changes and you want maximum traffic offloaded from your own servers.
