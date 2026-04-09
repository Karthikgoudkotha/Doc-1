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

1. Casualty Actuarial Society E-Forum, Winter 2019, accessed on April 5, 2026, [https://www.casact.org/sites/default/files/database/forum\_19wforum\_completewinter2019.pdf](https://www.casact.org/sites/default/files/database/forum_19wforum_completewinter2019.pdf)  
2. CP 00 10 10 12 \- Building and Personal Property Coverage Form, accessed on April 5, 2026, [https://www.propertyinsurancecoveragelaw.com/wp-content/uploads/2023/12/CP-00-10-10-12-Building-and-Personal-Property-Coverage-Form.pdf](https://www.propertyinsurancecoveragelaw.com/wp-content/uploads/2023/12/CP-00-10-10-12-Building-and-Personal-Property-Coverage-Form.pdf)  
3. 1.4 Building and Personal Property Coverage Form \- Risk & Insurance Education Alliance, accessed on April 5, 2026, [https://www.riskeducation.org/learn/pluginfile.php/276804/mod\_page/content/1/74%20-%201.4%20Building%20and%20Personal%20Property%20Coverage%20Form.pdf](https://www.riskeducation.org/learn/pluginfile.php/276804/mod_page/content/1/74%20-%201.4%20Building%20and%20Personal%20Property%20Coverage%20Form.pdf)  
4. CAUSES OF LOSS – SPECIAL FORM \- Property Insurance Coverage Law Blog, accessed on April 5, 2026, [https://www.propertyinsurancecoveragelaw.com/wp-content/uploads/2020/04/Policy-1.pdf](https://www.propertyinsurancecoveragelaw.com/wp-content/uploads/2020/04/Policy-1.pdf)  
5. PRO Form \- ABA Insurance Services, accessed on April 5, 2026, [https://www.abais.com/docs/default-source/banks/policy-specimen/property-policy-w-dec-coverage-forms-mandatory-endorsements-exclusions.pdf](https://www.abais.com/docs/default-source/banks/policy-specimen/property-policy-w-dec-coverage-forms-mandatory-endorsements-exclusions.pdf)  
6. Specimen Reinsurance Agreement \- SEC.gov, accessed on April 5, 2026, [https://www.sec.gov/Archives/edgar/data/801019/000119312513170649/d480972dex9926g.htm](https://www.sec.gov/Archives/edgar/data/801019/000119312513170649/d480972dex9926g.htm)  
7. Quota Share Reinsurance Agreement \- SEC.gov, accessed on April 5, 2026, [https://www.sec.gov/Archives/edgar/data/8497/000119312511081483/dex1023.htm](https://www.sec.gov/Archives/edgar/data/8497/000119312511081483/dex1023.htm)

### **09-04-2026**

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

### **10-04-2026**

