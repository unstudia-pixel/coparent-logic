# PRD: Subscription & Billing

## 1. Feature Name
Subscription & Billing

## 2. Feature Summary
Manages the payment and subscription lifecycle for Family Law Professionals. Professionals must subscribe before gaining full access to the platform. Parents have free access in Phase 1. Billing is handled via Stripe. A two-tier subscription model is planned, with the professional tier active in Phase 1.

## 3. Product Context
Subscription & Billing sits at the end of the professional onboarding funnel — it is the final gate before a verified professional gains full platform access. It is also a key revenue driver and a signal of platform commitment.

- **User journey position:** After credential verification is approved, the professional is prompted to subscribe before accessing any AI tools or case management features.
- **Product goals supported:** Revenue generation, professional activation rate, conversion from trial to paid.
- **Interacts with:** User Authentication & Role Management (subscription status affects account state), Professional Onboarding & Verification (approval triggers subscription prompt), Case Management (only subscribed professionals can create cases), Admin Portal (Admin can view subscription status and usage per professional).
- **Shared constraints:** Subscription status is checked on every protected route. Lapsed or cancelled subscriptions immediately restrict access.

## 4. Problem Statement
The platform provides high-value AI analysis tools to legal professionals. These professionals need a clear, trustworthy payment experience that reflects the platform's professional positioning. The business needs a reliable, recurring revenue stream that scales with platform usage.

## 5. Goals

**User goals:**
- Professionals can subscribe quickly and without friction after completing verification.
- Professionals have visibility into their subscription status, billing cycle, and the ability to manage or cancel.

**Business goals:**
- Convert verified professionals to paying subscribers.
- Track key metrics: conversion rate from trial to paid, churn rate, case volume per seat.
- Support a future two-tier model (professional + parent tiers).

**Operational goals:**
- Stripe handles all payment processing, reducing PCI compliance scope.
- Subscription status changes (activation, cancellation, failed payment) automatically update platform access.

## 6. Users / Personas

**Family Law Professional (subscriber)**
- Why they use this feature: To activate their account and maintain access to the platform.
- What they need: A simple subscribe flow, clear pricing, and a billing management page.
- Role-specific behavior: Must be verified before subscribing. Subscription is per-seat (individual) in Phase 1.

**CoParent Logic Admin**
- Why they use this feature: To monitor subscription status and usage across professionals.
- What they need: View of subscription state per professional account; ability to identify lapsed accounts or support billing issues.
- Role-specific behavior: Does not directly manage subscriptions; Stripe is the source of truth. Admin can see status from the Admin Portal.

**Parent**
- Parents do not interact with this feature in Phase 1. Parent access is free and not gated by subscription.

## 7. Feature Scope

**In scope:**
- Professional subscription flow (post-credential-approval)
- Stripe-powered payment: credit/debit card
- Subscription plan display (pricing, features included)
- Free trial period (if applicable — referenced in source materials as a success metric, assumed to be supported)
- Active subscription confirmation and platform access unlock
- Billing management page: view current plan, billing cycle, payment method, cancel subscription
- Webhook handling for Stripe events: payment success, payment failure, subscription cancelled, subscription renewed
- Automated access revocation on subscription lapse or cancellation
- Invoice/receipt delivery via email (Stripe-native)

**Out of scope:**
- Parent-tier billing (Phase 2)
- Firm/multi-seat billing (Phase 2)
- Annual billing option (not confirmed in source materials)
- Promo codes or discounts (not mentioned)
- Refund processing via platform UI (handled via Stripe dashboard / support)
- Invoicing for enterprise contracts (not in scope)

## 8. Functional Requirements

1. After credential approval, the professional is redirected to a subscription page before accessing any platform features.
2. The subscription page displays the available plan(s) with pricing, billing cycle, and included features clearly described.
3. The professional enters payment details via a Stripe-hosted or Stripe Elements form.
4. On successful payment, the account moves to "active" state and the professional is redirected to the platform dashboard.
5. A confirmation email (Stripe receipt) is sent to the professional upon successful subscription.
6. The platform listens to Stripe webhooks to keep subscription state in sync:
   - `invoice.payment_succeeded` → maintain or activate subscription
   - `invoice.payment_failed` → flag account, notify professional, begin grace period
   - `customer.subscription.deleted` → revoke access
   - `customer.subscription.updated` → reflect plan changes
7. A professional whose payment fails receives an email notification and is given a grace period (duration TBD) to update their payment method before access is revoked.
8. A professional can access a billing management page from their account settings to:
   - View current subscription plan and status
   - View next billing date
   - Update payment method (via Stripe)
   - Cancel subscription
9. On cancellation, the professional retains access until the end of the current paid period, then access is revoked.
10. A professional with a lapsed/cancelled subscription who attempts to access protected features sees a clear message prompting them to resubscribe.
11. The Admin Portal displays subscription status (active, lapsed, cancelled, trial) for each professional account.
12. A free trial period, if offered, must track trial start date and expiry. On trial expiry without subscription, access is revoked and the professional is prompted to subscribe.

## 9. Workflow / User Journey

**Subscribe (post-verification):**
1. Professional receives approval email ("Your credentials have been verified. Subscribe to get started.").
2. Professional logs in and is redirected to the subscription page.
3. Reviews plan and pricing.
4. Enters payment details.
5. Clicks "Subscribe."
6. Payment is processed by Stripe.
7. On success: account activates, professional is redirected to the dashboard.
8. On failure: error message displayed, professional can retry or use a different card.

**Billing management:**
1. Professional navigates to Account Settings → Billing.
2. Views current plan, next billing date, and payment method.
3. Can update payment method (redirected to Stripe).
4. Can cancel subscription (confirmation dialog with reminder that access ends at period end).

**Payment failure / recovery:**
1. Stripe webhook fires for failed payment.
2. Professional receives email: "Payment failed. Update your payment method to maintain access."
3. Professional updates payment method within the grace period.
4. Next payment attempt succeeds → access maintained.
5. If grace period expires without resolution → access revoked, professional must resubscribe.

**Resubscribe after lapse:**
1. Professional logs in and sees "Your subscription has lapsed" screen.
2. Clicks "Resubscribe" and is taken to the subscription page.
3. Completes payment — account reactivates.

## 10. Business Rules

- A professional cannot access any AI features, case creation, or communication analysis without an active subscription (or active trial).
- Subscription is per individual professional account in Phase 1. Firm/team billing is Phase 2.
- On cancellation, access continues until the end of the paid period — no pro-rata refunds via the platform.
- A professional on a free trial is subject to the same access rules as a paid subscriber; trial expiry triggers the same access revocation flow.
- Stripe is the system of record for all billing events. The platform's subscription state is derived from Stripe webhook events.
- Parent accounts are never charged in Phase 1. Free access is granted upon invite acceptance.

## 11. Dependencies

- **Stripe:** All payment processing, subscription lifecycle management, webhooks, and invoice delivery.
- **User Authentication & Role Management:** Subscription status is a factor in the access control model. Account state must update based on subscription events.
- **Professional Onboarding & Verification:** Subscription prompt is only shown after credential approval.
- **Mailgun:** Payment failure notification emails (in addition to Stripe's native receipt emails).
- **Admin Portal:** Must display subscription status and allow Admin to view subscription history per professional.
- **Convex:** Stores local subscription state derived from Stripe webhook events.

## 12. Data / Inputs / Outputs

**Inputs:**
- Payment method details (collected via Stripe — not stored on platform servers)
- Plan selection (if multiple plans exist)

**Data stored:**
- Subscription record: user ID, Stripe customer ID, Stripe subscription ID, plan, status (trialing / active / past_due / cancelled), current period start/end, trial end date
- Webhook event log: event type, Stripe event ID, timestamp, processed status

**Outputs:**
- Account state change (active / lapsed / cancelled)
- Stripe receipt email
- Payment failure notification email
- Access grant/revocation

**Key states:**
- `pending_subscription` → `trialing` or `active`
- `active` → `past_due` (payment failure)
- `past_due` → `active` (payment recovered) or `cancelled` (grace period expired)
- `active` → `cancelled` (user cancels)

## 13. UX / Design Notes

- The subscription page should reinforce the professional positioning of the product — it is a sales moment. Pricing and feature list should be clearly presented.
- Payment form should use Stripe Elements or Stripe Checkout for security and trust signals (Stripe branding, SSL indicators).
- Billing management page should be clean and predictable — professionals expect standard SaaS billing UX.
- Cancellation should include a confirmation dialog with clear language about when access ends, to reduce accidental cancellations.
- Lapsed/cancelled access state should be communicated clearly without being punitive — professionals may return after a case pause.
- Screen Mapping PNG likely shows billing/subscription screens; specific UI layouts not analyzed in detail.

## 14. Edge Cases and Exceptions

- Professional submits payment but Stripe confirmation webhook is delayed — platform should handle eventual consistency gracefully (show "processing" state, not error).
- Professional tries to subscribe after their trial has already been used on a previous attempt — business rule around trial eligibility not defined; assumption: one trial per professional account.
- Professional cancels but then resubscribes within the same period — Stripe handles this; platform should reflect the new active state from webhook.
- Admin needs to grant temporary free access (e.g., for a demo or support case) — no mechanism defined in source materials; flagged as assumption/open question.
- Payment method update fails mid-cycle — professional remains in grace period; UI should reflect this.

## 15. Non-Functional Considerations

- **Security:** Payment data is never stored on the platform's servers. Stripe handles all PCI compliance. Platform stores only Stripe customer and subscription IDs.
- **Reliability:** Webhook processing must be idempotent — Stripe may send the same event more than once.
- **Auditability:** All subscription state changes must be logged with source (webhook event ID).
- **Performance:** Subscription status check must not add noticeable latency to page loads; status should be cached and refreshed via webhooks, not fetched from Stripe on every request.

## 16. Open Questions / Assumptions

- **Pricing:** Specific pricing is not defined in the source materials. Needs product/business input before implementation.
- **Free trial:** Referenced in success metrics ("Conversion from Trial to Paid") but trial duration and eligibility are not defined.
- **Multiple plans:** Source mentions a 2-tier subscription model (professional + parent tier), but the professional tier in Phase 1 may only have a single plan. Multiple plan options need clarification.
- **Grace period duration:** Not defined for failed payments. Assumption: 7 days (industry standard for SaaS).
- **Admin free access override:** No mechanism defined for granting complimentary access. Needs clarification for demo/support scenarios.
- **Annual vs monthly billing:** Not specified. Assumption: monthly only in Phase 1.

## 17. Source Summary

- **Product Discovery Report.docx:** Referenced Stripe as the payment provider. Mentioned a 2-tier subscription model. Listed "Conversion from Trial to Paid" as a key success metric.
- **Meeting Transcript (April 9, 2026):** Discussed that parent access is free in Phase 1, with billing to be revisited in Phase 2. Noted that Lucy (divorce coach) may incorporate app cost into her hourly client fees. Confirmed Phase 1 focus is on professional subscription.
- **Screen Mapping.png:** Likely includes subscription/billing screens; not fully analyzed.
- **Confidence:** High for overall model and Stripe integration. Low for specific pricing, trial duration, and plan structure (not defined in source materials).
