# Availability (Quality Attribute) — Notes

## Why Availability Matters

**From the user's perspective:**
- Imagine trying to buy something online and the page won't load, or checkout fails with an error — pure frustration.
- If you're an email provider and everyone loses access for hours or a day, the impact is huge.
- If you provide services to *other companies* (B2B), the impact compounds — one outage affects many businesses at once.
  - **Real example:** AWS S3 outage (Feb 2017) — took down 100,000+ websites since so many companies depended on it. Nearly broke a chunk of the internet.
- In mission-critical systems (air traffic control, hospital patient management), downtime isn't just inconvenient — **lives can be on the line**.

**From the business perspective — two risks:**
1. If users can't use the system, ability to make money drops to zero.
2. If downtime is too long or too frequent, users eventually leave for competitors.
- So: system downtime = risk of losing both **money** and **customers**.

## Formal Definition

- **Availability** = the fraction of time (or probability) that a service is operationally functional and accessible to the user.
- **Uptime** = time system is functional/accessible.
- **Downtime** = time system is unavailable.
- Formula: **Availability = Uptime / (Uptime + Downtime)**, expressed as a percentage.
- Note: "Uptime" and "Availability" are often used interchangeably in agreements/contracts.

## Alternative Way to Measure/Estimate: MTBF & MTTR

- **MTBF (Mean Time Between Failures)** — average time the system stays operational before failing.
  - Useful when dealing with multiple hardware components (computers, routers, hard drives), each with its own lifespan.
  - Not usually needed for existing cloud services/third-party APIs — they publish their own uptime/availability numbers directly.
- **MTTR (Mean Time to Recovery)** — average time to detect and recover from a failure (essentially = average downtime).
- **Formula:** Availability = MTBF / (MTBF + MTTR)
- **Key insight:** This formula shows something the uptime/downtime formula doesn't make as obvious — if you can shrink MTTR (detect + recover) close to zero, you approach 100% availability, *regardless of how often failures happen (MTBF)*.
  - In practice, recovery time can never be truly zero — but this shows why **fast detection and fast recovery** are a real path to high availability.

## How Much Availability Is "Enough"?

100% sounds ideal to users, but practically impossible — it would leave zero room for planned or emergency maintenance.

| Availability | Downtime per day | Downtime per year |
|---|---|---|
| 90% | ~2.4 hours | ~36 days |
| 95% | ~1 hour | ~18 days |
| 99.9% ("three nines") | ~1.5 minutes | ~8 hours |

- 90% and 95% both sound high but translate to weeks of downtime per year — clearly not "high availability."
- Industry standard set by cloud vendors: generally **99% to 100%**, typically **99.9% or higher**.
- At 99.9%: downtime is barely noticeable day-to-day (~1.5 min/day), but still allows ~8 hours/year for planned maintenance if needed.

## The "Nines" Shorthand

- 99.9% = "three nines"
- 99.99% = "four nines"
- More nines = higher availability standard.

## Quick Recap

| Concept | Meaning |
|---|---|
| Availability | Uptime / (Uptime + Downtime), as a % |
| MTBF | Average time between failures |
| MTTR | Average time to detect + recover from failure |
| High Availability | Generally 99.9% ("three nines") or above |

**Key takeaway:** Availability measures how much of the time your system is actually usable — both formulas (uptime/downtime, and MTBF/MTTR) describe the same idea, but MTTR specifically shows that fast failure detection and recovery is a direct path to high availability. Industry standard for "high availability" is 99.9% or higher.
