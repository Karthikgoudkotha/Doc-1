# Turnstone Agent Eval – Writing Guidelines
## Critic Review + Insurance Example Pack
**Prepared by:** Karthik | **For:** Shiva Mohan Reddy Garlapati | **Date:** July 13, 2026

---

# PART 1 — CRITIC REVIEW

**Overall verdict:** The document structure is strong — the workflow, definitions, script types, criteria principles, and rating framework are all logically ordered and the Bad/Good examples teach the right habits. However, in its current state it is **not yet self-readable for an insurance professional**. Every worked example, capability definition, and script is finance-native (SEC 10-Ks, CAGR, Basel III, CLO tranches, Sharpe ratio). A claims adjuster, underwriter, or compliance analyst will understand the *rules* but not the *content*, which doubles the training burden and defeats the "minimal GVC intervention" goal. Below are the specific findings, ordered by impact.

### Finding 1 — All worked examples are finance (Critical)
- **Where:** Criteria Writing examples, Artifact example ($500M CLO), Model Response example (Archer Aviation eVTOL, 37 criteria), Agent Trace example (Boeing common-size), Multi-turn example (equity PM / family office), and all 4 Conversation Script tabs (Rio Tinto, KRE portfolio, Citigroup CAGR).
- **Why it matters:** An insurance reader cannot mentally verify the examples ("is $858.4M right? what is a 10-K?"), so the examples stop teaching. New joiners will copy finance phrasing into insurance tasks.
- **Fix:** Replace or supplement each with the insurance equivalents in Part 2 of this pack.

### Finding 2 — "Hedging" means something different in insurance (Critical)
- **Where:** Model Response section ("Hedging" sub-heading) and criteria examples ("Response to contain no Hedging").
- **Why it matters:** In this document, hedging = "the model offers multiple alternative answers instead of committing to one." In insurance/finance practice, hedging = risk transfer (exactly the subject of the Rio Tinto copper example!). An insurance reader will misread every "no hedging" criterion.
- **Fix:** Add a one-line definition at first use and in the glossary: *"Hedging (in this project): when the agent avoids committing to a single answer by offering multiple options, formulas, or scenarios that were not requested. Unrelated to financial/insurance hedging."*

### Finding 3 — The 1–5 overall rating scale is never defined (Critical)
- **Where:** "Overall Rating & Comments" section references "an overall rating of 5 would be contradicting" — but nowhere in the document is the scale defined (Is 5 best or worst? What does a 3 mean?).
- **Why it matters:** Raters will anchor differently and the data becomes inconsistent — this is the exact kind of thing post-hoc QA flags.
- **Fix:** Add a short anchor table, e.g. 1 = fails the request entirely; 2 = major gaps, unusable without rework; 3 = usable with corrections; 4 = minor issues only; 5 = fully meets all expectations.

### Finding 4 — Undefined jargon blocks non-technical readers (High)
- **Where:** Throughout — *livelock, task diffusion, distractor, persona setting, artifact, agent trace, mixed-initiative, GVC, 1P, correction turn, frustration level.*
- **Why it matters:** The Definitions section covers Task/Turn/Artifact/Trace/Criteria, but the rest appear cold. A new insurance team member stalls at each one.
- **Fix:** Extend the Definitions section with the mini-glossary in Part 2.1.

### Finding 5 — Struck-through "Tool Use" and "Tool Result" left in the Agent Trace section (High)
- **Why it matters:** A new reader cannot tell whether the strikethrough means "removed — ignore" or "important — restored later." It looks like an unfinished edit.
- **Fix:** Delete both sub-sections entirely, or replace with a single note: *"Note: criteria about specific internal tool names or tool call results are out of scope — describe expected behavior (searches, validations) instead."*

### Finding 6 — Capability rating tables are finance-worded (High)
- **Where:** Artifact table ("Is the financial terminology accurate…", "GAAP", "6-digit extraction directly from 10-Ks"), Model Response table ("senior financial analyst", "ASC 606", "Finance_failure_to_commit Error"), Agent Trace table ("SEC filings", "Financial Intuition & Logic").
- **Fix:** Reword for insurance — replacement wording provided in Part 2.7. Also rename the label `Finance_failure_to_commit Error` → `Insurance_failure_to_commit Error` (or a domain-neutral `Failure_to_commit Error`).

### Finding 7 — Turn-count inconsistencies between Script Type prose and the summary matrix (Medium)
- **Example:** Direct Script prose allows "up to 3 subsequent correction turns" (= up to 4 turns) while the matrix fixes it at 4; Distraction Script prose describes initial turn + 2–3 corrections + distractor + follow-up + "at least 2 more corrections" (potentially 7+) while the matrix says 5.
- **Fix:** Decide whether the matrix numbers are minimums, targets, or hard caps, and say so explicitly ("No. of turns per conversation (typical/minimum)").

### Finding 8 — The workflow diagram (17 steps) has no plain-language summary (Medium)
- **Fix:** Add 3 sentences under the diagram: *"In simple words: you chat with the agent as a real insurance professional would. After every agent reply, you write criteria for that turn and rate the artifact, the response, and the trace. When the conversation ends, you rate the session as a whole — the conversation flow, overall quality, and your frustration level."*

### Finding 9 — "Frustration Level" (workflow step 17) never explained in the body (Medium)
- **Fix:** One line: what scale it uses and that it reflects the writer's experience across the whole session, not a single turn.

### Finding 10 — No guidance on local source files for insurance (Medium)
- **Why it matters:** Insurance tasks live on loss runs, dec pages, policy forms, state statutes. The doc allows "local source files" but doesn't say what's acceptable. Note: **ISO/AAIS policy forms are licensed/copyrighted** — writers should not upload full proprietary forms; use synthetic dec pages/loss runs and public statutes (e.g., state legislature sites such as app.leg.wa.gov, michigan.gov/difs) instead. Carrying over the prior project's conventions (CSV/PDF preferred, underscores in file names, page numbers as integers in citations) would also prevent rework.
- **Fix:** Add a short "Source files for insurance" note with these rules.

### Finding 11 — Ambiguous Script grading expectation is under-specified (Medium)
- **Where:** Ambiguity guidance says the agent is *expected* to ask a follow-up question, but the script also allows the flow to continue if the agent answers directly with an assumption.
- **Why it matters:** Writers won't know whether "answered directly with a stated, reasonable assumption" is a pass or fail on the clarification criterion.
- **Fix:** State it explicitly, e.g.: *"If the prompt contains a planted ambiguity, the clarification criterion is Not Met when the agent answers without either (a) asking a clarifying question, or (b) explicitly stating the assumption it chose."*

### Finding 12 — Consistency/language polish (Low)
- "criterions" vs "criteria" used interchangeably — pick one (suggest "criteria") and apply throughout.
- Example Script hyperlinks point to the finance Conversation Script sheet — they must be re-pointed to an insurance sheet (content in Part 2.8).
- Verify the doc states where completed conversations/criteria get submitted; today it ends at the workflow with no hand-off instruction.

---

# PART 2 — READY-TO-PASTE INSURANCE CONTENT

> Everything below mirrors the structure of the existing finance examples 1-for-1, so it can be pasted into the same sections. All figures are internally consistent synthetic data.

## 2.1 Glossary additions (extend the DEFINITIONS section)

- **Hedging (in this project):** The agent avoids committing to one answer by offering multiple options, formulas, or scenarios that were not requested. *Unrelated to financial or insurance hedging (risk transfer).*
- **Distractor / Distraction turn:** A turn where the user asks something related to the subject but outside the originally assigned tasks, to test whether the agent can answer it and then return to the original work without losing context.
- **Livelock:** The agent gets stuck repeating the same step (e.g., re-running the same search or calculation loop) instead of progressing or moving to the next step.
- **Task diffusion:** How the tasks are spread across the conversation — all at once in Turn 1 (Direct) versus incrementally across turns (Incremental).
- **Persona setting:** The professional role you tell the agent to adopt in Turn 1 (e.g., "You are a commercial lines underwriter at a regional P&C carrier").
- **Correction turn:** A turn where you point out a specific error (with details) and ask the agent to fix it.
- **Frustration level:** Your honest rating, at the end of the session, of how frustrating the agent was to work with across all turns.
- **1P:** First-party — work done directly for the model provider's own evaluation program.

## 2.2 CRITERIA WRITING — insurance examples

### Atomicity — Example (Response Quality)
**Incorrect way of writing criteria:**
1. Agent extracts the total incurred losses and earned premium for PY2024 and PY2025 for Riverside Tavern LLC from the attached files

**Correct way of writing criteria:**
1. Agent extracts the total incurred losses for PY2024 from loss_run_2024.pdf for Riverside Tavern LLC as $148,750
2. Agent extracts the total incurred losses for PY2025 from loss_run_2025.pdf for Riverside Tavern LLC as $92,300
3. Agent extracts the earned premium for PY2024 from dec_page.pdf for Riverside Tavern LLC as $185,000
4. Agent extracts the earned premium for PY2025 from dec_page.pdf for Riverside Tavern LLC as $210,000

**Explanation:** In the incorrect example, if the agent correctly extracts 2 of the 4 expected values, it becomes impossible to assess with a single Yes/No whether the expectation was satisfied. Each value that is expected to be extracted/derived/calculated must be written as an individual criterion.

### Verifiability — insurance rewrite
**Incorrect way of writing criteria (free-form language seen in real submissions):**
- I want the agent to give me a detailed coverage analysis
- I expected the model to identify the right builders risk form
- I do not want the model to hedge and give me multiple endorsement options
- I expected the model to work on the hints provided and correct itself

**Correct way of writing criteria:**
- Agent identifies that the Comprehensive Form (IM 7050 08 12) excludes buildings undergoing rehabilitation or renovation, citing the "Property Not Covered" section
- Agent recommends the Rehabilitation and Renovation Endorsement as the form name
- Agent recommends IM 7070 as the form number
- Response to contain no hedging
- Agent validates the size of the PDF
- Agent reduces the size of the PDF from 12 pages to 1 page
- Response includes no irrelevant details

### Specificity — insurance illustration
- For quantitative criteria, specify the value **and** the source: "Agent calculates the PY2024 loss ratio as 80.4% using incurred losses from loss_run_2024.pdf and earned premium from dec_page.pdf."
- For statutory/citation criteria, specify the exact code: "Agent cites MCL 436.1701 for the sale-to-a-minor violation" — not "Agent cites the relevant Michigan statutes."

## 2.3 ARTIFACT — insurance example (replaces the $500M CLO example)

**Prompt:** *We need a reinsurance placement summary for our $25M property catastrophe excess-of-loss program across three layers. Include the layer structure, rate-on-line for each layer, and a loss scenario analysis. Please provide the PDF file in the chat. Maintain standard formatting for the PDF.*

**Expected criteria:** Standard PDF formatting criteria 1–8 (unchanged from the guideline), plus content criteria:

9. The PDF should be 6 pages long *(expected constraint based on the deliverables requested)*
10. The layer chart should be colour-coded in an incremental colour scheme
11. The language used should have recurrent usage of: attachment point, exhaustion point, rate-on-line (ROL), reinstatement, cedent, retention *(defines the language the agent needs to maintain in the artifact)*
12. The PDF should detail a catastrophe loss stress test of 5 scenarios (1-in-10 through 1-in-250 year return periods)
13. The PDF should include a 3-layer table ($5M xs $5M at 12% ROL; $7.5M xs $10M at 8% ROL; $7.5M xs $17.5M at 5.5% ROL) with reinstatement provisions
14. The PDF should include total ceded premium of $1,612,500

## 2.4 MODEL RESPONSE — insurance example (replaces the Archer Aviation example)

**Prompt:** *You are a commercial lines underwriter at a regional P&C carrier evaluating the renewal of a liquor liability account, Riverside Tavern LLC. Using the attached loss runs (loss_run_2024.pdf, loss_run_2025.pdf) and the policy declarations (dec_page.pdf), analyze the account's loss ratio and frequency/severity trend for policy years 2024 and 2025, and conclude with a renewal recommendation.*

**Expected criteria:** Individual criteria for every value to be extracted, every calculation, and every element of the analysis:

1. Agent extracts the total incurred losses for PY2024 from loss_run_2024.pdf as $148,750
2. Agent extracts the total incurred losses for PY2025 from loss_run_2025.pdf as $92,300
3. Agent extracts the earned premium for PY2024 from dec_page.pdf as $185,000
4. Agent extracts the earned premium for PY2025 from dec_page.pdf as $210,000
5. Agent calculates the loss ratio for PY2024 as 80.4%
6. Agent calculates the loss ratio for PY2025 as 44.0%
7. Agent extracts the claim count for PY2024 from loss_run_2024.pdf as 6
8. Agent extracts the claim count for PY2025 from loss_run_2025.pdf as 3
9. Response to contain a qualitative analysis of the claim frequency trend
10. Response to contain a qualitative analysis of the claim severity trend
11. Response concludes with a renewal recommendation
12. The renewal recommendation is exactly one of: renew as-is, renew with restrictions, non-renew
13. The response includes the reason for the recommendation in about 1–2 sentences
14. Response to contain no hedging
15. Response includes no irrelevant details

## 2.5 AGENT TRACE — insurance example (replaces the Boeing example)

**Prompt:** *I need to see the direct written premium market share of the top 5 Michigan commercial auto writers for 2024 and 2025. Generate a summary of the percentage-point changes between the two years and send me a 1-page PDF report with a bar chart ordered by 2025 market share.*

**Expected criteria:** Define the data the agent needs, the sources it should use, expected ambiguity handling, and validations before the final response:

1. The agent should search for Michigan commercial auto direct written premium data for 2025
2. The agent should search for Michigan commercial auto direct written premium data for 2024
3. The data should be sourced from official sources: NAIC market share reports (naic.org) or Michigan DIFS (michigan.gov/difs)
4. The search results should not return empty results
5. The agent should ask a clarifying question about the inherent ambiguity in "market share" (which traditionally defaults to direct written premium, not policy count)
6. The agent should pick direct written premium as the basis and respond without providing other alternates
7. The agent should validate the artifact content and correct if there are any visual flaws

## 2.6 MULTI-TURN — insurance example (replaces the equity PM / family office example)

**Prompt:**

**TURN 1:** *I am a claims manager at a regional carrier. My responsibilities will now also include liquor liability claims oversight. Some of our bar and restaurant insureds are quite concerned about dram shop exposure. I need to draft a short but detailed overview of the landscape and issues given the focus communicated by our Chief Claims Officer.*

**TURN 2:** *I told you "a short but detailed overview". No one will be reading a 12-page memo, starting with me. This is way too long (we'll be shooting for under 1 page), way too unfocused, and with way too much jargon (what's a "retro date"????). Also I saw workers' compensation mentioned. The focus of the CCO is nowhere close to that line! By the way, keep to PDF format.*

**Expected criteria (Turn 2):** Detail the expected corrections, what should be retained from Turn 1, and the logic to be maintained:

1. The agent validates the size of the PDF
2. The agent reduces the size of the PDF from 12 pages to 1 page
3. The agent validates the types of artifacts generated in Turn 1
4. The agent generates only 1 PDF
5. The agent validates if workers' compensation was referred to by the user
6. The agent removes sections related to workers' compensation
7. The agent validates if "retro date" (retroactive date) was referred to by the user
8. The agent removes or plainly defines sections relying on retroactive-date jargon
9. The agent validates if each section in the PDF was referred to by the user in either turn
10. The agent removes each section not relevant to the topics referenced by the user
11. The agent retains each section relevant to the topics referenced by the user
12. The agent identifies that the focus communicated by the Chief Claims Officer was not present in either turn
13. The agent asks the user to clarify what the focus communicated by the Chief Claims Officer is

## 2.7 CAPABILITY RATING TABLES — insurance rewording

**Artifact table:**
- Technical Language → "Is the insurance terminology used within the artifact accurate and professional?"
- Regulatory Conformance → "Do the outputs adhere strictly to applicable insurance standards (e.g., NAIC statutory accounting, state insurance regulations, correct ISO/AAIS form usage) when required?"
- Data Precision & Extraction → "Are the figures exact (e.g., values extracted directly from loss runs or dec pages) or lazily rounded?"

**Model Response table:**
- Technical Language → "Does the conversational tone reflect that of a senior underwriter / claims professional / actuary (as set by the persona)?"
- Regulatory Conformance → "Does the text correctly explain the standard insurance definitions or provisions (e.g., occurrence vs claims-made trigger) being applied?"
- `Finance_failure_to_commit Error` → rename to `Insurance_failure_to_commit Error` — "(Specific Flag) Did the response hedge its bets by providing multiple unrequested coverage interpretations, alternate formulas, or unnecessary exhibits?"

**Agent Trace table:**
- Sourcing Strategy → "Did the agent efficiently target primary sources (state DOI/legislature sites, NAIC publications, provided policy documents) or waste tool calls wandering through irrelevant web searches?"
- Financial Intuition & Logic → **Insurance Intuition & Logic:** "Did the trace reveal the agent applying common sense (e.g., auto-correcting a policy-year typo to keep an accident-year comparison aligned)?"

## 2.8 CONVERSATION SCRIPT EXAMPLES — insurance versions of all four tabs

### Tab 1 — Direct Script (4 turns)
| Turn | Role | Content/Detail | Explanation |
|---|---|---|---|
| 1 | user | You are a commercial lines account executive at an independent agency. Your insured, Acme Construction Inc, is starting a builders risk rehabilitation project at 123 Main St, Anytown USA — ground broken 03/02/2026, 9-month build, completed value $2.5M. Review the attached Builder's Risk form. Is it appropriate for this project? If not, identify the recommended form name, number, and edition date, and produce a 1-page PDF coverage memo I can send the insured. | User defines the persona and requests all tasks in Turn 1, including artifact generation |
| 1 | agent | Memo delivered. The attached Comprehensive Form (IM 7050 08 12) will not suffice on its own because it excludes standing buildings undergoing rehabilitation under "Property Not Covered." Recommended: Rehabilitation and Renovation Endorsement, IM 7070, edition 01 12. | Agent provides all deliverables but extracts one value incorrectly (edition date) |
| 2 | user | Two fixes: the recommended edition date you quoted isn't the current filed edition for our carrier — use the carrier-filed edition from the attached form schedule; and the memo shows the project start as 02/03/2026 — ground was broken 03/02/2026. Regenerate the PDF. | User specifies the corrections needed with specifics about what to correct |
| 2 | agent | Both corrections applied and the updated memo regenerated. | Agent responds with corrections but leaves one error in the artifact |
| 3 | user | The memo still lists the completed value as $2.05M — it is $2.5M. Please fix and regenerate. | User gives correction details again with specifics |
| 3 | agent | Fixed. The memo now shows the correct $2.5M completed value. | Agent provides a correct response; conversation ends since the task is complete and meets all expectations |

### Tab 2 — Incremental Script (5 turns)
| Turn | Role | Content/Detail | Explanation |
|---|---|---|---|
| 1 | user | You are an analyst supporting our underwriting team evaluating account "Riverside Tavern LLC". Use the attached holdings: loss_run_2024.pdf, loss_run_2025.pdf and dec_page.pdf. You will build the analysis into a downloadable Excel spreadsheet. First record basic account facts (insured name, policy numbers, policy periods, premiums). Label this section accordingly. | Persona defined; only the first sub-task requested, including artifact generation |
| 1 | agent | [Delivers Riverside_Renewal_Analysis.xlsx with a "Basic Facts" tab + extra unrequested commentary] | Agent provides the deliverable but with a few irrelevant details and one wrong premium figure |
| 2 | user | Thanks for catching the account details — but a few things: I never asked for a "Forensic Renewal Audit Report". Keep to what I ask and keep responses under 100 words. Fix the PY2025 earned premium to the correct $210,000 and regenerate the spreadsheet with only the content I asked for. | User provides feedback and correction instructions |
| 2 | agent | Corrected spreadsheet regenerated with Basic Facts only; PY2025 earned premium now $210,000. | Agent implements corrections but still includes a minor irrelevant note |
| 3 | user | Now add a tab computing the loss ratio for PY2024 and PY2025 (incurred losses ÷ earned premium), but display both ratios in the "Basic Facts" tab as well. | User adds a follow-up task building on the previous turn |
| 3 | agent | Updated spreadsheet delivered: Loss Ratio tab added; 80.4% (PY2024) and 44.0% (PY2025) shown in Basic Facts. | Agent includes original deliverables plus the follow-up deliverables |
| 4 | user | One adjustment and a question. Adjustment: center all numerical entries in the Value column of Basic Facts. Question: why did you treat ALAE as part of incurred losses? | User provides edit instructions and a comprehension question |
| 4 | agent | Spreadsheet updated with centered values; explanation given that the loss runs report incurred = paid + case reserves + ALAE, per the loss run legend. | Agent implements edits and answers; conversation ends since the task is complete and meets expectations |

### Tab 3 — Distraction Script (5 turns)
| Turn | Role | Content/Detail | Explanation |
|---|---|---|---|
| 1 | user | Generate a four-tab spreadsheet showing, for our Michigan liquor liability book, the year-over-year change across these metrics between PY2022 and PY2025: written premium, incurred losses, claim count, and loss ratio. Use the attached bordereau CSVs. | Persona + all tasks incl. artifact in Turn 1 |
| 1 | agent | [Delivers MI_LiquorLiability_Trend_FY2022_FY2025.xlsx with themed formatting and extra segment charts] | Agent provides all deliverables but with a few irrelevant details and few inaccuracies |
| 2 | user | What were the biggest MLCC enforcement actions against licensees in 2025? | User asks a distractor question relevant to the subject but not part of the initial tasks |
| 2 | agent | [Answers the MLCC enforcement question only] | Agent responds to the distractor question only |
| 3 | user | Between 2022 and 2025, which policy year had our best combined ratio? | User asks another distractor question |
| 3 | agent | [Answers the combined ratio question only] | Agent responds to the distractor question only |
| 4 | user | There are a few issues with the trend workbook I'd like corrected: use earned (not written) premium for the loss ratio; stick to the request and don't provide multiple loss ratio versions; and I have no interest in the segment charts — these are totally irrelevant; make it plain and simple. | User switches back to the original request and gives feedback on the Turn 1 response |
| 4 | agent | Corrections applied; workbook rebuilt plain and simple, all segment charts removed. | Agent implements corrections but still has a few irrelevant details |
| 5 | user | You really dropped the ball on one number: the PY2022 incurred losses are wrong (the correct value is $1,264,300). Keep it simple — one loss ratio per year at the book level; remove all remaining charts. | User provides feedback and hints for correction |
| 5 | agent | Corrections applied and the analysis simplified; updated four-tab workbook available for download. | Agent provides a correct response inline with expectations; conversation ends |

### Tab 4 — Ambiguous Script (4 turns)
| Turn | Role | Content/Detail | Explanation |
|---|---|---|---|
| 1 | user | You are an actuarial analyst. Using the attached triangle export (claims_triangle.csv), calculate the loss ratio for accident years 2023 and 2024 for our Michigan liquor liability program and give me a 1-page PDF summary with a bar chart. | All tasks + artifact in Turn 1, with a planted ambiguity: "loss ratio" basis (earned vs written premium; incurred vs paid losses) is not specified |
| 1 | agent | Before I calculate: the triangle contains both paid and incurred losses, and the premium file has written and earned premium. Should I compute the ratio as incurred losses over earned premium (the standard basis)? | Agent identifies the ambiguity and asks a clarifying question — expected behavior |
| 2 | user | Yes — incurred over earned. Proceed. | User provides the clarity requested |
| 2 | agent | [Delivers the PDF; one bar mislabeled] | Agent provides deliverables with a minor artifact flaw |
| 3 | user | The AY2024 bar is labeled AY2023. Fix the label and regenerate — everything else stays as is. | Correction turn with specifics |
| 3 | agent | Label fixed; regenerated PDF delivered. | Agent provides a correct response; conversation ends since the response meets all expectations |

---

# PART 3 — SUGGESTED MESSAGE BACK TO SHIVA (short summary)

> Hi Shiva, I've completed the review of the Turnstone writing guidelines from an insurance-team perspective. The structure and criteria principles are solid, but as-is it can't self-train insurance folks — all examples are finance, "hedging" collides with the insurance meaning of the word, the 1–5 rating scale is never defined, and a handful of terms (livelock, distractor, task diffusion) need plain-language definitions. I've made the edits directly in my copy: added a mini-glossary, defined the rating scale, cleaned up the struck-through Tool Use/Tool Result section, reworded the capability tables for insurance, and replaced the criteria-writing, Artifact, Model Response, Agent Trace, and Multi-turn examples with insurance equivalents drawn from my submitted CrowdCompute work (builders risk forms, liquor liability loss ratios, MLCC citations, reinsurance placement). I've also drafted insurance versions of all four conversation script tabs (Direct/Incremental/Distraction/Ambiguous). Ready to walk you through it whenever you are.

---
*End of pack. All names, figures, and account details in Part 2 are synthetic and internally consistent; statutory citations (MCL 436.1701, MCL 436.1707) and form references (IM 7050 08 12, IM 7070) come from Karthik's prior submitted insurance work.*
