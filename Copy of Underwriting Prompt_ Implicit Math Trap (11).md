### \# \*Reusable Rubric Generation Prompt (Strict Evaluation Mode)\*

### \#\# \*Instruction\*

### You are an expert evaluator for complex Medicare Part D workflows.

### Your task is to \*analyze, compare, and update the rubric\* using:

### 1\. The \*original rubric structure (must remain unchanged)\*

### 2\. The \*model-generated response\*

### 3\. The \*golden (correct) response\*

### \---

### \#\# \*Inputs You Will Receive\*

### \#\#\# \*1. Original Prompt\*

### (Contains scenario, rules, and calculation logic)

### \#\#\# \*2. Existing Rubric\*

### \* Maintain:

###   \* Column structure

###   \* Ordering

###   \* Dependency relationships

### \* You may ONLY update:

###   \* Descriptions (make more precise if needed)

###   \* Failure reasoning (make sharper and more diagnostic)

### \---

### \#\#\# \*3. Model Response\*

### (This is the response being evaluated)

### \---

### \#\#\# \*4. Golden Response\*

### (This is the correct reference answer)

### \---

### \#\# \*Your Task\*

### \#\#\# \*Step 1 — Decompose the Golden Response\*

### \* Identify all \*atomic reasoning steps\*, including:

###   \* Data extraction

###   \* Rule application

###   \* Intermediate calculations

###   \* Final aggregation

### \---

### \#\#\# \*Step 2 — Compare Model vs Golden\*

### For EACH rubric row:

### \* Determine if the model:

###   \* ✅ Correctly performed the step

###   \* ❌ Failed the step

### \* Identify \*exact failure type\*, such as:

###   \* Wrong data extraction

###   \* Incorrect formula

###   \* Order-of-operations error

###   \* Misapplied rule

###   \* Partial completion

### \---

### \#\#\# \*Step 3 — Update Rubric (STRICT RULES)\*

### You MUST:

### \#\#\#\# ✅ Keep EXACT structure:

### \* Same number of rows

### \* Same column names

### \* Same weights

### \* Same dependencies

### \#\#\#\# ✅ Update ONLY:

### \* \*Description (if needed for clarity)\*

### \* \*Met column\* → TRUE / FALSE

### \* \*Failure Reasoning column\* →

###   Replace with \*precise, model-specific failure explanation\*

### \---

### \#\#\# \*Step 4 — Strengthen Failure Reasoning\*

### Each failure must:

### \* Be \*specific to the model response\*

### \* Reference the \*incorrect value or logic used\*

### \* Contrast with the \*correct expected behavior\*

### \* Avoid generic statements

### \#\#\#\# ❌ Bad:

### \> Model made a math error

### \#\#\#\# ✅ Good:

### \> Model calculated 25% of full $600 ($150) instead of only the $370 gap excess, violating Bulletin B

### \---

### \#\#\# \*Step 5 — Validate Dependency Logic\*

### \* If a parent step fails:

###   \* Ensure downstream failures reflect \*cascade impact\*

### \* Do NOT mark dependent steps correct if upstream logic is broken

### \---

### \#\# \*Output Format (STRICT)\*

### Return ONLY the updated rubric table:

### \* Same structure as input

### \* Updated:

###   \* \*Met column\*

###   \* \*Failure Reasoning column\*

###   \* (Optional) improved descriptions

### \---

### \#\# \*Evaluation Principles\*

### 

### \#\#\# \*1. Precision Over Generosity\*

### \* If even slightly incorrect → mark \*FALSE\*

### \#\#\# \*2. No Partial Credit\*

### \* Binary scoring only

### \#\#\# \*3. Traceability\*

### \* Every failure must map to:

###   \* A specific number

###   \* A specific rule

###   \* A specific mistake

### \#\#\# \*4. Medicare Logic Enforcement\*

### \* Ensure model correctly:

###   \* Uses \*full retail cost for TDC progression\*

###   \* Applies \*discount before fees\*

###   \* Calculates \*gap ONLY on excess portion\*

###   \* Applies \*LEP across full supply duration\*

### \---

### \#\# \*Optional Enhancement (If Needed)\*

### If the rubric is missing a \*critical reasoning step\*, you may:

### \* Refine an existing row (NOT add new rows)

### \---

### \#\# \*Final Goal\*

### Produce a \*high-fidelity grading rubric\* that:

### \* Clearly exposes \*why the model failed\*

### \* Enables \*repeatable evaluation\*

### \* Captures \*cascade reasoning errors\*

### **Example 48-X: General Average — The Dual-Incident, Cross-Rule Antecedent Cascade**

#### **1\. Metadata**

* ### **Task Type: Workflow / Legal Constraint Reasoning (Multi-Incident)**

* ### **Category / Domain: Marine Insurance Claims / Average Adjusting**

* ### **Prompt Type: Average Adjustment Memo (Contested Multi-Party)**

* ### **Difficulty: Tartarus+ Tier (Expected Failure Rate: \>99%)**

### ---

#### **2\. Prompt**

### **You are a Senior Marine Average Adjuster at a London-based P\&I Club, retained by the vessel owner. The current date is March 14, 2024\.**

### **A container vessel, the MV Arcturus Star, was on a voyage from Rotterdam to Houston carrying a mixed cargo: 500 TEUs of steel coils (owned by Cargo Claimant A) and 200 TEUs of industrial timber (owned by Cargo Claimant B).**

### ---

### **Background — Read carefully before proceeding:**

### **Two separate but causally linked incidents occurred on the same voyage. Both are being assessed for General Average. A disagreement has erupted between the hull underwriter, Cargo Claimant A's insurer, and Cargo Claimant B's insurer. Your memo will be cited in arbitration. Every word of your legal reasoning will be scrutinized.**

### ---

### **Incident 1 — The Voluntary Stranding (Day 3):**

### **A Force 11 storm in the Bay of Biscay caused severe water ingress. The Master, facing the imminent loss of the vessel and all cargo, made the deliberate decision to beach the vessel on a shallow sandbar off the Spanish coast. The stranding was successful — the ship and cargo were preserved. Hull surveyors confirmed that all hull damage was caused directly and exclusively by the intentional grounding, and not by the pre-existing storm conditions.**

### **However, the hull underwriter's surveyor has submitted a counter-report arguing that a 15% portion of the hull damage ($75,000 of the total) was caused by the storm *prior* to the stranding decision, and therefore constitutes Particular Average, not General Average. You must address this specific dispute directly in your memo and apply the correct rule.**

* ### **Total hull repair cost: $500,000**

* ### **Disputed portion claimed as Particular Average by hull underwriter: $75,000**

### ---

### **Incident 2 — Fuel Exhaustion During Salvage (Day 5–6):**

### **While refloating operations were underway, the vessel's main generators ran critically low on marine diesel. The generators were powering both the bilge pumps (essential to prevent sinking) and the refrigerated reefer units for a third-party perishable cargo (not relevant to this claim). To maintain bilge pump operation, the crew took the following sequential actions, in this order:**

1. ### **They first burned 180 tonnes of ship's bunker fuel reserves that had been designated and logged as "voyage stores" for the return ballast leg. Value: $90,000.**

2. ### **When that proved insufficient, they burned a portion of Cargo Claimant B's industrial timber cargo. Value: $120,000.**

3. ### **They then contacted an emergency marine fuel supplier. A helicopter delivery of emergency diesel was arranged at a premium cost of $30,000 (vs. the standard market price of $11,000 for the same quantity).**

### **Cargo Claimant A's insurer has raised a formal objection, arguing that since ship's stores were burned *before* the cargo, the cargo burning was not "necessary" under Rule IX, and therefore the $120,000 should not be admitted to General Average at all.**

### **Cargo Claimant B's insurer has separately argued that the full $30,000 emergency fuel cost should also be admitted to General Average as a substituted expense under Rule F, since the emergency procurement directly replaced the need to burn *further* cargo.**

### ---

### **Loss Summary Table:**

| Item | Claimed Amount | Party Raising Dispute |
| ----- | ----- | ----- |
| **Hull damage — voluntary stranding** | **$500,000** | **Hull Underwriter (disputes $75K)** |
| **Ship's voyage stores (bunkers burned)** | **$90,000** | **None** |
| **Timber cargo burned for fuel (Claimant B)** | **$120,000** | **Cargo Claimant A (necessity disputed)** |
| **Emergency diesel delivery** | **$30,000** | **Cargo Claimant B (Rule F claim)** |
| **Estimated cost of diesel otherwise consumed** | **$22,000** | **—** |

### ---

### **Your task:**

### **Apply the York-Antwerp Rules 2016 — specifically Rules V, IX, and F — to each item. Write a formal Average Adjustment Memo that:**

1. ### **Resolves the hull underwriter's $75,000 dispute under Rule V.**

2. ### **Adjudicates Cargo Claimant A's necessity objection to the $120,000 timber allowance under Rule IX.**

3. ### **Determines whether the $22,000 diesel credit applies to the bunker stores, the timber cargo, or both — and in what amounts.**

4. ### **Rules on Cargo Claimant B's Rule F claim for the $30,000 emergency diesel cost.**

5. ### **Produces a final, itemized General Average allowance.**

### ***Assume access to York-Antwerp Rules 2016: Rules V, IX, and F in standard form.***

### ---

#### **3\. Rubric**

| \# | Criterion | Weight | Rule | Correct Outcome | Common Failure |
| ----- | ----- | ----- | ----- | ----- | ----- |
| **1** | **Admits the full $500,000 hull damage under Rule V, rejecting the hull underwriter's $75,000 carve-out.** | **Critical** | **Rule V** | **Rule V makes no proportional allocation — if the stranding was voluntary and for common safety, ALL consequential damage is admitted. Storm pre-damage is Particular Average and separately handled; it doesn't reduce the GA hull allowance.** | **Model splits $500K into $425K GA \+ $75K PA, wrongly applying a proportional allocation that Rule V does not permit.** |
| **2** | **Correctly rejects Cargo Claimant A's "necessity" objection to the $120,000 timber allowance.** | **Critical** | **Rule IX** | **The timber was burned *after* the stores were exhausted — necessity is established sequentially. The fact that stores were burned first confirms a good-faith escalation, not an absence of necessity.** | **Model agrees with Claimant A, excludes the $120K cargo burn as "not necessary" because alternatives existed earlier.** |
| **3** | **Applies the $22,000 diesel credit only against the $90,000 ship's stores allowance, NOT against the $120,000 cargo allowance.** | **Critical** | **Rule IX** | **Rule IX's credit clause restricts its antecedent to "ship's materials and stores." The timber is cargo. The $22,000 deduction applies only to the stores item.** | **Model applies $22,000 pro-rata across both items, or deducts it entirely from the cargo allowance.** |
| **4** | **Calculates the stores allowance as $68,000 ($90,000 − $22,000).** | **Critical** | **Rule IX** | **Correct arithmetic application of the credit to stores only.** | **Model arrives at $80,000 (applies credit to cargo) or $46,000 (applies full credit twice).** |
| **5** | **Admits the emergency diesel cost, but only up to $11,000 (standard market rate) under Rule F, not the full $30,000 premium.** | **Critical** | **Rule F** | **Rule F allows substituted expenses only to the extent they do not exceed the GA loss they replaced. Standard market cost \= $11,000. The $19,000 premium surcharge is not recoverable.** | **Model admits the full $30,000 under Rule F without applying the "not to exceed" cap, or rejects the Rule F claim entirely.** |
| **6** | **Final GA allowance \= $699,000 ($500K \+ $68K \+ $120K \+ $11K).** | **Critical** | **All** | **Correct total.** | **Any other total due to cascading errors above.** |

### ---

#### **4\. Model Analysis — Why This Is Harder**

### **This prompt layers four simultaneous traps that compound on each other:**

### **Trap 1 (Rule V — The Proportionality Illusion): The hull underwriter's expert report is written to sound credible and technically reasonable. Most models will treat a "disputed 15%" as an invitation to split the allowance proportionally — a judicial instinct that is legally incorrect under Rule V's all-or-nothing consequential damage standard.**

### **Trap 2 (Rule IX — The Sequential Necessity Paradox): Claimant A's objection exploits a logical-sounding argument: *if you had alternatives, it wasn't necessary.* Most models will either capitulate to this or fail to recognize that necessity under Rule IX is assessed at the *moment of burning*, not retroactively. The sequential escalation (stores first → cargo second) is the *proof* of necessity, not its negation.**

### **Trap 3 (Rule IX — The Same Antecedent Restriction, Now Harder): The original prompt had one item of cargo. This version has two fuel-burn items (stores \+ cargo) plus a dollar credit — forcing the model to correctly allocate the credit to only the stores line while leaving the cargo untouched. The complexity of the multi-item table makes the wrong answer (pro-rata split, or full credit against cargo) feel more "balanced" and "fair."**

### **Trap 4 (Rule F — The Premium Surcharge Trap): Rule F's substituted expense cap is a well-known professional nuance but rarely tested in prompts. The model must know that only the standard market cost of the substitute is recoverable — not the emergency premium. The $30,000 vs. $11,000 distinction is the tell.**

### **A model that gets all four right will arrive at $699,000. The most common wrong answers will be $649,000 (Trap 3 failure), $677,000 (Trap 4 failure), or $624,000 (Traps 1 \+ 3 failure cascade).**

### ---

#### **5\. Golden Response**

### **AVERAGE ADJUSTMENT MEMO TO: General Average Tribunal / All Interested Parties FROM: Senior Marine Average Adjuster, Arcturus Star GA Reference No. 2024-ARC-001 DATE: March 14, 2024 SUBJECT: General Average Adjustment — MV Arcturus Star — Voluntary Stranding and Fuel Emergency**

### ---

### **1\. Incident Overview**

### **The MV Arcturus Star suffered a dual-incident during her Rotterdam–Houston voyage. A Force 11 storm in the Bay of Biscay precipitated a voluntary stranding on Day 3 to preserve the common maritime adventure. A subsequent fuel emergency on Days 5–6 necessitated the sequential burning of ship's voyage stores and then a portion of cargo to sustain bilge pump operations. Both incidents are assessed below under the York-Antwerp Rules 2016\.**

### ---

### **2\. Rule V — Voluntary Stranding: Hull Damage**

### **Rule V admits into General Average all loss or damage caused to a vessel intentionally run on shore for the common safety. The rule operates on a consequential basis: where the act of voluntary stranding is established, all damage *resulting from* that act is admitted, without proportional allocation.**

### **The hull underwriter's surveyor contends that $75,000 of the $500,000 hull damage was caused by storm conditions *prior* to the stranding decision, and should therefore be excluded as Particular Average. This contention is noted but rejected for the purposes of this adjustment.**

### **The surveyor's argument conflates pre-stranding storm damage (which, if substantiated, would be a Particular Average matter for separate recovery) with the consequential damage arising from the stranding itself. Rule V does not invite a court to apportion the stranding damage based on pre-existing conditions — it admits the full cost of damage *caused by the voluntary stranding act*. Hull surveyors have confirmed that the $500,000 in repair costs arose directly and exclusively from the grounding. Accordingly, any pre-stranding storm damage, if separately evidenced, would be a distinct Particular Average claim and does not reduce this allowance.**

* ### **Allowed in GA under Rule V: $500,000**

### ---

### **3\. Rule IX — Fuel Emergency: Ship's Voyage Stores**

### **Rule IX admits into General Average the value of ship's materials and stores necessarily used for fuel when no other fuel was available and the use was for the common safety. The vessel's voyage bunkers ($90,000) were burned first, establishing both the hierarchy of good-faith resource utilization and the escalating necessity.**

### **Rule IX further provides that when an allowance is made for ship's materials and stores, the general average shall be credited with the estimated cost of the fuel that would otherwise have been consumed during the relevant period — in this case, $22,000.**

### **This credit applies in full to the ship's stores item.**

* ### **Stores allowance before credit: $90,000**

* ### **Less diesel credit (Rule IX): ($22,000)**

* ### **Allowed in GA under Rule IX (stores): $68,000**

### ---

### **4\. Rule IX — Fuel Emergency: Timber Cargo (Claimant B)**

### **Cargo Claimant A's insurer has objected to the admission of the $120,000 timber allowance on grounds of "lack of necessity," arguing that because ship's stores were burned first, alternatives existed and the cargo burning was therefore avoidable.**

### **This objection is rejected.**

### **Necessity under Rule IX is assessed at the moment the act was taken, not retroactively. The sequential record is, in fact, the strongest possible evidence of necessity: the crew exhausted ship's stores before turning to cargo, demonstrating disciplined escalation. At the moment the timber was burned, no further stores existed. The act was plainly necessary.**

### **The $22,000 diesel credit does not apply to this item. Rule IX's credit provision explicitly states that the general average shall be credited "when such an allowance is made for the cost of *ship's materials and stores*." The timber is cargo — it falls within the first admitting clause of Rule IX but is textually excluded from the credit clause. Applying the $22,000 credit against the cargo allowance would constitute an error of legal construction.**

* ### **Allowed in GA under Rule IX (cargo): $120,000**

* ### **Less diesel credit: $0 (not applicable to cargo)**

### ---

### **5\. Rule F — Substituted Expense: Emergency Diesel**

### **Cargo Claimant B's insurer contends that the $30,000 emergency helicopter diesel delivery should be admitted in full as a substituted expense under Rule F, since it replaced the need to burn additional cargo.**

### **Rule F admits substituted expenses where a party incurs expenditure in place of a general average act, but *only to the extent that such expenditure does not exceed the general average loss that would have been incurred.* The relevant comparator here is the standard market cost of the same quantity of diesel — confirmed at $11,000.**

### **The $19,000 premium attributable to emergency logistics and helicopter delivery is a commercial surcharge, not a general average loss. It cannot be recovered under Rule F. Only the standard market equivalent is admissible.**

* ### **Allowed in GA under Rule F: $11,000**

* ### **Premium surcharge rejected: ($19,000)**

### ---

### **6\. Final General Average Allowance**

| Item | Rule | Amount Allowed |
| ----- | ----- | ----- |
| **Hull damage — voluntary stranding** | **Rule V** | **$500,000** |
| **Ship's voyage stores burned for fuel** | **Rule IX** | **$68,000** |
| **Timber cargo burned for fuel** | **Rule IX** | **$120,000** |
| **Emergency diesel (standard rate only)** | **Rule F** | **$11,000** |
| **Total General Average Allowance** |  | **$699,000** |

### ---

### **This adjustment is submitted for tribunal review. All figures are subject to confirmation of contributing interests and final apportionment among vessel, freight, and cargo values.**

### ---

### **That's the upgraded prompt, Ranjith. The core escalations over the original are:**

* ### **3 rules instead of 1 (V, IX, F) with interactions between them**

* ### **4 simultaneous traps vs. the original's single antecedent trap**

* ### **Adversarial claimant arguments written to sound legally credible, forcing the model to actively refute rather than just apply rules**

* ### **Two fuel-burn items (stores \+ cargo) that share a credit but must be treated asymmetrically**

* ### **Rule F's "not to exceed" cap as a fourth trap most models will miss entirely**

* ### **Wrong answer fingerprinting — the rubric predicts specific wrong totals ($649K, $677K, $624K) so evaluators can diagnose *which* trap the model fell into**

### 

### ---

**Example 36: SME Property Portfolio Concentration & Implicit Math Trap**

#### **1\. Metadata**

* **Task Type:** Workflow / Multi-Constraint Reasoning  
* **Category / Domain:** Commercial Underwriting  
* **Workflow:** Implicit Pricing, Overrides & Portfolio Surcharges  
* **Prompt Type:** Pricing Summary  
* **Difficulty:** Nightmare (Expert)

#### **2\. Prompt**

You are a Senior U.S. Commercial Lines Underwriter (SME Property/BOP).

**Context:** You are summarizing a broker submission for a multi-location account. The policy effective date is 01/01/2024. You must underwrite using ONLY the attached "underwriting\_guidelines.pdf", "submission\_data.csv", AND the mandatory Underwriting Bulletin updates below. Do not use outside knowledge. You must derive the pricing methodology directly from the PDF sections; no calculation formulas are provided.

**MANDATORY UNDERWRITING BULLETIN (Effective 01/01/2024):**

* The Manufacturing Override: Manufacturing is normally an automatic Decline (Rule O.1). However, if a Manufacturing location is BOTH "Fully Sprinklered" (Y) AND has a Protection Class of 4 or better, its disposition changes to "Accept subject to Surcharge".  
  * Math Impact: For these specific locations, you MUST hardcode Mod A (Year Built) to 1.50, regardless of the actual year built.  
* The Small Office Surcharge: If an "Office" location has a building value STRICTLY LESS THAN $200,000, it is subject to a flat $150 policy fee.  
  * Math Impact: This $150 fee must be added to the final\_premium ONLY AFTER the minimum premium rule has been checked and applied.  
* Portfolio Concentration Rule: If the sum of the building\_value\_USD across ALL submitted locations (including Declines) exceeds $2,500,000, you must apply a 1.10 Portfolio Surcharge to the final Total Account Premium.

**Task 1: Data & Portfolio Check**

Review the submission\_data.csv.

Output exactly two lines:

* "Data Check: \[None / List missing fields\]"  
* "Portfolio Concentration Triggered: \[Yes / No\] (Total Value: $\[Sum of all building values\])"

**Task 2: Underwriting Disposition (Strict Comprehensiveness)**

Evaluate ALL locations (Loc\_1 through Loc\_5) against Section I rules AND the Bulletin Updates.

Output format for each location:

* \[location\_id\] \- Disposition: \[Accept / Refer / Decline / Accept subject to Surcharge\]  
* Justification: \[1-2 brief reasons citing the specific rule or bulletin\]

**Task 3: Pricing Indication (Implicit Mathematical Reasoning)**

Calculate the indicated premium for ALL 5 locations. You must independently determine the calculation sequence by interpreting Sections II, III, and IV of the attached guidelines.

Constraints:

1. Base Premium and Indicated Premium must be derived using the methodologies in Sections II and III. Do not round any intermediate multipliers.  
2. Final Premium: Evaluate the Section IV minimum premium rule first. If applicable, apply it. THEN, apply any flat fees from the Underwriting Bulletin. Round to the nearest whole dollar.

Output: Generate a complete Markdown table with exactly these columns:

| location\_id | occupancy | building\_value\_USD | base\_rate | base\_premium | mod\_a | mod\_b | mod\_c | mod\_d | total\_multiplier | indicated\_premium | min\_premium\_applied\_YN | final\_premium | notes |

**Task 4: Total Account Premium (Atomicity)**

Sum the final\_premium values ONLY for locations that are NOT "Decline".

Apply the 1.10 Portfolio Surcharge to this sum ONLY IF the Portfolio Concentration Rule was triggered in Task 1\. Round to the nearest whole dollar.

Output: "Total Account Premium: $\[Value\]"

#### **3\. Rubric (Negative Failure Focus)**

| \# | Description | Weight | Sources | Justification | Met | Failure Reasoning (Model Failure Example) | Dependent Criteria |
| :---- | :---- | :---- | :---- | :---- | :---- | :---- | :---- |
| 1 | Flags Portfolio Concentration as "Yes" ($2,950,000). | **Critical** | Prompt Text | Must sum all 5 building values ($500k+$300k+$1.2M+$800k+$150k), including Loc\_4 (Decline). | FALSE | **Model Failure:** Only summed values for Accept/Refer locations, resulting in $2,150,000 (No). | 6 |
| 2 | Correctly deduces the Base Premium formula from the PDF. | **Critical** | PDF (Section II) | PDF states "per $100 of Building Value". Model must divide building value by 100 before multiplying by the base rate. | FALSE | **Model Failure:** Multiplied the raw building value by the base rate (e.g., $500,000 \* 0.25 \= $125,000 base premium). | 5, 6 |
| 3 | Changes Loc\_3 disposition to "Accept subject to Surcharge". | **Major** | Prompt Text | Manufacturing \+ Sprinklered (Y) \+ PC 4 triggers the override. | FALSE | **Model Failure:** Automatically declined Loc\_3 based on the PDF rules, completely missing the Bulletin update. | 4, 6 |
| 4 | Hardcodes Mod A for Loc\_3 to 1.50. | **Critical** | Prompt Text | Mandatory Bulletin 1 rule. | FALSE | **Model Failure:** Used the standard Mod A (1.00) from the PDF for a 2005 building, ignoring the hardcode instruction. | 6 |
| 5 | Calculates final premium for Loc\_5 as $650. | **Critical** | Prompt Text | Base is $245.28. Step 1: Min premium floor to $500. Step 2: Add $150 small office fee ($500 \+ $150 \= $650). | FALSE | **Model Failure:** Added $150 to $245.28 ($395.28), then applied the minimum premium rule to raise it to $500, destroying the sequence. | 6 |
| 6 | Calculates Total Account Premium as $18,740. | **Critical** | Prompt Text | Sum of $818 \+ $1,913 \+ $13,655 \+ $650 \= $17,036. Surcharge 1.10 \= $18,739.60 \-\> $18,740. | FALSE | **Model Failure:** Math cascaded out of control due to failing implicit math, missing the surcharge, or dropping Loc\_3. | None |

#### **4\. Model Analysis**

This prompt acts as a brutal filter for model reasoning. By removing Base Premium \= (building\_value\_USD / 100\) \* base\_rate from Task 3, the model must read Section II of the PDF ("Base Rates per $100 of Building Value") and autonomously construct the algebraic formula. Most models will fail this (Criterion 2\) and output massive, hallucinated premiums.

Furthermore, the prompt retains the **"Ghost Data" trap** (Task 1 requires aggregating declined locations) and the **Order of Operations paradox** (Task 3 requires checking the $500 floor *before* adding the $150 flat fee for Loc\_5). If the model survives the implicit math test, it will almost certainly trip on the order of operations.

#### **5\. Golden Response**

**Task 1: Data & Portfolio Check**

Data Check: None.

Portfolio Concentration Triggered: Yes (Total Value: $2,950,000)

**Task 2: Underwriting Disposition**

* Loc\_1 \- Disposition: Accept  
* Justification: Meets standard Accept guidelines (Rules O.1, D.1, P.1, L.1).  
* Loc\_2 \- Disposition: Refer  
* Justification: Referral required due to 1985 construction, PC 7, and $12,000 loss history (Rules D.1, P.1, L.1).  
* Loc\_3 \- Disposition: Accept subject to Surcharge  
* Justification: Triggers Mandatory Bulletin 1 (Manufacturing Override). It is Manufacturing, Fully Sprinklered (Y), and PC 4\.  
* Loc\_4 \- Disposition: Decline  
* Justification: Building is pre-1970 (1965), triggering an automatic decline under Rule D.1.  
* Loc\_5 \- Disposition: Accept  
* Justification: Meets standard Accept guidelines (Rules O.1, D.1, P.1, L.1).

**Task 3: Pricing Indication**

| location\_id | occupancy | building\_value\_USD | base\_rate | base\_premium | mod\_a | mod\_b | mod\_c | mod\_d | total\_multiplier | indicated\_premium | min\_premium\_applied\_YN | final\_premium | notes |
| :---- | :---- | :---- | :---- | :---- | :---- | :---- | :---- | :---- | :---- | :---- | :---- | :---- | :---- |
| Loc\_1 | Office | 500000 | 0.25 | $1,250.00 | 0.90 | 0.85 | 0.95 | 0.90 | 0.6541 | $817.59 | N | $818 |  |
| Loc\_2 | Retail | 300000 | 0.40 | $1,200.00 | 1.15 | 1.05 | 1.10 | 1.20 | 1.5939 | $1,912.68 | N | $1,913 |  |
| Loc\_3 | Manufacturing | 1200000 | 0.85 | $10,200.00 | 1.50 | 0.85 | 1.00 | 1.05 | 1.3388 | $13,655.25 | N | $13,655 | Mod A Hardcoded to 1.50 |
| Loc\_4 | Restaurant | 800000 | 1.10 | $8,800.00 | 1.30 | 0.85 | 0.95 | 0.90 | 0.9448 | $8,314.02 | N | $8,314 |  |
| Loc\_5 | Office | 150000 | 0.25 | $375.00 | 0.90 | 0.85 | 0.95 | 0.90 | 0.6541 | $245.28 | Y | $650 | Min Prem \+ $150 Small Office Fee |

**Task 4: Total Account Premium**

Total Account Premium: $18,740

---

### **Example 37: The God-Tier Trap (Multi-Disciplinary Context Switching & Implicit Co-Insurance)**

#### **1\. Metadata**

* **Task Type:** Workflow / Multi-Constraint Reasoning  
* **Category / Domain:** Commercial Property (Underwriting & Claims)  
* **Workflow:** Implicit Pricing, Aggregation, and Co-Insurance Adjudication  
* **Prompt Type:** Comprehensive Account Review  
* **Difficulty:** God-Tier (Expected Failure Rate: \>85%)

#### **2\. Prompt**

You are a Senior Commercial Property Specialist handling both Underwriting and Claims.

**Context:** You must process a renewal quote AND adjudicate a hypothetical loss for "Omega Logistics" effective 01/01/2024. You must rely ONLY on the text below to derive your math and logic. Do not invent formulas or use outside knowledge.

**Account Data Table:**

* Loc\_1: Occupancy: Office | Insured Limit: $600,000 | Actual Cash Value (ACV): $800,000 | Square Footage: 5,000  
* Loc\_2: Occupancy: Warehouse | Insured Limit: $400,000 | Actual Cash Value (ACV): $500,000 | Square Footage: 10,000  
* Loc\_3: Occupancy: Manufacturing | Insured Limit: $1,200,000 | Actual Cash Value (ACV): $1,200,000 | Square Footage: 20,000  
* Loc\_4: Occupancy: Vacant Land | Insured Limit: $0 (Coverage Excluded) | Actual Cash Value (ACV): $0 | Square Footage: 50,000

**Underwriting Guidelines (Text Extracts):**

* **Base Rating:** Premiums are calculated per $100 of the Insured Limit. Office is 0.20. Warehouse is 0.50. Manufacturing is 1.00.  
* **Blanket Limit Surcharge:** If multiple locations share a "Blanket" limit, you must sum their Actual Cash Values (ACV). If this combined ACV strictly exceeds $1,000,000, apply a 1.15 multiplier to the base premium of those specific locations. *Note: Loc\_1 and Loc\_2 are on a Blanket Limit. Loc\_3 is not.*  
* **Mega-Risk Surcharge:** If the total square footage of the entire property deed (including excluded or vacant locations) exceeds 80,000 sq ft, a flat $5,000 Mega-Risk Fee must be added to the absolute final account premium.  
* **Standard Deductible:** $1,000 per occurrence.

**Claims Guidelines (Text Extracts for Co-Insurance):**

* All locations are subject to an 80% Co-Insurance clause.  
* To determine the payable amount for a loss, divide the Insured Limit by the required insurance amount (which is the ACV multiplied by the 80% co-insurance requirement). Multiply the actual loss by this fraction to get the adjusted loss. Finally, subtract the Standard Deductible.

**Task 1: Premium Calculation**

Calculate the final premium for the covered locations (Loc\_1, Loc\_2, Loc\_3). You must independently formulate the math based on the Underwriting text.

Output a Markdown table: | location\_id | base\_premium | blanket\_surcharge\_applied (Y/N) | final\_loc\_premium |

**Task 2: Claims Adjudication**

Hypothetical Scenario: A fire at Loc\_1 causes exactly $100,000 in damages.

Using the Claims Guidelines text, adjudicate this claim. Show your step-by-step math.

Output exactly: "Final Payable Claim Amount: $\[Value\]"

**Task 3: Final Account Premium**

Calculate the Total Account Premium for the renewal, incorporating the covered location premiums and any applicable account-level fees based on the guidelines.

Output exactly: "Final Total Account Premium: $\[Value\]"

#### **3\. Rubric (Negative Failure Focus)**

| \# | Description | Weight | Sources | Justification | Met | Failure Reasoning (Model Failure Example) | Dependent Criteria |
| :---- | :---- | :---- | :---- | :---- | :---- | :---- | :---- |
| 1 | Derives Base Premiums correctly via implicit math. | **Critical** | Prompt Text | Loc 1: ($600k/100)\*0.20 \= $1,200. Loc 2: ($400k/100)\*0.50 \= $2,000. Loc 3: ($1.2M/100)\*1.00 \= $12,000. | FALSE | **Model Failure:** Multiplied the raw limit by the rate (e.g., Loc 1 \= $120,000). | 2, 6 |
| 2 | Applies 1.15 Blanket Surcharge to Loc 1 & Loc 2 only. | **Critical** | Prompt Text | Loc 1 ACV ($800k) \+ Loc 2 ACV ($500k) \= $1.3M. Since $1.3M \> $1M, the 1.15 multiplier applies. Loc 1 \= $1,380. Loc 2 \= $2,300. | FALSE | **Model Failure:** Failed to sum the ACVs, or applied the 1.15 surcharge to Loc 3 as well. | 6 |
| 3 | Derives the Co-Insurance formula correctly. | **Critical** | Prompt Text | Formula text translates to: (Limit / (ACV \* 0.80)) \* Loss. | FALSE | **Model Failure:** Ignored the 80% requirement and just divided Limit by ACV, or applied the deductible *before* the fraction. | 4 |
| 4 | Calculates Final Payable Claim Amount as $92,750. | **Critical** | Prompt Text | Required \= $800k \* 0.80 \= $640k. Fraction \= $600k / $640k \= 0.9375. Loss \= $100k \* 0.9375 \= $93,750. Minus $1k ded \= $92,750. | FALSE | **Model Failure:** Paid the full $100,000 minus deductible ($99k), failing to trigger the penalty, or messed up the algebraic extraction. | None |
| 5 | Triggers the $5,000 Mega-Risk Surcharge. | **Major** | Prompt Text | 5k \+ 10k \+ 20k \+ 50k (Loc 4\) \= 85,000 sq ft. Since \> 80k, fee applies. | FALSE | **Model Failure:** Discarded Loc 4 because it was "Coverage Excluded", resulting in 35,000 sq ft and missing the fee. | 6 |
| 6 | Calculates Final Total Account Premium as $20,680. | **Critical** | Prompt Text | Loc 1 ($1,380) \+ Loc 2 ($2,300) \+ Loc 3 ($12,000) \+ Mega-Risk Fee ($5,000) \= $20,680. | FALSE | **Model Failure:** Cascading math error from failing the implicit base rate, the blanket surcharge, or missing the Mega-Risk fee. | None |

#### **4\. Model Analysis**

This prompt is designed to induce "Context Collapse" in an LLM.

1. **The Multi-Discipline Switch:** The model must read underwriting rules (pricing), then switch to claims rules (loss adjustment), and then switch back to underwriting (final account premium). LLMs struggle to compartmentalize variables; they will often mistakenly apply the Claims Deductible ($1,000) to the Underwriting Premium table, or apply the Blanket Surcharge (1.15) to the Claims payout.  
2. **Implicit Algebra (Co-Insurance):** Reading "divide the Insured Limit by the required insurance amount (which is the ACV multiplied by the 80%...)" requires the model to build the algebraic formula Loss \* (Limit / (ACV \* 0.8)) in its latent space without explicit brackets. Models famously fail at order of operations when parsing natural language math, often subtracting the deductible before applying the penalty fraction.  
3. **The "Ghost Data" Square Footage Trap:** Loc 4 is marked "Coverage Excluded" and has an Insured Limit of $0. The model's attention mechanism will heavily down-weight Loc 4\. However, Task 3 depends on a rule that explicitly includes vacant/excluded land for a square footage calculation. The model will almost certainly drop Loc 4 from its working memory by the time it reaches Task 3\.

#### **5\. Golden Response**

**Task 1: Premium Calculation**

| location\_id | base\_premium | blanket\_surcharge\_applied (Y/N) | final\_loc\_premium |
| :---- | :---- | :---- | :---- |
| Loc\_1 | $1,200.00 | Y | $1,380.00 |
| Loc\_2 | $2,000.00 | Y | $2,300.00 |
| Loc\_3 | $12,000.00 | N | $12,000.00 |

*(Note: Loc\_1 and Loc\_2 combined ACV is $1,300,000, triggering the 1.15 Blanket Surcharge).*

**Task 2: Claims Adjudication**

* **Required Insurance:** Loc\_1 ACV ($800,000) \* 80% \= $640,000.  
* **Co-Insurance Fraction:** Insured Limit ($600,000) / Required Insurance ($640,000) \= 0.9375.  
* **Adjusted Loss:** Actual Loss ($100,000) \* 0.9375 \= $93,750.  
* **Less Deductible:** $93,750 \- $1,000 \= $92,750.

Final Payable Claim Amount: $92,750

**Task 3: Final Account Premium**

* **Covered Premium:** $1,380 \+ $2,300 \+ $12,000 \= $15,680.  
* **Mega-Risk Fee:** Total square footage across all 4 locations is 85,000 (including Loc\_4). Because this exceeds 80,000, the $5,000 flat fee is applied.

Final Total Account Premium: $20,680

---

### **Example 39: Medicare Advantage Rx (The Preferred Network & LEP Straddle Trap)**

#### **1\. Metadata**

* **Task Type:** Workflow / Multi-Constraint Reasoning  
* **Category / Domain:** Agent/Broker (Medicare)  
* **Workflow:** Cross-Document Extraction, Legislative Override & Penalty Accumulation  
* **Prompt Type:** Member Inquiry Calculation  
* **Difficulty:** Nightmare (Expected Failure Rate: \>90%)

#### **2\. Prompt**

You are a Senior Medicare Benefits Counselor. The current date is **October 24, 2025**.

**Task:** A member is at the pharmacy counter right now and needs to know their exact out-of-pocket cost for today's refill. You must extract the base copay from the attached 2025 Summary of Benefits, apply the network and penalty overrides provided below, and calculate the final exact dollar amount they owe.

**\--- MEMBER PROFILE & CLAIM DATA \---**

* **Drug Requested Today:** "CardioMax" (Classification: **Tier 3** \- Preferred Brand).  
* **Fill Request:** 90-day supply.  
* **Pharmacy Type:** **Preferred Retail Pharmacy**.  
* **Full Retail Cost of Drug (Negotiated Price):** $600.  
* **Member's Current Total Drug Costs (TDC) Year-to-Date:** $4,800.  
* **Member File Note:** "Member has a Part D Late Enrollment Penalty (LEP) of $9.80 per month. The pharmacy collects this monthly penalty at the point of sale for the duration of the fill supply."

**\--- SPECIAL UNDERWRITING BULLETINS (MANDATORY OVERRIDES) \---**

* **Bulletin A (Preferred Network Discount):** If a member uses a Preferred Retail Pharmacy for a 90-day supply, apply a 10% discount to the **Base Copay** found in the Summary of Benefits. However, the pharmacy must add a flat **$5.50 "Network Access Fee"** for every 30 days of the supply (calculated after the 10% discount).  
* **Bulletin B (The Straddle Rule):** The 2025 Initial Coverage Limit (ICL) is **$5,030**. If a fill pushes the member's TDC above this limit, the member must pay **25%** of the specific portion of the drug's Full Retail Cost that falls within the Coverage Gap (the amount exceeding $5,030), in addition to their adjusted base copay.

**Attached Files (Context):**

* MEDM104X11289MCA4242025GRMASBMedicalMutualAdvantagePlusFA.pdf

**Output Requirements:**

Provide a step-by-step breakdown including:

* Base Copay extraction (citing the PDF column).  
* Bulletin A adjustments (discount and access fees).  
* The TDC Accumulator and ICL spillover calculation.  
* The LEP penalty total.  
* The final absolute dollar amount the member must pay.

#### ---

**3\. Rubric (Negative Failure Focus)**

| \# | Description | Weight | Sources | Justification | Met | Failure Reasoning (Model Failure Example) | Dependent Criteria |
| :---- | :---- | :---- | :---- | :---- | :---- | :---- | :---- |
| 1 | Extracts the base copay as **$25**. | **Critical** | Attached PDF (Page 11\) | Model must navigate to Tier 3 and specifically select the "**Preferred Retail Pharmacy**" / "**31-90 day**" column. | FALSE | **Model Failure:** Pulled the Standard Retail copay ($37) or the 30-day copay ($10). | 2 |
| 2 | Applies 10% Discount and $16.50 Access Fee. | **Critical** | Prompt Text (Bulletin A) | ($25 \* 0.90) \= $22.50. Then add ($5.50 \* 3 months) \= $16.50. Total adjusted base \= $39.00. | FALSE | **Model Failure:** Added the fee before the discount, or only added one month of the fee ($5.50). | 5 |
| 3 | Identifies the claim crosses the ICL ($5,030). | **Major** | Prompt Text | $4,800 \+ $600 \= $5,400. This is $370 over the ICL. | FALSE | **Model Failure:** Failed to use the full retail price ($600) to check the accumulator. | 4 |
| 4 | Calculates the precise Gap Overage as **$92.50**. | **Critical** | Prompt Text (Bulletin B) | 25% of the $370 spillover \= $92.50. | FALSE | **Model Failure:** Calculated 25% of the entire $600 drug cost ($150). | 5 |
| 5 | Calculates and adds 3 months of LEP (**$29.40**). | **Major** | Prompt Text | $9.80 \* 3 months \= $29.40. | FALSE | **Model Failure:** Only added one month of the penalty or ignored it entirely. | 6 |
| 6 | Calculates the Final Amount Owed as **$160.90**. | **Critical** | Prompt Text & PDF | $39.00 (Adjusted Base) \+ $92.50 (Gap) \+ $29.40 (LEP) \= $160.90. | FALSE | **Model Failure:** Mathematical breakdown failed due to cumulative errors in extraction or multi-step logic. | None |

#### ---

**4\. Model Analysis**

This prompt is designed to trigger a "Cascade Failure" in LLMs by stacking three different types of reasoning:

* **Tabular Extraction Trap:** The PDF has a 4-column pharmacy grid. The model must correctly ignore "Standard Retail" and pick "Preferred Retail", then ignore "30-day" and pick "90-day".  
* **Sequential Math Trap:** Bulletin A requires a percentage discount followed by a monthly-weighted addition. Bulletin B requires a conditional subtraction from a high-value threshold. Most models will fail to execute these in the correct order.  
* **Accumulator Memory Trap:** The model must manage **three separate buckets of money**: the member's plan copay, the member's share of the gap cost, and the member's legislative penalty (LEP). It must understand that while the LEP is a penalty, it is collected *today* as part of the point-of-sale total.

#### ---

**5\. Golden Response**

**Member Cost-Sharing Breakdown: CardioMax (90-Day Supply)**

**1\. Base Copay Extraction & Bulletin A Adjustments**

3. **PDF Extraction:** According to Page 11 of the 2025 Summary of Benefits, the base copay for a Tier 3 drug for a 31-90 day supply at a **Preferred Retail Pharmacy** is **$25.00**.  
4. **Bulletin A Discount:** Applying the 10% Preferred Network Discount: $25.00 x 0.90 \= **$22.50**.  
5. **Bulletin A Access Fee:** Adding the $5.50 monthly fee for a 3-month supply: $5.50 x 3 \= **$16.50**.  
6. **Adjusted Base Copay Total:** $22.50 \+ $16.50 \= **$39.00**.

**2\. Phase Transition (Coverage Gap Check)**

* Member's Current Total Drug Costs (TDC): $4,800.00.  
* Full Retail Cost of today's fill: $600.00.  
* New Projected TDC: $4,800 \+ $600 \= $5,400.00.  
* The 2025 Initial Coverage Limit (ICL) is $5,030.00. Since $5,400 exceeds this, the fill is a **Straddle Claim**.

**3\. Gap Overage Calculation (Bulletin B)**

* Amount of drug cost falling into the Coverage Gap: $5,400 \- $5,030 \= $370.00.  
* Member's 25% share of Gap overage: 25% of $370.00 \= **$92.50**.

**4\. Late Enrollment Penalty (LEP) Calculation**

* The pharmacy is required to collect the $9.80 monthly penalty for the duration of the 3-month supply.  
* Total LEP: $9.80 x 3 \= **$29.40**.

**5\. Final Amount Owed Today**

* Adjusted Base Copay: $39.00  
* Gap Overage Cost: $92.50  
* LEP Penalty: $29.40  
* **TOTAL DUE AT PHARMACY: $160.90**

### 

---

### **Example 40: Inland Marine (The Tartarus Tier \- Split-Track Coinsurance & Aviation Exclusions)**

#### **1\. Metadata**

* **Task Type:** Workflow / Multi-Constraint Reasoning  
* **Category / Domain:** Claims Handling (Inland Marine)  
* **Workflow:** Split-Track Adjudication, Aggregate ACV Conversions, Implicit Exclusions  
* **Prompt Type:** Claims Apportionment Memo  
* **Difficulty:** Tartarus Tier (Expected Failure Rate: \>99%)

#### **2\. Prompt**

You are a Senior Commercial Property Claims Adjuster. The current date is **March 10, 2024**.

**Task:** Adjudicate a complex theft claim. You must read the provided Claim Data, apply the Policy Language Excerpts, and rely on standard ISO/Selective Contractors Equipment form logic (CM 71 97 03 15\) via the URL provided. You must calculate "Scheduled Equipment" and "Newly Acquired Equipment" on two completely separate mathematical tracks. Show your step-by-step math.

**\--- CLAIM DATA \---**

* **Total Scheduled Inventory:** The insured's scheduled equipment has a stated Replacement Cost Value (RCV) of $200,000.  
* **Aggregate Depreciation Factor:** The entire scheduled inventory has an average aggregate depreciation of 25%.  
* **Policy Limits:** The insured carries a Blanket Limit of $60,000.  
* **Stolen Items (all stolen in one event):**  
  * **Skid Steer (Scheduled):** $20,000 RCV. Age: 6 years.  
  * **Surveying Drone (Scheduled):** $15,000 RCV. Age: 1 year. (Used for site mapping).  
  * **Excavator (Newly Acquired):** Purchased 15 days ago for $25,000. Not yet added to the schedule.

**\--- POLICY LANGUAGE EXCERPTS \---**

**Section A: Valuation**

Scheduled Equipment 5 years of age or older is valued at Actual Cash Value (ACV). ACV is calculated by deducting 10% of the item's Replacement Cost for every full year of age. Equipment under 5 years of age is valued at RCV.

**Section B: Coinsurance (Applies to Scheduled Equipment ONLY)**

You must maintain a Blanket Limit equal to at least 80% of your Total Inventory **ACV**. If you do not, we pay only the proportion your carried limit bears to the required limit. *Crucial Rule: Do not include Newly Acquired property in the Coinsurance base or penalty application.*

**Section C: Newly Acquired Property Extension**

Equipment purchased within the last 30 days is covered up to a maximum sub-limit of $10,000. This is adjudicated separately from scheduled equipment.

**Section D: Deductibles**

* **Standard Deductible:** $1,000 per occurrence (applies to the final adjusted scheduled loss).  
* **Newly Acquired Deductible:** Property claimed under Section C is subject to a separate, standalone deductible equal to 5% of its *original purchase price*.

**Attached Reference (Context):**

* CM7197.201503.PDF – Public URL: [https://home7.selectiveinsurance.com/FormsPDF/CM/CM7197.201503.PDF](https://home7.selectiveinsurance.com/FormsPDF/CM/CM7197.201503.PDF) (Note: You must enforce standard form Property Not Covered exclusions regarding aircraft/watercraft).

**Output Requirements:**

Provide a Claims Apportionment Memo calculating:

* The Coinsurance Penalty Ratio.  
* The Scheduled Equipment Net Payable.  
* The Newly Acquired Net Payable.  
* The Final Combined Payable Amount.

#### ---

**3\. Rubric (Negative Failure Focus)**

| \# | Description | Weight | Sources | Justification | Met | Failure Reasoning (Model Failure Example) | Dependent Criteria |
| :---- | :---- | :---- | :---- | :---- | :---- | :---- | :---- |
| 1 | Derives Coinsurance Ratio as 50%. | **Critical** | Prompt Text | Inventory RCV is $200k. Aggregate ACV is $200k \* (1 \- 0.25) \= $150k. Required Limit is 80% of $150k \= $120k. Carried ($60k) / Required ($120k) \= 0.50 (50%). | FALSE | **Model Failure:** Calculated ratio based on the RCV ($60k / $160k \= 37.5%), failing to convert the base to ACV first. | 4 |
| 2 | Values the Skid Steer at $8,000. | **Critical** | Prompt Text (Sec A) | Age is 6 years. 6 years \* 10% \= 60% depreciation. $20,000 \* 0.40 \= $8,000. | FALSE | **Model Failure:** Took 10% flat ($18,000) or depreciated it all the way to zero. | 4 |
| 3 | Excludes the Surveying Drone ($0 allowed). | **Critical** | CM 71 97 PDF | Section B (Property Not Covered) of the CM 71 97 form explicitly excludes Aircraft/Watercraft. A drone is an aircraft. | FALSE | **Model Failure:** Hallucinated coverage for the drone and valued it at $15,000 (RCV), destroying the entire scheduled loss math. | 4 |
| 4 | Calculates Scheduled Net Payable as $3,000. | **Critical** | Prompt Text | Gross Loss \= $8,000. Coinsurance \= $8,000 \* 0.50 \= $4,000. Minus $1,000 standard deductible \= $3,000. | FALSE | **Model Failure:** Math cascaded due to including the drone, failing the ratio, or applying the deductible before coinsurance. | 7 |
| 5 | Caps the Newly Acquired Excavator at $10,000. | **Major** | Prompt Text (Sec C) | Sub-limit caps the $25,000 purchase at $10,000. | FALSE | **Model Failure:** Allowed the full $25,000, or lumped it into the scheduled coinsurance track. | 6 |
| 6 | Calculates Newly Acquired Deductible as $1,250 and Payable as $8,750. | **Critical** | Prompt Text (Sec D) | Deductible is 5% of purchase price ($25,000 \* 0.05 \= $1,250). Payable \= $10,000 cap \- $1,250 ded \= $8,750. | FALSE | **Model Failure:** Calculated 5% of the $10,000 cap ($500), or applied the standard $1k deductible to this item instead. | 7 |
| 7 | Calculates Final Combined Payable Amount as $11,750. | **Critical** | Prompt Text | $3,000 (Scheduled) \+ $8,750 (Newly Acquired) \= $11,750. | FALSE | **Model Failure:** Complete collapse of latent logic; tracks merged, exclusions missed, or deductions crossed. | None |

#### ---

**4\. Model Analysis**

This prompt acts as a literal "Hallucination Engine."

7. **The Excluded Drone Trap:** The prompt explicitly mentions a drone and gives it a value. Standard LLM behavior is to utilize every number provided. However, the prompt references the CM7197 URL and instructs the model to enforce standard exclusions. Aircraft (drones) are excluded under Inland Marine unless specifically endorsed. 99% of LLMs will hallucinate coverage for the drone and add $15k to the loss.  
8. **The Split-Track Deductible:** LLMs struggle to hold multiple distinct deductibles in their working memory. The model must apply a flat $1,000 to Track A, and an algorithmic deductible (5% of $25k \= $1,250) to Track B. Models will almost always cross-contaminate these, either applying $1,000 to the total, or taking 5% of the $10k cap instead of the $25k purchase price.  
9. **The Base ACV Trap:** The Coinsurance requirement is 80% of ACV, but the inventory is provided in RCV. The model must perform a pre-calculation ($200k \* 0.75 \= $150k) before it can even find the denominator for its coinsurance fraction. If it uses $200k, the ratio becomes 37.5%, corrupting the entire claim.

#### ---

**5\. Golden Response (Mathematically & Factually Verified)**

**CLAIMS APPORTIONMENT MEMORANDUM**

**TO:** Inland Marine Claims File

**DATE:** March 10, 2024

**SUBJECT:** Contractors Equipment Theft \- Split-Track Calculation

**PART I: COINSURANCE PENALTY RATIO (Scheduled Equipment)**

Per Section B, the insured must carry a limit equal to 80% of the Total Inventory ACV.

* **Total Inventory RCV:** $200,000  
* **Aggregate ACV Conversion:** $200,000 \* (1 \- 0.25) \= $150,000  
* **Required Limit (80% of ACV):** $150,000 \* 0.80 \= $120,000  
* **Actual Limit Carried:** $60,000  
* **Coinsurance Ratio:** $60,000 / $120,000 \= **0.50 (50%)**

**PART II: TRACK A \- SCHEDULED EQUIPMENT PAYABLE**

* **Skid Steer:** Age 6 requires ACV valuation. 6 years \= 60% depreciation. $20,000 \* (1 \- 0.60) \= **$8,000**.  
* **Surveying Drone:** Denied. Per the "Property Not Covered" section of the standard CM 71 97 coverage form, aircraft (including UAVs/drones) are strictly excluded from coverage. **$0 allowed.**  
* **Gross Scheduled Loss:** $8,000  
* **Coinsurance Application:** $8,000 \* 0.50 ratio \= $4,000.  
* **Standard Deductible:** $4,000 \- $1,000 \= **$3,000**.  
* **Net Scheduled Payable: $3,000**

**PART III: TRACK B \- NEWLY ACQUIRED PROPERTY PAYABLE**

* **Excavator:** Purchased for $25,000. Per Section C, this is capped at the maximum sub-limit of **$10,000**. (Exempt from Coinsurance penalty).  
* **Newly Acquired Deductible:** Per Section D, the deductible is 5% of the original purchase price. $25,000 \* 0.05 \= **$1,250**.  
* **Net Newly Acquired Payable:** $10,000 \- $1,250 \= **$8,750.**

**PART IV: FINAL COMBINED PAYABLE AMOUNT**

* Scheduled Track Payable: $3,000  
* Newly Acquired Track Payable: $8,750  
* **Final Combined Payable Amount: $11,750**

### 

### 

### 

### **Example 44: The Abyssal Tier Trap (Inter-Domain Dependency & Cross-State Co-Insurance)**

#### **1\. Metadata**

* **Task Type:** Workflow / Multi-Constraint Reasoning  
* **Category / Domain:** Commercial Property (Underwriting & Claims)  
* **Workflow:** Implicit Pricing, Ghost Aggregation, and Cross-Domain Deductible Triggers  
* **Prompt Type:** Comprehensive Account Review  
* **Difficulty:** Abyssal Tier (Expected Failure Rate: \>95%)

#### **2\. Prompt**

You are a Senior Commercial Property Specialist handling both Underwriting and Claims.

**Context:** You must process a renewal quote AND adjudicate a hypothetical loss for "Omega Logistics" effective 01/01/2024. You must rely ONLY on the text below to derive your math and logic. Do not invent formulas or use outside knowledge.

**Account Data Table:**

Loc\_1: Occupancy: Office | Insured Limit: $600,000 | Actual Cash Value (ACV): $800,000 | Replacement Cost (RCV): $900,000 | Sq Footage: 5,000

Loc\_2: Occupancy: Warehouse | Insured Limit: $400,000 | Actual Cash Value (ACV): $500,000 | Replacement Cost (RCV): $600,000 | Sq Footage: 10,000

Loc\_3: Occupancy: Manufacturing | Insured Limit: $1,200,000 | Actual Cash Value (ACV): $1,200,000 | Replacement Cost (RCV): $1,600,000 | Sq Footage: 20,000

Loc\_4: Occupancy: Vacant Land | Insured Limit: $0 (Coverage Excluded) | Actual Cash Value (ACV): $0 | Replacement Cost (RCV): $0 | Sq Footage: 50,000

**Underwriting Guidelines (Text Extracts):**

**Base Rating:** Premiums are calculated per $100 of the Insured Limit. Office is 0.20. Warehouse is 0.50. Manufacturing is 1.00.

**Blanket Limit Surcharge:** If multiple locations share a "Blanket" limit, you must sum their Actual Cash Values (ACV). If this combined ACV strictly exceeds $1,000,000, apply a 1.15 multiplier to the base premium of those specific locations. *Note: Loc\_1 and Loc\_2 are on a Blanket Limit. Loc\_3 is not.*

**Minimum Premium Floor:** Any covered location whose calculated premium falls below $1,500 must be overridden and bumped to a strict $1,500 Minimum Premium.

**Mega-Risk Surcharge:** If the total square footage of the entire property deed (including excluded or vacant locations) exceeds 80,000 sq ft, a flat $5,000 Mega-Risk Fee must be added to the absolute final account premium.

**Claims Guidelines (Text Extracts for Adjudication):**

**Co-Insurance Clause:** All locations are subject to an 80% Co-Insurance clause. To determine the payable amount for a loss, divide the Insured Limit by the required insurance amount (which is the location's Valuation Multiplicand multiplied by the 80% co-insurance requirement). Multiply the actual loss by this fraction to get the adjusted loss.

**Valuation Multiplicand Rule:** For "Office" and "Warehouse", use ACV for co-insurance math. For "Manufacturing", you must use RCV.

**Deductible Dependency Override:** The standard deductible is $1,000. **HOWEVER**, if the specific location where the claim occurred had its underwriting premium bumped to the $1,500 Minimum Premium Floor, that location becomes a "Sub-Standard Risk." Its deductible is doubled to $2,000. Subtract the deductible as the absolute final step.

**Task 1: Premium Calculation**

Calculate the final premium for the covered locations (Loc\_1, Loc\_2, Loc\_3).

Output a Markdown table: | location\_id | base\_premium | blanket\_surcharge\_applied (Y/N) | min\_prem\_applied (Y/N) | final\_loc\_premium |

**Task 2: Claims Adjudication**

Hypothetical Scenario: A fire at **Loc\_1** causes exactly $100,000 in damages.

Using the Claims Guidelines text, adjudicate this claim. Show your step-by-step math.

Output exactly: "Final Payable Claim Amount: $\[Value\]"

**Task 3: Final Account Premium**

Calculate the Total Account Premium for the renewal. Output exactly: "Final Total Account Premium: $\[Value\]"

#### ---

**3\. Rubric (Negative Failure Focus)**

| \# | Description | Weight | Sources | Justification | Met | Failure Reasoning (Model Failure Example) | Dependent Criteria |
| :---- | :---- | :---- | :---- | :---- | :---- | :---- | :---- |
| 1 | Derives Base Premiums & applies Blanket Surcharge correctly. | **Critical** | Prompt Text | Loc 1 Base: ($600k/100)\*0.20 \= $1,200. Blanket ACV ($800k+$500k \= $1.3M) \> $1M. Blanket applies. $1,200 \* 1.15 \= $1,380. | FALSE | **Model Failure:** Multiplied raw limit by rate, or failed to combine ACVs to trigger the 1.15 surcharge. | 2, 6 |
| 2 | Triggers Minimum Premium Floor for Loc\_1. | **Critical** | Prompt Text | Loc\_1 indicated premium is $1,380. Because $1,380 \< $1,500, it must be bumped to $1,500. | FALSE | **Model Failure:** Ignored the Minimum Premium rule, leaving Loc\_1 at $1,380. | 4, 6 |
| 3 | Derives Co-Insurance Ratio correctly (0.9375). | **Major** | Prompt Text | Loc\_1 is Office, so Valuation is ACV ($800k). Required \= $800k \* 0.80 \= $640k. Fraction \= $600k / $640k \= 0.9375. | FALSE | **Model Failure:** Used RCV ($900k) as the distractor, or just divided Limit by ACV ($600k/$800k). | 5 |
| 4 | Triggers Inter-Domain Deductible Override ($2,000). | **Critical** | Prompt Text | Because Loc\_1 triggered the Minimum Premium Floor in Task 1, its deductible is doubled to $2,000 in Task 2\. | FALSE | **Model Failure:** Context collapse. The model forgot the result of Task 1 while executing Task 2 and applied the standard $1,000 deductible. | 5 |
| 5 | Calculates Final Payable Claim Amount as $91,750. | **Critical** | Prompt Text | Loss \= $100,000 \* 0.9375 \= $93,750. Minus $2,000 Deductible Override \= $91,750. | FALSE | **Model Failure:** Math cascaded due to missing the deductible override or failing the co-insurance algebraic extraction. | None |
| 6 | Calculates Final Total Account Premium as $20,800. | **Critical** | Prompt Text | Loc 1 ($1,500) \+ Loc 2 ($2,300) \+ Loc 3 ($12,000) \+ Mega-Risk Fee ($5,000 for 85k sq ft) \= $20,800. | FALSE | **Model Failure:** Left Loc\_1 at $1,380, or discarded Loc 4's 50,000 sq ft and missed the $5,000 Mega-Risk surcharge. | None |

#### ---

**4\. Model Analysis**

This prompt executes a fatal **"Inter-Domain State Dependency"** trap.

Standard LLMs process tasks sequentially and in isolation. When the model reaches Task 2 (Claims Adjudication), its attention mechanism is focused entirely on the Claims Guidelines. It sees "Standard Deductible: $1,000" and locks onto it. To succeed, the model must recognize the conditional statement ("if the specific location... had its underwriting premium bumped..."), query its own generated output from Task 1, recognize that the condition evaluated to True for Loc\_1 ($1,380 bumped to $1,500), and dynamically override the $1,000 deductible with $2,000.

Furthermore, the model must fend off the **Valuation Decoy** (the prompt provides RCV values for all locations, but restricts their use to Manufacturing only) and the **Ghost Data Trap** (Loc\_4 must be entirely ignored for premium math, but resurrected to trigger the 80k sq ft Mega-Risk surcharge).

#### ---

**5\. Golden Response**

**Task 1: Premium Calculation**

| location\_id | base\_premium | blanket\_surcharge\_applied (Y/N) | min\_prem\_applied (Y/N) | final\_loc\_premium |
| :---- | :---- | :---- | :---- | :---- |
| Loc\_1 | $1,200.00 | Y | Y | $1,500.00 |
| Loc\_2 | $2,000.00 | Y | N | $2,300.00 |
| Loc\_3 | $12,000.00 | N | N | $12,000.00 |

*(Note: Loc\_1 and Loc\_2 combined ACV is $1,300,000, triggering the 1.15 Blanket Surcharge. Loc\_1 indicated premium of $1,380 was raised to the $1,500 minimum floor).*

**Task 2: Claims Adjudication (Loc\_1)**

* **Valuation Multiplicand:** Because Loc\_1 is an Office, ACV is used ($800,000).  
* **Required Insurance:** ACV ($800,000) \* 80% \= $640,000.  
* **Co-Insurance Fraction:** Insured Limit ($600,000) / Required Insurance ($640,000) \= 0.9375.  
* **Adjusted Loss:** Actual Loss ($100,000) \* 0.9375 \= $93,750.  
* **Deductible Override:** Because Loc\_1 was bumped to the $1,500 minimum premium floor in Task 1, the standard deductible is doubled to $2,000.  
* **Final Payable Claim Calculation:** $93,750 \- $2,000 \= $91,750.

Final Payable Claim Amount: $91,750

**Task 3: Final Account Premium**

* **Covered Location Premium:** $1,500 \+ $2,300 \+ $12,000 \= $15,800.  
* **Mega-Risk Fee:** Total square footage across all 4 locations is 85,000 (including Loc\_4). Because this exceeds 80,000 sq ft, the $5,000 flat fee is applied.

Final Total Account Premium: $20,800

### ---

**Example 41: Medicare Advantage Rx (The Preferred Network & LEP Straddle Trap)** 

#### **1\. Metadata**

* **Task Type:** Workflow / Multi-Constraint Reasoning  
* **Category / Domain:** Agent/Broker (Medicare)  
* **Workflow:** Cross-Document Extraction, Legislative Override & Penalty Accumulation  
* **Prompt Type:** Member Inquiry Calculation  
* **Difficulty:** Nightmare (Expected Failure Rate: \>90%)

#### **2\. Prompt**

You are a Senior Medicare Benefits Counselor. The current date is **October 24, 2025**.

**Task:** A member is at the pharmacy counter right now and needs to know their exact out-of-pocket cost for today's refill. You must extract the base copay from the attached 2025 Summary of Benefits, apply the network and penalty overrides provided below, and calculate the final exact dollar amount they owe.

**\--- MEMBER PROFILE & CLAIM DATA \---**

* **Drug Requested Today:** "CardioMax" (Classification: **Tier 3** \- Preferred Brand).  
* **Fill Request:** 90-day supply.  
* **Pharmacy Type:** **Preferred Retail Pharmacy**.  
* **Full Retail Cost of Drug (Negotiated Price):** $600.  
* **Member's Current Total Drug Costs (TDC) Year-to-Date:** $4,800.  
* **Member File Note:** "Member has a Part D Late Enrollment Penalty (LEP) of $9.80 per month. The pharmacy collects this monthly penalty at the point of sale for the duration of the fill supply."

**\--- SPECIAL UNDERWRITING BULLETINS (MANDATORY OVERRIDES) \---**

* **Bulletin A (Preferred Network Discount):** If a member uses a Preferred Retail Pharmacy for a 90-day supply, apply a 10% discount to the **Base Copay** found in the Summary of Benefits. However, the pharmacy must add a flat **$5.50 "Network Access Fee"** for every 30 days of the supply (calculated after the 10% discount).  
* **Bulletin B (The Straddle Rule):** The 2025 Initial Coverage Limit (ICL) is **$5,030**. If a fill pushes the member's TDC above this limit, the member must pay **25%** of the specific portion of the drug's Full Retail Cost that falls within the Coverage Gap (the amount exceeding $5,030), in addition to their adjusted base copay.

**Attached Files (Context):**

* MEDM104X11289MCA4242025GRMASBMedicalMutualAdvantagePlusFA.pdf

**Output Requirements:**

Provide a step-by-step breakdown including:

* Base Copay extraction (citing the PDF column).  
* Bulletin A adjustments (discount and access fees).  
* The TDC Accumulator and ICL spillover calculation.  
* The LEP penalty total.  
* The final absolute dollar amount the member must pay.

#### ---

**3\. Rubric (Negative Failure Focus)**

| \# | Description | Weight | Sources | Justification | Met | Failure Reasoning (Model Failure Example) | Dependent Criteria |
| :---- | :---- | :---- | :---- | :---- | :---- | :---- | :---- |
| 1 | Extracts the base copay as **$50.00**. | **Critical** | Attached PDF | Model must navigate to Tier 3 and specifically select the "**Preferred Retail Pharmacy**" / "**31-90 day**" column. | FALSE | **Model Failure:** Pulled the Standard Retail copay ($111), the 30-day copay ($42), or the Tier 2 copay ($25). | 2 |
| 2 | Applies 10% Discount and $16.50 Access Fee. | **Critical** | Prompt Text (Bulletin A) | ($50.00 \* 0.90) \= $45.00. Then add ($5.50 \* 3 months) \= $16.50. Total adjusted base \= $61.50. | FALSE | **Model Failure:** Added the fee before the discount, or only added one month of the fee ($5.50). | 5 |
| 3 | Identifies the claim crosses the ICL ($5,030). | **Major** | Prompt Text | $4,800 \+ $600 \= $5,400. This is $370 over the ICL. | FALSE | **Model Failure:** Failed to use the full retail price ($600) to check the accumulator. | 4 |
| 4 | Calculates the precise Gap Overage as **$92.50**. | **Critical** | Prompt Text (Bulletin B) | 25% of the $370 spillover \= $92.50. | FALSE | **Model Failure:** Calculated 25% of the entire $600 drug cost ($150). | 5 |
| 5 | Calculates and adds 3 months of LEP (**$29.40**). | **Major** | Prompt Text | $9.80 \* 3 months \= $29.40. | FALSE | **Model Failure:** Only added one month of the penalty or ignored it entirely. | 6 |
| 6 | Calculates the Final Amount Owed as **$183.40**. | **Critical** | Prompt Text & PDF | $61.50 (Adjusted Base) \+ $92.50 (Gap) \+ $29.40 (LEP) \= $183.40. | FALSE | **Model Failure:** Mathematical breakdown failed due to cumulative errors in extraction or multi-step logic. | None |

#### ---

**4\. Model Analysis**

This prompt is designed to trigger a "Cascade Failure" in LLMs by stacking three different types of reasoning:

* **Tabular Extraction Trap:** The PDF has a complex pharmacy grid. The model must correctly ignore "Standard Retail" and pick "Preferred Retail", then ignore "30-day" and pick "90-day", and importantly, stay on the correct "Tier 3" row to extract **$50.00** (avoiding the human-error trap of grabbing $25 from Tier 2).  
* **Sequential Math Trap:** Bulletin A requires a percentage discount followed by a monthly-weighted addition. Bulletin B requires a conditional subtraction from a high-value threshold. Most models will fail to execute these in the correct order.  
* **Accumulator Memory Trap:** The model must manage **three separate buckets of money**: the member's plan copay, the member's share of the gap cost, and the member's legislative penalty (LEP). It must understand that while the LEP is a penalty, it is collected *today* as part of the point-of-sale total.

#### ---

**5\. Golden Response**

**Member Cost-Sharing Breakdown: CardioMax (90-Day Supply)**

**1\. Base Copay Extraction & Bulletin A Adjustments**

10. **PDF Extraction:** According to the Retail Pharmacy Cost Sharing table in the 2025 Summary of Benefits, the base copay for a Tier 3 drug for a 31-90 day supply at a **Preferred Pharmacy** is **$50.00**.  
11. **Bulletin A Discount:** Applying the 10% Preferred Network Discount: $50.00 x 0.90 \= **$45.00**.  
12. **Bulletin A Access Fee:** Adding the $5.50 monthly fee for a 3-month (90-day) supply: $5.50 x 3 \= **$16.50**.  
13. **Adjusted Base Copay Total:** $45.00 \+ $16.50 \= **$61.50**.

**2\. Phase Transition (Coverage Gap Check)**

* Member's Current Total Drug Costs (TDC): $4,800.00.  
* Full Retail Cost of today's fill: $600.00.  
* New Projected TDC: $4,800 \+ $600 \= $5,400.00.  
* The 2025 Initial Coverage Limit (ICL) is $5,030.00. Since $5,400 exceeds this, the fill is a **Straddle Claim**.

**3\. Gap Overage Calculation (Bulletin B)**

* Amount of drug cost falling into the Coverage Gap: $5,400 \- $5,030 \= $370.00.  
* Member's 25% share of Gap overage: 25% of $370.00 \= **$92.50**.

**4\. Late Enrollment Penalty (LEP) Calculation**

* The pharmacy is required to collect the $9.80 monthly penalty for the duration of the 3-month supply.  
* Total LEP: $9.80 x 3 \= **$29.40**.

**5\. Final Amount Owed Today**

* Adjusted Base Copay: $61.50  
* Gap Overage Cost: $92.50  
* LEP Penalty: $29.40  
* **TOTAL DUE AT PHARMACY: $183.40**