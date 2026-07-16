# System Requirements — Notes

## Why System Requirements Matter

- "System requirements" simply means figuring out exactly what we need to build for the client.
- As engineers, we're used to getting requirements for small things (a method, a class, an algorithm). Designing a **large scale system** is different in two big ways.

### Difference 1: Scope and Abstraction
- For a small task (like a method), we usually know the exact input/output.
- For a big system (like a file storage app, video streaming service, or ride-sharing app), there are countless ways to solve it, and the scope is so huge it's hard to even picture the final result.
- This is why designing a whole system can feel overwhelming at first.

### Difference 2: High Ambiguity
- **Reason 1:** Requirements often come from a non-technical person (client/product manager) who describes things in a high-level, vague way. It's our job to convert this into precise technical requirements.
- **Reason 2:** Sometimes the client themselves doesn't fully know what they need — they only know the *problem* they want solved. Asking the right questions **is part of the solution**.

**Example:** Client wants a "hitchhiking service" (join drivers already going somewhere).
Questions we must ask:
- Real-time service or book in advance?
- Mobile, desktop, or both?
- Should payment happen inside our app, or directly between rider and driver?

This is exactly why system design interviews test your ability to ask clarifying questions — it shows you know how to narrow down a vague problem.

## Why Getting Requirements Right Upfront Is Critical

- Unlike small code changes, large systems can't be casually rewritten overnight.
- Big systems need many engineers (sometimes multiple teams), take months to build, and cost significant engineering time.
- Often involves buying hardware/software licenses upfront, and signing contracts with deadlines and financial commitments.
- Missing deadlines or delivering late can permanently damage the company's reputation.
- **Bottom line:** Because change is expensive and slow at this scale, getting requirements right *before* building is very important.

## The 3 Types of Requirements (a.k.a. "Architectural Drivers")

These three types shape our design decisions — they narrow down endless possibilities into one solution that fits the client's needs.

### 1. Functional Requirements (Features)
- Describe **what the system does** — its behavior.
- Think of the system as a black box: user input/events go in, and results/actions come out.
- Functional requirements do **not** decide the architecture — almost any architecture can deliver the same feature.

**Example (hitchhiking app):**
- Input: rider logs into the app → Output: system shows a map with nearby drivers within 5 miles.
- Input: ride is completed → Output: system charges rider's card and pays the driver (minus fees).

### 2. Non-Functional Requirements (Quality Attributes)
- Describe **what qualities the system must have**, not what it does.
- Examples: scalability, availability, reliability, security, performance.
- Unlike functional requirements, these **do decide the architecture**. Different architectures give different quality results.
- Simple way to remember: *the architecture defines the quality attributes.*

### 3. System Constraints
- The real-world limitations we must design around.
- Examples: tight deadlines, limited budget, small engineering team.
- These force trade-offs and push us toward certain architectural choices we wouldn't otherwise make.

## Quick Recap

| Type | Answers the question | Affects architecture? |
|---|---|---|
| Functional Requirements | What does the system do? | No |
| Quality Attributes | What properties must it have? | Yes |
| System Constraints | What limits do we have? | Yes (forces trade-offs) |

**Key takeaway:** Gathering requirements isn't just a first step before design — asking the right questions IS part of designing the solution. Get this stage right, because large systems are costly and slow to change later.
