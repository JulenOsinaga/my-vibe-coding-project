# PROMPTS.md: Living Prompt Pack

> Module 3 · Prompt Chaining. Re-architect the build with prompt chains; capture the reusable ones here.

## How to use this pack

_Each prompt is a reusable step. Chain them: the output of one becomes the input to the next._

## Prompt chain: Provider Trust Flow Completion

### Step 1: Expand, build new screens in a strict sequence
```
Extend the existing Marketplace Trust Problem prototype by adding the missing screens and views required to complete the buyer trust-to-booking journey.

Do not redesign the existing prototype. Preserve its current dark marketplace aesthetic, near-black background, lifted card surfaces, hairline borders, cyan accent, rounded components, geometric typography, spacing system, and interaction patterns.

Build the following additions in this exact order:

1. Expanded Verification Details

Create an Expanded Verification Details view that opens from the existing “8 of 9 checks passed” control in the trust panel on:

/studio/kiln-and-cove

Use the existing Studio profile as the primary visual anchor. Match its:

- Card construction
- Border radius
- Surface elevation
- Typography
- Icon treatment
- Cyan accent usage
- Spacing and content density

The new view must explain the provider's verification status in more detail rather than simply showing a numeric count.

Include:

- Provider identity/header context for Kiln & Cove
- Overall verification summary: 8 of 9 checks verified
- Individual verification checks
- A visible status for each check:
+ VERIFIED
+ IN REVIEW
- A concise explanation of what each check means or what was validated
- Clear distinction between completed and pending verification
- A close/back interaction returning the user to the same position on the Studio profile

Prefer a drawer, expandable panel, or focused detail view that feels like a natural extension of the current trust panel rather than a completely separate application screen.

This view establishes the trust-detail component patterns used by the remaining additions.

2. Sample Request Flow

Create a focused Request a Sample flow launched from the existing “Request a sample” action on the Studio profile.

Visually inherit from:

- The existing Message studio drawer for form structure, side-panel treatment, controls, attachment patterns, and CTA styling
- The Studio profile for provider context and trust cues

Include:

- Studio mini-card for Kiln & Cove
- Selected product/sample context where relevant
- Sample quantity
- Delivery information
- Optional note or request details
- Expected dispatch or response timing
- Any available cost/free-sample information
- Primary Request sample CTA
- Secondary cancel/back action

Do not turn this into checkout. It is a pre-booking trust and product-evaluation interaction.

Navigation:

- Studio profile → Request a sample → Sample request form
- Closing/cancelling returns to the profile without losing the current page context
- Successful submission will later be handled in STEP 2

Reuse the existing drawer and form components wherever possible rather than introducing a new form system.

3. Zero-Review Reassurance Detail

Extend the existing reviews empty state on the Studio profile with a focused Why there are no reviews yet / Alternative trust evidence view.

This view must make it clear that:

- Kiln & Cove is new to the marketplace
- The lack of marketplace reviews is not equivalent to failed verification
- Other evidence is available to help the buyer assess the provider

Use the Studio profile's existing Reviews empty state, Why buyers choose this studio section, and trust panel as visual anchors.

Include alternative trust evidence already consistent with the prototype, such as:

- Years in business
- Verified provider checks
- Sustainable materials
- Product/process evidence
- Response-time information
- Other early social-proof signals already represented by the prototype

Do not invent customer reviews, review scores, testimonials, or marketplace history that does not exist.

Navigation:

- The reviews empty state should expose a clear Learn why / View trust evidence action
- Opening it reveals the additional reassurance content
- The user can return directly to the same reviews section

This should feel like an extension of the existing zero-review state, not a new top-level feature.

4. Post-Booking Confirmation

Create a dedicated Booking Request Confirmed screen reached after the existing checkout flow succeeds.

Visually inherit from:

/checkout

Match its:

- Stepper and content width
- Order summary
- Provider verification card
- Buyer-protection presentation
- Button hierarchy
- Card and spacing patterns

Include:

- Clear success confirmation
- Kiln & Cove provider context
- Product/order request summary
- Quantity
- Submitted delivery information
- Expected next step
- Expected provider response time
- Confirmation that the request was submitted successfully
- Message studio action
- View provider profile action
- View experiment readout action

Navigation:

Studio profile → Product evidence → Checkout → Booking Request Confirmed → Experiment readout

The experiment readout must remain accessible exactly as it is today.

Flow Integration

After the four additions are created, connect them to the existing journey:

- Studio profile
+ “8 of 9 checks passed” → Expanded Verification Details
+ “Request a sample” → Sample Request Flow
+ Reviews empty state → Zero-Review Reassurance Detail
- Product evidence
+ Preserve all existing behavior
+ Continue to checkout through the current purchase path
- Checkout
+ Successful confirmation → Booking Request Confirmed
- Booking Request Confirmed
+ View experiment readout → /experiment

Do not modify search, authentication, real payments, provider dashboards, or any other functionality explicitly outside the prototype scope.

Build these additions in the specified order so the Expanded Verification Details view establishes the trust-information hierarchy and component language reused by the sample, reassurance, and confirmation experiences.

Treat the existing Studio profile as the principal visual anchor for trust-related additions and the existing Checkout screen as the principal visual anchor for the final conversion state.
```

### Step 2: Behavior, hard-code the states
```
The prototype has now been structurally expanded. Preserve all screens, routes, components, navigation, and visual hierarchy created in STEP 1.

Add realistic interaction logic and resilience behavior to the existing and newly created buyer flow. Focus specifically on loading, empty, error, retry, disabled, success, and conditional states.

Do not redesign the screens.

1. Trust and Product Evidence Loading

For the Studio profile and product-evidence surfaces, add loading states for:

- Hero imagery
- Trust panel
- Verification details
- Product gallery
- Product evidence sections

While these areas are loading:

- Preserve the final component dimensions to avoid layout shift
- Show skeleton placeholders that match the shape and density of the real content
- Keep page navigation and already-loaded actions usable where possible
- Do not replace the whole page with a spinner

For verification content, use a compact card-based skeleton matching the final verification-check layout.

If only one content area is loading, skeletonize that area only.

2. Zero-Review Empty State

On the Studio profile, preserve the intentional zero-review state.

Display the primary message:

“No marketplace reviews yet”

Supporting copy:

“Kiln & Cove is new to the marketplace. You can still evaluate the studio through verified checks, product evidence, business information, and other trust signals.”

Provide a clear action:

“View trust evidence”

Selecting it opens the Zero-Review Reassurance Detail created in STEP 1.

Do not display:

- Placeholder reviews
- Artificial ratings
- Fake testimonials
- A numerical review score when no reviews exist

If reviews later become available as part of a test state, replace this empty state with the normal reviews presentation.

3. Verification Conditional States

In Expanded Verification Details:

- VERIFIED means the check is completed.
- IN REVIEW means the check has not yet been completed and must not be interpreted as verified.

If a check is IN REVIEW:

- Keep it visible
- Disable any interaction that implies successful verification
- Show the status label “IN REVIEW”
- Use supporting text where needed:

“This check is still being reviewed.”

The overall summary must be calculated from completed checks only.

Example:

“8 of 9 checks verified”

Do not count IN REVIEW checks as verified.

4. Sample Request Validation and Submission

In the Request a Sample flow:

Required fields must be completed before submission.

At minimum, validate:

- Sample quantity
- Delivery information
- Any other field already represented as required in the form

Until all required fields are valid:

- Disable the Request sample CTA
- Preserve entered values
- Show validation feedback near the affected field

On submission:

- Disable repeated submission
- Show an in-button or localized loading state
- Do not clear the form until the request succeeds

On success, show:

“Sample request sent”

Supporting message:

“Kiln & Cove has received your request. The studio will respond within the expected response time shown on their profile.”

Provide:

- Done
- Message studio

“Done” returns to the Studio profile.

If submission fails, show:

“Your sample request could not be sent.”

Supporting message:

“Your information has been preserved. Try again to send the request.”

Provide:

“Try again”

Retry must reuse the existing entered information.

A failed submission must not be recorded as a successful trust/conversion event.

5. Message Studio Failure and Success

Preserve the existing Message studio drawer.

When sending:

- Disable the Send button
- Preserve the message contents
- Show localized sending feedback

On success:

“Message sent”

Supporting message:

“Kiln & Cove has received your message.”

On failure:

“Your message could not be sent.”

Supporting message:

“Your message has been preserved. Try again when you're ready.”

Provide:

“Try again”

Never clear user-entered content on failure.

6. Checkout Submission Behavior

Preserve the existing checkout structure and numbered steps.

While confirming the booking/request:

- Disable the confirmation CTA
- Prevent duplicate submission
- Keep the order summary visible
- Show localized progress feedback

On success:

- Record the booking event exactly once
- Navigate to Booking Request Confirmed
- Preserve the submitted order information for the confirmation screen

On failure, remain on checkout and show:

“Your booking request could not be confirmed.”

Supporting message:

“No booking has been created. Your order details have been preserved so you can try again.”

Provide:

“Try again”

The interface must never leave the user uncertain about whether the booking succeeded.

A failed checkout submission must not:

- Increment the booking metric
- Trigger the success state
- Be treated as a confirmed booking by the experiment readout

7. Booking Confirmation Behavior

On the Booking Request Confirmed screen:

- Display the submitted order data
- Keep the success state persistent during the current session
- Prevent refreshing or revisiting the screen from generating another booking event

Actions:

- Message studio → open the existing Message studio drawer
- View provider profile → /studio/kiln-and-cove
- View experiment readout → /experiment

8. Experiment Tracking Integrity

Maintain the existing session-based experiment tracking.

Record meaningful trust interactions only when the user actually consumes them.

Examples:

- Opening Verification Details → trust interaction
- Expanding a meaningful evidence section → trust interaction
- Viewing Zero-Review Reassurance Detail → trust interaction
- Successfully submitting a sample request → interaction event
- Successfully confirming checkout → booking event

Do not count:

- Skeleton states
- Failed submissions
- Disabled action clicks
- Retry attempts by themselves
- Merely rendering a component

The existing Ship / Iterate / Kill logic must remain intact unless required to correctly distinguish successful from failed interactions.

Maintain all visual structures established in STEP 1. Make the flow resilient and realistically testable without introducing unrelated functionality or visual redesign.
```

### Step 3: Refine, one surgical polish
```
The expanded screens and resilient interaction states are now implemented.

Perform a surgical visual refinement of the provider verification summary and its immediate trust-panel context only.

Do not redesign the rest of the prototype.

Target element

Refine the existing control that currently communicates:

“8 of 9 checks passed”

and the Expanded Verification Details experience attached to it.

Use the existing Studio profile trust panel as the primary visual reference. Compare the refined component against the surrounding trust cards, typography, spacing, card treatment, icon language, and CTA hierarchy already established on that screen.

1. Compare and diagnose

Before changing the UI, identify the three most significant visual discrepancies in the current verification summary/detail implementation.

Focus specifically on:

- Hierarchy
+ Is the verification total immediately understandable?
+ Is VERIFIED visually stronger than IN REVIEW?
+ Can the pending state be mistaken for completed verification?
- Typography and density
+ Does the score/status summary use the same typographic scale and weight logic as the surrounding trust panel?
+ Is supporting text too prominent or too dense?
+ Can the user scan the verification state without reading every line?
- Spacing and component styling
+ Does the summary align with the existing card grid, padding, radii, borders, icons, and cyan-accent usage?
+ Does the expanded detail feel like a natural continuation of the Studio profile rather than an inserted component from another design system?

Use these three findings as the basis for the refinement.

2. Correct the discrepancies

Refine the compact trust summary so it communicates the state at a glance.

Prefer a structure equivalent to:

8 of 9 checks verified

8 VERIFIED · 1 IN REVIEW

View verification details →

Apply the following hierarchy:

- Make “8 of 9 checks verified” the primary message
- Make the verified count visually confident and aligned with the prototype's trust accent treatment
- Keep IN REVIEW visible but intentionally less prominent
- Use status labels and/or icons so the difference does not depend solely on color
- Keep the “View verification details” action visually secondary to the trust result itself

Do not make IN REVIEW look disabled or broken; it should read as a legitimate pending status.

3. Refine the expanded verification details

Within the Expanded Verification Details view:

- Ensure VERIFIED rows are easy to scan
- Reduce the visual weight of IN REVIEW rows
- Keep labels, icons, spacing, and descriptions aligned consistently
- Preserve enough separation between individual checks to maintain readability
- Avoid adding decorative elements that do not communicate verification status
- Keep the summary and detailed checks visually connected

The user should be able to answer the following within a glance:

- How many checks are complete?
- Is anything still pending?
- Which specific check is pending?
- Where can I learn what each verification means?

4. Preserve everything else

Do not modify:

- Routes
- Navigation
- Checkout logic
- Experiment tracking
- Loading states
- Empty states
- Error and retry behavior
- Sample-request logic
- Message logic
- Booking confirmation behavior
- Product evidence layout
- Overall Studio profile layout
- Global typography
- Global color palette
- Unrelated cards or components

The goal is not to redesign the trust panel. The goal is to make the verification summary and VERIFIED vs. IN REVIEW hierarchy more legible, credible, and professionally integrated into the existing interface while preserving all functionality introduced in STEP 1 and STEP 2.
```

## Reusable techniques learned

- Using the README.md file when refining the prompt increases and makes easier to define the context.
- Dividing the problem on specific areas (Expand, behavior, refine) increases the guarantee better results and makes easier to define the problem.
- In my case, using an app for prompting and another for prototyping improves the credits usage.

## What broke (and the fix)

_Where a single mega-prompt failed and chaining fixed it._

The plan process with the 3 prompts and the final prompt execution went smoothly. 
