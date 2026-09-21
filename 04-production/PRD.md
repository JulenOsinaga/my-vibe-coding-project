# Living PRD

> Module 4 · Production Specs. Refactor for readability; extract a living PRD that stays true as the build evolves.

## Problem

_What user problem does this solve? Tie to the validated hypothesis._

Tied to the validated hypothesis: zero-review providers convert at 2.3% vs 14% for reviewed providers, median 19 days to first booking, −4% QoQ booking growth, with the buyer quote ("If there are no reviews, I assume something's wrong with them") and provider quote (the chicken-and-egg trap). States the hypothesis as tested in the prototype and the kill-switch rule: if trust evidence doesn't accelerate or increase first bookings, trust isn't the barrier — pivot.

## Users & jobs

- **Primary user:** The buyer evaluating an unreviewed studio.
- **Job to be done:** Decide whether a new provider is safe to book without a track record

## Scope

- **In:** Hypothesis brief, studio profile with trust panel, verification summary and expanded details, zero-review empty state plus reassurance detail, product evidence page, message studio, sample request, checkout, booking confirmation, experiment readout.
- **Out (explicitly):** Search, authentication, real payments, provider dashboards, reviews once they exist, notifications, real messaging delivery.

## Requirements

| # | Requirement | Priority | Acceptance criteria |
|---|---|---|---|
| 1 | Trust panel with verification summary on the provider profile | Must | Profile shows a summary card reading "8 of 9 checks verified" with an uppercase breakdown (✓ 8 VERIFIED · ◷ 1 IN REVIEW) and a secondary "View verification details →" link; counts are derived from check data, never hard-coded; VERIFIED is visually stronger than IN REVIEW, and the difference never depends on color alone (icon + label). (Mocked: check data is seeded content; Real: count derivation and hierarchy.) |
| 2 | Zero-review empty state | Must | Reviews section shows "No marketplace reviews yet" with copy explaining the studio is new; primary action "View trust evidence" opens the reassurance detail. No placeholder reviews, artificial ratings, scores or testimonials appear at any point. (Real: the state logic; Mocked: the underlying "no reviews exist" fact.) |
| 3 | Trust panel with verification summary on the provider profile | Must | Profile shows a summary card reading "8 of 9 checks verified" with an uppercase breakdown (✓ 8 VERIFIED · ◷ 1 IN REVIEW) and a secondary "View verification details →" link; counts are derived from check data, never hard-coded; VERIFIED is visually stronger than IN REVIEW, and the difference never depends on color alone (icon + label). *(Mocked: check data is seeded content; Real: count derivation and hierarchy.)* |
| 4 | Expanded verification details drawer | Must | Opens from the summary; verified checks render as confident inner-cards with what was validated; in-review checks are visibly pending ("This check is still being reviewed."), never read as verified or as broken; closing returns to the profile at the same position. *(Mocked: check statuses and validation descriptions.)* |
| 5 | Zero-review empty state | Must | Reviews section shows "No marketplace reviews yet" with copy explaining the studio is new; primary action "View trust evidence" opens the reassurance detail. No placeholder reviews, artificial ratings, scores or testimonials appear at any point. *(Real: the state logic; Mocked: the underlying "no reviews exist" fact.)* |
| 6 | Zero-review reassurance detail | Must | Explains lack of reviews ≠ failed verification and lists only evidence that exists in the prototype (years in business, verified checks, sustainable materials, documented process, response time, escrowed payments); actions open verification details or product evidence and return to the same reviews section. |
| 7 | Sample request flow | Must | Drawer with quantity (max 3), delivery info, optional note; CTA disabled until required fields are valid; inline validation feedback; entered values preserved across failure; success confirms the studio will respond within its stated response time; failure offers Try again reusing entered information. A failed submission is never recorded as a trust/conversion event. *(Mocked: submission, fulfillment, "free sample" cost rules.)* |
| 8 | Message studio | Must | Intent picker (question / sample / quote / custom) with drafts; Send disabled while sending; message content never cleared on failure; success and failure use the exact defined copy with a Try again path. *(Mocked: delivery — no message actually reaches anyone.)* |
| 9 | Product evidence page | Must | Full product detail with gallery, specs, and four evidence sections (materials & sourcing, production process, sustainability, shipping) presented as verifiable, specific claims — the "product evidence" arm of the hypothesis. *(Mocked: all product content.)* |
| 10 | Checkout submission behavior | Must | Numbered steps; confirmation CTA disabled while confirming; duplicate submission prevented; order summary stays visible; success records the booking **exactly once** and navigates to Booking Request Confirmed preserving submitted order data; failure stays on checkout with "Your booking request could not be confirmed. / No booking has been created. Your order details have been preserved so you can try again." and Try again. The user is never left uncertain whether the booking succeeded; a failed submission must not increment the booking metric. *(Mocked: payment and order creation — Real: the exactly-once event semantics.)* |
| 11 | Booking confirmation | Must | Displays the submitted order data from the persisted record; success state is persistent within the session; refreshing or revisiting never generates a second booking event; actions open message drawer, provider profile, and experiment readout. *(Mocked: everything after submission — no studio response, no escrow.)* |
| 12 | Loading / resilience states | Must | Skeleton placeholders match the shape and dimensions of final content (no layout shift, no full-page spinner) for hero, trust panel, verification details, gallery and evidence sections; page navigation and already-loaded actions stay usable while a section loads. *(Mocked: latency is simulated at 800–1400 ms.)* |
| 13 | Experiment tracking integrity | Must | Events are recorded only on real consumption: verification details opened, evidence section expanded, reassurance opened, successful sample request, successful checkout confirmation. Skeletons, failed submissions, disabled clicks and retries are never counted. Each signal is recorded once per session. *(Real: the tracking logic — Mocked: the analytics backend, which is sessionStorage only.)* |
| 14 | Experiment readout with kill switch | Must | Readout lists all 10 trust signals with timestamps, time-to-booking, and a verdict: Ship (booking + ≥6 signals + ≤15 min), Iterate (booking but weak signal coverage), Kill (≥4 signals consumed, no booking → trust isn't the barrier, pivot), Inconclusive otherwise. Includes session reset and a "Force next request to fail" testing control. *(Mocked: verdict thresholds stand in for the production metrics — 7%+ booking rate and <10 days.)* |

## Data & events

_What gets stored, what gets tracked._

The ten tracked signals and their firing rules (once each, only on real consumption), session state that persists them, the booking record fields captured at checkout, the derived metrics (trust signals used, minutes to first booking) and the ship / iterate / kill thresholds.

## Open questions

Real ones the prototype cannot answer: which verification checks buyers actually weight, whether a pending check helps or hurts, whether samples substitute for reviews or delay bookings, what happens at the first real review, provider-side effort and verification cost, the sample-cost policy, and what sample size and duration a real test needs.
