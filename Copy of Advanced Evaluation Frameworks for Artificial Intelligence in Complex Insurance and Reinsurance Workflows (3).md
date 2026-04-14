# **Toning prompt**

### **The Tuned Prompt**

## **Prompt 1** 

### **A: Role: You are a Senior U.S. Commercial Lines Underwriter (SME Property/BOP).**

Context:   
You are putting together a quick summary for a morning referral huddle based on a broker submission for a multi-location account. The master policy effective date is 01/01/2024. You must underwrite strictly using the attached "underwriting\_guidelines.pdf" and "submission\_data.csv". 

Critical Mid-Term Addition Rule:  
Loc\_5 is being added mid-term effective 04/01/2024. Its coverage ends on the master expiration date of 01/01/2025. You must calculate the pro-rata multiplier for Loc\_5 using an exact daily basis. Note: 2024 is a leap year (366 days).

Task 1: Data Verification  
Review the submission\_data.csv. Check if any required fields are missing, blank, or obviously invalid for all 5 locations.   
Output: If there are no issues, output exactly: "Data Check: None."

Task 2: Underwriting Disposition (Strict Comprehensiveness & Override)  
Evaluate ALL locations (Loc\_1 through Loc\_5) against Section I: Eligibility Rules.   
\* NEW OVERRIDE RULE: If any location has a Protection Class of 6 or worse AND is strictly older than 40 years (calculated as 2024 minus Year Built), its disposition automatically downgrades from Refer to "Decline \- Age/PC Override", regardless of the base rule.

Output format for each location:  
\* \[location\_id\] \- Disposition: \[Accept / Refer / Decline / Decline \- Age/PC Override\]  
\* Justification: \[1-2 brief reasons citing the specific rule, e.g., "Rule O.1", "Rule D.1"\]

Task 3: Pricing Indication (Strict Mathematical Accuracy)  
Calculate the indicated premium for ALL 5 locations.   
Mathematical Constraints (Execute in this exact order):  
1\. Base Premium \= (building\_value\_USD / 100\) \* base\_rate  
2\. Total Multiplier \= Mod A \* Mod B \* Mod C \* Mod D (Do not round the intermediate multiplier)  
3\. Indicated Premium \= Base Premium \* Total Multiplier  
4\. Mid-Term Pro-Rata: Apply the pro-rata multiplier ONLY to Loc\_5's Indicated Premium.  
5\. Final Premium \= Apply Section IV Minimum Premium rule ($500) if applicable to the pro-rated amount, then round to the nearest whole dollar. Note: The $500 minimum is a hard floor and is NOT prorated.

Output: Generate a complete Markdown table with exactly these columns:  
| location\_id | occupancy | base\_premium | total\_multiplier | pro\_rata\_factor | indicated\_premium | min\_premium\_applied\_YN | final\_premium |

Task 4: Total Account Premium & Portfolio Discount (Multi-Pass Logic)  
1\. Calculate the Pre-Discount Total: Sum the \`final\_premium\` values ONLY for locations that are not any form of "Decline".  
2\. Portfolio Discount: If the Pre-Discount Total is strictly greater than $3,000, apply a 10% discount ONLY to the \`final\_premium\` of locations graded as "Accept". Locations graded as "Refer" do not receive the discount.   
3\. Re-sum the Final Account Premium. (Note: The 10% discount CAN drop an Accept location below the $500 minimum).

Output:   
"Pre-Discount Total: $\[Value\]"  
"Discount Applied: \[Yes/No\]"  
"Final Account Premium: $\[Value\]"

### **B: Role: You are a Senior U.S. Commercial Lines Underwriter (SME Property/BOP).**

Context:   
You are summarizing a broker submission for a multi-location account. The policy effective date is 01/01/2024. You must underwrite using the attached "underwriting\_guidelines.pdf", "submission\_data.csv", AND the mandatory Underwriting Bulletin updates below. Do not use outside knowledge.

MANDATORY UNDERWRITING BULLETIN (Effective 01/01/2024):  
1\. The Manufacturing Override: Manufacturing is normally an automatic Decline (Rule O.1). However, if a Manufacturing location is BOTH "Fully Sprinklered" (Y) AND has a Protection Class of 4 or better, its disposition changes to "Accept subject to Surcharge".   
   \* Math Impact: For these specific locations, you MUST hardcode Mod A (Year Built) to 1.50, regardless of the actual year built.   
2\. The Small Office Surcharge: If an "Office" location has a building value STRICTLY LESS THAN $200,000, it is subject to a flat $150 policy fee.   
   \* Math Impact: This $150 fee must be added to the final\_premium ONLY AFTER the Minimum Premium rule has been checked and applied.  
3\. Portfolio Concentration Rule: If the sum of the \`building\_value\_USD\` across ALL submitted locations (including Declines) exceeds $2,500,000, you must apply a 1.10 Portfolio Surcharge to the final Total Account Premium.

Task 1: Data & Portfolio Check  
Review the submission\_data.csv.   
Output exactly two lines:  
\* "Data Check: \[None / List missing fields\]"  
\* "Portfolio Concentration Triggered: \[Yes / No\] (Total Value: $\[Sum of all building values\])"

Task 2: Underwriting Disposition (Strict Comprehensiveness)  
Evaluate ALL locations (Loc\_1 through Loc\_5) against Section I rules AND the Bulletin Updates.   
Output format for each location:  
\* \[location\_id\] \- Disposition: \[Accept / Refer / Decline / Accept subject to Surcharge\]  
\* Justification: \[1-2 brief reasons citing the specific rule or bulletin\]

Task 3: Pricing Indication (Strict Mathematical Accuracy)  
Calculate the indicated premium for ALL 5 locations.   
Mathematical Constraints:  
1\. Base Premium \= (building\_value\_USD / 100\) \* base\_rate  
2\. Total Multiplier \= Mod A \* Mod B \* Mod C \* Mod D (Do not round the intermediate multiplier)  
3\. Indicated Premium \= Base Premium \* Total Multiplier  
4\. Final Premium \= Check the Section IV Minimum Premium rule ($500). Apply the $500 minimum if applicable. THEN add any flat fees from the Underwriting Bulletin. Round the final result to the nearest whole dollar.

Output: Generate a complete Markdown table with exactly these columns:  
| location\_id | occupancy | building\_value\_USD | base\_rate | base\_premium | mod\_a | mod\_b | mod\_c | mod\_d | total\_multiplier | indicated\_premium | min\_premium\_applied\_YN | final\_premium | notes |

Task 4: Total Account Premium (Atomicity)  
Sum the \`final\_premium\` values ONLY for locations that are NOT "Decline".   
Apply the 1.10 Portfolio Surcharge to this sum ONLY IF the Portfolio Concentration Rule was triggered in Task 1\. Round to the nearest whole dollar.  
Output: "Total Account Premium: $\[Value\]"

### **C: Role: You are a Senior U.S. Commercial Lines Underwriter (SME Property/BOP).**

### 

Context:   
You are putting together a quick summary for a morning referral huddle based on a broker submission for a multi-location account. The policy effective date is 01/01/2024. You must underwrite strictly using the attached "underwriting\_guidelines.pdf" and "submission\_data.csv", plus the Special Bulletins listed below. Do not invent rules or use outside knowledge.

Special Underwriting Bulletins (Overrides attached PDF):  
\* Exception Bulletin 1 (The Sprinkler Override): Any location that would normally be an automatic "Decline" based solely on Occupancy is upgraded to "Refer" IF AND ONLY IF the Year Built is 2005 or newer AND Sprinkler\_YN is "Y".   
\* Exception Bulletin 2 (Deductible Tiering): All eligible/referred locations receive a standard $1,000 deductible (Factor: 0.95). However, any location located in Protection Class 6-10 MUST be assigned a mandatory $2,500 deductible (Factor: 0.85).   
\* Exception Bulletin 3 (Account-Wide Loss Surcharge): Calculate the sum of 3-year losses across ALL submitted locations (including any that end up Declined). If this Total Submission Loss amount exceeds $15,000, a 1.15 "Account Surcharge" must be applied to the Final Total Account Premium.

Task 1: Data Verification  
Review the submission\_data.csv. Check if any required fields are missing, blank, or obviously invalid for all 5 locations.   
Output: If there are no issues, output exactly: "Data Check: None."

Task 2: Underwriting Disposition (Strict Comprehensiveness)  
Evaluate ALL 5 locations against the guidelines and Special Bulletins.   
Output format for each location:  
\* \[location\_id\] \- Disposition: \[Accept / Refer / Decline\]  
\* Justification: \[Explain the logic, explicitly noting if an Exception Bulletin was triggered\].

Task 3: Pricing Indication (Strict Mathematical Sequence)  
Calculate the indicated premium for ALL 5 locations.   
Mathematical Constraints \- You MUST follow this exact sequence:  
1\. Base Premium \= (building\_value\_USD / 100\) \* base\_rate  
2\. Guideline Multiplier \= Mod A \* Mod B \* Mod C \* Mod D (Do not round yet)  
3\. Base Indicated \= Base Premium \* Guideline Multiplier  
4\. Deductible Adjusted Premium \= Base Indicated \* Applicable Deductible Factor (from Bulletin 2).  
5\. Final Location Premium \= Apply Section IV Minimum Premium rule ($500) if applicable, then round to the nearest whole dollar.

Output: Generate a complete Markdown table with exactly these columns:  
| location\_id | base\_premium | guideline\_mult | base\_indicated | deductible\_factor | ded\_adjusted\_prem | min\_premium\_applied\_YN | final\_location\_premium | disposition\_status |

Task 4: Total Account Premium (Cross-State Dependency)  
Sum the \`final\_location\_premium\` values ONLY for locations that are not "Decline".   
Then, evaluate Special Bulletin 3 (Account-Wide Loss Surcharge) and apply the 1.15 multiplier if required by the rules. Round the absolute final number to the nearest whole dollar.  
Output format:  
\* Total Eligible Premium (Before Surcharge): $\[Value\]  
\* Total Submission Losses: $\[Value\]  
\* Surcharge Applied: \[Yes/No\]  
\* Final Total Account Premium: $\[Value\]

## **Prompt 3**

### **Example 4: Employment Practices Liability (Mixed Claim Allocation & Floating-Point Trap)**

**1\. Metadata**

* Task Type: Workflow  
* Category / Domain: Management Liability / Claims  
* Workflow: Quantum Calculation & Allocation  
* Prompt Type: Claims Strategy Memo  
* Difficulty: Super Hard (Expert)

**2\. Prompt**

You are a Management Liability Claims Adjuster. The current date is February 15, 2026\.

Your insured, a logistics company, holds an ABAIS Employment Practices Liability (EPLI) policy (Form D 71100-G). The policy has a $1,000,000 Limit of Liability. The Declarations page indicates that Endorsement D 71706(7) "Cost of Defense Sub-Limit For \- Wage and Hour Violations" is attached, with a sub-limit of $100,000.00.

The insured was sued by a former employee (Claim ID: EPL-405). The lawsuit contained two distinct causes of action:

* A Wage and Hour violation alleging failure to pay overtime under the FLSA.  
* A Retaliation claim, alleging the employee was wrongfully terminated specifically for reporting the FLSA violation to human resources.

The lawsuit was settled last week. Assume the policy's Retention has already been fully exhausted by the insured on a previous, unrelated claim this policy year, meaning we are calculating first-dollar coverage for this incident.

Review the attached legal\_and\_settlement\_ledger.csv and the ABAIS EPLI policy specimen. Draft a Claims Strategy Memo. You must:

* Extract the exact floating-point defense costs and settlement (back-pay) amounts for both causes of action under Claim EPL-405. Be careful to avoid decoy records.  
* Conduct a comparative analysis of how the base policy treats "back-pay" under the Definition of Loss (Exclusion H) versus the absolute bar on Wage and Hour claims (Exclusion G), explicitly analyzing any nested exceptions for Retaliation.  
* Evaluate the specific mechanics of the D 71706(7) endorsement to determine indemnity and defense coverage strictly for the FLSA portion of the claim.  
* Calculate the exact, final floating-point dollar amount the insurer will pay for this mixed claim, detailing the mathematical segregation of the sub-limits.

**Attached Files (Context):**

legal\_and\_settlement\_ledger.csv

Code snippet

Claim\_ID,Cause\_of\_Action,Expense\_Type,Incurred\_Amount  
EPL-404,Discrimination,Defense,$45000.25  
EPL-405,FLSA Overtime (W\&H),Settlement (Back-Pay),$88450.75  
EPL-405,FLSA Overtime (W\&H),Defense,$134250.50  
EPL-405,Retaliation,Settlement (Back-Pay),$65200.25  
EPL-405,Retaliation,Defense,$42100.25

ABAIS EPLI Policy Specimen: https://www.abais.com/docs/default-source/small-business/epl/epli-policy-specimen.pdf

**3\. Rubric (Negative Failure Focus)**

| \# | Description | Weight | Sources (with exact PDF pages) | Justification | Met | Failure Reasoning (Model Failure Example) | Dependent Criteria |
| :---- | :---- | :---- | :---- | :---- | :---- | :---- | :---- |
| 1 | Extracts data for the correct claim from the CSV. | Minor | legal\_and\_settlement\_ledger.csv, Rows 3-6 | Must bypass the EPL-404 decoy and extract the four exact floating-point values for EPL-405. | FALSE | **Model Failure (Extraction):** Pulled the $45,000.25 value from the decoy row due to sequential reading bias. | None |
| 2 | Analyzes the Exclusion G Retaliation carve-back. | Critical | ABAIS EPLI PDF, Page 10 | Must recognize that Exclusion G completely excludes Wage & Hour claims but explicitly states the exclusion "shall not apply to that part of any Claim alleging retaliation". | FALSE | **Model Failure (Logic):** Read "Wage & Hour" and assumed the entire lawsuit (including the retaliation claim) was excluded or subject to the sub-limit. | None |
| 3 | Analyzes Exclusion H for Back-Pay. | Major | ABAIS EPLI PDF, Page 10 | Must note that Exclusion H generally excludes wages but contains an exception preserving coverage for back-pay, making the Retaliation settlement fully covered. | FALSE | Failed to identify the back-pay exception that saves the Retaliation indemnity. | 2 |
| 4 | Evaluates Endorsement D 71706(7). | Critical | ABAIS EPLI PDF, Page 21 | Must identify that the endorsement only buys back "Costs of Defense" for the Wage & Hour claim up to $100k, providing zero indemnity for the W\&H settlement. | FALSE | **Model Failure (Logic):** Assumed the endorsement provided both indemnity and defense coverage up to the sub-limit. | None |
| 5 | Calculates the FLSA (Wage & Hour) payout. | Critical | Prompt Text | Must deny the $88,450.75 W\&H settlement entirely and mathematically cap the $134,250.50 W\&H defense costs at exactly $100,000.00. | FALSE | **Model Failure (Math):** Paid the $88,450.75 W\&H settlement because it saw the "back-pay" exception in Exclusion H, ignoring the absolute bar in Exclusion G. | 4 |
| 6 | Calculates the Retaliation payout. | Critical | Prompt Text | Must fully cover the $42,100.25 Retaliation defense and the $65,200.25 Retaliation settlement ($107,300.50 total). | FALSE | **Model Failure (Synthesis):** Capped the retaliation costs under the Wage & Hour sub-limit. | 2, 3 |
| 7 | Provides an actionable, exact payout recommendation. | Critical | Prompt Text | Must sum the allowed buckets ($100,000.00 \+ $107,300.50) to recommend a final, total claim payout of exactly $207,300.50. | FALSE | **Model Failure (Final):** Recommended a payout of $100,000.00 (capping the whole claim at the sub-limit) or $330,001.75 (missing all exclusions and limits). | 5, 6 |

**4\. Model Analysis**

This prompt corrects the previous version by removing the explicit instructions on how to handle the sub-limits and exclusions. It operates as a severe "broad-brush" policy-hierarchy trap. Because the root of the lawsuit is an FLSA violation, standard LLMs will classify the *entire* lawsuit as a Wage & Hour claim. They will mistakenly apply the $100,000 defense sub-limit to the entire aggregate loss and deny all indemnity, recommending a flat $100,000 payout. To reach the Golden Response, the model must independently execute strict legal parsing of the ABAIS policy text. It must discover the nested exception in Exclusion G, which states that the Wage & Hour exclusion *does not apply* to retaliation claims. Therefore, the Retaliation portion is severed and treated as a standard, fully covered Employment Practices Wrongful Act. The model must segregate the floating-point math: The W\&H settlement ($88,450.75) is excluded, the W\&H defense ($134,250.50) is capped at $100,000.00, but the Retaliation settlement ($65,200.25) and Retaliation defense ($42,100.25) are paid in full.

**5\. Golden Response**

CLAIMS STRATEGY MEMO

TO: Claims Director

FROM: Management Liability Claims Adjuster

DATE: February 15, 2026

SUBJECT: EPLI Coverage Determination – Mixed Wage & Hour and Retaliation Claim

* CSV Data Extraction & Claim Overview  
  Pursuant to the legal\_and\_settlement\_ledger.csv, we are evaluating Claim EPL-405 (bypassing the EPL-404 decoy). The incurred floating-point amounts for this mixed claim are as follows:  
* FLSA Overtime (W\&H) Settlement (Back-Pay): $88,450.75  
* FLSA Overtime (W\&H) Defense: $134,250.50  
* Retaliation Settlement (Back-Pay): $65,200.25  
* Retaliation Defense: $42,100.25  
* Comparative Analysis: Exclusion G (Wage & Hour) vs. Exclusion H (Loss Definition)  
  Evaluating this mixed claim requires separating the causes of action and applying the ABAIS D 71100-G policy exclusions independently.  
  Exclusion H generally dictates that "Loss" does not include salary, wages, or compensation, but it contains an explicit carve-back preserving coverage for front pay and back pay. However, Exclusion G serves as an absolute bar to coverage for any actual or alleged Wage and Hour Violation, overriding the back-pay allowance for that specific peril.  
  Crucially, Exclusion G contains its own nested exception: the exclusion does not apply to "that part of any Claim alleging retaliation for the exercise of any rights under such laws or regulations governing wage, hour and payroll practices." Therefore, the Retaliation cause of action is legally severed from the Wage & Hour exclusion and remains fully covered, allowing the back-pay exception in Exclusion H to apply to the Retaliation settlement.  
* Scenario Evaluation: Endorsement D 71706(7) Application  
  The insured purchased Endorsement D 71706(7), the "Cost of Defense Sub-Limit For \- Wage and Hour Violations." This endorsement amends Exclusion G, stating that the exclusion shall not apply to Costs of Defense as a result of any Claim for Wage and Hour Violations up to the applicable Sub-Limit of Liability. This endorsement is strictly limited to defense costs and provides zero indemnity coverage for Wage and Hour settlements or judgments.  
* Quantum Calculation and Segregation  
  Because the policy Retention has already been satisfied, we calculate first-dollar liability for the remaining amounts based on the allocated ledger:  
* **FLSA (W\&H) Settlement ($88,450.75):** Strictly excluded under Exclusion G. No indemnity is provided by the D 71706(7) endorsement. Payable: $0.00.  
* **FLSA (W\&H) Defense Costs ($134,250.50):** Covered solely by Endorsement D 71706(7), but subject to a strict hard cap. Payable: $100,000.00 (Maximum Sub-Limit).  
* **Retaliation Settlement ($65,200.25):** Fully covered. The Exclusion G carve-back protects retaliation claims, and the Exclusion H carve-back expressly permits the payment of back-pay for covered wrongful acts. Payable: $65,200.25.  
* **Retaliation Defense Costs ($42,100.25):** Fully covered under the main $1,000,000 policy limit and not subject to the Wage & Hour sub-limit. Payable: $42,100.25.  
* Actionable Recommendation  
  I recommend issuing a formal coverage determination letter outlining the mixed nature of this claim. We must formally deny the $88,450.75 FLSA Wage and Hour settlement portion. However, we should authorize a final, total claim payout of exactly **$207,300.50** to the insured. This consists of the capped $100,000.00 FLSA defense costs, the $42,100.25 Retaliation defense costs, and the $65,200.25 Retaliation settlement.

---

### **Example 1: Medicare Advantage Part D (Floating-Point Deductible & TrOOP CSV Trap)**

**1\. Metadata**

* Task Type: Workflow  
* Category / Domain: Healthcare Claims / Medicare Advantage  
* Workflow: Quantum Calculation & Benefit Explanation  
* Prompt Type: Claims Determination Memo  
* Difficulty: Super Hard (Expert)

**2\. Prompt**

You are a Medicare Benefits Counselor. The current date is February 20, 2026\.

A member named **Robert Smith (ID: MEM-442B)** is currently at the pharmacy counter attempting to fill a 90-day supply of a Tier 3 (preferred brand) maintenance drug. The pharmacy he is using is classified as a **Standard Retail** network pharmacy. The total retail cost of the drug submitted by the pharmacy is $845.25.

The member is enrolled in the MedMutual Advantage Plus PPO plan (Group C41390). He wants to know exactly how much he will have to pay out-of-pocket today before he authorizes the pharmacist to run the card.

Review the attached medicare\_rx\_claims\_log.csv and the MedMutual Advantage Plus PPO Summary of Benefits.

Draft a Claims Determination Memo. You must:

1. Extract Robert Smith's exact Prior\_TrOOP\_Paid (True Out-of-Pocket) amount from the highly dense CSV. Be extremely precise to avoid the decoy records.  
2. Conduct a comparative analysis of the MedMutual Part D Deductible Stage versus the Initial Coverage Stage, determining the exact remaining deductible Robert must satisfy.  
3. Locate the specific copay required for a Tier 3 drug, for a 90-day supply, at a Standard Retail pharmacy.  
4. Execute the exact mathematical calculation to determine Robert's total out-of-pocket cost for this specific fill (Remaining Deductible \+ Applicable Copay).  
5. Formulate an actionable, binding recommendation stating the final, exact floating-point dollar amount Robert must pay at the counter today.

**Attached Files (Context):**

medicare\_rx\_claims\_log.csv

Code snippet

Member\_ID,Plan\_Name,Prior\_TrOOP\_Paid,Drug\_Tier,Fill\_Type,Days\_Supply,Total\_Retail\_Cost  
MEM-442A,MedMutual Premium PPO,$185.50,Tier 3,Preferred Retail,90,$845.25  
MEM-442B,MedMutual Plus PPO,$185.50,Tier 3,Standard Retail,90,$845.25  
MEM-442C,MedMutual Plus PPO,$500.00,Tier 4,Standard Retail,90,$845.25

MedMutual Summary of Benefits: https://www.medmutual.com/-/media/MedMutual/Files/For-Medicare/2025/Summary-of-Benefits/EGWP/MEDM104X11289MCA4242025GRMASBMedicalMutualAdvantagePlusFA.pdf

**3\. Rubric (Negative Failure Focus)**

| \# | Description | Weight | Sources (with exact PDF/Link pages) | Justification | Met | Failure Reasoning (Model Failure Example) | Dependent Criteria |
| :---- | :---- | :---- | :---- | :---- | :---- | :---- | :---- |
| 1 | Extracts data for the target member from the CSV. | Critical | medicare\_rx\_claims\_log.csv, Row 3 | Must bypass the MEM-442A and MEM-442C decoys and pull MEM-442B: Prior TrOOP \= $185.50. | FALSE | **Model Failure (Extraction):** Extracted MEM-442C because it saw "$500.00" and associated it with the deductible. | None |
| 2 | Identifies the Part D Deductible. | Major | MedMutual PDF, Page 11 | Must state that the plan features a $500.00 Part D deductible. | FALSE | **Model Failure (Logic):** Assumed the deductible was already met because the prompt mentions "maintenance drug." | None |
| 3 | Calculates the exact Remaining Deductible. | Critical | Prompt Text | Must subtract the $185.50 Prior TrOOP from the $500.00 deductible to arrive at exactly $314.50. | FALSE | **Model Failure (Math):** Subtracted $185.50 from the total retail cost ($845.25) instead of the deductible limit. | 1, 2 |
| 4 | Extracts the precise copay tier requirement. | Critical | MedMutual PDF, Page 11 | Must navigate the grid to find Tier 3 \> 31-90 day supply \> Standard Retail Pharmacy \= $63.00 copay. | FALSE | **Model Failure (Extraction):** Pulled the Preferred Retail copay ($50), the Mail-Order copay ($37), or the 30-day copay ($15). | None |
| 5 | Calculates the Final Out-of-Pocket Cost. | Critical | Prompt Text | Must sum the Remaining Deductible ($314.50) \+ Initial Coverage Stage Copay ($63.00) \= $377.50. | FALSE | **Model Failure (Synthesis):** Charged the member the full $845.25, or just $63.00, failing to split the cost across the two coverage phases. | 3, 4 |
| 6 | Provides an actionable payment recommendation. | Major | Prompt Text | Must clearly advise the member to pay exactly $377.50 at the pharmacy counter. | FALSE | Provided an ambiguous range instead of the exact floating-point dollar amount. | 5 |

**4\. Model Analysis**

This prompt guarantees a \>30% failure rate on the rubric for standard models by compounding extraction decoys with a multi-phase benefit calculation. The CSV contains three rows with nearly identical member IDs and retail costs. If the model's extraction attention wavers, it will pull MEM-442C, assume the $500 deductible is met, and only charge the copay. Furthermore, the PDF contains a complex, multi-column grid for pharmacy pricing. Models frequently hallucinate the "Preferred Retail" ($50) or "Mail Order" ($37) prices instead of the requested "Standard Retail" ($63) price. The ultimate math trap requires the model to realize that the member must pay the remaining $314.50 to clear the Deductible Stage, which triggers the Initial Coverage Stage for the remainder of the $845.25 retail cost, allowing the flat $63.00 copay to be applied to the balance. A successful model will execute the sequential floating-point math flawlessly to output exactly $377.50.

**5\. Golden Response**

CLAIMS DETERMINATION MEMO

TO: Robert Smith (Member ID: MEM-442B)

FROM: Medicare Benefits Counselor

DATE: February 20, 2026

SUBJECT: Pharmacy Out-of-Pocket Calculation – 90-Day Tier 3 Fill

1. CSV Data Extraction  
   To accurately calculate your out-of-pocket costs at the pharmacy counter today, we must isolate your specific historical data from the medicare\_rx\_claims\_log.csv. Bypassing the other similar records, the extracted data for your specific profile (MEM-442B) is:  
* Prior TrOOP Paid (True Out-of-Pocket): $185.50  
* Drug Tier: Tier 3  
* Pharmacy Type: Standard Retail  
* Days Supply: 90  
* Total Retail Cost: $845.25  
2. Comparative Analysis: Deductible Stage vs. Initial Coverage Stage  
   Under the MedMutual Advantage Plus PPO Summary of Benefits (Page 11), your prescription drug coverage operates in stages. You must first satisfy a $500.00 Part D Deductible. During the Deductible Stage, you pay the full retail cost of the drug until your out-of-pocket spending reaches $500.00. Once you have paid $500.00, you leave the Deductible Stage and move to the Initial Coverage Stage, where you only pay a flat copay for the remainder of the drug's cost.  
* **Remaining Deductible Calculation:** $500.00 (Plan Deductible) \- $185.50 (Prior TrOOP Paid) \= **$314.50**.  
3. Copay Extraction  
   Once your $314.50 remaining deductible is satisfied, the remaining balance of the drug's $845.25 retail cost falls into the Initial Coverage Stage. According to the Retail Pharmacy Cost Sharing grid in your Summary of Benefits (Page 11), the specific copay for a Tier 3 drug, for a 31-90 day supply, at a Standard network pharmacy is **$63.00**.  
4. Final Mathematical Calculation and Actionable Recommendation  
   Your total out-of-pocket cost today is calculated by combining your remaining deductible obligation with your Initial Coverage Stage copay:  
* Remaining Deductible: $314.50  
* Initial Coverage Copay: $63.00  
* **Total Out-of-Pocket Cost: $377.50**

I advise you to authorize the pharmacist to process the claim. Your exact payment at the counter today will be **$377.50**.

---

### **Example 2: Reinsurance Sliding Scale Commission (Carryforward & Floating-Point Formula Trap)**

**1\. Metadata**

* Task Type: Workflow  
* Category / Domain: Reinsurance / Actuarial  
* Workflow: Treaty Pricing & Adjudication  
* Prompt Type: Actuarial Adjustment Memo  
* Difficulty: Super Hard (Expert)

**2\. Prompt**

You are a Reinsurance Actuary. The current date is February 25, 2026\.

You must calculate the final sliding scale commission due to a ceding company for the "2024" Treaty Year under their Quota Share Reinsurance Agreement.

The treaty parameters dictate:

* Provisional Commission: 31.25% at a 62.50% Loss Ratio.  
* The commission slides 0.75% for every 1.00% decrease in the Loss Ratio below 62.50%.  
* Any "Carryforward Penalty" from prior years must be added directly to the current year's Loss Ratio *before* calculating the slide.

Review the attached treaty\_accounting\_ledger.csv.

Draft an Actuarial Adjustment Memo. You must:

1. Extract the Earned Premium, Paid Losses, Prior Year Outstanding Losses, Current Year Outstanding Losses, and the Carryforward Penalty specifically for the **2024** Treaty Year. Be highly precise to avoid extracting the 2023 or 2025 decoy data.  
2. Calculate the "Incurred Losses" for 2024 using the standard reinsurance formula: (Paid Losses \+ Current Year Outstanding Losses \- Prior Year Outstanding Losses).  
3. Calculate the Actual Loss Ratio for 2024, and then apply the extracted carryforward penalty to determine the "Adjusted Loss Ratio".  
4. Calculate the final sliding scale commission percentage based on the Adjusted Loss Ratio.  
5. Formulate an actionable recommendation stating the exact final commission rate the reinsurer will pay.

**Attached Files (Context):**

treaty\_accounting\_ledger.csv

Code snippet

Treaty\_Year,Earned\_Premium,Paid\_Losses,OS\_Losses\_Prior\_Yr,OS\_Losses\_Current\_Yr,Carryforward\_Penalty  
2023,$15450250.50,$6150200.25,$1250100.00,$2150300.75,0.00%  
2024,$18500000.00,$8450250.50,$2150750.75,$2950500.25,1.85%  
2025,$18500000.00,$8145100.50,$2950500.25,$3150500.50,0.00%

Reinsurance Reference: https://www.soa.org/globalassets/assets/files/edu/edu-2014-exam-at-study-note-basics-rein.pdf

**3\. Rubric (Negative Failure Focus)**

| \# | Description | Weight | Sources (with exact PDF/Link pages) | Justification | Met | Failure Reasoning (Model Failure Example) | Dependent Criteria |
| :---- | :---- | :---- | :---- | :---- | :---- | :---- | :---- |
| 1 | Extracts data for 2024 from the CSV. | Critical | treaty\_accounting\_ledger.csv, Row 3 | Must correctly pull data strictly from the 2024 row, bypassing 2023 and 2025\. | FALSE | **Model Failure (Extraction):** Pulled the 2023 or 2025 data, or mixed values across the rows due to floating-point similarity. | None |
| 2 | Extracts the correct OS\_Losses\_Prior\_Yr. | Major | treaty\_accounting\_ledger.csv, Row 3 | Must identify the prior year reserve as exactly $2,150,750.75. | FALSE | **Model Failure (Extraction):** Assumed "Prior Yr" meant looking at the 2023 row and pulled $1,250,100.00 by mistake. | 1 |
| 3 | Calculates the Incurred Losses. | Critical | Reinsurance Accounting Principles | Must apply the formula: Paid (8,450,250.50) \+ Current OS (2,950,500.25) \- Prior OS (2,150,750.75) \= exactly $9,250,000.00. | FALSE | **Model Failure (Math):** Added all the reserves together failing the basic actuarial sign logic formula. | 2 |
| 4 | Calculates the Actual Loss Ratio. | Major | Prompt Text | Must divide 9,250,000.00 by 18,500,000.00 to arrive at exactly 50.00%. | FALSE | Miscalculated the base loss ratio division. | 3 |
| 5 | Calculates the Adjusted Loss Ratio. | Critical | SOA PDF, Page 14 | Must add the 1.85% carryforward penalty to the 50.00% loss ratio to equal an Adjusted Loss Ratio of 51.85%. | FALSE | **Model Failure (Logic):** Deducted the penalty from the commission or ignored it completely. | 4 |
| 6 | Calculates the final sliding scale commission. | Critical | Prompt Text | Must calculate the improvement: (62.50 \- 51.85) \= 10.65%. 10.65% \* 0.75 \= 7.9875%. Final \= 31.25% \+ 7.9875% \= 39.2375%. | FALSE | **Model Failure (Math):** Applied the slide to the raw 50% loss ratio instead of the adjusted 51.85%, generating a 40.625% commission. | 5 |
| 7 | Provides an actionable recommendation. | Minor | Prompt Text | Must state the final commission rate to be paid is 39.2375%. | FALSE | Failed to provide the final, exact four-decimal precision rate. | 6 |

**4\. Model Analysis**

This prompt is designed to devastate an LLM's extraction and mathematical sequencing capabilities, guaranteeing a \>30% failure rate on the rubric if the model isn't highly tuned. The trap lies in the CSV header OS\_Losses\_Prior\_Yr. An LLM reading the row for "2024" will see OS\_Losses\_Prior\_Yr and frequently "jump" to the 2023 row to extract that number, grabbing $1,250,100.00 instead of the $2,150,750.75 actually listed in the 2024 column array. This extraction failure destroys criteria 1, 2, and 3\. Furthermore, LLMs struggle with actuarial sign logic; they often add Prior Outstanding Reserves to Paid Losses rather than subtracting them, inflating the Incurred Loss. Finally, the model must remember to apply the extracted 1.85% carryforward penalty to the *Loss Ratio*, not the *Commission*, before executing the 0.75:1 sliding scale formula on complex decimals.

**5\. Golden Response**

ACTUARIAL ADJUSTMENT MEMO

TO: Reinsurance Accounting

FROM: Reinsurance Actuary

DATE: February 25, 2026

SUBJECT: 2024 Treaty Year – Sliding Scale Commission Calculation

1. CSV Data Extraction  
   Based on the treaty\_accounting\_ledger.csv, the specific financial variables for the 2024 Treaty Year are as follows:  
* Earned Premium: $18,500,000.00  
* Paid Losses: $8,450,250.50  
* OS Losses Prior Year: $2,150,750.75  
* OS Losses Current Year: $2,950,500.25  
* Carryforward Penalty: 1.85%  
2. Incurred Loss Calculation  
   Per standard reinsurance accounting practices, Incurred Losses are calculated by taking the Paid Losses, adding the Outstanding (OS) Loss Reserve for the current year, and subtracting the OS Loss Reserve from the prior year (which was already accounted for in the previous adjustment).  
* Formula: $8,450,250.50 (Paid) \+ $2,950,500.25 (Current OS) \- $2,150,750.75 (Prior OS)  
* **Incurred Losses: $9,250,000.00**  
3. Loss Ratio and Carryforward Adjustment  
   The Actual Loss Ratio is the ratio of Incurred Losses to Earned Premium:  
* $9,250,000.00 / $18,500,000.00 \= 50.00% Actual Loss Ratio.  
  However, the treaty carries a 1.85% penalty from prior years. As detailed in standard actuarial guidance, this carryforward shifts the slide by adding directly to the current year's loss ratio.  
* 50.00% \+ 1.85% \= **51.85% Adjusted Loss Ratio**  
4. Sliding Scale Commission Calculation  
   The treaty provides a provisional commission of 31.25% at a 62.50% loss ratio, sliding 0.75% for every 1.00% decrease in the loss ratio below 62.50%.  
* Improvement: 62.50% (Threshold) \- 51.85% (Adjusted LR) \= 10.65 points of improvement.  
* Slide Application: 10.65 points \* 0.75 slide factor \= 7.9875% additional commission.  
* Final Commission: 31.25% (Provisional) \+ 7.9875% \= **39.2375%**  
5. Actionable Recommendation  
   I recommend authorizing a final sliding scale commission rate of **39.2375%** for the 2024 Treaty Year. This calculation properly credits the reinsurer for the 1.85% historical carryforward penalty before applying the 0.75:1 profitability slide to the cedent's current year performance.

# **New prompts**

---

### **Example 1: Surety Bond Legal Precedent and Forfeiture Risk**

**1\. Metadata**

Task Type: Workflow

Category / Domain: Underwriting / Legal Analysis

Workflow: Risk Assessment

Prompt Type: Underwriting Memo

Difficulty: High

**2\. Prompt**

You are a Senior Surety Underwriter. The current date is October 1, 2025\.

A large general contractor (your principal) is bidding on a $25 million public works project. The obligee is demanding that the contractor use a custom manuscript performance bond instead of the standard AIA A312-2010 Performance Bond.

You note the following issues with the proposed manuscript bond:

1. It completely removes the 10-day conference requirement and the notice of intent to declare default.  
2. It contains a penal sum forfeiture clause stating that upon any regulatory or safety violation on the job site, the full penal sum of the bond is instantly forfeited to the obligee.  
3. It requires the surety to explicitly waive the material alteration defense.

Review this scenario. Compare the obligee's rights and the surety's exposure under this manuscript bond versus the standard AIA A312-2010. Evaluate the specific legal and financial risks of the forfeiture clause and the material alteration waiver. Finally, draft an actionable recommendation memo to the Chief Underwriting Officer detailing whether to approve this bond or demand a return to the A312-2010 form.

**3\. Rubric**

| Description | Weight | Justification | Met | Failure Reasoning |
| :---- | :---- | :---- | :---- | :---- |
| **Identifies the AIA A312 conditions precedent.** | Critical | Must explain that A312 Section 3 requires notice and a conference before default can be declared. | FALSE | Failed to mention Section 3 notice and conference requirements. |
| **Analyzes the impact of bypassing the surety's A312 Section 5 options.** | Major | Must note that unilateral action by an obligee under an A312 typically discharges the surety. | FALSE | Did not discuss the surety's loss of mitigation rights. |
| **Evaluates the manuscript's penal sum forfeiture clause.** | Critical | Must assess the severe risk of the entire bond penalty being demanded for a single regulatory violation. | FALSE | Ignored the penal sum forfeiture risk. |
| **Analyzes the waiver of the material alteration defense.** | Major | Must recognize that waiving this defense allows the obligee to change the contract terms without discharging the surety. | FALSE | Failed to address the material alteration waiver. |
| **Provides an actionable recommendation.** | Critical | Must formulate a clear business decision to decline the manuscript bond. | FALSE | Did not provide a definitive underwriting recommendation. |
| **Formats the response as a formal Underwriting Memo.** | Minor | Required output styling. | FALSE | Wrote an unstructured paragraph. |

**4\. Model Analysis**

This prompt requires the LLM to perform a high-level comparative analysis between a standardized industry contract (AIA A312) and an aggressive manuscript bond. The model must demonstrate an understanding of surety case law, specifically that the A312 uses strict conditions precedent to protect the surety from obligee "self-help." By assessing the scenario, the model must recognize that the manuscript bond's forfeiture clause and waiver of material alteration strip away the surety's common law and contractual defenses, making the risk uninsurable. The final step requires synthesizing this legal analysis into a decisive business recommendation.

**5\. Golden Response**

UNDERWRITING MEMO

TO: Chief Underwriting Officer

FROM: Senior Surety Underwriter

DATE: October 1, 2025

SUBJECT: Risk Assessment & Recommendation – Manuscript Performance Bond

1. Comparative Analysis: AIA A312 vs. Manuscript Bond  
   The proposed manuscript bond presents severe deviations from the industry-standard AIA A312-2010 Performance Bond. Under the standard A312, Section 3 outlines mandatory conditions precedent that an obligee must satisfy to trigger the surety's liability. This structured process requires the owner to first provide notice that it is considering declaring a contractor default (Section 3.1) and, subsequently, to formally declare a default and terminate the contract (Section 3.2). This allows the surety to exercise its remediation options under Section 5\.

Courts strictly enforce these A312 conditions; if an obligee unilaterally terminates the principal and completes the work without proper notice, it is a material breach that discharges the surety's obligations, as it deprives the surety of its contractually agreed-upon opportunity to participate in remedying the default. The proposed manuscript bond completely deletes these conditions precedent, authorizing the obligee to bypass the surety's mitigation rights entirely.

2. Risk Assessment and Scenario Evaluation  
   The manuscript bond introduces two fatal underwriting risks:  
* **Penal Sum Forfeiture:** The bond operates as a penal sum forfeiture instrument. By mandating that any regulatory violation triggers the immediate forfeiture of the full $25 million penal sum, the bond transforms from a performance guarantee into a punitive financial penalty.  
* **Waiver of Material Alteration:** The bond expressly requires the surety to waive any material alteration defenses. In a scenario where the obligee drastically expands the contractor's scope of work or extends the timeline without our consent, we would normally be discharged from our bond obligations. Waiving this defense means we are guaranteeing an unknown and infinitely expandable contract.  
3. Actionable Recommendation  
   Given the extreme risk profile, the surety's exposure under this manuscript bond is unquantifiable. I recommend that we absolutely decline to issue this manuscript bond. We must issue a counter-proposal to the obligee stipulating that we will only support our principal if the bond is executed on the standard AIA A312-2010 form, which preserves our equitable claims-handling options and limits our exposure to actual damages.

---

### **Example 2: Property Claims Scenario and Ordinance or Law Application**

**1\. Metadata**

Task Type: Workflow

Category / Domain: Claims Handling

Workflow: Commercial Property

Prompt Type: Claims Determination Memo

Difficulty: High

**2\. Prompt**

You are a Commercial Property Claims Adjuster. The current date is June 1, 2025\.

Your insured owns a commercial building insured under an ISO CP 00 10 Building and Personal Property Coverage Form paired with a CP 10 30 Causes of Loss \- Special Form. The building limit is $1,000,000. The insured also purchased the CP 04 05 09 17 Ordinance or Law Coverage endorsement, which includes Coverages A, B, and C with adequate limits.

A severe fire (a covered peril) caused $600,000 in direct physical damage, destroying exactly 60% of the building. Due to a local municipal zoning and building code, any structure damaged by more than 50% must be completely demolished, and the replacement structure must be rebuilt to include newly mandated wind-resistive construction and an automatic sprinkler system.

The costs are as follows:

* Direct fire damage: $600,000  
* Value of the undamaged 40% of the building: $400,000  
* Cost to demolish and clear the undamaged 40% of the building: $50,000  
* Increased cost of construction for wind-resistive upgrades and sprinklers: $150,000

Evaluate this scenario. Analyze how the CP 10 30 base form responds to the local building codes, and then conduct a comparative assessment of how the CP 04 05 endorsement alters the coverage. Formulate an actionable claims payout recommendation, detailing exactly how the costs align with Coverages A, B, and C.

**3\. Rubric**

| Description | Weight | Justification | Met | Failure Reasoning |
| :---- | :---- | :---- | :---- | :---- |
| **Identifies the base policy exclusion.** | Major | Must state that the CP 10 30 base form excludes increased costs from the enforcement of ordinances or laws. | FALSE | Assumed the base policy covered code upgrades. |
| **Evaluates Coverage A application.** | Critical | Must assign the $400,000 value of the undamaged building portion to Coverage A. | FALSE | Failed to cover the undamaged portion. |
| **Evaluates Coverage B application.** | Critical | Must assign the $50,000 demolition cost of the undamaged portion to Coverage B. | FALSE | Miscategorized the demolition costs. |
| **Evaluates Coverage C application.** | Critical | Must assign the $150,000 upgrade costs to Coverage C. | FALSE | Miscategorized the increased cost of construction. |
| **Provides an actionable claims payout recommendation.** | Critical | Must recommend paying all associated costs since the CP 04 05 endorsement was purchased. | FALSE | Denied the claim. |
| **Formats the response as a Claims Determination Memo.** | Minor | Required output styling. | FALSE | Wrote an unstructured paragraph. |

**4\. Model Analysis**

This prompt tests the LLM's ability to execute a Scenario Evaluation involving a complex interaction between base property exclusions and a buy-back endorsement. The model must understand that standard commercial property policies do not cover the cascading financial effects of municipal building codes following a partial loss. It requires the logic to segregate four different financial values into their correct coverage buckets (Base Limit, Coverage A, Coverage B, and Coverage C) and issue an actionable payout directive based on the mechanics of the CP 04 05 endorsement.

**5\. Golden Response**

CLAIMS DETERMINATION MEMO

TO: Claims File

FROM: Commercial Property Claims Adjuster

DATE: June 1, 2025

SUBJECT: Coverage Analysis – Ordinance or Law (CP 04 05\)

1. Scenario Evaluation & Base Policy Analysis The insured suffered a fire resulting in $600,000 of direct physical damage to a building with a $1,000,000 limit. Because the building was 60% destroyed, a local ordinance mandates that the remaining 40% be demolished and the entire structure rebuilt to modern codes. Under the standard Causes of Loss – Special Form (CP 10 30), the policy explicitly excludes loss or damage caused directly or indirectly by the enforcement of or compliance with any ordinance or law regulating the construction, use, or repair of any property, or requiring the tearing down of any property. If only the base policy were active, the insured would solely receive $600,000 for the direct fire damage.  
2. Risk Assessment & Endorsement Application  
   However, the insured purchased the Ordinance Or Law Coverage endorsement (CP 04 05 09 17), which specifically buys back these excluded exposures when a covered peril (fire) forces compliance with a building code. The CP 04 05 provides three distinct coverages that apply directly to this scenario:  
* **Coverage A (Loss to the Undamaged Portion of the Building):** Because the ordinance requires the demolition of the undamaged portion of the building, Coverage A pays for the value of that undamaged 40% (calculated at $400,000). This loss falls within the overall building limit.  
* **Coverage B (Demolition Cost Coverage):** This section covers the expense to demolish and clear the site of the undamaged parts of the building as a consequence of the ordinance. The $50,000 demolition cost applies here.  
* **Coverage C (Increased Cost of Construction Coverage):** This section pays the increased cost to repair or reconstruct both damaged and undamaged portions to meet the minimum standards of the ordinance or law. The $150,000 for wind-resistive upgrades and sprinklers applies here.  
3. Actionable Recommendation  
   I recommend that we approve the entirety of this claim. The $600,000 in direct fire damage is covered under the base CP 00 10 form. The remaining $400,000 value of the undamaged building is covered under Coverage A. The $50,000 demolition cost and the $150,000 upgrade cost are fully covered under Coverages B and C, respectively, subject to the scheduled limits on the endorsement.

### **Example 3: Construction Loan Title Insurance (Mechanic's Lien Priority)**

**1\. Metadata**

Task Type: Workflow

Category / Domain: Title Insurance / Commercial Real Estate

Workflow: Underwriting & Claims

Prompt Type: Coverage Analysis Memo

Difficulty: High

**2\. Prompt**

You are a Commercial Title Insurance Claims Counsel. The current date is May 10, 2025\.

Your insured, a commercial lender, is financing a $50 million high-rise construction project. The lender has an ALTA Loan Policy. During the third month of construction, the lender disbursed $2 million for the latest draw request. The draw documents explicitly designated $500,000 to be paid to the primary steel subcontractor. However, the general contractor misappropriated those funds, and the steel subcontractor never received payment. Consequently, the steel subcontractor filed a valid mechanic's lien against the property, threatening the priority of the lender's mortgage.

The lender's policy contains an ALTA 32 series Construction Loan \- Loss of Priority endorsement.

Review this scenario and draft a Coverage Analysis Memo to the VP of Claims. You must:

1. Conduct a comparative analysis of how coverage would apply under three different endorsement versions: ALTA 32-06, ALTA 32.1-06, and ALTA 32.2-06.  
2. Evaluate the scenario to determine if the lender's loss of priority to the mechanic's lien is covered under each of the three specific endorsement types based on the facts provided.  
3. Assess the risk profile these endorsements present to the title insurer when general contractors handle downstream payments.  
4. Formulate an actionable recommendation for the title underwriting department regarding which specific endorsement they should mandate for future draws on this project to minimize the insurer's exposure.

**3\. Rubric**

| Description | Weight | Justification | Met | Failure Reasoning |
| :---- | :---- | :---- | :---- | :---- |
| **Executes comparative analysis of ALTA 32 series.** | Critical | Must correctly distinguish between "designated for payment" (32-06) and "direct payment" (32.1-06 and 32.2-06) triggers. | FALSE | Failed to identify the contractual differences between the three endorsements. |
| **Evaluates the scenario under ALTA 32-06.** | Major | Must conclude coverage likely applies because the funds were "designated for payment" in the draw documents. | FALSE | Incorrectly denied coverage under 32-06. |
| **Evaluates the scenario under ALTA 32.1-06 and 32.2-06.** | Major | Must conclude coverage is denied under both because "direct payment" to the lien claimant was never made. | FALSE | Incorrectly approved coverage under the direct payment endorsements. |
| **Assesses the risk of contractor misappropriation.** | Minor | Must articulate the risk the title insurer assumes when relying on general contractors to distribute funds downstream. | FALSE | Omitted the risk assessment. |
| **Provides an actionable recommendation.** | Critical | Must recommend switching to ALTA 32.1-06 (or 32.2-06) and requiring direct payments/lien waivers to mitigate future risk. | FALSE | Did not provide a clear underwriting recommendation. |

**4\. Model Analysis**

This prompt significantly elevates logic complexity by forcing the model to compare three closely related, highly technical endorsements within a single factual scenario. It moves beyond simple extraction by requiring the model to deduce the outcome for each endorsement (Scenario Evaluation), analyze the systemic risk of general contractor fund mismanagement (Risk Assessment), and translate that legal analysis into a forward-looking business strategy (Actionable Recommendation).

**5\. Golden Response**

COVERAGE ANALYSIS MEMO

TO: VP of Claims

FROM: Commercial Title Insurance Claims Counsel

DATE: May 10, 2025

SUBJECT: Mechanic's Lien Priority Claim and ALTA 32 Series Evaluation

1. Comparative Analysis: ALTA 32-06 vs. 32.1-06 vs. 32.2-06  
   The ALTA 32 series endorsements are designed to provide limited mechanic's lien coverage for construction loans. Their primary difference lies in the mechanism required to trigger coverage for materials or services provided:  
* **ALTA 32-06:** Protects against the lack of priority of the insured mortgage over a mechanic's lien, but only to the extent that the charges for the services or materials were "designated for payment" in the documents supporting a construction loan advance disbursed on or before the date of coverage.  
* **ALTA 32.1-06:** Limits coverage strictly to instances where "direct payment" to the mechanic's lien claimant has been made by the Title Company or by the Insured with the Title Company's written approval.  
* **ALTA 32.2-06:** Limits coverage to instances where "direct payment" to the mechanic's lien claimant has been made by the Insured or on the Insured's behalf.  
2. Scenario Evaluation  
   Under the facts presented, the lender disbursed funds that were explicitly designated for the steel subcontractor, but the subcontractor was never actually paid.  
* If the policy contains the **ALTA 32-06**, the claim is likely COVERED. The endorsement's requirement was met because the funds were "designated for payment" in the supporting draw documents. The subsequent misappropriation by the general contractor does not void the lender's coverage under this specific form.  
* If the policy contains the **ALTA 32.1-06** or **ALTA 32.2-06**, the claim is DENIED. Both of these endorsements expressly exclude coverage to the extent that the mechanic's lien claimant was not directly paid. Because the steel subcontractor never received direct payment, the loss of priority is entirely excluded.  
3. Risk Assessment  
   The ALTA 32-06 presents a severe risk profile to the title insurer. By relying merely on funds being "designated for payment" on a spreadsheet or draw request, the insurer assumes the massive credit and fraud risk of the general contractor failing to pass those funds down the chain.  
4. Actionable Recommendation  
   To mitigate our exposure going forward, I strongly recommend that the underwriting department immediately mandate the use of the ALTA 32.1-06 endorsement for all future draws on this project. Furthermore, we must transition to a direct disbursement model where the Title Company either pays the subcontractors directly or requires executed, unconditional lien waivers from the subcontractors prior to approving the Insured's disbursement.

---

### **Example 4: Cyber Insurance Business Interruption**

**1\. Metadata**

Task Type: Workflow

Category / Domain: Cyber Insurance / Claims

Workflow: Coverage Verification

Prompt Type: Claims Strategy Report

Difficulty: High

**2\. Prompt**

You are a Senior Cyber Claims Adjuster. The current date is August 22, 2025\.

Your insured, a logistics company, holds an AIG CyberEdge policy. Over the weekend, the insured's IT department deployed a routine firmware update to their internal routing servers. The update contained a critical error that caused an unintentional and unplanned software corruption, bringing the entire network down for 72 hours. No malicious actors, hackers, or viruses were involved.

Due to the downtime, the company suffered a $2.5 million loss in business income. They spent an additional $500,000 upgrading their servers to a newer, more resilient model to prevent this from happening again. They have filed a $3 million Business Interruption claim.

Draft a Claims Strategy Report. You must:

1. Conduct a comparative analysis of the policy definitions for "Security Failure" versus "System Failure" as they relate to Business Interruption triggers.  
2. Evaluate the scenario to determine if the $2.5 million business income loss is covered under either trigger.  
3. Assess the coverage for the $500,000 spent on upgrading the servers.  
4. Formulate an actionable recommendation for settling or denying the various components of this claim.

**3\. Rubric**

| Description | Weight | Justification | Met | Failure Reasoning |
| :---- | :---- | :---- | :---- | :---- |
| **Compares Security Failure and System Failure.** | Critical | Must establish that Security Failure requires unauthorized access/malicious acts, while System Failure covers unintentional/unplanned outages. | FALSE | Failed to differentiate between the two triggers. |
| **Evaluates the business income loss scenario.** | Major | Must identify the event as a System Failure, not a Security Failure, and note that coverage depends on whether System Failure cover was purchased. | FALSE | Categorized the event as a Security Failure. |
| **Assesses the hardware upgrade costs.** | Critical | Must exclude the $500,000 upgrade costs based on policy language prohibiting betterment/upgrades. | FALSE | Allowed coverage for the server upgrades. |
| **Provides an actionable settlement recommendation.** | Major | Must provide a clear directive on how to handle the two distinct financial blocks of the claim. | FALSE | Failed to provide a final claims determination. |

**4\. Model Analysis**

Cyber policies are highly modular. A common misconception is that all IT outages are covered under basic cyber liability. This prompt requires the LLM to perform a Comparative Analysis between a malicious breach and an internal technological failure. The Scenario Evaluation forces the model to categorize a botched firmware update correctly and recognize that a "Material Interruption" occurred. The Risk Assessment targets the exact financial damages, requiring the model to separate covered business income from excluded betterment (the server upgrades). The output demands an Actionable Recommendation for the claims team to execute.

**5\. Golden Response**

CLAIMS STRATEGY REPORT

TO: Cyber Claims Committee

FROM: Senior Cyber Claims Adjuster

DATE: August 22, 2025

SUBJECT: Business Interruption Claim – Firmware Update Failure

1. Comparative Analysis: Security Failure vs. System Failure  
   Under the standard AIG CyberEdge framework, Business Interruption coverage is modular and depends on the specific trigger causing the outage:  
* **Security Failure:** This trigger involves unauthorized access or use of the computer system, receipt or transmission of malicious code/viruses, or the physical theft of hardware.  
* **System Failure:** This trigger involves an unintentional and unplanned failure, or the unauthorized reprogramming or corruption of software that renders the computer system non-functional or useless for its intended purpose.  
2. Scenario Evaluation: $2.5 Million Business Income Loss  
   The insured's 72-hour network outage was caused by a routine firmware update that resulted in software corruption. There is no evidence of a hacker, malicious code, or unauthorized access. Therefore, this event does not qualify as a Security Failure. Instead, this is a textbook System Failure, as it was an unintentional and unplanned event that corrupted the software and caused a "Material Interruption" to the company's network. The $2.5 million loss in business income is only covered if the insured specifically purchased "System Failure Cover" as an optional module on their policy.  
3. Risk Assessment: $500,000 Server Upgrades  
   The insured is also claiming $500,000 for purchasing newer, more resilient servers following the outage. Cyber policies are designed to indemnify for actual loss sustained, not to fund IT capital improvements. The policy definition of "Loss" explicitly excludes costs or expenses arising out of updating, upgrading, enhancing, improving, or replacing any Computer System to a level beyond that which existed prior to the triggering event.  
4. Actionable Recommendation  
   I recommend the following claims handling actions:  
* **Regarding the $500,000 Upgrade:** Issue an immediate partial denial. The cost to upgrade the servers to a better model is strictly excluded from the definition of Loss.  
* **Regarding the $2.5 Million Income Loss:** Review the insured's Declarations Page immediately to verify if "System Failure Cover" was purchased. If System Failure Cover is active, proceed with quantifying the exact net profit loss and continuing operating expenses. If only Security Failure Cover was purchased, the entire claim must be denied, as the outage lacked a malicious or unauthorized trigger.

---

### **Example 5: Reinsurance Sliding Scale Commission Calculation**

**1\. Metadata**

Task Type: Workflow

Category / Domain: Reinsurance / Actuarial

Workflow: Treaty Pricing

Prompt Type: Actuarial Memo

Difficulty: High

**2\. Prompt**

You are a Reinsurance Actuary evaluating a treaty renewal. The current date is August 1, 2025\.

A ceding company is renewing its quota share treaty. The proposed sliding scale commission terms are as follows:

* Minimum Commission: 25% at a 60% current year loss ratio.  
* Sliding 1:1 to 35% at a 50% current year loss ratio.  
* Sliding 0.5:1 to a Maximum of 45% at a 30% current year loss ratio.

The ceding company has a loss carryforward from prior years amounting to a 5% addition to the loss ratio. The actual current year loss ratio before the carryforward is 35%.

Review this scenario. Calculate the final adjusted commission rate. Compare this sliding scale structure to a traditional fixed commission structure, assessing the risk transfer implications and alignment of interests between the cedent and reinsurer. Finally, formulate an actionable recommendation for the Chief Risk Officer on whether to bind this treaty or negotiate a flat commission.

**3\. Rubric**

| Description | Weight | Justification | Met | Failure Reasoning |
| :---- | :---- | :---- | :---- | :---- |
| **Calculates the adjusted loss ratio.** | Critical | Must add the 5% carryforward to the 35% actual loss ratio to reach a 40% adjusted loss ratio. | FALSE | Failed to apply the carryforward provision. |
| **Calculates the correct sliding scale commission.** | Critical | Must calculate the commission based on the 40% loss ratio (35% \+ (50% \- 40%) \* 0.5 \= 40%). | FALSE | Miscalculated the sliding scale math. |
| **Executes comparative analysis of commission structures.** | Major | Must contrast the risk-sharing nature of sliding scales with the guaranteed cost of fixed commissions. | FALSE | Omitted the comparative analysis. |
| **Assesses underwriting risk.** | Major | Must explain how the sliding scale acts as a reward for profitability and mitigates reinsurer exposure. | FALSE | Did not assess the risk transfer implications. |
| **Provides an actionable recommendation.** | Critical | Must deliver a definitive business decision regarding the treaty structure. | FALSE | Failed to provide a recommendation. |

**4\. Model Analysis**

This prompt tests the model's ability to perform multi-step actuarial calculations and synthesize the results into a strategic business recommendation. The model must first understand that a carryforward shifts the slide by the amount of the carryforward. It must accurately calculate the adjusted loss ratio (35% \+ 5% \= 40%) and apply the tiered sliding scale formula. Because the 40% loss ratio falls in the 0.5:1 sliding range (between 50% and 30%), the commission increases by 0.5% for every 1% the loss ratio drops below 50%. The model must then execute a comparative analysis of risk, recognizing that sliding scale commissions adapt the commission to the profitability of the ceded business.

**5\. Golden Response**

ACTUARIAL MEMO

TO: Chief Risk Officer

FROM: Reinsurance Actuary

DATE: August 1, 2025

SUBJECT: Treaty Renewal – Sliding Scale Commission Analysis

1. Actuarial Calculation  
   The proposed treaty includes a loss carryforward provision. A carryforward from prior years effectively shifts the slide by adding directly to the current year's loss ratio.  
* Current Year Loss Ratio: 35%  
* Carryforward: \+5%  
* Adjusted Loss Ratio: 40%

Using the adjusted loss ratio of 40%, we calculate the commission based on the treaty's tiers:

* At a 50% loss ratio, the commission is 35%.  
* For loss ratios between 50% and 30%, the commission slides at a 0.5:1 ratio.  
* The adjusted loss ratio (40%) is 10 points below the 50% threshold.  
* 10 points \* 0.5 \= 5% additional commission.  
* Final Commission Rate \= 35% \+ 5% \= 40%.  
2. Comparative Analysis and Risk Assessment  
   A sliding scale commission calculation is a system designed to adapt the reinsurance commission to the actual profitability of the ceded business, utilizing the relationship between incurred losses and earned premium. In a highly profitable year, the sliding scale acts as a "reward" granted by the reinsurer to the cedent.  
   Compared to a fixed flat commission, a sliding scale aligns the underwriting interests of both parties. If we accept a flat commission, we lock in our acquisition cost recovery but forfeit the upside of exceptional underwriting performance. Under this sliding scale, we share the risk: the reinsurer is protected from severe losses by the 25% minimum commission floor, while we are incentivized to maintain strict underwriting discipline to achieve the 45% maximum commission.  
3. Actionable Recommendation  
   I recommend that we bind the treaty with the proposed sliding scale commission. Based on our current year performance, we are securing a 40% commission rate even after absorbing the 5% historical carryforward penalty. This structure effectively mitigates the reinsurer's downside risk while providing us with superior financial leverage compared to standard flat commission offerings in the current market.

---

### **Example 6: Life Insurance 1035 Exchange and Cost Basis Carryover**

**1\. Metadata**

Task Type: Workflow

Category / Domain: Life Insurance / Taxation

Workflow: Policy Servicing

Prompt Type: Tax Evaluation Memo

Difficulty: High

**2\. Prompt**

You are a Life Insurance Compliance Officer. The current date is May 15, 2025\.

A client owns a non-qualified annuity contract and wishes to exchange it for a new annuity contract via an IRC Section 1035 tax-free exchange. The original contract has a cost basis that is higher than its current gross cash value. Furthermore, the client currently has an outstanding loan on the original policy and intends to extinguish the loan entirely during the exchange process rather than carrying it over to the new policy.

Evaluate this scenario. Conduct a comparative analysis of executing this transaction as a 1035 exchange versus surrendering the policy and purchasing a new one. Assess the specific tax risks and cost basis implications of extinguishing the loan during the exchange. Formulate an actionable recommendation for the client's financial advisor to ensure the transaction complies with IRS regulations and optimizes the client's tax position.

**3\. Rubric**

| Description | Weight | Justification | Met | Failure Reasoning |
| :---- | :---- | :---- | :---- | :---- |
| **Analyzes the 1035 exchange basis carryover rules.** | Major | Must explain that a 1035 exchange allows the policy owner to carry over the higher basis into the new contract. | FALSE | Failed to address the preservation of the higher cost basis. |
| **Evaluates the impact of extinguishing the loan.** | Critical | Must state that extinguishing the loan reduces the original policy's basis that is carried over to the new policy. | FALSE | Missed the tax/basis implications of the loan extinguishment. |
| **Compares a 1035 exchange to a standard surrender.** | Major | Must contrast the non-recognition of gain/loss in a 1035 exchange with the taxable nature of a standard surrender. | FALSE | Did not compare the 1035 exchange to a surrender. |
| **Provides an actionable recommendation.** | Critical | Must recommend executing the 1035 exchange while advising on the basis reduction due to the loan. | FALSE | Failed to provide a recommendation for the advisor. |

**4\. Model Analysis**

This prompt requires the model to navigate complex IRS regulations regarding life insurance and annuities. The model must perform a Scenario Evaluation of an IRC Section 1035 exchange. It must understand that Section 1035(a)(3) provides that no gain or loss is recognized on the exchange of an annuity contract for another annuity contract. Crucially, the model must execute a Risk Assessment regarding the extinguished loan. It must explain that if the basis of the original contract is higher than its gross cash value, a 1035 exchange allows the preservation of that higher basis, but extinguishing a loan will decrease the carried-over basis to the extent the loan exceeds the policy's gain. The model must then translate this into an Actionable Recommendation for the advisor.

**5\. Golden Response**

TAX EVALUATION MEMO

TO: Financial Advisor Team

FROM: Life Insurance Compliance Officer

DATE: May 15, 2025

SUBJECT: Section 1035 Exchange – Loan Extinguishment and Cost Basis

1. Comparative Analysis: 1035 Exchange vs. Surrender  
   Under IRC Section 1035, the exchange of an annuity contract for another annuity contract is treated as a non-taxable event, provided the contracts are exchanged directly between the insurance companies. If the client were to surrender the policy, receive a check, and use the proceeds to buy a new contract, the transaction would not qualify as a 1035 exchange, and any amounts received would be taxed on an income-first basis under Section 72(e).

Because the original contract's basis is higher than its gross cash value, utilizing a 1035 exchange is highly advantageous. A 1035 exchange allows the policy owner to carry over the higher basis into the new contract, meaning the new contract will have a higher basis than the amount actually placed into it.

2. Risk Assessment: Extinguishing the Loan  
   While the 1035 exchange preserves the basis, the client's intention to extinguish the outstanding loan complicates the transaction. If a loan is extinguished in the exchange, the amount of the original policy's basis that is carried over to the new policy will decrease to the extent the loan exceeds the policy's gain. Any reduction in the cost basis will increase the taxable amount of future distributions from the new policy. Furthermore, the reduction cannot exceed the policy's basis without triggering a taxable event.  
3. Actionable Recommendation  
   I recommend proceeding with the 1035 exchange to preserve the higher cost basis and avoid the immediate tax consequences of a surrender. However, you must advise the client that extinguishing the loan during the transfer will result in a permanent reduction of their carried-over cost basis. To optimize the tax position, evaluate whether the client has the liquidity to repay the loan prior to initiating the exchange; doing so would allow the new contract to accumulate more cash value with higher surrender proceeds and fully preserve the original high cost basis. Ensure the exchange is processed directly from carrier to carrier to satisfy IRS strict interpretation rules.

---

### **Example 7: York-Antwerp Rules 2016 General Average Scenario**

**1\. Metadata**

Task Type: Workflow

Category / Domain: Marine Insurance / Claims

Workflow: Claims Adjustment

Prompt Type: General Average Adjustment Report

Difficulty: High

**2\. Prompt**

You are a Marine Average Adjuster. The current date is November 10, 2025\.

A commercial cargo vessel was caught in a severe storm. To prevent the vessel from sinking and to save the cargo, the Master intentionally ran the ship on shore (a voluntary stranding). The vessel was successfully beached. The next day, the crew attempted to refloat the vessel by heavily overworking the propelling machinery and boilers. During this attempt to refloat, the ship's boilers were severely damaged.

Evaluate this scenario under the York-Antwerp Rules 2016\. Conduct a comparative analysis of how the Rules treat the damages from the intentional stranding versus the damages sustained by the machinery during the refloating attempt. Assess the financial liabilities of the cargo owners regarding General Average contributions for both events. Formulate an actionable adjustment recommendation on which specific losses should be admitted as General Average.

**3\. Rubric**

| Description | Weight | Justification | Met | Failure Reasoning |
| :---- | :---- | :---- | :---- | :---- |
| **Identifies Rule V (Voluntary Stranding).** | Critical | Must state that intentional stranding for common safety is admitted as General Average. | FALSE | Failed to analyze the voluntary stranding under Rule V. |
| **Identifies Rule VII (Damage to Machinery and Boilers).** | Critical | Must state that machinery damage while ashore attempting to refloat is admitted as General Average. | FALSE | Failed to analyze the machinery damage under Rule VII. |
| **Conducts a comparative analysis of the damages.** | Major | Must compare the two distinct sacrifices and confirm both meet the criteria for common safety. | FALSE | Did not compare the two types of damage. |
| **Assesses cargo owner liability.** | Major | Must explain that cargo owners are proportionally liable for both sacrifices. | FALSE | Ignored the financial impact on the contributing interests. |
| **Provides an actionable adjustment recommendation.** | Critical | Must recommend allowing both the stranding damage and the boiler damage in the General Average adjustment. | FALSE | Failed to provide a final adjustment directive. |

**4\. Model Analysis**

This prompt requires the model to apply international maritime law (York-Antwerp Rules 2016\) to a complex factual scenario. The model must perform a Scenario Evaluation to determine that the intentional stranding falls under Rule V, which states that consequent loss or damage to property involved in the common maritime adventure shall be allowed in general average. The model must also evaluate the subsequent boiler damage under Rule VII, which explicitly allows damage caused to machinery and boilers of a ship which is ashore in endeavoring to refloat for common safety. The model must assess the risk to the cargo owners (who must contribute to both) and provide an actionable adjustment ruling.

**5\. Golden Response**

GENERAL AVERAGE ADJUSTMENT REPORT

TO: Interested Parties (Hull and Cargo)

FROM: Marine Average Adjuster

DATE: November 10, 2025

SUBJECT: General Average Assessment – Voluntary Stranding and Machinery Damage

1. Scenario Evaluation and Rule Application  
   This incident involves two distinct events that must be evaluated under the York-Antwerp Rules 2016 to determine if they qualify as General Average acts, which require extraordinary sacrifices intentionally and reasonably made for the common safety to preserve property from peril.  
* **The Intentional Stranding:** The Master's decision to intentionally run the ship on shore to prevent sinking falls directly under Rule V – Voluntary Stranding. Under Rule V, when a ship is intentionally run on shore for common safety, the consequent loss or damage to the property involved in the common maritime adventure shall be allowed in General Average.  
* **The Machinery and Boiler Damage:** The subsequent damage to the boilers occurred while the vessel was ashore and attempting to refloat. This is governed by Rule VII – Damage to Machinery and Boilers. Rule VII explicitly dictates that damage caused to machinery and boilers of a ship which is ashore and in a position of peril, in endeavoring to refloat, shall be allowed in General Average when shown to have arisen from an actual intention to float the ship for the common safety at the risk of such damage.  
2. Comparative Analysis and Financial Risk Assessment  
   Both the initial stranding and the subsequent machinery damage share a unified objective: the preservation of the common maritime adventure. Unlike machinery damage that occurs while a ship is afloat (which is strictly excluded under Rule VII), damage incurred while ashore to force a refloating is treated as an intentional sacrifice. Consequently, the financial risk is distributed across all contributing interests. The cargo owners will be required to bear a proportional share of the costs for repairing the hull damage caused by the beaching and the cost of replacing the destroyed boilers, calculated based on the arrived value of the saved cargo.  
3. Actionable Adjustment Recommendation  
   I recommend that both the damages resulting from the voluntary stranding and the damages to the boilers sustained during the refloating attempt be formally admitted as General Average. The adjustment process will proceed by calculating the total value of these sacrifices and assessing proportional contributions from the shipowner, the cargo owners, and any pending freight interests.

---

### **Example 8: Lawyers Professional Liability Fee Dispute Exclusion**

**1\. Metadata**

Task Type: Workflow

Category / Domain: Professional Liability / Claims

Workflow: Coverage Determination

Prompt Type: Coverage Strategy Memo

Difficulty: High

**2\. Prompt**

You are Claims Counsel for a Professional Liability Insurer. The current date is February 2, 2025\.

Your insured, a mid-sized law firm, is facing a lawsuit from a former client. The complaint alleges two distinct causes of action:

1. Legal malpractice, claiming the firm's negligence in drafting a contract caused the client a $500,000 economic loss.  
2. Disgorgement of fees, demanding the return of $50,000 in legal fees previously paid by the client, arguing the fees were unreasonable given the negligent work.

The insured's Lawyers Professional Liability (LPL) policy contains a standard "Fee Dispute" exclusion that states the policy does not apply to "any dispute over fees or costs, or any claim that seeks whether directly or indirectly the return, reimbursement, or disgorgement of fees."

Draft a Coverage Strategy Memo. You must conduct a comparative analysis of how the policy responds to the malpractice damages versus the demand for fee disgorgement. Evaluate the "dual capacity" of the complaint. Assess the financial risk to the insurer regarding the duty to defend a mixed claim. Formulate an actionable recommendation on whether to provide a defense to the insured law firm, and detail any necessary reservations of rights.

**3\. Rubric**

| Description | Weight | Justification | Met | Failure Reasoning |
| :---- | :---- | :---- | :---- | :---- |
| **Analyzes the fee dispute exclusion.** | Critical | Must correctly apply the exclusion to bar coverage for the $50,000 fee disgorgement demand. | FALSE | Failed to apply the fee dispute exclusion. |
| **Differentiates between covered and uncovered claims.** | Major | Must recognize that the $500,000 malpractice claim remains covered despite the fee dispute exclusion. | FALSE | Incorrectly denied coverage for the malpractice claim. |
| **Evaluates the duty to defend a mixed claim.** | Critical | Must explain that the presence of the covered malpractice claim triggers a duty to defend the entire suit. | FALSE | Failed to acknowledge the broad duty to defend mixed claims. |
| **Provides an actionable recommendation with a Reservation of Rights.** | Critical | Must recommend defending the suit while issuing a Reservation of Rights regarding the fee disgorgement. | FALSE | Did not include a Reservation of Rights recommendation. |

**4\. Model Analysis**

This prompt tests the model's ability to navigate complex mixed-claim scenarios in professional liability insurance. Fee dispute exclusions are a significant minority feature in LPL policies, designed to prevent insurers from becoming collection agencies or refunding unearned fees. The model must perform a Scenario Evaluation to separate the $50,000 disgorgement (excluded) from the $500,000 malpractice damage (covered). The Risk Assessment must address the legal concept of the "duty to defend," recognizing that if even one allegation is potentially covered, the insurer must typically defend the entire lawsuit. The Actionable Recommendation requires directing the claims team to issue a Reservation of Rights (ROR) letter, agreeing to defend the firm but refusing to indemnify the fee disgorgement portion.

**5\. Golden Response**

COVERAGE STRATEGY MEMO

TO: LPL Claims Department

FROM: Claims Counsel

DATE: February 2, 2025

SUBJECT: Coverage Determination – Malpractice and Fee Disgorgement

1. Comparative Analysis of the Complaint  
   The complaint against our insured law firm presents a mixed claim involving two distinct causes of action: a tort claim for professional negligence ($500,000) and a contractual/equitable claim for the disgorgement of legal fees ($50,000).  
   Our Lawyers Professional Liability policy is designed to cover damages arising out of wrongful acts, errors, or omissions in the performance of legal services. However, the policy contains a strict "Fee Dispute" exclusion, which precludes coverage for claims seeking the return, reimbursement, or disgorgement of fees. While the client alleges that they are entitled to disgorgement of fees because the attorney's malpractice caused them to pay for unnecessary work, the exclusion applies to the $50,000 demand regardless of the underlying rationale for the fee dispute.  
2. Scenario Evaluation and Risk Assessment  
   The financial risk to the insurer requires carefully segregating the indemnity exposure from the defense obligation.  
* **Indemnity Risk:** We have zero indemnity exposure for the $50,000 fee disgorgement demand. However, the $500,000 malpractice claim falls squarely within the policy's insuring agreement and is not barred by the fee dispute exclusion.  
* **Defense Risk:** In professional liability insurance, the duty to defend is broader than the duty to indemnify. Because the lawsuit contains at least one covered allegation (the $500,000 malpractice claim), we are legally obligated to provide a defense for the entire lawsuit, including the uncovered fee dispute portion.  
3. Actionable Recommendation  
   I recommend that we immediately accept the defense of the insured law firm, as a failure to defend the covered malpractice claim would result in bad faith exposure. However, we must issue a comprehensive Reservation of Rights (ROR) letter. The ROR must explicitly cite the "Fee Dispute" exclusion and inform the insured that while we will fund the defense of the entire action, we will not indemnify or pay any judgment or settlement amount that represents the return, reimbursement, or disgorgement of the $50,000 in legal fees.

---

### **Example 9: Workers Compensation and the Dual Capacity Doctrine**

**1\. Metadata**

Task Type: Workflow

Category / Domain: Workers Compensation / Legal

Workflow: Claims Litigation Strategy

Prompt Type: Legal Analysis Memo

Difficulty: High

**2\. Prompt**

You are a Workers Compensation Defense Attorney. The current date is September 10, 2025\.

Your client, an industrial manufacturing corporation, is facing a complex liability scenario. One of their employees was severely injured on the factory floor while operating a specialized piece of heavy machinery. The employee filed for and is currently receiving statutory workers' compensation benefits.

However, the specialized machinery was actually designed and manufactured by your client (the employer) and is also sold to the general public. The injured employee has now filed a separate civil product liability lawsuit against your client, seeking general and punitive damages for the defective design of the machinery.

Evaluate this scenario. Conduct a comparative analysis of the exclusive remedy protections of the workers' compensation system versus the legal threat posed by the "Dual Capacity" doctrine. Assess the financial and legal risks this civil suit poses to the employer's general liability and workers' compensation policies. Formulate an actionable legal defense strategy recommendation.

**3\. Rubric**

| Description | Weight | Justification | Met | Failure Reasoning |
| :---- | :---- | :---- | :---- | :---- |
| **Analyzes the exclusive remedy provision.** | Critical | Must define that workers' compensation is generally a no-fault system that shields employers from civil lawsuits by employees. | FALSE | Failed to explain the workers' comp exclusive remedy. |
| **Explains the Dual Capacity doctrine.** | Critical | Must define dual capacity as an exception where an employer owes a separate duty to the employee (e.g., as a product manufacturer). | FALSE | Did not identify or explain the dual capacity doctrine. |
| **Executes comparative risk assessment.** | Major | Must contrast the limited statutory payouts of workers' comp against the unlimited exposure of civil product liability. | FALSE | Omitted the risk assessment comparing the two exposures. |
| **Provides an actionable defense strategy.** | Major | Must provide a recommendation on how to defend the civil suit or navigate the policy exclusions. | FALSE | Failed to provide a legal or insurance strategy. |

**4\. Model Analysis**

This prompt elevates logic complexity by requiring the model to analyze the intersection of statutory workers' compensation law and common law torts. The model must establish the baseline rule: workers' compensation is an "exclusive remedy" protecting employers from personal injury suits. It must then evaluate the specific scenario using the "dual capacity" doctrine, which treats the employer as having a second persona (a manufacturer) owing a duty of care to the public. The Risk Assessment must highlight that dual capacity strips the employer of their workers' comp immunity, exposing them to massive civil damages. The Actionable Recommendation involves strategizing the defense or engaging the employer's Commercial General Liability (CGL) carrier.

**5\. Golden Response**

LEGAL ANALYSIS MEMO

TO: Corporate Risk Management Team

FROM: Workers Compensation Defense Attorney

DATE: September 10, 2025

SUBJECT: Employer Liability – Application of the Dual Capacity Doctrine

1. Comparative Analysis: Exclusive Remedy vs. Dual Capacity  
   Historically, the workers' compensation system operates as a "grand bargain" and a no-fault insurance system. Employees receive guaranteed, limited benefits for workplace injuries, and in exchange, employers are shielded by "exclusive remedy" provisions that prevent employees from bringing personal injury civil lawsuits against them.

However, the employee's civil lawsuit relies on a recognized exception to this rule: the "Dual Capacity" doctrine. This doctrine asserts that an employer can be sued beyond the scope of workers' compensation if they occupy an alternative, second capacity in which they owe a separate duty of care to the general public. Because your corporation manufactured the defective machinery and sells it to the public, the employee is suing the company not in its capacity as an "employer," but in its dual capacity as a "product manufacturer".

2. Risk Assessment of Liability Exposure  
   This scenario presents a severe financial risk. While the workers' compensation policy covers the employee's medical bills and lost wages on a strict statutory schedule, a successful dual capacity product liability lawsuit bypasses those caps. If the court allows the civil suit to proceed, the corporation faces unlimited exposure for pain and suffering, loss of consortium, and potential punitive damages—none of which are covered by the workers' compensation policy. Furthermore, standard Employers Liability (Part Two of the WC policy) often excludes damages arising out of dual capacity scenarios, leaving the corporation vulnerable.  
3. Actionable Defense Strategy Recommendation  
   I recommend a two-pronged defense strategy:  
* **Legal Defense:** We must file an immediate motion for summary judgment to dismiss the civil suit, arguing against the application of the dual capacity doctrine in this jurisdiction. We will argue that the injury arose strictly out of the employment relationship and that the machinery was provided to the employee solely as a tool of their trade, thereby reinforcing the exclusive remedy shield.  
* **Insurance Coordination:** Immediately tender the product liability complaint to the corporation's Commercial General Liability (CGL) carrier. While CGL policies contain an "Employer's Liability" exclusion for injuries to employees, the unique nature of a dual capacity claim (suing the company as a manufacturer) may trigger the CGL policy's duty to defend.

### **Example 10: Commercial Property Proportionate Loss Allocation (Ordinance or Law)**

**1\. Metadata**

Task Type: Workflow

Category / Domain: Commercial Property Claims

Workflow: Coverage and Quantum Calculation

Prompt Type: Claims Adjustment Memo

Difficulty: Expert

**2\. Prompt**

You are a Commercial Property General Adjuster. The current date is May 10, 2025\.

Your insured's commercial building has a Replacement Cost value of $1,000,000. Following a severe storm, the building suffered total physical damage of $600,000. The adjuster's investigation determined that $150,000 of the damage was caused by wind (a covered peril), and $450,000 of the damage was caused by storm surge/flood (an excluded peril).

Because the total physical damage exceeded 50% of the building's value, the local municipality enforced a building code requiring the immediate demolition of the remaining undamaged 40% of the building. The insured has an ISO CP 00 10 policy with a $1,000,000 building limit, endorsed with the ISO CP 04 05 09 17 Ordinance or Law Coverage. They have a combined limit of $200,000 for Coverages B (Demolition) and C (Increased Cost of Construction). The cost to demolish the undamaged portion is $40,000, and the increased cost to rebuild to code is $80,000 (Total Ordinance loss \= $120,000).

Draft a Claims Adjustment Memo. You must:

1. Conduct a comparative analysis of how this loss would be treated under an unendorsed CP 00 10 versus a policy endorsed with the CP 04 05\.  
2. Evaluate the mixed-causation scenario using the specific Proportionate Loss rules outlined in the CP 04 05 09 17 endorsement.  
3. Assess the specific financial risk and calculate the exact dollar amount payable by the insurer for the combined Coverage B & C expenses.  
4. Formulate an actionable recommendation for the final payout of the Ordinance or Law claim, detailing the mathematical steps.

**3\. Rubric**

| Description | Weight | Justification | Met | Failure Reasoning |
| :---- | :---- | :---- | :---- | :---- |
| **Compares CP 00 10 vs CP 04 05 coverage.** | Major | Must explain that standard CP 00 10 excludes ordinance costs, necessitating the CP 04 05 endorsement for demolition/upgrade costs. | FALSE | Failed to differentiate between the base form and the endorsement. |
| **Identifies the Proportionate Loss rule.** | Critical | Must state that when a loss involves both covered and excluded perils, the CP 04 05 pays only a proportional share of the ordinance costs. | FALSE | Ignored the mixed-causation limitation in the endorsement. |
| **Calculates the correct damage proportion.** | Critical | Must divide the covered damage ($150,000) by the total damage ($600,000) to arrive at 25%. | FALSE | Calculated the proportion incorrectly. |
| **Calculates the correct final payout.** | Critical | Must multiply the 25% proportion by the $120,000 total ordinance loss to arrive at a $30,000 payout. | FALSE | Failed to calculate the final dollar amount. |
| **Provides an actionable claims recommendation.** | Major | Must recommend issuing a payment of $30,000 for the Coverage B and C components. | FALSE | Did not provide a clear payout directive. |

**4\. Model Analysis**

This prompt forces the LLM to apply advanced policy mechanics to a mathematical scenario. A less capable model will simply see that the $120,000 ordinance loss is under the $200,000 limit and award the full amount. To succeed, the model must execute a Scenario Evaluation based on the ISO CP 04 05 provisions, which dictate that when an ordinance is triggered by a mix of covered and non-covered damage, the insurer only pays the proportion that the covered damage bears to the total damage. The model must perform the calculation (150k / 600k \= 25%) and correctly assess the insurer's liability at $30,000.

**5\. Golden Response**

CLAIMS ADJUSTMENT MEMO

TO: Claims File

FROM: Commercial Property General Adjuster

DATE: May 10, 2025

SUBJECT: Ordinance or Law Claim – Proportionate Loss Calculation

1. Comparative Analysis: CP 00 10 vs. CP 04 05 Under an unendorsed ISO CP 00 10 Building and Personal Property Coverage Form, losses resulting from the enforcement of any building ordinance or law are strictly excluded. If the insured had not purchased the Ordinance or Law Coverage endorsement, the insurer would owe nothing for the demolition of the undamaged portion of the building or the required code upgrades. Because the insured purchased the CP 04 05 09 17 endorsement, Coverages B (Demolition Cost) and Coverage C (Increased Cost of Construction) are activated to cover these exact regulatory exposures.  
2. Scenario Evaluation: Mixed Causation  
   This claim presents a mixed-causation scenario involving both a covered peril (wind) and an excluded peril (flood). The CP 04 05 endorsement contains specific language addressing situations where a building ordinance is enforced due to a combination of covered and non-covered damage. The policy dictates that the insurer will not pay the full ordinance loss; instead, it will only pay a proportion of the loss.  
3. Risk Assessment and Financial Calculation  
   To assess the insurer's financial liability, we must calculate the proportionate loss as required by the CP 04 05 endorsement:  
* Step 1: Determine the total physical damage to the building ($600,000).  
* Step 2: Determine the covered direct physical damage ($150,000).  
* Step 3: Determine the proportion that the covered damage bears to the total damage ($150,000 / $600,000 \= 25%).  
* Step 4: Apply that proportion to the total Ordinance or Law loss (Coverages B & C \= $120,000).  
* Final Calculation: 25% of $120,000 \= $30,000.  
4. Actionable Recommendation  
   I recommend issuing a partial denial for the non-covered portion of the ordinance expenses and releasing a final payout of $30,000 for the Coverage B and C components. While the total ordinance loss was $120,000 and within the insured's $200,000 limit, the proportionate loss rule caps our liability to the extent that our covered peril contributed to the enforcement of the demolition order.

---

### **Example 11: Fiduciary Liability Public Entity vs ERISA Exposure**

**1\. Metadata**

Task Type: Workflow

Category / Domain: Executive Liability / Public Sector

Workflow: Liability Underwriting

Prompt Type: Risk Strategy Memo

Difficulty: Expert

**2\. Prompt**

You are a Fiduciary Liability Underwriting Director. The current date is August 14, 2025\.

A large municipal government (a public entity) is applying for a Fiduciary Liability policy. They sponsor a self-funded health and welfare plan for their employees. Recently, an internal audit revealed two issues:

1. An administrative error caused the plan to systematically underpay out-of-network medical service providers over the last three years. The municipality is currently facing a class-action lawsuit under state common law for breach of fiduciary duty seeking compensatory and punitive damages.  
2. The municipality wishes to correct a separate, unrelated administrative defect by entering into a government Voluntary Compliance Program, which will cost $100,000 in legal and correction fees.

Draft a Risk Strategy Memo. You must:

1. Conduct a comparative analysis of the liability exposure for a public entity plan versus a private, ERISA-governed plan.  
2. Assess the underwriting risk of the state law breach of fiduciary duty claim, specifically addressing the threat of punitive damages.  
3. Evaluate the scenario regarding the $100,000 Voluntary Compliance Program costs under standard fiduciary liability insuring agreements.  
4. Formulate an actionable recommendation on whether to bind this risk, detailing any necessary sublimits or exclusions required to protect the insurer's capital.

**3\. Rubric**

| Description | Weight | Justification | Met | Failure Reasoning |
| :---- | :---- | :---- | :---- | :---- |
| **Compares Public Entity vs ERISA plans.** | Critical | Must identify that public entity plans are exempt from ERISA and governed by state/common law. | FALSE | Failed to note the ERISA exemption for municipalities. |
| **Assesses the risk of state law claims.** | Major | Must articulate that avoiding ERISA exposes the entity to broader state law remedies, including punitive damages. | FALSE | Did not identify the risk of punitive or consequential damages under state law. |
| **Evaluates Voluntary Compliance Program coverage.** | Major | Must confirm that modern Fiduciary policies offer coverage/sublimits for Voluntary Settlement Programs. | FALSE | Denied coverage for voluntary compliance without justification. |
| **Provides an actionable underwriting recommendation.** | Critical | Must provide a clear decision on binding the risk and suggest terms (e.g., adding sublimits or punitive damage exclusions). | FALSE | Did not provide a definitive binding strategy. |

**4\. Model Analysis**

This prompt requires the model to understand the jurisdictional boundaries of the Employee Retirement Income Security Act (ERISA). It must perform a Comparative Analysis noting that public/governmental entities are exempt from ERISA. The Risk Assessment must highlight that while ERISA strictly limits plaintiffs to recovering lost benefits, state common law allows for massive punitive and consequential damage awards, making public plans inherently riskier to underwrite. The model must also recognize that Voluntary Compliance Program expenditures are affirmatively covered under modern fiduciary policies, usually via a specific insuring agreement and sublimit. The output must culminate in an actionable business strategy.

**5\. Golden Response**

RISK STRATEGY MEMO

TO: Public Sector Underwriting Team

FROM: Fiduciary Liability Underwriting Director

DATE: August 14, 2025

SUBJECT: Public Entity Fiduciary Risk Assessment – Breach & Voluntary Compliance

1. Comparative Analysis: Public Entity vs. ERISA Plans  
   Public-entity benefit plans operate in a vastly different legal landscape than private corporate plans. Public-entity plans are typically created by statute and are entirely exempt from the Employee Retirement Income Security Act (ERISA). Instead, they are subject to the laws of the jurisdiction where the plan was created. Because they do not fall under ERISA, these plans lack the protective boundaries established by federal law, meaning the standard of conduct and remedies for breach are dictated by state statutes or common law.  
2. Risk Assessment: State Law Breach and Punitive Damages  
   The state law lawsuit alleging systemic underpayment to out-of-network providers represents a severe exposure. Under a qualified ERISA plan, plaintiffs are statutorily barred from recovering consequential or punitive damages. Furthermore, ERISA's exclusivity provision preempts state law claims. Because this municipality is exempt from ERISA, it does not get the benefit of these limitations. Consequently, the fiduciaries face unlimited liability exposure for state law claims, including common law breach of fiduciary duty, and are entirely vulnerable to the plaintiffs' demands for punitive damages.  
3. Scenario Evaluation: Voluntary Compliance Program Costs  
   Regarding the $100,000 in costs to correct the unrelated administrative defect, modern fiduciary liability policies contain specific Insuring Agreements for Settlement Programs. These provisions affirmatively cover voluntary settlement and defense costs resulting from a notice given to the insurer regarding a government-sponsored voluntary compliance program. Assuming the policy contains this standard insuring agreement and an adequate sublimit (e.g., $250,000), these correction fees would be covered.  
4. Actionable Recommendation  
   Given the ongoing class-action litigation and the severe exposure to un-capped state law punitive damages, I recommend that we decline to bind this risk at this time. If competitive pressure requires us to offer a quote, we must enforce strict risk-mitigation terms. We must attach an Absolute Punitive Damages Exclusion endorsement to shield our capital from the state-level tort exposure. Additionally, we should cap the Voluntary Compliance Program sublimit at $100,000 to minimize our exposure to their ongoing administrative corrections.

---

### **Example 12: Commercial Auto vs. Mobile Equipment Classification**

**1\. Metadata**

Task Type: Workflow

Category / Domain: Commercial Auto / General Liability

Workflow: Coverage Verification

Prompt Type: Claims Strategy Memo

Difficulty: Expert

**2\. Prompt**

You are a Claims Legal Director for a large commercial insurer. The date is October 15, 2025\.

Your insured, a municipality, operates a fleet of self-propelled street cleaning vehicles. One of these vehicles was involved in a severe at-fault accident on a public highway, causing $500,000 in bodily injury to a third party.

The municipality filed the claim under their Commercial General Liability (CGL) policy (ISO CG 00 01 04 13), assuming the street cleaner falls under the definition of "Mobile Equipment." However, their Business Auto policy (ISO CA 00 01\) only provides liability coverage for specifically described autos (Symbol 7), and the street cleaners are not listed on the auto schedule.

Draft a Claims Strategy Memo. You must:

1. Conduct a comparative analysis of the definitions of "Auto" versus "Mobile Equipment" under the post-2004 ISO policy forms.  
2. Evaluate the scenario to determine exactly how the street cleaning vehicle is classified by the policy language.  
3. Assess the financial risk and identify the specific coverage gap this creates for the insured municipality.  
4. Formulate an actionable recommendation for the claims department on how to respond to the current $500,000 claim, and provide a recommendation for the insured's broker on how to restructure their auto policy symbols to prevent future gaps.

**3\. Rubric**

| Description | Weight | Justification | Met | Failure Reasoning |
| :---- | :---- | :---- | :---- | :---- |
| **Compares Auto vs Mobile Equipment definitions.** | Critical | Must explain that vehicles subject to compulsory financial responsibility laws are classified as autos. | FALSE | Failed to address the impact of motor vehicle laws on the definitions. |
| **Evaluates the street sweeper exception.** | Critical | Must identify the specific ISO provision stating that self-propelled street cleaning equipment is inherently classified as an "Auto". | FALSE | Incorrectly classified the street cleaner as mobile equipment. |
| **Assesses the coverage gap.** | Major | Must explain that the CGL excludes "Autos," and the Auto policy (Symbol 7\) excludes unscheduled vehicles, leaving the claim uncovered. | FALSE | Did not identify the coverage gap. |
| **Provides actionable claims and broker recommendations.** | Critical | Must recommend denying the CGL claim and advising the broker to use Symbol 1 (Any Auto) on the Business Auto policy. | FALSE | Failed to provide a definitive claim decision or broker advice. |

**4\. Model Analysis**

This prompt tests the LLM's ability to navigate the complex boundary between Commercial General Liability (CGL) and Business Auto coverage. The model must perform a Comparative Analysis of ISO definitions, recognizing a critical carve-out: while many specialized vehicles are "mobile equipment," self-propelled vehicles designed chiefly for snow removal, road maintenance, and street cleaning are explicitly defined as "autos." The Scenario Evaluation forces the model to realize that because it is an "auto," it is excluded from the CGL. The Risk Assessment highlights the danger of using restrictive auto symbols (Symbol 7\) instead of broad coverage (Symbol 1). The Actionable Recommendation requires denying the claim while offering a structural fix for the client's insurance portfolio.

**5\. Golden Response**

CLAIMS STRATEGY MEMO

TO: Claims Department & Brokerage Team

FROM: Claims Legal Director

DATE: October 15, 2025

SUBJECT: Coverage Analysis – Street Cleaner Accident (Auto vs. Mobile Equipment)

1. Comparative Analysis: Auto vs. Mobile Equipment  
   Under ISO policy forms (post-2004), the dividing line between Commercial General Liability (CGL) and Business Auto coverage is dictated by the definitions of "Auto" and "Mobile Equipment." Generally, off-road machinery and vehicles that travel on crawler treads are considered mobile equipment and are covered under the CGL. However, if a vehicle is subject to a compulsory or financial responsibility law or other motor vehicle insurance law where it is licensed or garaged, it is classified as an "Auto."  
2. Scenario Evaluation: Street Cleaner Classification  
   The insured assumed their street cleaner was mobile equipment. However, the ISO CG 00 01 and CA 00 01 forms contain highly specific carve-outs in their definitions. The policy explicitly states that if a vehicle is permanently fitted with equipment designed chiefly for snow removal, road maintenance, or street cleaning, and is capable of moving under its own power, it is automatically defined as an "Auto" and not "Mobile Equipment." Therefore, the self-propelled street cleaner involved in this accident is legally classified as an Auto under the policy framework.  
3. Risk Assessment and Coverage Gap  
   Because the street cleaner is an Auto, the $500,000 bodily injury claim is absolutely excluded under the CGL policy's Aircraft, Auto, or Watercraft exclusion. The insured should have sought coverage under their Business Auto policy. Unfortunately, their Business Auto policy relies on Symbol 7 (Specifically Described Autos). Because the municipality failed to list the street cleaners on the auto schedule, the Business Auto policy will not respond either. This creates a total coverage gap, leaving the municipality entirely uninsured for the $500,000 liability.  
4. Actionable Recommendations  
* **Claims Handling:** The Claims Department must issue a formal Denial of Coverage letter under the CGL policy, citing the definition of "Auto" encompassing street cleaning vehicles, and the corresponding CGL Auto exclusion. A secondary denial must be issued under the Business Auto policy because the vehicle was not scheduled under Symbol 7\.  
* **Broker Strategy:** To prevent this catastrophic gap from occurring again, I strongly recommend that the insured's broker immediately request to rewrite the Business Auto policy using Symbol 1 (Any Auto) for liability coverage. Symbol 1 provides automatic coverage for owned, hired, and non-owned autos, entirely eliminating the risk of an administrative failure to schedule a specific vehicle like a street sweeper.

---

### **Example 13: New York No-Fault (PIP) Regulatory Compliance and Timelines**

**1\. Metadata**

Task Type: Workflow

Category / Domain: Auto Claims / Regulatory Compliance

Workflow: Claims Adjustment

Prompt Type: Claims Determination Memo

Difficulty: Expert

**2\. Prompt**

You are a New York Personal Injury Protection (PIP) Claims Adjuster. The current date is April 1, 2025\.

On January 1, 2025, a pedestrian was struck by your insured's commercial delivery van in Manhattan. The pedestrian suffered significant injuries, missing work and accumulating medical bills.

The pedestrian's attorney submitted the following documentation to your office:

* Form NF-2 (Application for Motor Vehicle No-Fault Benefits) received on January 25, 2025\.  
* The first set of medical bills (for emergency treatment rendered on January 1 and 2\) received on March 1, 2025\. No written explanation for the delay was provided.  
* A claim for lost earnings (spanning January through March) received on March 15, 2025\.

Draft a Claims Determination Memo. You must:

1. Conduct a comparative analysis of priority of coverage, determining whether the pedestrian's own health insurance or the commercial van's PIP coverage is primary.  
2. Evaluate the timeline scenario by applying the strict statutory deadlines of the New York Motor Vehicle No-Fault Insurance Law to each of the three submissions (the NF-2, the medical bills, and the wage loss claim).  
3. Assess the financial risk to the insurer regarding the potential for lawsuits bypassing the No-Fault system.  
4. Formulate an actionable recommendation directing the claims team on exactly which portions of the claim to accept, deny, or investigate further.

**3\. Rubric**

| Description | Weight | Justification | Met | Failure Reasoning |
| :---- | :---- | :---- | :---- | :---- |
| **Identifies the primary payer.** | Critical | Must state that the commercial vehicle's PIP policy is primary over health insurance for a pedestrian struck in NY. | FALSE | Incorrectly assigned primary liability to the pedestrian's health insurance. |
| **Evaluates the NF-2 submission timeline.** | Major | Must confirm the NF-2 was filed within the mandatory 30-day window. | FALSE | Rejected the NF-2 as untimely. |
| **Evaluates the medical bill submission timeline.** | Critical | Must deny the medical bills because they were submitted 59 days after treatment, violating the strict 45-day rule. | FALSE | Approved the medical bills despite the missed deadline. |
| **Evaluates the lost earnings submission timeline.** | Critical | Must approve the wage loss claim because it was submitted 73 days after the accident, falling within the 90-day limit. | FALSE | Denied the wage loss claim. |
| **Provides an actionable claims recommendation.** | Major | Must provide clear directives to deny medicals and accept the wage loss claim. | FALSE | Failed to provide a final claims determination. |

**4\. Model Analysis**

This prompt requires the model to navigate the notoriously strict and unforgiving deadlines of the New York No-Fault (PIP) system. The model must perform a Scenario Evaluation to mathematically check the number of days elapsed for three separate types of submissions. It must know that NF-2 forms have a 30-day deadline, medical bills have a 45-day deadline, and lost earnings have a 90-day deadline. By comparing these statutory limits against the dates provided, the model must isolate which parts of the claim survive and which are time-barred, ultimately delivering a split Actionable Recommendation.

**5\. Golden Response**

CLAIMS DETERMINATION MEMO

TO: Commercial Auto Claims Team

FROM: Senior NY PIP Adjuster

DATE: April 1, 2025

SUBJECT: Priority of Coverage and Statutory Deadline Analysis

1. Comparative Analysis: Priority of Coverage  
   In New York, when a pedestrian is struck by a motor vehicle, the insurance policy of the vehicle that struck the pedestrian is responsible for paying up to $50,000 for medical bills and basic economic losses. This coverage acts as the primary payer, meaning it pays before the pedestrian's personal health insurance. Therefore, our commercial auto policy must respond as primary PIP.  
2. Scenario Evaluation: Statutory Deadlines  
   The New York Motor Vehicle No-Fault Insurance Law imposes strict timelines for claim submissions. A failure to adhere to these deadlines, absent a reasonable written explanation, results in a denial of benefits.  
* **Form NF-2:** The injured party has 30 days from the accident to file the NF-2 application. Received on January 25 (Day 24), this submission is TIMELY.  
* **Medical Bills:** Bills for health care services must be submitted within 45 days of treatment. The bills for Jan 1 and 2 were received on March 1 (Day 59). Because no written explanation for the delay was provided, this submission is LATE and barred.  
* **Lost Earnings:** Claims for lost earnings and other reasonable and necessary expenses must be submitted within 90 days. Received on March 15 (Day 73), this submission is TIMELY.  
3. Risk Assessment: Lawsuit Exposure  
   While the No-Fault system pays basic economic losses, it limits lawsuits for minor injuries. However, if the pedestrian's injuries meet the statutory "serious injury" threshold, they can bypass the No-Fault system and file a bodily injury liability lawsuit directly against our insured. We must monitor the medical reports for signs of permanent consequential limitation or significant disfigurement.  
4. Actionable Recommendation  
   Proceed with the following actions immediately:  
* Acknowledge receipt and validity of the NF-2 application.  
* Issue a formal Denial of Claim for the January 1 and 2 medical bills due to the violation of the 45-day submission rule.  
* Open the wage loss claim, as it was received within the 90-day window, and request the employer's wage verification forms to process the lost earnings payments.

---

### **Example 14: Life Insurance MEC 7-Pay Test and 1035 Exchange**

**1\. Metadata**

Task Type: Workflow

Category / Domain: Life Insurance / Advanced Markets

Workflow: Policy Servicing

Prompt Type: Tax Strategy Memo

Difficulty: Expert

**2\. Prompt**

You are an Advanced Markets Tax Specialist for a life insurance carrier. The current date is May 15, 2025\.

Your client purchased a whole life insurance policy with a $500,000 death benefit on January 1, 2024\. According to the policy documents, the TAMRA 7-pay premium limit is $12,000 annually.

The client's premium payment history is as follows:

* Year 1 (2024): Paid $10,000  
* Year 2 (2025): Paid $10,000

It is now Year 2 (2025). The client has requested to pay a lump sum premium of $20,000 into the policy today. Furthermore, the client currently has a $5,000 policy loan outstanding. They informed their advisor that next month, they plan to initiate an IRC Section 1035 tax-free exchange to move the cash value to a new carrier's annuity, and they intend to extinguish the $5,000 loan during the exchange rather than carrying it over.

Draft a Tax Strategy Memo. You must:

1. Conduct a comparative analysis of standard life insurance tax benefits versus the tax implications of a Modified Endowment Contract (MEC).  
2. Evaluate the scenario using the IRS 7-pay test calculation logic to determine exactly if the proposed $20,000 payment in Year 3 will trigger MEC status.  
3. Assess the tax risk of extinguishing the $5,000 policy loan during the planned 1035 exchange.  
4. Formulate an actionable recommendation for the financial advisor on how to structure the Year 3 premium payment and the loan payoff to optimize the client's tax position.

**3\. Rubric**

| Description | Weight | Justification | Met | Failure Reasoning |
| :---- | :---- | :---- | :---- | :---- |
| **Analyzes MEC tax implications.** | Major | Must explain that MECs lose certain tax advantages, such as FIFO tax-free withdrawals, and may face a 10% penalty for early withdrawals. | FALSE | Failed to explain the consequences of MEC status. |
| **Calculates the cumulative 7-pay limit.** | Critical | Must calculate the cumulative limit by Year 3 ($12,000 \* 3 \= $36,000). | FALSE | Did not calculate the cumulative allowable limit. |
| **Evaluates the MEC trigger scenario.** | Critical | Must conclude that $10k \+ $10k \+ $20k \= $40,000, which exceeds the $36,000 cumulative limit, thereby triggering MEC status. | FALSE | Incorrectly stated the policy would not become a MEC. |
| **Assesses the 1035 exchange loan extinguishment.** | Critical | Must identify that extinguishing the loan reduces the cost basis and can trigger a taxable event (taxable boot). | FALSE | Failed to address the tax consequences of the loan extinguishment. |
| **Provides an actionable recommendation.** | Major | Must recommend capping the Year 3 premium at $16,000 to avoid MEC status and paying off the loan out-of-pocket prior to the 1035 exchange. | FALSE | Did not provide a clear mathematical mitigation strategy. |

**4\. Model Analysis**

This prompt tests the model's ability to execute complex actuarial and tax logic. The model must understand that the IRS 7-pay test for Modified Endowment Contracts is cumulative. While the client underpaid in Years 1 and 2 ($10k vs the $12k limit), the model must calculate the total allowable limit by Year 3 ($36,000) and compare it to the proposed cumulative premium ($40,000). The model must also evaluate the secondary risk of the 1035 exchange, recognizing that an extinguished loan acts as a taxable "boot." The synthesis of these two distinct tax traps requires a highly specific, actionable recommendation to save the client from adverse tax consequences.

**5\. Golden Response**

TAX STRATEGY MEMO

TO: Financial Advisor Team

FROM: Advanced Markets Tax Specialist

DATE: May 15, 2025

SUBJECT: Policy Funding, MEC 7-Pay Test, and 1035 Exchange Considerations

1. Comparative Analysis: Standard Life Insurance vs. MEC  
   Standard cash value life insurance allows policyholders to access cash value on a tax-deferred basis, typically utilizing First-In, First-Out (FIFO) accounting for withdrawals, which makes withdrawals up to the cost basis tax-free. However, if a life insurance policy is designated as a Modified Endowment Contract (MEC), it loses some of its cash value tax advantages. Distributions from a MEC are taxed on a Last-In, First-Out (LIFO) basis (gains are taxed first as ordinary income), and withdrawals made prior to age 59.5 may be subject to a 10% IRS penalty. Once classified as a MEC, a policy cannot revert to regular life insurance status.  
2. Scenario Evaluation: The 7-Pay Test Calculation  
   The IRS uses the 7-pay test to determine if a policy is overfunded and should be classified as a MEC. The test calculates the annual premium a life insurance policy would need to be paid up after seven level annual premiums. Crucially, the 7-pay test is cumulative.  
* The annual 7-pay limit is $12,000.  
* By Year 3, the maximum cumulative premium allowed is $36,000 ($12,000 x 3 years).  
* The client has paid $20,000 so far ($10,000 in Year 1 \+ $10,000 in Year 2).  
* If the client pays the proposed $20,000 lump sum in Year 3, the total premiums paid will equal $40,000.  
  Because $40,000 exceeds the cumulative $36,000 limit, executing this $20,000 payment will immediately trigger MEC status.  
3. Risk Assessment: 1035 Exchange with Loan Extinguishment  
   Under IRC Section 1035, exchanging a life insurance policy for an annuity allows the policyowner to carry over their cost basis without recognizing gain. However, the client intends to extinguish a $5,000 loan during the exchange. If a loan is extinguished in the exchange, the amount of the original policy's basis that is carried over to the new policy will decrease to the extent the loan exceeds the policy's gain. Furthermore, a reduction in the basis cannot exceed the policy's basis without incurring a tax. This extinguished loan will act as taxable "boot" to the client.  
4. Actionable Recommendation  
   To optimize the client's tax position, you must instruct the client to modify their transaction sequence:  
* **Avoid MEC Status:** Limit the Year 3 premium payment to a maximum of $16,000. This will bring the cumulative premiums paid to exactly $36,000 ($10k \+ $10k \+ $16k), keeping the policy compliant with the 7-pay test and preserving its tax advantages.  
* **1035 Exchange Preparation:** Advise the client to pay off the $5,000 policy loan with out-of-pocket funds prior to initiating the 1035 exchange. By settling the debt beforehand, the new contract will accumulate more cash value, and the client will fully preserve their higher cost basis without triggering a taxable event.

---

### **Example 15: Medicare Secondary Payer (MSP) ESRD Coordination Period**

**1\. Metadata**

Task Type: Workflow

Category / Domain: Healthcare Claims / Medicare Compliance

Workflow: Coordination of Benefits

Prompt Type: Claims Determination Memo

Difficulty: Expert

**2\. Prompt**

You are a Medicare Coordination of Benefits Analyst. The current date is October 1, 2025\.

A patient covered by an employer-sponsored Group Health Plan (GHP) was diagnosed with End-Stage Renal Disease (ESRD). The patient began a regular course of outpatient hemodialysis on January 15, 2023\. The patient did not participate in a self-dialysis training program and had no prior Medicare entitlement.

A medical bill for dialysis services rendered on September 15, 2025, has been submitted to Medicare.

Review CMS Medicare Secondary Payer Manual (Pub. 100-05), Chapter 2, Section 20.1. Calculate the exact start and end dates of the 30-month coordination period. Determine whether Medicare or the GHP is the primary payer for the September 15, 2025 date of service. Draft a Claims Determination Memo detailing your mathematical timeline.

**3\. Rubric (Negative Failure Focus)**

| Description | Weight | Justification | Met | Failure Reasoning (Model Failure Example) |
| :---- | :---- | :---- | :---- | :---- |
| **Calculates the Medicare entitlement start date.** | Critical | Must state April 1, 2023, which is the third month after the month dialysis began. | FALSE | **Model Failure:** Failed to apply the 3-month waiting period and anchored to the January 15 dialysis start date instead. |
| **Calculates the end of the 30-month coordination period.** | Critical | Must state September 30, 2025\. | FALSE | **Model Failure:** Miscalculated the 30-month window, arriving at July 2025 by simply adding 30 months to January. |
| **Determines the Primary Payer for the Sept 15, 2025 claim.** | Critical | Must state the GHP is primary because the date of service falls within the 30-month window. | FALSE | **Model Failure:** Incorrectly stated Medicare is primary because the flawed timeline put September 2025 outside the coordination period. |
| **Provides an actionable claims recommendation.** | Major | Must recommend denying the primary Medicare claim and instructing the provider to bill the GHP first. | FALSE | **Model Failure:** Approved the Medicare claim for primary payment. |

**4\. Model Analysis**

This prompt is designed to induce a specific negative model failure regarding statutory timelines. LLMs frequently fail this task by exhibiting "anchoring bias" to the first date provided in the prompt (January 15, 2023\) and simply adding 30 months to it, arriving at a coordination end date of July 2025\. A model failing in this manner will incorrectly conclude that the 30-month coordination period has expired, shifting primary liability to Medicare. To achieve the golden response, the model must overcome this failure trap by strictly adhering to CMS manual rules, which state that Medicare entitlement (and the coordination period) begins with the third month *after* the month in which a regular course of dialysis starts. Only by accurately calculating the start date as April 1, 2023, will the model correctly realize the coordination period ends on September 30, 2025, making the GHP primary for the September 15th claim.

**5\. Golden Response**

CLAIMS DETERMINATION MEMO

TO: Medicare Claims Processing

FROM: Coordination of Benefits Analyst

DATE: October 1, 2025

SUBJECT: ESRD 30-Month Coordination Period Analysis

1. Timeline Calculation and CMS Guidelines  
   Under the CMS Medicare Secondary Payer Manual, Chapter 2, Section 20.1, Medicare is the secondary payer to Group Health Plans (GHPs) for individuals eligible for Medicare based on ESRD during a 30-month coordination period.

Crucially, the 30-month coordination period does not begin on the date of the first dialysis treatment. According to Section 20.1.1, the coordination period begins with the first month of entitlement to Medicare Part A. Medicare entitlement usually begins with the third month after the month in which the individual starts a regular course of dialysis.

* **Dialysis Start Date:** January 15, 2023\.  
* **Coordination Period Start Date:** April 1, 2023 (the third month after January).  
* **Coordination Period End Date:** September 30, 2025 (30 months from April 1, 2023).  
2. Scenario Evaluation and Coverage Determination  
   The medical bill in question is for services rendered on September 15, 2025\. Because September 15, 2025, falls within the 30-month coordination period (which ends September 30, 2025), the employer-sponsored GHP remains the primary payer.  
3. Actionable Recommendation  
   I recommend denying this claim for primary Medicare payment. The provider must be instructed to bill the patient's GHP as the primary payer. Medicare will only act as the secondary payer for this date of service if the GHP does not cover the claim in full.

---

### **Example 16: Reinsurance Excess of Policy Limits (XPL) Cap Calculation**

**1\. Metadata**

Task Type: Workflow

Category / Domain: Reinsurance / Claims

Workflow: Treaty Adjudication

Prompt Type: Reinsurance Adjustment Report

Difficulty: Expert

**2\. Prompt**

You are a Reinsurance Claims Adjuster. The current date is May 10, 2025\.

Your ceding company has submitted a claim under their Excess Catastrophe Reinsurance Contract (effective July 1, 2020).

The ceding company suffered an actual catastrophe loss of $10,000,000 from a hurricane. During the claims handling process for one of the damaged homes, the ceding company exhibited bad faith by refusing to settle within the homeowner's policy limits. The homeowner sued, resulting in a massive court judgment. Consequently, the ceding company incurred a $4,000,000 "Loss in excess of policy limits" (XPL).

Using the definitions found in Article 9 of the Excess Catastrophe Reinsurance Contract, calculate the total covered XPL amount the reinsurer is obligated to accept. Detail the mathematical application of any percentages and caps in a Reinsurance Adjustment Report.

**3\. Rubric (Negative Failure Focus)**

| Description | Weight | Justification | Met | Failure Reasoning (Model Failure Example) |
| :---- | :---- | :---- | :---- | :---- |
| **Identifies the base 90% XPL coverage rule.** | Critical | Must state that XPL is covered at 90% of the amount paid in excess of limits. | FALSE | **Model Failure:** Missed the 90% coinsurance requirement for XPL. |
| **Calculates the preliminary XPL amount.** | Major | Must multiply $4,000,000 by 90% to get $3,600,000. | FALSE | **Model Failure:** Failed to calculate the 90% value, assuming the full $4,000,000 was in play. |
| **Identifies the 25% actual catastrophe loss cap.** | Critical | Must state XPL cannot exceed 25% of the actual catastrophe loss. | FALSE | **Model Failure:** Ignored the secondary 25% cap constraint entirely. |
| **Calculates the maximum XPL cap.** | Critical | Must multiply the $10,000,000 catastrophe loss by 25% to get $2,500,000. | FALSE | **Model Failure:** Calculated 25% of the XPL amount ($1,000,000) instead of 25% of the Catastrophe Loss. |
| **Determines the final payable XPL amount.** | Critical | Must conclude the reinsurer will accept $2,500,000, as the cap is lower than the 90% calculation. | FALSE | **Model Failure:** Authorized the full $3,600,000, exposing the reinsurer to an uncapped extra-contractual loss. |

**4\. Model Analysis**

This prompt exposes a classic negative model failure related to "premature convergence" in multi-variable constraint logic. When presented with a massive penalty ($4,000,000) and a percentage rule (90%), models tend to perform the first mathematical step ($3,600,000) and stop, assuming the problem is solved. The model fails to cross-reference the secondary constraint—the absolute cap tied to a completely different variable (25% of the actual catastrophe loss). Furthermore, even when models do identify the 25% cap, they frequently hallucinate the base variable, calculating 25% of the $4,000,000 XPL rather than the $10,000,000 Catastrophe Loss. A successful model must navigate both constraints independently and compare the outputs to determine the final liability limit.

**5\. Golden Response**

REINSURANCE ADJUSTMENT REPORT

TO: Reinsurance Accounting

FROM: Reinsurance Claims Adjuster

DATE: May 10, 2025

SUBJECT: Article 9 XPL Cap Calculation

1. Contractual Mechanics for XPL  
   Under Article 9 (Definitions) of the Excess Catastrophe Reinsurance Contract, "Loss in excess of policy limits" (XPL) is defined as 90.0% of any amount paid by the ceding company that exceeds its policy limits due to negligence or bad faith in rejecting a settlement offer.  
   However, Article 9 places a strict ceiling on this exposure. It dictates that any XPL made in connection with the contract shall not exceed 25.0% of the actual catastrophe loss.  
2. Mathematical Adjustment and Scenario Evaluation  
   To determine the reinsurer's ultimate liability for this bad faith judgment, we must evaluate both constraints independently:  
* **Constraint 1 (90% Rule):** The ceding company incurred $4,000,000 in XPL. Applying the 90% coverage rule yields a preliminary eligible amount of $3,600,000.  
* **Constraint 2 (25% Catastrophe Cap):** The actual catastrophe loss for the hurricane event was $10,000,000. The contract caps XPL at 25% of this actual catastrophe loss. Therefore, the maximum allowable XPL cap is $2,500,000 (25% of $10,000,000).  
3. Actionable Recommendation  
   Because the $3,600,000 preliminary XPL calculation exceeds the absolute contractual cap of $2,500,000, the reinsurer's liability is strictly limited to the cap. I recommend authorizing an XPL acceptance of exactly $2,500,000 and denying the remaining $1,500,000 submitted by the ceding company.

---

### **Example 17: Commercial Auto Underwriting (Moving vs. Equipment Violations)**

**1\. Metadata**

Task Type: Workflow

Category / Domain: Commercial Auto / Underwriting

Workflow: Driver Eligibility

Prompt Type: Underwriting Memo

Difficulty: High

**2\. Prompt**

You are a Commercial Auto Underwriter using the MMG Commercial Auto Underwriting Guidelines. The current date is August 1, 2025\. A logistics company submits three new drivers for a fleet policy.

* **Driver 1:** Age 28\. Has a citation for a "speed contest" from 4 years ago. No other violations.  
* **Driver 2:** Age 45\. Has 2 standard moving violations in the past 3 years.  
* **Driver 3:** Age 30\. Has a citation for "faulty equipment" (broken taillight) from 2 months ago and 1 standard moving violation from 1 year ago.

Evaluate each driver's eligibility based strictly on the MMG guidelines. Conduct a risk assessment for each driver, explaining exactly why they are accepted or rejected. Formulate a final actionable recommendation for the broker.

**3\. Rubric (Negative Failure Focus)**

| Description | Weight | Justification | Met | Failure Reasoning (Model Failure Example) |
| :---- | :---- | :---- | :---- | :---- |
| **Evaluates Driver 1 as Ineligible.** | Critical | Must recognize "speed contest" as a Major Violation with a 5-year lookback. | FALSE | **Model Failure:** Approved Driver 1 by mistakenly applying the standard 3-year moving violation lookback to a Major Violation. |
| **Evaluates Driver 2 as Eligible.** | Major | Must apply the rule: 2 violations in 3 years is acceptable for drivers 25 and older. | FALSE | **Model Failure:** Rejected Driver 2 by incorrectly applying the "under age 25" restriction (1 violation in 3 years). |
| **Evaluates Driver 3 as Eligible.** | Critical | Must recognize that faulty equipment is not a moving violation, leaving the driver with only 1 moving violation in 3 years. | FALSE | **Model Failure:** Rejected Driver 3 by erroneously counting the equipment violation as a moving violation, concluding they had 2 violations in 12 months. |
| **Provides an actionable broker recommendation.** | Major | Must clearly direct the broker to exclude Driver 1 and add Drivers 2 and 3\. | FALSE | **Model Failure:** Failed to provide a definitive underwriting decision. |

**4\. Model Analysis**

This prompt tests the LLM's ability to execute classification logic without falling into common heuristic traps. Models frequently fail by conflating "Minor" and "Major" violations. A prime negative failure occurs when the model sees Driver 1's violation was 4 years ago and automatically approves them, hallucinating that a 3-year lookback applies to all infractions, rather than applying the strict 5-year lookback for Major Violations like a "speed contest". Additionally, models often fail on Driver 3 by viewing any citation as a "moving violation", ignoring the explicit guideline carve-out that protects drivers with equipment violations. A successful model strictly isolates the violation classifications before assessing the timelines.

**5\. Golden Response**

UNDERWRITING MEMO

TO: Brokerage Team

FROM: Commercial Auto Underwriter

DATE: August 1, 2025

SUBJECT: Driver Roster Eligibility Assessment

1. Scenario Evaluation & Risk Assessment  
   Based on the MMG Commercial Auto Underwriting Guidelines, I have assessed the eligibility of the three submitted drivers:  
* **Driver 1 (Age 28\) \- INELIGIBLE:** Driver 1 has a citation for a "speed contest" on their record from 4 years ago. Under the guidelines, a speed contest is strictly defined as a Major Violation. For a risk to be eligible, drivers 25 and older must have zero Major Violations in the past 5 years. Because the violation occurred 4 years ago, this driver falls within the prohibited window and represents an unacceptable risk.  
* **Driver 2 (Age 45\) \- ELIGIBLE:** Driver 2 has 2 moving violations in the past 3 years. Because this driver is 25 or older, the guidelines permit a maximum of 1 moving violation in 12 months or 2 moving violations in 3 years. Driver 2 has exactly met, but not exceeded, the 3-year threshold and is therefore eligible.  
* **Driver 3 (Age 30\) \- ELIGIBLE:** Driver 3 has one citation for faulty equipment and one moving violation. The MMG guidelines explicitly state that equipment violations (e.g., faulty equipment, inspection, registration) are not considered moving violations. Therefore, for underwriting purposes, Driver 3 only has 1 moving violation in the past 3 years. This is well within the acceptable tolerance for a driver over the age of 25\.  
2. Actionable Recommendation  
   Please issue a revised driver schedule. We can successfully bind coverage for the fleet with Driver 2 and Driver 3\. However, you must explicitly exclude Driver 1 from the policy. If Driver 1 is permitted to operate any scheduled vehicles, it will violate our underwriting guidelines and jeopardize the fleet's coverage.

.

---

### **Example 18: Medicare Secondary Payer (MSP) ESRD Coordination Period**

**1\. Metadata**

Task Type: Workflow

Category / Domain: Healthcare Claims / Medicare Compliance

Workflow: Coordination of Benefits

Prompt Type: Claims Determination Memo

Difficulty: Expert

**2\. Prompt**

You are a Medicare Coordination of Benefits Analyst. The current date is October 1, 2025\.

A patient covered by an employer-sponsored Group Health Plan (GHP) was diagnosed with End-Stage Renal Disease (ESRD). The patient began a regular course of outpatient hemodialysis on January 15, 2023\. The patient did not participate in a self-dialysis training program and had no prior Medicare entitlement.

A medical bill for dialysis services rendered on September 15, 2025, has been submitted to Medicare.

Review CMS Medicare Secondary Payer Manual (Pub. 100-05), Chapter 2, Section 20.1. Calculate the exact start and end dates of the 30-month coordination period. Determine whether Medicare or the GHP is the primary payer for the September 15, 2025 date of service. Draft a Claims Determination Memo detailing your mathematical timeline.

**3\. Rubric (Negative Failure Focus)**

| Description | Weight | Justification | Met | Failure Reasoning (Model Failure Example) |
| :---- | :---- | :---- | :---- | :---- |
| **Calculates the Medicare entitlement start date.** | Critical | Must state April 1, 2023, which is the third month after the month dialysis began. | FALSE | **Model Failure:** Failed to apply the 3-month waiting period and anchored to the January 15 dialysis start date instead. |
| **Calculates the end of the 30-month coordination period.** | Critical | Must state September 30, 2025\. | FALSE | **Model Failure:** Miscalculated the 30-month window, arriving at July 2025 by simply adding 30 months to January. |
| **Determines the Primary Payer for the Sept 15, 2025 claim.** | Critical | Must state the GHP is primary because the date of service falls within the 30-month window. | FALSE | **Model Failure:** Incorrectly stated Medicare is primary because the flawed timeline put September 2025 outside the coordination period. |
| **Provides an actionable claims recommendation.** | Major | Must recommend denying the primary Medicare claim and instructing the provider to bill the GHP first. | FALSE | **Model Failure:** Approved the Medicare claim for primary payment. |

**4\. Model Analysis**

This prompt is designed to induce a specific negative model failure regarding statutory timelines. LLMs frequently fail this task by exhibiting "anchoring bias" to the first date provided in the prompt (January 15, 2023\) and simply adding 30 months to it, arriving at a coordination end date of July 2025\. A model failing in this manner will incorrectly conclude that the 30-month coordination period has expired, shifting primary liability to Medicare. To achieve the golden response, the model must overcome this failure trap by strictly adhering to CMS manual rules, which state that Medicare entitlement (and the coordination period) begins with the third month *after* the month in which a regular course of dialysis starts. Only by accurately calculating the start date as April 1, 2023, will the model correctly realize the coordination period ends on September 30, 2025, making the GHP primary for the September 15th claim.

**5\. Golden Response**

CLAIMS DETERMINATION MEMO

TO: Medicare Claims Processing

FROM: Coordination of Benefits Analyst

DATE: October 1, 2025

SUBJECT: ESRD 30-Month Coordination Period Analysis

1. Timeline Calculation and CMS Guidelines  
   Under the CMS Medicare Secondary Payer Manual, Chapter 2, Section 20.1, Medicare is the secondary payer to Group Health Plans (GHPs) for individuals eligible for Medicare based on ESRD during a 30-month coordination period.

Crucially, the 30-month coordination period does not begin on the date of the first dialysis treatment. According to Section 20.1.1, the coordination period begins with the first month of entitlement to Medicare Part A. Medicare entitlement usually begins with the third month after the month in which the individual starts a regular course of dialysis.

* **Dialysis Start Date:** January 15, 2023\.  
* **Coordination Period Start Date:** April 1, 2023 (the third month after January).  
* **Coordination Period End Date:** September 30, 2025 (30 months from April 1, 2023).  
2. Scenario Evaluation and Coverage Determination  
   The medical bill in question is for services rendered on September 15, 2025\. Because September 15, 2025, falls within the 30-month coordination period (which ends September 30, 2025), the employer-sponsored GHP remains the primary payer.  
3. Actionable Recommendation  
   I recommend denying this claim for primary Medicare payment. The provider must be instructed to bill the patient's GHP as the primary payer. Medicare will only act as the secondary payer for this date of service if the GHP does not cover the claim in full.

---

### **Example 19: Reinsurance Excess of Policy Limits (XPL) Cap Calculation**

**1\. Metadata**

Task Type: Workflow

Category / Domain: Reinsurance / Claims

Workflow: Treaty Adjudication

Prompt Type: Reinsurance Adjustment Report

Difficulty: Expert

**2\. Prompt**

You are a Reinsurance Claims Adjuster. The current date is May 10, 2025\.

Your ceding company has submitted a claim under their Excess Catastrophe Reinsurance Contract (effective July 1, 2020).

The ceding company suffered an actual catastrophe loss of $10,000,000 from a hurricane. During the claims handling process for one of the damaged homes, the ceding company exhibited bad faith by refusing to settle within the homeowner's policy limits. The homeowner sued, resulting in a massive court judgment. Consequently, the ceding company incurred a $4,000,000 "Loss in excess of policy limits" (XPL).

Using the definitions found in Article 9 of the Excess Catastrophe Reinsurance Contract, calculate the total covered XPL amount the reinsurer is obligated to accept. Detail the mathematical application of any percentages and caps in a Reinsurance Adjustment Report.

**3\. Rubric (Negative Failure Focus)**

| Description | Weight | Justification | Met | Failure Reasoning (Model Failure Example) |
| :---- | :---- | :---- | :---- | :---- |
| **Identifies the base 90% XPL coverage rule.** | Critical | Must state that XPL is covered at 90% of the amount paid in excess of limits. | FALSE | **Model Failure:** Missed the 90% coinsurance requirement for XPL. |
| **Calculates the preliminary XPL amount.** | Major | Must multiply $4,000,000 by 90% to get $3,600,000. | FALSE | **Model Failure:** Failed to calculate the 90% value, assuming the full $4,000,000 was in play. |
| **Identifies the 25% actual catastrophe loss cap.** | Critical | Must state XPL cannot exceed 25% of the actual catastrophe loss. | FALSE | **Model Failure:** Ignored the secondary 25% cap constraint entirely. |
| **Calculates the maximum XPL cap.** | Critical | Must multiply the $10,000,000 catastrophe loss by 25% to get $2,500,000. | FALSE | **Model Failure:** Calculated 25% of the XPL amount ($1,000,000) instead of 25% of the Catastrophe Loss. |
| **Determines the final payable XPL amount.** | Critical | Must conclude the reinsurer will accept $2,500,000, as the cap is lower than the 90% calculation. | FALSE | **Model Failure:** Authorized the full $3,600,000, exposing the reinsurer to an uncapped extra-contractual loss. |

**4\. Model Analysis**

This prompt exposes a classic negative model failure related to "premature convergence" in multi-variable constraint logic. When presented with a massive penalty ($4,000,000) and a percentage rule (90%), models tend to perform the first mathematical step ($3,600,000) and stop, assuming the problem is solved. The model fails to cross-reference the secondary constraint—the absolute cap tied to a completely different variable (25% of the actual catastrophe loss). Furthermore, even when models do identify the 25% cap, they frequently hallucinate the base variable, calculating 25% of the $4,000,000 XPL rather than the $10,000,000 Catastrophe Loss. A successful model must navigate both constraints independently and compare the outputs to determine the final liability limit.

**5\. Golden Response**

REINSURANCE ADJUSTMENT REPORT

TO: Reinsurance Accounting

FROM: Reinsurance Claims Adjuster

DATE: May 10, 2025

SUBJECT: Article 9 XPL Cap Calculation

1. Contractual Mechanics for XPL  
   Under Article 9 (Definitions) of the Excess Catastrophe Reinsurance Contract, "Loss in excess of policy limits" (XPL) is defined as 90.0% of any amount paid by the ceding company that exceeds its policy limits due to negligence or bad faith in rejecting a settlement offer.  
   However, Article 9 places a strict ceiling on this exposure. It dictates that any XPL made in connection with the contract shall not exceed 25.0% of the actual catastrophe loss.  
2. Mathematical Adjustment and Scenario Evaluation  
   To determine the reinsurer's ultimate liability for this bad faith judgment, we must evaluate both constraints independently:  
* **Constraint 1 (90% Rule):** The ceding company incurred $4,000,000 in XPL. Applying the 90% coverage rule yields a preliminary eligible amount of $3,600,000.  
* **Constraint 2 (25% Catastrophe Cap):** The actual catastrophe loss for the hurricane event was $10,000,000. The contract caps XPL at 25% of this actual catastrophe loss. Therefore, the maximum allowable XPL cap is $2,500,000 (25% of $10,000,000).  
3. Actionable Recommendation  
   Because the $3,600,000 preliminary XPL calculation exceeds the absolute contractual cap of $2,500,000, the reinsurer's liability is strictly limited to the cap. I recommend authorizing an XPL acceptance of exactly $2,500,000 and denying the remaining $1,500,000 submitted by the ceding company.

---

### **Example 20: Commercial Auto Underwriting (Moving vs. Equipment Violations)**

**1\. Metadata**

Task Type: Workflow

Category / Domain: Commercial Auto / Underwriting

Workflow: Driver Eligibility

Prompt Type: Underwriting Memo

Difficulty: High

**2\. Prompt**

You are a Commercial Auto Underwriter using the MMG Commercial Auto Underwriting Guidelines. The current date is August 1, 2025\. A logistics company submits three new drivers for a fleet policy.

* **Driver 1:** Age 28\. Has a citation for a "speed contest" from 4 years ago. No other violations.  
* **Driver 2:** Age 45\. Has 2 standard moving violations in the past 3 years.  
* **Driver 3:** Age 30\. Has a citation for "faulty equipment" (broken taillight) from 2 months ago and 1 standard moving violation from 1 year ago.

Evaluate each driver's eligibility based strictly on the MMG guidelines. Conduct a risk assessment for each driver, explaining exactly why they are accepted or rejected. Formulate a final actionable recommendation for the broker.

**3\. Rubric (Negative Failure Focus)**

| Description | Weight | Justification | Met | Failure Reasoning (Model Failure Example) |
| :---- | :---- | :---- | :---- | :---- |
| **Evaluates Driver 1 as Ineligible.** | Critical | Must recognize "speed contest" as a Major Violation with a 5-year lookback. | FALSE | **Model Failure:** Approved Driver 1 by mistakenly applying the standard 3-year moving violation lookback to a Major Violation. |
| **Evaluates Driver 2 as Eligible.** | Major | Must apply the rule: 2 violations in 3 years is acceptable for drivers 25 and older. | FALSE | **Model Failure:** Rejected Driver 2 by incorrectly applying the "under age 25" restriction (1 violation in 3 years). |
| **Evaluates Driver 3 as Eligible.** | Critical | Must recognize that faulty equipment is not a moving violation, leaving the driver with only 1 moving violation in 3 years. | FALSE | **Model Failure:** Rejected Driver 3 by erroneously counting the equipment violation as a moving violation, concluding they had 2 violations in 12 months. |
| **Provides an actionable broker recommendation.** | Major | Must clearly direct the broker to exclude Driver 1 and add Drivers 2 and 3\. | FALSE | **Model Failure:** Failed to provide a definitive underwriting decision. |

**4\. Model Analysis**

This prompt tests the LLM's ability to execute classification logic without falling into common heuristic traps. Models frequently fail by conflating "Minor" and "Major" violations. A prime negative failure occurs when the model sees Driver 1's violation was 4 years ago and automatically approves them, hallucinating that a 3-year lookback applies to all infractions, rather than applying the strict 5-year lookback for Major Violations like a "speed contest". Additionally, models often fail on Driver 3 by viewing any citation as a "moving violation", ignoring the explicit guideline carve-out that protects drivers with equipment violations. A successful model strictly isolates the violation classifications before assessing the timelines.

**5\. Golden Response**

UNDERWRITING MEMO

TO: Brokerage Team

FROM: Commercial Auto Underwriter

DATE: August 1, 2025

SUBJECT: Driver Roster Eligibility Assessment

1. Scenario Evaluation & Risk Assessment  
   Based on the MMG Commercial Auto Underwriting Guidelines, I have assessed the eligibility of the three submitted drivers:  
* **Driver 1 (Age 28\) \- INELIGIBLE:** Driver 1 has a citation for a "speed contest" on their record from 4 years ago. Under the guidelines, a speed contest is strictly defined as a Major Violation. For a risk to be eligible, drivers 25 and older must have zero Major Violations in the past 5 years. Because the violation occurred 4 years ago, this driver falls within the prohibited window and represents an unacceptable risk.  
* **Driver 2 (Age 45\) \- ELIGIBLE:** Driver 2 has 2 moving violations in the past 3 years. Because this driver is 25 or older, the guidelines permit a maximum of 1 moving violation in 12 months or 2 moving violations in 3 years. Driver 2 has exactly met, but not exceeded, the 3-year threshold and is therefore eligible.  
* **Driver 3 (Age 30\) \- ELIGIBLE:** Driver 3 has one citation for faulty equipment and one moving violation. The MMG guidelines explicitly state that equipment violations (e.g., faulty equipment, inspection, registration) are not considered moving violations. Therefore, for underwriting purposes, Driver 3 only has 1 moving violation in the past 3 years. This is well within the acceptable tolerance for a driver over the age of 25\.  
2. Actionable Recommendation  
   Please issue a revised driver schedule. We can successfully bind coverage for the fleet with Driver 2 and Driver 3\. However, you must explicitly exclude Driver 1 from the policy. If Driver 1 is permitted to operate any scheduled vehicles, it will violate our underwriting guidelines and jeopardize the fleet's coverage.

### **Example 21: Marine Cargo General Average and "Deleted" Exclusions Trap**

**1\. Metadata**

Task Type: Workflow

Category / Domain: Marine Insurance / Admiralty Law

Workflow: Claims Adjudication

Prompt Type: Claims Determination Memo

Difficulty: Expert

**2\. Prompt**

You are a Marine Claims Adjuster. The current date is March 15, 2026\.

Your insured, a logistics company, is shipping a multi-million dollar industrial generator. The cargo is insured under the Institute Cargo Clauses (A) 2009\. During policy negotiations, the insured specifically requested, and the underwriter agreed, to completely delete Exclusion 4.5 ("Delay") from the policy.

During the voyage across the Pacific, the vessel experienced severe engine issues. To avoid a rapidly approaching typhoon and save the ship and cargo (the common maritime adventure), the Master intentionally overworked the propelling machinery far beyond its safe limits. This successfully allowed the ship to outrun the storm and limp into a port of refuge, but it completely destroyed the vessel's boilers. The vessel remained afloat in open water for the entire duration of the storm and the machinery breakdown.

The shipowner has declared General Average (GA) under the York-Antwerp Rules 2016 and has assessed a $250,000 GA contribution against your insured's cargo.

Additionally, because the vessel was stuck at the port of refuge, the cargo was delayed by a month. To prevent their buyer from canceling the contract, your insured spent $40,000 to charter a heavy-lift aircraft to fly the generator to its final destination. They have submitted a claim for both the $250,000 GA contribution and the $40,000 in expedited forwarding/delay costs.

Draft a Claims Determination Memo. You must:

* Evaluate the $250,000 GA claim strictly under Rule VII of the York-Antwerp Rules 2016\.  
* Evaluate the $40,000 forwarding claim, analyzing the specific legal effect of the underwriter deleting the "Delay" exclusion (Exclusion 4.5) from the ICC (A) 2009 policy.  
* Formulate an actionable recommendation on whether to pay or deny each component of the $290,000 claim.

**Attached Files (Context):**

* YAR-2016-English-Version.pdf – Public URL: [https://comitemaritime.org/wp-content/uploads/2023/01/YAR-2016-English-Version.pdf](https://comitemaritime.org/wp-content/uploads/2023/01/YAR-2016-English-Version.pdf)  
* institute-cargo-clauses-a-2009.pdf – Public URL: [https://www.if-insurance.com/globalassets/industrial/files/marine-cargo/institute-clauses/institute-cargo-clauses-a-2009.pdf](https://www.if-insurance.com/globalassets/industrial/files/marine-cargo/institute-clauses/institute-cargo-clauses-a-2009.pdf)

**3\. Rubric (Negative Failure Focus)**

| \# | Description | Weight | Sources (with exact PDF pages) | Justification | Met | Failure Reasoning (Model Failure Example) | Dependent Criteria |
| :---- | :---- | :---- | :---- | :---- | :---- | :---- | :---- |
| 1 | Analyzes the GA claim under YAR 2016 Rule VII. | Critical | YAR-2016-English-Version.pdf, PDF Page 5 | Must cite that Rule VII strictly prohibits GA allowance for machinery damage if the ship is "afloat." | FALSE | **Model Failure:** Approved the $250,000 GA claim because the Master intentionally sacrificed the boiler for the "common safety," ignoring the absolute "afloat" exception in the treaty. |  |
| 2 | Analyzes the effect of the deleted Delay exclusion. | Critical | Insurance Principles | Must explain that deleting an exclusion does not create affirmative coverage or convert delay into an insured peril. | FALSE | **Model Failure:** Approved the $40,000 forwarding costs, hallucinating that because the delay exclusion was removed, delay-related economic losses are automatically covered. |  |
| 3 | Denies the General Average contribution. | Major | YAR-2016-English-Version.pdf, PDF Page 5 | Must instruct the insured that they bear zero liability for the shipowner's engine damage. | FALSE | **Model Failure:** Recommended paying the $250,000 GA assessment. | 1 |
| 4 | Denies the expedited forwarding costs. | Major | institute-cargo-clauses-a-2009.pdf, PDF Page 1 | Must completely deny the $40,000 economic loss claim. | FALSE | **Model Failure:** Recommended paying the $40,000 forwarding costs. | 2 |
| 5 | Provides an actionable, $0 payout recommendation. | Critical | Prompt Text | Must conclude with a definitive rejection of the entire $290,000 claim. | FALSE | **Model Failure:** Authorized payment for any portion of the claim. | 3, 4 |

**4\. Model Analysis**

This prompt features two distinct traps that reliably cause LLM failure. First, the "General Average" trap: The scenario describes a textbook GA act (an intentional sacrifice for the common safety). An LLM relying on general maritime principles will instinctively approve the GA contribution. It must bypass its general knowledge and apply the strict prohibition in Rule VII of the York-Antwerp Rules 2016, which dictates that machinery overworked while a ship is afloat can "in no circumstances" be allowed as GA. Second, the "Deleted Exclusion" trap: LLMs frequently hallucinate that when an underwriter deletes an exclusion (in this case, Delay), the policy automatically grants affirmative coverage for that peril. The model must demonstrate advanced legal reasoning by concluding that removing an exclusion simply means the insurer cannot rely on it as a defense; the loss must still be proximately caused by an insured peril, and delay itself does not constitute physical loss or damage.

**5\. Golden Response**

CLAIMS DETERMINATION MEMO

TO: Marine Claims Department

FROM: Marine Claims Adjuster

DATE: March 15, 2026

SUBJECT: General Average Assessment and Expedited Forwarding Claim

* Scenario Evaluation: General Average (York-Antwerp Rules 2016\)  
  The shipowner is demanding a $250,000 contribution for the intentional destruction of the vessel's boilers. While the Master's actions represent a sacrifice made for the common safety, this adjustment is governed by the York-Antwerp Rules 2016\. Under Rule VII (Damage to Machinery and Boilers), there is an absolute prohibition regarding vessels in open water. The rule explicitly mandates that where a ship is afloat, no loss or damage caused by working the propelling machinery and boilers shall in any circumstances be allowed as general average. Because the vessel remained afloat during the entirety of the storm and the subsequent machinery failure, the loss is expressly barred from GA consideration. The cargo owner has no legal liability to contribute to the shipowner's hull damage.  
* Scenario Evaluation: Deletion of Exclusion 4.5 (Delay)  
  The insured is claiming $40,000 in economic losses (forwarding charges) due to the delay at the port of refuge, arguing that the underwriter specifically deleted the "Delay" exclusion (Exclusion 4.5) from their ICC (A) 2009 policy. However, it is a fundamental principle of insurance law that the deletion of an exclusion does not have the effect of providing positive cover against the peril that would have been excluded. The effect of deleting an exclusion is simply that the underwriter can no longer rely on it to reject a claim that would otherwise be recoverable under the policy. The insured must still prove that the loss was caused by a covered peril. Simply deleting the exclusion of delay does not have the effect of converting delay into a named peril or affirmative coverage grant.  
* Actionable Recommendation  
  I recommend a full denial of the entire $290,000 claim.  
* Regarding the $250,000 GA Demand: We will formally reject the GA assessment against our insured's cargo, citing the "afloat" provision of YAR 2016 Rule VII.  
* Regarding the $40,000 Forwarding Costs: We will issue a denial letter explaining that the policy covers physical loss or damage, and that the deletion of the delay exclusion does not manufacture affirmative coverage for economic losses or expedited shipping costs incurred solely to fulfill a commercial contract.

### ---

**Example 22: Medical Stop-Loss "Laser" Disclosure and Threshold Math**

**1\. Metadata**

Task Type: Workflow

Category / Domain: Health / Medical Stop-Loss

Workflow: Underwriting Compliance

Prompt Type: Claims Strategy Memo

Difficulty: Expert

**2\. Prompt**

You are a Medical Stop-Loss Claims Director. The current date is March 1, 2026\.

A self-funded employer (the Plan Sponsor) purchased a Specific Stop-Loss Insurance policy with a Specific Attachment Point of $60,000 per covered life. The policy effective date was January 1, 2026\.

On December 1, 2025, the employer completed the required Standard Stop Loss Disclosure Form as part of the binding process. The employer checked "No" to all questions regarding known risks and did not list any specific individuals on the disclosure schedule.

However, your claims audit reveals that on November 15, 2025, an employee ("Employee Z") was diagnosed with a severe cardiovascular illness. This specific illness is represented by an ICD-10 code explicitly listed on the insurer's Trigger Diagnosis List. Between November 15 and November 30, 2025, the employer's third-party administrator received and paid $14,000 in medical bills for Employee Z's condition.

In February 2026, Employee Z required emergency surgery, generating $85,000 in new, paid medical claims. The employer has submitted a Specific Stop-Loss claim for $39,000 (calculated as $14,000 \+ $85,000 \= $99,000 in total claims, minus the $60,000 specific attachment point).

Draft a Claims Strategy Memo. You must:

* Conduct a mathematical analysis of the disclosure threshold required by the Standard Stop Loss Disclosure Form based on the employer's Specific Attachment Point.  
* Evaluate the timeline and facts to determine if the employer breached the disclosure requirements regarding Employee Z prior to the policy effective date.  
* Assess the insurer's contractual liability for the claim based on the disclosure rules.  
* Formulate a binding claims recommendation regarding the $39,000 request.

**Attached Files (Context):**

* Standard\_Disclosure.pdf – Public URL: [https://www.iisinet.com/uploads/docs/Standard\_Disclosure.pdf](https://www.iisinet.com/uploads/docs/Standard_Disclosure.pdf)

**3\. Rubric (Negative Failure Focus)**

| \# | Description | Weight | Sources (with exact PDF pages) | Justification | Met | Failure Reasoning (Model Failure Example) | Dependent Criteria |
| :---- | :---- | :---- | :---- | :---- | :---- | :---- | :---- |
| 1 | Calculates the 20% disclosure threshold. | Critical | Standard\_Disclosure.pdf, PDF Page 1 | Must calculate that 20% of the $60,000 Specific Attachment Point equals $12,000. | FALSE | **Model Failure:** Failed to calculate the secondary threshold, assuming disclosure was only required if claims exceeded $50,000. |  |
| 2 | Identifies the disclosure breach. | Critical | Standard\_Disclosure.pdf, PDF Page 1 | Must recognize that Employee Z had a trigger diagnosis AND incurred $14,000 in claims, which exceeds the $12,000 threshold, thereby mandating disclosure. | FALSE | **Model Failure:** Stated the employer did not breach the contract because $14,000 was below the $60,000 attachment point. | 1 |
| 3 | Applies the penalty for non-disclosure. | Major | Standard\_Disclosure.pdf, PDF Page 1 | Must state that the insurer has zero liability for claims on a risk that was known but not disclosed. | FALSE | **Model Failure:** Allowed the claim to proceed or suggested issuing a retroactive "laser" deductible instead of denying liability. | 2 |
| 4 | Provides an actionable denial recommendation. | Critical | Prompt Text | Must recommend a full denial of the $39,000 claim request. | FALSE | **Model Failure:** Approved the $39,000 claim request because total claims ($99k) exceeded the retention ($60k). | 3 |

**4\. Model Analysis**

This prompt is built around a mathematical threshold trap hidden within Stop-Loss disclosure guidelines. Standard LLMs will calculate the gross loss ($99,000) against the retention ($60,000) and immediately converge on approving the $39,000 overage. To succeed, the model must execute a rigorous Scenario Evaluation of the pre-inception timeline. It must know that Stop-Loss disclosure forms require the declaration of any individual who has a trigger diagnosis AND has incurred expenses exceeding the lesser of $50,000 or 20% of the specific retention. The model must perform the calculation (20% of $60,000 \= $12,000) and compare it to the incurred claims prior to the disclosure date ($14,000). Because $14,000 is greater than $12,000, the employee was a "Known" risk that was omitted. The model must then enforce the absolute penalty: zero liability for the undisclosed risk.

**5\. Golden Response**

CLAIMS STRATEGY MEMO

TO: Stop-Loss Underwriting and Claims Unit

FROM: Medical Stop-Loss Claims Director

DATE: March 1, 2026

SUBJECT: Stop-Loss Claim Denial – Failure to Disclose Known Risk

* Mathematical Analysis of Disclosure Thresholds  
  Under the rules governing the Standard Stop Loss Disclosure Form, a Plan Sponsor is contractually obligated to disclose any risks known to have been diagnosed within the past 12 months with a condition represented by any of the ICD-10 codes contained on the Trigger Diagnosis List, provided they have also received medical services costing the lesser of $50,000 or 20% of the lowest Specific Retention applied for during the same period.  
* Specific Retention (Attachment Point): $60,000  
* 20% of Specific Retention: $12,000  
  Therefore, the employer was required to disclose any individual with a trigger diagnosis who had incurred $12,000 or more in medical expenses prior to the completion of the disclosure form.  
* Scenario Evaluation: Breach of Disclosure Requirements  
  The employer completed the Disclosure Statement on December 1, 2025, marking "No" to all questions. However, as of November 30, 2025, Employee Z had already been diagnosed with a severe cardiovascular illness explicitly listed on the Trigger Diagnosis List. Furthermore, Employee Z had already incurred $14,000 in paid claims. Because the $14,000 in claims exceeded the $12,000 calculated threshold, Employee Z met the exact criteria of a "Known" risk. By failing to list Employee Z on the schedule, the employer breached the disclosure requirements.  
* Risk Assessment and Contractual Liability  
  The Stop-Loss contract dictates strict penalties for non-disclosure. If the Plan Sponsor fails to disclose any risk known to fall into one of the designated categories, either intentionally or because a thorough review of all records was not conducted, then the Company will have no liability for claims on the risk not disclosed. Because the employer failed to report this known risk, we are not obligated to reimburse any funds associated with Employee Z.  
* Actionable Recommendation  
  I recommend issuing a total denial for the $39,000 reimbursement request. While the employee's total claims ($99,000) technically exceed the $60,000 Specific Attachment Point, the employer's failure to disclose Employee Z on the December 1, 2025 Disclosure Statement voids our liability for this specific claimant. No reimbursement will be issued.

### ---

**Example 23: Employment Practices Liability (Wage & Hour Exception Trap)**

**1\. Metadata**

Task Type: Workflow

Category / Domain: Management Liability / Claims

Workflow: Quantum Calculation

Prompt Type: Claims Strategy Memo

Difficulty: Expert

**2\. Prompt**

You are a Management Liability Claims Adjuster. The current date is March 10, 2026\.

Your insured, a regional logistics company, was sued in a class-action lawsuit by its warehouse employees for Wage and Hour violations, specifically alleging failure to pay overtime under the Fair Labor Standards Act (FLSA).

The lawsuit was successfully mediated and settled last week. The insured incurred exactly $100,000 in defense costs (attorney fees) and agreed to pay a $150,000 settlement directly to the employees for the unpaid back-pay.

The insured has a Great American Employment Practices Liability (EPLI) policy (Form D 71100-G).

Policy Details:

* Overall Policy Limit: $1,000,000  
* Retention: $25,000  
* The Declarations page indicates that Endorsement D 71706(7) "Cost of Defense Sub-Limit For \- Wage and Hour Violations" is attached, with a sub-limit of $150,000.

Draft a Claims Strategy Memo to the Claims Director. You must:

* Conduct a comparative analysis of how the base policy form treats "back-pay" under the Definition of Loss (Exclusion H) versus how it treats Wage and Hour violations (Exclusion G).  
* Evaluate the scenario and the specific mechanics of the D 71706(7) endorsement to determine if the $150,000 back-pay settlement is covered.  
* Calculate the exact amount the insurer will pay for this claim, demonstrating the mathematical application of the retention and the sub-limit against the incurred costs.  
* Formulate an actionable recommendation stating the final total dollar payout for this claim.

**Attached Files (Context):**

* epli-policy-specimen.pdf – Public URL: [https://www.abais.com/docs/default-source/small-business/epl/epli-policy-specimen.pdf](https://www.abais.com/docs/default-source/small-business/epl/epli-policy-specimen.pdf)

**3\. Rubric (Negative Failure Focus)**

| \# | Description | Weight | Sources (with exact PDF pages) | Justification | Met | Failure Reasoning (Model Failure Example) | Dependent Criteria |
| :---- | :---- | :---- | :---- | :---- | :---- | :---- | :---- |
| 1 | Analyzes the base policy exclusion conflict. | Critical | epli-policy-specimen.pdf, PDF Page 10 | Must explain that while Exclusion H exceptions back-pay for general claims, Exclusion G acts as an absolute bar for all Wage and Hour claims. | FALSE | **Model Failure:** Read the back-pay exception in Exclusion H and incorrectly assumed all back-pay is covered regardless of the cause of action. |  |
| 2 | Evaluates the D 71706(7) endorsement. | Critical | epli-policy-specimen.pdf, PDF Page 21 | Must identify that the endorsement only buys back "Costs of Defense" for Wage and Hour claims, leaving the indemnity/settlement entirely excluded. | FALSE | **Model Failure:** Assumed the endorsement provided both indemnity and defense coverage since the $150,000 settlement matched the $150,000 sub-limit. |  |
| 3 | Denies the settlement. | Major | Prompt Text | Must issue a complete denial for the $150,000 back-pay settlement. | FALSE | **Model Failure:** Authorized payment for the $150,000 settlement. | 1, 2 |
| 4 | Calculates the covered Defense Costs. | Critical | Prompt Text | Must apply the $25,000 retention against the $100,000 in defense costs to arrive at $75,000. | FALSE | **Model Failure:** Subtracted the retention from the sub-limit limit ($150,000 \- $25,000) or added the settlement and defense together before applying the retention. |  |
| 5 | Provides an actionable payout recommendation. | Critical | Prompt Text | Must recommend a final, total claim payout of exactly $75,000. | FALSE | **Model Failure:** Recommended a payout of $150,000, $225,000, or $125,000 by failing to properly segregate the excluded indemnity from the defense costs. | 3, 4 |

**4\. Model Analysis**

This prompt is designed to trigger an "exception fallacy" and a "number matching" model failure. Standard EPLI policies generally cover back-pay for wrongful termination or discrimination. Consequently, the base policy's Exclusion H contains an exception preserving coverage for back-pay. When an LLM reads the policy, it frequently spots the "back-pay" exception in Exclusion H and prematurely converges on approving the settlement. Furthermore, because the settlement amount ($150,000) matches the sub-limit amount ($150,000), the LLM will hallucinate that the sub-limit was designed to pay the settlement. A successful model must navigate the policy hierarchy: Exclusion G explicitly excludes Wage and Hour violations in their entirety. The attached endorsement, D 71706(7), is highly restrictive and only amends Exclusion G to provide a sub-limit for Costs of Defense. The model must recognize that the $150,000 back-pay settlement remains completely excluded, isolate the $100,000 in defense costs, apply the $25,000 retention ($100,000 \- $25,000 \= $75,000), and recognize this falls well within the $150,000 defense sub-limit.

**5\. Golden Response**

CLAIMS STRATEGY MEMO

TO: Claims Director

FROM: Management Liability Claims Adjuster

DATE: March 10, 2026

SUBJECT: EPLI Coverage Determination – Wage and Hour Class Action

* Comparative Analysis: Exclusion H (Back-Pay) vs. Exclusion G (Wage & Hour)  
  Under the Great American D 71100-G policy form, evaluating coverage requires a careful reading of intersecting exclusions. Exclusion H generally excludes obligations to pay salary, wages, and compensation, but contains a specific carve-back stating this exclusion shall not apply to front pay and back pay. While this preserves coverage for back-pay in standard employment claims (like discrimination), it does not override other absolute exclusions.  
  Exclusion G serves as an absolute bar to coverage for any actual or alleged Wage and Hour Violation. Because the employees' lawsuit alleges a failure to pay overtime under the FLSA, it falls squarely within the definition of a Wage and Hour Violation. Therefore, the entire claim is inherently excluded by Exclusion G, rendering the back-pay exception in Exclusion H irrelevant to this specific cause of action.  
* Scenario Evaluation: Endorsement D 71706(7)  
  The insured purchased Endorsement D 71706(7), the "Cost of Defense Sub-Limit For \- Wage and Hour Violations". This endorsement explicitly amends Exclusion G by stating that the exclusion shall also not apply to Costs of Defense as a result of any Claim for Wage and Hour Violations up to the applicable Sub-Limit of Liability.  
  Crucially, this endorsement only buys back coverage for attorney fees and legal expenses (Costs of Defense). It provides zero indemnity coverage for judgments or settlements. Therefore, the $150,000 settlement agreed to by the insured for unpaid back-pay remains absolutely excluded and is the sole financial responsibility of the insured.  
* Mathematical Calculation of Covered Costs  
  The policy provides a $150,000 sub-limit for Wage and Hour Defense Costs, and each such claim is subject to the retention set forth in the Declarations. The calculation is applied strictly to the eligible Costs of Defense as follows:  
* Gross Costs of Defense Incurred: $100,000  
* Less Applicable Retention: \-$25,000  
* Net Eligible Costs of Defense: $75,000

Because the Net Eligible Costs of Defense ($75,000) is well below the applicable $150,000 sub-limit maximum, the insurer will pay the full net amount.

* Actionable Recommendation  
  I recommend issuing a formal coverage determination letter that definitively denies indemnity coverage for the $150,000 back-pay settlement based on Exclusion G. Simultaneously, we will approve the defense costs. A final payout wire of exactly $75,000 should be issued to the insured to satisfy our obligations under Endorsement D 71706(7).

### ---

**Example 24: Cyber Insurance (System Failure vs. Infrastructure Exclusion)**

**1\. Metadata**

Task Type: Workflow

Category / Domain: Cyber Insurance / Claims

Workflow: Coverage Verification

Prompt Type: Coverage Strategy Memo

Difficulty: Expert

**2\. Prompt**

You are a Cyber Claims Adjuster. The current date is March 25, 2026\.

Your insured, an e-commerce retailer, holds an AIG CyberEdge policy. They opted to purchase the optional "OSP System Failure Cover" which protects against outages at their Outsource Service Providers (OSP).

Last week, the insured's primary cloud hosting facility (a scheduled OSP) went completely offline for 48 hours. The outage occurred because a massive winter storm caused a catastrophic electrical power interruption to the regional power grid. When the grid failed, the OSP's internal backup generators also experienced an unintentional and unplanned mechanical failure, leaving the data center completely without power.

The insured suffered a $500,000 loss of business income during the 48-hour downtime. They have filed a claim under the "OSP System Failure Cover" asserting that because the OSP's own backup generators suffered an unplanned mechanical failure, the resulting outage qualifies as an OSP System Failure.

Draft a Coverage Strategy Memo. You must:

* Conduct a comparative analysis of the policy definitions for "OSP System Failure" versus the Exclusion 3.5 for "Infrastructure".  
* Evaluate the proximate cause of the scenario to determine if the failure of the OSP's backup generators overrides the Infrastructure exclusion.  
* Formulate an actionable recommendation on whether to accept or deny the $500,000 Business Interruption claim.

**Attached Files (Context):**

* cyberedge-policy-documentation.pdf – Public URL: [https://www.aig.co.uk/content/dam/aig/emea/united-kingdom/documents/Financial-lines/Cyber/cyberedge-policy-documentation.pdf](https://www.aig.co.uk/content/dam/aig/emea/united-kingdom/documents/Financial-lines/Cyber/cyberedge-policy-documentation.pdf)

**3\. Rubric (Negative Failure Focus)**

| \# | Description | Weight | Sources (with exact PDF pages) | Justification | Met | Failure Reasoning (Model Failure Example) | Dependent Criteria |
| :---- | :---- | :---- | :---- | :---- | :---- | :---- | :---- |
| 1 | Identifies the OSP System Failure Cover. | Minor | cyberedge-policy-documentation.pdf, PDF Page 5 | Must note that OSP System Failure covers unintentional and unplanned outages of the provider's systems. | FALSE | Failed to define the basic insuring agreement. |  |
| 2 | Identifies Exclusion 3.5 (Infrastructure). | Critical | cyberedge-policy-documentation.pdf, PDF Page 27 | Must state that the policy excludes losses arising out of electrical power interruptions from infrastructure not under the control of the OSP. | FALSE | **Model Failure:** Failed to apply the Infrastructure exclusion to the regional power grid failure. |  |
| 3 | Evaluates proximate cause against the exclusion. | Critical | Insurance Principles | Must conclude that the loss "arose out of" the regional grid failure, triggering the absolute exclusion regardless of the subsequent generator failure. | FALSE | **Model Failure:** Approved the claim by deciding the OSP's generator failure was the dominant cause and qualified as an internal system failure. | 2 |
| 4 | Provides an actionable denial recommendation. | Major | Prompt Text | Must recommend a full denial of the $500,000 claim. | FALSE | **Model Failure:** Recommended paying the claim. | 3 |

**4\. Model Analysis**

This prompt is designed to test an LLM's ability to apply the doctrine of "arising out of" in the context of concurrent causation and strict policy exclusions. The insured's argument is highly persuasive: they bought OSP System Failure coverage, and the OSP's internal generators suffered a mechanical failure. An LLM will naturally align with this logic and approve the claim. To succeed and avoid the failure trap, the model must locate Exclusion 3.5 (Infrastructure) in the AIG CyberEdge policy. This exclusion applies to losses arising out of or attributable to electrical power interruptions of infrastructure not under the control of the OSP (the regional grid). Because the initial triggering event was the regional blackout, the exclusion applies absolutely. The failure of the backup generators is secondary to the excluded peril, meaning the entire claim must be denied.

**5\. Golden Response**

COVERAGE STRATEGY MEMO

TO: Cyber Claims Unit

FROM: Cyber Claims Adjuster

DATE: March 25, 2026

SUBJECT: Coverage Determination – OSP System Failure and Power Grid Outage

* Comparative Analysis: OSP System Failure vs. Infrastructure Exclusion  
  Under the AIG CyberEdge policy, the "OSP System Failure Cover" is designed to insure against a Material Interruption to an outside service provider's computer system caused by an unintentional and unplanned failure of that system. If viewed in isolation, the mechanical failure of the OSP's backup generators appears to fit this description.  
  However, insuring agreements are subject to the policy's exclusions. Exclusion 3.5 "Infrastructure" explicitly states that the Insurer shall not be liable for Loss arising out of, based upon or attributable to any electrical or mechanical failure of infrastructure not under the control of an Outsource Service Provider, including any electrical power interruption, surge, brownout or blackout.  
* Scenario Evaluation: Proximate Cause and Generator Failure  
  The insured argues that the mechanical failure of the OSP's backup generators represents an internal system failure that should trigger coverage. However, the application of Exclusion 3.5 relies on the broad preamble language "arising out of, based upon or attributable to." The catastrophic electrical power interruption to the regional power grid was an infrastructure failure entirely outside the control of the OSP. The facility lost power, and the subsequent failure of the backup generators to mitigate that loss does not change the origin of the outage. Because the entire chain of events—and the resulting 48 hours of downtime—arose directly out of the regional electrical power interruption, Exclusion 3.5 is triggered.  
* Actionable Recommendation  
  I recommend that we issue a full denial of coverage for the $500,000 Business Interruption claim. While the insured purchased OSP System Failure Cover, the loss was ultimately attributable to the failure of the regional electrical grid. Draft the denial letter citing Exclusion 3.5 (Infrastructure), noting that the policy expressly excludes losses arising out of electrical power interruptions not under the control of the Outsource Service Provider.

### **Example 25: Life Insurance ERISA Preemption (The "Egelhoff" Divorce Trap)**

**1\. Metadata**

* Task Type: Workflow  
* Category / Domain: Life Insurance / Claims Legal  
* Workflow: Claims Adjudication & Beneficiary Dispute  
* Prompt Type: Claims Strategy Memo  
* Difficulty: Super Hard (Expert)

**2\. Prompt**

You are a Senior Life Insurance Claims Counsel. The current date is February 15, 2026\.

You are reviewing a contested death benefit claim for an employee of a large technology firm. The employee passed away recently, and two different parties have submitted claims for the death benefit:

* The employee's current spouse (and executor of the estate), who argues that under Washington State law (RCW 11.07.010), the designation of an ex-spouse as a beneficiary is automatically revoked upon divorce.  
* The employee's ex-spouse, who is still listed on the original beneficiary designation form filed with the employer.

Review the attached employee\_records.csv to determine the specific plan type and policy details. Then, review the Supreme Court precedent in *Egelhoff v. Egelhoff* (532 U.S. 141).

Draft a Claims Strategy Memo to the claims disbursement team. You must:

* Extract the Plan Type, Coverage Amount, and Beneficiary on File from the CSV.  
* Conduct a comparative analysis of the Washington State automatic revocation statute versus the federal Employee Retirement Income Security Act of 1974 (ERISA).  
* Evaluate the scenario using the *Egelhoff* precedent to determine if the state law successfully revoked the ex-spouse's beneficiary status.  
* Assess the risk of "double liability" if the insurer incorrectly pays the current spouse/estate.  
* Formulate an actionable recommendation on exactly which party receives the payout and the exact dollar amount.

**Attached Files (Context):**

employee\_records.csv

Code snippet

Emp\_ID,Name,Plan\_Type,Coverage\_Amount,Beneficiary\_on\_File,Divorce\_Date,Death\_Date  
992,David Smith,ERISA Group Life,$500000,Donna Smith (Ex-Spouse),2025-01-15,2025-11-20  
993,Sarah Jones,Individual Term Life,$250000,Mark Jones (Spouse),N/A,N/A

Reference URL: https://supreme.justia.com/cases/federal/us/532/141/

**3\. Rubric (Negative Failure Focus)**

| \# | Description | Weight | Sources (with exact PDF pages) | Justification | Met | Failure Reasoning (Model Failure Example) | Dependent Criteria |
| :---- | :---- | :---- | :---- | :---- | :---- | :---- | :---- |
| 1 | Extracts data from the CSV. | Minor | employee\_records.csv, Row 2 | Must identify the Plan Type as "ERISA Group Life", Amount as $500,000, and Beneficiary as Donna Smith. | FALSE | Failed to pull the correct row from the raw CSV data. | None |
| 2 | Identifies the conflict of laws. | Major | Justia *Egelhoff* Syllabus | Must articulate that Washington state law automatically revokes an ex-spouse, but the plan is governed by federal ERISA rules. | FALSE | **Model Failure:** Assumed state law seamlessly applies to all life insurance contracts within that state. | 1 |
| 3 | Applies the *Egelhoff* ERISA preemption. | Critical | Justia *Egelhoff* Syllabus | Must conclude that ERISA expressly preempts the state revocation statute because it dictates plans must be administered according to plan documents. | FALSE | **Model Failure:** Hallucinated that divorce legally invalidated the beneficiary form, directing funds to the estate/current spouse. | 2 |
| 4 | Assesses the risk of double liability. | Major | Insurance Legal Principles | Must note that paying the estate based on state law exposes the insurer to a second valid claim from the ex-spouse under federal law. | FALSE | Did not assess the financial/legal risk of an incorrect disbursement. | 3 |
| 5 | Provides an actionable payout recommendation. | Critical | Prompt Text | Must recommend paying the full $500,000 to the ex-spouse (Donna Smith), as she remains the legally valid beneficiary under ERISA. | FALSE | **Model Failure:** Authorized payment to the current spouse or the estate, falling for the state law decoy. | 3, 4 |

**4\. Model Analysis**

This prompt utilizes a devastating legal hierarchy trap. Standard LLMs are heavily trained on common law principles, including the widely adopted state laws that automatically revoke an ex-spouse as a life insurance beneficiary upon divorce. When presented with a divorce scenario, an untuned model will instantly validate the current spouse's claim. To succeed, the model must extract the "ERISA Group Life" tag from the CSV and apply the Supreme Court's ruling in *Egelhoff v. Egelhoff*. The model must override its default state-law logic, recognize that federal ERISA law preempts state revocation statutes, and direct the massive $500,000 payout to the ex-spouse, despite the apparent "unfairness" of the situation.

**5\. Golden Response**

CLAIMS STRATEGY MEMO

TO: Claims Disbursement Team

FROM: Senior Life Insurance Claims Counsel

DATE: February 15, 2026

SUBJECT: Beneficiary Dispute – ERISA Preemption and the Egelhoff Precedent

* CSV Data Extraction  
  A review of employee\_records.csv indicates that the deceased employee (David Smith, ID 992\) was covered under an "ERISA Group Life" plan with a Coverage Amount of $500,000. The formal Beneficiary on File at the time of his death was his ex-spouse, Donna Smith.  
* Comparative Analysis: State Law vs. ERISA  
  The current spouse argues that Washington State law (RCW 11.07.010) automatically revokes the designation of a spouse as the beneficiary of a nonprobate asset upon divorce. In standard individual life insurance policies, this state law would likely control, and the ex-spouse would be bypassed. However, because this policy is an employer-sponsored plan, it is governed by the federal Employee Retirement Income Security Act of 1974 (ERISA).  
* Scenario Evaluation: The Egelhoff Precedent  
  The conflict between state revocation laws and federal ERISA guidelines was explicitly resolved by the United States Supreme Court in *Egelhoff v. Egelhoff* (532 U.S. 141). The Court ruled that ERISA's preemption section supersedes any state laws that "relate to" any employee benefit plan. ERISA mandates that plans must be administered, and benefits paid, strictly in accordance with the written plan documents. Therefore, the Washington statute is expressly preempted. The divorce did not legally revoke Donna Smith's status as the beneficiary because the employee never formally updated the ERISA plan documents.  
* Risk Assessment: Double Liability  
  If the claims team incorrectly relies on state law and disburses the $500,000 to the current spouse or the estate, the insurer will face severe double liability. The ex-spouse would have an absolute right under federal law to sue the insurer for a second $500,000 payment, plus statutory interest and attorney's fees, for failing to pay the beneficiary named in the plan documents.  
* Actionable Recommendation  
  I recommend that we formally deny the claim submitted by the current spouse/estate. The claims team must authorize the disbursement of the full $500,000 death benefit to the ex-spouse, Donna Smith. While this may contradict state law and the presumed intent of the deceased, federal ERISA compliance requires strict adherence to the beneficiary designation on file.

### ---

**Example 26: Texas Anti-Indemnity Act (The "Third-Party Over" Exception Trap)**

**1\. Metadata**

* Task Type: Workflow  
* Category / Domain: Commercial Casualty / Construction Law  
* Workflow: Coverage Determination  
* Prompt Type: Coverage Strategy Memo  
* Difficulty: Super Hard (Expert)

**2\. Prompt**

You are a Commercial General Liability (CGL) Adjuster. The current date is March 1, 2026\.

Your insured is a framing Subcontractor on a large commercial project in Texas. The Subcontractor signed a contract with the General Contractor (GC). The contract requires the Subcontractor to indemnify the GC for liabilities arising out of the project, *even if the liability is caused by the GC's sole negligence*. It also requires the Subcontractor to name the GC as an Additional Insured on their CGL policy.

A major crane accident occurred, which OSHA determined was caused by the *sole negligence of the GC*.

Review the attached claim\_incidents.csv and the Texas Anti-Indemnity Act (TAIA) guidelines. The GC has tendered two separate claims to your insured's CGL policy, demanding defense and indemnity as an Additional Insured for both lawsuits stemming from the crane accident.

Draft a Coverage Strategy Memo. You must:

* Extract the details of Claim A-101 and Claim A-102 from the CSV.  
* Conduct a comparative analysis of the broad indemnification clause in the contract versus the statutory prohibitions of the Texas Anti-Indemnity Act (Texas Insurance Code Chapter 151).  
* Evaluate Claim A-101 (Pedestrian) to determine if the GC's demand for Additional Insured coverage is valid.  
* Evaluate Claim A-102 (Subcontractor's Employee) by applying the specific statutory exceptions within the TAIA.  
* Formulate a split, actionable recommendation on whether to accept or deny the GC's tender for each specific claim.

**Attached Files (Context):**

claim\_incidents.csv

Code snippet

Claim\_ID,Injured\_Party,Cause,Damages\_Demanded  
A-101,Pedestrian walking by site,GC Sole Negligence,$200000  
A-102,Subcontractor's own employee,GC Sole Negligence,$500000

Reference URL (Texas Anti-Indemnity Whitepaper): https://www.cokinoslaw.com/wp-content/uploads/2020/02/CY-Anti-Indemnity-White-Paper-Feb.-2020-Update-4836-4946-6805-v.1.pdf

**3\. Rubric (Negative Failure Focus)**

| \# | Description | Weight | Sources (with exact PDF pages) | Justification | Met | Failure Reasoning (Model Failure Example) | Dependent Criteria |
| :---- | :---- | :---- | :---- | :---- | :---- | :---- | :---- |
| 1 | Extracts claim data from CSV. | Minor | claim\_incidents.csv | Must identify A-101 as a pedestrian injury and A-102 as a subcontractor employee injury. | FALSE | Failed to differentiate the two injured parties. | None |
| 2 | Analyzes the TAIA prohibition. | Major | Reference URL | Must state that the Texas Anti-Indemnity Act generally voids construction contracts that require indemnification or additional insured coverage for the indemnitee's (GC's) sole negligence. | FALSE | **Model Failure:** Assumed freedom of contract prevailed and allowed the GC to transfer its sole negligence. | None |
| 3 | Evaluates Claim A-101 (Pedestrian). | Critical | Prompt Text & Reference URL | Must deny the GC's tender for A-101 because TAIA voids the indemnification and additional insured requirement for the GC's sole negligence against a third-party pedestrian. | FALSE | **Model Failure:** Accepted the claim based on the contract language, ignoring the state statute. | 1, 2 |
| 4 | Analyzes the TAIA "Employee" exception. | Critical | Reference URL | Must identify that TAIA contains a specific statutory exception allowing indemnification for a GC's sole negligence if the claim involves bodily injury to the indemnitor's (Subcontractor's) own employee. | FALSE | **Model Failure:** Missed the nuanced statutory carve-back and applied the anti-indemnity ban universally. | 2 |
| 5 | Evaluates Claim A-102 (Employee). | Critical | Prompt Text & Reference URL | Must accept the GC's tender for A-102 because the injured party is the Subcontractor's employee, fitting perfectly into the TAIA exception. | FALSE | **Model Failure:** Denied the claim, erroneously ruling that TAIA completely voids all "sole negligence" transfers. | 1, 4 |
| 6 | Provides an actionable split recommendation. | Major | Prompt Text | Must recommend denying defense/indemnity for A-101 while accepting defense/indemnity for A-102. | FALSE | Issued a blanket acceptance or blanket denial for both claims. | 3, 5 |

**4\. Model Analysis**

This prompt executes a sophisticated "statutory exception" trap. Standard LLMs that are aware of state Anti-Indemnity Statutes will correctly identify that a Subcontractor cannot be forced to insure a General Contractor for the GC's *sole negligence*. Consequently, the model will proudly declare the contract void under the Texas Anti-Indemnity Act (TAIA) and deny both claims. However, it fails to recognize the "Third-Party Over Action" carve-out. The TAIA explicitly contains an exception: a Subcontractor *can* indemnify a GC for the GC's sole negligence if the injured party is the Subcontractor's own employee. The model must carefully parse the CSV, separate the pedestrian claim from the employee claim, apply the general ban to the pedestrian, apply the statutory exception to the employee, and issue a highly counter-intuitive split coverage determination.

**5\. Golden Response**

COVERAGE STRATEGY MEMO

TO: Construction Defect/Casualty Claims Team

FROM: CGL Adjuster

DATE: March 1, 2026

SUBJECT: Additional Insured Tender – Application of the Texas Anti-Indemnity Act

* CSV Data Extraction & Claim Overview  
  The General Contractor (GC) has tendered two claims to our insured's CGL policy, both resulting from a crane accident caused by the GC's sole negligence. Per the claim\_incidents.csv, Claim A-101 involves a third-party pedestrian seeking $200,000, and Claim A-102 involves our insured's (the Subcontractor's) own employee seeking $500,000.  
* Comparative Analysis: Contractual Indemnity vs. TAIA  
  The subcontract requires our insured to indemnify the GC and provide Additional Insured coverage even for the GC's sole negligence. However, under the Texas Anti-Indemnity Act (TAIA), any provision in a construction contract that requires an indemnitor to indemnify, hold harmless, or defend an indemnitee against the indemnitee's own negligence is generally void and unenforceable as against public policy. Furthermore, TAIA voids any corresponding requirement to provide Additional Insured coverage for that same uninsurable negligence.  
* Scenario Evaluation: Claim A-101 (Pedestrian)  
  Because Claim A-101 involves a third-party pedestrian injured by the GC's sole negligence, the general prohibition of TAIA strictly applies. The contractual requirement for our insured to cover the GC for this loss is void. Consequently, the Additional Insured endorsement on our policy cannot be triggered for the GC's sole fault. We owe no duty to defend or indemnify the GC for the $200,000 pedestrian claim.  
* Scenario Evaluation: Claim A-102 (Subcontractor's Employee)  
  Claim A-102 presents a uniquely different legal scenario. While TAIA generally voids indemnification for the indemnitee's sole negligence, the statute contains a critical exception. In light of the prevalence of "Third-Party Over" actions in Texas, TAIA explicitly provides an exception for bodily injury to the indemnitor's (Subcontractor's) own employees. Under these specific circumstances, indemnification and Additional Insured coverage for the GC's own negligence is allowed by law and the contract provision survives.  
* Actionable Recommendation  
  I recommend issuing a split coverage determination to the General Contractor:  
* **Claim A-101 (Pedestrian):** Issue a formal Denial of Coverage. Advise the GC that the indemnity and Additional Insured requirements are void under TAIA for third-party injuries caused by their sole negligence.  
* **Claim A-102 (Employee):** Accept the tender. Because this claim falls under the TAIA employee exception, the contract provision is enforceable, and we must defend and indemnify the GC for the $500,000 suit as an Additional Insured under our policy.

### ---

### **Example 27: D\&O "Bump-Up" Exclusion vs. Section 14(a) Statutory Remedy Trap**

**1\. Metadata**

* Task Type: Workflow  
* Category / Domain: Executive Liability (D\&O) / Claims Legal  
* Workflow: Coverage Determination & Litigation Strategy  
* Prompt Type: Claims Strategy Memo  
* Difficulty: Super Hard (Expert)

**2\. Prompt**

You are a D\&O Claims Counsel for a major insurer. The current date is January 20, 2026\.

Your insured, a publicly traded tech company, successfully acquired a smaller rival in 2024 via a "reverse triangular merger." Following the acquisition, the shareholders of the acquired company filed a class-action lawsuit against your insured's Directors and Officers.

The lawsuit alleged violations of Section 14(a) of the Securities Exchange Act of 1934, claiming the proxy statements contained material misrepresentations that misled shareholders into voting for an undervalued transaction. The lawsuit recently settled.

The primary D\&O policy includes a standard "Bump-Up" Exclusion, which states: *"In the event of a Claim alleging that the price or consideration paid for the acquisition of an entity is inadequate, Loss shall not include any amount of any judgment or settlement representing the amount by which such price or consideration is effectively increased."*

Review the attached merger\_settlement\_data.csv and the AIG D\&O policy specimen. Draft a Claims Strategy Memo. You must:

* Extract the specific cause of action and the settlement amount from the CSV.  
* Conduct a comparative analysis of the "Bump-Up" exclusion's language against the specific statutory remedies available under Section 14(a) of the Securities Exchange Act.  
* Evaluate the scenario using recent 2024/2025 case law trends (e.g., the *Towers Watson* 4th Circuit or *Harman* Delaware decisions) regarding whether a Section 14(a) settlement legally constitutes an "effective increase in consideration."  
* Assess the litigation risk if the insurer attempts to apply the Bump-Up exclusion to this specific settlement.  
* Formulate an actionable recommendation on whether to accept or deny coverage for the settlement amount.

**Attached Files (Context):**

merger\_settlement\_data.csv

Code snippet

Claim\_ID,Acquisition\_Type,Cause\_of\_Action,Defense\_Costs,Settlement\_Amount  
SEC-991,Direct Merger,Breach of Fiduciary Duty,$2000000,$15000000  
SEC-992,Reverse Triangular Merger,Section 14(a) Proxy Violation,$4500000,$40000000

AIG D\&O Policy Specimen: https://www.aig.com/content/dam/aig/america-canada/us/documents/business/management-liability/aig-statement-series/d-and-o-statement-policy.pdf

**3\. Rubric (Negative Failure Focus)**

| \# | Description | Weight | Sources (with exact PDF pages) | Justification | Met | Failure Reasoning (Model Failure Example) | Dependent Criteria |
| :---- | :---- | :---- | :---- | :---- | :---- | :---- | :---- |
| 1 | Extracts claim data from CSV. | Minor | merger\_settlement\_data.csv, Row 3 | Must identify Claim SEC-992, noting the Section 14(a) cause of action and the $40,000,000 settlement amount. | FALSE | Failed to pull the correct row from the raw CSV data. | None |
| 2 | Analyzes the Bump-Up Exclusion mechanics. | Major | AIG D\&O Policy Specimen | Must state the exclusion bars coverage for settlements that represent an increase in the purchase price/consideration of an acquired entity. | FALSE | Failed to accurately describe the purpose of the exclusion. | 1 |
| 3 | Evaluates the Section 14(a) statutory remedy nuance. | Critical | Securities Exchange Act Case Law | Must recognize that Section 14(a) allows only for *compensatory damages* based on proxy misstatements, not an equitable reformation or actual "increase in consideration." | FALSE | **Model Failure:** Read the exclusion and the fact that shareholders wanted more money, and incorrectly assumed the settlement legally classified as "increased consideration." | 2 |
| 4 | Applies recent appellate case law (*Towers/Harman*). | Critical | Delaware/4th Circuit Case Law 2024-2025 | Must cite that recent courts strictly construe this exclusion against insurers, holding that a reverse triangular merger and a Section 14(a) claim do not fit the narrow definition of the Bump-Up exclusion. | FALSE | **Model Failure:** Assumed the insurer's exclusion is broadly interpreted and applies to any M\&A shareholder dispute over price. | 3 |
| 5 | Provides an actionable approval recommendation. | Critical | Prompt Text | Must recommend *approving* coverage for the $40,000,000 settlement, overriding the Bump-Up exclusion. | FALSE | **Model Failure:** Recommended denying the $40M settlement, falling directly into the exclusionary trap. | 3, 4 |

**4\. Model Analysis**

This prompt is a highly sophisticated legal trap. An untuned LLM will read the "Bump-Up" exclusion, see that the shareholders sued because the acquisition price was "undervalued," and immediately conclude that the $40M settlement is an excluded "bump-up" in price. It will confidently deny the claim. However, an expert-level model must recognize the exact statutory nature of the CSV data: the claim is a *Section 14(a) Proxy Violation*. Under recent watershed rulings (like the 4th Circuit *Towers Watson* decision or Delaware's *Harman* case), courts ruled that Section 14(a) claims only allow for compensatory damages, not equitable "consideration increases," and that reverse triangular mergers often fall outside the narrow wording of these exclusions. The model must suppress its semantic association (undervalued \= bump-up) and apply strict legal interpretation to authorize the massive $40M payout.

**5\. Golden Response**

CLAIMS STRATEGY MEMO

TO: D\&O Claims Disbursement Team

FROM: D\&O Claims Counsel

DATE: January 20, 2026

SUBJECT: Bump-Up Exclusion Applicability – SEC-992 Reverse Triangular Merger

* CSV Data Extraction & Claim Overview  
  Pursuant to merger\_settlement\_data.csv, we are evaluating Claim SEC-992. The insured completed a Reverse Triangular Merger and is facing a $40,000,000 settlement arising exclusively from a Section 14(a) Proxy Violation cause of action, alongside $4,500,000 in defense costs.  
* Comparative Analysis: Bump-Up Exclusion vs. Section 14(a) Remedies  
  The primary policy contains a standard Bump-Up Exclusion, which precludes coverage for any settlement representing an amount by which the price or consideration of an acquisition is "effectively increased." On its face, because the shareholders alleged the transaction was undervalued, the claims department's initial instinct is to apply this exclusion.  
  However, this ignores the strict statutory nature of the plaintiffs' cause of action. The plaintiffs sued under Section 14(a) of the Securities Exchange Act, which prohibits material misstatements in proxy solicitations. The legal remedy for a Section 14(a) violation is compensatory damages for the economic harm caused by the misleading proxy, *not* an equitable reformation of the merger contract or a retroactive "increase in consideration."  
* Scenario Evaluation & Case Law Precedent  
  Recent appellate decisions have severely restricted insurers' ability to apply the Bump-Up exclusion to these specific fact patterns. Following the precedent established in cases such as the 4th Circuit's *Towers Watson* decision and the Delaware Superior Court's *Harman* decision, courts have held that insurers bear a heavy burden to prove the exclusion unambiguously applies. Because Section 14(a) yields compensatory damages rather than increased deal consideration, and because a reverse triangular merger often does not fit the strict definition of the "acquisition of all or substantially all" of the entity's assets, courts are routinely ruling that the Bump-Up exclusion does *not* apply.  
* Risk Assessment & Actionable Recommendation  
  If we issue a denial based on the Bump-Up exclusion, we face an extreme risk of bad-faith litigation and breach of contract, as the prevailing case law currently heavily favors the policyholder in 14(a) proxy dispute settlements.  
  Therefore, I recommend that we waive the Bump-Up exclusion defense. We must ACCEPT coverage for the $40,000,000 settlement (and the $4,500,000 in defense costs), subject to the policy's remaining aggregate limits and applicable retentions.

### ---

**Example 28: CGL "Self-Insured Retention" vs. "Bankruptcy" Condition Trap**

**1\. Metadata**

* Task Type: Workflow  
* Category / Domain: Commercial General Liability / Complex Casualty  
* Workflow: Coverage Verification & Litigation  
* Prompt Type: Coverage Determination Memo  
* Difficulty: Super Hard (Expert)

**2\. Prompt**

You are a Complex Casualty Claims Director. The current date is March 5, 2026\.

Your insured, a heavy civil contractor, holds an ISO CG 00 01 04 13 Commercial General Liability policy. The policy is modified by a manuscript Self-Insured Retention (SIR) Endorsement.

During a major excavation project, the insured severed a critical municipal water main, causing massive third-party property damage. Shortly after the incident, the insured filed for Chapter 7 bankruptcy and completely dissolved. The third-party claimants have now sued your insurance company directly, demanding that you drop down to defend and indemnify the claim from dollar one.

The claimants argue that under Section IV (Commercial General Liability Conditions), Paragraph 1 of the CG 00 01 form, the policy clearly states: *"Bankruptcy or insolvency of the insured or of the insured's estate will not relieve us of our obligations under this Coverage Part."*

Review the attached sir\_erosion\_log.csv and the ISO CG 00 01 specimen. Draft a Coverage Determination Memo. You must:

* Extract the insured's SIR limit and the amount of the SIR actually paid by the insured from the CSV.  
* Conduct a comparative analysis of the policy's base "Bankruptcy" condition versus the strict exhaustion mechanics of a true Self-Insured Retention endorsement.  
* Evaluate the scenario to determine if the insurer has a "Duty to Defend" or indemnify the claimants given the insured's insolvency.  
* Formulate an actionable recommendation on whether to accept the claimants' demand to drop down and cover the loss from dollar one.

**Attached Files (Context):**

sir\_erosion\_log.csv

Code snippet

Policy\_Num,SIR\_Amount,SIR\_Eroded\_by\_Defense,SIR\_Eroded\_by\_Indemnity,Remaining\_SIR\_Obligation,Insured\_Status  
CGL-8820,$500000,$0,$0,$500000,Chapter 7 Bankrupt  
CGL-8821,$250000,$250000,$0,$0,Active

ISO CG 00 01 04 13 Specimen: https://ogs.ny.gov/system/files/documents/2021/09/cg-00-01-01-96.pdf (Assume standard 04 13 conditions apply).

**3\. Rubric (Negative Failure Focus)**

| \# | Description | Weight | Sources (with exact PDF pages) | Justification | Met | Failure Reasoning (Model Failure Example) | Dependent Criteria |
| :---- | :---- | :---- | :---- | :---- | :---- | :---- | :---- |
| 1 | Extracts SIR data from the CSV. | Minor | sir\_erosion\_log.csv, Row 2 | Must identify Policy CGL-8820, noting a $500,000 SIR with $0 eroded and a remaining obligation of $500,000. | FALSE | Failed to pull the correct row from the raw CSV data. | None |
| 2 | Analyzes the base Bankruptcy condition. | Minor | ISO CG 00 01 04 13, Section IV.1 | Must acknowledge the claimants' argument that the base policy states bankruptcy does not relieve the insurer of its obligations. | FALSE | Failed to address the claimant's primary contractual argument. | None |
| 3 | Distinguishes SIRs from Deductibles. | Critical | Insurance Principles | Must explain that an SIR is not a deductible; the insurer's liability and duty to defend *do not attach* until the SIR is physically exhausted by the insured's own payments. | FALSE | **Model Failure:** Conflated the SIR with a deductible, assuming the insurer must pay first-dollar and try to collect from the bankrupt estate later. | 1, 2 |
| 4 | Evaluates the hierarchy of the Endorsement. | Critical | Insurance Contract Law | Must state that the manuscript SIR endorsement alters the insuring agreement and supersedes the base bankruptcy condition. Because the $500k was not paid, the condition precedent to coverage was failed. | FALSE | **Model Failure:** Hallucinated that the Bankruptcy clause forces the insurer to "drop down" and absorb the unpaid SIR. | 3 |
| 5 | Provides an actionable denial recommendation. | Major | Prompt Text | Must recommend a total denial of the claimants' demand for first-dollar defense and indemnity. | FALSE | **Model Failure:** Authorized the claims team to drop down and defend the lawsuit due to the insured's insolvency. | 4 |

**4\. Model Analysis**

This prompt tests the LLM against the "Bankruptcy Drop-Down" trap. Standard CGL policies contain a statutory "Bankruptcy" clause protecting third parties if the insured goes broke. When an untuned LLM reads the claimant's argument citing this exact ISO CG 00 01 clause, it will instantly validate the claim and order the insurer to step in. However, the policy is modified by a *Self-Insured Retention (SIR)* endorsement. An expert model must understand the fundamental legal difference between a deductible and an SIR. With an SIR, the policy does not even attach—and the duty to defend does not exist—until the insured *actually pays* the full SIR amount. The CSV proves the insured paid $0. Therefore, the condition precedent to trigger the policy was never met. The bankruptcy clause prevents an insurer from using bankruptcy as an excuse to avoid a *triggered* policy; it does *not* force an insurer to drop down and absorb an unexhausted SIR. The model must override the base policy clause and deny the claim.

**5\. Golden Response**

COVERAGE DETERMINATION MEMO

TO: Casualty Claims Unit

FROM: Complex Casualty Claims Director

DATE: March 5, 2026

SUBJECT: Claimant Demand for Drop-Down Coverage – Unexhausted SIR and Bankruptcy

* CSV Data Extraction  
  A review of the sir\_erosion\_log.csv for the heavy civil contractor (Policy CGL-8820) confirms that the policy carries a $500,000 Self-Insured Retention (SIR). At the time of the insured's Chapter 7 bankruptcy filing, $0 had been paid toward defense, and $0 had been paid toward indemnity. The remaining SIR obligation is the full $500,000.  
* Comparative Analysis: Bankruptcy Condition vs. SIR Exhaustion  
  The third-party claimants are relying on Section IV, Paragraph 1 of the ISO CG 00 01 form, which states that the bankruptcy or insolvency of the insured will not relieve the insurer of its obligations. If this policy simply contained a standard deductible, the claimants would be correct: the insurer would be obligated to defend and indemnify from dollar one and become an unsecured creditor in the bankruptcy court to recover the deductible.  
  However, this policy is modified by a manuscript Self-Insured Retention (SIR) Endorsement. In insurance law, an SIR is fundamentally different from a deductible. A true SIR endorsement establishes a condition precedent: the insurance policy (and the corresponding Duty to Defend) does not attach or become active until the insured has actually paid the full SIR amount out-of-pocket.  
* Scenario Evaluation  
  The manuscript SIR endorsement supersedes the standard provisions of the base CGL form. While the bankruptcy clause prohibits us from using the insured's insolvency to avoid paying a *triggered* obligation, it does not rewrite the attachment point of the policy. Because the insured paid $0 of their $500,000 SIR prior to dissolving, the condition precedent was never satisfied. The CGL policy has not attached. Therefore, we have no legal duty to drop down, absorb the $500,000 retention, or assume the defense of the bankrupt entity.  
* Actionable Recommendation  
  I recommend that we issue a formal denial to the third-party claimants. We must explicitly reject their demand for first-dollar defense and indemnity. Our correspondence should cite the failure to satisfy the $500,000 SIR as a failure of a condition precedent, confirming that our excess coverage obligations under the CGL policy have not been triggered.

### **Example 29: Commercial General Liability (The "Impaired Property" vs. Subcontractor Exception Trap)**

**1\. Metadata**

* Task Type: Workflow  
* Category / Domain: Commercial General Liability / Construction Defect  
* Workflow: Coverage Determination  
* Prompt Type: Claims Strategy Memo  
* Difficulty: Super Hard (Expert)

**2\. Prompt**

You are a Complex Casualty Claims Adjuster. The current date is January 12, 2026\.

Your insured is a General Contractor (GC) who was hired to build a climate-controlled pharmaceutical storage warehouse. The GC hired a specialized Subcontractor to install the custom HVAC system.

Shortly after the warehouse opened, the HVAC system failed entirely due to defective installation by the Subcontractor. Because the warehouse could not maintain the required temperature, the owner could not store any pharmaceuticals. There was no physical damage to the warehouse building itself (no fires, leaks, or broken walls), nor was any pharmaceutical inventory actually destroyed. However, the warehouse owner lost three months of expected rental income/use while waiting for the Subcontractor to tear out and replace the defective HVAC units.

The warehouse owner has sued your insured (the GC) for the lost revenue. The GC has filed a claim under their ISO CG 00 01 Commercial General Liability policy. The GC's broker argues that because the defective work was performed by a Subcontractor, the "subcontractor exception" to the "Damage to Your Work" exclusion applies, meaning the loss of use claim must be fully covered.

Review the attached project\_damages\_log.csv and the ISO CG 00 01 specimen.

Draft a Claims Strategy Memo. You must:

* Extract the specific elements of damage and their physical injury status from the CSV.  
* Conduct a comparative analysis of Exclusion "l" (Damage to Your Work) versus Exclusion "m" (Impaired Property).  
* Evaluate the broker's argument regarding the subcontractor exception in the context of the specific damages claimed.  
* Assess the insurer's financial risk and duty to defend this specific lawsuit.  
* Formulate an actionable recommendation on whether to accept or deny the claim for the lost revenue.

**Attached Files (Context):**

project\_damages\_log.csv

Code snippet

Claim\_ID,Damaged\_Element,Damage\_Type,Physical\_Injury\_Present,Performed\_By,Amount\_Claimed  
CGL-901,HVAC Units,Defective Installation,Yes,Subcontractor,$150000  
CGL-902,Warehouse Facility,Loss of Use / Lost Revenue,No,N/A,$450000

ISO CG 00 01 Specimen: https://ogs.ny.gov/system/files/documents/2021/09/cg-00-01-01-96.pdf

**3\. Rubric (Negative Failure Focus)**

| \# | Description | Weight | Sources (with exact PDF pages) | Justification | Met | Failure Reasoning (Model Failure Example) | Dependent Criteria |
| :---- | :---- | :---- | :---- | :---- | :---- | :---- | :---- |
| 1 | Extracts data from the CSV. | Minor | project\_damages\_log.csv | Must identify that the Warehouse Facility suffered "Loss of Use", no physical injury is present, and the claim is for $450,000. | FALSE | Failed to differentiate the physical damage to the HVAC from the economic loss of use to the warehouse. | None |
| 2 | Analyzes Exclusion L (Damage to Your Work). | Major | ISO CG 00 01 PDF, Page 4 | Must explain that Exclusion "l" normally excludes damage to the insured's work, but contains an exception restoring coverage if the work was performed by a subcontractor. | FALSE | Failed to define the subcontractor exception that the broker is relying upon. | None |
| 3 | Identifies Exclusion M (Impaired Property). | Critical | ISO CG 00 01 PDF, Page 4 & 11 | Must state that Exclusion "m" applies to loss of use of tangible property that has not been physically injured, if it can be restored by repairing the defective work. | FALSE | **Model Failure:** Completely ignored Exclusion M, focusing entirely on the subcontractor exception in Exclusion L. | 1 |
| 4 | Evaluates the hierarchy trap (No Subcontractor Exception in M). | Critical | ISO CG 00 01 PDF, Page 4 | Must recognize that unlike Exclusion "l", Exclusion "m" *does not* contain a subcontractor exception. Therefore, Exclusion "m" overrides the broker's argument. | FALSE | **Model Failure:** Hallucinated that the subcontractor exception in Exclusion L also applies to Exclusion M, or assumed that any subcontractor involvement forces coverage. | 2, 3 |
| 5 | Provides an actionable denial recommendation. | Critical | Prompt Text | Must recommend a full denial of the $450,000 loss of use claim. | FALSE | **Model Failure:** Authorized coverage for the $450,000 claim, falling for the broker's decoy argument. | 4 |

**4\. Model Analysis**

This prompt is designed to trigger a "decoy exception" model failure. The prompt explicitly feeds the LLM the broker's argument: the work was done by a subcontractor, so the "Damage to Your Work" (Exclusion L) subcontractor exception saves the coverage. An untuned LLM will read the CSV, confirm the subcontractor did the work, read the exception in Exclusion L, and confidently approve the $450,000 claim. The model fails to recognize the independent, absolute nature of Exclusion M ("Damage to Impaired Property or Property Not Physically Injured"). Because the warehouse was not physically injured (as confirmed by the CSV) and simply lost its usefulness due to the defective HVAC, it meets the exact definition of Impaired Property. Exclusion M has *no* subcontractor exception. The model must suppress its instinct to agree with the heavily structured broker argument, pivot to the secondary exclusion, and issue a hard denial.

**5\. Golden Response**

CLAIMS STRATEGY MEMO

TO: Casualty Claims Unit

FROM: Complex Casualty Claims Adjuster

DATE: January 12, 2026

SUBJECT: Coverage Determination – HVAC Defect and Impaired Property

* CSV Data Extraction & Claim Overview  
  According to the project\_damages\_log.csv, we are evaluating Claim CGL-902 for $450,000. This amount represents the warehouse owner's "Loss of Use / Lost Revenue." Crucially, the CSV confirms that there is "No" physical injury present to the warehouse facility itself. The physical damage is isolated strictly to the HVAC units (CGL-901), which were installed by a Subcontractor.  
* Comparative Analysis: Exclusion "l" vs. Exclusion "m"  
  The insured's broker argues that the claim is covered because of the "subcontractor exception" to Exclusion "l" (Damage To Your Work). Exclusion "l" eliminates coverage for property damage to the insured's own completed work. The broker is correct that this exclusion does not apply if the damaged work was performed on the insured's behalf by a subcontractor. If the claim were for physical damage to the walls caused by the HVAC, the exception would apply.  
  However, policies are subject to all exclusions operating independently. Exclusion "m" (Damage To Impaired Property or Property Not Physically Injured) excludes coverage for property damage to property that has not been physically injured, arising out of a defect in "your work," if the property can be restored to use by the repair or replacement of that work.  
* Scenario Evaluation: The Hierarchy Trap  
  The warehouse meets the exact ISO definition of "Impaired Property." It is tangible property that cannot be used because it incorporates the insured's defective work (the HVAC), but there is no physical injury to the warehouse itself, and it will be restored to full use once the HVAC is replaced.  
  Critically, unlike Exclusion "l", Exclusion "m" does *not* contain a subcontractor exception. The fact that a subcontractor performed the defective installation is entirely irrelevant when evaluating Impaired Property. Exclusion "m" acts as an absolute bar to coverage for the loss of use of the uninjured warehouse.  
* Actionable Recommendation  
  I recommend issuing a formal Denial of Coverage for the $450,000 loss of use claim. The broker's reliance on the subcontractor exception in Exclusion "l" is misguided because the claim is independently and absolutely barred by Exclusion "m" (Impaired Property). As there is no potential for coverage under the policy for this purely economic loss of use without physical injury, we have no duty to defend or indemnify the insured in this lawsuit.

### ---

**Example 30: D\&O Liability (Application Severability vs. Insolvency Drop-Down Trap)**

**1\. Metadata**

* Task Type: Workflow  
* Category / Domain: Executive Liability (D\&O) / Underwriting & Claims  
* Workflow: Coverage Verification & Rescission  
* Prompt Type: Claims Strategy Memo  
* Difficulty: Super Hard (Expert)

**2\. Prompt**

You are a D\&O Claims Counsel. The current date is February 25, 2026\.

Your insured, a publicly traded healthcare company, purchased a primary Directors & Officers (D\&O) liability policy. Six months into the policy period, the company collapsed into Chapter 7 bankruptcy liquidation.

Following the bankruptcy, a shareholder class action was filed against three independent, outside directors ("the Innocent Directors"). The lawsuit alleges they breached their fiduciary duties by failing to supervise the Chief Financial Officer (CFO). Investigations have proven that the CFO was actively committing massive accounting fraud during the time the D\&O policy was being negotiated. The CFO was the sole signatory on the D\&O insurance application and knowingly attached falsified financial statements to secure the coverage. The three Innocent Directors had absolutely no knowledge of this fraud.

The Innocent Directors have demanded Side A coverage (Non-Indemnifiable Loss) to fund their defense, noting that the bankrupt company cannot indemnify them. Their counsel cites the policy's "Bankruptcy" clause, which states: *"Bankruptcy or insolvency of the Company shall not relieve the Insurer of any of its obligations hereunder."*

Review the attached application\_metadata.csv and the industry D\&O Severability guidelines. Draft a Claims Strategy Memo. You must:

* Extract the application signing authority and policy severability type from the CSV.  
* Conduct a comparative analysis of the "Bankruptcy" clause versus the legal mechanics of "Limited Application Severability."  
* Evaluate the scenario to determine if the CFO's fraud can be imputed to the Innocent Directors for the purpose of rescinding the policy.  
* Assess the legal risk and formulate an actionable recommendation on whether to drop down and pay the Innocent Directors' Side A defense costs, or rescind the policy entirely.

**Attached Files (Context):**

application\_metadata.csv

Code snippet

Policy\_Num,Application\_Signer,Signer\_Knowledge\_of\_Fraud,Innocent\_Directors\_Knowledge,Severability\_Type  
DNO-4451,Chief Financial Officer,Yes,No,Limited Severability

D\&O Severability Guidelines: https://baileycav.com/wp-content/uploads/2023/02/d\_o\_insurance\_application\_severability.pdf

**3\. Rubric (Negative Failure Focus)**

| \# | Description | Weight | Sources (with exact PDF pages) | Justification | Met | Failure Reasoning (Model Failure Example) | Dependent Criteria |
| :---- | :---- | :---- | :---- | :---- | :---- | :---- | :---- |
| 1 | Extracts data from the CSV. | Minor | application\_metadata.csv, Row 2 | Must identify that the CFO signed the app, knew of the fraud, the Innocent Directors did not, and the policy has "Limited Severability". | FALSE | Failed to pull the correct row/data from the raw CSV. | None |
| 2 | Defines Limited Severability. | Critical | Severability Guidelines URL | Must explain that under "Limited Severability," the knowledge of the person who signed the application (the CFO) is imputed to *all* other insureds. | FALSE | **Model Failure:** Confused "Limited" with "Full" severability, hallucinating that the innocent directors' lack of knowledge protected them. | 1 |
| 3 | Analyzes the Application vs Conduct Exclusion trap. | Major | Insurance Contract Law | Must differentiate between innocent insured protections in *claims conduct* versus misrepresentations in the *application* that void the contract. | FALSE | **Model Failure:** Applied the concept of an "innocent insured" to the underwriting application, rather than recognizing it as a rescission issue. | 2 |
| 4 | Evaluates the Bankruptcy Clause argument. | Critical | Prompt Text & Insurance Principles | Must conclude that the Bankruptcy clause only applies to a *valid* contract. If the policy is rescinded *ab initio* due to application fraud, the Bankruptcy clause is legally voided along with the rest of the policy. | FALSE | **Model Failure:** Ordered the insurer to provide Side A drop-down coverage because the company was bankrupt and the directors were innocent. | 3 |
| 5 | Provides an actionable rescission recommendation. | Critical | Prompt Text | Must recommend rescinding the policy in its entirety and denying all defense costs for the Innocent Directors. | FALSE | **Model Failure:** Recommended paying the defense costs under Side A. | 4 |

**4\. Model Analysis**

This prompt is built around a severe emotional and legal hierarchy trap. The prompt emphasizes that the directors are "Innocent" and that the company is bankrupt, which strongly triggers an LLM's training regarding Side A D\&O coverage (which specifically exists to protect innocent directors when a company is insolvent). An untuned LLM will read the Bankruptcy clause, sympathize with the innocent directors, and authorize the Side A payout. However, the model must extract the Severability\_Type from the CSV ("Limited Severability"). According to D\&O jurisprudence, under limited severability, the knowledge of the individual who signs the application is imputed to *all* insureds. Because the fraudulent CFO signed the app, his fraud is imputed to the innocent directors during the underwriting process. The legal consequence is rescission *ab initio* (the contract never existed). If the contract never existed, the Bankruptcy clause and Side A coverage are completely void. The model must override the "innocent" decoy and ruthlessly apply the rescission mechanism.

**5\. Golden Response**

CLAIMS STRATEGY MEMO

TO: D\&O Claims & Underwriting Unit

FROM: D\&O Claims Counsel

DATE: February 25, 2026

SUBJECT: Rescission Analysis – CFO Application Fraud and Limited Severability

* CSV Data Extraction  
  According to the application\_metadata.csv for Policy DNO-4451, the insurance application was signed exclusively by the Chief Financial Officer. The data confirms the CFO had active knowledge of the fraud at the time of signing, while the three outside directors had no knowledge. Crucially, the policy was issued with "Limited Severability" regarding the application.  
* Comparative Analysis: Limited Severability vs. Innocent Insureds  
  The directors' counsel is attempting to rely on the general concept of an "innocent insured." While many policies protect innocent directors from conduct exclusions (e.g., if one director steals, the others still get defense coverage for the ensuing lawsuit), misrepresentations made on the insurance *application* are governed by the Severability clause.  
  As outlined in standard D\&O guidelines, a "Limited Severability" provision dictates that the knowledge of the individual who physically signs the application (in this case, the CFO) is automatically imputed to all other Insureds. Because the CFO knew the attached financials were materially false, that fraudulent knowledge is imputed to the Innocent Directors for the purposes of determining the validity of the contract formation.  
* Scenario Evaluation: The Bankruptcy Clause Trap  
  The directors' counsel cites the "Bankruptcy" condition to demand Side A drop-down coverage. This is a legally flawed argument. The Bankruptcy clause ensures that if the policy is in force, the insurer cannot use the corporate entity's insolvency to avoid paying a triggered claim. However, because the application contained material, fraudulent misrepresentations signed by the CFO, the insurer has the right to rescind the policy *ab initio* (from the beginning). If the policy is rescinded, the entire insurance contract—including the Bankruptcy clause and the Side A insuring agreement—is rendered null and void. The insurer is not relying on the bankruptcy to avoid paying; it is relying on fraud in the inducement.  
* Actionable Recommendation  
  I recommend that we immediately initiate an action to rescind Policy DNO-4451 in its entirety. Return the paid premium to the bankruptcy estate. We must issue a formal denial of the Innocent Directors' demand for Side A defense costs. Under the strict mechanics of Limited Severability, the CFO's fraud is imputed to them, voiding the policy *ab initio* and extinguishing any duty to defend.

### **Example 31: Workers' Compensation (LHWCA Section 3(e) Aggregate Credit Trap)**

**1\. Metadata**

* Task Type: Workflow  
* Category / Domain: Workers' Compensation / Federal Admiralty Law  
* Workflow: Claims Adjudication & Quantum Calculation  
* Prompt Type: Legal Analysis Memo  
* Difficulty: Super Hard (Expert)

**2\. Prompt**

You are a Federal Workers' Compensation Claims Examiner. The current date is March 1, 2026\.

An employee of a maritime construction company was severely injured while repairing a pier over navigable waters, establishing concurrent jurisdiction under both the State Workers' Compensation Act and the federal Longshore and Harbor Workers' Compensation Act (LHWCA).

The claimant initially filed under the State Act. The state workers' compensation carrier accepted the claim and made several payments before the claimant subsequently filed a concurrent claim under the federal LHWCA seeking higher federal benefit rates. An Administrative Law Judge (ALJ) has just issued the final federal LHWCA award.

The claimant's attorney argues that under a "category-by-category" approach, the employer owes a federal deficit because the federal Temporary Total Disability (TTD) award was higher than the state TTD payment, completely ignoring the fact that the state paid a Permanent Partial Disability (PPD) award that the federal judge denied.

Review the attached concurrent\_claim\_ledger.csv and Section 3(e) of the LHWCA. Draft a Legal Analysis Memo. You must:

* Extract the exact State Act payments and Federal LHWCA awards from the CSV.  
* Conduct a comparative analysis of the claimant's "category-by-category" argument versus the federal aggregate credit doctrine under LHWCA Section 3(e).  
* Evaluate the mathematical scenario to determine the total employer credit.  
* Formulate an actionable recommendation detailing the exact dollar amount (if any) the employer still owes the claimant under the LHWCA award.

**Attached Files (Context):**

concurrent\_claim\_ledger.csv

Code snippet

Benefit\_Category,State\_Act\_Amount\_Paid,Federal\_LHWCA\_Amount\_Awarded  
Medical\_Expenses,$50000,$60000  
Temporary\_Total\_Disability\_(TTD),$30000,$40000  
Permanent\_Partial\_Disability\_(PPD),$20000,$0  
Total,$100000,$100000

LHWCA Section 3(e) Reference: https://www.dol.gov/agencies/oalj/PUBLIC/LONGSHORE/REFERENCES/REFERENCE\_WORKS/2002\_LHWCA\_TOPIC\_3

**3\. Rubric (Negative Failure Focus)**

| \# | Description | Weight | Sources (with exact PDF pages) | Justification | Met | Failure Reasoning (Model Failure Example) | Dependent Criteria |
| :---- | :---- | :---- | :---- | :---- | :---- | :---- | :---- |
| 1 | Extracts data from the CSV. | Minor | concurrent\_claim\_ledger.csv | Must identify that the State paid $100,000 in total across three categories, and the Federal ALJ awarded $100,000 across two categories. | FALSE | Failed to pull the correct values from the raw CSV data. | None |
| 2 | Analyzes LHWCA Section 3(e) credit rules. | Critical | LHWCA Reference URL | Must state that Section 3(e) provides a credit against any liability imposed by the LHWCA for amounts paid under a state workers' compensation law. | FALSE | **Model Failure:** Failed to identify the federal statutory offset provision entirely. | 1 |
| 3 | Evaluates the "Category-by-Category" fallacy. | Critical | Federal Maritime Jurisprudence | Must explicitly reject the claimant's argument. Case law dictates that credit for LHWCA benefits must be calculated based on a comparison of the total benefit payments and awards under each act, regardless of category. | FALSE | **Model Failure:** Sided with the claimant's attorney, applying a line-item offset that ignores the $20,000 state PPD overpayment. | 2 |
| 4 | Calculates the aggregate credit. | Major | Prompt Text | Must calculate that the $100,000 total State payment fully offsets the $100,000 total Federal award. | FALSE | **Model Failure:** Calculated a $10,000 TTD deficit and a $10,000 Medical deficit by isolating the categories. | 3 |
| 5 | Provides an actionable $0 payout recommendation. | Critical | Prompt Text | Must recommend a $0 net payout to the claimant, as the federal award is entirely satisfied by the aggregate state credit. | FALSE | **Model Failure:** Directed the employer to pay the claimant an additional $20,000 to satisfy the "deficits" in the Medical and TTD buckets. | 4 |

**4\. Model Analysis**

This prompt utilizes a highly effective mathematical and legal "category-matching" trap. When presented with a CSV showing discrepancies in specific benefit buckets (e.g., Federal TTD is $10k higher than State TTD), an untuned LLM will naturally isolate the rows and calculate the differences, determining that the employer owes the claimant the $20k shortfall in the Medical and TTD categories. The model will assume that the $20k State PPD payment is irrelevant because the federal judge awarded $0 for PPD. However, under LHWCA Section 3(e) and established Supreme Court precedent, credits for concurrent state and federal payments are calculated strictly on an *aggregate* basis, regardless of the specific category of disability. Because the total state payment ($100k) equals the total federal award ($100k), the employer's liability is entirely extinguished. The model must override its basic line-by-line spreadsheet logic and apply the rigid aggregate statutory rule.

**5\. Golden Response**

LEGAL ANALYSIS MEMO

TO: Claims Adjudication Unit

FROM: Federal LHWCA Claims Examiner

DATE: March 1, 2026

SUBJECT: Concurrent Jurisdiction – LHWCA Section 3(e) Credit Calculation

* CSV Data Extraction & Claim Overview  
  According to the concurrent\_claim\_ledger.csv, the employer has already paid the claimant a total of $100,000 under the State Workers' Compensation Act, consisting of $50,000 for Medical, $30,000 for Temporary Total Disability (TTD), and $20,000 for Permanent Partial Disability (PPD). Concurrently, the Federal ALJ awarded a total of $100,000 under the LHWCA, broken down as $60,000 for Medical, $40,000 for TTD, and $0 for PPD.  
* Comparative Analysis: Category-by-Category vs. Aggregate Credit  
  The claimant's attorney argues that the employer owes an additional $10,000 for TTD and $10,000 for Medical because the federal awards in those specific categories exceeded the state payments. This "category-by-category" argument is legally invalid. Section 3(e) of the LHWCA provides that any amounts paid to an employee pursuant to any other workers' compensation law shall be credited against any liability imposed by the LHWCA. Jurisprudence overwhelmingly holds that the credit for LHWCA disability benefits must be calculated based upon a comparison of the total disability benefit payments and awards under each act, regardless of category.  
* Scenario Evaluation and Quantum Calculation  
  By applying the aggregate credit doctrine, the employer is entitled to pool the entire $100,000 paid under the state system—including the $20,000 paid for a PPD rating that the federal judge ultimately rejected—and apply it against the gross federal liability.  
* Total Federal LHWCA Liability: $100,000  
* Less Total Section 3(e) State Credit: \-$100,000  
* Net Amount Due to Claimant: $0  
* Actionable Recommendation  
  I recommend that we issue a formal denial of the claimant's demand for additional funds. Under LHWCA Section 3(e), the employer receives a full, dollar-for-dollar aggregate credit for all state payments. Because the $100,000 state payment completely offsets the $100,000 federal award, the employer's liability is fully extinguished, and no further payments will be issued.

### ---

**Example 32: Commercial Property (ISO Builders Risk "Permission to Occupy" Trap)**

**1\. Metadata**

* Task Type: Workflow  
* Category / Domain: Commercial Property / Construction  
* Workflow: Coverage Determination  
* Prompt Type: Claims Strategy Memo  
* Difficulty: Super Hard (Expert)

**2\. Prompt**

You are a Commercial Property General Adjuster. The current date is February 20, 2026\.

Your insured is a real estate developer constructing a 10-story mixed-use high-rise. The project is insured under a standard ISO CP 00 20 Builders Risk Coverage Form with a $20,000,000 limit.

On February 15, 2026, a severe electrical fire started on the 8th floor, destroying the top three unfinished floors. The estimated cost to repair the fire damage is $4,500,000.

During your claims investigation, you extracted the project timeline from the insured's project management software (attached as project\_milestones.csv). The CSV reveals that on February 1, 2026, the developer allowed a commercial coffee shop tenant to move into the fully completed ground-floor retail space and open for business to generate early revenue. The insured did not inform your company of this, nor did they receive written consent from the insurer to occupy the building. The fire on the 8th floor was entirely unrelated to the coffee shop's operations on the 1st floor.

Review the attached CSV and the ISO Builders Risk guidelines. Draft a Claims Strategy Memo. You must:

* Extract the date of first occupancy and the date of loss from the CSV.  
* Conduct a comparative analysis of the ISO CP 00 20 "When Coverage Ceases" provisions, specifically evaluating the "Permission to Occupy" requirement.  
* Evaluate the scenario to determine if the unrelated nature of the fire preserves coverage for the $4,500,000 loss on the 8th floor.  
* Formulate an actionable recommendation on whether to accept or deny the $4,500,000 fire damage claim.

**Attached Files (Context):**

project\_milestones.csv

Code snippet

Phase\_ID,Description,Location,Completion\_Date,Status  
PH-01,Foundation Pour,Site-Wide,2025-04-10,Complete  
PH-02,Ground Floor Retail Buildout,1st Floor,2026-01-25,Complete  
PH-03,Tenant Move-In / Occupancy,1st Floor,2026-02-01,Active  
PH-04,Fire Incident (Loss),8th Floor,2026-02-15,Catastrophic Loss

Builders Risk Guide URL: https://www.icsc.com/uploads/education/GS4\_Recovering\_the\_Whole\_Enchilada\_Materials.pdf

**3\. Rubric (Negative Failure Focus)**

| \# | Description | Weight | Sources (with exact PDF pages) | Justification | Met | Failure Reasoning (Model Failure Example) | Dependent Criteria |
| :---- | :---- | :---- | :---- | :---- | :---- | :---- | :---- |
| 1 | Extracts timeline data from CSV. | Minor | project\_milestones.csv, Rows 3-4 | Must note that the 1st Floor was occupied on February 1, 2026, which is 14 days prior to the February 15, 2026 fire loss. | FALSE | Failed to establish the chronological sequence of events. | None |
| 2 | Analyzes the Occupancy termination clause. | Critical | Builders Risk Guide URL, Page 37 | Must state that under the ISO CP 00 20 Builders Risk Coverage Form, insurance coverage ends automatically if the covered building is occupied in whole or in part without the insurer's prior written consent. | FALSE | **Model Failure:** Completely ignored the occupancy clause or assumed Builders Risk policies last until the final project end date regardless of tenant move-ins. | 1 |
| 3 | Evaluates the "unrelated causation" fallacy. | Critical | Insurance Contract Law | Must recognize that the occupancy clause acts as a strict condition subsequent terminating the entire policy. The fact that the fire on the 8th floor was unrelated to the 1st-floor occupancy is legally irrelevant. | FALSE | **Model Failure:** Paid the $4.5M claim by reasoning that the coffee shop occupancy on the 1st floor had no causal connection to the fire on the 8th floor. | 2 |
| 4 | Provides an actionable denial recommendation. | Major | Prompt Text | Must recommend a total denial of the $4,500,000 claim because the policy had already ceased to exist prior to the fire. | FALSE | **Model Failure:** Recommended paying the claim, or recommended only denying coverage for the 1st floor while covering the 8th floor. | 3 |

**4\. Model Analysis** This prompt is an aggressive test of an LLM's ability to resist the "causation fallacy" when interpreting strict policy conditions. Because the prompt explicitly states that the 1st-floor occupancy was "entirely unrelated" to the 8th-floor fire, the LLM's natural logic will dictate that it is unfair to deny the massive $4.5M claim. An untuned model will typically rule that the lack of causal connection preserves coverage. However, the model must extract the timeline from the CSV and apply the strict contractual forfeiture mechanism found in the ISO CP 00 20 form. Under the Builders Risk Coverage Forms, coverage will end if the covered building or structure is occupied in whole or in part without the insurer's prior written consent. Once the tenant occupied the 1st floor on February 1 without a "Permission to Occupy" endorsement, the entire policy immediately ceased. Because the policy was dead 14 days before the fire occurred, the insurer owes nothing. The expert model must ruthlessly enforce the termination clause.

**5\. Golden Response**

CLAIMS STRATEGY MEMO

TO: Commercial Property Claims Unit

FROM: Commercial Property General Adjuster

DATE: February 20, 2026

SUBJECT: Builders Risk Coverage Determination – Unauthorized Occupancy

* CSV Data Extraction & Timeline Overview  
  A review of the insured's project\_milestones.csv establishes a critical timeline regarding this claim. On February 1, 2026, Phase PH-03 commenced, indicating that a commercial tenant moved into the 1st Floor and took active occupancy of the premises. Exactly 14 days later, on February 15, 2026, the catastrophic fire loss (PH-04) occurred on the 8th floor.  
* Comparative Analysis: ISO CP 00 20 "When Coverage Ceases" Builders Risk policies are strictly designed to cover properties during the course of construction, as the risk profile of an active construction site is vastly different from an occupied commercial building. Under the ISO CP 00 20 Builders Risk Coverage Form, the insurance provided by the policy will end automatically when one of several triggering events occurs. Specifically, coverage will end if the covered building or structure is occupied in whole or in part without the insurer's prior written consent. To avoid this, an insured must obtain a "Permission to Occupy" endorsement from the underwriter prior to allowing any tenants into the building.

* Scenario Evaluation: The Causation Fallacy  
  The insured argues that the fire on the 8th floor was entirely unrelated to the coffee shop's occupancy on the 1st floor, implying that coverage should remain intact for the upper floors. This relies on a causation fallacy. The occupancy restriction in the Builders Risk form is not an exclusion that requires a causal link to the damage; it is a strict condition subsequent that defines the absolute lifespan of the policy. When the insured allowed the tenant to occupy the 1st floor on February 1 without our written consent, the entire Builders Risk policy automatically ceased to exist.  
* Actionable Recommendation  
  I recommend issuing a total denial of coverage for the $4,500,000 fire damage claim. Because the insured violated the occupancy provision on February 1, 2026, the ISO CP 00 20 coverage form terminated on that date. There was no active insurance policy in force on February 15, 2026, when the fire occurred. We have no liability for this loss.

### **Example 33: Marine Cargo General Average (York-Antwerp Rules Floating-Point & Afloat Trap)**

**1\. Metadata**

* Task Type: Workflow  
* Category / Domain: Marine Insurance / Admiralty Law  
* Workflow: Claims Adjudication & Quantum Calculation  
* Prompt Type: General Average Assessment  
* Difficulty: Super Hard (Expert)

**2\. Prompt**

You are a Marine Average Adjuster. The current date is February 28, 2026\.

A commercial cargo vessel was caught in a severe typhoon. To save the ship and cargo from sinking, the Master intentionally ran the ship on shore (a voluntary stranding).

While ashore and in a position of extreme peril, the Master ordered the engineering crew to overwork the engines to refloat the ship, causing significant damage to the main engine. The ship successfully refloated. However, while afloat in the open ocean, the storm intensified again. To outrun the storm and safely reach the port of refuge for the common safety of the maritime adventure, the Master intentionally overworked the propelling machinery and boilers, causing catastrophic damage to the boilers.

The shipowner has declared General Average (GA) under the York-Antwerp Rules 2016 for the machinery damages and is seeking a proportional contribution from the cargo owners.

Review the attached voyage\_ga\_assessments.csv and the York-Antwerp Rules 2016\. Draft a General Average Assessment for your client, **Apex Logistics**.

You must:

* Extract the exact floating-point financial values for Voyage **V-994** from the CSV. Be highly precise to avoid the decoy voyages.  
* Conduct a comparative analysis of the engine damage under Rule VII versus the boiler damage under Rule VII.  
* Calculate your client's exact proportional liability percentage (Cargo Value divided by Total Contributory Value).  
* Evaluate the mathematical scenario to determine the exact dollar amount of the allowable GA pool, strictly omitting any non-allowable sacrifices.  
* Formulate an actionable recommendation detailing the final, exact dollar amount Apex Logistics must contribute to the General Average fund.

**Attached Files (Context):**

voyage\_ga\_assessments.csv

Code snippet

Voyage\_ID,Vessel\_Name,Cargo\_Owner,Cargo\_Value,Total\_Contributory\_Value,Ashore\_Machinery\_Dmg,Afloat\_Boiler\_Dmg  
V-993,Oceanic Prince,Apex Logistics,$3456789.50,$25456789.50,$452333.40,$789999.85  
V-994,Oceanic Queen,Apex Logistics,$3456789.50,$34567895.00,$452333.40,$789999.85  
V-995,Oceanic King,Zenith Cargo,$3456789.50,$34567895.00,$422333.40,$719999.85

York-Antwerp Rules 2016 PDF: https://comitemaritime.org/wp-content/uploads/2023/01/YAR-2016-English-Version.pdf

**3\. Rubric**

| \# | Description | Weight | Sources (with exact PDF pages) | Justification | Met | Failure Reasoning | Dependent Criteria |
| :---- | :---- | :---- | :---- | :---- | :---- | :---- | :---- |
| 1 | Extracts data for the correct voyage from the CSV. | Critical | voyage\_ga\_assessments.csv, Row 3 | Must bypass the decoy V-993 and V-995 rows and extract the exact floats for V-994 (Cargo: $3,456,789.50, Total: $34,567,895.00). | FALSE | **Model Failure (Extraction):** Extracted V-993 because it matched the client name, missing the strict Voyage ID requirement. | None |
| 2 | Calculates the proportional liability percentage. | Major | Prompt Text | Must divide $3,456,789.50 by $34,567,895.00 to arrive at exactly 10%. | FALSE | **Model Failure (Math):** Calculated the float incorrectly or used the decoy V-993 total value. | 1 |
| 3 | Analyzes Rule VII for the Ashore Machinery Damage. | Major | YAR 2016 PDF, Page 12 | Must state that machinery damage ($452,333.40) while *ashore* attempting to refloat is admitted as GA. | FALSE | Failed to identify the geographic status of the vessel during the first engine damage. | None |
| 4 | Analyzes Rule VII for the Afloat Boiler Damage. | Critical | YAR 2016 PDF, Page 12 | Must state that Rule VII absolutely prohibits GA allowance for boiler damage ($789,999.85) while the ship is *afloat*. | FALSE | **Model Failure (Logic):** Approved the afloat damage because the Master sacrificed it for the "common safety," ignoring the strict treaty exception. | None |
| 5 | Calculates the total admitted GA amount. | Critical | Prompt Text | Must designate the admitted GA pool as exactly $452,333.40, expressly omitting the $789,999.85. | FALSE | **Model Failure (Math):** Combined both damages into a pool of $1,242,333.25. | 3, 4 |
| 6 | Calculates the final client contribution. | Critical | Prompt Text | Must multiply the 10% liability by the $452,333.40 allowable pool to arrive at a final contribution of exactly $45,233.34. | FALSE | **Model Failure (Final):** Ordered the client to pay $124,233.32 (10% of the combined wrong pool). | 2, 5 |

**4\. Model Analysis**

This prompt is a highly tuned extraction and logic trap. The CSV is littered with identical floating-point numbers across different columns and rows. If the LLM anchors to the first row containing "Apex Logistics", it will use a Total\_Contributory\_Value of $25,456,789.50, which destroys the 10% math ratio. Furthermore, the scenario describes a textbook GA act (intentional sacrifice for common safety). Untuned models will apply general maritime philosophy, add both the afloat and ashore damages together ($1,242,333.25), and charge the client 10% of the total ($124,233.32). The model must suppress its general knowledge, strictly apply the geographic condition (ashore vs. afloat) embedded in YAR 2016 Rule VII to strip out the $789,999.85 loss, and execute the floating-point math flawlessly to output $45,233.34.

**5\. Golden Response**

GENERAL AVERAGE ASSESSMENT

TO: Apex Logistics (Cargo Owner)

FROM: Marine Average Adjuster

DATE: February 28, 2026

SUBJECT: General Average Claim – Voyage V-994

* CSV Data Extraction & Proportional Liability  
  Based on the voyage\_ga\_assessments.csv, we have isolated the financial variables for your shipment on Voyage V-994 aboard the Oceanic Queen:  
* Apex Logistics Cargo Value: $3,456,789.50  
* Total Contributory Value: $34,567,895.00  
  By dividing your cargo value by the total contributory value, your proportional liability for any allowable General Average (GA) is exactly 10.0%.  
* Scenario Evaluation: Rule VII (Damage Ashore)  
  The shipowner is claiming $452,333.40 in damage to the main engine that occurred while the vessel was ashore and attempting to refloat. Under Rule VII of the York-Antwerp Rules 2016, damage caused to any machinery of a ship which is ashore and in a position of peril, in endeavouring to refloat, shall be allowed in general average. This $452,333.40 amount is fully admitted to the GA pool.  
* Scenario Evaluation: Rule VII (Damage Afloat)  
  The shipowner is also claiming $789,999.85 in damage to the boilers. Although the Master intentionally overworked the boilers for the common safety to outrun the storm, the vessel was no longer ashore. Rule VII contains an absolute prohibition for this specific circumstance, stating that where a ship is afloat no loss or damage caused by working the propelling machinery and boilers shall in any circumstances be allowed as general average. Therefore, the $789,999.85 claim must be strictly excluded from the adjustment.  
* Actionable Recommendation and Final Calculation  
  I recommend a partial admission of the shipowner's claim. The total admitted General Average pool is exactly $452,333.40. Apex Logistics bears zero liability for the $789,999.85 afloat boiler destruction.  
  Applying your 10% proportional liability to the $452,333.40 allowable pool, your final required GA contribution is exactly **$45,233.34**.

### ---

**Example 34: Commercial Property (ISO CP 00 10 Debris Removal Floating-Point Formula Trap)**

**1\. Metadata**

* Task Type: Workflow  
* Category / Domain: Commercial Property Claims / Actuarial  
* Workflow: Quantum Calculation  
* Prompt Type: Claims Strategy Memo  
* Difficulty: Super Hard (Expert)

**2\. Prompt**

You are a Commercial Property General Adjuster. The current date is March 15, 2026\.

Your insured owns a portfolio of commercial buildings insured under an ISO CP 00 10 10 12 Building and Personal Property Coverage Form. A recent tornado caused severe direct physical damage and scattered massive amounts of debris across one of their properties.

The insured has submitted a complex claim for the property located at **100 South Main St**. You must calculate the exact payout for the Debris Removal Additional Coverage.

Review the attached property\_loss\_schedule.csv. Draft a Claims Strategy Memo. You must:

* Extract the exact Building Limit, Deductible, Covered Direct Loss, and Actual Debris Cost for the specific target location (100 South Main St) from the CSV. Be extremely careful to avoid decoy rows.  
* Conduct a comparative analysis of the ISO CP 00 10 10 12 Debris Removal formula, defining the base 25% calculation and the conditions for the "Additional $25,000" limit.  
* Execute the exact mathematical calculation required to determine the base debris removal limit, demonstrating the addition of the deductible to the paid direct loss *before* applying the 25% factor.  
* Calculate the final, exact total claim payout (Direct Loss Paid \+ Debris Removal Paid) and formulate an actionable recommendation for the claims department.

**Attached Files (Context):**

property\_loss\_schedule.csv

Code snippet

Loc\_ID,Address,Bldg\_Limit,Deductible,Covered\_Direct\_Loss,Actual\_Debris\_Cost  
01,100 North Main St,$2875500.00,$15550.50,$1556888.40,$435677.95  
02,100 South Main St,$2875500.00,$14250.75,$1556888.40,$435677.95  
03,100 South Main Ave,$3000500.00,$14250.75,$1556888.40,$350000.00

ISO CP 00 10 10 12 Reference: https://www.propertyinsurancecoveragelaw.com/wp-content/uploads/2023/12/CP-00-10-10-12-Building-and-Personal-Property-Coverage-Form.pdf

**3\. Rubric**

| \# | Description | Weight | Sources (with exact PDF pages) | Justification | Met | Failure Reasoning (Model Failure Example) | Dependent Criteria |
| :---- | :---- | :---- | :---- | :---- | :---- | :---- | :---- |
| 1 | Extracts data for the correct location from the CSV. | Critical | property\_loss\_schedule.csv, Row 3 | Must bypass the "North Main" and "Main Ave" decoys and pull Loc 02: Deductible $14,250.75, Direct Loss $1,556,888.40, Debris $435,677.95. | FALSE | **Model Failure (Extraction):** Extracted Loc 01 or Loc 03 due to keyword similarity, pulling the wrong deductible. | None |
| 2 | Calculates the "Amount Paid" for Direct Loss. | Major | Prompt Text | Must subtract the deductible from the covered direct loss ($1,556,888.40 \- $14,250.75 \= $1,542,637.65). | FALSE | **Model Failure (Math):** Failed to apply the deductible to the direct loss before proceeding. | 1 |
| 3 | Defines the Base 25% Debris Formula. | Critical | ISO CP 00 10 10 12 PDF, Page 3 | Must state that the base debris limit is 25% of the sum of the deductible *plus* the amount paid for direct physical loss. | FALSE | **Model Failure (Logic):** Hallucinated that the limit is 25% of the Building Limit, or 25% of the gross loss without factoring the deductible. | None |
| 4 | Calculates the Base Debris Limit. | Critical | Prompt Text | Must calculate 25% of ($1,542,637.65 \+ $14,250.75) \= 25% of $1,556,888.40 \= exactly $389,222.10. | FALSE | **Model Failure (Math):** Calculated the float incorrectly, or forgot to add the deductible back into the multiplier. | 2, 3 |
| 5 | Applies the Additional $25,000 Limit. | Critical | ISO CP 00 10 10 12 PDF, Page 4 | Must recognize the actual debris cost ($435,677.95) exceeds the base limit ($389,222.10), triggering the additional $25,000 limit. Total debris payable \= $414,222.10. | FALSE | **Model Failure (Synthesis):** Approved the full $435,677.95 because it fell under the $2.87M building limit, ignoring the sub-limit structure. | 4 |
| 6 | Calculates the Final Total Payout. | Major | Prompt Text | Must sum the Direct Paid ($1,542,637.65) \+ Debris Paid ($414,222.10) to arrive at exactly $1,956,859.75. | FALSE | Miscalculated the final combined check amount. | 2, 5 |

**4\. Model Analysis**

This prompt guarantees a \>30% failure rate on the rubric for standard models by compounding extraction decoys with a highly counter-intuitive algebraic formula. The CSV contains three rows with nearly identical street names and the exact same gross loss/debris figures, but different deductibles. If the model's extraction attention wavers, it will pull the $15,550.50 deductible from Loc 01, instantly failing the rest of the math. Furthermore, the ISO CP 00 10 Debris Removal formula is legally complex. The model must first determine the amount *paid* for direct loss (Loss minus Deductible), and then the debris limit is 25% of the (Amount Paid *plus* the Deductible). Untuned models will take 25% of the Building Limit ($718,875.00) and simply pay the entire $435k debris bill, completely missing the sub-limit trap. A successful model must extract the exact float values, execute the multi-step algebraic formula flawlessly, cap the debris at the base limit plus $25,000, and output the exact combined settlement of $1,956,859.75.

**5\. Golden Response**

CLAIMS STRATEGY MEMO

TO: Commercial Property Claims Unit

FROM: Commercial Property General Adjuster

DATE: March 15, 2026

SUBJECT: Debris Removal Calculation – 100 South Main St

* CSV Data Extraction  
  To accurately calculate this claim, we must isolate the specific data for the target location at 100 South Main St. Reviewing the property\_loss\_schedule.csv, we must bypass the "North Main" and "Main Ave" decoys. The extracted data for Loc\_ID 02 (100 South Main St) is:  
* Building Limit: $2,875,500.00  
* Deductible: $14,250.75  
* Covered Direct Loss: $1,556,888.40  
* Actual Debris Cost: $435,677.95  
* Comparative Analysis: ISO CP 00 10 10 12 Debris Removal Formula  
  Under the ISO CP 00 10 10 12 Building and Personal Property Coverage Form, Debris Removal is provided as an Additional Coverage. It is not simply paid up to the building limit. The policy dictates that the amount we will pay for debris removal expense is limited to 25% of the sum of the deductible plus the amount that we pay for direct physical loss.  
  If the actual debris removal expense exceeds this 25% base limitation, the policy provides an additional, hard-capped limit of $25,000 for debris removal expense at each location.  
* Mathematical Calculation of the Debris Limit  
  We must execute the calculation sequentially based on the extracted floating-point values:  
* **Step A (Determine Direct Amount Paid):** Covered Direct Loss ($1,556,888.40) \- Deductible ($14,250.75) \= **$1,542,637.65**.  
* **Step B (Calculate Base Debris Limit):** 25% of the sum of (Amount Paid \+ Deductible).  
  * $1,542,637.65 \+ $14,250.75 \= $1,556,888.40.  
  * 25% of $1,556,888.40 \= **$389,222.10**.  
* **Step C (Apply Additional Limit):** The Actual Debris Cost is $435,677.95, which exceeds the Base Debris Limit of $389,222.10. Therefore, the additional $25,000 coverage is triggered.  
  * Maximum Debris Payable \= $389,222.10 \+ $25,000.00 \= **$414,222.10**.  
* *Note: The remaining $21,455.85 in debris costs is uninsured and must be borne by the insured.*  
* Final Total Claim Payout and Actionable Recommendation  
  To determine the final check amount, we combine the payable direct loss and the payable debris removal:  
* Direct Loss Paid: $1,542,637.65  
* Debris Removal Paid: $414,222.10  
* **Total Claim Payout: $1,956,859.75**

I recommend issuing a formal settlement to the insured in the exact amount of **$1,956,859.75**. The claim file should clearly document the application of the $14,250.75 deductible and the strict application of the 25% plus $25,000 debris removal caps as outlined above.

# 

# 

# **Advanced Evaluation Frameworks** 

---

**Example 1: Ocean Marine General Average (York-Antwerp Rules 2016\)**

#### **1\. Metadata**

* **Category / Domain:** Claims Handling  
* **Difficulty:** High  
* **Themes:** General Average, Voluntary Stranding, Cargo Sacrifices, Cost Offsets  
* **Task Type:** Workflow  
* **Workflow:** Quantum Calculation  
* **Prompt Type:** General Average Adjustment Memo

#### **2\. Prompt**

You are a Marine Average Adjuster. The current date is November 10, 2023\.

A commercial vessel encountered a severe storm. To prevent the vessel from sinking and to save the voyage, the captain intentionally ran the ship on shore (voluntary stranding). While stranded, the ship’s generators ran out of diesel. To keep the bilge pumps running and maintain the common safety of the maritime adventure, the crew was forced to burn a portion of the vessel's cargo (lumber) as fuel.

You must calculate the total amount allowed in General Average for this incident based *only* on the rules provided in the attached York-Antwerp Rules 2016\. Present your calculations and rationale in a formal Average Adjustment Memo.

**Loss Details:**

* Direct damage to the ship's hull caused by the intentional stranding: $500,000  
* Value of the lumber cargo burned for fuel: $100,000  
* The estimated cost of the diesel fuel that *would* have otherwise been consumed to run the generators during that period: $20,000

**Attached Files (Context):**

* **2016-York-Antwerp-Rules.pdf** – Public URL: https://comitemaritime.org/wp-content/uploads/2018/06/2016-York-Antwerp-Rules-with-Rule-XVII-correction.pdf

#### **3\. Rubric**

| \# | Description | Weight | Sources (with public URLs) | Justification | Met | Failure Reasoning | Dependent Criteria |
| :---- | :---- | :---- | :---- | :---- | :---- | :---- | :---- |
| 1 | Identifies that Rule V governs Voluntary Stranding. | Minor | 2016-York-Antwerp-Rules.pdf | Locates the specific rule regarding intentional grounding. | FALSE | Hallucinated a different rule or applied common law. |  |
| 2 | Confirms the $500,000 hull damage is allowed in General Average. | Critical | 2016-York-Antwerp-Rules.pdf | Rule V states that when a ship is intentionally run on shore for common safety, the consequent damage is allowed in GA. | FALSE | Denied the hull damage stating it was an intentional act. | 1 |
| 3 | Identifies that Rule IX governs Cargo used for Fuel. | Minor | 2016-York-Antwerp-Rules.pdf | Locates the specific rule regarding burning cargo. | FALSE | Failed to locate Rule IX. |  |
| 4 | Confirms the $100,000 cargo loss is eligible for General Average. | Major | 2016-York-Antwerp-Rules.pdf | Rule IX allows cargo necessarily used for fuel for common safety to be admitted to GA. | FALSE | Excluded the cargo loss. | 2 |
| 5 | Identifies the offset requirement in Rule IX for estimated fuel costs. | Critical | 2016-York-Antwerp-Rules.pdf | Rule IX dictates the GA must be credited with the estimated cost of the fuel which would otherwise have been consumed. | FALSE | Missed the credit offset provision. | 2 |
| 6 | Subtracts the $20,000 estimated fuel cost from the $100,000 cargo loss. | Major | Prompt Text | $100,000 \- $20,000 \= $80,000. | FALSE | Failed to execute the math for the offset. | 3 |
| 7 | Calculates the final allowed Cargo General Average as $80,000. | Major | Prompt Text | The net amount allowed after the Rule IX credit. | FALSE | Allowed the full $100,000. | 5 |
| 8 | Calculates the Total Allowed General Average as $580,000. | Critical | Prompt Text | $500,000 (Hull) \+ $80,000 (Net Cargo) \= $580,000. | FALSE | Incorrect final summation. | 6 |
| 9 | Formats the output as a formal Average Adjustment Memo. | Minor | Prompt Text | Required styling. | FALSE | Provided an unstructured list. |  |
| 10 | Includes the date November 10, 2023\. | Minor | Prompt Text | Temporal anchor. | FALSE | Omitted the date. |  |

#### **4\. Model Analysis**

Models frequently fail Marine General Average calculations because they struggle with the concept of "credit offsets" in 19th-century maritime language. A standard model will easily identify that both the intentional stranding (Rule V) and the burning of cargo (Rule IX) are allowed in General Average, leading it to add $500,000 and $100,000 for a total of $600,000. The model will miss the trailing clause in Rule IX, which dictates that the GA pool must be *credited* (reduced) by the cost of the fuel the ship *would* have used anyway. This requires advanced syntactic comprehension to translate "credited with the estimated cost" into a mathematical subtraction step.

#### **5\. Golden Response**

**AVERAGE ADJUSTMENT MEMORANDUM**

**TO:** Maritime Claims File

**FROM:** Marine Average Adjuster

**DATE:** November 10, 2023

**SUBJECT:** General Average Adjustment – Voluntary Stranding and Cargo Sacrifice

**1\. Incident Overview**

The vessel was intentionally run aground to secure the common safety of the maritime adventure during a severe storm. Subsequently, the crew sacrificed $100,000 of lumber cargo to fuel the generators.

**2\. Rule Application & Quantum Calculation**

This adjustment is governed by the York-Antwerp Rules 2016\.

* **Hull Damage (Rule V \- Voluntary Stranding):**  
  Rule V dictates that when a ship is intentionally run on shore for the common safety, the consequent damage to the property is allowed in general average.  
  *Allowed Amount: $500,000*  
* **Cargo Sacrifice (Rule IX \- Cargo Used for Fuel):**  
  Rule IX dictates that cargo necessarily used for fuel for the common safety shall be allowed as general average. However, the rule strictly requires that the general average be credited with the estimated cost of the fuel which would otherwise have been consumed.  
  *Base Cargo Sacrifice:* $100,000  
  *Less Estimated Fuel Cost Credit:* ($20,000)  
  *Allowed Amount: $80,000*

**3\. Final General Average Allowance**

The total amount allowed in General Average for this incident is **$580,000** ($500,000 Hull \+ $80,000 Cargo).

### ---

**Example 2: Builders Risk Soft Costs (Delayed Opening)**

#### **1\. Metadata**

* **Category / Domain:** Claims Handling  
* **Difficulty:** Medium-High  
* **Themes:** Builders Risk, Soft Costs, Enumerated Perils, Contractor Penalties  
* **Task Type:** Workflow  
* **Workflow:** Quantum Calculation  
* **Prompt Type:** Claims Apportionment Memo

#### **2\. Prompt**

You are a Commercial Claims Adjuster. The current date is August 14, 2023\.

A commercial building under construction suffered a massive fire (a covered cause of loss). Due to the fire, the completion date of the project was delayed by two months. The insured developer has submitted a claim for "Soft Costs" arising out of this delay.

Review the attached Soft Costs Endorsement. Determine which of the insured's claimed expenses are eligible for reimbursement, apply the deductible, and calculate the final payable amount.

**Claimed Soft Cost Expenses:**

* Additional interest incurred on the construction loan: $30,000  
* Additional leasing and marketing expenses due to the delay: $20,000  
* Financial penalties levied against the developer for missing the contractual completion deadline: $40,000  
* **Soft Costs Deductible:** $5,000

**Attached Files (Context):**

* **Soft-Costs-Endorsement-NW-SCE-12-1998.pdf** – Public URL: https://heartlandmutualinsurance.com/wp-content/uploads/2023/07/Soft-Costs-Endorsement-NW-SCE-12-1998.pdf

#### **3\. Rubric**

| \# | Description | Weight | Sources (with public URLs) | Justification | Met | Failure Reasoning | Dependent Criteria |
| :---- | :---- | :---- | :---- | :---- | :---- | :---- | :---- |
| 1 | Identifies that the endorsement limits coverage to 6 specifically enumerated items. | Minor | Soft-Costs-Endorsement-NW-SCE-12-1998.pdf | The policy states "These 'Soft Costs' shall be limited to:..." | FALSE | Assumed all delay expenses were covered. |  |
| 2 | Approves the $30,000 for Additional interest on the construction loan. | Major | Soft-Costs-Endorsement-NW-SCE-12-1998.pdf | Matches enumerated Item \#2. | FALSE | Denied the interest expense. |  |
| 3 | Approves the $20,000 for Leasing and Marketing expenses. | Major | Soft-Costs-Endorsement-NW-SCE-12-1998.pdf | Matches enumerated Item \#3. | FALSE | Denied the marketing expense. |  |
| 4 | Denies the $40,000 for contractual completion penalties. | Critical | Soft-Costs-Endorsement-NW-SCE-12-1998.pdf | Contractual penalties are not listed among the 6 covered items. | FALSE | Approved the penalty payment. | 1 |
| 5 | Calculates the gross eligible soft costs as $50,000. | Major | Prompt Text | $30,000 \+ $20,000 \= $50,000. | FALSE | Math error or inclusion of the penalty. | 6 |
| 6 | Subtracts the $5,000 deductible. | Major | Prompt Text | Standard deductible application. | FALSE | Failed to apply the deductible. |  |
| 7 | Calculates the final payable amount as $45,000. | Critical | Prompt Text | $50,000 \- $5,000 \= $45,000. | FALSE | Incorrect final quantum. | 4 |
| 8 | Explains that the coverage relies on direct physical damage to the project. | Minor | Soft-Costs-Endorsement-NW-SCE-12-1998.pdf | Contextualizes the trigger for soft costs. | FALSE | Omitted the trigger requirement. |  |
| 9 | Formats the response as a Claims Apportionment Memo. | Minor | Prompt Text | Output styling. | FALSE | Wrote a casual email. |  |
| 10 | Includes the date August 14, 2023\. | Minor | Prompt Text | Temporal anchor. | FALSE | Omitted the date. |  |

#### **4\. Model Analysis**

Language models often fail at "closed list" policy interpretations. The attached Soft Costs endorsement explicitly limits coverage to six exact categories (Financial Costs, Additional Interest, Leasing/Marketing, Legal/Accounting, Misc Carrying Costs, Gross Rent). Because "financial penalties for delays" sounds like a logical soft cost to a human, an LLM will frequently assume it falls under "Financial Costs" or "Misc Carrying Costs" and approve the $40,000 penalty. A highly capable model must execute strict contractual construction, recognizing that punitive contractual penalties are not covered unless specifically enumerated, leading to the denial of the $40,000 and a final calculation of $45,000.

#### **5\. Golden Response**

**CLAIMS APPORTIONMENT MEMORANDUM**

**TO:** Builders Risk Claim File

**FROM:** Commercial Claims Adjuster

**DATE:** August 14, 2023

**SUBJECT:** Soft Costs Endorsement Evaluation – Delayed Opening

**1\. Coverage Trigger**

The project was delayed as a direct consequence of a covered peril (fire) causing direct physical damage to the construction project. Therefore, the Soft Costs Endorsement is triggered.

**2\. Evaluation of Claimed Expenses**

The Soft Costs Endorsement strictly limits coverage to six enumerated categories. We have evaluated the insured's demands against this closed list:

* **Additional interest on the construction loan ($30,000):** Covered. This aligns directly with enumerated Item \#2 (Additional Interest Expenses).  
* **Additional leasing and marketing expenses ($20,000):** Covered. This aligns directly with enumerated Item \#3 (Leasing and Marketing Expenses).  
* **Financial penalties for missing the contractual deadline ($40,000):** Denied. Contractual performance penalties are not included in the strictly limited list of six eligible soft costs under this endorsement.

**3\. Final Calculation**

* **Total Eligible Soft Costs:** $50,000  
* **Less Deductible:** ($5,000)  
* **Final Payable Amount:** **$45,000**

### ---

**Example 3: Aviation Hull Insurance \- Proportional Betterment**

#### **1\. Metadata**

* **Category / Domain:** Claims Handling  
* **Difficulty:** High  
* **Themes:** Aviation Hull, Proportional Deduction, Overhaul Life  
* **Task Type:** Workflow  
* **Workflow:** Quantum Calculation  
* **Prompt Type:** Loss Settlement Report

#### **2\. Prompt**

You are an Aviation Claims Adjuster. The current date is January 15, 2024\.

An insured aircraft suffered a severe accident upon landing. The aircraft is insured under the attached Aviation & Hull Package Policy. You have elected to pay for the repairs rather than declare a total loss. Calculate the final payout based on the policy's specific deduction rules for engine parts.

**Loss & Policy Details:**

* Agreed Value of Aircraft: $1,000,000  
* Standard Policy Deductible: $25,000  
* Cost to repair the fuselage: $300,000  
* Cost to repair the engine: $200,000  
* The damaged engine unit has a manufacturer Overhaul Life of 2,000 hours.  
* At the time of the crash, the engine had accumulated a "used time" of 1,500 hours since new.

Using Section I of the attached wording, apply the proportional deduction to the engine repair, calculate the net repair cost, apply the deductible, and state the final payable amount.

**Attached Files (Context):**

* **AVIATION\_and\_HULL\_PACKAGE\_POLICY\_Wording.pdf** – Public URL: https://content.sbigeneral.in/uploads/AVIATION\_and\_HULL\_PACKAGE\_POLICY\_Wording\_b3de868ec0.pdf

#### **3\. Rubric**

| \# | Description | Weight | Sources (with public URLs) | Justification | Met | Failure Reasoning | Dependent Criteria |
| :---- | :---- | :---- | :---- | :---- | :---- | :---- | :---- |
| 1 | Locates the "Amounts to be deducted from the claim" provision in Section I, 1(c). | Critical | AVIATION\_and\_HULL\_PACKAGE\_POLICY\_Wording.pdf | The clause governing overhaul deductions. | FALSE | Failed to find the deduction rule. |  |
| 2 | Extracts the rule requiring a deduction based on the proportion of used time to Overhaul Life. | Major | AVIATION\_and\_HULL\_PACKAGE\_POLICY\_Wording.pdf | Specifies the formula: Used Time / Overhaul Life. | FALSE | Ignored the betterment clause. | 1 |
| 3 | Calculates the proportional deduction fraction as 75% (or 0.75). | Major | Prompt Text | 1,500 hours / 2,000 hours \= 0.75. | FALSE | Calculated the fraction backwards. | 6 |
| 4 | Calculates the monetary deduction for the engine as $150,000. | Critical | Prompt Text | $200,000 (Engine Repair) \* 75% \= $150,000. | FALSE | Applied 75% to the entire $500k loss. | 2 |
| 5 | Calculates the net allowable engine repair cost as $50,000. | Major | Prompt Text | $200,000 \- $150,000 \= $50,000. | FALSE | Math error. | 3 |
| 6 | Calculates the total net repair cost before deductible as $350,000. | Major | Prompt Text | $300,000 (Fuselage) \+ $50,000 (Net Engine) \= $350,000. | FALSE | Forgot to add the fuselage back in. | 4 |
| 7 | Subtracts the $25,000 policy deductible. | Major | Prompt Text | Applies the deductible per Section I, 1(c)(i). | FALSE | Deducted from the wrong total. |  |
| 8 | Calculates the final payable amount as $325,000. | Critical | Prompt Text | $350,000 \- $25,000 \= $325,000. | FALSE | Incorrect final quantum. | 5 |
| 9 | Formats output as a Loss Settlement Report. | Minor | Prompt Text | Output styling. | FALSE | Provided an unstructured text block. |  |
| 10 | Includes the date January 15, 2024\. | Minor | Prompt Text | Temporal anchor. | FALSE | Omitted the date. |  |

#### **4\. Model Analysis**

Aviation hull policies contain unique "betterment" clauses designed to prevent insureds from getting a brand new engine out of an insurance claim when their old engine was near the end of its life. Section 1(c)(ii) requires the adjuster to deduct a proportion of the repair cost equal to the ratio of "used time" to "Overhaul Life". A standard LLM will usually fail to isolate this formula and apply it *only* to the engine. It will either ignore the flight hours entirely (paying the full $500,000 minus deductible) or erroneously apply the 75% deduction to the entire $500,000 loss (including the fuselage). Accurately bifurcating the claim, applying the fractional reduction solely to the engine, and then re-aggregating the total is a rigorous test of an LLM's logical parsing.

#### **5\. Golden Response**

**LOSS SETTLEMENT REPORT**

**DATE:** January 15, 2024

**SUBJECT:** Aviation Hull Final Payout Calculation

**1\. Policy Provisions**

The aircraft is insured under the Aviation & Hull Package Policy. Because the insurer has elected to pay for repairs, Section I, Paragraph 1(c) dictates the "Amounts to be deducted from the claim." Specifically, subsection (ii) requires a deduction for engine units based on "such proportion of the Overhaul Cost of any Unit repaired or replaced as the used time bears to the Overhaul Life of the Unit."

**2\. Engine Betterment Deduction**

* **Overhaul Life:** 2,000 hours  
* **Used Time:** 1,500 hours  
* **Proportional Deduction:** 1,500 / 2,000 \= 75%  
* **Gross Engine Repair Cost:** $200,000  
* **Less 75% Deduction:** ($150,000)  
* **Net Allowable Engine Repair:** $50,000

**3\. Final Payout Calculation**

The fuselage repair is not subject to the overhaul life deduction.

* **Fuselage Repair:** $300,000  
* **Net Engine Repair:** $50,000  
* **Total Allowable Repair:** $350,000  
* **Less Policy Deductible:** ($25,000)

**Final Payable Amount:** **$325,000**

### ---

**Example 4: Employment Practices Liability (Wage & Hour Defense)**

#### **1\. Metadata**

* **Category / Domain:** Claims Handling  
* **Difficulty:** High  
* **Themes:** EPLI, Defense Within Limits, Wage and Hour Sub-limits, Retentions  
* **Task Type:** Workflow  
* **Workflow:** Quantum Calculation  
* **Prompt Type:** Coverage Determination

#### **2\. Prompt**

You are a Management Liability Adjuster. The current date is May 10, 2023\.

Your insured was sued by employees for Wage and Hour violations (failure to pay overtime). The lawsuit settled. The insured incurred $150,000 in defense costs (attorney fees) and agreed to pay $50,000 in back-pay (damages) to the employees.

Using the attached ABAIS Employment Practices Liability (EPLI) specimen, calculate the exact amount the insurer will pay for this claim.

**Policy Details:**

* Overall Policy Limit: $1,000,000  
* Retention: $25,000  
* The Declarations page indicates that Endorsement **D 71706(7) "Cost of Defense Sub-Limit For \- Wage and Hour Violations"** is attached, with a sub-limit of $100,000.

*Note: You must apply the standard EPLI treatment for Wage and Hour back-pay damages, apply the retention to the eligible defense costs, and cap the payout according to the endorsement.*

**Attached Files (Context):**

* **epli-policy-specimen.pdf** – Public URL: https://www.abais.com/docs/default-source/small-business/epl/epli-policy-specimen.pdf

#### **3\. Rubric**

| \# | Description | Weight | Sources (with public URLs) | Justification | Met | Failure Reasoning | Dependent Criteria |
| :---- | :---- | :---- | :---- | :---- | :---- | :---- | :---- |
| 1 | Identifies that Wage and Hour claims typically exclude back-pay damages. | Critical | Insurance Principles | Standard EPLI policies do not indemnify employers for unpaid wages they legally owed. | FALSE | Paid the $50k back-pay. |  |
| 2 | Denies the $50,000 back-pay settlement. | Major | Prompt Text | Application of the exclusion. | FALSE | Added $50k to the total claim. | 1 |
| 3 | Identifies the policy includes "Defense Within Limits." | Minor | epli-policy-specimen.pdf | The header of the Dec page explicitly states defense costs reduce limits and apply to retentions. | FALSE | Ignored the defense within limits provision. |  |
| 4 | Identifies the Wage and Hour Defense Sub-limit is $100,000. | Major | Prompt Text; epli-policy-specimen.pdf | Endorsement D 71706 limits defense costs for this specific peril. | FALSE | Used the $1M limit. |  |
| 5 | Applies the $25,000 retention to the $150,000 gross defense costs. | Critical | Prompt Text | The retention is taken out of the total incurred defense cost first. | FALSE | Subtracted retention from the sub-limit directly. | 2 |
| 6 | Calculates the net eligible defense cost as $125,000. | Major | Prompt Text | $150,000 \- $25,000 \= $125,000. | FALSE | Math error. | 4 |
| 7 | Caps the net eligible defense cost at the $100,000 sub-limit. | Critical | Prompt Text | The $125,000 remaining exceeds the absolute cap of $100,000. | FALSE | Paid the full $125,000. | 3 |
| 8 | Concludes the total amount the insurer will pay is exactly $100,000. | Critical | Prompt Text | $0 damages \+ $100k capped defense. | FALSE | Incorrect final quantum. | 6 |
| 9 | Adopts a professional claims tone. | Minor | Prompt Text | Required styling. | FALSE | Unprofessional tone. |  |
| 10 | Includes the date May 10, 2023\. | Minor | Prompt Text | Temporal anchor. | FALSE | Omitted the date. |  |

#### **4\. Model Analysis**

This prompt tests an LLM on the interaction between Retentions and Sub-limits in a "Defense Within Limits" policy. Wage & Hour endorsements (like D 71706\) are notoriously tricky because they offer a sub-limit *strictly for defense costs*, explicitly excluding the actual back-pay. Models frequently fail by either covering the $50k back-pay, or by executing the retention math backward. A flawed model will take the $100k sub-limit, subtract the $25k retention, and state the insurer pays $75k. The correct structural math requires subtracting the retention from the *gross loss* ($150k \- $25k \= $125k) and then capping that remaining liability at the $100k sub-limit, resulting in a $100,000 payout.

#### **5\. Golden Response**

**COVERAGE DETERMINATION**

**DATE:** May 10, 2023

**SUBJECT:** Wage and Hour Claim Apportionment

Based on the policy provisions and the attached Endorsement D 71706(7), the financial apportionment for the Wage and Hour lawsuit is as follows:

**1\. Settlement Damages (Back-Pay):**

Under standard Employment Practices Liability policies, Wage and Hour endorsements provide coverage exclusively for defense costs. Employers are not indemnified for unpaid wages that were legally owed to employees under the Fair Labor Standards Act (FLSA). Therefore, the $50,000 settlement for back-pay is strictly excluded and must be paid out-of-pocket by the insured.

**2\. Defense Costs:**

The policy operates on a "Defense Within Limits" basis, meaning the Retention is applied directly to the incurred defense costs. Furthermore, Endorsement D 71706(7) establishes a strict $100,000 Cost of Defense Sub-Limit for Wage and Hour Violations.

* **Total Defense Costs Incurred:** $150,000  
* **Less Insured's Retention:** ($25,000)  
* **Net Eligible Defense Costs:** $125,000  
* **Sub-Limit Cap:** Because the $125,000 net eligible cost exceeds the $100,000 sub-limit, the insurer's liability is capped at the maximum sub-limit.

**Conclusion:**

The exact amount the insurer will pay for this claim is **$100,000**. The insured is responsible for the remaining $50,000 in defense costs and the $50,000 settlement.

### ---

**Example 5: Directors & Officers (D\&O) Crisis Event Management**

#### **1\. Metadata**

* **Category / Domain:** Claims Handling  
* **Difficulty:** Medium  
* **Themes:** D\&O, Crisis Management, Sub-limits, Deductible Exemptions  
* **Task Type:** Workflow  
* **Workflow:** Quantum Calculation  
* **Prompt Type:** Claims Allocation Brief

#### **2\. Prompt**

You are a D\&O Claims Adjuster. The current date is March 20, 2024\.

Your insured, a public company, suffered a major data breach resulting in negative national media coverage. The board of directors was sued. The insured incurred $200,000 in standard legal defense costs. Additionally, the company spent $80,000 to hire a public relations firm to mitigate the reputational damage.

Review the attached SBI General Suraksha Pro D\&O policy prospectus. Calculate the total payout from the insurer.

**Policy Details:**

* Base Policy Deductible: $25,000  
* The Crisis Management Coverage extension (Section 4.20) has a Sub-limit of $50,000.

*Note: Determine exactly how the deductible applies to the standard defense costs versus the Crisis Management PR costs based on the policy language.*

**Attached Files (Context):**

* **SURAKSHA\_PRO\_D\_and\_O\_INSURANCE\_Wording.pdf** – Public URL: https://content.sbigeneral.in/uploads/0ec0abae0c2c4db38338445032211077.pdf

#### **3\. Rubric**

| \# | Description | Weight | Sources (with public URLs) | Justification | Met | Failure Reasoning | Dependent Criteria |
| :---- | :---- | :---- | :---- | :---- | :---- | :---- | :---- |
| 1 | Identifies the Crisis Management Coverage extension (Section 4.20). | Minor | SURAKSHA\_PRO\_D\_and\_O\_INSURANCE\_Wording.pdf | Locates the applicable coverage for PR costs. | FALSE | Failed to find the extension. |  |
| 2 | Identifies that NO deductible applies to the Crisis Management extension. | Critical | SURAKSHA\_PRO\_D\_and\_O\_INSURANCE\_Wording.pdf | The policy explicitly states "no deductible shall apply on this". | FALSE | Applied the $25k deductible to the PR costs. | 1 |
| 3 | Caps the $80,000 PR costs at the $50,000 sub-limit. | Major | Prompt Text | The maximum available for Crisis Management. | FALSE | Allowed the full $80k. | 1 |
| 4 | Calculates the insurer payout for Crisis Management as $50,000. | Critical | Prompt Text | $80k cost \-\> no deductible \-\> capped at $50k limit. | FALSE | Math error on the sub-limit. | 6 |
| 5 | Applies the $25,000 policy deductible to the standard legal defense costs. | Major | Prompt Text | The main deductible applies to standard loss. | FALSE | Failed to deduct $25k from the legal fees. |  |
| 6 | Calculates the net payable legal defense costs as $175,000. | Critical | Prompt Text | $200,000 \- $25,000 \= $175,000. | FALSE | Math error. | 4 |
| 7 | Calculates the Total Insurer Payout as $225,000. | Critical | Prompt Text | $50,000 (PR) \+ $175,000 (Legal) \= $225,000. | FALSE | Incorrect final summation. | 3 |
| 8 | Explains why the deductible was applied differently to the two tranches of costs. | Major | SURAKSHA\_PRO\_D\_and\_O\_INSURANCE\_Wording.pdf | Demonstrates comprehension of the sub-limit exception. | FALSE | Provided numbers without contractual justification. | 6 |
| 9 | Formats the response as a Claims Allocation Brief. | Minor | Prompt Text | Output styling. | FALSE | Provided an unstructured list. |  |
| 10 | Includes the date March 20, 2024\. | Minor | Prompt Text | Temporal anchor. | FALSE | Omitted the date. |  |

#### **4\. Model Analysis**

D\&O policies contain numerous extensions with unique limit and deductible treatments. Under the SBI Suraksha Pro policy, the Crisis Management Coverage (Section 4.20) explicitly states that "no deductible shall apply on this." A base LLM will read the prompt, see a total loss of $280,000 ($200k \+ $80k), subtract the $25,000 deductible across the board, and attempt to pay $255,000. It fails to bifurcate the claim into its respective coverage buckets. To succeed, the model must isolate the $80k PR cost, recognize the zero-deductible exception, cap it at $50k, and then independently subtract the $25k deductible from the $200k legal defense bucket before summing the total.

#### **5\. Golden Response**

**CLAIMS ALLOCATION BRIEF**

**DATE:** March 20, 2024

**SUBJECT:** D\&O Payout Calculation – Data Breach & Crisis Management

**1\. Coverage Analysis & Allocation**

The insured's claim consists of two distinct cost centers which are treated differently under the SBI General Suraksha Pro D\&O policy.

* **Standard Legal Defense Costs:** The $200,000 incurred in legal defense costs is subject to the base policy provisions and the standard $25,000 deductible.  
* **Crisis Management Costs:** The $80,000 incurred for the public relations firm falls under the Crisis Management Coverage extension (Section 4.20). According to the policy wording, "The sublimit under this extension is as mentioned in the Policy Schedule and no deductible shall apply on this."

**2\. Quantum Calculation**

We must calculate the payouts for each bucket independently:

* **Legal Defense Payout:**  
  Total Incurred: $200,000  
  Less Base Deductible: ($25,000)  
  **Net Payable Legal:** $175,000  
* **Crisis Management Payout:**  
  Total Incurred: $80,000  
  Less Deductible: $0 (waived per Section 4.20)  
  Sub-limit Cap: The $80,000 exceeds the $50,000 maximum sub-limit.  
  **Net Payable PR:** $50,000

**3\. Final Payout**

The total payout from the insurer is **$225,000** ($175,000 for legal defense \+ $50,000 for crisis management).

#### **Works cited**

1. Casualty Actuarial Society E-Forum, Winter 2019, accessed on April 5, 2025, [https://www.casact.org/sites/default/files/database/forum\_19wforum\_completewinter2019.pdf](https://www.casact.org/sites/default/files/database/forum_19wforum_completewinter2019.pdf)  
2. CP 00 10 10 12 \- Building and Personal Property Coverage Form, accessed on April 5, 2025, [https://www.propertyinsurancecoveragelaw.com/wp-content/uploads/2023/12/CP-00-10-10-12-Building-and-Personal-Property-Coverage-Form.pdf](https://www.propertyinsurancecoveragelaw.com/wp-content/uploads/2023/12/CP-00-10-10-12-Building-and-Personal-Property-Coverage-Form.pdf)  
3. 1.4 Building and Personal Property Coverage Form \- Risk & Insurance Education Alliance, accessed on April 5, 2025, [https://www.riskeducation.org/learn/pluginfile.php/276804/mod\_page/content/1/74%20-%201.4%20Building%20and%20Personal%20Property%20Coverage%20Form.pdf](https://www.riskeducation.org/learn/pluginfile.php/276804/mod_page/content/1/74%20-%201.4%20Building%20and%20Personal%20Property%20Coverage%20Form.pdf)  
4. CAUSES OF LOSS – SPECIAL FORM \- Property Insurance Coverage Law Blog, accessed on April 5, 2025, [https://www.propertyinsurancecoveragelaw.com/wp-content/uploads/2020/04/Policy-1.pdf](https://www.propertyinsurancecoveragelaw.com/wp-content/uploads/2020/04/Policy-1.pdf)  
5. PRO Form \- ABA Insurance Services, accessed on April 5, 2025, [https://www.abais.com/docs/default-source/banks/policy-specimen/property-policy-w-dec-coverage-forms-mandatory-endorsements-exclusions.pdf](https://www.abais.com/docs/default-source/banks/policy-specimen/property-policy-w-dec-coverage-forms-mandatory-endorsements-exclusions.pdf)  
6. Specimen Reinsurance Agreement \- SEC.gov, accessed on April 5, 2025, [https://www.sec.gov/Archives/edgar/data/801019/000119312513170649/d480972dex9926g.htm](https://www.sec.gov/Archives/edgar/data/801019/000119312513170649/d480972dex9926g.htm)  
7. Quota Share Reinsurance Agreement \- SEC.gov, accessed on April 5, 2025, [https://www.sec.gov/Archives/edgar/data/8497/000119312511081483/dex1023.htm](https://www.sec.gov/Archives/edgar/data/8497/000119312511081483/dex1023.htm)

### **09-04-2025**

### ---

**Example 1: Commercial Auto Liability and "Care, Custody, or Control"**

#### **1\. Metadata**

* **Task Type:** Workflow  
* **Category / Domain:** Claims Handling  
* **Workflow:** Coverage Verification  
* **Prompt Type:** Claims Determination Memo  
* **Difficulty:** High

#### **2\. Prompt**

You are a Commercial Auto Claims Adjuster. The current date is August 14, 2022\.

Your insured, a custom art fabrication company, was delivering a $50,000 bronze statue they had manufactured to a buyer across the state. The insured's employee was driving a company-owned box truck, which is a covered "auto" under their Business Auto Coverage Form (ISO CA 00 01). During the transit, the employee misjudged the clearance of a bridge and struck the overpass. The truck sustained minor damage, but the $50,000 statue in the back of the truck was completely destroyed.

The insured has filed a claim requesting payment for the destroyed statue under the "Property Damage" Liability section of their CA 00 01 policy.

Review the attached ISO CA 00 01 policy form. Determine if the policy's liability coverage pays for the damage to the statue. Present your findings in a formal Claims Determination Memo citing the exact exclusion that applies.

**Attached Files (Context):**

* **BusinessAutoCoverageForm\_CA\_00\_01.pdf** – Public URL: https://www.archive.asrc.net/ASRC-Admin/ASRC-Financial-Insurance/ASRC-Insurance/2020-02-01-ASRC-Insurance-3-Auto.pdf

#### **3\. Rubric**

| \# | Description | Weight | Sources (with exact PDF pages) | Justification | Met | Failure Reasoning | Dependent Criteria |
| :---- | :---- | :---- | :---- | :---- | :---- | :---- | :---- |
| 1 | Acknowledges the incident occurred in a covered "auto". | Minor | Prompt Text | Establishes the initial premise of the claim. | FALSE | Stated the truck was not covered. |  |
| 2 | Identifies that the claim is being made under Section II \- Covered Autos Liability Coverage. | Minor | BusinessAutoCoverageForm\_CA\_00\_01.pdf, PDF Page 38 | Maps the claim to the correct section of the policy. | FALSE | Evaluated the claim under Physical Damage coverage. |  |
| 3 | Locates the exclusions section under Liability Coverage. | Major | BusinessAutoCoverageForm\_CA\_00\_01.pdf, PDF Page 39 | Identifies where coverage restrictions are listed. | FALSE | Failed to review the liability exclusions. |  |
| 4 | Identifies Exclusion 6, "Care, Custody or Control". | Critical | BusinessAutoCoverageForm\_CA\_00\_01.pdf, PDF Page 39 | The specific exclusion governing cargo in transit. | FALSE | Cited a different, incorrect exclusion. | 1 |
| 5 | Extracts the rule that Property Damage liability excludes property transported by the insured. | Major | BusinessAutoCoverageForm\_CA\_00\_01.pdf, PDF Page 39 | Exclusion 6.b. explicitly excludes property transported by an "insured." | FALSE | Failed to note the transportation exception. | 2 |
| 6 | Extracts the rule that Property Damage liability excludes property in the care, custody or control of the insured. | Major | BusinessAutoCoverageForm\_CA\_00\_01.pdf, PDF Page 39 | Exclusion 6.b. explicitly excludes property in the care of the insured. | FALSE | Missed the care, custody, or control language. | 2 |
| 7 | Connects the $50,000 statue to the definition of property being transported / in the insured's care. | Critical | Prompt Text | Synthesizes the facts of the prompt with the policy exclusion. | FALSE | Argued the statue was a third-party liability. | 3 |
| 8 | Concludes definitively that the claim for the $50,000 statue is DENIED. | Critical | Prompt Text | The final claims determination. | FALSE | Approved the claim for the statue. | 5 |
| 9 | Recommends that the insured would need an Inland Marine or Motor Truck Cargo policy for this exposure. | Additional | Insurance Principles | Standard adjuster advice for this specific coverage gap. | FALSE | Did not provide advice on alternative coverage. | 6 |
| 10 | Formats output as a formal Claims Determination Memo. | Minor | Prompt Text | Output styling requirement. | FALSE | Provided an unstructured paragraph. |  |

#### **4\. Model Analysis**

Commercial Auto Liability policies are designed to cover damage the insured's vehicle causes to *other people's* property (e.g., hitting another car or a building), not the property the insured is hauling. LLMs often struggle with this distinction. Because the policy broadly covers "property damage," a base model frequently assumes the cargo in the truck is covered, missing the explicit "Care, Custody or Control" exclusion found on PDF Page 39 of the CA 00 01 form. A highly capable model must accurately locate this exclusion, apply it to the cargo in transit, and deny the claim, recognizing the boundary between Auto Liability and Inland Marine/Cargo exposures.

#### **5\. Golden Response**

**CLAIMS DETERMINATION MEMO**

**TO:** Auto Claims File

**FROM:** Commercial Auto Claims Adjuster

**DATE:** August 14, 2022

**SUBJECT:** Liability Coverage Denial – Cargo in Transit

**1\. Claim Overview**

The insured has submitted a Property Damage Liability claim under their Business Auto Coverage Form (CA 00 01\) for a $50,000 bronze statue that was destroyed when the insured's employee struck a bridge with a company box truck.

**2\. Policy Language and Application**

The claim is being evaluated under Section II \- Covered Autos Liability Coverage. While the policy does pay for "property damage" that an insured legally causes in a covered auto, this coverage is subject to specific exclusions.

According to PDF Page 39 of the attached CA 00 01 form, **Exclusion 6\. Care, Custody Or Control** applies. This provision explicitly states that the policy does not cover "Property damage" to or "Covered pollution cost or expense" involving:

* a. Property owned or transported by the "insured" or in the "insured's" care, custody or control.

**3\. Coverage Determination**

Because the $50,000 bronze statue was actively being transported by the insured's employee inside the covered auto at the time of the accident, it falls squarely within the "Care, Custody Or Control" exclusion. The Business Auto policy is not designed to act as cargo insurance.

Therefore, the claim for the $50,000 statue is **DENIED**. The insured would need to file this claim under a separate Motor Truck Cargo or Inland Marine policy, if one is in force.

### ---

**Example 2: Workers' Compensation Premium & Loss Cost Multipliers**

#### **1\. Metadata**

* **Task Type:** Workflow  
* **Category / Domain:** Underwriting  
* **Workflow:** Premium Calculation  
* **Prompt Type:** Rating Worksheet  
* **Difficulty:** High

#### **2\. Prompt**

You are a Commercial Underwriter. The current date is November 15, 2023\.

You are pricing a Workers' Compensation policy for a construction company operating in New York. The business operates under two distinct classification codes:

* Code 2802 (Carpentry): Estimated Annual Payroll of $150,000  
* Code 3365 (Welding): Estimated Annual Payroll of $75,000

The insurance carrier's filed Loss Cost Multiplier (LCM) is 1.20.

Using the provided New York Loss Cost Comparison bulletin, extract the active rates for October 2023 for both class codes. Calculate the manual premium for each class code, and then sum them for the total manual premium. Round all final dollar amounts to the nearest whole dollar. Show your math in a structured Rating Worksheet.

**Attached Files (Context):**

* **NY-LC-Comparison-10-1-2023.pdf** – Public URL: https://www.nycirb.org/bulletins/rc2610.pdf

#### **3\. Rubric**

| \# | Description | Weight | Sources (with exact PDF pages) | Justification | Met | Failure Reasoning | Dependent Criteria |
| :---- | :---- | :---- | :---- | :---- | :---- | :---- | :---- |
| 1 | Extracts the Oct. 2023 rate for Code 2802 as 4.11. | Major | NY-LC-Comparison-10-1-2023.pdf, PDF Page 4 | Requires extracting the correct historical rate from the table. | FALSE | Used the 2024 rate (3.99). |  |
| 2 | Extracts the Oct. 2023 rate for Code 3365 as 6.47. | Major | NY-LC-Comparison-10-1-2023.pdf, PDF Page 4 | Requires extracting the correct rate from the table. | FALSE | Used the 2024 rate (5.23). |  |
| 3 | Divides the Code 2802 payroll by 100 to find the exposure base. | Critical | Workers Compensation Principles | WC rates are universally applied per $100 of payroll. | FALSE | Multiplied the rate against the full $150,000 payroll. |  |
| 4 | Divides the Code 3365 payroll by 100 to find the exposure base. | Critical | Workers Compensation Principles | WC rates are universally applied per $100 of payroll. | FALSE | Multiplied the rate against the full $75,000 payroll. |  |
| 5 | Calculates Code 2802 premium as $7,398. | Critical | Prompt Text | ($150,000/100) \* 4.11 \* 1.20 \= 7,398. | FALSE | Math error or failed to apply LCM. | 7 |
| 6 | Calculates Code 3365 premium as $5,823. | Critical | Prompt Text | ($75,000/100) \* 6.47 \* 1.20 \= 5,823. | FALSE | Math error or failed to apply LCM. | 8 |
| 7 | Calculates the total manual premium as $13,221. | Critical | Prompt Text | $7,398 \+ $5,823 \= $13,221. | FALSE | Incorrect sum. | 3 |
| 8 | Formats the output clearly as a Rating Worksheet. | Minor | Prompt Text | Output styling. | FALSE | Provided an unstructured paragraph. |  |
| 9 | Includes the date November 15, 2023\. | Minor | Prompt Text | Temporal anchor. | FALSE | Omitted the date. |  |
| 10 | Follows the instruction to round to the nearest whole dollar. | Minor | Prompt Text | Formatting constraint. | FALSE | Included decimal values in the final numbers. | 3 |

#### **4\. Model Analysis**

Models frequently fail Workers' Compensation calculations because they lack the domain-specific knowledge that rating exposures are calculated "per $100 of payroll." An untrained model will extract the rate of 4.11 and multiply it directly by $150,000, resulting in a hallucinated premium that is inflated by a factor of 100\. Furthermore, the prompt's anchor date is November 15, 2023, requiring the model to exercise temporal reasoning by selecting the "Oct. 2023" column from the NYCIRB PDF instead of the "Oct. 2024" column.

#### **5\. Golden Response**

**Workers' Compensation Rating Worksheet**

**Date:** November 15, 2023

Based on the NYCIRB Loss Cost Comparison bulletin, the active rates for October 2023 are utilized. Workers' Compensation premiums are calculated by dividing the gross payroll by 100 to determine the exposure base, multiplying by the class code loss cost rate, and then multiplying by the carrier's Loss Cost Multiplier (LCM).

* **Class 2802 (Carpentry)**:  
  * Exposure Base: $150,000 payroll / 100 \= 1,500  
  * Loss Cost Rate (Oct 2023): 4.11  
  * LCM: 1.20  
  * Premium: 1,500 \* 4.11 \* 1.20 \= **$7,398**  
* **Class 3365 (Welding)**:  
  * Exposure Base: $75,000 payroll / 100 \= 750  
  * Loss Cost Rate (Oct 2023): 6.47  
  * LCM: 1.20  
  * Premium: 750 \* 6.47 \* 1.20 \= **$5,823**  
* **Total Manual Premium**: $7,398 \+ $5,823 \= **$13,221**

### ---

**Example 3: Surety Performance Bond Contractual Obligations**

#### **1\. Metadata**

* **Task Type:** Capability  
* **Category / Domain:** Underwriting  
* **Workflow:** Bond Issuance  
* **Prompt Type:** Legal Brief  
* **Difficulty:** Low

#### **2\. Prompt**

You are a Surety Underwriter. The current date is April 10, 2022\.

A contractor is applying for a performance bond for a public works project. Review the attached City of Tukwila Performance Bond template. Under what two specific, enumerated conditions will the Principal's obligation under this bond continue in effect until officially released in writing by the City?

Provide the exact headers for these two conditions in a short legal brief.

**Attached Files (Context):**

* **DCD-Performance-Bond-Template.pdf** – Public URL: https://www.tukwilawa.gov/wp-content/uploads/DCD-Performance-Bond-Template.pdf

#### **3\. Rubric**

| \# | Description | Weight | Sources (with exact PDF pages) | Justification | Met | Failure Reasoning | Dependent Criteria |
| :---- | :---- | :---- | :---- | :---- | :---- | :---- | :---- |
| 1 | Identifies Condition 1 as "Description of Work". | Critical | DCD-Performance-Bond-Template.pdf, PDF Page 2 | Exact header from the form governing the release of the bond. | FALSE | Hallucinated a generic surety condition. |  |
| 2 | Identifies Condition 2 as "Compliance of Work with Specifications". | Critical | DCD-Performance-Bond-Template.pdf, PDF Page 2 | Exact header from the form governing the release of the bond. | FALSE | Hallucinated a generic surety condition. |  |
| 3 | Notes that the obligation continues until released in writing by the City of Tukwila. | Major | DCD-Performance-Bond-Template.pdf, PDF Page 2 | Captures the contextual requirement for the release. | FALSE | Failed to mention the written release requirement. |  |
| 4 | Identifies the Principal as the party performing the conditions. | Minor | DCD-Performance-Bond-Template.pdf, PDF Page 2 | Clarifies the roles in the surety contract. | FALSE | Stated the Surety performs the work. |  |
| 5 | Formats the response as a short legal brief. | Minor | Prompt Text | Required output styling. | FALSE | Provided an unstructured paragraph. |  |
| 6 | Maintains an objective, legal tone. | Minor | Prompt Text | Tone alignment. | FALSE | Used conversational language. |  |
| 7 | Includes the date April 10, 2022\. | Minor | Prompt Text | Temporal anchor. | FALSE | Omitted the date. |  |
| 8 | Does not hallucinate general "payment of subcontractors" as a condition. | Major | Prompt Text | Payment bonds are separate; this form focuses strictly on performance. | FALSE | Included payment conditions not found under the enumerated list. | 7 |
| 9 | Explicitly names the "City of Tukwila" as the obligee. | Minor | DCD-Performance-Bond-Template.pdf, PDF Page 1 | Correctly identifies the municipal entity. | FALSE | Used a generic term like "the municipality". |  |
| 10 | Avoids providing unprompted underwriting advice on collateral requirements. | Minor | Prompt Text | Scope focus. | FALSE | Included an unnecessary section on securing LOCs. |  |

#### **4\. Model Analysis**

When tasked with extracting legal conditions from a specific bond template, LLMs often bypass the attached document and inject generic knowledge regarding surety bonds (e.g., "the bond is released upon timely completion and payment of all subcontractors"). To succeed, the model must suppress its training data and strictly extract the exact numbered headers (1. Description of Work, 2\. Compliance of Work with Specifications) listed on PDF Page 2 of the provided municipal template.

#### **5\. Golden Response**

**SURETY LEGAL BRIEF**

**DATE:** April 10, 2022

**SUBJECT:** Performance Bond Release Conditions – City of Tukwila

**Overview:**

Under the terms of the City of Tukwila Performance Bond template, the surety obligation is strictly tied to the Principal's performance. The bond explicitly dictates that the obligation shall continue in effect until released in writing by the City of Tukwila.

**Enumerated Conditions for Release:**

This written release will only occur after the Principal has performed and satisfied the following two specifically enumerated conditions:

* **Description of Work**  
* **Compliance of Work with Specifications**

The Principal must complete the work according to the applicable ordinances and standards of the City, State, and Federal statutes to the satisfaction of the City prior to the bond being exonerated.

### ---

**Example 4: Inland Marine Contractors Equipment \- Sublimit Application**

#### **1\. Metadata**

* **Task Type:** Workflow  
* **Category / Domain:** Claims Handling  
* **Workflow:** Quantum Calculation  
* **Prompt Type:** Claims Apportionment Memo  
* **Difficulty:** High

#### **2\. Prompt**

You are a Commercial Property Claims Adjuster. The current date is January 5, 2023\.

Your insured, a local construction contractor, had their job site trailer burglarized overnight. The thieves stole a $12,000 skid steer loader (owned by the insured) and $4,000 worth of personal tools belonging to the insured's foreman.

The insured has an active Inland Marine policy utilizing the attached Selective Contractors Equipment Coverage Form. The policy has a blanket Limit of Insurance of $50,000 and a Deductible of $500.

Review the attached coverage form to determine if both items are covered, apply any applicable sub-limits, subtract the deductible, and calculate the final payable amount. Present your calculations in a Claims Apportionment Memo.

**Attached Files (Context):**

* **CM7197.201503.PDF** – Public URL: https://home7.selectiveinsurance.com/FormsPDF/CM/CM7197.201503.PDF

#### **3\. Rubric**

| \# | Description | Weight | Sources (with exact PDF pages) | Justification | Met | Failure Reasoning | Dependent Criteria |
| :---- | :---- | :---- | :---- | :---- | :---- | :---- | :---- |
| 1 | Identifies that the $12,000 skid steer loader is Covered Property. | Minor | CM7197.201503.PDF, PDF Page 1 | Fits the definition of "Contractors equipment" under Section A.1.b. | FALSE | Denied the skid steer. |  |
| 2 | Identifies that the foreman's personal tools are eligible for coverage. | Major | CM7197.201503.PDF, PDF Page 5 | Section C.2 provides an extension for personal property of employees, including employee tools. | FALSE | Denied the employee tools entirely. |  |
| 3 | Locates the sub-limit for employee tools. | Critical | CM7197.201503.PDF, PDF Page 5 | Section C.2 limits employee tools to $2,500 for any one item. | FALSE | Failed to find the sub-limit. | 8 |
| 4 | Caps the $4,000 tool loss at the $2,500 sub-limit. | Critical | Prompt Text; Policy | The loss exceeds the sub-limit, so it must be truncated. | FALSE | Allowed the full $4,000. | 1 |
| 5 | Calculates the gross eligible loss as $14,500. | Major | Prompt Text | $12,000 (skid steer) \+ $2,500 (capped tools) \= $14,500. | FALSE | Math error or failed to cap the tools. | 7 |
| 6 | Subtracts the $500 deductible from the gross eligible loss. | Major | Prompt Text | Standard deductible application. | FALSE | Applied the deductible incorrectly. |  |
| 7 | Calculates the final payable amount as $14,000. | Critical | Prompt Text | $14,500 \- $500 \= $14,000. | FALSE | Incorrect final quantum. | 3 |
| 8 | Confirms the $14,000 total is well within the $50,000 blanket Limit of Insurance. | Minor | Prompt Text | Validates the policy limit was not breached. | FALSE | Stated the policy limit was exhausted. | 5 |
| 9 | Formats the response as a Claims Apportionment Memo. | Minor | Prompt Text | Required output styling. | FALSE | Wrote a casual email. |  |
| 10 | Includes the date January 5, 2023\. | Minor | Prompt Text | Temporal anchor. | FALSE | Omitted the date. |  |

#### **4\. Model Analysis**

Inland Marine forms for contractors routinely include small sub-limits for "employee tools" or "property of others" because the primary intent of the policy is to cover the named insured's heavy machinery. A base LLM will read that the policy has a $50,000 limit, see a total loss of $16,000 ($12k \+ $4k), subtract the $500 deductible, and authorize a payment of $15,500. It requires deep document parsing to locate the specific Limitation under Section C.2 on PDF Page 5 that restricts employee tools to $2,500, requiring the model to truncate the $4k loss before aggregating the final total.

#### **5\. Golden Response**

**CLAIMS APPORTIONMENT MEMORANDUM**

**TO:** Inland Marine Claims File

**FROM:** Commercial Property Claims Adjuster

**DATE:** January 5, 2023

**SUBJECT:** Contractors Equipment Theft Claim

**1\. Coverage Verification**

The insured suffered a theft loss of equipment from a job site. The loss is evaluated under the Selective Contractors Equipment Coverage Form (CM 71 97).

* **Skid Steer Loader ($12,000):** This unit is owned by the insured and qualifies as "Contractors equipment" under Section A.1.b. It is fully covered.  
* **Foreman's Personal Tools ($4,000):** Section C. LIMITS OF INSURANCE, paragraph 2, provides coverage for the "personal property of employees, including employee tools." Therefore, the tools are eligible for coverage.

**2\. Sub-Limit Application**

While the employee tools are covered, Section C.2 explicitly limits our payment for this exposure. The policy states: "The most we will pay in any one occurrence for loss or damage to any one item of personal property of employees, including employee tools, is $2,500."

Therefore, the $4,000 claim for the foreman's tools must be capped at the **$2,500** sub-limit.

**3\. Quantum Calculation**

* Eligible Skid Steer Loss: $12,000  
* Eligible Employee Tools Loss: $2,500  
* **Gross Eligible Loss:** $14,500  
* Less Policy Deductible: ($500)  
* **Final Payable Amount:** **$14,000**

*Note: The $14,000 payment is well within the policy's $50,000 blanket Limit of Insurance.*

### ---

**Example 5: Commercial General Liability (CGL) Impaired Property Exclusion**

#### **1\. Metadata**

* **Task Type:** Workflow  
* **Category / Domain:** Claims Handling  
* **Workflow:** Coverage Verification  
* **Prompt Type:** Claims Determination Memo  
* **Difficulty:** High

#### **2\. Prompt**

You are a Commercial General Liability Claims Adjuster. The current date is March 12, 2022\.

Your insured, an HVAC contractor, installed a custom-built industrial cooling unit in a commercial bakery. The unit was defective and failed to cool the facility. As a result, the bakery was forced to shut down for 3 days while the insured repaired the defective unit.

There was no physical damage to the bakery's building or to any of the bakery's dough/products. However, the bakery lost $20,000 in business income during the 3-day shutdown and has sued your insured for the lost income. The insured has filed a claim under their standard CGL policy (ISO CG 00 01 01 96\) to cover the $20,000.

Review the attached ISO CGL policy. Determine if the policy covers this $20,000 economic loss. Cite the specific exclusion that applies to this scenario and explain its mechanics in a Claims Determination Memo.

**Attached Files (Context):**

* **cg-00-01-01-96.pdf** – Public URL: https://ogs.ny.gov/system/files/documents/2021/09/cg-00-01-01-96.pdf

#### **3\. Rubric**

| \# | Description | Weight | Sources (with exact PDF pages) | Justification | Met | Failure Reasoning | Dependent Criteria |
| :---- | :---- | :---- | :---- | :---- | :---- | :---- | :---- |
| 1 | Acknowledges the claim is for "property damage" in the form of loss of use. | Minor | Prompt Text | Loss of use of tangible property that is not physically injured is defined as property damage. | FALSE | Claimed it was bodily injury or not property damage at all. |  |
| 2 | Locates Exclusion 'm', Damage To Impaired Property or Property Not Physically Injured. | Critical | cg-00-01-01-96.pdf, PDF Page 4 | The specific exclusion governing defective products that cause loss of use without physical injury. | FALSE | Missed the exclusion entirely. |  |
| 3 | Extracts the rule that coverage is excluded for property that has not been physically injured arising out of a defect in "your product". | Major | cg-00-01-01-96.pdf, PDF Page 4 | Textual extraction of Exclusion m(1). | FALSE | Quoted the wrong part of the exclusion. | 8 |
| 4 | Connects the defective HVAC unit to the definition of "your product" or "your work". | Major | Prompt Text | Synthesizes the prompt facts with the policy language. | FALSE | Argued the HVAC was not the insured's product. | 8 |
| 5 | Recognizes that the bakery could be restored to use by the repair of the HVAC unit. | Major | cg-00-01-01-96.pdf, PDF Page 4 | Satisfies the condition of the exclusion that the property can be restored to use by repairing the defective work. | FALSE | Ignored the restoration clause of the exclusion. | 8 |
| 6 | Concludes definitively that the claim for the $20,000 in lost income is DENIED. | Critical | Prompt Text | The final claims determination. | FALSE | Approved the claim. | 8 |
| 7 | Does not hallucinate coverage based on the "Products-Completed Operations" hazard. | Major | Insurance Principles | The impaired property exclusion applies even if the work is completed. | FALSE | Authorized the claim under Products-Completed Operations. | 4 |
| 8 | Explains that CGL policies are not designed to guarantee the insured's work or products. | Minor | Insurance Principles | Contextualizes the purpose of the exclusion. | FALSE | Omitted the rationale for the exclusion. |  |
| 9 | Formats the response as a formal Claims Determination Memo. | Minor | Prompt Text | Required output styling. | FALSE | Wrote an unstructured paragraph. |  |
| 10 | Includes the date March 12, 2022\. | Minor | Prompt Text | Temporal anchor. | FALSE | Omitted the date. |  |

#### **4\. Model Analysis**

The "Impaired Property" exclusion (Exclusion m) is one of the most complex and frequently misunderstood provisions in a Commercial General Liability policy. It is designed to prevent the CGL policy from acting as a warranty for the insured's defective work. Because the bakery suffered a financial loss ("loss of use" is defined as property damage), an LLM will often assume that because third-party property damage occurred, the CGL policy must respond. To succeed, the model must trace the logic of Exclusion m: the bakery is property that was not physically injured, its loss of use arose from a defect in the insured's product (the HVAC), and the bakery was restored to use simply by repairing the HVAC. Therefore, the exclusion applies perfectly, and the claim must be denied.

#### **5\. Golden Response**

**CLAIMS DETERMINATION MEMO**

**TO:** CGL Claims File

**FROM:** Commercial General Liability Claims Adjuster

**DATE:** March 12, 2022

**SUBJECT:** Coverage Denial – Loss of Use and Impaired Property

**1\. Claim Overview**

The insured HVAC contractor has been sued for $20,000 in lost business income by a commercial bakery. The loss occurred because a custom cooling unit built and installed by the insured was defective, causing the bakery to shut down for 3 days until the insured could repair it. There was no physical damage to the bakery or its products.

**2\. Policy Language and Application**

The bakery's claim for lost income constitutes a claim for "loss of use of tangible property that is not physically injured," which falls under the definition of "Property damage" in the CGL policy.

However, we must look to the exclusions under Section I \- Coverage A. According to PDF Page 4 of the attached ISO CG 00 01 form, **Exclusion m. Damage To Impaired Property Or Property Not Physically Injured** applies.

This provision explicitly excludes "Property damage" to property that has not been physically injured arising out of:

* (1) A defect, deficiency, inadequacy or dangerous condition in "your product" or "your work".

The exclusion applies if the property can be restored to use by the repair or replacement of "your product" or "your work."

**3\. Coverage Determination**

The bakery suffered a loss of use without physical injury. This loss arose directly from a defect in the insured's product/work (the HVAC unit). Furthermore, the bakery was restored to use as soon as the insured repaired the defective unit.

General Liability policies are not performance bonds or warranties for an insured's defective products. Because the facts of this loss trigger Exclusion m perfectly, there is no coverage for the bakery's economic loss. The claim for $20,000 is **DENIED**.

### **10-04-2025**

### ---

**Example 1: Fiduciary Liability (Defense Within Limits)**

#### **1\. Metadata**

* **Task Type:** Workflow  
* **Category / Domain:** Claims Handling  
* **Workflow:** Quantum Calculation  
* **Prompt Type:** Claims Apportionment Memo  
* **Difficulty:** High

#### **2\. Prompt**

You are a Professional Liability Claims Adjuster. The current date is December 1, 2023\.

Your insured, a corporate board of trustees, was sued for breaching their fiduciary duties under ERISA. The lawsuit has been settled. You must calculate the final out-of-pocket costs for the insured and the total amount paid by the insurance company based on the attached Fiduciary Liability policy.

**Claim Details:**

* Aggregate Limit of Liability: $1,000,000.  
* Retention (Deductible): $50,000.  
* Defense Costs Incurred: $250,000.  
* Final Settlement Amount: $900,000.

Using the specific provisions in the attached specimen policy regarding how claim expenses interact with the limits and deductibles, calculate the final financial apportionment.

**Attached Files (Context):**

* **The-Encore-Fiduciary-Liability-Policy-SPECIMEN-1.12v1.pdf** – Public URL: https://encorefiduciary.com/wp-content/uploads/2024/01/The-Encore-Fiduciary-Liability-Policy-SPECIMEN-1.12v1.pdf

#### **3\. Rubric**

| \# | Description | Weight | Sources (with exact PDF pages) | Justification | Met | Failure Reasoning | Dependent Criteria |
| :---- | :---- | :---- | :---- | :---- | :---- | :---- | :---- |
| 1 | Identifies that the policy utilizes "Defense Within Limits" (eroding limits). | Critical | The-Encore-Fiduciary-Liability-Policy-SPECIMEN-1.12v1.pdf, PDF Page 2 | The Declarations page explicitly states "CLAIM EXPENSES ARE INCLUDED IN THE LIMITS OF LIABILITY". | FALSE | Assumed defense costs were paid in addition to the limit. |  |
| 2 | Calculates the total gross loss (Defense \+ Settlement) as $1,150,000. | Minor | Prompt Text | $250k \+ $900k \= $1.15M. | FALSE | Failed to sum the total incurred costs. |  |
| 3 | Applies the $50,000 retention to the gross loss. | Major | Prompt Text | Insured must pay the first $50k. | FALSE | Deducted $50k from the policy limit instead of the loss. | 6 |
| 4 | Determines the net loss subject to the insurer's limit is $1,100,000. | Major | Prompt Text | $1,150,000 \- $50,000 \= $1,100,000. | FALSE | Incorrect subtraction. | 2 |
| 5 | Compares the net loss ($1.1M) to the Policy Limit ($1M). | Critical | Policy Principles | Because defense costs erode the limit, the total available limit for both is $1M. | FALSE | Treated the $1M limit as applying only to the settlement. | 1 |
| 6 | Concludes the Insurer pays exactly $1,000,000. | Critical | Prompt Text | The net loss exceeds the policy limit, capping the insurer payout at the limit. | FALSE | Authorized a payment above the $1M limit. | 4 |
| 7 | Concludes the Insured's total out-of-pocket cost is exactly $150,000. | Critical | Prompt Text | The insured pays their $50k retention plus the $100k that exceeds the policy limit. | FALSE | Only charged the insured for the $50k retention. | 3 |
| 8 | Explains the mechanics of "Defense Within Limits" in the memo narrative. | Minor | The-Encore-Fiduciary-Liability-Policy-SPECIMEN-1.12v1.pdf, PDF Page 2 | Demonstrates comprehension of the core policy feature. | FALSE | Provided numbers without explaining the eroding limit concept. |  |
| 9 | Formats output as a professional claims apportionment memo. | Minor | Prompt Text | Required output styling. | FALSE | Provided an unstructured data dump. |  |
| 10 | Includes the date December 1, 2023\. | Minor | Prompt Text | Temporal anchor. | FALSE | Omitted the date. |  |

#### **4\. Model Analysis**

This scenario tests a model's understanding of "Defense Within Limits" (or shrinking limits), a critical concept in professional liability. Standard General Liability policies pay defense costs *in addition* to the policy limit. Models trained on general insurance data will routinely assume the insurer pays the $250k defense costs in full, and then pays the $900k settlement out of the $1M limit (less the retention). The attached policy specifically notes on Page 2 in bold text that claim expenses *reduce* the limit of liability. A successful model must catch this nuance, sum the total loss, apply the retention, and recognize that the remaining balance ($1.1M) exhausts the eroding $1M limit, leaving the insured responsible for the $100k overage.

#### **5\. Golden Response**

**CLAIMS APPORTIONMENT MEMORANDUM**

**Date:** December 1, 2023

**To:** Fiduciary Liability Claim File

**Subject:** Final Financial Apportionment

**1\. Policy Architecture Review**

The insured's Fiduciary Liability policy features a $1,000,000 Limit of Liability and a $50,000 Retention. Crucially, as outlined on PDF Page 2 of the policy Declarations in bold text, this is an eroding limits policy: "CLAIM EXPENSES ARE INCLUDED IN THE LIMITS OF LIABILITY." This means that the $1,000,000 limit is the absolute maximum the insurer will pay for the *combined* total of defense costs and the final settlement.

**2\. Loss Aggregation and Retention Application**

* **Total Defense Costs Incurred:** $250,000  
* **Final Settlement Amount:** $900,000  
* **Total Gross Loss:** $1,150,000

The insured is responsible for the first $50,000 of the total gross loss.

* **Net Loss Subject to Policy Limit:** $1,150,000 \- $50,000 \= $1,100,000.

**3\. Final Apportionment**

The net loss remaining after the retention is $1,100,000. Because the policy's Limit of Liability is $1,000,000, the remaining loss exhausts the available limit, and the insured is responsible for the overage.

* **Total Amount Paid by Insurer:** **$1,000,000**  
* **Insured's Total Out-of-Pocket Cost:** **$150,000** ($50,000 retention \+ $100,000 limit overage).

### ---

**Example 2: Workers' Compensation Premium Calculation**

#### **1\. Metadata**

* **Task Type:** Workflow  
* **Category / Domain:** Underwriting  
* **Workflow:** Premium Calculation  
* **Prompt Type:** Rating Worksheet  
* **Difficulty:** Medium

#### **2\. Prompt**

You are a Commercial Underwriter. The current date is November 1, 2023\.

Calculate the total manual premium for a landscaping/nursery risk located in New York. The business operates under two classification codes: Code 2802 (Payroll: $150,000) and Code 3365 (Payroll: $75,000). The company's filed Loss Cost Multiplier (LCM) is 1.15. Round final premiums to the nearest whole dollar.

Use the provided New York Loss Cost Comparison bulletin to extract the active rates.

**Attached Files (Context):**

* **rc2610.pdf** – Public URL: https://www.nycirb.org/bulletins/rc2610.pdf

#### **3\. Rubric**

| \# | Description | Weight | Sources (with exact PDF pages) | Justification | Met | Failure Reasoning | Dependent Criteria |
| :---- | :---- | :---- | :---- | :---- | :---- | :---- | :---- |
| 1 | Extracts the Oct. 2023 rate for Code 2802 as 4.11. | Major | rc2610.pdf, PDF Page 4 | Requires extracting the correct historical rate from the table. | FALSE | Used the 2024 rate. |  |
| 2 | Extracts the Oct. 2023 rate for Code 3365 as 6.47. | Major | rc2610.pdf, PDF Page 4 | Requires extracting the correct historical rate from the table. | FALSE | Used the 2024 rate. |  |
| 3 | Divides the Code 2802 payroll by 100 to find the exposure base. | Critical | Workers Compensation Principles | WC rates are universally applied per $100 of payroll. | FALSE | Multiplied the rate against the full $150,000 payroll. |  |
| 4 | Divides the Code 3365 payroll by 100 to find the exposure base. | Critical | Workers Compensation Principles | WC rates are universally applied per $100 of payroll. | FALSE | Multiplied the rate against the full $75,000 payroll. |  |
| 5 | Calculates Code 2802 premium as $7,090. | Critical | Prompt Text | ($150,000/100) \* 4.11 \* 1.15 \= 7,089.75 (rounds to $7,090). | FALSE | Math error or failed to apply LCM. | 1 |
| 6 | Calculates Code 3365 premium as $5,580. | Critical | Prompt Text | ($75,000/100) \* 6.47 \* 1.15 \= 5,580.375 (rounds to $5,580). | FALSE | Math error or failed to apply LCM. | 6 |
| 7 | Calculates the total manual premium as $12,670. | Critical | Prompt Text | $7,090 \+ $5,580 \= $12,670. | FALSE | Incorrect sum. | 4 |
| 8 | Formats the output clearly as a Rating Worksheet. | Minor | Prompt Text | Output styling. | FALSE | Provided an unstructured paragraph. |  |
| 9 | Includes the date November 1, 2023\. | Minor | Prompt Text | Temporal anchor. | FALSE | Omitted the date. |  |
| 10 | Follows the instruction to round to the nearest whole dollar. | Minor | Prompt Text | Formatting constraint. | FALSE | Included decimal values in the final numbers. | 4 |

#### **4\. Model Analysis**

Models frequently fail Workers' Compensation calculations because they do not intuitively understand that rates are applied "per $100 of payroll". A model will often extract the rate of 4.11 and multiply it directly by $150,000, resulting in a hallucinated premium that is inflated by a factor of 100\. Furthermore, the prompt's anchor date is November 1, 2023, requiring the model to correctly select the "Oct. 2023" column from the NYCIRB PDF instead of the "Oct. 2024" column, testing chronological reasoning and data extraction precision.

#### **5\. Golden Response**

**Workers' Compensation Rating Worksheet**

**Date:** November 1, 2023

Based on the NYCIRB Loss Cost Comparison bulletin, the active rates for October 2023 are utilized. Workers' Compensation premiums are calculated by dividing the gross payroll by 100 to determine the exposure base, multiplying by the loss cost rate, and then multiplying by the carrier's Loss Cost Multiplier (LCM).

* **Class 2802**:  
  * Exposure Base: $150,000 payroll / 100 \= 1,500  
  * Loss Cost Rate (Oct 2023): 4.11  
  * LCM: 1.15  
  * Premium: 1,500 \* 4.11 \* 1.15 \= $7,089.75 (Rounded to **$7,090**)  
* **Class 3365**:  
  * Exposure Base: $75,000 payroll / 100 \= 750  
  * Loss Cost Rate (Oct 2023): 6.47  
  * LCM: 1.15  
  * Premium: 750 \* 6.47 \* 1.15 \= $5,580.375 (Rounded to **$5,580**)  
* **Total Manual Premium**: $7,090 \+ $5,580 \= **$12,670**

### ---

**Example 3: Excess of Loss Reinsurance Layering**

#### **1\. Metadata**

* **Task Type:** Workflow  
* **Category / Domain:** Reinsurance  
* **Workflow:** Quantum Calculation  
* **Prompt Type:** Treaty Recovery Statement  
* **Difficulty:** High

#### **2\. Prompt**

You are a Reinsurance Analyst. The current date is February 1, 2025\.

A massive hurricane generated a gross loss of $6.5 billion for your company. According to your Corporate Catastrophe Excess-of-Loss Reinsurance Treaty (attached), calculate the exact dollar amount of the recovery you will receive from this specific treaty across all applicable layers. Show your step-by-step mathematical calculations for each layer.

**Attached Files (Context):**

* **tm2510932d1\_ars.pdf** – Public URL: https://www.sec.gov/Archives/edgar/data/86312/000110465925032230/tm2510932d1\_ars.pdf

#### **3\. Rubric**

| \# | Description | Weight | Sources (with exact PDF pages) | Justification | Met | Failure Reasoning | Dependent Criteria |
| :---- | :---- | :---- | :---- | :---- | :---- | :---- | :---- |
| 1 | Extracts the base retention amount as $4.0 billion. | Major | tm2510932d1\_ars.pdf, PDF Page 31 | The treaty explicitly states the first layer attaches in excess of $4.0 billion. | FALSE | Failed to identify the attachment point. |  |
| 2 | Identifies the limits of the First Layer as $4.0 billion to $5.0 billion. | Minor | tm2510932d1\_ars.pdf, PDF Page 31 | Identifies the boundaries of the first layer. | FALSE | Assumed the first layer was uncapped. | 1 |
| 3 | Extracts the recovery percentage for the First Layer as 80%. | Major | tm2510932d1\_ars.pdf, PDF Page 31 | The treaty dictates 80% participation for this specific band. | FALSE | Used the 95% or 100% participation figures from higher bands. | 6 |
| 4 | Calculates the First Layer recovery as $800,000,000. | Critical | Prompt Text; Treaty | $1.0B (amount in layer) \* 0.80 \= $800M. | FALSE | Math error or applied 80% to the entire gross loss. | 6 |
| 5 | Identifies the limits of the Second Layer as $5.0 billion to $7.5 billion. | Minor | tm2510932d1\_ars.pdf, PDF Page 31 | Identifies the boundaries of the second layer. | FALSE | Failed to map the second band. |  |
| 6 | Extracts the recovery percentage for the Second Layer as 95%. | Major | tm2510932d1\_ars.pdf, PDF Page 31 | The treaty dictates 95% participation for this specific band. | FALSE | Applied the wrong percentage. | 4 |
| 7 | Calculates the loss amount subject to the Second Layer as $1.5 billion. | Major | Prompt Text | Total loss is $6.5B. Layer attaches at $5.0B. ($6.5B \- $5.0B \= $1.5B inside the layer). | FALSE | Assumed the entire $2.5B layer limit was exhausted. | 4 |
| 8 | Calculates the Second Layer recovery as $1,425,000,000. | Critical | Prompt Text; Treaty | $1.5B (amount in layer) \* 0.95 \= $1.425B. | FALSE | Math error. | 5 |
| 9 | Correctly states that the Third Layer ($7.5B to $8.0B) is not triggered. | Minor | Prompt Text | The $6.5B gross loss does not reach the $7.5B attachment point for the 100% layer. | FALSE | Attempted to calculate a recovery in the third layer. |  |
| 10 | Calculates the Total Final Recovery amount as $2,225,000,000. | Critical | Prompt Text | $800M (Layer 1\) \+ $1.425B (Layer 2\) \= $2.225B. | FALSE | Incorrect sum due to failure in previous layering steps. | 3 |

#### **4\. Model Analysis**

Models frequently fail to process banded Excess of Loss treaties correctly. They routinely apply a single participation rate to the entire gross loss, rather than isolating the specific dollar amounts that pierce each respective attachment point and applying the distinct co-participation percentages to those tranches. A model might see the 95% figure and attempt to apply it to the entire $2.5 billion excess of the $4.0 billion retention, resulting in massive hallucinations in recovery calculations.

#### **5\. Golden Response**

**TREATY RECOVERY CALCULATION**

**DATE:** February 1, 2025

**SUBJECT:** Corporate Catastrophe Excess-of-Loss Reinsurance Recovery

Based on the Corporate Catastrophe Excess-of-Loss Reinsurance Treaty terms on PDF Page 31, our recovery for the $6,500,000,000 gross hurricane loss is calculated in banded layers. The treaty provides varying co-participation recovery percentages as the loss escalates.

**Base Retention:**

The company retains the first **$4,000,000,000** of the gross loss.

**Layer 1 ($4.0B to $5.0B):**

* **Total Capacity of Layer:** $1,000,000,000  
* **Amount of Loss in Layer:** $1,000,000,000  
* **Recovery Percentage:** 80%  
* **Layer 1 Recovery:** $1,000,000,000 ![][image1] 0.80 \= **$800,000,000**

**Layer 2 ($5.0B to $7.5B):**

* **Total Capacity of Layer:** $2,500,000,000  
* **Amount of Loss in Layer:** The gross loss is $6.5B. Subtracting the $5.0B exhausted by the retention and Layer 1 leaves **$1,500,000,000** subject to this layer.  
* **Recovery Percentage:** 95%  
* **Layer 2 Recovery:** $1,500,000,000 ![][image1] 0.95 \= **$1,425,000,000**

**Layer 3 ($7.5B to $8.0B):**

* **Recovery Percentage:** 100%  
* **Layer 3 Recovery:** $0. (The gross loss of $6.5B did not reach the $7.5B attachment point for this layer).

**Total Final Recovery:**

* $800,000,000 (Layer 1\) \+ $1,425,000,000 (Layer 2\) \= **$2,225,000,000**

### ---

**Example 4: Cyber Insurance Incident Response & Sub-limit Adjudication**

#### **1\. Metadata**

* **Task Type:** Workflow  
* **Category / Domain:** Claims Handling  
* **Workflow:** Quantum Calculation  
* **Prompt Type:** Coverage Determination and Settlement Memo  
* **Difficulty:** High

#### **2\. Prompt**

You are a Cyber Insurance Claims Adjuster. The current date is March 1, 2020\.

You are evaluating a claim submitted by the City of Franklin following a severe ransomware attack that crippled their municipal network. The city has submitted invoices and demands for reimbursement under their Cyber Insurance policy. You must review the damages, compare them to the explicit sub-limits and the overarching Policy Aggregate Limit of Liability, and calculate the final payable amount by the insurer. Present your findings in a formal Settlement Memo. Ignore any retentions/deductibles for the purpose of this calculation.

**Loss Details Submitted by Insured:**

* The City incurred $700,000 in expenses to restore their digital assets and recover lost business income (Business Income and Digital Asset Restoration).  
* The City paid a $600,000 ransom to the attackers (Cyber Extortion).  
* The City spent $200,000 on forensics, legal counsel, and public relations (Security Breach Response Coverage).

**Attached Files (Context):**

8. **Franklin\_Cyber\_Policy\_Decs.pdf** – Public URL: https://www.franklin.in.gov/egov/documents/1568056135\_75369.pdf

#### **3\. Rubric**

| \# | Description | Weight | Sources (with exact PDF pages) | Justification | Met | Failure Reasoning | Dependent Criteria |
| :---- | :---- | :---- | :---- | :---- | :---- | :---- | :---- |
| 1 | Identifies the overall Aggregate Limit of Liability for the policy as $1,000,000. | Critical | Franklin\_Cyber\_Policy\_Decs.pdf, PDF Page 2 | The declarations page explicitly sets the maximum aggregate limit at $1,000,000. | FALSE | Missed the overriding cap for the policy. |  |
| 2 | Identifies the sub-limit for Business Income and Digital Asset Restoration as $1,000,000. | Major | Franklin\_Cyber\_Policy\_Decs.pdf, PDF Page 2 | Listed under Item 3, Coverage G. | FALSE | Hallucinated a different sub-limit. |  |
| 3 | Identifies the sub-limit for Cyber Extortion as $1,000,000. | Major | Franklin\_Cyber\_Policy\_Decs.pdf, PDF Page 2 | Listed under Item 3, Coverage F. | FALSE | Hallucinated a different sub-limit. |  |
| 4 | Identifies the sub-limit for Security Breach Response Coverage as $1,000,000. | Major | Franklin\_Cyber\_Policy\_Decs.pdf, PDF Page 2 | Listed under Item 3, Coverage C. | FALSE | Hallucinated a different sub-limit. |  |
| 5 | Calculates the total incurred eligible losses across all categories as $1,500,000. | Major | Prompt Text | $700k \+ $600k \+ $200k \= $1.5M. | FALSE | Failed basic arithmetic aggregation of the submitted losses. |  |
| 6 | Determines that all individual categorical losses are fully eligible because they do not exceed their respective $1M sublimits. | Major | Franklin\_Cyber\_Policy\_Decs.pdf, PDF Page 2 | $700k, $600k, and $200k are all individually under the $1M caps. | FALSE | Truncated one of the sub-limits incorrectly. | 6 |
| 7 | Applies the overriding rule that the combined total payout cannot exceed the Aggregate Limit of Liability. | Critical | Franklin\_Cyber\_Policy\_Decs.pdf, PDF Page 2 | The sum of all sub-limit claims ($1.5M) cannot exceed the $1M aggregate policy cap. | FALSE | Attempted to pay out the sum of the sub-limits ($1.5M) ignoring the aggregate cap. | 1 |
| 8 | Calculates the final total payable amount as exactly $1,000,000. | Critical | Prompt Text; Franklin\_Cyber\_Policy\_Decs.pdf, PDF Page 2 | The total eligible claim ($1.5M) is truncated down to the maximum aggregate limit of $1M. | FALSE | Paid an amount greater than the policy's absolute maximum limit. | 7 |
| 9 | Formats output as a formal claims settlement memo. | Minor | Prompt Text | Narrative styling requirement. | FALSE | Output only a numeric value with no prose. |  |
| 10 | Includes the date March 1, 2020\. | Minor | Prompt Text | Temporal anchor. | FALSE | Omitted the date. |  |

#### **4\. Model Analysis**

Cyber insurance policies frequently utilize a structure where multiple individual coverage sub-limits equal the total policy aggregate limit, creating a trap for basic extraction logic. A standard LLM is expected to score between thirty and fifty percent on this rubric. The model will successfully extract the one-million-dollar sub-limits for Extortion, Breach Response, and Digital Asset Restoration. However, because the incurred losses cross multiple categories, models will typically add the eligible amounts together (e.g., $700k BI \+ $600k extortion \+ $200k response \= $1.5M) and recommend paying the total sum. The model fails to recognize the hierarchical relationship between "Per Claim Sublimits" and the overriding "Aggregate Limit of Liability." Understanding that the entire policy maxes out at $1,000,000 regardless of how many individual sub-limits are triggered requires a complex synthesis of insurance contract architecture that base models lack.

#### **5\. Golden Response**

**SETTLEMENT MEMORANDUM**

**TO:** Cyber Claims File – City of Franklin

**FROM:** Cyber Insurance Claims Adjuster

**DATE:** March 1, 2020

**SUBJECT:** Coverage Determination and Final Quantum – Ransomware Incident

**1\. Incident Overview**

The insured, the City of Franklin, sustained a severe ransomware attack resulting in network encrypton and extortion demands. The insured has submitted invoices totaling $1,500,000 across three coverage categories: Business Income/Digital Asset Restoration, Cyber Extortion, and Security Breach Response.

**2\. Sub-Limit Adjudication**

Per the Declarations Page (Item 3), each specific coverage grant is subject to its own sub-limit.

* **Business Income and Digital Asset Restoration (Coverage G):** The insured incurred $700,000. This is within the $1,000,000 Per Claim Sublimit for this coverage, so the full $700,000 is eligible.  
* **Cyber Extortion (Coverage F):** The insured incurred $600,000. This is within the $1,000,000 sub-limit, so the full $600,000 is eligible.  
* **Security Breach Response (Coverage C):** The insured incurred $200,000. This is within the $1,000,000 sub-limit, so the full $200,000 is eligible.  
* **Total Eligible Under Sub-Limits:** $700,000 \+ $600,000 \+ $200,000 \= **$1,500,000**.

**3\. Aggregate Limit Application**

While the sum of the eligible claims under the individual sub-limits equals $1,500,000, all payments are subject to the overarching Policy Aggregate Limit of Liability. Item 3(A) of the Declarations explicitly states that the Aggregate Limit of Liability for the entire policy is **$1,000,000**.

**4\. Final Quantum**

Because the total eligible damages ($1,500,000) exceed the absolute maximum limit of the policy, the final payout must be truncated. The insurer's maximum exposure is reached.

**Final Payable Amount:** **$1,000,000**

### ---

**Example 5: General Liability "Expected or Intended Injury" Exception**

#### **1\. Metadata**

* **Task Type:** Workflow  
* **Category / Domain:** Claims Handling  
* **Workflow:** Coverage Verification  
* **Prompt Type:** Claims Determination Memo  
* **Difficulty:** High

#### **2\. Prompt**

You are a Commercial General Liability Claims Adjuster. The current date is May 10, 2022\.

A patron at the insured's restaurant was injured when the restaurant owner (the insured) intentionally tackled the patron to the ground. The insured tackled the patron because the patron had pulled out a knife and was threatening another customer. The tackle broke the patron's arm, and the patron is now suing the restaurant for $25,000 in bodily injury damages.

The insured has filed a claim under their standard CGL policy (ISO CG 00 01\) to cover the $25,000. Review the attached ISO CG 00 01 form, specifically Exclusion "a" under Section I \- Coverage A.

Based *only* on the exact wording of this exclusion and its exceptions, determine if the policy covers this $25,000 bodily injury lawsuit. Present your findings in a Claims Determination Memo.

**Attached Files (Context):**

* **cg-00-01-01-96.pdf** – Public URL: https://ogs.ny.gov/system/files/documents/2021/09/cg-00-01-01-96.pdf

#### **3\. Rubric**

| \# | Description | Weight | Sources (with exact PDF pages) | Justification | Met | Failure Reasoning | Dependent Criteria |
| :---- | :---- | :---- | :---- | :---- | :---- | :---- | :---- |
| 1 | Identifies Exclusion "a" as "Expected Or Intended Injury". | Minor | cg-00-01-01-96.pdf, PDF Page 1 | Locates the specific exclusion governing intentional acts. | FALSE | Hallucinated a different exclusion. |  |
| 2 | Extracts the rule that coverage is excluded for bodily injury expected or intended from the standpoint of the insured. | Major | cg-00-01-01-96.pdf, PDF Page 1 | Exact wording of the main exclusion. | FALSE | Misinterpreted the exclusion's intent. | 1 |
| 3 | Acknowledges that tackling someone is an intentional act causing expected/intended injury. | Major | Prompt Text | Synthesizes the facts (the prompt explicitly states it was an "intentional" tackle). | FALSE | Argued the tackle was an accident. |  |
| 4 | Identifies the explicit exception to Exclusion "a" regarding "reasonable force". | Critical | cg-00-01-01-96.pdf, PDF Page 1 | The exclusion does not apply to bodily injury resulting from the use of reasonable force to protect persons or property. | FALSE | Missed the "reasonable force" carve-back entirely. | 1 |
| 5 | Evaluates the prompt facts (stopping a knife attack) against the "reasonable force" exception. | Major | Prompt Text | Protecting another customer from a knife meets the threshold for protecting persons. | FALSE | Failed to apply the prompt's context to the exception. | 3 |
| 6 | Concludes that the "Expected Or Intended Injury" exclusion does NOT apply to this specific incident. | Critical | cg-00-01-01-96.pdf, PDF Page 1 | The exception overrides the exclusion. | FALSE | Applied the exclusion and denied the claim. | 3 |
| 7 | Determines that the $25,000 claim IS covered (subject to limits). | Critical | Prompt Text | The final claims determination. | FALSE | Denied the claim. | 5 |
| 8 | Does not hallucinate an arbitrary deductible for Liability coverage. | Minor | Insurance Principles | Standard CGL Liability (Coverage A) does not carry a deductible unless endorsed. | FALSE | Deducted a random amount from the $25,000. |  |
| 9 | Formats the response as a formal Claims Determination Memo. | Minor | Prompt Text | Required output styling. | FALSE | Wrote an unstructured paragraph. |  |
| 10 | Includes the date May 10, 2022\. | Minor | Prompt Text | Temporal anchor. | FALSE | Omitted the date. |  |

#### **4\. Model Analysis**

State-of-the-art LLMs frequently struggle with the legal nuances of "exceptions to exclusions." When an LLM reads that the insured "intentionally tackled" someone, its semantic matching heavily biases toward the "Expected or Intended Injury" exclusion, leading the model to summarily deny the claim based on the concept that insurance does not cover intentional torts. A highly capable model must read past the first sentence of the exclusion to locate the critical exception clause: "This exclusion does not apply to 'bodily injury' resulting from the use of reasonable force to protect persons or property." Failing to apply this exception to the self-defense scenario causes the model to incorrectly deny a perfectly valid $25,000 claim.

#### **5\. Golden Response**

**CLAIMS DETERMINATION MEMO**

**TO:** CGL Claims File

**FROM:** Commercial General Liability Claims Adjuster

**DATE:** May 10, 2022

**SUBJECT:** Coverage Determination – Intentional Act and Self-Defense

**1\. Claim Overview**

The insured restaurant owner is facing a $25,000 bodily injury lawsuit after intentionally tackling a patron who had brandished a knife and was threatening another customer. The claim is being evaluated under Section I \- Coverage A of the ISO CG 00 01 policy.

**2\. Policy Language and Application**

Because the insured intentionally tackled the patron, we must review the policy's exclusions regarding intentional acts. According to PDF Page 1 of the attached CG 00 01 form, **Exclusion a. Expected Or Intended Injury** applies. This provision explicitly excludes coverage for:

* "'Bodily injury' or 'property damage' expected or intended from the standpoint of the insured."

However, this exclusion contains a critical carve-back exception. The policy explicitly states:

* "This exclusion does not apply to 'bodily injury' resulting from the use of reasonable force to protect persons or property."

**3\. Coverage Determination**

While the act of tackling the patron was intentional and the resulting broken arm was an expected/intended injury from the standpoint of the insured, the insured's actions were taken specifically to protect another customer from a knife threat. This constitutes the use of reasonable force to protect a person.

Therefore, the exception to Exclusion "a" is triggered, meaning the exclusion does not apply. The $25,000 bodily injury claim **IS COVERED** under the policy, subject to the policy's occurrence limits.

### ---

**Example 6: ALTA 32-06 Construction Loan / Mechanic's Lien**

#### **1\. Metadata**

* **Task Type:** Capability  
* **Category / Domain:** Underwriting  
* **Workflow:** Policy Issuance  
* **Prompt Type:** Underwriting Brief  
* **Difficulty:** High

#### **2\. Prompt**

You are a Title Insurance Underwriter. The current date is October 1, 2021\.

A lender has requested the ALTA 32-06 (Construction Loan) endorsement for a $5,000,000 commercial construction project. Before issuing the endorsement, the lender wants to confirm exactly how it protects them. They present the following hypothetical scenario:

"We disburse a $500,000 construction draw on November 1\. The documents supporting this draw only designate payment for concrete and lumber. However, the general contractor fails to pay their structural steel supplier. On December 1, the unpaid steel supplier files a $100,000 Mechanic's Lien against the property."

Based strictly on the exclusions defined in the attached ALTA 32-06 endorsement, write a brief explaining whether the ALTA 32-06 endorsement would protect the lender's priority against this specific $100,000 steel supplier's Mechanic's Lien.

**Attached Files (Context):**

* **Endorsement-32-06-Construction-Loan.pdf** – Public URL: https://nntg.com/wp-content/uploads/2019/09/Endorsement-32-06-Construction-Loan.pdf

#### **3\. Rubric**

| \# | Description | Weight | Sources (with exact PDF pages) | Justification | Met | Failure Reasoning | Dependent Criteria |
| :---- | :---- | :---- | :---- | :---- | :---- | :---- | :---- |
| 1 | Identifies the ALTA 32-06 provides coverage against the lack of priority over Mechanic's Liens. | Minor | Endorsement-32-06-Construction-Loan.pdf, PDF Page 1 | Identifies the core insuring provision of Section 3.c. | FALSE | Stated the endorsement does not cover mechanic's liens. |  |
| 2 | Identifies the critical condition that the charges must be "designated for payment" in the supporting documents. | Critical | Endorsement-32-06-Construction-Loan.pdf, PDF Page 1 | The coverage in Section 3.c only extends "to the extent that the charges... were designated for payment in the documents supporting a Construction Loan Advance." | FALSE | Missed the "designated for payment" caveat. |  |
| 3 | Evaluates the prompt's facts against this condition. | Major | Prompt Text | Synthesizes the fact that the steel was NOT designated in the draw documents. | FALSE | Ignored the prompt's stipulation about what was in the draw documents. | 6 |
| 4 | Identifies Exclusion 4.b. regarding services not designated for payment. | Major | Endorsement-32-06-Construction-Loan.pdf, PDF Page 2 | Explicitly excludes mechanic's liens for services not designated for payment. | FALSE | Failed to locate Exclusion 4.b. |  |
| 5 | Concludes definitively that the ALTA 32-06 does NOT protect the lender against this specific $100,000 steel lien. | Critical | Prompt Text | The final underwriting determination. | FALSE | Assured the lender they were covered. | 2 |
| 6 | Explains the rationale that the lender is only protected for the specific line items they funded. | Major | Title Insurance Principles | Demonstrates deep comprehension of the endorsement's mechanics. | FALSE | Provided a yes/no answer with no explanation. | 4 |
| 7 | Does not hallucinate that ALTA 32-06 provides "blanket" mechanic's lien coverage. | Major | Title Insurance Principles | The 32-06 is explicitly designed to avoid blanket forward-looking coverage. | FALSE | Implied the endorsement covers all future liens. | 4 |
| 8 | Formats the response as an Underwriting Brief. | Minor | Prompt Text | Required output styling. | FALSE | Wrote a casual email. |  |
| 9 | Includes the date October 1, 2021\. | Minor | Prompt Text | Temporal anchor. | FALSE | Omitted the date. |  |
| 10 | Uses professional title and escrow terminology. | Minor | Prompt Text | Tone alignment. | FALSE | Used unprofessional language. |  |

#### **4\. Model Analysis**

Mechanic's lien coverage during construction is highly restricted in title insurance. Base LLMs frequently read the title of the ALTA 32-06 endorsement ("Loss of Priority") and assume it provides blanket protection against all mechanic's liens filed after the loan is recorded. The model must parse the dense legal syntax of Section 3.c and Exclusion 4.b on PDF Pages 1 and 2 to realize that the endorsement only protects the lender against liens for which the lender *explicitly disbursed funds* based on designated supporting documents. Because the steel was not designated in the draw documents, the model must successfully deny coverage for the hypothetical scenario.

#### **5\. Golden Response**

**TITLE UNDERWRITING BRIEF**

**DATE:** October 1, 2021

**SUBJECT:** ALTA 32-06 Mechanic's Lien Coverage Applicability

**Overview of ALTA 32-06 Coverage**

The ALTA 32-06 (Construction Loan – Loss of Priority) endorsement does provide protection against the lack of priority of the Insured Mortgage over a Mechanic's Lien. However, this is not blanket, forward-looking coverage for all liens filed on the project.

According to Section 3.c on PDF Page 1, coverage only applies "to the extent that the charges for the services, labor, materials or equipment for which the Mechanic's Lien is claimed were designated for payment in the documents supporting a Construction Loan Advance."

Furthermore, Section 4.b on PDF Page 2 explicitly excludes from coverage any Mechanic's Lien arising from materials or equipment "to the extent that the Mechanic's Lien claimant's charges... were not designated for payment in the documents supporting a Construction Loan Advance."

**Application to the Hypothetical Scenario**

In your scenario, the November 1 draw documents only designated payment for concrete and lumber. The structural steel was never designated for payment in the documents supporting that advance.

Therefore, if the unpaid steel supplier files a $100,000 Mechanic's Lien on December 1, the ALTA 32-06 endorsement **will not** protect your priority against that specific lien. The endorsement limits the title insurer's liability strictly to the line items the lender has actually reviewed and funded.

### ---

**Example 7: Medicare Secondary Payer (ESRD 30-Month Coordination)**

#### **1\. Metadata**

* **Task Type:** Workflow  
* **Category / Domain:** Compliance  
* **Workflow:** Benefit Coordination  
* **Prompt Type:** Compliance Adjudication Report  
* **Difficulty:** High

#### **2\. Prompt**

You are a Medicare Compliance Analyst. The current date is December 10, 2023\.

An employee covered by their employer's Group Health Plan (GHP) was diagnosed with End-Stage Renal Disease (ESRD). They began a regular course of dialysis on January 1, 2021\.

The employee incurred a massive $150,000 hospital bill for ESRD-related treatments on October 15, 2023\. The hospital billed Medicare as the primary payer, and Medicare paid the bill.

Using the attached CMS Medicare Secondary Payer (MSP) Manual, determine the exact start and end dates of the 30-month coordination period. Based on those dates, determine if Medicare was legally the primary payer for the October 15, 2023 claim, or if the GHP should have paid primary. Present your findings in a Compliance Adjudication Report.

**Attached Files (Context):**

* **msp105c02.pdf** – Public URL: https://www.cms.gov/regulations-and-guidance/guidance/manuals/downloads/msp105c02.pdf

#### **3\. Rubric**

| \# | Description | Weight | Sources (with exact PDF pages) | Justification | Met | Failure Reasoning | Dependent Criteria |
| :---- | :---- | :---- | :---- | :---- | :---- | :---- | :---- |
| 1 | Locates the rules for the Duration of the Coordination Period (Section 20.1.1). | Minor | msp105c02.pdf, PDF Page 11 | Locates the specific rules governing ESRD Medicare Secondary Payer timelines. | FALSE | Used working-aged or disability rules instead. |  |
| 2 | Extracts the rule that Medicare entitlement usually begins with the third month *after* the month dialysis starts. | Critical | msp105c02.pdf, PDF Page 11 | "Medicare entitlement usually begins with the third month after the month in which the individual starts a regular course of dialysis." | FALSE | Assumed entitlement began on the exact date dialysis started (Jan 1). |  |
| 3 | Calculates the start date of the 30-month coordination period as April 1, 2021\. | Critical | Prompt Text; Manual | Jan 1 (dialysis) \-\> Feb (1), Mar (2), Apr (3). April 1 is the start of the 3rd month after. | FALSE | Calculated the start date as Jan 1, 2021\. | 6 |
| 4 | Calculates the end date of the 30-month coordination period as September 30, 2023\. | Major | Prompt Text | 30 months from April 1, 2021\. | FALSE | Calculated the end date based on a Jan 1 start (ending June 2023). | 2 |
| 5 | Identifies the date of the claim as October 15, 2023\. | Minor | Prompt Text | Stated in prompt. | FALSE | Missed the claim date. |  |
| 6 | Compares the claim date (Oct 15\) to the coordination period end date (Sept 30). | Major | Prompt Text | The claim occurred *after* the 30-month period expired. | FALSE | Concluded the claim fell inside the 30-month window. | 3 |
| 7 | Extracts the rule that Medicare becomes the primary payer after the 30th month of ESRD entitlement. | Major | msp105c02.pdf, PDF Page 11 | "Medicare becomes the primary payer after the 30th month of ESRD-based eligibility/entitlement." | FALSE | Assumed the GHP is always primary. |  |
| 8 | Concludes definitively that Medicare WAS legally the primary payer for the October 15, 2023 claim. | Critical | Prompt Text | Final compliance adjudication. | FALSE | Concluded the GHP should have paid primary. | 5 |
| 9 | Formats the response as a Compliance Adjudication Report. | Minor | Prompt Text | Required output styling. | FALSE | Wrote an unstructured paragraph. |  |
| 10 | Includes the date December 10, 2023\. | Minor | Prompt Text | Temporal anchor. | FALSE | Omitted the date. |  |

#### **4\. Model Analysis**

The Medicare Secondary Payer (MSP) rules for End-Stage Renal Disease are a notorious trap for healthcare administrators and AI models alike. A base LLM will read "30-month coordination period," see that dialysis started on January 1, 2021, and simply add 30 months to arrive at an end date of June 30, 2023\. Because the claim occurred in October, the model might accidentally get the final answer right, but its logic will be fatally flawed. To score perfectly, the model must read Section 20.1.1 on PDF Page 11, which dictates a 3-month waiting period: the coordination period does not start until the third month *after* the month dialysis begins (April 1, 2021). The model must accurately calculate 30 months from April 1 (ending Sept 30, 2023), proving that the October 15 claim fell exactly 15 days after Medicare became primary.

#### **5\. Golden Response**

**COMPLIANCE ADJUDICATION REPORT**

**DATE:** December 10, 2023

**SUBJECT:** MSP ESRD 30-Month Coordination Period Analysis

**1\. Applicable CMS Guidelines**

This claim is governed by the Medicare Secondary Payer (MSP) rules for End-Stage Renal Disease (ESRD) beneficiaries. According to Section 20.1.1 (Duration of Coordination Period) on PDF Page 11 of the CMS MSP Manual (Pub. 100-05, Chapter 2), an individual is subject to a 30-month coordination period during which the employer's Group Health Plan (GHP) is the primary payer.

Crucially, the manual states that this 30-month coordination period begins with the first month of eligibility/entitlement to Medicare Part A. Furthermore, "Medicare entitlement usually begins with the third month after the month in which the individual starts a regular course of dialysis."

**2\. Timeline Calculation**

* **Dialysis Start Date:** January 1, 2021\.  
* **Medicare Entitlement/Coordination Start Date:** April 1, 2021 (The third month after the month dialysis started).  
* **Coordination Period Duration:** 30 months.  
* **Coordination Period End Date:** September 30, 2023\.

**3\. Adjudication and Conclusion**

The medical claim in question was incurred on October 15, 2023\.

Because the 30-month coordination period expired on September 30, 2023, the GHP was no longer the primary payer. The CMS manual explicitly states: "Medicare becomes the primary payer after the 30th month of ESRD-based eligibility/entitlement."

Therefore, **Medicare was legally the primary payer** for the October 15, 2023 hospital bill, and the hospital billed the correct entity.

### ---

**Example 8: Workers' Compensation Dual Capacity Doctrine**

#### **1\. Metadata**

* **Task Type:** Capability  
* **Category / Domain:** Claims Handling  
* **Workflow:** Subrogation & Legal Defense  
* **Prompt Type:** Legal Brief  
* **Difficulty:** High

#### **2\. Prompt**

You are a Workers' Compensation Claims Examiner. The current date is September 1, 2021\.

A nurse employed by a hospital injured her back while lifting a patient. She went to the emergency room located within the same hospital where she works. The emergency room physician (also employed by the hospital) negligently administered an epidural injection, causing severe nerve damage and paralysis.

The nurse is receiving workers' compensation benefits for the initial back injury. However, she has now filed a medical malpractice civil lawsuit against her employer (the hospital) for the paralysis caused by the negligent injection.

The hospital's defense attorney claims the lawsuit must be dismissed immediately due to the "Exclusive Remedy" rule of workers' compensation. Using the attached legal compendium on Workers' Compensation Law, evaluate this defense. Determine if the nurse is legally permitted to sue her employer for malpractice in this specific scenario, and identify the exact legal doctrine that applies. Present your findings in a Legal Brief.

**Attached Files (Context):**

* **Compendium-Chp-12-Major-Issues-of-Workmens-Compensation-Law.pdf** – Public URL: https://ruwoco.rutgers.edu/wp-content/uploads/2021/03/Compendium-Chp-12-Major-Issues-of-Workmens-Compensation-Law.pdf

#### **3\. Rubric**

| \# | Description | Weight | Sources (with exact PDF pages) | Justification | Met | Failure Reasoning | Dependent Criteria |
| :---- | :---- | :---- | :---- | :---- | :---- | :---- | :---- |
| 1 | Acknowledges the general "Exclusive Liability" or "Exclusive Remedy" rule. | Minor | Prompt Text | Contextualizes the defense attorney's argument. | FALSE | Ignored the exclusive remedy concept. |  |
| 2 | Locates the "Dual-capacity doctrine" section. | Critical | Compendium-Chp-12.pdf, PDF Page 5 | The specific legal carve-out that applies to this exact scenario. | FALSE | Failed to find the dual-capacity doctrine. |  |
| 3 | Extracts the rule that treats employer-physicians as having "split personalities" or a "separate existence." | Major | Compendium-Chp-12.pdf, PDF Page 5 | Explains the legal mechanism of the doctrine. | FALSE | Misinterpreted how the doctrine views the employer. | 6 |
| 4 | Applies the facts to the rule: The hospital acted as the employer for the back injury, but acted as a medical provider for the injection. | Major | Prompt Text | Synthesizes the prompt facts with the legal rule. | FALSE | Failed to separate the two roles of the hospital. | 2 |
| 5 | Concludes definitively that the nurse IS permitted to sue the hospital for malpractice. | Critical | Prompt Text; Compendium-Chp-12.pdf | The final legal determination requested by the prompt. | FALSE | Agreed with the defense attorney that the suit is barred. | 6 |
| 6 | Explains that this doctrine creates an exception to the exclusive liability rule. | Major | Compendium-Chp-12.pdf, PDF Page 5 | Demonstrates comprehension of why the defense attorney is wrong. | FALSE | Provided a yes/no answer without explaining the exception. | 4 |
| 7 | Does not hallucinate that this is a "Third Party Over Suit." | Minor | Insurance Principles | Differentiates between similar but legally distinct concepts on the same page. | FALSE | Confused dual capacity with third-party over actions. |  |
| 8 | Formats the response as a formal Legal Brief. | Minor | Prompt Text | Required output styling. | FALSE | Wrote a casual email. |  |
| 9 | Uses professional legal and claims terminology. | Minor | Prompt Text | Tone alignment. | FALSE | Used unprofessional language. |  |
| 10 | Includes the date September 1, 2021\. | Minor | Prompt Text | Temporal anchor. | FALSE | Omitted the date. |  |

#### **4\. Model Analysis**

The "Exclusive Remedy" provision is the bedrock of Workers' Compensation law—employees cannot sue their employers for workplace injuries. LLMs are heavily trained on this fundamental concept and will almost universally agree with the hospital's defense attorney, concluding that the nurse's malpractice suit must be dismissed because her exclusive remedy is through the workers' comp system. To succeed, the model must locate a highly specific, niche legal carve-out on PDF Page 5 of the attached compendium: the "Dual-capacity doctrine." It must recognize that because the hospital stepped outside its role as an employer and assumed the role of a medical provider, it can be sued for malpractice just like any third-party doctor, completely bypassing the exclusive remedy shield.

#### **5\. Golden Response**

**LEGAL BRIEF**

**DATE:** September 1, 2021

**SUBJECT:** Applicability of Exclusive Remedy vs. Dual-Capacity Doctrine

**1\. Factual Overview**

The claimant, a hospital nurse, sustained a compensable workplace back injury. Subsequently, an emergency room physician employed by the same hospital negligently administered an epidural, causing paralysis. The claimant has filed a medical malpractice lawsuit against her employer. The hospital asserts the suit is barred by the Workers' Compensation Exclusive Remedy rule.

**2\. Legal Analysis**

As a general principle, Workers' Compensation provides the exclusive liability remedy for employees injured in the course and scope of their employment, shielding the employer from civil lawsuits.

However, PDF Page 5 of the attached legal compendium outlines a critical exception to this rule: the **Dual-capacity doctrine**.

The compendium explicitly uses this exact scenario as the defining example of the doctrine: "An employee working for a physician may suffer a compensable injury and receive negligent medical treatment from his employer. For purposes of a malpractice action, the employer is treated as having a separate existence, apart from the employment relationship, and is looked at strictly as a physician providing medical care to a patient."

**3\. Conclusion**

When the hospital treated the claimant in its emergency room, it was operating in a "dual capacity" with a "separate existence" as a medical provider, rather than strictly as her employer. Therefore, the hospital remains open to a civil suit for the negligent medical care.

The defense attorney's reliance on the Exclusive Remedy rule is incorrect in this specific circumstance. **The nurse is legally permitted to sue her employer for medical malpractice.**

### ---

**Example 9: Ordinance or Law (Increased Cost of Construction)**

#### **1\. Metadata**

* **Task Type:** Workflow  
* **Category / Domain:** Claims Handling  
* **Workflow:** Quantum Calculation  
* **Prompt Type:** Claims Apportionment Memo  
* **Difficulty:** High

#### **2\. Prompt**

You are a Commercial Property Claims Adjuster. The current date is June 5, 2022\.

A commercial building owned by your insured suffered a massive fire. The building's Replacement Cost Value (RCV) is $1,000,000. The fire caused $600,000 in direct physical damage to the structure.

Because the building was more than 50% destroyed, a local city ordinance requires the entire building to be brought up to current code. This requires two things:

1. The undamaged $400,000 portion of the building must be completely demolished and cleared (The cost to execute the demolition is $50,000).  
2. The new building must be constructed with an automatic sprinkler system, which was not part of the original structure (The cost to add the sprinkler system is $100,000).

The insured has a CP 00 10 policy with a $1,000,000 Building Limit. They also purchased the attached ISO CP 04 05 "Ordinance or Law Coverage" endorsement, with Coverages A, B, and C triggered. Coverage B & C have a Combined Limit of $250,000.

Using the attached endorsement, calculate the final payout for Coverages A, B, and C.

**Attached Files (Context):**

* **CP-04-05-09-17-Ordinance-or-Law-Coverage.pdf** – Public URL: https://www.propertyinsurancecoveragelaw.com/wp-content/uploads/2023/12/CP-04-05-09-17-Ordinance-or-Law-Coverage.pdf

#### **3\. Rubric**

| \# | Description | Weight | Sources (with exact PDF pages) | Justification | Met | Failure Reasoning | Dependent Criteria |
| :---- | :---- | :---- | :---- | :---- | :---- | :---- | :---- |
| 1 | Allocates the $600,000 direct physical fire damage to the base building limit, not the endorsement. | Major | Property Claims Principles | Base damage is covered by the CP 00 10, the endorsement covers the ordinance impacts. | FALSE | Placed the fire damage under Coverage A. |  |
| 2 | Identifies Coverage A covers the loss to the undamaged portion of the building. | Minor | CP-04-05-09-17-Ordinance-or-Law-Coverage.pdf, PDF Page 1 | Identifies the function of Coverage A. | FALSE | Confused Coverage A with Demolition. |  |
| 3 | Calculates the payout under Coverage A as $400,000. | Critical | Prompt Text | The value of the undamaged portion required to be destroyed. | FALSE | Failed to pay for the undamaged portion. | 6 |
| 4 | Identifies Coverage B covers the cost to demolish the undamaged parts of the building. | Minor | CP-04-05-09-17-Ordinance-or-Law-Coverage.pdf, PDF Page 2 | Identifies the function of Coverage B. | FALSE | Confused Coverage B with construction costs. |  |
| 5 | Allocates the $50,000 demolition cost to Coverage B. | Major | Prompt Text | Maps the prompt fact to the correct bucket. | FALSE | Missed the demolition cost. | 3 |
| 6 | Identifies Coverage C covers the increased cost to reconstruct the building to comply with the ordinance. | Minor | CP-04-05-09-17-Ordinance-or-Law-Coverage.pdf, PDF Page 2 | Identifies the function of Coverage C. | FALSE | Denied the sprinkler system. |  |
| 7 | Allocates the $100,000 sprinkler system cost to Coverage C. | Major | Prompt Text | Maps the prompt fact to the correct bucket. | FALSE | Missed the sprinkler cost. | 5 |
| 8 | Verifies that the sum of Coverage B & C ($150,000) does not exceed the $250,000 combined limit. | Critical | Prompt Text | Applies the endorsement's combined sub-limit. | FALSE | Stated the limit was exceeded. | 4 |
| 9 | Calculates the Total Claim Payout as $1,150,000. | Critical | Prompt Text | $600k (Base) \+ $400k (Cov A) \+ $50k (Cov B) \+ $100k (Cov C). | FALSE | Math error or failed to combine base damage with the endorsement payouts. | 1 |
| 10 | Includes the date June 5, 2022\. | Minor | Prompt Text | Temporal anchor. | FALSE | Omitted the date. |  |

#### **4\. Model Analysis**

Ordinance or Law coverage is a highly complex, multi-bucket property endorsement. Base LLMs will typically look at the $1,000,000 base limit and state that the insurer only owes $600,000 for the fire, incorrectly applying the Ordinance or Law exclusion from the main policy. Alternatively, they will confuse the buckets, placing the $50k demolition cost in the base policy's standard "Debris Removal" extension rather than Coverage B, or placing the $400k undamaged property loss in Coverage B instead of Coverage A. The model must accurately parse the endorsement on PDF Pages 1 and 2 to realize that Coverage A pays for the *value* of the unburnt wood, Coverage B pays for the bulldozers to knock it down, and Coverage C pays for the new sprinklers—and that the total payout will actually exceed the $1M face value of the policy.

#### **5\. Golden Response**

**CLAIMS APPORTIONMENT MEMO**

**TO:** Commercial Property Claims File

**FROM:** Claims Adjuster

**DATE:** June 5, 2022

**SUBJECT:** Ordinance or Law (CP 04 05\) Claim Calculation

Based on the facts of the loss and the application of the ISO CP 04 05 Ordinance or Law Coverage endorsement, the final payout is calculated across four distinct coverage buckets:

**1\. Base Policy (Direct Physical Damage)**

The direct physical damage caused by the covered peril (fire) is paid out of the base policy Building Limit.

* **Base Payout:** $600,000

**2\. Coverage A (Loss to the Undamaged Portion of the Building)**

Because the city ordinance requires the demolition of the undamaged portion of the building, Coverage A responds. As stated on PDF Page 1 of the endorsement, Coverage A pays for the loss in value of the undamaged portion of the building.

* **Coverage A Payout:** $400,000 (The value of the undamaged portion). Note: The $600,000 base payout \+ the $400,000 Coverage A payout exhausts the $1,000,000 base Building Limit.

**3\. Coverage B (Demolition Cost Coverage)**

As stated on PDF Page 2, Coverage B pays the cost to demolish and clear the site of the undamaged parts of the building required by the ordinance.

* **Coverage B Allocation:** $50,000

**4\. Coverage C (Increased Cost of Construction Coverage)**

As stated on PDF Page 2, Coverage C pays the increased cost to reconstruct the building to comply with the minimum standards of the ordinance (in this case, adding the automatic sprinkler system).

* **Coverage C Allocation:** $100,000

**Combined B & C Limit Check:**

The total for Coverages B and C is $150,000 ($50,000 \+ $100,000). This is well within the $250,000 Combined Limit for Coverages B and C purchased by the insured.

**Final Total Claim Payout:**

The total amount payable to the insured is **$1,150,000** ($600,000 Base \+ $400,000 Cov A \+ $50,000 Cov B \+ $100,000 Cov C).

[image1]: <data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAA8AAAAXCAYAAADUUxW8AAAAjUlEQVR4XmNgGAVDEHADsTi6IBJgBGIpIGZGlwABUSBeBcQm6BIMEI0JQDwZiFlRpRBABoh3ALEZkhhRGmEA2QCSNMIAzIApDCRqBAGQjUVA/BqIrdDk8AKQxhwGiI1yQLyeATUMcAJkjTCnSjAQYQBIYxYQT2DA9CNBA7SAuIkBUyMMCAFxF5QeBXQHAMRmESwieeNfAAAAAElFTkSuQmCC>