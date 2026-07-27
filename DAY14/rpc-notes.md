# RPC (Remote Procedure Call) — Notes

## What Is RPC?

- RPC = the ability for a client application to execute a function/subroutine on a remote server.
- **Unique feature:** calling a remote method looks and feels exactly like calling a normal *local* method in code — this is called **local transparency**.
- Many (not all) RPC frameworks also support **multiple programming languages**, so apps written in different languages can still talk to each other via RPC.

## How RPC Works (Step by Step)

1. The API and its data types are defined using an **Interface Description Language (IDL)** — a schema definition of the client-server communication (varies by framework).
2. A **compiler/code generator** (part of the RPC framework) uses this definition to auto-generate two implementations:
   - **Server stub** — server-side implementation.
   - **Client stub** — client-side implementation.
   - These stubs handle all the underlying communication details.
3. Custom object types defined in the IDL get compiled into classes/structs — these are called **DTOs (Data Transfer Objects)**.
4. **At runtime:**
   - Client calls the RPC method → client stub **serializes/marshals** the data (encodes it).
   - Client stub sends the data over the network to the server stub.
   - Server stub **deserializes** the data and invokes the actual method implementation on the server.
   - Server finishes → result passed back to server stub → server stub serializes the response → sends back to client.
   - Client stub deserializes the response → returns it to the caller, looking just like a normal local method's return value.

## Why This Design Is Useful

- Once the API is defined via the IDL and published, the client and server are **completely decoupled**.
- Server team can generate their server stub once implementation is done.
- Any new client just needs the published API definition to generate their own client stub — no need to coordinate directly with the server team.
- If the framework supports multiple languages, the server team isn't locked into matching the client's language choice (or vice versa).

## Benefits of RPC

1. **Convenience** — client developers just call methods on objects like normal local code; all networking/communication details are abstracted away.
2. **Simple failure handling** — communication failures show up as regular errors/exceptions, just like normal method calls.
3. **Local transparency** — remote calls feel like local calls in code.

## Drawbacks of RPC

### 1. Slowness
- Remote calls are much slower than local method calls — but they *look* just as fast/simple in code, which can surprise developers with hidden performance bottlenecks.
- **Mitigation:** provide asynchronous versions of slow methods (same idea covered in the API best-practices lecture).

### 2. Unreliability
- The client runs remotely (sometimes on another company's infrastructure) and depends on the network — this introduces uncertainty.
- **Classic problem example:** A credit card company's "debit account" API is called by an online store. If the call fails/times out:
  - Retry → risk charging the user twice.
  - Don't retry → risk not charging the user at all.
  - The client has no way to know if the server actually processed it and only the acknowledgment got lost, or if the server crashed before processing.
- **Mitigation:** Can't fully eliminate this, but **making operations idempotent** (from the earlier API best-practices lecture) reduces the risk — safe retries become possible.

## When to Use RPC (and When Not To)

- **Great fit for:**
  - Communication between two backend systems (very common use case).
  - Communication between internal components within a large-scale system.
  - Situations where you want to fully abstract away network details and focus purely on "actions" the client performs.
- **Less common for:** frontend clients like web browsers (some frameworks support it, but it's not typical).
- **Not a good fit when:**
  - You want direct access to things like HTTP cookies/headers — RPC abstracts the network away, so this doesn't fit well. Other API styles suit this better.
  - Your API is more **data/resource-centric** with simple CRUD operations — RPC revolves around **actions** (each action = a new method with its own name/signature, and you can define as many as you want). A different style is better suited for data-centric APIs (covered in the next lecture).

## Quick Recap

| Component | Role |
|---|---|
| IDL (Interface Description Language) | Defines API methods + data types (schema) |
| Client Stub | Auto-generated client-side code; handles serialization + network calls |
| Server Stub | Auto-generated server-side code; handles deserialization + invokes real method |
| DTO (Data Transfer Object) | Auto-generated classes/structs from custom types in the IDL |

**Key takeaway:** RPC lets a client call a remote method as if it were local (local transparency), using auto-generated client/server stubs from an interface definition. It's great for backend-to-backend and internal component communication, but comes with real trade-offs — slowness (hidden behind simple-looking code) and unreliability (network failures create ambiguous retry situations) — both of which are mitigated using async methods and idempotent operations.
