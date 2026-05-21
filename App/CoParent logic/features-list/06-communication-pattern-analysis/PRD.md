# PRD: Communication Pattern Analysis

## 1. Feature Name
Communication Pattern Analysis

## 2. Feature Summary
An AI-powered tool that ingests co-parent message exchanges from multiple sources, analyzes them for behavioral patterns, and produces court-defensible behavioral reports with anchored citations. The tool identifies recurring behaviors such as child triangulation, gatekeeping, unilateral decisions, and coercive control, tags them with a standardized behavioral taxonomy, tracks patterns longitudinally, and surfaces high-severity alerts for escalating or safety-related behaviors.

## 3. Product Context
Communication Pattern Analysis is the second primary AI value proposition of CoParent Logic, alongside PlanGuard™. Where PlanGuard addresses written plan documents, this feature addresses the lived reality of co-parent communication — the messages that become evidence in custody disputes.

- **User journey position:** Accessed from within a case. A professional uploads or imports message exports from co-parent communication platforms, then views and works with the AI analysis results.
- **Product goals supported:** Objective, bias-resistant behavioral documentation; court-defensible evidence preparation; professional efficiency (replaces hours of manual message review); differentiation from competitors who offer record storage but no analysis.
- **Interacts with:** Case Management (all analyses are scoped to a case), User Authentication & Role Management (professionals only), Admin Portal (usage analytics and audit), AI-Guided Messaging (Phase 2 uses behavioral insights to coach parent messaging), Parenting Plan Analyzer (behavioral patterns may inform plan risk areas).
- **Shared constraints:** ZDR policy applies — message content must not be retained beyond analysis. All citations must be anchored to specific message IDs and timestamps for legal defensibility. Analysis must be objective and bias-resistant.

## 4. Problem Statement
Family law professionals spend significant time manually reviewing hundreds or thousands of co-parent messages to identify behavioral patterns relevant to a case. This is time-consuming, inconsistent, and subjective. Courts require objective, citable evidence. Professionals need a tool that automates behavioral pattern identification across large message volumes, provides court-credible citations, and surfaces escalation trends before they reach crisis level.

## 5. Goals

**User goals:**
- Professionals can upload message exports and receive a structured behavioral analysis in minutes rather than hours.
- Analysis results are citable, traceable to specific messages, and suitable for use in legal documents.
- Professionals can track how behavior patterns evolve over time to support case narratives.

**Business goals:**
- Demonstrate measurable time savings for professionals (key adoption driver).
- Produce analysis that is meaningfully differentiated from competitor platforms (OurFamilyWizard, TalkingParents) which offer records but no behavioral analysis.
- Support professional referral and growth through credible, court-tested outputs.

**Operational goals:**
- High-severity alerts allow professionals to act quickly on safety concerns.
- Longitudinal tracking supports ongoing case management, not just point-in-time review.

## 6. Users / Personas

**Family Law Professional**
- Why they use this feature: To analyze co-parent communication behavior without manually reading every message. To produce objective, citable behavioral reports for case proceedings.
- What they need: Easy upload of message exports from multiple sources, a clear behavioral summary, drill-down to specific cited messages, and an exportable report.
- Role-specific behavior: Full access. The professional interprets and acts on the analysis. They are responsible for the legal use of the output.

**Parent (indirect)**
- Parents do not access Communication Pattern Analysis directly in Phase 1. However, in Phase 2, behavioral insights from this feature may inform the AI-guided messaging coaching shown to parents.

## 7. Feature Scope

**In scope:**
- Multi-source message ingestion: OurFamilyWizard (OFW) exports, SMS exports, email exports, TalkingParents exports
- Manual upload of message export files (Phase 1 — no direct API integration with messaging platforms)
- AI behavioral pattern analysis using a taxonomy of 10–15 core behavioral categories
- Behavioral tagging at the message or conversation level
- Anchored citations: each identified pattern instance linked to specific message ID and timestamp
- Longitudinal trend tracking: pattern frequency over time (escalation / improvement visualization)
- High-severity alerts for child safety-related or escalating behavior patterns
- Aggregate behavioral summary report per case
- Export of behavioral report (format TBD)
- All analysis scoped to and persisted within a case

**Out of scope:**
- Direct API integration with OFW, TalkingParents, or other platforms (Phase 2)
- Voice/audio message analysis (not in scope per discovery report)
- Real-time message analysis (Phase 1 is batch upload only)
- Parent-facing behavioral report access (professional-only in Phase 1)
- Financial behavior analysis from messages (not in scope)
- Automated court filing or report submission

## 8. Functional Requirements

1. From within a case, a professional can initiate a new communication analysis.
2. The professional uploads one or more message export files. Supported formats:
   - OurFamilyWizard (OFW) export format
   - TalkingParents export format
   - SMS export (common formats TBD — e.g., Google Messages backup, iOS backup extract)
   - Email export (format TBD)
3. Multiple files from different sources can be uploaded and combined into a single analysis run.
4. The system processes uploaded files to extract: sender identity, recipient identity, message text, timestamp, and message ID (where available).
5. The AI (Azure OpenAI GPT-5) analyzes extracted messages and:
   - Tags each relevant message or conversation segment with one or more behavioral pattern labels from the platform's taxonomy (10–15 core patterns)
   - Generates a confidence level per tag (if supported by the model)
   - Produces anchored citations: behavioral pattern instances are linked to specific message IDs and timestamps
6. The behavioral taxonomy includes (but is not limited to):
   - Child triangulation
   - Gatekeeping
   - Unilateral decision-making
   - Coercive control language
   - Parental alienation indicators
   - Non-compliance with court orders
   - Escalation / threatening language
   - Inappropriate communication timing
   - Financial coercion
   - (Additional categories defined by product/AI team)
7. The analysis results view shows:
   - Summary: total messages analyzed, date range covered, behavioral patterns detected (frequency per pattern)
   - Longitudinal trend chart: pattern frequency over time (by week or month)
   - Per-pattern detail: list of cited message instances with the message text excerpt, timestamp, source, and sender
   - High-severity alert section: flagged messages meeting alert criteria (child safety mentions, explicit threats, escalation indicators)
8. Professionals can navigate to any cited message instance to view the full message context.
9. High-severity alerts are visually prominent and may trigger in-platform notifications when detected.
10. The professional can export a behavioral report containing the summary, pattern analysis, and citations.
11. Multiple analysis runs can be performed on the same case as new message exports become available. Each run is timestamped and stored.
12. Longitudinal tracking compiles results across multiple analysis runs over time to show behavioral trends at the case level.
13. All analysis runs, uploads, and export actions are logged per case.

## 9. Workflow / User Journey

**Initial analysis:**
1. Professional opens a case and navigates to Communication Analysis.
2. Selects "New Analysis."
3. Selects source type(s) (OFW, TalkingParents, SMS, email).
4. Uploads file(s).
5. System validates file format and extracts message data (loading state).
6. Professional clicks "Run Analysis."
7. AI analysis runs (loading state — may take time for large message volumes).
8. Results page shows:
   - Summary statistics
   - Behavioral pattern breakdown with frequency counts
   - Longitudinal trend chart
   - Cited message instances per pattern
   - High-severity alerts (if any)
9. Professional reviews results, drills into citations as needed.
10. Professional exports report.

**Subsequent analysis runs (as new messages become available):**
1. Professional opens the case's Communication Analysis section.
2. Sees history of prior analysis runs.
3. Uploads new message export(s).
4. Runs new analysis.
5. Results are appended to the longitudinal trend view.

**Failure paths:**
- Unrecognized file format: display error with list of supported formats.
- File too large or message volume too high: display warning with options to split upload.
- AI analysis fails: display error with retry option; original file preserved.
- No behavioral patterns detected (all clean communication): display result with "No significant patterns detected" and summary stats.

## 10. Business Rules

- All behavioral tags must be anchored to specific messages — unanchored summary conclusions are not acceptable for legal use.
- The analysis is advisory — the professional is responsible for interpreting and acting on results.
- Message content is processed in accordance with ZDR policy — the AI model must not retain input message content beyond the analysis transaction.
- High-severity alerts are defined by the product/AI team's taxonomy and are not configurable by the professional in Phase 1.
- Analysis outputs are scoped to the case and visible only to the professional who owns the case (and CoParent Logic Admins for audit).
- Pattern taxonomy is defined and maintained by the CoParent Logic team — it is not customizable by professionals in Phase 1.

## 11. Dependencies

- **Case Management:** All analyses are attached to and scoped within a case.
- **Azure OpenAI GPT-5:** AI model that performs behavioral pattern identification and tagging.
- **Convex:** Stores analysis records, citation data, and activity logs.
- **User Authentication & Role Management:** Only verified, subscribed professionals can run analyses.
- **Admin Portal:** Admin can view analysis activity for audit purposes.
- **AI-Guided Messaging (Phase 2):** Behavioral insights from this feature will inform the coaching suggestions shown to parents in their messaging interface.

## 12. Data / Inputs / Outputs

**Inputs:**
- Uploaded message export files (OFW, TalkingParents, SMS, email formats)
- Case and professional context (passed to AI as metadata for analysis framing)

**Data stored:**
- Analysis run record: analysis ID, case ID, professional ID, source files (references), run timestamp, status
- Message records (for citation purposes): message ID (from source), sender, timestamp, text excerpt, behavioral tags
- Behavioral summary: pattern frequencies, alert flags, longitudinal data points
- Note: Full message content storage duration subject to ZDR policy — may be processed and discarded, with only citation excerpts retained

**Outputs:**
- Behavioral summary (pattern counts, date range, source coverage)
- Per-pattern citation list (message excerpts with metadata)
- Longitudinal trend data
- High-severity alert list
- Exportable behavioral report

**Key states:**
- Analysis run: `uploading` → `processing` → `complete` / `failed`

## 13. UX / Design Notes

- The results view is the most complex UI in this feature. It needs to balance a high-level summary (for quick professional orientation) with deep drill-down capability (for evidence preparation).
- Longitudinal trend chart should be simple and readable — line or bar chart showing pattern frequency over time, with the ability to filter by pattern category.
- Citation view should show enough message context (surrounding messages) to be meaningful without showing entire conversation threads — recommended: 2–3 message context window per citation.
- High-severity alerts should appear at the top of the results page with a visually distinct style (red banner or alert card).
- The analysis run history should be accessible in a sidebar or tab — professionals need to compare runs over time.
- Screen Mapping PNG contains communication analysis screens; design team should be consulted for layout specs.

## 14. Edge Cases and Exceptions

- Messages from both co-parents are included in the export — system must correctly distinguish sender identity for pattern attribution.
- Message export contains duplicate entries (same message appears in two exports): system should deduplicate based on message ID and timestamp.
- Co-parent uses coded or euphemistic language to avoid detection: AI tagging accuracy depends on model capability; system should not over-represent certainty in such cases.
- Export file contains data from multiple cases mixed together: no automated case detection — professional is responsible for uploading the correct export file.
- Very large message volume (thousands of messages): performance and timeout handling required; chunked processing may be needed.
- Messages in a language other than English: behavior not defined; assumed English only for MVP.

## 15. Non-Functional Considerations

- **Security:** Message content is highly sensitive — contains personal communications, potentially including child safety information. Strict encryption at rest and in transit. ZDR agreements honored.
- **Auditability:** Every analysis run must be logged. Citations are the primary audit artifact — they must be immutable once generated.
- **Performance:** AI analysis of large message sets may take significant time. Async processing with a progress indicator is likely required.
- **Legal defensibility:** Citation anchoring to message IDs and timestamps is a core non-functional requirement — without this, the analysis cannot be used in court proceedings.
- **Bias and accuracy:** The AI model must be carefully prompted to minimize bias in pattern attribution. Misattributing a behavioral pattern could have serious legal consequences for a co-parent.

## 16. Open Questions / Assumptions

- **Full behavioral taxonomy:** The discovery report mentions 10–15 core patterns but lists only a subset. Complete taxonomy definition is needed before AI prompts can be finalized.
- **Message export formats:** Specific file formats for OFW, TalkingParents, SMS, and email exports are not defined in source materials. Technical investigation needed.
- **ZDR compliance for message content:** Whether full message text is stored after analysis or only citation excerpts is not defined. This is a critical compliance question.
- **High-severity alert definition:** Precise criteria for what triggers a high-severity alert (child safety, explicit threats, etc.) are not defined. Needs product/legal input.
- **Confidence thresholds:** Whether low-confidence behavioral tags are shown or filtered out is not defined.
- **Multi-language support:** Assumed English only for MVP.
- **Report export format:** PDF, DOCX, or structured data — not specified.

## 17. Source Summary

- **Product Discovery Report.docx:** Detailed description of communication pattern analysis — multi-source ingestion (OFW, SMS, email, TalkingParents), AI behavioral taxonomy (10–15 patterns), anchored citations with message IDs and timestamps, longitudinal trend tracking, high-severity alerts for child safety and escalation. Listed as an MVP epic.
- **Meeting Transcript (April 9, 2026):** Phase 1 relies on manual file uploads (no direct platform integration). Behavioral insights are intended to support professionals, not parents, in Phase 1. Discussed that professionals need objective, court-defensible outputs.
- **Screen Mapping.png:** Contains communication analysis screens; not fully analyzed.
- **Confidence:** High for feature concept, behavioral taxonomy direction, and citation requirements. Medium for specific UI and report format. Low for precise taxonomy, export file formats, ZDR compliance details, and alert thresholds.
