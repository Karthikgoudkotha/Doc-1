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

1. The Manufacturing Override: Manufacturing is normally an automatic Decline (Rule O.1). However, if a Manufacturing location is BOTH "Fully Sprinklered" (Y) AND has a Protection Class of 4 or better, its disposition changes to "Accept subject to Surcharge".  
   * Math Impact: For these specific locations, you MUST hardcode Mod A (Year Built) to 1.50, regardless of the actual year built.  
2. The Small Office Surcharge: If an "Office" location has a building value STRICTLY LESS THAN $200,000, it is subject to a flat $150 policy fee.  
   * Math Impact: This $150 fee must be added to the final\_premium ONLY AFTER the minimum premium rule has been checked and applied.  
3. Portfolio Concentration Rule: If the sum of the building\_value\_USD across ALL submitted locations (including Declines) exceeds $2,500,000, you must apply a 1.10 Portfolio Surcharge to the final Total Account Premium.

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

### **Example 39: Medicare Advantage Rx (The 2025 Straddle Claim Trap)**

#### **1\. Metadata**

* **Task Type:** Workflow / Multi-Constraint Reasoning  
* **Category / Domain:** Agent/Broker (Medicare)  
* **Workflow:** Document Extraction & Implicit Actuarial Math  
* **Prompt Type:** Member Inquiry Calculation  
* **Difficulty:** God Tier (Expected Failure Rate: \>85%)

#### **2\. Prompt**

You are a Senior Medicare Benefits Counselor. The current date is **October 24, 2025**.

**Task:** A member on the MedMutual Advantage Plus plan is at the pharmacy counter right now and needs to know their exact out-of-pocket cost for today's refill. You must extract their base copay from the attached 2025 Summary of Benefits, apply the Coverage Gap rules provided below, and calculate the exact dollar amount they owe today.

**\--- MEMBER PROFILE & CLAIM DATA \---**

* **Drug Requested Today:** "CardioMax" (Classification: Tier 3 \- Preferred Brand).  
* **Fill Request:** 90-day supply (31-90 day tier).  
* **Pharmacy Type:** Standard Retail Pharmacy.  
* **Full Retail Cost of Drug (Negotiated Price):** $600.  
* **Member's Current Total Drug Costs (TDC) Year-to-Date:** $4,600.

**\--- 2025 COVERAGE GAP RULES (MANDATORY MATH) \---**

Standard Medicare Advantage plans have an Initial Coverage Limit (ICL). For 2025, the ICL is **$5,030**.

If a single prescription fill crosses the $5,030 limit and pushes the member into the Coverage Gap, this is known as a **"Straddle Claim."** To calculate the member's final out-of-pocket cost for a Straddle Claim, you must sum the following two amounts:

1. **The Base Copay:** Extract the exact standard copay for this specific tier and days' supply directly from the attached Summary of Benefits.  
2. **PLUS The Gap Overage:** Calculate the portion of the drug's Full Retail Cost that pushes the Total Drug Costs *above* the $5,030 Initial Coverage Limit. The member must pay **25%** of this overage amount.

**Attached Files (Context):**

* MEDM104X11289MCA4242025GRMASBMedicalMutualAdvantagePlusFA.pdf

**Output Requirements:**

Output a structured breakdown of the base copay extraction, the phase transition calculation, and the final exact amount owed by the member today. Do not include conversational filler.

#### **3\. Rubric (Negative Failure Focus)**

| \# | Description | Weight | Sources | Justification | Met | Failure Reasoning (Model Failure Example) | Dependent Criteria |
| :---- | :---- | :---- | :---- | :---- | :---- | :---- | :---- |
| 1 | Extracts the base copay as $37. | **Critical** | Attached PDF (Page 11\) | Model must navigate the PDF table to find Tier 3, then navigate horizontally to the 31-90 day supply column. | FALSE | **Model Failure:** Pulled the 30-day supply copay ($15), or multiplied the 30-day copay by 3 ($45) instead of reading the 90-day column. | 4 |
| 2 | Correctly determines the claim crosses the Initial Coverage Limit (ICL). | **Major** | Prompt Text | Current TDC ($4,600) \+ Drug Cost ($600) \= $5,200. This is strictly greater than the $5,030 ICL. | FALSE | **Model Failure:** Only looked at the base copay and failed to add the $600 full cost to the $4,600 accumulator to check the threshold. | 3, 4 |
| 3 | Calculates the precise Gap Overage amount as $42.50. | **Critical** | Prompt Text | $5,200 (New TDC) \- $5,030 (ICL) \= $170 in the gap. 25% of $170 \= $42.50. | FALSE | **Model Failure:** Took 25% of the *entire* $600 drug cost ($150), or 25% of the $5,030 limit. | 4 |
| 4 | Calculates the Final Amount Owed as $79.50. | **Critical** | Prompt Text & PDF | Base Copay ($37) \+ Gap Overage Cost ($42.50) \= $79.50. | FALSE | **Model Failure:** Math cascaded out of control due to extracting the wrong base copay from the PDF or failing the accumulator logic. | None |

#### **4\. Model Analysis**

This prompt is designed to break LLMs by forcing them to execute **cross-modal reasoning**.

First, the model must successfully parse a complex, multi-column PDF table to extract the exact $37 value (a task LLMs frequently fail by grabbing the first number in the row, which is the 30-day supply cost of $15).

Second, it must seamlessly switch from *document extraction* to *implicit algorithmic math*. The prompt does not give the formula (Current TDC \+ Drug Cost) \- ICL. The model must linguistically deduce that $4,600 \+ $600 \= $5,200, realize this is $170 over the $5,030 limit, calculate 25% of $170, and add it back to the $37 it found in the PDF. LLMs typically fail to hold the extracted PDF value in their working memory while executing the multi-step algebraic word problem.

#### **5\. Golden Response**

**Member Cost-Sharing Breakdown: CardioMax (90-Day Supply)**

**1\. Base Copay Extraction**

* According to the attached 2025 Summary of Benefits, the standard copay for a Tier 3 drug at a 31-90 day supply is **$37.00**.

**2\. Phase Transition (Coverage Gap Check)**

* Member's Current Total Drug Costs (TDC): $4,600.  
* Full Retail Cost of requested drug: $600.  
* New Projected TDC: $4,600 \+ $600 \= $5,200.  
* The new TDC ($5,200) exceeds the 2025 Initial Coverage Limit of $5,030. This triggers a Straddle Claim.

**3\. Gap Overage Calculation**

* Portion of the drug cost falling *above* the ICL: $5,200 \- $5,030 \= $170.  
* Under the Straddle Claim rules, the member pays 25% of this overage.  
* Gap Overage Cost: 25% of $170 \= **$42.50**.

**4\. Final Exact Amount Owed Today**

* Base Copay ($37.00) \+ Gap Overage Cost ($42.50) \= **$79.50**.  
* 

---

### **Example 40: Inland Marine (The Coinsurance & Unsecured Job Site Trap)**

#### **1\. Metadata**

* **Task Type:** Workflow / Multi-Constraint Reasoning  
* **Category / Domain:** Claims Handling (Inland Marine)  
* **Workflow:** Implicit Math, Coinsurance Penalties & Deductible Overrides  
* **Prompt Type:** Claims Apportionment Memo  
* **Difficulty:** God Tier (Expected Failure Rate: \>85%)

#### **2\. Prompt**

You are a Commercial Property Claims Adjuster. The current date is January 5, 2023\.

**Task:** Your insured had their job site burglarized overnight. Calculate the final payable amount for the claim. You must read the provided Policy Language Excerpts below, deduce the correct mathematical formulas, and apply them. Show your step-by-step math.

**\--- CLAIM DATA \---**

* **Total Inventory Value:** The insured's total schedule of equipment across all their active jobs is valued at $50,000.  
* **Policy Limits:** The insured carries a Blanket Policy Limit of $25,000.  
* **Theft Details:** Thieves broke into an *unfenced* job site at 2:00 AM.  
* **Stolen Items:**  
  1. Skid Steer Loader (Owned by insured): $12,000 replacement value.  
  2. Portable Generator (Owned by insured): $3,000 replacement value.  
  3. Foreman's Personal Tools: $4,000 replacement value.

**\--- POLICY LANGUAGE EXCERPTS \---**

**Section A: Sub-Limits**

The most we will pay in any one occurrence for loss or damage to *any one item* of Contractor's Equipment is $5,000. Separately, the most we will pay for any one item of personal property of employees (including employee tools) is $2,500.

**Section B: Coinsurance Clause**

You must maintain a Blanket Policy Limit equal to at least 80% of your Total Inventory Value. If your carried limit is less than this 80% requirement, we will penalize you. We will only pay the proportion that your carried limit bears to the required 80% limit. *Note: This proportional penalty must be applied to the Gross Eligible Loss (after sub-limits are applied, but before the deductible).*

**Section C: Deductibles**

The standard per-occurrence deductible is $500. **Security Endorsement:** If a theft occurs after business hours from a job site that does not have a locked, fenced enclosure, the standard deductible is increased by an additional $1,000.

**Output Requirements:**

Provide a Claims Apportionment Memo detailing the sub-limit caps, the coinsurance math, the deductible application, and the final net payable amount.

#### **3\. Rubric (Negative Failure Focus)**

| \# | Description | Weight | Sources | Justification | Met | Failure Reasoning (Model Failure Example) | Dependent Criteria |
| :---- | :---- | :---- | :---- | :---- | :---- | :---- | :---- |
| 1 | Applies the $5,000 per-item cap to the Skid Steer. | **Critical** | Prompt Text (Sec A) | Skid Steer is $12,000. Cap is $5,000. | FALSE | **Model Failure:** Allowed the full $12,000, missing the per-item equipment limit entirely. | 3, 5 |
| 2 | Applies the $2,500 employee tools cap. | **Critical** | Prompt Text (Sec A) | Tools are $4,000. Cap is $2,500. Generator ($3k) is under the $5k cap and is fully allowed. | FALSE | **Model Failure:** Allowed the full $4,000 for tools. | 3, 5 |
| 3 | Calculates the Gross Eligible Loss as $10,500. | **Major** | Prompt Text | $5,000 (Steer) \+ $3,000 (Gen) \+ $2,500 (Tools) \= $10,500. | FALSE | **Model Failure:** Summed the raw amounts ($19k) before applying limits. | 5 |
| 4 | Derives and calculates the exact Coinsurance Penalty Ratio as 62.5%. | **Critical** | Prompt Text (Sec B) | Total value is $50k. 80% of $50k \= $40,000 required limit. Insured carries $25,000. $25k / $40k \= 0.625 (62.5%). | FALSE | **Model Failure:** Calculated 25,000 / 50,000 (50%), or just deducted 20%. LLMs struggle to translate "proportion that your carried limit bears to the required 80% limit" into an algebraic formula. | 5 |
| 5 | Calculates the Loss after Coinsurance as $6,562.50. | **Critical** | Prompt Text (Sec B) | $10,500 \* 0.625 \= $6,562.50. | FALSE | **Model Failure:** Applied the coinsurance penalty *before* the sub-limits, or applied it *after* the deductible, violating the sequence rule in Section B. | 7 |
| 6 | Applies the correct $1,500 Security Deductible. | **Major** | Prompt Text (Sec C) | The theft was overnight at an unfenced site. The base $500 is "increased by an additional $1,000" \= $1,500. | FALSE | **Model Failure:** Just used the standard $500, or used $1,000 instead of $1,500. | 7 |
| 7 | Calculates the Final Payable Amount as $5,062.50. | **Critical** | Prompt Text | $6,562.50 \- $1,500.00 \= $5,062.50. | FALSE | **Model Failure:** Math cascaded out of control due to missing the algebraic coinsurance setup or failing the deductible trigger. | None |

#### **4\. Model Analysis**

This prompt is a death trap for next-token predictors.

First, it requires **Algorithmic Reading Comprehension**. The model must read Section B and independently construct the formula: (Carried Limit / (Total Value \* 0.80)) \* Gross Loss. Standard models will almost always fail to multiply the Total Value by 0.80 *before* creating the ratio, resulting in a 50% penalty instead of the correct 62.5% ratio.

Second, the **Order of Operations** is strictly enforced but counter-intuitive. The model must truncate the sub-limits *first*, apply the hidden ratio *second*, and then catch a conditional deductible override *third*. The language "increased by an additional $1k" forces the model to do $500 \+ $1000 rather than just replacing the deductible with $1000. It requires perfect working memory across 4 distinct mathematical states.

#### **5\. Golden Response**

**CLAIMS APPORTIONMENT MEMORANDUM**

**TO:** Inland Marine Claims File

**DATE:** January 5, 2023

**SUBJECT:** Contractors Equipment Theft \- Coverage Calculation

**1\. Sub-Limit Application (Gross Eligible Loss)**

Per Section A, the following limits apply to the stolen items:

* **Skid Steer ($12,000 value):** Capped at the $5,000 maximum per-item limit for Contractor's Equipment.  
* **Portable Generator ($3,000 value):** Fully allowed, as it is under the $5,000 per-item limit.  
* **Foreman's Tools ($4,000 value):** Capped at the $2,500 maximum limit for employee tools.  
* **Gross Eligible Loss:** $5,000 \+ $3,000 \+ $2,500 \= **$10,500.**

**2\. Coinsurance Penalty Calculation**

Per Section B, the insured must carry a limit equal to 80% of their total inventory value to avoid a penalty.

* **Total Inventory Value:** $50,000  
* **Required Limit (80%):** $40,000  
* **Actual Limit Carried:** $25,000  
* **Coinsurance Ratio:** $25,000 / $40,000 \= **0.625 (62.5%)**

Applying this ratio to the Gross Eligible Loss:

* $10,500 \* 0.625 \= **$6,562.50.**

**3\. Deductible Application**

Per Section C, the standard deductible is $500. However, because the theft occurred at an unfenced job site after hours, the Security Endorsement increases the deductible by an additional $1,000.

* **Applicable Deductible:** $500 \+ $1,000 \= **$1,500.**

**4\. Final Quantum Calculation**

* Coinsurance Adjusted Loss: $6,562.50  
* Less Deductible: ($1,500.00)  
* **Final Net Payable Amount: $5,062.50**

