# Engineering Handoff Note

> Module 4 · Production Specs. Open the black box, make the build legible to an engineer.

## What this is

_One paragraph an engineer can read in 60 seconds._

This is a frontend-only TanStack Start prototype testing whether stronger product evidence, contact options, verification, a trust panel, and honest zero-review reassurance can increase first bookings for new providers. The buyer journey is experiment brief → Kiln & Cove profile → optional verification/sample/reassurance/message interactions → product evidence → checkout → persistent booking confirmation → experiment readout. Routes are thin definitions; implementation is grouped by PRD feature under src/features, shared presentation and simulated-request utilities live under src/shared, and all content is seeded locally. There is no backend, authentication, payment processing, provider dashboard, real messaging, sample fulfillment, or analytics pipeline: successful actions and one booking are stored only in browser sessionStorage. The interaction model is deliberate and tested, but the content, latency, failures, and outcome measurement are prototype simulations rather than production services.

## Architecture (plain language)

- **Frontend:** TanStack Start and React render six routed experiences plus four profile-level detail screens. Route files under src/routes own URLs, page metadata, search validation, and which screen to render. Screen components are named from the Living PRD and grouped by feature:      experiment-brief: MarketplaceTrustProblemScreen     provider-profile: StudioProfileScreen, ZeroReviewReassuranceScreen     verification: ExpandedVerificationDetailsScreen, VerificationSummary     contact: MessageStudioScreen, SampleRequestScreen     product-evidence: ProductEvidenceScreen     booking: CheckoutScreen, BookingConfirmationScreen     experiment-readout: ExperimentReadoutScreen  Shared header, breadcrumbs, provider summary, side panel, status banner, and loading skeletons live under src/shared/components. Styling remains the existing Tailwind-based dark marketplace system; the refactor does not change visual behavior.
- **Backend / data:** There is no backend. Feature data modules contain the fixed Kiln & Cove profile, product, verification checks, sample/message defaults, booking copy, and experiment brief. Browser sessionStorage supplies session-only state:      trust-experiment-session-v1: deduplicated trust events and experiment start time.     trust-booking-v1: the successful booking record used by confirmation and reload.     trust-force-fail-v1: the “force next request to fail” test switch.     trust-attempts-v1: deterministic per-action attempt counts.  Simulated message, sample, and checkout requests wait 800–1400 ms. A request fails when forced or on every third attempt for that action. Nothing leaves the browser.
- **Key flows:** The studio profile records profile_viewed after the real screen mounts.
Opening verification, reassurance, product evidence, gallery, or contact surfaces records the corresponding event only when consumed.
Message and sample events are recorded only after a successful simulated submission; validation, disabled clicks, failures, and retries do not count as success.
Checkout prevents duplicate submission while confirming. A failed request preserves order details and creates no booking.
A successful checkout writes one booking record, records booking_confirmed once, and navigates to confirmation.
Confirmation reads the existing booking. Reloading it does not create another booking or event.
The readout derives Pending, Ship, Iterate, or Kill from the unchanged event list and timing rules.

## What's solid vs. what's duct tape

| Area | State | Notes |
|---|---|---|
| PRD-aligned screen names and feature ownership. | solid | _____ |
| PRD-aligned screen names and feature ownership. | solid | _____ |
| Thin routes with preserved URLs, metadata, and search validation. | solid | _____ |
| Data separated from display and browser persistence separated from screens. | solid | _____ |
| Typed event keys and stable, deduplicated tracking. | solid | _____ |
| VERIFIED and IN REVIEW semantics are data-derived; pending checks are never counted as verified. | solid | _____ |
| Loading, validation, disabled, error, retry, success, empty, and persistent confirmation states. | solid | _____ |
| Exactly-once booking behavior within a browser session. | solid | _____ |
| End-to-end browser verification across the complete buyer flow with no console or page errors. | solid | _____ |
| One hard-coded provider and one bookable product. | rough | _____ |
| One hard-coded provider and one bookable product. | solid | _____ |
| Seeded content and images; no content service or provider-managed data. | solid | _____ |
| `sessionStorage` instead of durable storage, user identity, or cross-device state. | solid | _____ |
| Random latency and deterministic every-third-attempt failures instead of real network behavior. | solid | _____ |
| “Message,” “sample,” “checkout,” and “payment” actions are simulations; no external party receives anything and no money moves. | solid | _____ |
| Experiment results describe one local session, not a controlled test or statistically valid marketplace result. | solid | _____ |
| The force-failure control is intentionally exposed on the readout for prototype testing. | solid | _____ |

## Risks & assumptions for the team

Storage keys are compatibility boundaries. Renaming them discards the current session and may break reload persistence.
The ten SignalKey values and their deduplication are compatibility boundaries for verdict calculation.
URLs are part of the tested journey and should remain stable unless navigation and tests change together.
The current data shape assumes a single provider/product; multi-provider support requires IDs in booking and event state plus route-driven data selection.
sessionStorage is isolated per tab and cleared when the browser session ends; it cannot support real experiment analysis.
The “exactly once” guarantee applies to this browser session, not server-side idempotency across devices or retries.
Simulated failures are useful for UI resilience testing but do not model real API error classes, timeouts, offline recovery, or partial success.
Baseline and target metrics are supplied research inputs. The prototype validates interaction behavior, not the causal hypothesis in production.
Copy claiming confirmation emails, escrow, verification operations, shipping, and provider response timing is illustrative unless backed by future services and operations.

## How to run it

```
Requirements: Bun and a current browser.

bun install
bun run dev

Open the local URL printed by Vite. The usual development address is http://localhost:8080 in the Lovable workspace.

Useful checks:

bunx tsgo --noEmit
bun run lint
bun run build

Recommended manual verification:

    Open /studio/kiln-and-cove and wait for the profile and trust skeletons to resolve.
    Open verification details and confirm 8 VERIFIED checks plus 1 IN REVIEW check.
    Open and close the sample request; verify invalid data disables submission and a failed request preserves the form.
    Open “View trust evidence,” then continue to the product evidence page.
    Expand one evidence section and continue to /checkout.
    Optionally enable “Force next request to fail” on /experiment, return to checkout, confirm failure preserves the order, then retry.
    Confirm the request and verify navigation to /booking-confirmed?qty=24.
    Reload confirmation and verify the booking reference remains unchanged.
    Open /experiment and verify one booking event, the consumed trust signals, and the derived verdict.
```
