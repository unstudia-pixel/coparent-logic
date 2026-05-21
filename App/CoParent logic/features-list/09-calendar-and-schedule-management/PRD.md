# PRD: Calendar & Schedule Management

## 1. Feature Name
Calendar & Schedule Management

## 2. Feature Summary
A shared calendar and scheduling tool for co-parents, accessible via the parent-facing mobile/web app. It displays the custody schedule derived from the finalized parenting plan, allows parents to create one-off events, and manages custody swap requests — including a confirmation/acceptance workflow between co-parents. All scheduling activity (including rejected swaps) is logged and visible to the professional for case oversight. Accepted swap requests lock the calendar to prevent later disruption.

## 3. Product Context
The Calendar is the primary day-to-day interaction surface for parents on the platform. It translates the legal custody schedule into a practical, shared operational tool for co-parent families.

- **User journey position:** Accessible to parents after they accept their invite and the professional has finalized and shared a parenting plan. The calendar is populated from the plan data.
- **Product goals supported:** Reducing scheduling conflict, providing a neutral shared record of custody events, and generating evidence of co-parent behavior (swap rejections, late changes) for professional review.
- **Interacts with:** Parenting Plan Visualization (plan data populates the calendar), Parent Invite & Access (parents must be onboarded before accessing the calendar), Case Management (all calendar activity is scoped to a case), Communication Pattern Analysis (scheduling-related behavior may inform behavioral analysis), AI-Guided Messaging (scheduling disputes may prompt message coaching).
- **Shared constraints:** The calendar is a shared view — both co-parents and the professional can see it. Changes require agreement between both parents via the swap request workflow. All scheduling events and decisions are logged as evidence.

## 4. Problem Statement
Co-parents frequently dispute custody schedules — who has the children on which days, whether a swap was agreed to, and whether the other parent followed the schedule. Without a neutral, shared, immutable scheduling record, these disputes escalate and become litigation flashpoints. The calendar provides a transparent, evidence-grade scheduling tool that reduces ambiguity and creates an auditable record.

## 5. Goals

**User goals:**
- Parents can see their custody schedule clearly and without confusion.
- Parents can request custody swaps and receive a clear response from the other co-parent.
- Parents can add one-off events to the shared calendar.

**Business goals:**
- Drive daily active engagement from parents (calendar is the highest-frequency use case).
- Create an evidence layer that supports professional case management.
- Differentiate from competitors by linking the calendar directly to the legal parenting plan.

**Operational goals:**
- All scheduling decisions (swaps accepted, rejected) are logged as evidence for professionals.
- Calendar state is always consistent — accepted swaps lock to prevent retroactive changes.

## 6. Users / Personas

**Parent (co-parent 1 and co-parent 2)**
- Why they use this feature: To see their custody schedule, manage events, and coordinate schedule changes with their co-parent.
- What they need: A clear, easy-to-read calendar showing who has the children on each day; a simple way to request and respond to swap requests; and a record of agreed-upon schedule changes.
- Role-specific behavior: Both parents see the same shared calendar. Either parent can initiate a swap request. The receiving parent accepts or declines.

**Family Law Professional**
- Why they use this feature: To monitor scheduling behavior, review swap request history, and identify scheduling-related conflicts.
- What they need: A read-only view of the case calendar with activity logs showing swap request history and outcomes.
- Role-specific behavior: Read-only. Cannot create events or initiate swaps on behalf of parents.

## 7. Feature Scope

**In scope:**
- Calendar view showing custody schedule (populated from the finalized parenting plan)
- Visual indication of which parent has custody on each day/period
- Parent ability to create one-off events (school events, medical appointments, etc.)
- Custody swap request workflow: request, accept, decline, lock
- Rejected swap requests logged and visible to the professional
- Accepted swap requests lock the calendar entry (cannot be modified retroactively)
- Professional read-only view of the case calendar with swap history
- In-app notification to the co-parent when a swap request is received
- In-app notification to both parents and the professional when a swap is accepted or rejected
- Calendar activity log (all events, swap requests, outcomes)

**Out of scope:**
- Automated schedule population without a finalized parenting plan (calendar requires a plan as its source)
- Professional ability to edit the calendar directly
- Integration with external calendars (Google Calendar, Apple Calendar) — not confirmed in source materials for Phase 1
- Recurring event creation by parents beyond what is in the parenting plan
- Financial event tracking (expense sharing, child support schedule) — separate feature area
- Court-ready schedule export — Post-MVP

## 8. Functional Requirements

1. The parent calendar is populated automatically when a professional finalizes and shares a parenting plan. The calendar reflects the custody schedule defined in the plan.
2. The calendar displays a monthly/weekly view with:
   - Days color-coded or labeled by which parent has custody
   - One-off events added by parents
   - Swap request status overlays (pending, accepted, rejected)
3. Either parent can create a one-off event by selecting a date and entering: event name, date/time, notes (optional). The event is visible to both parents and the professional.
4. Either parent can initiate a custody swap request by selecting a specific custody day and requesting to swap it with another day. The request includes:
   - The original custody day (currently assigned to the requesting parent's co-parent)
   - The proposed swap day (the requesting parent's custody day they are offering in exchange)
   - Optional message to the co-parent
5. The co-parent receives an in-app notification of the swap request.
6. The receiving co-parent can:
   a. Accept the swap → calendar is updated to reflect the swap; both days are locked (cannot be modified further); both parents and the professional are notified.
   b. Decline the swap → calendar remains unchanged; the decline is logged; both parents and the professional are notified.
7. A pending swap request expires if not responded to within a defined period (duration TBD — assumption: 48 hours).
8. Once a swap is accepted and locked, neither parent can modify those calendar entries. The professional can see the swap history.
9. All rejected swap requests are logged with: requesting parent, requested swap details, declining parent, timestamp, and are accessible to the professional from the case view.
10. The professional views the case calendar in read-only mode from the case detail page. They can see all events, swap history, and current custody assignments.
11. All calendar events and swap request outcomes are logged with timestamps in the case activity log.
12. Parents receive in-app (and optionally email) notifications for:
    - New swap request received
    - Swap accepted or declined
    - Reminder if a swap request is pending and approaching expiry

## 9. Workflow / User Journey

**Parent views custody schedule:**
1. Parent logs in to the parent dashboard.
2. Navigates to the Calendar.
3. Sees monthly view with custody days highlighted per parent.
4. Can switch to weekly view for more detail.

**Parent creates a one-off event:**
1. Parent clicks on a date in the calendar.
2. Selects "Add Event."
3. Enters event name, date/time, optional notes.
4. Saves. Event appears on the calendar for both parents and the professional.

**Parent initiates a swap request:**
1. Parent sees a custody day they need to swap.
2. Clicks on the day and selects "Request Swap."
3. Selects the swap day they are offering in exchange.
4. Optionally adds a message.
5. Submits the request.
6. Co-parent receives an in-app notification.
7. Calendar shows the swap as "Pending" on both affected days.

**Co-parent responds to a swap request:**
1. Co-parent sees the swap request notification.
2. Opens the calendar or notification.
3. Reviews the swap request details.
4. Accepts or Declines.
   - Accept: calendar updates, both days locked, both parents and professional notified.
   - Decline: calendar unchanged, decline logged, both parents and professional notified.

**Professional reviews swap history:**
1. Professional opens a case detail page.
2. Navigates to the calendar view.
3. Sees the current calendar with all events and swap overlays.
4. Can filter the activity log to show only swap requests and their outcomes.

**Failure paths:**
- Swap request sent to expired/inactive parent account: system should handle gracefully (notification not delivered but request is still logged).
- Parent tries to swap a locked (already-swapped) day: display an error explaining the day is locked.
- Swap request expires without a response: status changes to "Expired." Both parents and professional notified. Calendar returns to original state.

## 10. Business Rules

- The base custody schedule on the calendar is derived from and controlled by the finalized parenting plan. Parents cannot modify the base schedule directly — only the professional can update the plan.
- Accepted swap requests are permanently locked — neither parent can retroactively undo an accepted swap.
- Rejected swap requests are logged as evidence and visible to the professional. Rejection does not change the calendar.
- A swap request can only exchange two custody days — multi-day or partial-day swap definitions TBD.
- One-off events do not change custody assignments — they are informational overlays on the calendar.
- The professional's view of the calendar is read-only in Phase 1.
- Expired swap requests are treated as declined for evidence logging purposes.

## 11. Dependencies

- **Parenting Plan Visualization:** The plan must be finalized and shared by the professional before the calendar is populated.
- **Parent Invite & Access:** Parents must have accepted their invite before they can access the calendar.
- **Case Management:** Calendar activity is scoped to a case.
- **AI-Guided Messaging (Phase 2):** Swap rejections or calendar disputes may trigger messaging coaching prompts.
- **Communication Pattern Analysis:** Scheduling-related behavior (swap rejection patterns, late requests) may be inputs to behavioral analysis.
- **Convex:** Calendar data, swap request state, and activity logs.

## 12. Data / Inputs / Outputs

**Inputs:**
- Parenting plan custody schedule (auto-populated from plan)
- One-off event details (parent input)
- Swap request: original day, proposed swap day, optional message (parent input)
- Swap response: accept or decline (co-parent input)

**Data stored:**
- Calendar event records: event ID, case ID, event type (custody block / one-off event), creator ID, date/time, details, status
- Swap request records: request ID, case ID, requesting parent ID, original day, proposed day, message, status (pending / accepted / declined / expired), response timestamp, responding parent ID
- Activity log: all calendar events, swap requests, and outcomes with timestamps

**Outputs:**
- Calendar view for parents and professional
- In-app notifications (swap received, swap accepted/declined, swap expiry reminder)
- Locked calendar entries on accepted swaps
- Case activity log entries
- Swap history report for professional

**Key states:**
- Swap request: `pending` → `accepted` / `declined` / `expired`
- Calendar day: `scheduled` → `locked` (after accepted swap)

## 13. UX / Design Notes

- The calendar is the daily-use interface for parents — it must be simple, clear, and mobile-first (parents will check it on their phones).
- Custody ownership should be immediately obvious: distinct colors per parent (e.g., blue for Parent 1, orange for Parent 2), not just labels.
- Swap request status should be visually clear on the affected calendar days — a badge or overlay distinguishing "pending swap" vs "accepted/locked swap" vs "declined swap."
- Swap request creation should be a simple, guided flow — parents may not be technically sophisticated.
- The professional's calendar view should look similar to the parent view but with read-only controls and an additional "Swap History" or activity panel.
- Meeting transcript confirmed that swap requests need confirmation from the other parent and that the calendar locks once accepted — this is a core UX requirement, not just a business rule.
- Screen Mapping PNG contains calendar screens; design team should confirm exact mobile and web layout specs.

## 14. Edge Cases and Exceptions

- Parenting plan is updated by the professional after the calendar is already populated: the calendar must reflect plan updates. Behavior for already-locked swaps when the underlying plan changes is not defined — flagged as open question.
- One parent does not respond to a swap request before expiry: request expires, calendar unchanged, logged as "expired."
- Both parents attempt to send a swap request for the same day simultaneously: race condition handling not defined — likely first-submitted takes priority, second gets an error.
- Parent does not have the app active when a swap notification is sent: notification must be stored and surfaced when parent next opens the app (standard notification queue behavior).
- Case is archived while parents have active swap requests: behavior not defined — flagged as open question.

## 15. Non-Functional Considerations

- **Mobile-first:** Parents interact with the calendar primarily on mobile. Responsive design or a native-feeling PWA/app experience is expected.
- **Real-time updates:** Both parents should see calendar changes (accepted swaps, new events) in near real-time to avoid coordination confusion.
- **Auditability:** Swap history is evidence-grade data. Entries must be immutable once created.
- **Notifications:** Swap request notifications must be delivered reliably. A missed notification could have real-world consequences (wrong parent picks up a child).
- **Accessibility:** Calendar interface must be navigable for users with accessibility needs.

## 16. Open Questions / Assumptions

- **Swap request expiry duration:** Not defined. Assumption: 48 hours (allowing co-parents a reasonable window to respond).
- **Multi-day swaps:** Whether a parent can request to swap multiple consecutive days at once is not defined. Assumption: single-day swaps only in Phase 1.
- **Plan updates affecting existing calendar:** How base schedule changes propagate when the professional updates the parenting plan is not defined. This is a significant edge case.
- **External calendar sync:** Google Calendar / Apple Calendar integration is not confirmed for Phase 1. Flagged as a likely Phase 2 request.
- **One-off event editing/deletion:** Whether parents can edit or delete one-off events they created is not defined. Assumption: creator can edit/delete before the event date.
- **Partial-day custody blocks:** Whether the plan can define half-day or hourly custody blocks (relevant for some parenting models) is not addressed.
- **Archived case calendar access:** Whether parents can view the calendar of an archived case is not defined.

## 17. Source Summary

- **Product Discovery Report.docx:** Phase 2 originally, but meeting transcript clarified as Phase 1 mobile app feature. Described calendar as linked to parenting plan, with swap request functionality.
- **Meeting Transcript (April 9, 2026):** Key discussion on swap requests — must be confirmed by other parent, calendar locks once accepted. All rejections logged as evidence for professionals. Professional receives all scheduling logs. One-off events created by parents. Swap confirmation workflow explicitly described. Custody schedule derived from finalized plan.
- **Screen Mapping.png:** Contains calendar screens; not fully analyzed.
- **Confidence:** High for core calendar model, swap workflow, and evidence logging. Medium for notification specifics and UX layout. Low for plan update propagation, multi-day swap rules, and archived case behavior.
