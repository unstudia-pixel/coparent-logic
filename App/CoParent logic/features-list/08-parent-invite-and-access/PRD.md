# PRD: Parent Invite & Access

## 1. Feature Name
Parent Invite & Access

## 2. Feature Summary
The mechanism by which a Family Law Professional invites co-parents to the platform for a specific case. Parents receive an email invitation, create an account, and gain access to a limited, parent-specific view of the platform — including their custody calendar, parenting plan, and messaging tools. In Phase 1, parent access is free and invite-only; parents cannot self-register. The parent experience is intentionally scoped and separate from the professional-facing tools.

## 3. Product Context
Parent Invite & Access is the bridge between the professional-facing platform and the parent-facing experience. It is how co-parents enter the product and how the professional's work (parenting plans, schedules) becomes actionable for the families they serve.

- **User journey position:** Initiated by the professional from within a case. Appears after case creation when the professional is ready to bring the co-parents onto the platform.
- **Product goals supported:** Platform adoption within families, enabling calendar and messaging features, and positioning CoParent Logic as a full co-parenting tool rather than a professional-only tool.
- **Interacts with:** Case Management (invites are tied to a specific case), User Authentication & Role Management (invite acceptance creates a parent account), Calendar & Schedule Management (parent gains access to their custody calendar upon joining), Parenting Plan Visualization (parent sees the finalized plan after joining), AI-Guided Messaging (parent gains access to the messaging coaching tool).
- **Shared constraints:** Parents cannot self-register in Phase 1. A parent's access is always linked to a specific professional's case. Parent accounts are free. Invite links are time-limited and single-use.

## 4. Problem Statement
In Phase 1, opening independent parent registration would create edge cases where parents engage with the platform outside of a professional relationship — leading to confusion, misuse, and potential liability. The invite model ensures every parent is connected to a verified professional and a specific case, maintaining the platform's controlled, professional context.

## 5. Goals

**User goals:**
- Professionals can invite co-parents to a case with minimal friction.
- Parents can accept an invitation and set up their account with a simple, non-intimidating experience.
- Both parents in a case have separate, independent accounts and views.

**Business goals:**
- Drive parent adoption within professional cases — active parent usage is evidence of product value.
- Control the platform entry point for parents to avoid unsupported use cases.
- Build toward a future independent parent tier (Phase 2) by establishing a positive parent onboarding experience.

**Operational goals:**
- Clear tracking of which parents have been invited, accepted, and are active per case.
- Invite status is visible to the professional from the case detail page.

## 6. Users / Personas

**Family Law Professional (inviter)**
- Why they use this feature: To bring co-parents onto the platform so they can interact with the parenting plan, calendar, and messaging tools.
- What they need: A simple invite action from the case detail page, with status visibility (invited, accepted, active).
- Role-specific behavior: Can invite up to 2 parents per case (one per co-parent role). Can resend an invite if a parent hasn't responded.

**Parent (invitee)**
- Why they use this feature: To create their account and access the tools the professional has set up for their case.
- What they need: A clear, simple email invitation, a minimal registration form, and immediate access to their case view after accepting.
- Role-specific behavior: Cannot self-register. Cannot access professional tools (PlanGuard, communication analysis). Access is limited to their own case data (calendar, plan, messaging).

## 7. Feature Scope

**In scope:**
- Professional initiates invite from the case detail page (enters parent name and email)
- System sends an invitation email with a unique, time-limited invite link
- Parent clicks the link and completes a simplified registration form (name, password)
- Parent account is created with the Parent role and linked to the case
- Parent is directed to the parent-facing dashboard after registration
- Professional can see invite status per parent on the case detail page: not invited / invited (pending) / accepted
- Professional can resend an invite to a parent who hasn't responded
- Professional can cancel a pending invite (before acceptance)
- Invite link expiry and single-use enforcement
- Parent can log in to their account after initial setup

**Out of scope:**
- Independent parent self-registration (Phase 2)
- Parent ability to invite other parents or professionals
- Parent billing (Phase 2)
- Parent ability to see or edit the professional's case notes, analyses, or reports
- Multiple professionals inviting the same parent to different cases (not defined for Phase 1; each invite is case-specific)

## 8. Functional Requirements

1. From a case detail page, a professional can initiate an invite for Parent 1 or Parent 2 (up to 2 parents per case).
2. The invite form collects: parent's first name, last name, and email address.
3. The system generates a unique, time-limited invite token and sends an invitation email to the parent.
4. The invite email contains:
   - A personalized greeting (using the parent's first name)
   - A brief explanation of what CoParent Logic is and why they are being invited
   - A clear call-to-action button / link to accept the invitation
   - The name of the professional who invited them
   - Invite link expiry information (duration TBD — assumption: 7 days)
5. The invite link is unique and single-use — it cannot be reused after acceptance.
6. Clicking the invite link directs the parent to a simplified registration page pre-filled with their name and email (from the invite).
7. The parent completes registration by setting a password (and confirming it). No credential upload. No payment.
8. On successful registration, the parent's account is created with the Parent role, linked to the inviting professional's case.
9. The parent is directed to the parent-facing dashboard (their case calendar, plan, and messaging tools).
10. The case detail page updates to show the parent's invite status as "accepted."
11. The professional can see invite status for both parent slots on the case detail page: not invited / invited (pending) / accepted / expired.
12. If the invite link expires before acceptance, the status shows "expired." The professional can resend a new invite.
13. The professional can resend an invite to a parent whose invite is pending or expired. Resending generates a new invite link and invalidates the previous one.
14. The professional can cancel a pending invite (before acceptance). The invite link is invalidated.
15. A parent who has already accepted an invite and created an account logs in via the standard login page — not via invite link.

## 9. Workflow / User Journey

**Professional invites a parent:**
1. Professional opens a case detail page.
2. Sees "Parent 1" and "Parent 2" slots — each shows invite status.
3. Clicks "Invite Parent 1."
4. Enters parent's name and email.
5. Clicks "Send Invite."
6. System sends invite email. Status updates to "Invited (pending)."

**Parent accepts the invite:**
1. Parent receives invitation email.
2. Clicks the "Accept Invitation" link.
3. Directed to the CoParent Logic registration page pre-filled with their name and email.
4. Sets a password and confirms.
5. Clicks "Create Account."
6. Account is created; parent is redirected to the parent dashboard (their case view).
7. Professional's case detail page updates to show the parent's status as "Active."

**Invite expires or is not accepted:**
1. Invite link expires after 7 days (assumption).
2. Status on case detail page changes to "Expired."
3. Professional can resend invite from the case detail page.
4. New invite email sent; old link invalidated.

**Failure paths:**
- Invite email bounces / is undeliverable: professional sees invite status as "pending" but the email was not delivered. No automated retry — professional must confirm the email address and resend.
- Parent tries to use an expired link: displayed a message saying the invitation has expired and to contact their professional for a new one.
- Parent tries to register with an email already used on the platform: display an error. If they already have an account from a prior case, they may need a different flow (not fully defined — flagged as open question).

## 10. Business Rules

- A case can have a maximum of 2 parent accounts (one per co-parent role).
- A parent can only be invited by a professional — no self-registration in Phase 1.
- Each invite link is unique and single-use. It expires after a defined period (assumption: 7 days).
- A parent's account is created with the Parent role and linked to the specific case — they cannot access other cases or the professional's tools.
- Parent access is free in Phase 1 — no payment step in the invite acceptance flow.
- Resending an invite invalidates all previous invite links for that parent slot on that case.
- A parent cannot be linked to a case they were not explicitly invited to.

## 11. Dependencies

- **Case Management:** Invites are initiated from the case detail page. The parent's account is linked to a case.
- **User Authentication & Role Management:** Invite acceptance creates a parent account via the auth system. The parent role and case linkage must be established at account creation.
- **Mailgun:** Invitation emails are sent via Mailgun.
- **Calendar & Schedule Management:** Parent gains access to their custody calendar after accepting.
- **Parenting Plan Visualization:** Parent can view their finalized parenting plan after accepting.
- **AI-Guided Messaging:** Parent gains access to messaging tools after accepting.
- **Convex:** Invite token management, parent account creation, and case linkage.

## 12. Data / Inputs / Outputs

**Inputs:**
- Parent first name, last name, email (entered by professional)
- Parent's password (entered during registration)

**Data stored:**
- Invite record: invite ID, case ID, professional ID, parent name, parent email, invite token (hashed), expiry timestamp, status (pending / accepted / expired / cancelled), created at
- Parent user record (on acceptance): user ID, name, email, hashed password, role (parent), linked case ID, account status, created at

**Outputs:**
- Invitation email sent to parent (via Mailgun)
- Parent account created
- Case detail page invite status updated
- Audit log entries (invite sent, invite accepted, invite expired, invite cancelled)

**Key states:**
- Invite: `not_invited` → `pending` → `accepted` / `expired` / `cancelled`
- Parent account: created on invite acceptance

## 13. UX / Design Notes

- The invite initiation from the case detail page should be a simple, low-friction action — one click to open a small form, enter name and email, and send.
- The invitation email is the first impression parents have of CoParent Logic. It should be warm, clear, and professional — not intimidating given that parents may be in a stressful family situation. The professional's name and role should be prominent.
- The parent registration page should be minimal: name (pre-filled), email (pre-filled, read-only), password, confirm password. No extra steps. No professional-looking complexity.
- After registration, the parent should immediately see their case (calendar, plan) — not an empty state. If the professional hasn't yet added plan or calendar data, a friendly placeholder should guide them.
- Case detail page invite status should be clearly readable at a glance: color-coded status badges (e.g., grey = not invited, yellow = pending, green = active, red = expired).
- Screen Mapping PNG contains parent invite and parent onboarding screens; design team should be consulted for layout specifics.

## 14. Edge Cases and Exceptions

- Parent is already a registered user (from a prior case or prior invite acceptance): Behavior when a parent's email is already registered is not defined. Options: (a) send them a case access link instead of a registration link, or (b) show an error to the professional. This is an open question.
- Professional invites the wrong person (wrong email): Professional must cancel the pending invite and re-invite with the correct email.
- Parent registers but then becomes inactive or case is archived: Parent's account persists but their access to case data should reflect the case status.
- Both co-parents are invited but only one accepts: Fully valid. The case can operate with one active parent.
- A parent uses the invite from a different device than where they opened the email: The invite link must work cross-device (standard web link, no device binding).

## 15. Non-Functional Considerations

- **Security:** Invite tokens must be cryptographically random and single-use. Expired tokens must not work. Parent accounts must be strictly scoped to their linked case(s).
- **Privacy:** Invite emails contain the parent's name and the professional's name — both are PII. Emails must be handled per the platform's data policies.
- **Reliability:** Invite email delivery failure must be surfaced to the professional. Silent failures are not acceptable.
- **Accessibility:** Invite acceptance and registration flow must meet WCAG 2.1 AA standards. Parents may be using mobile devices; the flow must be mobile-responsive.

## 16. Open Questions / Assumptions

- **Invite expiry duration:** Not defined in source materials. Assumption: 7 days (standard for invite-based onboarding).
- **Existing user invite:** Behavior when a parent's email is already registered is not defined. This is a significant edge case that needs product decision before implementation.
- **Parent access to multiple cases:** Whether a parent can be linked to more than one case (e.g., they are involved in two separate proceedings with two different professionals) is not addressed. Assumption: one case per parent account in Phase 1.
- **Invite email customization:** Whether the professional can add a personal message to the invite email is not defined. Assumption: not customizable in Phase 1.
- **Parent notification of plan/calendar updates:** After accepting, whether parents receive notifications when the professional updates plan or calendar data is not defined in this feature's scope — likely belongs to the Calendar and Plan Visualization features.

## 17. Source Summary

- **Product Discovery Report.docx:** Described parent role as invite-based (Post-MVP originally, but clarified as Phase 1 invite in the meeting). Parents get visual parenting plan access and are invited by professionals.
- **Meeting Transcript (April 9, 2026):** Key decision confirmed — parent access is invite-only in Phase 1 to avoid unrepresented parent edge cases. Free parent access in Phase 1. Lucy (stakeholder) confirmed that professionals (especially parent coordinators) have authority to direct parents to alternative tools, making invite-based onboarding workable.
- **Screen Mapping.png:** Contains parent invite and registration screens; not fully analyzed.
- **Confidence:** High for invite-only model, free access, and core invite flow. Medium for email content and UX specifics. Low for existing-user edge case handling and multi-case parent support.
