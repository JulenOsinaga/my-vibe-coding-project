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

## Data & events

_What gets stored, what gets tracked._

The ten tracked signals and their firing rules (once each, only on real consumption), session state that persists them, the booking record fields captured at checkout, the derived metrics (trust signals used, minutes to first booking) and the ship / iterate / kill thresholds.

## Open questions

Real ones the prototype cannot answer: which verification checks buyers actually weight, whether a pending check helps or hurts, whether samples substitute for reviews or delay bookings, what happens at the first real review, provider-side effort and verification cost, the sample-cost policy, and what sample size and duration a real test needs.
