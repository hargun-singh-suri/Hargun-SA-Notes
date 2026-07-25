# SLA, SLO, SLI — Notes

These three terms aggregate the promises we make to users about our quality attributes.

## 1. SLA (Service Level Agreement)

- A **legal contract** between the service provider and clients/users.
- States the promises around quality (availability, performance, data durability, incident response time, etc.).
- Explicitly states **penalties** for failing to deliver — full/partial refunds, subscription extensions, service credits, etc.
- **Who it applies to:**
  - Almost always exists for **paying external users**.
  - Sometimes exists for **free external users** (e.g., during a free trial — if the service has major issues, you might extend the trial or give credits).
  - Occasionally exists for **internal users/teams** within the company — but usually without penalties.
  - Most fully-free services avoid publishing any SLA at all, to avoid strict public commitments.
- Internal teams that build on top of your service need to know **your** SLA to be able to meet **their own** SLA to external customers.

## 2. SLO (Service Level Objective)

- An **individual goal/target** your system commits to hitting — a specific value or range.
- **Examples:**
  - Availability SLO: three nines (99.9%).
  - Response time SLO: under 100ms at the 90th percentile.
  - Issue resolution time SLO: between 24–48 hours.
- All the quality attribute requirements gathered during initial design eventually become one or more SLOs.
- **Relationship to SLA:** an SLA is essentially a collection of SLOs bundled into one legal document. If a system has no SLA, it should still have SLOs — otherwise users (internal or external) have no idea what to expect from the system.

## 3. SLI (Service Level Indicator)

- A **quantitative, measured value** — the actual number you track via monitoring/logs — showing how well you're meeting an SLO.
- **Examples:**
  - % of requests that got a successful response → used as an indicator for the availability SLO.
  - Collected response times, bucketed and turned into percentile distributions → compared against the response time SLO (e.g., 100ms at 90th percentile).
- **Why quality attributes must be measurable:** if you can't measure something, you can't create an SLI for it — and without an SLI, you can't prove you're meeting your SLO — and without that, you can't prove you're honoring your SLA (a legal contract).

## Who Defines What

- **SLA** — crafted by business and legal teams.
- **SLOs and SLIs** — engineers/architects have much more control here.

## 4 Key Considerations When Defining SLOs

1. **Pick metrics users actually care about first, then define SLOs around them** — don't just create an objective for every metric you're capable of measuring.
2. **Fewer SLOs is better.** Too many makes prioritization hard. With just a few, you can focus your whole architecture around meeting them properly.
3. **Set realistic goals with a safety buffer — don't over-promise.**
   - Just because your system *can* do 5 nines doesn't mean you should commit to that externally.
   - Committing lower saves cost and leaves room for unexpected issues.
   - **Common pattern:** external SLO looser, internal SLO more aggressive.
     - Example: commit to 99.9% availability externally, but aim for 99.99% internally.
     - This lets you strive for better quality internally while avoiding financial penalties if you fall short of your own (higher) internal bar.
4. **Have a recovery plan ready in advance** for when SLIs show you're missing your SLOs.
   - Decide ahead of time what happens if the system goes down for long periods, performance degrades, or bug reports spike suddenly.
   - Plan should include: automatic alerts to engineers/DevOps, automatic failovers, restarts, rollbacks, auto-scaling policies, and predefined handbooks — so the on-call person doesn't have to improvise during an emergency.

## Quick Recap

| Term | What it is | Who defines it |
|---|---|---|
| SLA | Legal contract with penalties | Business/legal team |
| SLO | Specific target/goal for a metric | Engineers/architects |
| SLI | Actual measured number (from monitoring/logs) | Engineers/architects |

**Key takeaway:** SLA is the legal promise to users (with penalties), SLOs are the specific measurable targets that make up that promise, and SLIs are the real numbers you track to prove you're hitting those targets — and good SLO design means picking only the metrics users truly care about, keeping the list short, setting a safety buffer, and having a recovery plan ready before things go wrong.
