# Scalability (Quality Attribute) — Notes

## Why Scalability Matters

- Traffic/load on a system is never constant. Patterns can be:
  - **Seasonal** — e.g., retail sites spike during holidays/sales.
  - **Event-driven** — e.g., search engines spike during a global news event.
  - **Daily patterns** — e.g., news sites or workplace tools see different traffic on weekdays vs weekends, day vs night.
  - **Long-term growth** — if business is doing well, expect steadily rising traffic over time.
- From the previous lecture (Performance): every system has a **degradation point** — where performance starts getting worse as load increases.
- Comparing two system designs: if both get the same budget/time to upgrade, and one can handle much higher load afterward than the other, that one is **more scalable** — same effort, better result.

## Formal Definition

- **Scalability** = the system's ability to handle a growing amount of work in an easy and cost-effective way, by adding resources.
- If you plot "work the system can handle" vs "resources added":
  - **Best case (rare in practice):** linear scalability — double the resources → double the capacity.
  - **Realistic case:** scaling gives diminishing returns.
  - **Worst case:** adding more resources actually makes performance worse (due to overhead of managing/coordinating those resources).

## The 3 Scalability Dimensions (Orthogonal — can use 1, 2, or all 3)

### 1. Vertical Scalability ("Scaling Up")
- Upgrade the existing single machine — faster CPU, more memory, better network card, or move the database to stronger hardware.
- **Pros:**
  - Almost any application benefits without code changes.
  - Migration is simple — move to a stronger machine when traffic is high, move to a weaker (cheaper) one when traffic is low. Especially easy with cloud providers.
- **Cons:**
  - Hard limit — you can only upgrade hardware so much, and internet-scale systems hit that ceiling fast.
  - Bigger issue: it locks you into a **centralized system**, which can't provide high availability and fault tolerance (two important quality attributes covered later).

### 2. Horizontal Scalability ("Scaling Out")
- Instead of upgrading one machine, add more machines/instances running the same service — spreading load across a group.
- Same idea applies to databases — run across multiple machines instead of one.
- **Pros:**
  - Virtually no scaling limit — just keep adding (or removing) machines as needed.
  - If designed correctly, gives high availability and fault tolerance "out of the box."
- **Cons:**
  - Not every application can easily run as multiple instances — may need significant code changes (but usually only once; after that, adding/removing machines is easy).
  - Increased complexity and coordination overhead between instances.

### 3. Team / Organizational Scalability
- Scalability isn't just about handling traffic — it's also about a **team's ability to keep delivering work** (features, bug fixes, releases) as more engineers join.
- On one big monolithic codebase: productivity increases with more engineers... up to a point. After that point, productivity actually **decreases**. Reasons:
  - Meetings become more frequent and crowded → less productive.
  - More engineers working simultaneously → constant code merge conflicts.
  - New developers take longer to onboard as the codebase grows.
  - Testing becomes slow and risky — no isolation means any small change could break something, so everything needs re-testing.
  - Releases become risky (too many changes bundled together) → companies release less often → which makes each release even riskier (a vicious cycle).
- **Architectural fix:**
  1. Split codebase into **modules/libraries** — helps some, but they're still tightly coupled at release time (same service).
  2. Split into **separate services** — each with its own codebase, stack, and release schedule, communicating over loosely coupled protocols. This is what really unlocks team scalability.
  - (Breaking a monolith into services isn't free — trade-offs covered later under architectural patterns.)

## Quick Recap

| Dimension | Focus | Achieved By |
|---|---|---|
| Vertical (Up) | Performance | Upgrading a single machine's hardware |
| Horizontal (Out) | Performance | Adding more machines/instances |
| Team/Organizational | Productivity | Splitting monolith into independent services |

**Key takeaway:** Scalability means handling more work cost-effectively by adding resources — and it has 3 independent dimensions: scaling up (vertical), scaling out (horizontal), and scaling the team (splitting into services so more engineers can work without stepping on each other).
