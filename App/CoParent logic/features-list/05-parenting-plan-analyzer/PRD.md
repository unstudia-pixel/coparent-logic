# PRD: Parenting Plan Analyzer (PlanGuard™)

## 1. Feature Name
Parenting Plan Analyzer — PlanGuard™

## 2. Feature Summary
An AI-powered tool that allows Family Law Professionals to upload, paste, or input custody/parenting plan documents and receive an automated risk analysis identifying ambiguous, vague, or conflictual language — referred to as "litigation bait." The tool scores the plan for conflict risk, flags problematic clauses, suggests conflict-resistant rewrites aligned with AFCC (Association of Family and Conciliation Courts) best practices, and supports iterative drafting toward a higher-quality final document. Version history is maintained against an original draft baseline.

## 3. Product Context
PlanGuard™ is one of the two primary AI-powered value propositions of CoParent Logic (alongside Communication Pattern Analysis). It directly addresses a core pain point for family law professionals: parenting plans with vague language that generate future litigation.

- **User journey position:** Accessed from within a case. A professional opens a case, navigates to the PlanGuard tool, and begins analyzing or drafting a plan for that specific co-parent family.
- **Product goals supported:** Conflict reduction, professional efficiency, court-defensible documentation, and differentiation from competitors (who offer messaging records but no plan analysis).
- **Interacts with:** Case Management (plans are scoped to a case), User Authentication & Role Management (only verified professionals can access), Admin Portal (plan analysis activity may be visible in usage analytics).
- **Shared constraints:** All document content is processed in alignment with the platform's Zero Data Retention (ZDR) policy. Plan text may contain sensitive PII (parent names, children names, addresses) — handling must comply with data privacy requirements. OCR is used for PDF/image inputs via Azure Document Intelligence.

## 4. Problem Statement
Family law professionals routinely draft or review parenting plans containing vague, ambiguous language (e.g., "as agreed," "reasonable notice," "whenever possible"). These terms become litigation triggers when co-parents disagree on interpretation. Professionals need an efficient way to identify problematic clauses and replace them with clear, specific, AFCC-aligned language — but manual review is time-consuming and inconsistent.

## 5. Goals

**User goals:**
- Professionals can quickly identify all high-risk clauses in a plan without reading every word manually.
- Professionals can accept, modify, or reject AI-suggested rewrites while maintaining control of the final document.
- Professionals can track how the conflict-reduction score improves across drafting iterations.

**Business goals:**
- PlanGuard™ is a primary differentiator — it justifies the professional subscription.
- Reduce the likelihood that plans drafted using the platform result in future litigation (supports testimonials and case studies).
- Track conflict-reduction score improvement as a platform success metric.

**Operational goals:**
- Provide a structured, auditable drafting history for each plan.
- Support downstream court submission by producing a clean, exportable output.

## 6. Users / Personas

**Family Law Professional**
- Why they use this feature: To improve the quality of custody/parenting plans, reduce future conflict, and produce more defensible documents.
- What they need: Fast risk identification, clear explanations of why a clause is flagged, high-quality rewrite suggestions, and easy iterative editing.
- Role-specific behavior: Full access to all PlanGuard functions. The professional is the author — AI assists but does not override.

**Parent**
- Parents do not use PlanGuard directly in Phase 1. They receive the finalized, locked plan via the Parenting Plan Visualization feature.

## 7. Feature Scope

**In scope:**
- Document input: PDF upload, DOCX upload, plain text paste
- OCR processing for PDF and image-based inputs via Azure Document Intelligence
- AI risk scoring of the entire plan (0–100 conflict-reduction score)
- Domain-specific sub-scores: scheduling, medical decisions, expenses/financial
- Clause-level flagging: identification of specific problematic phrases or clauses
- Explanation of why each clause is flagged (litigation risk rationale)
- AI-generated conflict-resistant rewrite suggestions per flagged clause
- Professional ability to accept, edit, or reject suggestions
- Iterative drafting: each revision generates a new score
- Version history with the original uploaded/pasted draft as the baseline
- Final plan export (format TBD — likely PDF or DOCX)
- All analysis results scoped to the parent case

**Out of scope:**
- AI-generated plan creation from scratch (Phase 2 — Parenting Plan Generator)
- Court-ready formatted PDF report (Post-MVP per discovery report)
- Financial settlement analysis (explicitly out of scope)
- Jurisdiction-specific legal validation (plan checks are AFCC-aligned but not jurisdiction-specific legal advice)
- Multi-plan comparison across cases
- Parent-facing plan editing

## 8. Functional Requirements

1. From within a case, a professional can initiate a new plan analysis.
2. The professional can input the plan via: PDF upload, DOCX upload, or pasting plain text directly.
3. PDFs and DOCX files are processed via OCR (Azure Document Intelligence) to extract text.
4. The system sends extracted plan text to the AI (Azure OpenAI GPT-5) for analysis.
5. The AI returns:
   - An overall conflict-reduction score (0–100) for the plan
   - Sub-scores for: scheduling, medical decisions, expenses/finances
   - A list of flagged clauses, each with:
     - The original clause text (with location reference if possible)
     - A risk category label (e.g., "vague scheduling," "undefined terms," "coercive control language")
     - A plain-language explanation of why the clause is problematic
     - One or more conflict-resistant rewrite suggestions
6. Flagged clauses are displayed in context (ideally alongside the original document text, or as a list with references).
7. For each flagged clause, the professional can:
   - Accept a suggested rewrite (replaces or marks the original clause)
   - Edit the suggested rewrite before accepting
   - Reject the suggestion (marks the clause as reviewed but unchanged)
   - Add a note to the clause
8. After reviewing and accepting/editing clauses, the professional can re-run the analysis to generate an updated conflict-reduction score for the revised plan.
9. Version history is maintained: the original uploaded/pasted document is always preserved as the baseline. Each re-analysis creates a new version with a timestamp and score.
10. The professional can view and navigate between versions.
11. The professional can export the current or any prior version of the plan.
12. All analysis activity (plan uploads, analyses, version creation) is logged per case.
13. If a document cannot be parsed (corrupted file, unreadable scan), the system displays an error with guidance to use a different input method or re-upload a clearer file.
14. Partial analysis results (if AI returns incomplete data) should be handled gracefully — show what was returned and indicate any sections that could not be analyzed.

## 9. Workflow / User Journey

**New plan analysis:**
1. Professional opens a case and navigates to PlanGuard™.
2. Selects input method: upload PDF, upload DOCX, or paste text.
3. Uploads file or pastes content.
4. If file-based: OCR extraction runs (loading state shown).
5. Extracted text confirmed / text shown to professional (optional review step — TBD).
6. Professional clicks "Analyze Plan."
7. AI analysis runs (loading state shown).
8. Results page shows:
   - Overall conflict-reduction score
   - Sub-scores (scheduling, medical, expenses)
   - List of flagged clauses with risk labels, explanations, and rewrite suggestions
9. Professional reviews each flagged clause and accepts, edits, or rejects suggestions.
10. Professional clicks "Re-analyze" to generate an updated score for the revised plan.
11. New version is saved with the updated score.
12. Professional continues iterating until satisfied.
13. Professional exports the final plan.

**Failure paths:**
- OCR fails (unreadable scan): error message, prompt to re-upload or paste text.
- AI returns no flags (clean plan): display score of 100 (or near 100) with a positive confirmation message.
- AI returns an error: display a user-friendly error message; allow retry.
- Very large document causes timeout: TBD — may need chunked processing.

## 10. Business Rules

- The original uploaded document is always preserved. Edits do not modify the original — they create new versions.
- AI suggestions are advisory only. The professional has full authority to accept, modify, or reject any suggestion.
- Conflict-reduction score is recalculated only when the professional explicitly requests a re-analysis — it does not update automatically as edits are made.
- Sub-scores are domain-specific (scheduling, medical, expenses) — these align with AFCC best practice domains.
- All analyses are scoped to the case — a plan analyzed in one case is not visible in another.
- "Litigation bait" phrases include (but are not limited to): "reasonable," "as agreed," "when possible," "appropriate notice," "at the professional's discretion" — specific taxonomy owned by the AI model and AFCC guidelines.

## 11. Dependencies

- **Case Management:** Plans are attached to and scoped within a case. A case must exist before a plan can be analyzed.
- **Azure OpenAI GPT-5:** The AI model that performs clause-level risk analysis and generates rewrite suggestions.
- **Azure Document Intelligence (OCR):** Processes uploaded PDFs and DOCX files to extract plan text.
- **Convex:** Stores plan versions, analysis results, and activity logs.
- **User Authentication & Role Management:** Only verified, subscribed professionals can access PlanGuard.

## 12. Data / Inputs / Outputs

**Inputs:**
- Uploaded file (PDF or DOCX) or pasted plain text
- Re-analysis trigger after professional edits

**Data stored:**
- Plan record: plan ID, case ID, professional ID, original document (file reference), created at
- Version record: version ID, plan ID, version number, text content at that version, conflict-reduction score, sub-scores, created at
- Flagged clause records: clause text, risk category, explanation, suggestions, professional action (accepted / edited / rejected), final text

**Outputs:**
- Conflict-reduction score (overall and sub-scores)
- Flagged clause list with risk labels, explanations, and rewrite suggestions
- Iterative versions of the plan
- Exported final plan document

**Key states:**
- Plan: `uploading` → `processing` → `analyzed` → `draft_in_progress` → `finalized`
- Clause: `flagged` → `accepted` / `edited` / `rejected`

## 13. UX / Design Notes

- The analysis results page is the core UI of this feature. It must balance showing the full plan context while calling attention to flagged clauses — likely a split view or annotated document view.
- Conflict-reduction score should be visually prominent — a score gauge or progress indicator helps professionals track improvement at a glance.
- Sub-scores (scheduling, medical, expenses) should be clearly labeled and easy to interpret. A color-coded breakdown (red/yellow/green) is a likely design pattern.
- Rewrite suggestions should be shown inline next to flagged clauses — not in a separate panel that requires toggling.
- The version history should be accessible but not dominate the UI — a sidebar or dropdown history selector is appropriate.
- The prototype shown in the April 9 meeting included a "plan analysis" section in the dashboard — but detailed PlanGuard UI specifics were not discussed.
- Screen Mapping PNG contains PlanGuard screens; design team should be consulted for component-level layout.

## 14. Edge Cases and Exceptions

- Plan contains no flagged clauses (already high quality): display congratulatory message and a high score. Do not artificially generate flags.
- Plan is in a non-English language: behavior not defined in source materials. Assumption: English only for MVP.
- Very long plan (e.g., 50+ pages): AI processing may require chunking or pagination of results. Timeout and performance handling TBD.
- Professional uploads the same plan twice (exact duplicate): no defined behavior — assumption: treated as a new version.
- Plan contains highly sensitive PII (SSNs, home addresses): system should not store this data longer than necessary per ZDR policy; AI processing must not retain input data.
- Rewrite suggestion is legally incorrect for a specific jurisdiction: system includes a disclaimer that AI suggestions are not legal advice and should be reviewed by the professional.

## 15. Non-Functional Considerations

- **Performance:** OCR processing and AI analysis may take several seconds to minutes for large documents. Loading states and progress indicators are required.
- **Security:** Plan documents contain sensitive family and legal PII. Storage is encrypted at rest. ZDR agreements must be honored — data should not be retained by the AI model beyond the transaction.
- **Auditability:** All analysis runs, version creations, and clause-level decisions must be logged.
- **Reliability:** AI analysis failure should not corrupt the plan record — originals must always be preserved regardless of downstream processing errors.
- **Accessibility:** The flagged clause review interface must be navigable via keyboard and screen reader.

## 16. Open Questions / Assumptions

- **Clause reference format:** Whether flagged clauses are shown in context within a rendered document view or as a standalone list with text excerpts is not fully defined. Assumption: list with excerpts in Phase 1; in-document annotation as a Phase 2 enhancement.
- **Export format:** The final plan export format (PDF, DOCX, or both) is not defined in source materials.
- **OCR quality threshold:** What happens if OCR confidence is low (partially scanned or handwritten document) is not defined.
- **AI disclaimer:** Whether the UI includes a legal disclaimer on AI-generated rewrites is assumed to be required but not confirmed in source materials.
- **Language support:** Assumed English only for MVP.
- **Chunking strategy for large documents:** Not addressed; may be a significant technical decision.

## 17. Source Summary

- **Product Discovery Report.docx:** Described PlanGuard™ in detail — AI risk scoring, AFCC alignment, litigation bait identification, iterative drafting, version history, 0–100 conflict-reduction score with sub-scores (scheduling, medical, expenses), PDF/DOCX/text input, OCR via Azure Document Intelligence, Azure OpenAI GPT-5.
- **Meeting Transcript (April 9, 2026):** Confirmed PlanGuard is a Phase 1 feature. Manual uploads are the input method in Phase 1 (no auto-import from court systems). An AI-powered plan generator (creating plans from scratch) is Phase 2.
- **Screen Mapping.png:** Contains PlanGuard screens; not fully analyzed in text.
- **Confidence:** High for AI analysis capabilities, scoring model, and input methods. Medium for clause-level UX and export format. Low for large document handling, language support, and specific AI prompt/taxonomy details.
