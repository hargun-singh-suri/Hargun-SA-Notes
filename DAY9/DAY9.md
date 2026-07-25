# Fault Tolerance (Achieving High Availability) — Notes

## 3 Sources of Failures

1. **Human error** — pushing a faulty config to production, running the wrong command/script, or deploying a poorly tested version.
2. **Software errors** — long garbage collection pauses, crashes (out-of-memory exceptions, null pointer exceptions, segmentation faults, etc.).
3. **Hardware failures** — servers/routers/storage devices dying due to age, power outages from natural disasters, or network failures from infrastructure issues/congestion.

## What Is Fault Tolerance?

- No matter how good your code review, testing, release process, or hardware maintenance is — **failures will still happen**.
- **Fault tolerance** = the system's ability to stay operational and available to users, even when one or more components fail.
- A fault-tolerant system may run at full performance or reduced performance during a failure, but it avoids becoming **completely unavailable**.

## 3 Major Tactics of Fault Tolerance

### 1. Failure Prevention (via Replication & Redundancy)
- Core idea: eliminate any **single point of failure**.
  - Example of a single point of failure: one server running your app, or one database instance holding all your data.
- **Solution: Replication/Redundancy**
  - Run multiple instances of your app across multiple servers — if one dies, traffic goes to the others.
  - Run multiple database replicas with the same data — losing one replica doesn't mean losing data.
- **Spatial redundancy** — running copies across different machines (what's described above).
- **Time redundancy** — repeating the same operation/request multiple times until it succeeds or you give up (useful for computations).

**Two replication strategies:**

| Strategy | How it works | Pros | Cons |
|---|---|---|---|
| **Active-Active** | Requests go to ALL replicas; they stay in sync | Load spreads across replicas (like horizontal scaling) → better performance & more traffic capacity | Keeping all replicas in sync is complex — needs coordination overhead |
| **Active-Passive** | One primary replica takes all requests; passive replicas just take periodic snapshots to follow the primary | Simple to implement — clear leader with most up-to-date data | Can't scale — all requests still go to one machine |

### 2. Failure Detection & Isolation
- If one instance crashes (software or hardware issue), you need to **detect** it and **isolate** it from the group — e.g., stop sending it requests.
- Usually done via a separate **monitoring service**, using either:
  - **Health checks** — monitoring service pings each instance periodically.
  - **Heartbeats** — healthy instances send periodic "I'm alive" messages to the monitor.
- If the monitor doesn't hear from a server for a set duration, it assumes that server is down.
- **False positives are okay** (e.g., a healthy host mistaken as faulty due to temporary network blip or GC pause) — but **false negatives are not okay** (a crashed server that goes undetected).
- Monitoring can go beyond simple pings — it can also track:
  - Error/exception rate per host per minute (spike = likely failure).
  - Response time per host (if it becomes too slow, something's probably wrong).

### 3. Recovery from Failure
- Callback to the MTBF/MTTR formula from the last lecture: **if you can detect and recover from failures faster than users notice, you effectively get high availability — regardless of how often failures happen.**
- Once a faulty instance is detected/isolated, possible recovery actions:
  1. **Stop traffic to it** — the most immediate step.
  2. **Restart it** — the problem may just go away after a restart.
  3. **Rollback** — go back to the last known stable/correct version.
     - Very common in databases — if data integrity is violated, roll back to the last correct state.
     - Also used in deployments — if a new software version causes a spike in errors across servers, automatically roll back to the previous stable version.

## Quick Recap

| Tactic | Goal | Key Techniques |
|---|---|---|
| Failure Prevention | Avoid single points of failure | Replication (active-active / active-passive), spatial & time redundancy |
| Failure Detection & Isolation | Find and quarantine faulty instances | Health checks, heartbeats, error rate/response time monitoring |
| Recovery | Get back to normal fast | Stop traffic, restart, rollback |

**Key takeaway:** Failures are unavoidable (human, software, hardware) — high availability doesn't come from preventing every failure, but from being fault tolerant: eliminate single points of failure through replication, detect and isolate faulty components quickly, and recover fast (restart/rollback) so failures barely affect the user.
