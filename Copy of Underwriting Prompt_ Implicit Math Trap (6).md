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

4. **Drug Requested Today:** "CardioMax" (Classification: **Tier 3** \- Preferred Brand).  
5. **Fill Request:** 90-day supply.  
6. **Pharmacy Type:** **Preferred Retail Pharmacy**.  
7. **Full Retail Cost of Drug (Negotiated Price):** $600.  
8. **Member's Current Total Drug Costs (TDC) Year-to-Date:** $4,800.  
9. **Member File Note:** "Member has a Part D Late Enrollment Penalty (LEP) of $9.80 per month. The pharmacy collects this monthly penalty at the point of sale for the duration of the fill supply."

**\--- SPECIAL UNDERWRITING BULLETINS (MANDATORY OVERRIDES) \---**

2. **Bulletin A (Preferred Network Discount):** If a member uses a Preferred Retail Pharmacy for a 90-day supply, apply a 10% discount to the **Base Copay** found in the Summary of Benefits. However, the pharmacy must add a flat **$5.50 "Network Access Fee"** for every 30 days of the supply (calculated after the 10% discount).  
3. **Bulletin B (The Straddle Rule):** The 2025 Initial Coverage Limit (ICL) is **$5,030**. If a fill pushes the member's TDC above this limit, the member must pay **25%** of the specific portion of the drug's Full Retail Cost that falls within the Coverage Gap (the amount exceeding $5,030), in addition to their adjusted base copay.

**Attached Files (Context):**

2. MEDM104X11289MCA4242025GRMASBMedicalMutualAdvantagePlusFA.pdf

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

### **Example 40: Inland Marine (The Document Extraction & Coinsurance Singularity)**

#### **1\. Metadata**

* **Task Type:** Workflow / Multi-Constraint Reasoning  
* **Category / Domain:** Claims Handling (Inland Marine)  
* **Workflow:** Cross-Document Extraction, Valuation, Implicit Coinsurance Math  
* **Prompt Type:** Claims Apportionment Memo  
* **Difficulty:** Singularity Tier (Expected Failure Rate: \>99%)

#### **2\. Prompt**

You are a Commercial Property Claims Adjuster. The current date is **April 15, 2026**.

**Task:** Your insured had their job site burglarized overnight. Calculate the final payable amount for the claim. You must read the attached Selective Contractors Equipment Coverage Form (CM 71 97), extract the mandatory per-item caps for both heavy equipment and employee tools, and apply them. Show your step-by-step math.

**\--- CLAIM & POLICY DATA \---**

10. **Total Inventory Value:** The actual value of the insured's total schedule of equipment at the time of loss was **$100,000**.  
11. **Policy Limits:** The insured carries a Blanket Policy Limit of **$50,000**.  
12. **Coinsurance Requirement:** The Declarations page mandates an **80% Coinsurance Clause**.  
13. **Standard Deductible:** **$1,000** per occurrence.

**\--- STOLEN ITEMS \---**

4. **Skid Steer Loader (Owned):** $15,000 Replacement Cost. Age: 3 years.  
5. **Bulldozer (Owned):** $20,000 Replacement Cost. Age: 8 years. (Calculated Actual Cash Value is $8,000).  
6. **Foreman's Personal Tools:** $4,000 Replacement Cost.

**\--- RULES OF ADJUDICATION \---**

3. **Valuation:** Equipment 5 years of age or older is valued at Actual Cash Value (ACV). Equipment under 5 years of age, and all employee tools, are valued at full Replacement Cost (RCV). Valuation must be established *before* checking any policy sub-limits.  
4. **Sub-Limits:** You MUST refer to **Section C (Limits of Insurance)** in the attached coverage form to find the maximum per-item payout for both "Contractors equipment" and "personal property of employees".  
5. **Coinsurance Penalty:** If the insured's Blanket Limit is less than 80% of the Total Inventory Value, a proportional penalty applies. This penalty ratio must be applied to the combined Gross Eligible Loss (which is the sum of all items *after* valuation and sub-limits are applied).  
6. **Deductible:** Subtract the deductible as the absolute final step.

**Attached Files (Context):**

* CM7197.201503.PDF – Public URL: [https://home7.selectiveinsurance.com/FormsPDF/CM/CM7197.201503.PDF](https://home7.selectiveinsurance.com/FormsPDF/CM/CM7197.201503.PDF)

**Output Requirements:**

Provide a Claims Apportionment Memo detailing the PDF extraction, valuation, the sub-limit caps, the coinsurance math, the deductible application, and the final net payable amount.

#### ---

**3\. Rubric (Negative Failure Focus)**

| \# | Description | Weight | Sources | Justification | Met | Failure Reasoning (Model Failure Example) | Dependent Criteria |
| :---- | :---- | :---- | :---- | :---- | :---- | :---- | :---- |
| 1 | Extracts the $5,000 per-item cap for Contractors Equipment. | **Critical** | PDF (Section C.1) | Model must parse the URL/PDF and identify that Section C.1 limits "any one item" of equipment to $5,000. | FALSE | **Model Failure:** Failed to extract the PDF data and hallucinated a limit (or allowed the full $15k/$8k). | 3, 5 |
| 2 | Extracts the $2,500 per-item cap for Employee Tools. | **Critical** | PDF (Section C.2) | Model must parse Section C.2 which limits employee tools to $2,500. | FALSE | **Model Failure:** Allowed the full $4,000 for tools. | 3, 5 |
| 3 | Evaluates and Caps the 3 Stolen Items Correctly. | **Critical** | Prompt & PDF | Skid Steer: RC applies ($15k) \-\> Capped at $5k. Bulldozer: ACV applies ($8k) \-\> Capped at $5k. Tools: RC applies ($4k) \-\> Capped at $2.5k. | FALSE | **Model Failure:** Failed the ACV valuation on the bulldozer and tried to cap $20k, or missed the PDF caps entirely. | 4, 5 |
| 4 | Calculates the Gross Eligible Loss as $12,500. | **Major** | Prompt Text | $5,000 (Steer) \+ $5,000 (Dozer) \+ $2,500 (Tools) \= $12,500. | FALSE | **Model Failure:** Summed the raw amounts ($39k or $27k) before applying limits. | 6 |
| 5 | Derives Coinsurance Penalty Ratio as 0.625 (62.5%). | **Critical** | Prompt Text | Required limit is 80% of $100k \= $80k. Carried is $50k. Ratio is $50k / $80k \= 0.625. | FALSE | **Model Failure:** Divided Limit by Total Value ($50k / $100k \= 50%), completely ignoring the 80% requirement multiplier. | 6 |
| 6 | Calculates the Loss after Coinsurance as $7,812.50. | **Critical** | Prompt Text | $12,500 \* 0.625 \= $7,812.50. | FALSE | **Model Failure:** Applied coinsurance *before* capping the tools/equipment, or *after* the deductible. | 7 |
| 7 | Calculates the Final Payable Amount as $6,812.50. | **Critical** | Prompt Text | $7,812.50 \- $1,000.00 \= $6,812.50. | FALSE | **Model Failure:** Final math collapsed due to failing the PDF extraction, the coinsurance ratio, or the order of operations. | None |

#### ---

**4\. Model Analysis**

This prompt is practically insurmountable for base LLMs because it requires **Cross-Modal Dependency**. The math cannot even begin until the model successfully navigates an external PDF document to extract the hidden variables ($5k and $2.5k).

Once the variables are extracted, the model faces a **Valuation & Truncation Paradox**: It must value the Bulldozer at ACV ($8k) due to age, but then realize that $8k is *still* higher than the extracted $5k PDF cap, truncating it again. Finally, the model must execute an **Implicit Coinsurance Algebraic Formula** Loss \* (Limit / (Total Value \* 0.80)) without making an order-of-operations error, ensuring the deductible is subtracted at the absolute end.

#### ---

**5\. Golden Response**

**CLAIMS APPORTIONMENT MEMORANDUM**

**TO:** Inland Marine Claims File

**DATE:** April 15, 2026

**SUBJECT:** Contractors Equipment Theft \- Coverage Calculation

**1\. Policy Limit Extraction (CM 71 97\)**

Based on a review of Section C (Limits of Insurance) in the attached Selective Contractors Equipment Coverage Form:

* **Section C.1:** The maximum payout for any one item of "Contractors equipment" is **$5,000**.  
* **Section C.2:** The maximum payout for any one item of personal property of employees (employee tools) is **$2,500**.

**2\. Valuation & Sub-Limit Application (Gross Eligible Loss)**

We must value the items based on age, then apply the extracted sub-limits:

7. **Skid Steer Loader:** Because the item is under 5 years old (3 years), it is valued at Replacement Cost ($15,000). This exceeds the per-item sub-limit, so it is capped at **$5,000**.  
8. **Bulldozer:** Because the item is 8 years old, it is valued at Actual Cash Value ($8,000). This value still exceeds the per-item sub-limit, so it is capped at **$5,000**.  
9. **Foreman's Tools:** Valued at Replacement Cost ($4,000). The value is capped by the employee sub-limit. Allowed amount is **$2,500**.  
10. **Gross Eligible Loss:** $5,000 \+ $5,000 \+ $2,500 \= **$12,500.**

**3\. Coinsurance Penalty Calculation**

The insured must carry a Blanket Policy Limit equal to 80% of their total inventory value.

* **Total Inventory Value:** $100,000  
* **Required Limit (80%):** $80,000  
* **Actual Limit Carried:** $50,000  
* **Coinsurance Ratio:** $50,000 / $80,000 \= **0.625 (62.5%)**

Applying this penalty ratio to the Gross Eligible Loss:

* $12,500 \* 0.625 \= **$7,812.50.**

**4\. Deductible & Final Quantum Calculation**

* Coinsurance Adjusted Loss: $7,812.50  
* Less Standard Deductible: ($1,000.00)  
* **Final Net Payable Amount: $6,812.50**

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

14. **Drug Requested Today:** "CardioMax" (Classification: **Tier 3** \- Preferred Brand).  
15. **Fill Request:** 90-day supply.  
16. **Pharmacy Type:** **Preferred Retail Pharmacy**.  
17. **Full Retail Cost of Drug (Negotiated Price):** $600.  
18. **Member's Current Total Drug Costs (TDC) Year-to-Date:** $4,800.  
19. **Member File Note:** "Member has a Part D Late Enrollment Penalty (LEP) of $9.80 per month. The pharmacy collects this monthly penalty at the point of sale for the duration of the fill supply."

**\--- SPECIAL UNDERWRITING BULLETINS (MANDATORY OVERRIDES) \---**

7. **Bulletin A (Preferred Network Discount):** If a member uses a Preferred Retail Pharmacy for a 90-day supply, apply a 10% discount to the **Base Copay** found in the Summary of Benefits. However, the pharmacy must add a flat **$5.50 "Network Access Fee"** for every 30 days of the supply (calculated after the 10% discount).  
8. **Bulletin B (The Straddle Rule):** The 2025 Initial Coverage Limit (ICL) is **$5,030**. If a fill pushes the member's TDC above this limit, the member must pay **25%** of the specific portion of the drug's Full Retail Cost that falls within the Coverage Gap (the amount exceeding $5,030), in addition to their adjusted base copay.

**Attached Files (Context):**

7. MEDM104X11289MCA4242025GRMASBMedicalMutualAdvantagePlusFA.pdf

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

11. **PDF Extraction:** According to the Retail Pharmacy Cost Sharing table in the 2025 Summary of Benefits, the base copay for a Tier 3 drug for a 31-90 day supply at a **Preferred Pharmacy** is **$50.00**.  
12. **Bulletin A Discount:** Applying the 10% Preferred Network Discount: $50.00 x 0.90 \= **$45.00**.  
13. **Bulletin A Access Fee:** Adding the $5.50 monthly fee for a 3-month (90-day) supply: $5.50 x 3 \= **$16.50**.  
14. **Adjusted Base Copay Total:** $45.00 \+ $16.50 \= **$61.50**.

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