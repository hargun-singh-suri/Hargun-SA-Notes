# Designing APIs — Notes

## What Is an API and Why We Need It

- Once functional requirements are captured, we can think of our system as a **black box** — it has behavior, and needs a well-defined interface for others to interact with it.
- That interface = a **contract** between the engineers who build the system and the client applications that use it.
- Since large-scale systems are called remotely over a network (not like calling a class/library directly), this interface is called an **Application Programming Interface (API)**.
- **Who calls it:** front-end clients (mobile apps, browsers), other backend systems (even from other companies), or internal systems within your own org.
- Once you break your system into components, **each component gets its own API** too, used internally by other parts of the system.

## 3 Categories of APIs

1. **Public APIs** — open to any developer/general public.
   - Best practice: require registration before allowing requests. Gives better control, security, and the ability to blacklist bad actors.
2. **Private/Internal APIs** — only exposed within the company.
   - Lets other internal teams use your system to build more value, without exposing it outside the org.
3. **Partner APIs** — similar to public APIs, but restricted to companies/users with an actual business relationship (customer agreement, subscription, etc.).

## Benefits of a Well-Defined API

1. Clients can use your system to grow their business **without needing to know your internal implementation**.
2. Clients don't need to wait for you to finish building the system — once the API contract is defined, they can start integration work in parallel.
3. Defining the API early makes it easier to design your system's internal architecture — since the API defines the actual endpoints/routes users can take.

## Best Practices for Designing a Good API

### 1. Full Encapsulation
- Completely hide internal design/implementation from the client.
- If a client needs to know internal business logic to use your API, the API has failed at its job.
- Also keeps the API decoupled from implementation — so you can change internals anytime without breaking the contract with existing clients.

### 2. Easy to Use, Hard to Misuse
- Only one way to get a piece of data or perform a task (not multiple confusing paths).
- Descriptive names for actions/resources.
- Expose only what the user actually needs — nothing more.
- Stay consistent across the whole API.

### 3. Idempotent Operations (Where Possible)
- **Idempotent** = performing the operation multiple times has the same effect as performing it once.
  - Example (idempotent): Updating a user's address — same result whether done 1 time or 10 times.
  - Example (NOT idempotent): Incrementing a user's balance by $100 — result changes every time you repeat it.
- **Why this matters:** APIs work over an unreliable network — a request or its response can get lost, or a system component can crash mid-request. The client has no way to know exactly what happened.
- If the operation is idempotent, the client can simply **retry/resend the same request safely** without worrying about bad side effects.

### 4. Pagination
- Needed when a response would otherwise contain a huge payload/dataset.
- Without it: clients can't handle the data well, or the user experience is terrible.
  - **Example:** imagine opening your email and seeing every email you've ever received on one page, instead of the last 10-20.
- **How it works:** client requests a small segment by specifying max size per response + an offset in the overall dataset. To get the next chunk, just increment the offset.

### 5. Asynchronous APIs (For Long-Running Operations)
- Some operations (big reports, large data analysis/aggregation, compressing large video files) take a long time, and there's no meaningful partial result to paginate through — only a final result matters.
- **Pattern:** client gets an immediate response (not the final result) — usually containing an identifier they can use to track progress and eventually fetch the final result once ready.

### 6. API Versioning
- Ideal API design lets you change internals without ever touching the API contract — but in practice, you'll sometimes need to make **non-backward-compatible changes**.
- Solution: explicitly version your API, maintain two versions simultaneously, and gradually deprecate the old one with proper communication to clients still using it.

## Quick Recap

| Practice | Why it matters |
|---|---|
| Encapsulation | Clients don't need internal knowledge; internals can change freely |
| Ease of use | Fewer ways to misuse the API, consistent and clear |
| Idempotency | Safe retries over unreliable networks |
| Pagination | Handles large datasets without overwhelming the client |
| Asynchronous APIs | Handles long-running operations without blocking the client |
| Versioning | Allows safe evolution without breaking existing clients |

**Key takeaway:** An API is a contract that lets others use your system without needing to know how it works internally. Design it around 3 categories (public/private/partner) based on who's using it, and follow best practices — encapsulation, simplicity, idempotency, pagination, async operations for long tasks, and versioning — to keep it easy to use and future-proof.
