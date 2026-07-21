# Performance (Quality Attribute) — Notes

## Why Focus on This

- Quality attributes list is huge, but not all matter equally for every system.
- This lecture (and the next few) focus on a small set of the most important quality attributes that matter across almost any large scale system.
- First one: **Performance**.

## Metric 1: Response Time

- **Response time** = time between a client sending a request and receiving a response.
- Made up of 2 parts:
  1. **Processing time** — time your system actually spends working on the request (code, database, business logic, building/sending response).
  2. **Waiting time** — time the request/response spends sitting idle (in network wires, switches, gateways, or software/network queues) — often forgotten, but very real.
- Naming note: some call waiting time "latency." Others use "response time" and "latency" interchangeably. To avoid confusion, response time is sometimes called **end-to-end latency**.
- Response time matters most when the system/subsystem is directly in the **critical path of a user interaction** — users get frustrated fast without quick feedback.
- Important myth to avoid: response time is **not** the ultimate/only performance metric. For some systems, another metric matters more (see below).

## Metric 2: Throughput

- **Example use case:** a distributed logging system ingesting streams of logs from thousands of servers — here, response time isn't the priority; the ability to **ingest and process large volumes of data per unit time** is.
- **Throughput** = either:
  1. Amount of work done per unit time (measured as tasks/requests per second), or
  2. Amount of data processed per unit time (measured in bits/bytes/megabytes per second).
- Higher throughput = higher performance, for systems where this is the priority metric.

## 3 Key Considerations When Measuring Performance

### 1. Measure Response Time Correctly (Include Waiting Time)
- Common mistake: engineers measure only **processing time** (time in code) and mistake it for response time — missing the waiting time entirely.
- This can make performance graphs look "normal" while users actually experience long delays.
- **Example:** Two requests arrive at the same time; system can only process one at a time.
  - Request 1: processed immediately → processing time = 10ms.
  - Request 2: waits for Request 1 to finish, then processes → processing time = 10ms, but real response time = 20ms (10ms processing + 10ms waiting).
  - If you only measure processing time, both look like "10ms" — hiding the real user experience.
  - True average response time here = 15ms, not 10ms.
- This happens anytime the system is even slightly overloaded, not just in a "one request at a time" scenario.

### 2. Look at Response Time Distribution, Not Just Averages
- In practice, across many servers/requests, response times are never identical — you always get a **distribution** (some fast, some slow).
- To analyze this: sort response time samples into a **histogram** (shows frequency of each response time).
- From the histogram, build a **percentile distribution**:
  - The Xth percentile = the value below which X% of samples fall.
  - Example: 20th percentile = 10ms → 20% of requests were under 10ms.
  - 50th percentile = **median** → 50% of requests were under this value.
  - 90th percentile = value under which 90% of requests fall.
- **Why this matters:** averages and medians can completely hide the fact that a chunk of users (e.g., 20%) are having a bad experience.
- This slow chunk at the far end of the distribution is called **tail latency** — the small percentage of response times that take the longest compared to the rest. Goal: keep tail latency as short as possible.
- **Best practice:** set performance goals using **percentiles**, not averages.
  - Example goal: "95th percentile of response times < 30ms" (only 5% of requests allowed to exceed this).
  - More aggressive goal: "99th percentile < 30ms" — leaves a little room for random system jitter.

### 3. Identify the Performance Degradation Point
- Applies to both response time and throughput.
- **Degradation point** = the point on a performance graph where performance starts getting significantly worse as load increases.
- A steep/fast degradation usually means a resource is maxed out. Common culprits:
  - CPU utilization near 100%.
  - High memory consumption.
  - Too many connections — OS/network can't handle the I/O.
  - A software queue at full capacity, unable to keep up with incoming messages.

## Quick Recap

| Metric/Consideration | What it means |
|---|---|
| Response Time | Processing time + Waiting time (both count) |
| Throughput | Work or data processed per unit of time |
| Measure correctly | Don't forget waiting time — it hides real user experience |
| Use percentiles | Averages hide tail latency — set goals with percentiles instead |
| Degradation point | Find where performance breaks down as load increases, and why (CPU, memory, connections, queues) |

**Key takeaway:** Performance isn't just "how fast" — it's response time (including hidden waiting time) AND throughput, and you must measure using percentiles (not averages) to catch the real experience of your slowest users, while also watching for the point where performance collapses under load.
