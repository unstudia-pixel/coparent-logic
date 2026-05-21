# PRD: User Authentication & Role Management

## 1. Feature Name
User Authentication & Role Management

## 2. Feature Summary
Secure, role-based registration and login system for all platform users. Supports three distinct roles — Family Law Professional, Parent, and CoParent Logic Admin — each with different registration paths, access levels, and permissions. This feature is the entry point to the entire platform and enforces role-based access control (RBAC) throughout.

## 3. Product Context
This is the foundational feature of CoParent Logic. Every other feature depends on a user being authenticated and assigned a valid role. The platform is a professional-grade AI tool for family law practitioners, which means access must be controlled strictly — wrong-role access to sensitive case data or AI tools could have legal consequences.

- **User journey position:** First touchpoint for all users before any functionality is accessible.
- **Product goals supported:** Trust and safety, legal defensibility, compliance with data access controls.
- **Interacts with:** Professional Onboarding & Verification (professionals must complete verification after signup), Subscription & Billing (professionals must subscribe before full access), Case Management (only verified professionals can create cases), Parent Invite & Access (parents are created through invite, not self-registration in Phase 1. Parents access is not part of this PRD, it will be on mob app. This PRD is for web app only), Admin Portal (admins manage platform state. Admin portal is not part of this PRD. Admin portal will be using refine.dev).
- **Shared constraints:** All users are subject to RBAC. Role assignment happens at registration and is immutable unless changed by an Admin. All auth events are audit-logged.

## 4. Problem Statement
Family law professionals need a platform that ensures only verified, credentialed practitioners can access sensitive AI analysis tools. Parents need a simplified, controlled entry path that doesn't expose them to professional tools. Admins need internal control over platform access without a self-service admin registration path.

## 5. Goals

**User goals:**
- Professionals can register, upload credentials, and access the platform with minimal friction.
- Parents can accept an invite and set up their account without needing to understand the professional side of the product.
- Admins can log in and manage the platform without special setup complexity.

**Business goals:**
- Ensure only verified professionals access sensitive tools (legal liability mitigation).
- Maintain a clean, controlled onboarding funnel that drives subscription conversion for professionals.
- Support future expansion of user roles (e.g., firm-level accounts in Phase 2).

**Operational goals:**
- Full audit trail of all login, registration, and role-change events.
- RBAC enforced at both UI and API levels.

## 6. Users / Personas

**Family Law Professional**
- Why they use this feature: To register for and access the platform.
- What they need: A clear, professional signup flow that collects their credentials and guides them to verification and subscription.
- Role-specific behavior: Must complete credential upload before gaining full access. Subscription required. Access is gated pending Admin approval.

**Parent**
- Why they use this feature: To accept an invitation and create an account to access their case's calendar and communication tools.
- What they need: A simple, low-friction invite acceptance flow. No credential upload. No subscription payment.
- Role-specific behavior: Cannot self-register. Account is created only when invited by a verified professional. Free access in Phase 1.

**CoParent Logic Admin**
- Why they use this feature: To log in and manage the platform.
- What they need: Secure login. No public registration path.
- Role-specific behavior: Account created internally. Has elevated permissions to manage professionals, view all cases at a platform level, and access audit logs.

## 7. Feature Scope

**In scope:**
- Email/password registration for Family Law Professionals
- Multi-factor authentication 
- Invite-based account creation for Parents (invite flow owned by Parent Invite & Access feature, but auth system must support it)
- Login for all roles (email/password)
- Session management (tokens, expiry, re-authentication)
- Role assignment at registration
- RBAC enforcement at route/API level
- Password reset / forgot password flow
- Audit logging of all auth events (login, logout, failed attempts, password changes)
- Admin account provisioning (internal only, no public signup)

**Out of scope:**
- Social login (Google, LinkedIn) — not mentioned in source materials
- Firm-level or multi-seat accounts — Phase 2
- Independent parent self-registration — Phase 2
- SSO / enterprise identity integration — not in scope

## 8. Functional Requirements

1. A Family Law Professional can register using an email address and password.
2. Registration form collects: first name, last name, email, password, professional role type (attorney, mediator, divorce coach, forensic evaluator, parent coordinator, guardian ad litem).
3. Upon registration, the professional account is created in a "pending verification" state — full platform access is not granted until credentials are verified and a subscription is active.
4. The system sends a verification email upon registration to confirm the email address.
5. A Parent cannot self-register. Parent accounts are created only via a professional's invite action.
6. Admin accounts are created internally by the CoParent Logic team. There is no public Admin registration path.
7. All users (Professional, Parent, Admin) log in via email and password.
8. The system enforces RBAC: each route, page, and API endpoint is accessible only to users with the appropriate role.
9. A Professional in "pending verification" state sees a restricted UI prompting them to complete credential upload and await approval.
10. A Professional with verified credentials but no active subscription sees a restricted UI prompting them to subscribe.
11. A Professional with verified credentials and an active subscription has full access to all professional features.
12. Sessions expire after a defined inactivity period (specific timeout TBD).
13. Users can log out at any time, which invalidates their session.
14. A "Forgot Password" flow allows users to reset their password via a time-limited email link.
15. All authentication events (successful login, failed login, logout, password reset request, password change) are written to the audit log with timestamp, user ID, IP address, and event type.
16. The system prevents brute-force login attempts (rate limiting or lockout after N failed attempts — specific threshold TBD).

## 9. Workflow / User Journey

**Professional Registration:**
1. Professional navigates to the platform landing page and clicks "Sign Up."
2. Completes registration form (name, email, password, role type).
3. Receives email verification link; clicks to confirm email.
4. Redirected to credential upload step (handled by Professional Onboarding & Verification feature).
5. Account enters "pending verification" state until Admin approves credentials.
6. Once approved, professional is prompted to subscribe (handled by Subscription & Billing feature).
7. After subscribing, full platform access is granted.

**Parent Registration (invite-based):**
1. Professional sends an invite to a parent's email (handled by Parent Invite & Access feature).
2. Parent receives an invite email with a unique, time-limited link.
3. Parent clicks the link and is directed to a simplified registration form (name, password).
4. Account is created with the Parent role and linked to the inviting professional's case.
5. Parent is directed to their dashboard (calendar, messaging tools).

**Login (all roles):**
1. User navigates to login page.
2. Enters email and password.
3. System validates credentials and role.
4. User is redirected to the role-appropriate dashboard.

**Password Reset:**
1. User clicks "Forgot Password" on login page.
2. Enters their email address.
3. Receives a time-limited reset link.
4. Clicks the link, sets a new password.
5. Session is invalidated; user is prompted to log in again.

**Failure paths:**
- Incorrect credentials: display generic error ("Invalid email or password") without specifying which field is wrong.
- Expired invite link: display a message prompting the parent to contact their professional.
- Expired reset link: display a message prompting the user to request a new one.
- Rate limit hit: display a lockout message with a wait period.

## 10. Business Rules

- A Professional cannot access any AI features until both credential verification AND an active subscription are confirmed.
- Role is assigned at registration and cannot be changed by the user themselves.
- An Admin can change a user's role or deactivate an account.
- Parents are always linked to at least one professional at the time of account creation.
- A Parent invited by one professional is not automatically visible to other professionals.
- Admin accounts are not publicly accessible and must be provisioned manually.
- Password must meet minimum security requirements (minimum length and complexity — specific policy TBD).

## 11. Dependencies

- **Professional Onboarding & Verification:** Auth system must support a "pending verification" state that blocks access until triggered externally.
- **Subscription & Billing:** Auth system must support a "pending subscription" state that blocks full access until an active subscription is confirmed via Stripe.
- **Parent Invite & Access:** Invite link generation and acceptance depend on the auth system's ability to create accounts from invite tokens.
- **Admin Portal:** Admin role must be provisioned before any credential review or platform management can occur.
- **Convex (backend):** User accounts and sessions are managed in Convex. RBAC logic must be enforced at the Convex function level.
- **Mailgun (email):** Verification emails and password reset emails are sent via Mailgun.

## 12. Data / Inputs / Outputs

**Inputs:**
- First name, last name, email, password, professional role type (at registration)
- Email + password (at login)
- Email (for password reset request)

**Data stored:**
- User record: ID, name, email, hashed password, role, account status (pending verification / pending subscription / active / suspended), timestamps
- Auth events: event type, user ID, timestamp, IP address
- Invite tokens: token, expiry, invited email, inviting professional ID, status (pending / accepted / expired)

**Outputs:**
- Authenticated session token
- Role-appropriate redirect after login
- Verification email
- Password reset email
- Audit log entries

**Key states:**
- `unverified_email` → `pending_credential_review` → `pending_subscription` → `active`
- `suspended` (admin action)
- `invited` → `registered` (parent flow)

## 13. UX / Design Notes

- The Screen Mapping diagram shows separate entry flows for professionals and parents, suggesting the landing page and signup pages are role-aware or have separate routes.
- Professional signup should feel polished and credibility-building — it is serving legal professionals who will scrutinize trustworthiness.
- Parent invite acceptance should be minimal and non-intimidating — parents may be in stressful family law situations.
- The professional dashboard includes an onboarding state indicator (seen in prototype description: dashboard shows active cases, communication log, plan analysis — implying a post-auth, post-onboarding view).
- Pending states (awaiting verification, awaiting subscription) need clear UI explanations so professionals are not confused about why features are locked.
- Design details for login/signup screens are visible in the Screen Mapping PNG but specific layout details were not extracted in text form; design team should be consulted for exact component specs.

## 14. Edge Cases and Exceptions

- Professional registers with an email domain already associated with another professional account — system should allow it (individuals, not firms, in Phase 1) but flag for review.
- Parent's invite link expires before they register — they must be re-invited by the professional.
- Professional submits credentials but Admin never reviews them — a timeout/reminder mechanism may be needed (assumption: not defined in source materials, needs clarification).
- User attempts to log in to an account that has been suspended — display a clear message to contact support.
- User changes their email address — not addressed in source materials; assume not supported in Phase 1 without admin intervention.
- Two parents in the same case are both invited — both can have separate accounts linked to the same case.

## 15. Non-Functional Considerations

- **Security:** Passwords must be hashed (bcrypt or equivalent). No plain-text passwords stored. All auth traffic over HTTPS. Session tokens must be invalidated on logout.
- **Auditability:** All auth events logged with sufficient detail to support forensic review if needed (given the legal context of the platform).
- **RBAC:** Enforced at the API/backend level (Convex functions), not just the frontend. Frontend-only RBAC is insufficient for a legal platform.
- **Reliability:** Auth service must be highly available — a login failure locks all users out of the platform.
- **Data privacy:** User PII (email, name) must be encrypted at rest per the platform's Zero Data Retention (ZDR) posture.
- **Accessibility:** Login and registration forms must meet WCAG 2.1 AA standards.

## 16. Open Questions / Assumptions

- **Session timeout:** Specific session duration not defined. Assumption: standard inactivity timeout applies (e.g., 30–60 minutes), but this needs product confirmation.
- **Brute-force policy:** Rate limiting threshold not specified. Assumption: industry standard (e.g., 5 failed attempts triggers a 15-minute lockout).
- **Email change:** Not addressed in source materials. Assumption: not self-service in Phase 1.
- **Credential review SLA:** How long before an Admin must review a professional's credentials is not defined. This may affect the professional's experience and conversion rate.
- **Firm accounts:** Multi-seat/firm accounts are called out as Phase 2 but the auth model must not preclude this. Assumption: individual accounts only in Phase 1, but data model should allow future association to a firm entity.

## 17. Source Summary

- **Product Discovery Report.docx:** Defined three user roles (Professional, Parent, Admin), described signup flows per role, outlined credential verification requirement, RBAC, and Zero Data Retention compliance requirements.
- **Meeting Transcript (April 9, 2026):** Confirmed that parent access is invite-only in Phase 1 to avoid unrepresented parent edge cases. Confirmed free parent access in Phase 1. Discussed that some parents may be court-ordered to use OFW, so invite-only protects the platform from being used outside of a professional relationship.
- **Screen Mapping.png:** Shows separate flows for professional and parent entry paths; specific screen layouts not fully analyzed but confirm role-separated UX.
- **Confidence:** High for role structure and registration flows. Medium for specific UX details (design file referenced but not fully analyzed). Low for specific technical policies (session timeout, MFA, brute-force limits).
