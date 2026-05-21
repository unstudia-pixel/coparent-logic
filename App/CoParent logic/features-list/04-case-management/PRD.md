# PRD: Case Management

## 1. Feature Name
Case Management

## 2. Feature Summary
Allows verified, subscribed Family Law Professionals to create and manage cases representing co-parent family units. A case is the central organizing unit of the platform — all parenting plan analysis, communication pattern analysis, and parent invitations are scoped to a case. Cases store family metadata including parent identities, children's information, jurisdiction, and civil case numbers.

## 3. Product Context
Case Management is the structural backbone of the professional's workflow. Every other professional-facing feature — PlanGuard analysis, communication analysis, parent invitations — operates within the context of a case. Without a case, there is nothing to analyze or share.

- **User journey position:** First action a professional takes after completing onboarding and subscribing. The dashboard displays active cases as the primary view.
- **Product goals supported:** Organizing professional workflow, enabling per-case AI analysis, establishing the data scope for audit and reporting.
- **Interacts with:** Parenting Plan Analyzer (analyses are attached to a case), Communication Pattern Analysis (messages are ingested and analyzed within a case), Parent Invite & Access (parents are invited to a case), Admin Portal (admins can view cases for oversight), Subscription & Billing (case volume per seat is a key metric).
- **Shared constraints:** All cases are scoped to the professional who created them. In Phase 1, cases cannot be shared between professionals. All case data is subject to the platform's encryption and ZDR policies.

## 4. Problem Statement
Family law professionals manage multiple active cases simultaneously, each involving different families, custody arrangements, and legal contexts. Without organized case structure, AI analysis outputs, communication logs, and plan documents would be mixed and unusable. Professionals need a clear, case-scoped workspace that reflects the reality of their practice.

## 5. Goals

**User goals:**
- Professionals can create a case quickly and with enough metadata to provide context for AI analysis.
- Professionals can navigate between their cases easily.
- Case information is clearly organized and accessible.

**Business goals:**
- Case volume per seat is a key platform metric. Case management must make it easy to create and activate new cases.
- Cases provide the audit scope for all platform activity (who analyzed what, when, for which case).

**Operational goals:**
- Case data is cleanly structured so AI features can consume it as context.
- All case activity is logged.

## 6. Users / Personas

**Family Law Professional (case owner)**
- Why they use this feature: To organize their work by family/case and scope all analyses and communications to the right context.
- What they need: Fast case creation, clear case overview, easy navigation between cases, and the ability to update case metadata.
- Role-specific behavior: Owns and manages all cases they create. Cannot see cases created by other professionals in Phase 1.

**CoParent Logic Admin**
- Why they use this feature: To oversee platform usage and investigate issues.
- What they need: Read-only access to all cases across all professionals for audit and support purposes.
- Role-specific behavior: Cannot create or modify cases. View-only.

**Parent**
- Parents do not manage cases. They are linked to a case when invited by the professional and can only view information relevant to their own case (calendar, plans, messages).

## 7. Feature Scope

**In scope:**
- Case creation with required and optional metadata fields
- Case listing / dashboard view (active, archived cases)
- Case detail view
- Case editing (updating metadata)
- Case archiving (soft-delete / inactive state)
- Parent association to a case (professional links parent accounts to the case)
- Civil case number / label tracking
- Jurisdiction field
- Family metadata: parent labels/names, children's ages and names
- Parenting model / custody arrangement type
- Case status tracking (active / archived)
- All case activity logged for audit

**Out of scope:**
- Case sharing between professionals (Phase 2 — multi-seat firm accounts)
- Case transfer to another professional
- Court document repository (not described as a case management function)
- Automated case import from external systems
- Case closure with formal outcome recording (not defined in source materials)

## 8. Functional Requirements

1. A verified, subscribed professional can create a new case from their dashboard.
2. Case creation requires the following fields:
   - Case label / civil case number (free text, used to identify the case in legal context)
   - Parent 1 label / name and contact email
   - Parent 2 label / name and contact email
   - Number of children and their ages (and optionally names)
   - Jurisdiction / state
   - Parenting model / custody arrangement type (e.g., sole custody, shared custody, bird's nest — predefined list or free text, TBD)
   - Professional's role in the case (e.g., attorney, mediator, divorce coach — pre-populated from their account but can be changed per case)
3. All fields except case label and parent information are optional at creation but encouraged for AI analysis quality.
4. The professional's dashboard shows a list of their active cases with: case label, parent names, last activity date, and a quick-access link to the case.
5. Each case has a detail page showing all metadata, linked analyses, uploaded documents, and associated parent accounts.
6. The professional can edit case metadata at any time.
7. The professional can archive a case. Archived cases are not shown in the active list but remain accessible in an archived view.
8. When a parent is invited (via Parent Invite & Access feature), they are linked to a specific case. The case detail page shows which parent accounts are associated.
9. A case can have a maximum of 2 parent accounts linked (one per co-parent role).
10. All case creation, edit, and archive events are logged with timestamp and professional user ID.
11. The Admin can view all cases across all professionals in the Admin Portal (read-only).

## 9. Workflow / User Journey

**Case creation:**
1. Professional clicks "New Case" from the dashboard.
2. Completes the case creation form (required and optional fields).
3. Submits the form.
4. Case is created and the professional is redirected to the new case detail page.
5. The case appears in the active cases list on the dashboard.

**Ongoing case management:**
1. Professional opens a case from the dashboard.
2. Sees case overview: metadata, linked parents, recent activity, quick links to PlanGuard and communication analysis.
3. Can edit metadata, invite parents, upload documents, or navigate to analysis tools.

**Archiving a case:**
1. Professional opens a case and selects "Archive Case."
2. Confirmation dialog: "Archived cases are hidden from your active list but all data is preserved."
3. Confirms. Case moves to archived state.
4. Professional can view archived cases from a filtered view.

**Failure paths:**
- Required fields not completed at case creation: inline validation errors.
- Duplicate civil case number: warn the professional (same number may exist across different professionals — warning, not block, since different jurisdictions may reuse numbers — assumption).

## 10. Business Rules

- A case belongs exclusively to the professional who created it in Phase 1.
- A case must have at least a case label and one parent's information to be created.
- A case can have at most 2 linked parent accounts.
- Archived cases retain all data and are not deleted.
- Case data (including all analyses and communications) is scoped to the case — no analysis output is shared across cases.
- The platform must enforce that a parent can only access the case(s) they are explicitly linked to.

## 11. Dependencies

- **User Authentication & Role Management:** Only verified, subscribed professionals can create cases. RBAC must enforce case ownership.
- **Parent Invite & Access:** Parent linking to a case is initiated from the case detail page but executed via the invite feature.
- **Parenting Plan Analyzer:** Plans are uploaded and analyzed within the context of a case.
- **Communication Pattern Analysis:** Message ingestion and analysis are scoped to a case.
- **Admin Portal:** Admin view of cases requires read access to the case data store.
- **Convex:** Case data model and RBAC rules are implemented in Convex.

## 12. Data / Inputs / Outputs

**Inputs:**
- Case label / civil case number
- Parent 1 and Parent 2: name, contact email
- Children: names (optional), ages
- Jurisdiction / state
- Parenting model / custody type
- Professional's role in the case

**Data stored:**
- Case record: case ID, professional ID, label, parent data, children data, jurisdiction, parenting model, professional role, status (active/archived), created at, updated at
- Activity log: event type, case ID, user ID, timestamp

**Outputs:**
- Case detail page
- Case list on dashboard
- Activity log entries
- Case context passed to AI analysis features (PlanGuard, communication analysis)

**Key states:**
- `active` → `archived`

## 13. UX / Design Notes

- The dashboard described in the meeting transcript and prototype review shows "active cases" as the primary view for professionals — case management UI is the home base of the professional experience.
- Case cards in the list view should show enough at a glance (case label, parents, last activity) to allow quick navigation without opening the detail page.
- The case detail page should act as a hub — from here the professional can access all related tools (PlanGuard, communication analysis, parent management).
- Case creation form should be short and scannable. Optional fields should be clearly marked to reduce friction during creation.
- The Screen Mapping PNG shows the case management area; the prototype includes a dashboard with "case creation" as a primary action.

## 14. Edge Cases and Exceptions

- Professional creates a case but never invites parents — fully valid, as professionals may use PlanGuard for document drafting without linking actual parents.
- Professional has the same parent involved in two different cases (e.g., a parent has cases with two different attorneys) — each case is independent; parent accounts are per-invite and per-case-link.
- A parent's email changes after being linked to a case — mechanism for updating parent contact info not defined in source materials; flagged as open question.
- Professional account is suspended while having active cases — cases must remain accessible to the Admin for audit but the professional loses access.
- Case is archived while a parent is still actively using the platform — parent access to the case's calendar and messaging should be reviewed; behavior not defined in source materials.

## 15. Non-Functional Considerations

- **Security:** Case data is sensitive family and legal information. Strict RBAC must ensure professionals can only access their own cases. Parents can only access their linked case data.
- **Auditability:** All case creation, modification, and archiving events must be logged. The legal context of the platform makes audit trails essential.
- **Performance:** The case list dashboard must load quickly even for professionals with many cases. Pagination or virtualization may be needed.
- **Data privacy:** All case data including parent and children names/ages is PII and must be encrypted at rest.

## 16. Open Questions / Assumptions

- **Parenting model options:** A predefined list of custody/parenting model types is not defined in source materials. Needs product input on whether this is a free-text field or a controlled vocabulary.
- **Civil case number uniqueness:** Whether the platform enforces uniqueness across all cases or only warns on duplicate is not defined. Assumption: warn but do not block, since numbers may legitimately repeat across jurisdictions.
- **Case transfer:** How a case is handled if a professional changes roles (e.g., an attorney is replaced by a new attorney) is not addressed. Flagged for Phase 2.
- **Parent contact update:** No mechanism described for updating a parent's email after they have been linked to a case.
- **Archived case parent access:** Whether parent access continues to archived case data (calendar, messaging) is not defined.
- **Number of children:** No maximum defined. Assumption: up to 10 children per case (edge case for large families).

## 17. Source Summary

- **Product Discovery Report.docx:** Described case management as requiring civil number/label-based creation, family metadata collection (parent labels, children ages, parenting model), professional role verification per case, and jurisdiction-specific localization. Listed case management as an MVP epic.
- **Meeting Transcript (April 9, 2026):** Confirmed that professionals create cases and invite parents from within cases. Discussed that professionals have different roles per case (attorney, parent coordinator, etc.). Dashboard prototype shown to Lucy included "active cases" as a primary view.
- **Screen Mapping.png:** Shows case creation and case detail screens; specific layout details not fully analyzed.
- **Confidence:** High for case structure and metadata requirements. Medium for field optionality and validation rules. Low for parenting model taxonomy and edge cases around multi-case parent relationships.
