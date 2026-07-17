# System Constraints — Notes

## What Are System Constraints?

- Once functional requirements are defined, we usually have a lot of freedom in how to structure the system — especially around quality attributes, where trade-offs are expected.
- A **system constraint** is a decision that has already been made for us (fully or partially), which restricts that freedom.
- Important mindset shift: don't see constraints only as negative (a choice taken away). Sometimes they're actually helpful — they give us a solid starting point. That's why constraints are sometimes called the **"pillars" of software architecture** — since they're usually non-negotiable, we design the rest of the system around them.

## The 3 Types of Constraints

### 1. Technical Constraints
- Things engineers deal with regularly.
- Examples:
  - Locked into specific hardware/cloud vendor (contract or security reasons).
  - Must use a specific programming language (existing in-house frameworks, or hiring difficulty).
  - Must use a specific database/technology.
  - Must support specific platforms, browsers, or OS versions (licensing, cost, or client demand).
- These can look like "just implementation details," but they genuinely affect architecture decisions.
- **Example:** If a company runs everything on-premise (security/financial reasons), cloud-native patterns like auto-scaling or serverless (function-as-a-service) become unavailable or very hard to build.
- **Example:** Supporting old browsers/low-end devices that can't handle certain protocols or data volumes forces you to design a different (simpler) architecture path for them, alongside a richer experience for modern devices.

### 2. Business Constraints
- Engineers can make the technically "right" call, but it may not align with business goals — so the business team imposes constraints.
- Examples:
  - **Limited budget / strict deadline** → leads to very different architecture choices than if you had unlimited time/money. (Startups often use different architectural patterns than large enterprises with big teams and budgets, for this exact reason.)
  - **Must use third-party services** — e.g., an online store may want to focus only on the shopping experience, while outsourcing shipping and billing to third parties. An investment platform may need to integrate with banks, brokers, and fraud-detection services.
- These third-party integrations come with their own APIs and paradigms you now must design around.

### 3. Legal Constraints
- Rules/regulations — can be global or region-specific.
- Examples:
  - **HIPAA** (US) — governs who can access medical/patient data, and what security must be in place.
  - **GDPR** (EU) — governs what data can be collected, stored, shared with third parties, and how long data can be retained.
- Depending on the country and industry, different regulations will shape your architecture.

## 2 Key Considerations When Dealing With Constraints

1. **Don't take any given constraint lightly — question if it's real or self-imposed.**
   - Legal/regulatory constraints usually aren't negotiable.
   - But budget/timeline constraints from business *might* be negotiable — deadlines could be pushed, budgets stretched.
   - Being locked to specific hardware/vendor/tech might just be an assumption worth challenging — this project could be the right time to explore alternatives.
   - Once you accept a constraint and build around it, it's very hard to walk back. If it turns out later the constraint wasn't actually real, your whole architecture may no longer make sense.

2. **Leave room to move away from constraints in the future.**
   - Even if you must use a specific database or third-party service now, avoid tightly coupling your system to that specific technology/API.
   - This way, if you need to switch later, it's a small change — not a full re-architecture.
   - (Techniques for this kind of decoupling are covered later, under architectural building blocks.)

## Quick Recap

| Type | Comes From | Example |
|---|---|---|
| Technical | Engineering/infra decisions | Locked to a cloud vendor, must support old browsers |
| Business | Business team's goals | Limited budget, must use third-party shipping/billing |
| Legal | Government regulations | HIPAA (US healthcare), GDPR (EU data privacy) |

**Key takeaway:** System constraints are decisions already made for us that restrict our design freedom — but instead of resenting them, treat them as a starting foundation. Just make sure to verify which constraints are truly non-negotiable, and always design with enough flexibility to move away from a constraint later without a full rebuild.
