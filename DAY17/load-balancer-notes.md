# Load Balancer — Notes

## Why We Need a Load Balancer

- High availability + horizontal scalability = running multiple identical instances of your app across multiple computers.
- **Problem without a load balancer:** the client app would need to know the exact addresses and count of all your server instances — tightly coupling the client to your internal implementation, making changes very hard.
- A **load balancer** solves this: it balances traffic across a group of servers so no single server gets overloaded, and it **abstracts** the group of servers to look like a single, powerful server to the client.

## Quality Attributes a Load Balancer Provides

1. **High Scalability**
   - By hiding servers behind a load balancer, you can scale horizontally — add servers when load increases, remove them when load decreases (saves money).
   - In the cloud, **auto-scaling policies** can automatically add/remove servers based on criteria like requests/sec or bandwidth.

2. **High Availability**
   - Load balancers can be configured to stop sending traffic to unreachable servers.
   - This monitoring lets the load balancer route traffic only to healthy servers, skipping dead/slow ones.

3. **Performance**
   - Adds a small amount of latency (acceptable trade-off).
   - But massively increases **throughput** — since you can have many backend servers instead of one, total requests/tasks handled per unit time goes way up.

4. **Maintainability**
   - Servers can be pulled out of rotation one at a time for maintenance/upgrades without disrupting clients.
   - Once done, add it back and repeat for the next server — this enables **rolling releases** while still honoring your availability SLA.

## 4 Types of Load Balancing Solutions

### 1. DNS Load Balancing
- **DNS (Domain Name System)** = the "phonebook of the internet" — maps human-friendly domains (amazon.com) to IP addresses.
- A single DNS record can map to **multiple IP addresses**. DNS servers often return this list in a different order per request (round-robin), and clients typically just pick the first address — this naturally spreads load.
- **Drawbacks:**
  1. **No health monitoring** — if a server goes down, DNS keeps sending clients to it anyway.
  2. Address lists are cached (client-side, and elsewhere) based on a configured TTL (time to live) — so a dead server may keep getting traffic for a while even after it fails.
  3. **Load balancing strategy is just simple round-robin** — doesn't account for some servers being more powerful, or some being more overloaded than others.
  4. **Security issue** — clients get direct IP addresses of all your servers, exposing implementation details. A malicious client could hammer just one IP directly, overloading a single server on purpose.

### 2. Hardware Load Balancers
- Run on **dedicated physical devices** built specifically for load balancing.

### 3. Software Load Balancers
- Just **programs** that run on general-purpose computers, doing the same job.

*(Hardware and Software load balancers share the same key benefits over DNS load balancing — covered together below.)*

**How they fix DNS's drawbacks:**
- All client-server communication passes through the load balancer — individual server IPs are **never exposed** to clients → much more secure.
- They actively monitor server health via **periodic health checks** → real failure detection, not just blind round-robin.
- They balance load **intelligently** — factoring in server hardware differences, current load, number of open connections, etc.
- Bonus: these load balancers aren't just for external traffic — they can also sit **between internal services** (e.g., separating a front-end service from a fulfillment service and a billing service in an online store), letting each service scale independently and transparently.
- **Limitation:** usually need to be **collocated** (physically close) with the servers they balance — putting them too far away adds latency to both request and response paths. This becomes a problem if you run servers across multiple geographical data centers.
- Also: on their own, they don't solve DNS resolution — you still need *some* DNS solution to map domain names to IPs in the first place.

### 4. GSLB (Global Server Load Balancer)
- A **hybrid** of DNS + hardware/software load balancer.
- Provides DNS service like a normal DNS server, **plus** smarter routing:
  - Detects the user's approximate location from their request's origin IP.
  - Has monitoring capabilities like a regular load balancer — knows the location and health state of servers (typically, load balancers in different data centers) registered with it.
- When a user sends a DNS query, GSLB can return the address of the **nearest healthy** load balancer/data center.
- **Routing strategies aren't limited to geography** — GSLB can route based on current traffic/CPU load per data center, or best estimated response time/bandwidth — giving each user the best possible performance regardless of location.
- **Disaster recovery benefit:** if a data center goes down (natural disaster, power outage), GSLB reroutes users elsewhere automatically → higher availability.
- **Avoiding single point of failure:** you can register *multiple* load balancers per region with the GSLB (or any DNS service) — clients get a list and pick one (first in list, or randomly).

## Quick Recap

| Type | Health Monitoring | Security | Best Fit |
|---|---|---|---|
| DNS | No | Weak (exposes server IPs) | Simple/cheap setups, small scale |
| Hardware | Yes | Strong | Single data center, high performance needs |
| Software | Yes | Strong | Single data center, flexible/cost-effective |
| GSLB | Yes | Strong | Multi-region, global systems, disaster recovery |

**Key takeaway:** A load balancer distributes traffic across servers and hides server details from clients, directly enabling scalability, availability, performance, and maintainability. DNS load balancing is simple but "blind" (no health checks, weak security); hardware/software load balancers fix this with active monitoring and smarter routing, but are best within one location; GSLB combines DNS + load balancer intelligence to route users globally to the nearest healthy data center.
