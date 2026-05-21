# PRD: AI-Guided Messaging

## 1. Feature Name
AI-Guided Messaging

## 2. Feature Summary
A parent-facing messaging tool within the platform that allows co-parents to send messages to each other about their children. Before sending, parents are offered an AI-generated rewrite suggestion that rephrases their message in a constructive, child-focused tone. The professional selects the message category (e.g., "school," "medical"), rather than the AI auto-detecting intent. Parents can choose to send the AI-suggested version, edit it, or send their original message unchanged. All messages are logged and behavioral patterns are available for professional review.

## 3. Product Context
AI-Guided Messaging is the parent-facing communication tool that sits within the broader platform's goal of reducing co-parent conflict. It connects directly to the Communication Pattern Analysis feature — messages sent through this tool are the future source of behavioral analysis data — and reflects the Phase 2 ambition to move from reactive analysis to proactive conflict prevention.

- **User journey position:** Accessible to parents after they accept their invite. Parents use this as their primary channel for co-parent communication on the platform.
- **Product goals supported:** Conflict reduction through proactive messaging guidance, platform engagement for parents, and building the behavioral data set for professional analysis.
- **Interacts with:** Parent Invite & Access (parents must be onboarded first), Case Management (messaging is scoped to a case), Communication Pattern Analysis (messages sent here become the input data for analysis), Calendar & Schedule Management (scheduling disputes may originate in messaging context), Parenting Plan Visualization (plan context may inform message categorization).
- **Shared constraints:** Messaging is between the two co-parents on a case. The professional has read-only visibility. All messages are stored as an immutable record. AI rewrites are suggestions only — parents always retain the choice to send their original message.

## 4. Problem Statement
Co-parent communication frequently escalates because messages are written in the heat of emotion — accusatory, ambiguous, or unilaterally directive. If parents can be nudged toward more constructive phrasing before sending, conflicts can be de-escalated before they start. Professionals also benefit from having an in-platform message record rather than relying on external platform exports.

## 5. Goals

**User goals:**
- Parents can communicate with their co-parent about their children within a structured, supportive tool.
- Parents are offered a constructive rewrite option before sending, without being forced to use it.
- Parents feel supported, not judged — the AI coaching is assistive, not punitive.

**Business goals:**
- Drive daily active engagement from parents (messaging is the highest-frequency interaction after the calendar).
- Create a native message record that reduces reliance on external tools (OFW, TalkingParents).
- Accumulate behavioral data in-platform that reduces the need for manual export-and-analyze workflows.

**Operational goals:**
- Professional has full visibility into parent messaging for case management.
- Message record is court-defensible and immutable.

## 6. Users / Personas

**Parent (sender)**
- Why they use this feature: To communicate with their co-parent about their children (scheduling, school, medical, etc.) within a safe, structured tool.
- What they need: A simple messaging interface, helpful AI suggestions that don't feel robotic, and the freedom to send their own words if they prefer.
- Role-specific behavior: Can compose and send messages. Can accept, edit, or reject AI rewrite suggestions. Cannot see the other parent's composed-but-not-sent drafts.

**Parent (recipient)**
- Why they use this feature: To receive messages from their co-parent and respond.
- What they need: Clear message delivery with notification, and access to the same AI-guided reply assistance when responding.
- Role-specific behavior: Receives messages. Can reply. Their replies are also subject to the same AI guidance flow.

**Family Law Professional**
- Why they use this feature: To monitor co-parent communication, review messaging patterns, and use message history as case evidence.
- What they need: Read-only access to the full message thread for a case. Visibility into which messages were AI-rewritten vs. sent as-is.
- Role-specific behavior: Read-only. Cannot send messages or intervene in the co-parent messaging thread.
- **Category selection role:** In Phase 1, the professional selects the message categories (e.g., "school," "medical," "legal") rather than the AI auto-detecting intent. This is a deliberate design choice to maintain professional control over how messages are classified.

## 7. Feature Scope

**In scope:**
- In-platform messaging thread between the two co-parents on a case
- Message composition interface for parents
- Professional-selected message categories (set at the case level or message level — TBD)
- AI rewrite suggestion: one primary suggestion per message, with a "Regenerate" option to get an alternative
- Parent options: send AI suggestion, edit AI suggestion then send, send original message
- Parent tracking of their own coaching acceptance rate (how often they send the AI version vs. their original)
- All messages stored as an immutable, timestamped record
- Professional read-only view of the full message thread
- Professional visibility of message category and whether AI rewrite was used
- In-app notifications for new messages received

**Out of scope:**
- Real-time live chat (assumption: asynchronous messaging in Phase 1)
- Group messaging or multi-party threads
- File/image attachments in messages (not confirmed in source materials for Phase 1)
- Automated behavioral coaching alerts during message composition (Phase 2 enhancement)
- Parent ability to delete or edit sent messages
- AI auto-detection of message intent/category (professional selects categories in Phase 1)
- End-to-end encryption beyond standard platform encryption (not specified)

## 8. Functional Requirements

1. Parents can access a messaging interface from the parent dashboard, scoped to their co-parent case.
2. The messaging thread shows all messages between the two parents in chronological order, with: sender name, message content, timestamp, message category label, and an indicator of whether the message was sent as written or via AI suggestion.
3. A parent can compose a new message by typing in the message input field.
4. Before sending, the parent can request an AI rewrite by clicking a "Suggest Rewrite" button (or the rewrite may be automatically offered — UX decision TBD).
5. The AI rewrite suggestion is displayed alongside the original message text. The suggestion rephrases the message in a constructive, child-focused, de-escalated tone.
6. The parent has three options:
   a. Send the AI suggestion as-is
   b. Edit the AI suggestion and then send
   c. Send their original message (without the AI suggestion)
7. A "Regenerate" button allows the parent to request an alternative AI suggestion if they don't like the first one.
8. In Phase 1, the professional selects the message category for the case or for specific message types. Categories include at minimum: school, medical, logistics, legal, financial (exact taxonomy TBD). The category is associated with the message when sent.
9. All sent messages are stored with: message ID, sender ID, recipient ID, case ID, timestamp, original message text, AI suggestion text (if generated), final sent text, category, and a flag indicating whether the AI version or original was sent.
10. The co-parent receives an in-app notification when a new message is received.
11. The professional can view the full message thread from the case detail page (read-only).
12. The professional can see: all messages, categories, whether AI rewrite was used, and the original vs. sent text (if different).
13. The parent can view their own coaching acceptance rate: a simple metric showing how often they chose to send the AI version vs. their original.
14. Messages are immutable once sent — no editing or deletion by parents.

## 9. Workflow / User Journey

**Parent sends a message:**
1. Parent opens the messaging section of the parent dashboard.
2. Sees the co-parent message thread.
3. Types a new message in the input field.
4. Clicks "Suggest Rewrite" (or rewrite appears automatically — TBD).
5. AI suggestion is displayed alongside the original.
6. Parent reviews:
   - Likes the suggestion → clicks "Send AI Version"
   - Wants to adjust → edits the suggestion → clicks "Send"
   - Prefers their original → clicks "Send Original"
7. Message is sent and appears in the thread for both parents.
8. Co-parent receives a notification.

**Parent receives and replies:**
1. Parent receives a notification of a new message.
2. Opens the messaging interface.
3. Reads the message.
4. Clicks "Reply."
5. Same composition + AI rewrite flow as above.

**Professional reviews message thread:**
1. Professional opens a case detail page.
2. Navigates to the messaging section.
3. Sees the full thread with all messages, categories, and AI rewrite indicators.
4. Can filter or search by category or date range (if filtering is implemented — not confirmed for Phase 1).

**Failure paths:**
- AI rewrite service is unavailable: parent can still send their original message without a suggestion. Display a brief, non-alarming notice ("Rewrite suggestions are temporarily unavailable").
- Message fails to deliver (network error): display a retry option. Message must not be silently lost.
- Co-parent has not accepted their invite yet: parent sees a placeholder state ("Waiting for [co-parent name] to join"). No messaging until both parents are onboarded.

## 10. Business Rules

- Messages cannot be edited or deleted after sending. The message record is immutable.
- The AI rewrite is always optional — parents are never forced to use it.
- The professional selects message categories in Phase 1 — parents do not categorize their own messages.
- A parent's coaching acceptance rate is visible only to themselves (and the professional — confirm whether professional sees per-parent rates).
- Messages are scoped to the case — there is no cross-case messaging.
- The professional receives all message logs and can see both the original and AI-suggested versions.
- Both parents must be onboarded (invite accepted) before messaging is active.

## 11. Dependencies

- **Parent Invite & Access:** Both parents must be onboarded before the messaging thread is active.
- **Case Management:** Messaging is scoped to a case.
- **Azure OpenAI GPT-5:** AI model that generates message rewrite suggestions.
- **Communication Pattern Analysis:** Messages sent via this tool become the native message record for the case — reducing the need for external platform exports in the long run.
- **Convex:** Message storage, thread management, notification triggers.
- **Calendar & Schedule Management:** Scheduling disputes may originate in the messaging context; these features exist in the same parent UX.

## 12. Data / Inputs / Outputs

**Inputs:**
- Parent message (free text)
- Category (set by professional at case or message level)
- Parent's choice: send AI version / edit AI version / send original

**Data stored:**
- Message record: message ID, case ID, sender ID, recipient ID, timestamp, original text, AI suggestion text (if generated), final sent text, category, ai_used flag (boolean)
- Coaching metrics: per-parent count of messages sent, count where AI version used, acceptance rate

**Outputs:**
- Sent message (visible to both parents and professional)
- In-app notification to recipient parent
- AI rewrite suggestion (transient, not stored after the message is sent — unless the AI suggestion text is retained for evidence purposes)
- Professional's message thread view
- Coaching acceptance rate metric for parent

**Key states:**
- Message: `composing` → `rewrite_suggested` → `sent`

## 13. UX / Design Notes

- The messaging interface should feel supportive, not clinical — the AI rewrite suggestion should be framed as helpful guidance ("Here's a calmer way to say this:"), not as a judgment.
- The three-option flow (send AI / edit AI / send original) must be clear and low-friction. A poor UX here will cause parents to default to "send original" every time, defeating the purpose.
- Displaying the original and AI suggestion side-by-side (or with a toggle) helps parents make an informed choice.
- The "Regenerate" option should be visually accessible but not dominant — it's a secondary action.
- The coaching acceptance rate display should be encouraging rather than shaming — e.g., "You've chosen the AI-suggested version in 7 out of 10 messages this month."
- Meeting transcript confirmed: Phase 1 uses a single top AI suggestion with a regenerate button. Real-time behavioral coaching (auto-triggering warnings while typing) is Phase 2.
- Screen Mapping PNG contains messaging screens for parents; design team should confirm layout and interaction patterns.

## 14. Edge Cases and Exceptions

- Parent sends a very short message (e.g., "OK") — AI rewrite may be identical or nonsensical. System should handle gracefully: if the rewrite adds no value, it may display a note ("Your message looks good as-is") rather than a forced suggestion.
- Parent sends a message containing sensitive PII (addresses, financial details) — system does not filter or block; professional is responsible for case guidance on appropriate message content.
- AI rewrite changes the meaning of the message — parents should be made aware that the suggestion is a rephrase, not a legal restatement. A disclaimer may be appropriate.
- Co-parent blocks the other (not a supported feature) — not addressed in source materials.
- Message thread grows very long (hundreds of messages over months) — pagination or lazy loading required.

## 15. Non-Functional Considerations

- **Privacy:** Messages between co-parents are highly sensitive. The message thread is shared only between the two co-parents and the professional. No other users can access it.
- **Immutability:** Sent messages must be stored in a way that prevents modification — they are potential legal evidence.
- **Reliability:** Message delivery must be reliable. A failed message send must surface to the sender (not silently fail).
- **Performance:** AI rewrite suggestions should be generated quickly — a multi-second wait before a parent can send a message will create friction. Acceptable latency TBD.
- **Auditability:** The complete message record (original, AI suggestion, sent version, category, metadata) must be preserved for the lifetime of the case.

## 16. Open Questions / Assumptions

- **Rewrite trigger:** Whether the AI rewrite is auto-suggested after the parent types (on a delay) or manually triggered by a button press is not confirmed. Meeting transcript implies a button ("Suggest Rewrite" pattern with single top suggestion + regenerate). Assumption: button-triggered in Phase 1.
- **Category assignment:** Whether the professional sets categories at the case level (all messages default to the same category), or selects per message type, or sets a taxonomy that the parent then applies per message, is not fully defined.
- **Professional access to original text:** Whether the professional can see both the parent's original message AND the AI suggestion alongside the sent version is not confirmed. Assumption: yes, for evidence purposes.
- **Coaching acceptance rate visibility:** Whether the professional sees each parent's acceptance rate or only aggregate metrics is not defined.
- **AI suggestion storage:** Whether the AI suggestion text is retained in the message record (for evidence showing what was suggested vs. what was sent) is not defined. Assumption: retained.
- **Attachments:** File/image attachments are assumed out of scope for Phase 1 but are a likely request.
- **Notification channel:** Whether message notifications are in-app only or also via email/push is not defined.

## 17. Source Summary

- **Product Discovery Report.docx:** Described AI-guided messaging as a parent feature with message rewrite suggestions. Referenced behavioral coaching as Phase 2.
- **Meeting Transcript (April 9, 2026):** Key design decisions confirmed — professional selects message categories rather than AI auto-detecting. Single top suggestion + regenerate button in Phase 1. Real-time behavioral coaching is Phase 2. Parents not forced to use rewrites. Parents can track their own coaching acceptance rate. Professional sees all message logs.
- **Screen Mapping.png:** Contains messaging screens; not fully analyzed.
- **Confidence:** High for rewrite flow, professional category selection, and immutable record model. Medium for exact UX interaction pattern and category taxonomy. Low for AI suggestion storage policy, notification channels, and coaching metric visibility scope.
