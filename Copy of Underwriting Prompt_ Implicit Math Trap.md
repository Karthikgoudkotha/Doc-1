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