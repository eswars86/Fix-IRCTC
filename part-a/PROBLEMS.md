# IRCTC Problem Discovery — Part A

## Summary
- Total problems documented: 6 (3 given + 3 self-discovered)
- Platform explored: irctc.co.in live booking flow and related linked pages, as of May 7, 2026
- Device used for this workspace pass: Desktop Chrome
- Evidence style: live flow observation, UI snapshots, and step-by-step reproduction notes

---

## Problem 1: Tatkal Booking Crashes at 10:00 AM [Given]

**What is broken:**
Tatkal booking becomes unstable at the exact moment users need it most. The flow gives almost no usable feedback when demand spikes: users do not get a queue position, a clear progress state, or a meaningful explanation for failure. To the user, the page often feels like it has frozen or silently failed while the quota disappears.

**Affected users:**
Last-minute travellers, migrant workers, students, patients, and anyone who depends on Tatkal for urgent travel. The impact is concentrated on people who can least afford a failed attempt because they are already working against a deadline.

**Frequency:**
Daily and recurring at the 10:00 AM Tatkal opening window. This is not an edge case; it is a predictable peak-load failure pattern.

**Current flow — step by step:**
1. User opens IRCTC around 9:50 AM and prepares for Tatkal booking.
2. User logs in or gets ready to log in so they can move quickly at opening time.
3. User enters source, destination, travel date, class, and quota.
4. User waits for the clock to reach 10:00 AM.
5. User submits search or opens the Tatkal booking flow right when quota becomes available.
6. The system becomes slow, stalls, or fails to continue cleanly under the surge.
7. User sees no queue position, no reliable progress indicator, and no clear recovery path.
8. By the time the page responds again, the Tatkal quota is often already gone.

**Where exactly it breaks:**
Step 6. The booking request hits peak traffic and the interface fails to tell the user what is happening while inventory is consumed.

---

## Problem 2: Search Filters Do Not Work Reliably [Given]

**What is broken:**
Train search filters do not consistently behave like trustworthy controls. When users apply filters such as sleeper class, available seats only, or morning departure, the visible result set does not always update in a way that is easy to verify. On top of that, filter state can be lost or behave inconsistently when navigating back and forth.

**Affected users:**
Regular travellers comparing options across trains, especially people trying to make quick decisions on class, seat availability, or departure time. This hurts users who need the search page to narrow choices fast and accurately.

**Frequency:**
Very frequent. It happens during ordinary search sessions whenever a user refines results or revisits the results page.

**Current flow — step by step:**
1. User opens IRCTC train search.
2. User enters origin and destination.
3. User picks a travel date.
4. User opens the class and availability filters.
5. User applies sleeper class, available seats only, and morning departure filters.
6. User reviews the results and checks whether the list actually changed.
7. User clicks back or changes the filter again.
8. The filter state does not reliably persist or clearly explain what changed.

**Where exactly it breaks:**
Step 6 to step 8. The search results and filter state lose credibility because the UI does not clearly reflect the filter action or preserve it consistently across navigation.

---

## Problem 3: Seat Selection Resets [Given]

**What is broken:**
Seat preference does not reliably carry forward from the seat map into the next booking step. Users choose a berth, but that selection can disappear or fail to show up on the passenger details screen. For people selecting lower berth specifically, that is a direct booking integrity failure.

**Affected users:**
Senior citizens, pregnant travellers, passengers with mobility constraints, families booking for comfort, and anyone who carefully selects a berth for a reason.

**Frequency:**
Recurring in the booking flow, with a higher reset rate reported on mobile. It is most damaging when users are forced to move quickly and cannot re-check every step.

**Current flow — step by step:**
1. User searches for a train and opens the booking flow.
2. User reaches the seat map or berth selection stage.
3. User selects a lower berth.
4. User clicks Proceed to continue booking.
5. User lands on the passenger details page.
6. User checks the selected berth in the next step.
7. The selected berth is missing, reset, or not visibly preserved.
8. User has to correct the selection or risk booking the wrong berth.

**Where exactly it breaks:**
Step 4 to step 6. The handoff from seat map to passenger details does not reliably preserve the berth choice.

---

## Problem 4: Login Modal Interrupts Basic Search [Self-Discovered]

**How I found it:**
I opened the live train search page and started typing source and destination stations. Instead of staying in a lightweight search flow, the page surfaced a login dialog on top of the booking experience.

**What is broken:**
The site interrupts basic search intent with a login modal before the user has finished evaluating trains. This creates an unnecessary gate in the middle of a discovery task and pushes the user away from the immediate goal of checking routes and availability.

**Affected users:**
First-time visitors, casual travellers, and unauthenticated users who only want to compare trains before deciding whether to sign in. It is especially disruptive for users who are still exploring options.

**Frequency:**
High. This appears whenever a user enters the search flow from the public landing page and the site decides to prompt for sign-in.

**Current flow — step by step:**
1. User opens the IRCTC home/search page.
2. User begins entering source station details.
3. User enters destination or starts the booking form.
4. The page surfaces a login dialog over the task.
5. The login dialog steals attention and blocks the main search flow.
6. The user has to close, dismiss, or work around the modal to continue.
7. The search intent is delayed before any train results appear.
8. Some users may abandon the flow entirely because they were not trying to log in yet.

**Where exactly it breaks:**
Step 4. The modal appears too early and interrupts the user before the core search task is complete.

**Screenshot or description:**
Live homepage screenshot captured during exploration showed the booking form in the background with a login modal overlay, while the user was still entering search details.

---

## Problem 5: Core Status Links Break the IRCTC Context [Self-Discovered]

**How I found it:**
From the live booking homepage, I inspected the top utility actions for PNR Status and Charts / Vacancy and followed the links to see whether the user stayed inside the same booking environment.

**What is broken:**
Important utility tasks are split across separate domains and windows instead of staying in one coherent journey. The user leaves the IRCTC booking shell for PNR and chart-related information, which makes the experience feel fragmented and harder to trust.

**Affected users:**
Travellers who just want a quick status check, chart lookup, or train confirmation update without mentally switching products. This also hurts less technical users who expect the same navigation and login state to persist.

**Frequency:**
Common. These links sit at the top of the home page and are likely used repeatedly by regular passengers, especially near departure time.

**Current flow — step by step:**
1. User opens the IRCTC home page.
2. User sees the prominent PNR Status and Charts / Vacancy actions.
3. User clicks one of those shortcuts expecting a quick in-app lookup.
4. The flow jumps to another site or a different window/tab.
5. The user loses the sense of being in a single booking journey.
6. Navigation, state, and context no longer feel unified.
7. The user must mentally re-orient to a different interface.
8. The simple status check becomes a fragmented cross-site detour.

**Where exactly it breaks:**
Step 4. The handoff to a different site breaks continuity right where the user expects a quick, integrated utility action.

**Screenshot or description:**
Live homepage screenshot showed the two top shortcuts directly above the booking form, making the cross-site jump visible from the first screen.

---

## Problem 6: Primary Booking Action Is Buried Under Clutter [Self-Discovered]

**How I found it:**
I reviewed the live home page visually before and during form entry. The main booking form is surrounded by promotional blocks, service tiles, ad surfaces, and secondary travel products that compete with the primary task.

**What is broken:**
The page hierarchy does not clearly privilege booking. Instead, the main task is embedded in a dense mix of promotions and side services, which makes the home screen feel noisy and reduces confidence that the booking flow is the center of the product.

**Affected users:**
New users, low-frequency travellers, older users, and mobile users who have less space to scan the page. It also affects experienced users when they are trying to act quickly.

**Frequency:**
Constant. Every visitor sees the same crowded first impression, so the problem is present on nearly every session.

**Current flow — step by step:**
1. User opens the IRCTC home page.
2. The page loads with multiple competing modules above and around the booking form.
3. The user looks for the primary ticket-booking action.
4. Several secondary products and promotional sections compete for attention.
5. The booking form is visible, but not visually dominant.
6. The user has to work harder to understand where to begin.
7. Important support options such as concessions are also not surfaced as a clean guided path.
8. The experience feels like a portal of unrelated services instead of a booking-first interface.

**Where exactly it breaks:**
Step 2 to step 6. The information architecture and layout dilute the primary booking task before the user even starts the flow.

**Screenshot or description:**
The live homepage showed the booking panel on the left with large secondary marketing content and service tiles occupying the surrounding space.

---

## Notes for Part B
- Problems 1 to 3 are the given issues and should remain fixed in scope.
- Problems 4 to 6 are self-discovered from live platform exploration and are distinct from the given issues.
- Each problem above is framed as a product and workflow failure, not a visual rebrand critique.
