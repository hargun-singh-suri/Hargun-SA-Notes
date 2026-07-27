# REST API — Notes

## What Is REST?

- **REST** = Representational State Transfer — introduced by Roy Fielding's 2000 dissertation.
- It's **not** a standard or protocol — it's an **architectural style**: a set of constraints and best practices for designing web APIs.
- An API following REST constraints is called a **RESTful API**.
- Designed to make APIs easy to use/understand, while naturally supporting scalability, high availability, and performance.

## REST vs RPC (Key Contrast)

| | RPC | REST |
|---|---|---|
| Core abstraction | Methods (actions) | Named resources |
| To add functionality | Add more methods | Usually reuse a small fixed set of operations on resources |
| Interface | Static, defined ahead of time | Dynamic (via hypermedia links) |

- In REST, the client requests a **named resource**, and the server responds with a **representation of that resource's current state**.
- The resource itself can be implemented completely differently internally (spread across multiple DB tables, files, external services) — the client never needs to know. The representation is just an abstraction.
  - **Example:** A news site's homepage (the resource) might be built from many different backend tables/services, but the client just gets back a webpage with title, articles, and pictures — the representation.

## Dynamic Nature: HATEOAS

- **HATEOAS** = Hypermedia As The Engine Of Application State.
- Unlike RPC (where all actions are statically fixed upfront), REST responses come with **hypermedia links** — the client follows these links to discover what it can do next / progress its state.
- **Example:** Requesting your messages in a chat/email app returns not just the messages, but also links describing further actions/resources you can access from there.

## How REST Achieves Performance, Scalability & Availability

1. **Statelessness** — the server keeps **no session information** about any client. Every request is handled in isolation, without depending on any previous request.
   - Benefit: since there's no session tied to a specific server, you can run a group of servers and freely spread load across them — the client won't notice or care which machine handled which request. This directly supports scalability and availability.
2. **Cacheability** — every response must be explicitly (or implicitly) marked as cacheable or non-cacheable.
   - Benefit: cached responses avoid unnecessary round trips to the server, speeding things up for the client and reducing load on the system.

## Resources: Structure and Naming

- Each resource is named/addressed via a **URI**.
- Resources form a **hierarchy**, represented with forward slashes (`/`).
- Two types of resources:
  1. **Simple resource** — has a state, and optionally sub-resources.
  2. **Collection resource** — contains a list of resources of the same type.

**Example (movie streaming service):**
- `movies` = collection resource.
- Each `movie` = simple resource, which can itself have sub-resource collections: `directors`, `actors`.
- Each `actor` = simple resource, which can have sub-resources like `profile picture`, `contact information`.

- A resource's representation can be many things: an image, a link to a stream, a JSON object, an HTML page, binary data, even executable JS code.

## Best Practices for Naming Resources

1. **Use nouns only** for resource names — this keeps a clear separation between resources (nouns) and actions (verbs, handled via HTTP methods).
2. **Distinguish collections vs simple resources** — plural names for collections (`movies`, `actors`), singular for simple resources.
3. **Use clear, meaningful names** — avoid overly generic names like `elements`, `items`, `entities`, `objects` — these are ambiguous and depend too much on context.
4. **Resource identifiers should be unique and URL-friendly** — safe to use directly in URLs.

## Operations on Resources (Mapped to HTTP Methods)

REST limits operations to a small fixed set, unlike RPC's unlimited custom methods:

| Operation | HTTP Method |
|---|---|
| Create a new resource | POST |
| Update an existing resource | PUT |
| Delete an existing resource | DELETE |
| Get current state (or list, for collections) | GET |

**HTTP semantics give useful guarantees:**
- **GET is "safe"** — never changes resource state.
- **GET, PUT, DELETE are idempotent** — repeating them has the same effect as doing them once.
- **GET is cacheable by default.** POST responses can be made cacheable too, via HTTP headers.
- Custom methods beyond these are possible but uncommon.
- Data sent with POST/PUT is typically in **JSON** format (XML is also acceptable, but less common now).

## Step-by-Step: Designing a REST API (Movie Streaming Example)

1. **Identify entities** in the system → become resources. (Example: users, movies, reviews, actors.)
2. **Map entities to URIs**, organizing them in a hierarchy based on relationships:
   - `users`, `movies`, `actors` = independent collection resources.
   - `reviews` = sub-resource under `movies` (since each review belongs to one specific movie).
3. **Choose a representation** for each resource — most commonly **JSON**.
   - Example: `movies` collection = an object with an array mapping movie names to movie IDs — makes it easy to search by name and get the ID.
   - A single movie resource object contains full details, plus hypermedia links (e.g., to play the movie, get its reviews, get its actors) — this is the HATEOAS part in action.
4. **Assign HTTP methods to actions** on each resource:
   - `POST /users` → register a new user (response includes new user ID).
   - `GET /users/{id}` → get all info for that user (e.g., favorite movies, watchlist).
   - `PUT /users/{id}` → update user profile/preferences.
   - `DELETE /users/{id}` → remove the user entirely.
   - Repeat this process for all other resources in the system.

## Quick Recap

| Concept | Meaning |
|---|---|
| Resource | Named entity, addressed via URI |
| Statelessness | No session info kept on server — supports scaling |
| Cacheability | Responses marked cacheable/non-cacheable — reduces load & latency |
| HATEOAS | Dynamic hypermedia links guide client's next actions |
| CRUD → HTTP | POST=Create, GET=Read, PUT=Update, DELETE=Delete |

**Key takeaway:** REST is a resource-oriented architectural style (not a protocol) where clients interact with named resources through a small fixed set of HTTP methods (GET/POST/PUT/DELETE). Statelessness and cacheability are what make REST naturally scalable and highly available, and a good REST API design follows a clear process: identify entities → map to URIs → choose representations → assign HTTP methods.
