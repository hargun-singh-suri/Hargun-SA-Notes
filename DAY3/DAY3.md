# Capturing Functional Requirements — Notes

## Why We Need a Formal Method

- One simple (but weak) way to gather requirements: just ask the client to describe everything the system should do.
- Problem: for complex systems with many features and multiple actors, the client will likely **forget details**. This approach doesn't scale.
- A better, more structured way is through **use cases** and **user flows**.

## Key Terms

- **Use case:** A specific situation/scenario where the system is used to achieve a user's goal.
- **User flow:** A detailed, step-by-step (often graphical) breakdown of a use case — showing exactly what happens.

## The 3-Step Formal Process

1. **Identify all actors/users** in the system.
   - If you skip this, you'll likely miss important use cases tied to a specific type of user.
2. **Describe all use cases** — every possible scenario where an actor interacts with the system.
3. **Expand each use case into a flow of events** — the back-and-forth interactions between actor and system. For each interaction, note the **action** and the **data** flowing in/out.

**Example (hitchhiking service):**
- Actors: Driver, Rider (only 2 in this case).
- Use cases: rider registration, driver registration, user login, driver going online to pick up riders, successful match (ride happens), unsuccessful match (no driver found).

## Visualizing a Use Case: Sequence Diagram

- To expand a use case into a flow, we use a **sequence diagram** — part of UML (Unified Modeling Language), a standard for visualizing system design.
- Note: UML isn't strictly followed in the industry, but sequence diagrams are still commonly used to show interactions between entities (not just objects).
- **How to read it:** time flows top to bottom. Each entity (actor/system) is a vertical line. Arrows = calls/messages. Broken/dashed arrows = responses going back.

## Walkthrough: "Successful Match" Use Case

1. Driver (already logged in) tells the system: "I'm on this route, ready to pick up a rider."
2. Rider (already logged in) tells the system: origin, destination, and that they need a driver.
3. System tries to **match** the rider with a driver. If matched, both driver and rider get notified.
4. Driver reaches rider's location → ride starts → rider gets a "ride started" confirmation.
5. Driver reaches destination → ride ends → system is notified.
6. System charges the rider's bank account and sends a receipt.
7. System takes its service fee, credits the rest to the driver's account.
8. Driver gets notified about the new credit.

*(Note: the diagram itself doesn't show the exact data in each message, but that data is still tracked separately for later use.)*

## Bonus Benefit

- Once you map out this user flow, you can almost directly derive the **future API** of the system.
- Each interaction between actor and system = one API call.
- The data flowing in each interaction = the arguments of that API call.
- (API design itself is covered in a separate lecture.)

## Quick Recap

| Step | What it does |
|---|---|
| 1. Identify actors | Find every type of user interacting with the system |
| 2. List use cases | Find every scenario each actor can trigger |
| 3. Expand into flow | Break each use case into step-by-step interactions (visualized via sequence diagram) |

**Key takeaway:** Instead of hoping the client remembers every detail, use a structured method — actors → use cases → sequence diagrams — to capture functional requirements completely and clearly, and get a free byproduct: your future API design.
