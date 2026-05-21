# PRD: Parenting Plan Visualization

## 1. Feature Name
Parenting Plan Visualization

## 2. Feature Summary
A parent-facing read-only view of the finalized parenting plan, shared by the Family Law Professional from within a case. Once the professional finalizes and locks a plan in PlanGuard™, it is published to the parent-facing app where both co-parents can view it. The plan populates the custody calendar. Parents cannot edit the plan — the professional retains full authorship and control. Independent parents (Phase 2) will be able to upload their own plans.

## 3. Product Context
Parenting Plan Visualization is the bridge between the professional's PlanGuard™ drafting work and the parents' day-to-day experience. It closes the loop — the professional refines the plan, and the parents receive a clear, accessible version that also drives their calendar.

- **User journey position:** Available to parents after they have accepted their invite AND the professional has finalized and shared a plan. It is a persistent view — parents return to it whenever they need to reference the custody agreement.
- **Product goals supported:** Transparency and alignment between co-parents on the agreed custody arrangement; reducing plan-related disputes; driving parent engagement on the platform.
- **Interacts with:** Parenting Plan Analyzer / PlanGuard™ (the professional finalizes a plan here before it is shared), Calendar & Schedule Management (the plan's schedule provisions populate the custody calendar), Parent Invite & Access (parents must be onboarded to see the plan), Case Management (plans are scoped to a case).
- **Shared constraints:** The plan is read-only for parents. Only the professional can update it. If the professional updates the plan, the parent view must reflect the latest version. Plan history is accessible to the professional but only the current version is shown to parents (or all versions — TBD).

## 4. Problem Statement
Co-parents often have different interpretations of what the parenting plan says — sometimes because the plan is ambiguous (addressed by PlanGuard™), sometimes because one parent hasn't read it carefully, and sometimes because the document is buried in email or a legal folder and rarely referenced. Making the finalized plan accessible, searchable, and visually clear within the app removes this ambiguity and reduces referencing disputes.

## 5. Goals

**User goals:**
- Parents can easily access and read their finalized parenting plan at any time.
- Parents can navigate the plan without needing to scroll through a raw legal document.
- Parents understand what the plan requires of them without needing to consult their attorney for basic questions.

**Business goals:**
- Increase plan-to-calendar utilization (a plan that populates the calendar drives calendar engagement).
- Reduce professional time spent re-explaining plan provisions to parents.
- Establish the platform as the authoritative source of truth for the custody agreement.

**Operational goals:**
- The professional has clear control over which version of the plan is published to parents.
- Plan updates are reflected in the parent view promptly.

## 6. Users / Personas

**Parent (both co-parents)**
- Why they use this feature: To read and reference the finalized parenting plan that governs their custody arrangement.
- What they need: A clear, structured, readable view of the plan — preferably organized by section (scheduling, medical, financial, etc.) rather than as a raw legal document.
- Role-specific behavior: Read-only. Both parents see the same plan. Cannot request changes through this interface (they would contact their professional separately).

**Family Law Professional**
- Why they use this feature: To publish (share) the finalized plan to the parents after completing the PlanGuard™ drafting process.
- What they need: A "Share Plan" or "Publish to Parents" action from within PlanGuard™ or the case detail page. Ability to update the shared plan if a revision is needed.
- Role-specific behavior: Full control over what is published. Can update the plan; updates propagate to the parent view. The professional does not view the parent-side UI directly.

## 7. Feature Scope

**In scope:**
- Professional "share/publish" action that pushes a finalized plan to the parent-facing view
- Parent-facing plan viewer: structured, readable display of plan content
- Plan organized by sections (derived from the plan structure — scheduling, medical, financial/expenses, holidays, etc.)
- Visual clarity: not a raw PDF dump, but a structured readable format
- Calendar population: shared plan's custody schedule provisions are used to populate the parent calendar
- Notification to parents when a plan is first published or updated
- Professional ability to update the shared plan (new version replaces or supplements the prior version)
- Version indicator for parents (e.g., "Plan updated on [date]")

**Out of scope:**
- Parent ability to edit or annotate the plan
- Parent ability to propose plan changes through this interface (no red-lining or comment flow)
- Independent parent plan upload (Phase 2)
- PDF download of the plan by parents (not confirmed in source materials for Phase 1 — court-ready PDF is Post-MVP)
- Side-by-side comparison of plan versions for parents
- Legal definitions or glossary for plan terms (not in source materials)

## 8. Functional Requirements

1. From PlanGuard™ (or the case detail page), the professional can select "Publish Plan to Parents" for a finalized plan version.
2. Confirmation dialog: "This plan will be visible to both co-parents. Continue?" Professional confirms.
3. The plan is marked as "published" and becomes visible in the parent-facing app for both co-parents linked to the case.
4. Both parents receive an in-app notification (and optionally email): "[Professional name] has shared your parenting plan. View it in the app."
5. The parent-facing plan view displays the plan content organized by logical sections. At minimum:
   - Custody/residential schedule
   - Decision-making authority (legal custody provisions)
   - Medical/health provisions
   - Education provisions
   - Holiday and vacation schedule
   - Communication rules
   - Financial/expense provisions (if included)
   - Other provisions
6. Each section is clearly labeled and navigable via a table of contents or section tabs.
7. Within each section, plan provisions are displayed in plain, structured text — not as a raw scanned document.
8. The custody schedule section is visually linked to the calendar — a prominent indicator or button allows parents to see the calendar populated from this plan.
9. The plan is read-only for parents. No edit controls are shown.
10. If the professional publishes an updated version of the plan, parents receive a notification ("Your parenting plan has been updated on [date]."). The new version replaces the previous display. A version timestamp is shown.
11. Parents can view previous plan versions (assumption: at minimum, the current and most recent prior version are accessible — full version history TBD).
12. All plan publish and update actions by the professional are logged.

## 9. Workflow / User Journey

**Professional publishes a plan:**
1. Professional finalizes a plan in PlanGuard™.
2. From the plan view or case detail page, clicks "Publish to Parents."
3. Confirmation dialog is shown.
4. Professional confirms.
5. Plan is published. Both parents are notified.

**Parent views the plan:**
1. Parent receives a notification: "Your parenting plan is available."
2. Parent opens the app and navigates to the Plan section (or taps the notification).
3. Sees the plan organized by sections.
4. Navigates to the schedule section.
5. Sees a link: "View this schedule in your calendar."
6. Taps through to the calendar, which shows custody days populated from the plan.

**Professional updates the plan:**
1. Professional makes changes to the plan in PlanGuard™ (new iteration).
2. Publishes the updated version.
3. Both parents are notified of the update.
4. Parents' plan view shows the updated plan with a "Updated on [date]" indicator.

**Failure paths:**
- No plan has been published yet: parent plan section shows a placeholder ("Your parenting plan will appear here once your professional has finalized and shared it.").
- Plan publish fails (server error): professional sees an error; plan is not shown to parents. Retry available.
- Parent's account is inactive or invite not yet accepted when plan is published: plan is queued and visible as soon as the parent accepts their invite.

## 10. Business Rules

- Only the professional can publish a plan to parents. Parents cannot self-publish or upload plans in Phase 1.
- The published plan is locked from parent editing — it is the professional's final, AFCC-reviewed document.
- Both co-parents in a case see the same plan — there are no parent-specific versions.
- When the professional publishes an updated plan, it immediately replaces the prior display. Parents are notified.
- The calendar is populated from the custody schedule provisions of the most recently published plan.
- A plan cannot be "unpublished" once shared — it can only be superseded by a newer version (assumption — needs confirmation).

## 11. Dependencies

- **Parenting Plan Analyzer (PlanGuard™):** The plan must be finalized in PlanGuard™ before it can be published. Plan content and structure come from the PlanGuard™ output.
- **Calendar & Schedule Management:** The custody schedule section of the plan directly populates the calendar. A structured, machine-readable schedule format is needed from PlanGuard™.
- **Parent Invite & Access:** Parents must have accepted their invite before they can see the plan.
- **Case Management:** Plans are scoped to a case.
- **Convex:** Plan version storage, publish state, and parent notification triggers.
- **Mailgun (optional):** Email notifications to parents on plan publish/update.

## 12. Data / Inputs / Outputs

**Inputs:**
- Professional publish action (triggers from PlanGuard™ or case detail page)
- Plan content (structured sections derived from PlanGuard™ output)

**Data stored:**
- Published plan record: plan ID, case ID, version number, publish timestamp, content (structured sections), professional ID, status (published / superseded)
- Plan view events (for analytics): parent ID, case ID, timestamp, action (plan opened, section viewed)

**Outputs:**
- Parent-facing plan view (structured, section-based display)
- In-app and email notifications to parents on publish/update
- Calendar population data (custody schedule fed to Calendar feature)
- Audit log entries (professional publish actions)

**Key states:**
- Plan: `draft` → `published` (from PlanGuard™)
- Published plan: `current` / `superseded` (when a newer version is published)

## 13. UX / Design Notes

- The parent plan view should feel like a clear, organized document — not a legal PDF. Think structured card-based or tab-based sections rather than continuous scrolling text.
- Each section should have a clear title and use plain-language labels where possible (e.g., "Who has the children and when" rather than "Residential custody provisions").
- The link between the schedule section and the calendar should be visually prominent — this is a key engagement driver.
- The "Updated on [date]" indicator for plan updates should be noticeable but not alarming — parents should understand that updates are a normal part of the process.
- On first plan publication, a brief explainer may help parents understand what they're looking at ("This is the parenting plan created by [Professional name]. It outlines the agreed custody arrangement.").
- Mobile-first: parents will access the plan from their phones. Sections should be easily tappable and readable on small screens.
- If the plan was generated from a scanned/OCR'd document (raw text), structured display may require section parsing by the AI during PlanGuard™ analysis. This is a technical dependency on PlanGuard™ output format.
- Screen Mapping PNG contains parent-facing plan screens; design team should confirm section layout and navigation patterns.

## 14. Edge Cases and Exceptions

- Plan has only a few sections (minimal custody agreement): display available sections, no empty section placeholders.
- Plan is extremely long (complex custody agreement with many provisions): section navigation is essential to avoid overwhelming parents.
- Professional publishes a new version while a parent is actively viewing the plan: behavior not defined — options: show a notification in the current view ("The plan has been updated, reload to see changes") or wait until the parent navigates away. Assumption: show a notification.
- Parent views plan before co-parent has accepted invite: parent can still view the plan; the co-parent's access to the same plan is independent.
- Plan is in a format that cannot be structured into sections (e.g., a poorly formatted input that PlanGuard™ couldn't parse cleanly): a fallback plain-text display may be needed. Assumption: minimum viable display is better than no display.
- Case archived while plan is published: parent access to archived case plan is not defined.

## 15. Non-Functional Considerations

- **Security:** The published plan is sensitive legal PII. Access is strictly limited to the two co-parents and the professional in the case.
- **Immutability:** Published plan versions are historical records — they should not be modifiable after publication (superseded, not deleted).
- **Performance:** Plan view must load quickly on mobile. Large plan documents should not cause slow load times.
- **Auditability:** All professional publish/update actions are logged. Parent plan views may be logged for analytics.
- **Accessibility:** Plan content must be readable and navigable for users with accessibility needs (screen readers, high-contrast modes).

## 16. Open Questions / Assumptions

- **Section parsing:** How the plan content from PlanGuard™ is structured into navigable sections for the parent view is a key open question. If PlanGuard™ only returns raw text with flagged clauses, additional parsing logic is needed to generate a section-based view.
- **Parent version history:** Whether parents can see previous plan versions or only the current one is not defined. Assumption: current version only, with a "Last updated on [date]" indicator.
- **PDF download for parents:** Whether parents can download a PDF copy of the plan is not confirmed for Phase 1. Assumption: not available in Phase 1 (court-ready PDF is Post-MVP per discovery report).
- **Plan un-publish:** Whether a professional can withdraw a published plan (e.g., in an emergency) is not addressed. Assumption: not supported; a new version must be published.
- **Independent parent plan upload:** Phase 2 feature — parents without a professional can upload their own plans. The architecture for this feature should not preclude this future use case.
- **Calendar schedule format:** A structured, machine-readable schedule format must be agreed between PlanGuard™ and the Calendar feature teams. This is a key technical dependency.

## 17. Source Summary

- **Product Discovery Report.docx:** Described parent role as having "visual parenting plan access." Plan populates the parent calendar. Independent parent plan upload is Phase 2. Court-ready PDF report is Post-MVP.
- **Meeting Transcript (April 9, 2026):** Confirmed that finalized professional plans are locked for parent viewing only. Parent cannot edit the plan. Plan populates the parent-facing calendar. Independent parents (Phase 2) can upload their own plans.
- **Screen Mapping.png:** Contains parent-facing plan screens; not fully analyzed.
- **Confidence:** High for read-only model, professional publish control, and calendar population intent. Medium for section structure of the parent view. Low for section parsing technical approach, version history for parents, and PDF download availability.
