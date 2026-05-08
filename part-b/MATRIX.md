# Impact vs Effort Matrix

|                   | Low Effort         | High Effort        |
|-------------------|--------------------|--------------------|
| **High Impact**   | Problem 2, Problem 3| Problem 1          |
| **Low Impact**    | Problem 4          | Problem 5, Problem 6|

## How I Scored Each Dimension

### Impact Scoring (1–5)
I scored Impact based on:
- Number of users affected (from Part A frequency)
- Whether the problem is in the core booking flow
- Severity of consequence for the user

### Effort Scoring (1–5)
I scored Effort based on:
- Number of system components touched
- Whether new infrastructure is required
- Risk of breaking existing flows
- Railway API dependencies

---

## Placement Justifications

### Problem 1: Tatkal Booking Crashes — High Impact / High Effort
Impact: High — Tatkal failures occur daily at 10:00 and affect urgent travellers (Part A frequency). Effort: High — requires new queuing infra, real-time channels, and careful inventory reconciliation with Railway APIs. Prioritisation: High but not a quick win; needs a focused sprint and staging plan.

### Problem 2: Search Filters Do Not Work Reliably — High Impact / Low Effort
Impact: High — affects nearly all search sessions and compromises trust. Effort: Low — mostly frontend improvements (persist state, URL encoding) with minimal backend changes. Prioritisation: Quick win: high ROI for small effort.

### Problem 3: Seat Selection Resets — High Impact / Low Effort
Impact: High — directly breaks booking integrity for berth-sensitive users. Effort: Low to Medium — introduce a lightweight booking draft and a lock API; can be phased in. Prioritisation: Quick win alongside filters.

### Problem 4: Login Modal Interrupts Search — Low Impact / Low Effort
Impact: Low to Medium — mostly affects exploration and bounce but not core booking for returning users. Effort: Low — UX change (banner vs modal). Prioritisation: Low-cost improvement to reduce friction.

### Problem 5: Core Status Links Break Context — Low Impact / High Effort
Impact: Low to Medium — useful but not critical to booking success. Effort: High — backend proxying or embedding and session glue work required. Prioritisation: Postpone until booking stability improvements are in.

### Problem 6: Primary Booking Action Is Buried — Low Impact / High Effort
Impact: Medium — affects first impressions and conversion but less urgent than transactional failures. Effort: High — design, A/B testing and stakeholder alignment required. Prioritisation: Plan for a later redesign sprint.

---

## Recommended Sprint Order
1. Problem 2 — Reliable Filters (Quick Win; low effort, high impact)
2. Problem 3 — Preserve Seat Selection (Quick Win; reduces booking failures)
3. Problem 1 — Tatkal Queue (High impact; build after quick wins to stabilize core flows)
4. Problem 4 — Non-blocking Search (Low effort UX improvement)
5. Problem 5 — In-app PNR & Chart (Backend work; schedule later)
6. Problem 6 — Homepage Redesign (Design + A/B testing; schedule in a dedicated UI sprint)
# Matrix

To be filled in Part B.
