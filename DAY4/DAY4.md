# Quality Attributes (Non-Functional Requirements) — Notes

## Why This Topic Matters

- Studies show systems often get redesigned not because features are missing, but because the system is too slow, can't scale, or is too hard to maintain/secure.
- After such redesigns, the system usually ends up doing the **same functionality** as before — just built differently.
- So getting quality attributes right from the start saves you from painful, expensive redesigns later.

## What Are Quality Attributes?

- Quality attributes = non-functional requirements that describe the **quality/property** of the system, not what it does.
- They don't say "what" the system does — they measure "how well" it does it, on some dimension.
- Direct effect: quality attributes **shape the architecture** (unlike functional requirements, which don't).

**Examples:**
- Functional requirement: "When user clicks search, show list of products."
- Quality attribute (performance dimension): "...within 100 milliseconds."
- System-wide example (availability dimension): "Online store must be available 99.9% of the time."
- Another system-wide example (deployability dimension): "Dev team must be able to deploy a new version at least twice a week."

Note: quality attributes aren't just for the client — they must satisfy **all stakeholders** (e.g., the dev team's need for frequent deployments).

## 3 Key Considerations When Defining Quality Attributes

1. **Must be measurable and testable.**
   - If you can't objectively measure it, you can't know if the system is actually meeting it.
   - Example: "Confirmation must display quickly" is not measurable. "Within 200ms" is — but only if you first define what "quickly" means in numbers.

2. **No single architecture can satisfy every quality attribute — trade-offs are required.**
   - If one architecture could deliver everything, there'd be no need for architects.
   - Some quality attributes conflict with each other, so you must prioritize.
   - Example: Login should be fast (under 1 second) vs. login should be secure (needs password + SSL). Security measures slow down login — so speed and security here are in direct conflict. The architect's job is to pick the right trade-off based on business needs.

3. **Feasibility — make sure the requirement is actually achievable.**
   - Clients (often non-technical) may ask for things that are technically impossible or too expensive.
   - It's the architect's job to flag this early.
   - Examples of unfeasible requirements:
     - Unrealistic low latency (e.g., guaranteeing <100ms when network latency alone is 100-150ms between regions).
     - 100% availability (means the system can never go down, even for maintenance).
     - Full protection against hackers, streaming HD video where bandwidth doesn't support it, or unreasonable storage growth expectations.
   - Sometimes you need to consult domain experts before approving a requirement, to confirm it's actually deliverable.

## Quick Recap

| Consideration | Why it matters |
|---|---|
| Measurable & testable | Without a number/metric, you can't verify success or failure |
| Trade-offs required | No architecture satisfies all quality attributes at once — some conflict |
| Feasibility | Some requirements are technically impossible or too costly — flag them early |

**Key takeaway:** Quality attributes tell you *how well* a system should perform (not what it does), they directly shape the architecture, and defining them well requires making sure they're measurable, involve smart trade-offs, and are actually feasible to deliver.
