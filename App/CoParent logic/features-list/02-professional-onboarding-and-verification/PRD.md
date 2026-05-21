# PRD: Professional Onboarding & Credential Verification

## 1. Feature Name
Professional Onboarding & Credential Verification

## 2. Feature Summary
The workflow by which a newly registered Family Law Professional submits their credentials for review, and a CoParent Logic Admin verifies and approves or rejects them before granting full platform access. This is a trust-and-safety gate that ensures only legitimately credentialed practitioners use the platform's AI tools in legal contexts.

## 3. Product Context
This feature sits immediately after initial registration (User Authentication & Role Management) and immediately before subscription activation (Subscription & Billing). A professional's account is in a "pending verification" state from signup until this process is complete.

- **User journey position:** Post-registration, pre-subscription. Professionals cannot access case creation, PlanGuard, or communication analysis until verified.
- **Product goals supported:** Legal credibility, liability protection, professional trust signal to all platform users.
- **Interacts with:** User Authentication & Role Management (account state transitions), Admin Portal (Admin reviews submissions here), Subscription & Billing (subscription step unlocks only after verification), Case Management (verified professionals can create cases).
- **Shared constraints:** All verification decisions are logged. Rejected professionals receive an explanation and can resubmit.

## 4. Problem Statement
Family law professionals operate in a regulated, legally sensitive environment. The platform's value proposition — court-defensible AI analysis — depends on only qualified practitioners using it. If any person can access PlanGuard or communication analysis, the professional legitimacy of outputs is undermined and the company faces liability. Manual credential review in Phase 1 is the safest approach while the user base is small.

## 5. Goals

**User goals:**
- Professionals can complete the verification process with minimal friction and clear guidance on what to submit.
- Professionals receive timely feedback on the status of their application.

**Business goals:**
- Ensure that only credentialed practitioners access AI tools.
- Build a whitelist of approved professionals that the Admin team can manage and audit.
- Create a foundation for AI-assisted validation (planned for Phase 2) by capturing structured credential data in Phase 1.

**Operational goals:**
- Admin can review, approve, and reject credential submissions efficiently.
- All decisions are logged for audit purposes.

## 6. Users / Personas

**Family Law Professional (applicant)**
- Why they use this feature: To submit credentials and gain full platform access.
- What they need: Clear instructions on what documents are required, a simple upload interface, and status visibility while waiting for review.
- Role-specific behavior: Can resubmit if rejected. Cannot access full platform features until approved.

**CoParent Logic Admin (reviewer)**
- Why they use this feature: To review, approve, or reject professional credential submissions.
- What they need: A clear queue of pending submissions with document previews and the ability to approve, reject with a reason, or request additional information.
- Role-specific behavior: Decision triggers an automated notification to the professional and updates their account state.

## 7. Feature Scope

**In scope:**
- Professional credential upload (documents: Bar ID, court appointment orders, or equivalent professional certification)
- Upload support for PDF and image formats (JPG, PNG)
- Admin review interface showing submitted documents and professional profile
- Admin approve / reject actions (with required rejection reason)
- Automated email notification to professional on approval or rejection
- Professional ability to resubmit after rejection
- Account state tracking (pending / approved / rejected / resubmitted)
- Audit log of all Admin decisions with timestamps and reviewer identity

**Out of scope:**
- AI-assisted credential validation (planned Phase 2 — Bar ID lookup against state registries)
- Automated approval without Admin review (Phase 2)
- Credential expiry tracking or renewal reminders (not in source materials for MVP)
- Video or identity verification (not mentioned)

## 8. Functional Requirements

1. After email verification, a Family Law Professional is presented with a credential submission step before they can proceed.
2. The credential upload form collects: professional role type (attorney, mediator, divorce coach, forensic evaluator, parent coordinator, guardian ad litem), jurisdiction/state, and one or more supporting documents.
3. Accepted document types: PDF, JPG, PNG. Maximum file size: TBD (assumption: 10 MB per file).
4. The professional can upload multiple documents in a single submission.
5. Upon submission, the account moves to "pending credential review" state and the professional is shown a confirmation screen explaining that review is underway.
6. The professional cannot access any case, analysis, or plan features while in pending state.
7. The Admin receives a notification (in-app and/or email) when a new credential submission is awaiting review.
8. The Admin review interface displays: professional's name, email, role type, jurisdiction, submitted documents (viewable inline or downloadable), and submission timestamp.
9. The Admin can approve the submission. On approval: account state moves to "pending subscription," and the professional receives an approval email prompting them to subscribe.
10. The Admin can reject the submission with a mandatory written reason. On rejection: account state moves to "rejected," the professional receives a rejection email with the reason and instructions to resubmit.
11. A rejected professional can update their submission and resubmit. The Admin review queue shows resubmissions with the original submission and rejection reason visible for context.
12. All Admin decisions (approve, reject) are logged with: Admin user ID, timestamp, decision, and reason (for rejections).
13. A professional who has been approved and then has their account suspended by Admin loses access until reactivated.

## 9. Workflow / User Journey

**Professional credential submission:**
1. Professional completes email verification (from auth flow).
2. Redirected to credential submission screen.
3. Selects their professional role type and jurisdiction.
4. Uploads one or more credential documents.
5. Submits the form.
6. Sees confirmation: "Your credentials are under review. You'll receive an email once reviewed."
7. Professional account enters "pending credential review" state.

**Admin review:**
1. Admin logs in and navigates to the credential review queue (in Admin Portal).
2. Sees a list of pending submissions sorted by submission date.
3. Opens a submission, reviews documents and professional profile.
4. Chooses: Approve or Reject.
   - If Approve: clicks Approve, confirmation dialog, confirms. Professional is notified by email.
   - If Reject: must enter a rejection reason, clicks Reject. Professional is notified by email with reason.
5. Submission is removed from the active queue and moved to "reviewed" history.

**Professional resubmission after rejection:**
1. Professional receives rejection email with reason.
2. Logs in and is shown their rejected submission with the Admin's reason.
3. Updates documents or information and resubmits.
4. Account re-enters "pending credential review" state.
5. Admin review queue receives the resubmission with full history visible.

**Failure paths:**
- File upload fails (too large, wrong format): display inline error with guidance.
- Professional closes the browser before completing submission: progress is not saved; they must restart from the credential upload step on next login.
- Admin approves a professional who later turns out to be fraudulent: Admin can suspend the account (handled in Admin Portal feature).

## 10. Business Rules

- A professional cannot access any AI features, create cases, or manage parenting plans until their account is in "approved + active subscription" state.
- Rejection reasons must be provided by the Admin — blank rejections are not permitted.
- A professional can resubmit credentials an unlimited number of times (no defined cap in source materials — assumption).
- Approved professionals are added to the "professional whitelist" maintained in the Admin Portal.
- Admin decisions are final unless overridden by another Admin.
- All credential documents uploaded by professionals must be treated as sensitive PII and stored with encryption at rest.

## 11. Dependencies

- **User Authentication & Role Management:** Account must exist in "pending credential review" state before this feature activates.
- **Admin Portal:** The credential review queue is a core component of the Admin Portal feature. These two features share the review interface.
- **Subscription & Billing:** Approval triggers the subscription step. This feature must emit a state change that triggers the subscription prompt.
- **Mailgun:** Approval and rejection emails are sent via Mailgun.
- **Convex:** Document storage and account state management.
- **Azure Document Intelligence (OCR):** Referenced in source materials as the OCR tool — may be used in Phase 2 for automated credential reading, but in Phase 1 documents are reviewed manually by Admin.

## 12. Data / Inputs / Outputs

**Inputs:**
- Professional role type (selected from predefined list)
- Jurisdiction / state
- Credential documents (PDF, JPG, PNG)

**Data stored:**
- Credential submission record: submission ID, professional user ID, role type, jurisdiction, document file references, submission timestamp, status, resubmission history
- Review record: Admin user ID, decision, reason (if rejected), decision timestamp

**Outputs:**
- Account state change (pending → approved or rejected)
- Approval email to professional
- Rejection email with reason to professional
- Audit log entries

**Key states:**
- `pending_credential_review` → `approved` (→ triggers subscription prompt)
- `pending_credential_review` → `rejected` (→ professional can resubmit)
- `rejected` → `pending_credential_review` (on resubmission)

## 13. UX / Design Notes

- The credential upload step should be presented as a guided, multi-step form rather than a single upload screen — helping professionals understand what is needed and why.
- The pending state screen (shown while awaiting review) should set clear expectations: estimated review time (TBD — not defined in source materials), and clear next steps.
- The Admin review interface should present documents in a readable, inline format — Admins should not need to download files to review them.
- Rejection reason field in Admin UI should be a free-text area, prominent, and required before the reject button is enabled.
- Professional resubmission view should clearly show the Admin's rejection reason in a visually distinct section above the resubmission form.
- Screen Mapping PNG shows onboarding flows but specific credential upload UI was not fully described in text; design team must provide component-level specs.

## 14. Edge Cases and Exceptions

- Professional uploads a file that is not a valid credential document (e.g., a photo of unrelated content) — Admin reviews and rejects with reason. System cannot auto-detect document validity in Phase 1.
- Professional submits credentials and then unsubscribes before being reviewed — account remains in review queue; Admin decision still proceeds.
- Admin accidentally approves the wrong submission — no undo in UI; Admin must manually suspend the account and handle via support.
- Professional's credential document is expired — Admin decides based on review; system does not enforce expiry in Phase 1.
- Multiple admins review the same submission simultaneously — system should prevent double-approval/rejection (optimistic locking or assignment model TBD).

## 15. Non-Functional Considerations

- **Security:** Credential documents contain sensitive PII. Storage must be encrypted at rest. Access to documents restricted to Admin role and the submitting professional.
- **Auditability:** Every Admin decision must be logged with full context. This is a compliance requirement given the legal context of the platform.
- **Performance:** Document upload must handle files up to 10 MB (assumption) without timeout. Admin review queue must be responsive even as the queue grows.
- **Reliability:** Submissions must not be lost if the server experiences a transient error. Uploads should be confirmed only after successful persistence.

## 16. Open Questions / Assumptions

- **Review SLA:** No target review time is defined in source materials. An unreviewed queue could block professional activation and hurt conversion. Assumption: Admin reviews within 1–2 business days, but an SLA and reminder mechanism should be defined.
- **Document types accepted:** Source materials mention Bar IDs and court appointment orders. The full list of accepted credential types per professional role is not defined. Needs product/legal input.
- **Multi-document limit:** Maximum number of documents per submission not defined. Assumption: up to 5 documents.
- **Jurisdiction scope:** Platform appears US-focused (Bar IDs, state registries, AFCC). Assumption: jurisdiction field is US state only for MVP.
- **Notification to Admin:** Whether the Admin receives email, in-app notification, or both for new submissions is not specified.
- **Concurrent Admin review:** No lock/assignment model is described. Needs technical design to prevent race conditions.

## 17. Source Summary

- **Product Discovery Report.docx:** Described credential verification as a requirement for professional access. Mentioned Admin role responsible for verification. Referenced AI-assisted Bar ID validation as a Phase 2 capability.
- **Meeting Transcript (April 9, 2026):** Confirmed professional onboarding as a Phase 1 priority. Discussed that different professional roles (attorney, parent coordinator, guardian ad litem) have different relationships with clients and court authority.
- **Screen Mapping.png:** Shows onboarding flow screens; specific UI layouts not fully analyzed.
- **Confidence:** High for overall flow and role structure. Medium for specific document requirements and UX details. Low for review SLA, concurrent admin handling, and document type completeness.
