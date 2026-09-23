# Full-Stack: Data, Access Rules, Edge Cases, Deploy

> Module 5 · Full-Stack. Add data schemas, access rules, and edge cases; stress-test and deploy.

## Deployed link

_The working, shareable link that survives real users._

https://trust-builder-prototype.lovable.app

## Data schema

| Entity | Key fields | Notes |
|---|---|---|
| Studio trust & contact content | `studio_id`, `trust_copy`, `next_steps`, `contact_settings`, `message_intent`, `ordering_index` | Store trust copy, next steps, contact configuration, and message-intent content in the database. Seed with the exact copy currently used in the prototype. |
| Product verification & presentation | `product_id`, `featured`, `position`, `verification_checked_at` | Add product featured/position flags and the date/time when verification was checked. Include the required grants and regenerate application types as part of the migration. |

## Access rules

_Who can see / do what? Where are the auth boundaries?_

Public studio/product pages remain readable while signed out. Resolve studio and product from the page address rather than fixed IDs. All write operations require authentication and must verify that the signed-in user is the owner of the affected resource. Signed-in journeys must preserve and verify authentication end to end. Page content must be loaded through proper database queries rather than duplicated/static data-file content.

## Edge cases hardened

| Case | Before | After |
|---|---|---|
| Empty / first-run state | Missing or incomplete data may result in undefined/blank content or depend on hard-coded prototype data. | Queries expose explicit **loading, empty, and error states**. Public pages remain readable when signed out, and database content is the source of truth. |
| Bad / malicious input | Writes or submissions may rely on client state, fixed IDs, or insufficient ownership/session validation. | Every write is **signed-in and owner-checked**. Studio/product identity comes from the page address. Expired sessions send the buyer to sign in and back. Repeat/concurrent submissions cannot create duplicate bookings or booking events. |
| Failure / offline | Failed submissions may lose form state, incorrectly appear successful, or leave inconsistent booking/event records. | Forms remain intact on failure, **no success event is recorded**, and booking confirmation reads from the database. Exactly one booking and one booking event are maintained per session. Fake delay/failure controls are isolated in a clearly marked testing panel. |

## Stress test results

_What you threw at it, and what held / broke._

I tried to submit same request multiple times, but is not possible. Buttons disables on first click.
