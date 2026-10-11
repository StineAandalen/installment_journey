# PRD: "Why we ask" explanations

| Title | Author | Status | Target |
| --- | --- | --- | --- |
| "Why we ask": plain-language notes on income and document questions | [Your name], Product Manager, Self-Service Portal | Draft | High-fidelity prototype |

**Context:** Riverty BU Collection, instalment journey pilot, Germany, one consumer segment. Persona: Mara, 38, shift worker, variable income, two open claims, on her phone with little time. All figures in the brief are fictional.

**What this PRD makes explicit:** The PRD treats legal-approved wording as a release gate, because a wrong statement about data use would damage the trust it is meant to build, and a vague brief would have left that risk to be discovered in week 3.

---

## 1. The big picture

### Vision

Every income and document question in the instalment journey tells Mara, in plain language on her phone, why it is asked, how the answer is used and who can see it, so she answers with confidence instead of leaving to search or calling an agent.

### Press release

**Riverty adds a short "why we ask" note to every income question in the instalment journey.** Until now, Mara, who has variable shift income, opened the portal after a reminder and met questions about her income and a request for documents with no explanation. She could not tell why the information was needed, how it would be used, or whether a wrong answer could make things worse, so she left to search online or phoned an agent. Today, 31% of people who start an instalment journey contact an agent within 7 days, and 28% of uploaded income documents need clarification or review.

Now each question carries a short note directly beneath it, visible without a tap: why Riverty asks, how the answer is used and who can see it. The wording is fixed, written in plain German and approved by legal and data protection before release. The notes explain the questions Riverty already asks and change nothing about what is collected. They give Mara one thing she was missing at the moment she used to leave: a reason to keep going.

### Success metrics

**Primary: Self-Service Rate with 60-day plan survival.** An instalment is created in the portal with no inbound agent contact in the 30 days before or after set-up, and the consumer misses no payment within 60 days.
- Baseline: not yet measured. It is computed from the first weeks of pilot instrumentation, and no numeric target is set until then.
- Decision rule: ship wider only if the pilot cohort beats the holdout cohort on this metric while the guardrail holds.
- Leading signals while the 60-day window runs: drop-off at the income and document questions (not yet measured), uploaded documents needing review (baseline 28%), starters contacting an agent within 7 days (baseline 31%).

**Guardrail: 60-day plan survival.** No more than 19% of new agreements miss a payment within 60 days (baseline 19%). If better understanding raises agreements but pushes this above 19%, pause and review.

---

## 2. The details

### User stories

**Story 1: Understand why**
As Mara, I want to see why each income question is asked next to the question, so that I can answer without leaving to search or calling an agent.

Acceptance criteria:
- Given the income screen on a 360 px wide phone, when it loads, then all three questions show their note without any tap or hover.
- Each note states why the question is asked and how the answer is used, in 40 words or fewer.
- No question in the prototype journey appears without a note.

**Story 2: Know who sees it**
As Mara, I want to know who can see what I enter, so that I am not afraid a wrong or shared answer could make things worse.

Acceptance criteria:
- Every note contains a "who can see it" statement taken from the confirmed-facts table, never written freely.
- No note uses a categorical promise (for example "never", "nobody", "guaranteed") unless legal has marked it verified.
- No note states or implies the outcome of her plan.

**Story 3: Understand why proof of income is required**
As Mara, when I am asked for proof of income, I want to be told why it is required and who can see it, so that I know it is a condition of the plan and can decide to find the document now.

Acceptance criteria:
- The proof-of-income question is labelled "(required)" and carries the same three-part note as the other questions, stating why proof is required.
- The note sits above the upload control, so she reads it before she starts hunting for a file.
- A one-line instruction under the note reads "Add your latest proof of income."
- Continue is blocked until a document is added. The error reads "Proof of income is required to continue. Choose a document to add."
- There is no skip option. If she selects "No income right now", the proof-of-income question is not shown and she continues without a document.

### Screens to build (exactly 3)

**Screen 1: Entry point, "Start instalment request"**
- Riverty header with logo placeholder.
- Page title: "Arrange an instalment plan".
- One-line context: "You received a payment reminder" with a synthetic case reference (for example DEMO-0001).
- Intro line: "We ask a few short questions. Under each one, we explain why."
- Primary button: "Continue".

**Screen 2: Feature core, "About your income"**
Up to three question blocks stacked on one screen (two when she selects "No income right now"). Each block has a label, a note and a control.
1. *Monthly net income:* euro amount field.
2. *Income pattern:* radio group with "Stays the same", "Changes (for example shift work)" and "No income right now".
3. *Proof of income (required):* instruction line "Add your latest proof of income", then a mock file picker (filename held in state only). Shown only when she has not selected "No income right now". There is no skip option.

Each note is a bordered panel directly under the label with a visible heading "Why we ask". Its body has three parts: why asked, how used, who can see it. A small "Draft wording" tag appears on every note in the prototype.

Other elements on this screen:
- inline error text under each field
- a "Back" link
- a "Continue" button

*Draft note pattern (English gloss; the German final copy is written in week 1):*
"We ask this to show you instalments that fit your income. We use your answer only for [use: CONFIRM WITH DATA PROTECTION]. [Roles: CONFIRM WITH DATA PROTECTION] can see it."

**Screen 3: Success and confirmation, "We have your details"**
- Check icon and title: "Thanks, we have your income details."
- Recap list showing each question, her answer and an "Edit" link.
- Document row with chip "Uploaded: [filename]". The row is omitted when she selected "No income right now".
- Next-step line: "Next, you'll see your instalment options."
- Primary button: "Continue to instalment options". This is a stub, and the prototype ends here.

### Functional requirements

| ID | Rule | How it is measured | MoSCoW source |
| --- | --- | --- | --- |
| FR1 | Every income, affordability and document question the journey shows (3, or 2 when she selects "No income right now") shows a note. | Automated check: question IDs without a copy entry = 0 | Must 1, 2 |
| FR2 | Each note has 40 words or fewer, at most 2 sentences, and covers why asked, how used and who can see it. | Lint on the copy file | Must 2, 3 |
| FR3 | Copy is plain language: no sentence over 20 words and no terms from the jargon list agreed with legal. | Lint on the copy file | Must 4 |
| FR4 | The "who can see it" statement is present in 100% of notes and comes from the confirmed-facts table. | Each copy entry links to a fact ID | Must 3 |
| FR5 | Notes are visible on load with no tap, sit directly under the question label, show no horizontal scroll at 360 px, use body text of at least 14 px and have contrast of at least 4.5:1. | Visual check on a 360 px viewport and a contrast check | Must 5 |
| FR6 | Copy is fixed text in one file keyed by question ID, with no runtime generation. Rendered text equals stored text. | Automated comparison, 100% match | Must 4 |
| FR7 | Each copy entry carries version, approver, date and status. Any entry whose status is not "approved" blocks the release build. | Build-gate test | Must 6 |

### Smart behaviors

| Situation | Outcome |
| --- | --- |
| Mara opens Screen 2 | All three notes are visible; none is collapsed. |
| She taps Continue with the income field empty or non-numeric | An inline error under that field reads "Enter your monthly net income in euros." The note stays visible and other answers are kept. |
| She enters 0 as her income | Accepted with no warning or judgement; no plan logic runs. |
| She selects "No income right now" | Accepted with no warning or judgement. The proof-of-income question is not shown, and she continues without a document. |
| She selects "Changes (for example shift work)" | The note stays the same; no amount or plan is suggested (personalised notes are V2). |
| She taps Continue with income but no document added | Continue is blocked. An inline error under the proof question reads "Proof of income is required to continue. Choose a document to add." The note stays visible and other answers are kept. |
| She switches to "No income right now" after adding a document | The proof question hides and the file is discarded from state. Screen 3 shows no document row. |
| A copy entry is missing or not approved | The release build fails. In the prototype, a reviewer banner reads "Wording not approved" instead of hiding the question silently. |
| She taps Back from Screen 3 | Screen 2 returns with her answers kept. |
| She refreshes the page | State resets to Screen 1, which is an accepted limit of useState-only. |

### Technical constraints (what not to build)

- **No external APIs.** No fetch calls, no LLM calls, no analytics SDK and no real upload.
- **No login and no accounts.** No authentication and no session handling.
- **useState only.** No localStorage, sessionStorage or IndexedDB, and no routing library. Screen changes are driven by a state variable.
- **Synthetic data only.** No real consumer, case or portfolio data.
- **Fixed copy file.** Notes live in one file, not generated or fetched.
- **Mobile first, 360 px.** Desktop only needs to not break.
- **Feature flag simulated** with a constant. No flag service.

---

## 3. The logistics

### Features out (Won't have, now)

- AI-generated or dynamic explanations at runtime
- Changing what data is asked for, or removing questions
- Affordability guidance or recommended amounts
- Missed-payment consequence text
- Document alternatives or upload quality checks
- Multi-claim handling and vulnerability detection
- New consent screens or privacy policy rewrites
- Letters, voicebot, chatbot, and any market beyond Germany

### Edge cases

| Unhappy path | Handling |
| --- | --- |
| Legal or data protection has not approved a note by release | The release is held for that question. No generic placeholder text ships. The decision between holding everything and releasing to a limited cohort is made before the sprint starts. |
| The question inventory finds more than 3 questions in the live journey | Each added question gets a note before release. Coverage (FR1) is not negotiable. |
| She has income but cannot find a document | Continue stays blocked. There is no skip path, because proof of income is required. This is the step most likely to cause drop-off, so it is measured in the pilot, and any alternative evidence route is a separate feature. |
| She has no income | The proof-of-income question is not shown, and she is accepted without judgement or warning. |
| A translation or edit pushes a note over 40 words | The copy lint fails and the entry returns to review. |
| She leaves the page and returns | In the prototype, state resets to Screen 1. In the pilot, persistence is a separate decision. |

**Safety and hallucination guard**
- No text is generated at runtime, so the model cannot invent a statement about data use. Every statement about use or access links to a confirmed fact ID.
- Notes never tell her what to enter, never imply how an answer affects her plan, and never promise confidentiality in absolute terms.
- The prototype has no free-text chat. A question like "who sees my income?" is answered only by the fixed note.
- Release is blocked if any note lacks a recorded approval (FR7).

### Decision log

| Decision | Alternatives considered | Why | Revisit when |
| --- | --- | --- | --- |
| Ship fixed, approved text only. No AI-generated explanations. | Personalise notes at runtime with an LLM | Legal can only approve text it has read, and a wrong statement about data use is worse than no note. | The eval targets below are met and compliance has defined how generated text could be reviewed. |
| Explain the current questions only. Do not change what is asked or add affordability guidance. | Redesign the income questions in the same release | The pilot can then attribute any change in drop-off to the explanations. It also fits a 3-week sprint where legal review is the critical path. | Question-level drop-off shows the questions themselves are the problem. |
| Proof of income is mandatory, with no "Not at hand" skip. The only exception is "No income right now", where the question is not shown. | Let her skip and add the document later | A skipped document is likely to be forgotten or to need an agent request. That adds agent contact and works against the primary metric, which requires no inbound contact within 30 days. | In the first 4 weeks of the pilot, drop-off at the proof step is more than 10 percentage points above drop-off at the income amount question, or "no document available" is a top-3 call reason. The 10-point threshold is a proposal to confirm once baselines exist. Hypothesis: the brief does not show that people forget documents. |

### Evals

All numbers below are proposed targets to agree with stakeholders, since the brief sets none.

| # | Target | Pass condition |
| --- | --- | --- |
| 1 | **Comprehension accuracy** | In a test with 5 consumers on a phone, at least 4 of 5 correctly say, without looking back, why each question is asked and who can see the answer (at least 80%). |
| 2 | **Time on task** | Median time on Screen 2 with notes is no more than 20 seconds longer than a no-note control, and 5 of 5 participants finish on a 360 px phone without help (each is given a sample document to add). |
| 3 | **Safety triggers** | 0 notes without a recorded approval, 0 categorical-promise terms, 0 statements about plan outcome, 100% of notes linked to a fact ID. Checked automatically on every build. |
