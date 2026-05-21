# PRD: Admin Portal

## 1. Feature Name
Admin Portal

## 2. Feature Summary
An internal management interface for CoParent Logic staff (Admins) to oversee the platform — reviewing and approving professional credentials, managing the professional whitelist, monitoring subscription and usage analytics, accessing audit logs, and investigating support issues. The Admin Portal is the operational control center of the platform.

## 3. Product Context
The Admin Portal is the back-office counterpart to the professional-facing product. While professionals and parents interact with the client-facing platform, Admins use this portal to maintain platform integrity, trust, and operational health.

- **User journey position:** Admin-only; not part of the professional or parent user journey. Admins access this as a separate, protected area of the platform.
- **Product goals supported:** Platform trust and safety (credential verification), revenue visibility (subscription tracking), compliance (audit logs), and operational excellence.
- **Interacts with:** Professional Onboarding & Verification (credential review queue is the Admin's primary workflow), User Authentication & Role Management (Admin can suspend/reactivate accounts), Subscription & Billing (Admin can view subscription status), Case Management (Admin read-only access to cases), all features (audit logging flows here).
- **Shared constraints:** Admin access is restricted to internal CoParent Logic staff. Admin actions are themselves logged. All decisions are traceable.

## 4. Problem Statement
As the platform grows, CoParent Logic staff need a centralized, efficient way to: review professional credentials before granting access, monitor who is using the platform and how, respond to support issues, and maintain a trustworthy professional community. Without a dedicated Admin Portal, these operations would require direct database access, creating risk and inefficiency.

## 5. Goals

**User goals (Admin):**
- Admins can review and act on credential submissions quickly.
- Admins have visibility into platform health metrics (users, subscriptions, case volume).
- Admins can investigate and resolve issues for any user account.

**Business goals:**
- Ensure credential review does not become a bottleneck to professional activation.
- Maintain a trusted, verified professional community.
- Monitor key business metrics (activation rate, subscription conversion, churn).

**Operational goals:**
- Full audit trail of all Admin actions.
- Reduce reliance on engineering team for routine operational tasks.

## 6. Users / Personas

**CoParent Logic Admin**
- Why they use this feature: To manage day-to-day platform operations — credential review, user management, monitoring.
- What they need: A clear queue of pending actions, user search and management tools, analytics dashboards, and access to audit logs.
- Role-specific behavior: Elevated permissions across the platform. All Admin actions are logged. Admin cannot access parent communication content directly (only metadata and analytics, per ZDR policy).

## 7. Feature Scope

**In scope:**
- Credential review queue: view, approve, reject professional submissions
- Professional whitelist management: view, search, filter, suspend, reactivate professional accounts
- AI-assisted credential validation (Phase 2 — Bar ID lookup against state registries; Phase 1 is manual review)
- User account management: view account details, change account status (active, suspended), view linked cases (read-only)
- Subscription status view per professional (via Stripe data)
- Usage analytics dashboard: total professionals, active cases, case volume per seat, analysis runs
- Audit log viewer: all platform events with filter by user, event type, date range
- In-platform notifications for new credential submissions awaiting review

**Out of scope:**
- Admin ability to create or modify cases (read-only access)
- Admin access to full message or plan content (metadata and analytics only, subject to ZDR)
- Financial reporting beyond subscription status (Stripe dashboard handles this)
- Customer support ticketing integration (not in source materials for MVP)
- Multi-admin role hierarchy (all Admins have equivalent permissions in Phase 1)

## 8. Functional Requirements

1. Admin logs in via the standard login flow with their Admin-role credentials.
2. Admin is redirected to the Admin Portal dashboard, which shows:
   - Count of pending credential submissions
   - Platform health metrics: total active professionals, total active cases, recent analysis runs (last 7/30 days)
   - Recent activity feed (latest audit log entries)
3. **Credential Review Queue:**
   a. Admin can view a list of all pending credential submissions, sorted by submission date (oldest first).
   b. Each queue item shows: professional name, email, role type, jurisdiction, submission date, number of prior submissions (to indicate resubmissions).
   c. Admin can open a submission to view: professional profile, submitted documents (viewable inline), prior submission history and rejection reasons (for resubmissions).
   d. Admin can Approve a submission → account state changes to "pending subscription," professional receives approval email.
   e. Admin can Reject a submission → must enter a rejection reason → account state changes to "rejected," professional receives rejection email with reason.
   f. Approved and rejected submissions move to a "reviewed" history list, accessible for audit.
4. **Professional Whitelist:**
   a. Admin can search and filter professionals by: name, email, role type, jurisdiction, account status (pending, active, suspended).
   b. Admin can view a professional's account detail: profile info, credential submission history, subscription status, case count, last active date.
   c. Admin can suspend a professional account → access immediately revoked, professional receives notification.
   d. Admin can reactivate a suspended account → access restored, professional receives notification.
5. **Subscription Overview:**
   a. Admin can view subscription status per professional (active, trialing, lapsed, cancelled).
   b. Subscription data is sourced from Stripe via webhook-synced records.
6. **Usage Analytics:**
   a. Admin can view aggregate platform metrics: total registered professionals, total active subscriptions, total active cases, total analysis runs (PlanGuard and communication analysis separately), average case volume per professional.
   b. Metrics should support date range filtering (last 7 days, last 30 days, last 90 days, custom range).
7. **Audit Log:**
   a. Admin can view a searchable, filterable audit log of all platform events.
   b. Filterable by: user (professional or parent), event type (login, case created, analysis run, credential decision, account status change, etc.), date range.
   c. Each log entry shows: timestamp, user ID, user name/email, event type, event details, IP address (for auth events).
   d. Audit log is read-only. Entries cannot be modified or deleted.
8. All Admin actions (credential decisions, account status changes) are themselves written to the audit log with Admin user ID, timestamp, and action details.

## 9. Workflow / User Journey

**Daily credential review:**
1. Admin logs in to the Admin Portal.
2. Dashboard shows N pending credential submissions.
3. Admin navigates to Credential Review Queue.
4. Opens oldest pending submission.
5. Reviews documents and profile.
6. Approves or rejects (with reason if rejecting).
7. Proceeds to next pending submission.
8. After clearing the queue, checks the analytics dashboard and audit log for any anomalies.

**Account investigation:**
1. Admin receives a support inquiry about a professional account.
2. Searches for the professional by email in the whitelist.
3. Opens the account detail page.
4. Reviews credential history, subscription status, case count, and recent audit log entries for that user.
5. Takes action (suspend, reactivate, or escalates to engineering if technical issue).

**Analytics review:**
1. Admin navigates to Usage Analytics.
2. Sets date range to last 30 days.
3. Reviews: new professional signups, activation rate, case volume, analysis run counts.
4. Identifies any unusual trends and investigates via the audit log or user search.

## 10. Business Rules

- Only users with the Admin role can access the Admin Portal.
- Admin credential decisions (approve/reject) are final but can be overridden by another Admin action (e.g., approving after a rejection, or suspending an approved account).
- Rejection reasons are mandatory — blank rejections are not permitted.
- Admin account suspension takes effect immediately — the professional loses access as soon as the action is confirmed.
- Admin actions are logged and attributable — shared credentials or anonymous Admin actions are not permitted.
- Audit log entries are immutable — they cannot be edited, deleted, or hidden.
- Admin cannot view the content of co-parent messages (message text) — only metadata (counts, timestamps, behavioral pattern labels) is accessible in analytics.

## 11. Dependencies

- **User Authentication & Role Management:** Admin role must be assigned for portal access. Account state changes (suspend, reactivate) are executed via the auth system.
- **Professional Onboarding & Verification:** Credential submission records are the Admin's primary workflow input.
- **Subscription & Billing:** Subscription status displayed in Admin Portal is synced from Stripe via webhook events.
- **Case Management:** Admin read-only case view is a dependency on the case data model.
- **Convex:** All data displayed in the Admin Portal is stored in and served from Convex.
- **Mailgun:** Approval, rejection, and status-change notifications to professionals are sent via Mailgun.

## 12. Data / Inputs / Outputs

**Inputs:**
- Admin decisions: approve/reject (with reason), suspend, reactivate
- Search/filter parameters (user lookup, audit log filters)
- Date range selections (analytics)

**Data accessed (read):**
- Professional accounts, credential submissions, subscription status, case records (metadata), audit log

**Outputs:**
- Account state changes (approval, rejection, suspension, reactivation)
- Notification emails to professionals (via Mailgun)
- Audit log entries for all Admin actions
- Analytics export (if applicable — not defined in source materials)

## 13. UX / Design Notes

- The Admin Portal should be functional and information-dense rather than consumer-friendly — this is an internal tool.
- The credential review queue is the highest-priority workflow — it should be immediately visible on the Admin dashboard, ideally with a count badge or alert if submissions are pending.
- Inline document preview is important — Admins must not need to download every document to review it. PDF and image inline rendering is required.
- Analytics dashboard should prioritize the key success metrics defined in the discovery report: professional activation rate, case volume per seat, analysis run counts.
- The audit log viewer should have efficient filtering and search — it may grow to tens of thousands of entries and must remain usable.
- Screen Mapping PNG likely contains Admin Portal screens; design team should be consulted for layout specs.

## 14. Edge Cases and Exceptions

- No pending credential submissions: Admin dashboard shows an empty queue with a "No pending submissions" state.
- Admin accidentally approves the wrong submission: No undo. Admin must manually suspend the account and handle via support.
- Admin attempts to approve a submission that another Admin has already reviewed: System should prevent double-processing (race condition handling — assignment model or optimistic locking required).
- Stripe subscription data is delayed or missing for a professional: Admin view should show "status unknown" or "sync pending" rather than displaying incorrect data.
- Audit log grows very large: Pagination, filtering, and search must be implemented before the log becomes unmanageable.

## 15. Non-Functional Considerations

- **Security:** Admin Portal must be accessible only to Admin-role users. Admin actions have direct, immediate effects on user access — access control must be strict.
- **Auditability:** Ironically, the Admin Portal is itself subject to audit — all Admin actions are logged. This creates a recursive audit requirement that must be designed for.
- **Performance:** Credential document preview must load quickly. Analytics queries must be efficient even as the dataset grows.
- **Reliability:** Admin credential decisions directly affect professional activation. The portal must be reliable; credential review delays directly impact conversion rates.

## 16. Open Questions / Assumptions

- **Admin notification channel:** Whether Admins are notified of new credential submissions via email, in-platform notification, or both is not defined. Assumption: in-platform notification with an optional email alert.
- **Multi-admin race condition:** No specific handling described for two Admins reviewing the same submission simultaneously. Technical design required.
- **AI-assisted credential validation:** Phase 2 capability. Phase 1 is fully manual. The data model should be designed to support Bar ID lookup integration in the future without a major schema change.
- **Admin analytics export:** Whether analytics data can be exported (CSV, etc.) is not defined in source materials.
- **Admin role hierarchy:** All Admins appear to have equivalent permissions. If a super-admin / limited-admin distinction is needed, this must be defined before implementation.
- **Message content access:** The assumption that Admins cannot view message content is based on ZDR policy context but is not explicitly confirmed in source materials.

## 17. Source Summary

- **Product Discovery Report.docx:** Described Admin Portal responsibilities — professional whitelist management, AI-assisted credential validation (Phase 2), usage analytics, subscription tracking, audit logging. Listed as an MVP epic.
- **Meeting Transcript (April 9, 2026):** No direct Admin Portal discussion; confirmed that credential verification is required before professional access and that Admin oversight of the professional community is part of the Phase 1 model.
- **Screen Mapping.png:** May contain Admin Portal screens; not fully analyzed.
- **Confidence:** High for credential review workflow and whitelist management. Medium for analytics specifics and audit log requirements. Low for multi-admin race condition handling, notification design, and AI-assisted validation details.
