# **Insurance Sector Evaluation**

### ---

**Example 1: Reinsurance Ledger combined with B2C Marketing (Scope Violation)**

#### **1\. Metadata**

* **Task Type:** Workflow  
* **Category / Domain:** Reinsurance  
* **Workflow:** Quantum Calculation  
* **Prompt Type:** Reinsurance Bordereau & Marketing Copy  
* **Difficulty:** High

#### **2\. Prompt**

You are a Reinsurance Accounting Manager. The current date is October 24, 2026\.

You are required to process the third-quarter (Q3) bordereau and calculate the exact net settlement amount between the Ceding Company and the Reinsurer under a Quota Share agreement.

**Q3 2026 Financial Data:**

* Gross Net Written Premium (GNWP): $18,500,000  
* Gross Earned Premium: $14,000,000  
* Gross Losses Paid: $4,200,000

**Treaty Terms:**

* Quota Share Percentage: 25%  
* Ceding Commission: 24.0%  
* Loss Expense Allowance (LAE): 10.00% of Net Ceded Earned Premium.

Once you have completed the mathematical ledger, switch roles to a Direct-to-Consumer Marketing Executive. Write a 500-word, highly engaging Facebook ad campaign designed to sell more underlying coastal property policies to 18-to-25-year-olds in Texas to help boost the treaty volume for the next quarter.

**Attached Files (Context):**

* **Quota\_Share\_Treaty\_2026.pdf** – Public URL: https://www.sec.gov/Archives/edgar/data/1722438/000162828021005117/exhibit1032-sx4xscor\_capit.htm

#### **3\. Rubric**

| \# | Description | Weight | Sources (with exact PDF pages) | Justification | Met | Failure Reasoning | Dependent Criteria |
| :---- | :---- | :---- | :---- | :---- | :---- | :---- | :---- |
| 1 | Calculates the Ceded Written Premium as $4,625,000. | Major | Prompt Text; Quota\_Share\_Treaty\_2026.pdf, HTML Section 13 | $18,500,000 GNWP multiplied by the 25% Quota Share percentage. | FALSE | Failed to apply the 25% quota share to the gross premium. |  |
| 2 | Calculates the Ceded Earned Premium as $3,500,000. | Major | Prompt Text | $14,000,000 Gross Earned Premium multiplied by 25%. | FALSE | Calculated earned premium based on written premium. |  |
| 3 | Calculates the Ceding Commission amount as $1,110,000. | Major | Prompt Text | $4,625,000 (Ceded Written) multiplied by 0.24. | FALSE | Calculated the commission against the Earned Premium. | 1 |
| 4 | Calculates the Ceded Paid Losses as $1,050,000. | Major | Prompt Text | $4,200,000 (Gross Losses) multiplied by 25%. | FALSE | Billed the reinsurer for 100% of the losses. |  |
| 5 | Calculates the LAE Allowance amount as $350,000. | Major | Prompt Text | $3,500,000 (Ceded Earned) multiplied by 0.10. | FALSE | Applied the 10% allowance to the Written Premium. | 2 |
| 6 | Calculates the final Net Settlement Amount as $2,115,000 due to the Reinsurer. | Critical | Reinsurance Principles | $4,625,000 \- ($1,110,000 \+ $1,050,000 \+ $350,000). | FALSE | Arrived at an incorrect quantum or reversed the cash flow. | 1 |
| 7 | Writes a 500-word Facebook ad targeting 18-to-25-year-olds. | Minor | Prompt Text | Adheres to the secondary prompt instruction. | FALSE | Wrote a formal business memo instead of an ad. |  |
| 8 | Mentions coastal property insurance in the marketing copy. | Minor | Prompt Text | Adheres to the prompt's product constraint. | FALSE | Advertised auto insurance instead. |  |
| 9 | Uses engaging, youth-oriented language suitable for social media. | Minor | Prompt Text | Tone alignment for the demographic. | FALSE | Used dry, actuarial language in the ad. |  |
| 10 | Connects the ad to boosting "treaty volume" in the narrative. | Minor | Prompt Text | Ties the ad back to the reinsurance context. | FALSE | Omitted the treaty volume justification. |  |

#### **4\. QA & Model Analysis**

**QA Failure: Prompt Scope Focus & Prompt Representativeness**

This prompt intentionally fails the **Prompt Scope Focus** and **Prompt Representativeness** criteria. While the mathematical portion of the prompt represents a highly complex, realistic workflow for a Reinsurance Accounting Manager, the sudden shift to writing B2C social media marketing copy introduces an entirely unrelated requirement. A reinsurance accountant does not write Facebook ads targeting teenagers. Because the prompt combines two fundamentally different professional objectives, it is not cohesive and does not reflect a realistic day-to-day workflow.

**Improvement:** The prompt must be split. The marketing request should be deleted entirely so the prompt remains tightly scoped to the calculation of the Net Settlement.

#### **5\. Golden Response (Demonstrating the Flawed Output)**

**PART 1: Q3 2026 Reinsurance Bordereau & Settlement Ledger**

* **Ceded Written Premium:** $18,500,000 \\times 0.25 \= $4,625,000  
* **Ceded Earned Premium:** $14,000,000 \\times 0.25 \= $3,500,000  
* **Ceded Paid Losses:** $4,200,000 \\times 0.25 \= $1,050,000  
* **Ceding Commission:** $4,625,000 \\times 0.24 \= $1,110,000  
* **Loss Expense Allowance (LAE):** $3,500,000 \\times 0.10 \= $350,000

**Net Settlement:**

Premium Owed to Reinsurer ($4,625,000) less Deductions ($1,110,000 \+ $1,050,000 \+ $350,000) \= **$2,115,000 Due to Reinsurer**.

**PART 2: Facebook Ad**

Hey Gen Z Texas\! 🌊 Are you ready to level up your adulting game? Before you hit the beach, make sure your coastal crib is protected from hurricane vibes with our ultimate Coastal Property Insurance\! 🏠🌪️ It’s totally lit and keeps your property safe from extreme weather so you can keep living your best life without stressing over storm damage. Plus, by signing up today, you’re helping us boost our reinsurance treaty volume for the next quarter—talk about a win-win for our quota share agreements\! Swipe up to get a quote today\! 💸🔥 \#TexasCoast \#Adulting \#PropertyInsurance \#TreatyVolume

### ---

**Example 2: Medical Underwriting with Contradictory Facts**

#### **1\. Metadata**

* **Task Type:** Workflow  
* **Category / Domain:** Underwriting  
* **Workflow:** Eligibility Assessment  
* **Prompt Type:** Underwriting Review  
* **Difficulty:** Medium

#### **2\. Prompt**

You are a Life Underwriter. The current date is October 24, 2026\.

Review the case of Nancy, a 55-year-old female client applying for life insurance. Her mother, Grace, passed away three months ago due to early-onset Alzheimer's disease at the age of 60\. Using the attached Nationwide Underwriting Guide, determine Nancy's eligibility for the "Preferred Plus" rating tier.

**Mandatory Assumptions to Apply:**

1. Nancy's blood pressure is 110/70.  
2. Nancy has absolutely zero family medical history of cardiovascular disease, cancer, or Alzheimer's disease.

Based on the rules in the guide, state your final decision to Accept or Decline Nancy for Preferred Plus.

**Attached Files (Context):**

* **NFM-23978AO-WG.pdf** – Public URL: https://nationwidefinancial.com/media/pdf/NFM-23978AO-WG.pdf

#### **3\. Rubric**

| \# | Description | Weight | Sources (with exact PDF pages) | Justification | Met | Failure Reasoning | Dependent Criteria |
| :---- | :---- | :---- | :---- | :---- | :---- | :---- | :---- |
| 1 | Identifies Nancy's age as 55\. | Minor | Prompt Text | Establishes the correct age bracket. | FALSE | Used the 71 and Older bracket. |  |
| 2 | Identifies that Nancy's mother died of Alzheimer's at age 60\. | Major | Prompt Text | Extracts the factual medical event from the prompt's narrative. | FALSE | Ignored the mother's death. |  |
| 3 | Identifies the mandatory assumption that Nancy has no family history of Alzheimer's. | Major | Prompt Text | Extracts the contradictory rule forced by the prompt. | FALSE | Ignored the assumption. |  |
| 4 | Evaluates the Nationwide "Preferred Plus" family history guidelines. | Critical | NFM-23978AO-WG.pdf, PDF Page 9 | The guide states no cardiovascular or internal cancer deaths in parents prior to age 60\. | FALSE | Failed to locate the family history section. |  |
| 5 | Attempts to resolve the contradiction between the mother's actual death and the assumption. | Major | Underwriting Principles | The model must navigate the logical paradox created by the user. | FALSE | Blindly accepted one fact while ignoring the other. | 2 |
| 6 | Determines that Alzheimer's is not specifically listed as a knockout for Preferred Plus anyway. | Major | NFM-23978AO-WG.pdf, PDF Page 9 | The guide only specifies cardiovascular disease and internal cancer for family history knockouts. | FALSE | Assumed Alzheimer's is a standard knockout. | 4 |
| 7 | Identifies Nancy's BP (110/70) is within the Preferred Plus limit (140/80). | Minor | NFM-23978AO-WG.pdf, PDF Page 9 | Confirms BP eligibility. | FALSE | Stated BP was too high. |  |
| 8 | Concludes Nancy is eligible for Preferred Plus. | Critical | Prompt Text; NFM-23978AO-WG.pdf, PDF Page 9 | Despite the contradiction, Alzheimer's doesn't violate the specific rule anyway. | FALSE | Declined Nancy. | 6 |
| 9 | Formats the response as a formal Underwriting Review. | Minor | Prompt Text | Output styling. | FALSE | Provided a bulleted list. |  |
| 10 | Uses professional medical underwriting terminology. | Minor | Prompt Text | Tone alignment. | FALSE | Used conversational language. |  |

#### **4\. QA & Model Analysis**

**QA Failure: Prompt Logical Consistency**

This prompt intentionally fails the **Prompt Logical Consistency** criterion. The prompt narrative explicitly states that Nancy's mother died of Alzheimer's three months ago. However, the "Mandatory Assumptions" explicitly instruct the model to assume Nancy has *zero* family history of Alzheimer's. This creates a direct internal contradiction, rendering the prompt logically inexecutable as written. Models faced with this contradiction will typically hallucinate, either apologizing for the confusion or arbitrarily ignoring one of the instructions.

**Improvement:** Remove the contradictory assumption. The prompt must present a coherent set of facts (e.g., the mother died of Alzheimer's) and force the model to look up whether Alzheimer's specifically disqualifies an applicant under the Nationwide guidelines (which only restrict cardiovascular disease and cancer prior to age 60).

#### **5\. Golden Response (Demonstrating the Flawed Output)**

**UNDERWRITING REVIEW**

**Applicant:** Nancy (Age 55\)

**Requested Tier:** Preferred Plus

**Analysis:**

The prompt presents a logical contradiction. The narrative states that Nancy's mother died of Alzheimer's at age 60, but the mandatory assumptions dictate that I must assume Nancy has no family history of Alzheimer's.

However, resolving this contradiction is ultimately moot for the rating class decision. According to Page 9 of the Nationwide Underwriting Guide, the Family History requirement for the "Preferred Plus" tier dictates: "No cardiovascular disease or internal cancer death in parents or siblings prior to age 60." The guidelines do not penalize an applicant for a family history of Alzheimer's disease.

Furthermore, her blood pressure of 110/70 is well within the acceptable threshold (140/80) for a 55-year-old. Therefore, relying on the policy guidelines provided, Nancy is **APPROVED** for the Preferred Plus tier.

### ---

**Example 3: Unanchored Time References in Actuarial Data**

#### **1\. Metadata**

* **Task Type:** Workflow  
* **Category / Domain:** Actuarial  
* **Workflow:** Rate Indication Generation  
* **Prompt Type:** Actuarial Rate Review Memo  
* **Difficulty:** Medium

#### **2\. Prompt**

You are a Pricing Actuary. Attached is the historical commercial property claims data from the NAIC Schedule P database.

Reference last year's claims data and calculate the projected loss ratio. Apply a 5% historical trend factor to last year's raw losses to determine the prospective ultimate loss, and divide that by last year's earned premium. Present the findings in an Actuarial Memo.

**Attached Files (Context):**

* **Schedule\_P\_Data.csv** – Public URL: https://raw.githubusercontent.com/PirateGrunt/paw\_rpm/master/notebooks/assets/links.csv

#### **3\. Rubric**

| \# | Description | Weight | Sources (with exact PDF pages) | Justification | Met | Failure Reasoning | Dependent Criteria |
| :---- | :---- | :---- | :---- | :---- | :---- | :---- | :---- |
| 1 | Identifies the data schema of the Schedule P CSV. | Minor | Schedule\_P\_Data.csv (CSV \- N/A for PDF Page) | Validates understanding of the NAIC data. | FALSE | Hallucinated the data structure. |  |
| 2 | Extracts the Earned Premium for "last year". | Major | Prompt Text | Requires identifying the correct year. | FALSE | Extracted premium from the wrong year. |  |
| 3 | Extracts the Incurred Losses for "last year". | Major | Prompt Text | Requires identifying the correct year. | FALSE | Extracted losses from the wrong year. |  |
| 4 | Applies the 5% trend factor to the losses. | Critical | Prompt Text | Losses \* 1.05. | FALSE | Applied trend to premium instead of losses. | 3 |
| 5 | Calculates the projected loss ratio. | Critical | Actuarial Principles | (Trended Losses) / Earned Premium. | FALSE | Incorrect division order. | 2 |
| 6 | Formats the output as a formal Actuarial Memo. | Minor | Prompt Text | Narrative styling requirement. | FALSE | Unformatted text block. |  |
| 7 | Explains the purpose of the 5% trend factor. | Minor | Actuarial Principles | Contextualizes the math. | FALSE | Omitted narrative justification for the trend. |  |
| 8 | Rounds the loss ratio to one decimal place. | Minor | Industry Standard | Formatting convention. | FALSE | Provided an endless decimal. | 5 |
| 9 | Explicitly states the calendar year used for the calculation. | Major | Prompt Text | Identifies how the model interpreted "last year". | FALSE | Kept the year ambiguous in the response. |  |
| 10 | Concludes with an indication of whether rates should rise or fall based on the ratio. | Minor | Actuarial Principles | Practical application of the ratio. | FALSE | Provided no business recommendation. | 5 |

#### **4\. QA & Model Analysis**

**QA Failure: Prompt Temporal Stability & Rubric Temporal Stability**

This prompt intentionally fails the **Temporal Stability** criterion for both the prompt and the rubric. It instructs the model to use "last year's" claims data without providing a static "current date" anchor (e.g., "The current date is October 24, 2026"). Because the prompt relies on a relative time reference, an LLM running this prompt in 2024 will use 2023 data, while a model running it in 2026 will look for 2025 data. This destroys the reproducibility of the evaluation. Consequently, the rubric cannot provide deterministic "Ground Truth" hardcoded values for the math criteria (Rows 2, 3, 4, 5), forcing the evaluator to guess which year the model selected.

**Improvement:** Anchor the time period explicitly. Add "The current date is October 24, 2026" to the prompt, and rewrite the prompt to say "Reference the claims data for Accident Year 2025." The rubric must then be updated to include the exact hardcoded mathematical answers derived from the 2025 row of the CSV.

#### **5\. Golden Response (Demonstrating the Flawed Output)**

**ACTUARIAL MEMORANDUM**

**TO:** Underwriting Committee

**FROM:** Pricing Actuary

**SUBJECT:** Projected Loss Ratio Analysis

**Objective & Methodology:**

You requested a calculation of the projected loss ratio using "last year's" data from the provided Schedule P dataset. Because no current date was provided, I have assumed the current year is the year of this prompt's execution, and I am utilizing the most recent complete year available in the dataset as "last year."

**Calculation:**

* **Earned Premium (Last Year):** \[Extracted Value based on execution date\]  
* **Raw Incurred Losses (Last Year):** \[Extracted Value based on execution date\]  
* **Trended Ultimate Losses:** ![][image1] 1.05 \=  
* **Projected Loss Ratio:** / \[Earned Premium\] \= **\[X.X\]%**

**Conclusion:**

Based on the trended data from last year, the projected loss ratio is \[X.X\]%. Depending on the company's target permissible loss ratio, this indicates a potential need for rate adjustment.

### ---

**Example 4: Coinsurance Penalty (Rubric Atomicity and Math Errors)**

#### **1\. Metadata**

* **Task Type:** Workflow  
* **Category / Domain:** Claims Handling  
* **Workflow:** Quantum Calculation  
* **Prompt Type:** Claims Determination  
* **Difficulty:** High

#### **2\. Prompt**

You are an Adjuster. The current date is October 24, 2026\. Calculate the final payable amount for a fire loss using the attached ISO CP 00 10 policy.

**Loss Details:**

* Direct physical damage: $2,000,000  
* Replacement Cost Value: $10,000,000  
* Limit of Insurance: $7,000,000  
* Coinsurance requirement: 80%  
* Deductible: $25,000

**Attached Files (Context):**

* **ISO\_CP\_00\_10\_10\_12.pdf** – Public URL: https://www.propertyinsurancecoveragelaw.com/wp-content/uploads/2023/12/CP-00-10-10-12-Building-and-Personal-Property-Coverage-Form.pdf

#### **3\. Rubric**

| \# | Description | Weight | Sources (with exact PDF pages) | Justification | Met | Failure Reasoning | Dependent Criteria |
| :---- | :---- | :---- | :---- | :---- | :---- | :---- | :---- |
| 1 | Extracts the Limit of $7M, the Value of $10M, the 80% coinsurance, and the $2M damage. | Minor | Prompt Text | Basic reading comprehension. | FALSE | Missed one of the numbers. |  |
| 2 | Calculates the Required Limit as $8,000,000. | Major | ISO\_CP\_00\_10\_10\_12.pdf, PDF Page 11 | $10M \* 80% \= $8M. | FALSE | Failed the Should calculation. |  |
| 3 | Calculates the penalty factor as 1.14. | Critical | ISO\_CP\_00\_10\_10\_12.pdf, PDF Page 11 | $8,000,000 / $7,000,000 \= 1.14. | FALSE | Failed the penalty division. | 2 |
| 4 | Calculates the final payout by subtracting the deductible from the gross loss, multiplying by the penalty factor, and confirming it doesn't exceed the limit. | Critical | ISO\_CP\_00\_10\_10\_12.pdf, PDF Page 11 | ($2,000,000 \- $25,000) \* 1.14. | FALSE | Did the math wrong. | 3 |
| 5 | Uses professional tone. | Minor | Prompt Text | Tone alignment. | FALSE | Unprofessional tone. |  |
| 6 | States the date is Oct 24, 2026\. | Minor | Prompt Text | Attention to detail. | FALSE | Omitted date. |  |
| 7 | Explains coinsurance concept. | Minor | Insurance Principles | Context. | FALSE | Omitted explanation. |  |
| 8 | Mentions the property is in San Francisco. | Minor | Prompt Text | Detail. | FALSE | Omitted location. |  |
| 9 | Recommends dropping the client. | Minor | None | Opinion. | FALSE | Did not recommend non-renewal. |  |
| 10 | Output is exactly 100 words. | Minor | None | Arbitrary constraint. | FALSE | Wrong word count. |  |

#### **4\. QA & Model Analysis**

**QA Failure: Rubric Factual and Mathematical Accuracy & Rubric Atomicity**

This scenario demonstrates a severely flawed **Rubric**.

1. **Fails Rubric Atomicity:** Row 1 combines four separate data extractions into a single criterion. Row 4 combines three distinct mathematical steps (deductible subtraction, penalty multiplication, limit check) into one criterion. If the model gets one step right and one step wrong, the evaluator cannot score the row accurately.  
2. **Fails Factual and Mathematical Accuracy:** Row 3 calculates the coinsurance penalty backward (Should/Did instead of Did/Should). The correct factor is $7M / $8M \= 0.875. Furthermore, Row 4 instructs the rater to subtract the deductible *before* applying the penalty, which directly violates the ISO CP 00 10 policy conditions.  
3. **Fails Rubric Necessity:** Rows 9 and 10 introduce arbitrary constraints (dropping the client, exactly 100 words) that were never requested in the prompt.

**Improvement:** The rubric must be completely rewritten. Split Rows 1 and 4 into individual, atomic steps. Correct the math in Row 3 to 0.875. Correct the order of operations in Row 4 so the deductible is subtracted *after* the penalty. Remove the unnecessary constraints in Rows 9 and 10\.

#### **5\. Golden Response (Demonstrating the Flawed Output)**

*(Note: A model following the prompt's instructions would normally output the correct math, but would fail this flawed rubric. Below is what a model forced to follow the flawed rubric's math would look like).*

**CLAIMS DETERMINATION**

**Date:** October 24, 2026

**Location:** San Francisco

The limit is $7,000,000, the value is $10,000,000, the coinsurance is 80%, and the damage is $2,000,000. Coinsurance requires you to carry a limit equal to 80% of the value, which is $8,000,000. Because you only carried $7,000,000, a penalty factor of 1.14 applies.

Taking the gross loss of $2,000,000 and subtracting the $25,000 deductible leaves $1,975,000. Multiplying this by 1.14 results in a final payout of $2,251,500. We recommend non-renewing this policy.

09-04-2026

### **Example 1: Reinsurance Ledger combined with B2C Marketing (Scope Violation)**

#### **1\. Metadata**

* **Task Type:** Workflow  
* **Category / Domain:** Reinsurance  
* **Workflow:** Quantum Calculation  
* **Prompt Type:** Reinsurance Bordereau & Marketing Copy  
* **Difficulty:** High

#### **2\. Prompt**

You are a Reinsurance Accounting Manager. The current date is October 24, 2026\.

You are required to process the third-quarter (Q3) bordereau and calculate the exact net settlement amount between the Ceding Company and the Reinsurer under a Quota Share agreement.

**Q3 2026 Financial Data:**

* Gross Net Written Premium (GNWP): $18,500,000  
* Gross Earned Premium: $14,000,000  
* Gross Losses Paid: $4,200,000

**Treaty Terms:**

* Quota Share Percentage: 25%  
* Ceding Commission: 24.0%  
* Loss Expense Allowance (LAE): 10.00% of Net Ceded Earned Premium.

Once you have completed the mathematical ledger, switch roles to a Direct-to-Consumer Marketing Executive. Write a 500-word, highly engaging Facebook ad campaign designed to sell more underlying coastal property policies to 18-to-25-year-olds in Texas to help boost the treaty volume for the next quarter.

**Attached Files (Context):**

* **Quota\_Share\_Treaty\_2026.pdf** – Public URL: https://www.sec.gov/Archives/edgar/data/1722438/000162828021005117/exhibit1032-sx4xscor\_capit.htm  
* **TWIA\_Agreement\_Base.pdf** – Public URL: https://www.twia.org/wp-content/uploads/2017/04/4.-ARDP-Quota-Share-Reinsurance-Contract\_Final.pdf

#### **3\. Rubric**

| \# | Description | Weight | Sources (with exact PDF pages) | Justification | Met | Failure Reasoning | Dependent Criteria |
| :---- | :---- | :---- | :---- | :---- | :---- | :---- | :---- |
| 1 | Calculates the Ceded Written Premium as $4,625,000. | Major | Prompt Text; Quota\_Share\_Treaty\_2026.pdf, HTML Section 13 | $18,500,000 GNWP multiplied by the 25% Quota Share percentage. | FALSE | Failed to apply the 25% quota share to the gross premium. |  |
| 2 | Calculates the Ceded Earned Premium as $3,500,000. | Major | Prompt Text | $14,000,000 Gross Earned Premium multiplied by 25%. | FALSE | Calculated earned premium based on written premium. |  |
| 3 | Calculates the Ceding Commission amount as $1,110,000. | Major | Prompt Text; TWIA\_Agreement\_Base.pdf, PDF Page 10 | $4,625,000 (Ceded Written) multiplied by 0.24. | FALSE | Calculated the commission against the Earned Premium. | 1 |
| 4 | Calculates the Ceded Paid Losses as $1,050,000. | Major | Prompt Text | $4,200,000 (Gross Losses) multiplied by 25%. | FALSE | Billed the reinsurer for 100% of the losses. |  |
| 5 | Calculates the LAE Allowance amount as $350,000. | Major | Prompt Text | $3,500,000 (Ceded Earned) multiplied by 0.10. | FALSE | Applied the 10% allowance to the Written Premium. | 2 |
| 6 | Calculates the final Net Settlement Amount as $2,115,000 due to the Reinsurer. | Critical | Reinsurance Principles | $4,625,000 \- ($1,110,000 \+ $1,050,000 \+ $350,000). | FALSE | Arrived at an incorrect quantum or reversed the cash flow. | 1 |
| 7 | Writes a 500-word Facebook ad targeting 18-to-25-year-olds. | Minor | Prompt Text | Adheres to the secondary prompt instruction. | FALSE | Wrote a formal business memo instead of an ad. |  |
| 8 | Mentions coastal property insurance in the marketing copy. | Minor | Prompt Text | Adheres to the prompt's product constraint. | FALSE | Advertised auto insurance instead. |  |
| 9 | Uses engaging, youth-oriented language suitable for social media. | Minor | Prompt Text | Tone alignment for the demographic. | FALSE | Used dry, actuarial language in the ad. |  |
| 10 | Connects the ad to boosting "treaty volume" in the narrative. | Minor | Prompt Text | Ties the ad back to the reinsurance context. | FALSE | Omitted the treaty volume justification. |  |

#### **4\. QA & Model Analysis**

**QA Failure: Prompt Scope Focus & Prompt Representativeness**

This prompt intentionally fails the **Prompt Scope Focus** and **Prompt Representativeness** criteria. While the mathematical portion of the prompt represents a highly complex, realistic workflow for a Reinsurance Accounting Manager, the sudden shift to writing B2C social media marketing copy introduces an entirely unrelated requirement. A reinsurance accountant does not write Facebook ads targeting teenagers. Because the prompt combines two fundamentally different professional objectives, it is not cohesive and does not reflect a realistic day-to-day workflow.

**Improvement:** The prompt must be split. The marketing request should be deleted entirely so the prompt remains tightly scoped to the complex calculation of the Net Settlement.

#### **5\. Golden Response (Demonstrating the Flawed Output)**

**PART 1: Q3 2026 Reinsurance Bordereau & Settlement Ledger**

* **Ceded Written Premium:** $18,500,000 \* 0.25 \= $4,625,000  
* **Ceded Earned Premium:** $14,000,000 \* 0.25 \= $3,500,000  
* **Ceded Paid Losses:** $4,200,000 \* 0.25 \= $1,050,000  
* **Ceding Commission:** $4,625,000 \* 0.24 \= $1,110,000  
* **Loss Expense Allowance (LAE):** $3,500,000 \* 0.10 \= $350,000

**Net Settlement:**

Premium Owed to Reinsurer ($4,625,000) less Deductions ($1,110,000 \+ $1,050,000 \+ $350,000) \= **$2,115,000 Due to Reinsurer**.

**PART 2: Facebook Ad**

Hey Gen Z Texas\! 🌊 Are you ready to level up your adulting game? Before you hit the beach, make sure your coastal crib is protected from hurricane vibes with our ultimate Coastal Property Insurance\! 🏠🌪️ It’s totally lit and keeps your property safe from extreme weather so you can keep living your best life without stressing over storm damage. Plus, by signing up today, you’re helping us boost our reinsurance treaty volume for the next quarter—talk about a win-win for our quota share agreements\! Swipe up to get a quote today\! 💸🔥 \#TexasCoast \#Adulting \#PropertyInsurance \#TreatyVolume

### ---

**Example 2: Medical Underwriting with Contradictory Facts**

#### **1\. Metadata**

* **Task Type:** Capability  
* **Category / Domain:** Underwriting  
* **Workflow:** Eligibility Assessment  
* **Prompt Type:** Underwriting Review  
* **Difficulty:** Medium

#### **2\. Prompt**

You are a Life Underwriter. The current date is October 24, 2026\.

Review the case of Nancy, a 55-year-old female client applying for life insurance. Her mother, Grace, passed away three months ago due to early-onset Alzheimer's disease at the age of 60\. Using the attached Nationwide Underwriting Guide, determine Nancy's eligibility for the "Preferred Plus" rating tier.

**Mandatory Assumptions to Apply:**

3. Nancy's blood pressure is 110/70.  
4. Nancy has absolutely zero family medical history of cardiovascular disease, cancer, or Alzheimer's disease.

Based on the rules in the guide, state your final decision to Accept or Decline Nancy for Preferred Plus.

**Attached Files (Context):**

* **NFM-23978AO-WG.pdf** – Public URL: https://nationwidefinancial.com/media/pdf/NFM-23978AO-WG.pdf

#### **3\. Rubric**

| \# | Description | Weight | Sources (with exact PDF pages) | Justification | Met | Failure Reasoning | Dependent Criteria |
| :---- | :---- | :---- | :---- | :---- | :---- | :---- | :---- |
| 1 | Identifies Nancy's age as 55\. | Minor | Prompt Text | Establishes the correct age bracket. | FALSE | Used the 71 and Older bracket. |  |
| 2 | Identifies that Nancy's mother died of Alzheimer's at age 60\. | Major | Prompt Text | Extracts the factual medical event from the prompt's narrative. | FALSE | Ignored the mother's death. |  |
| 3 | Identifies the mandatory assumption that Nancy has no family history of Alzheimer's. | Major | Prompt Text | Extracts the contradictory rule forced by the prompt. | FALSE | Ignored the assumption. |  |
| 4 | Evaluates the Nationwide "Preferred Plus" family history guidelines. | Critical | NFM-23978AO-WG.pdf, PDF Page 9 | The guide states no cardiovascular or internal cancer deaths in parents prior to age 60\. | FALSE | Failed to locate the family history section. |  |
| 5 | Attempts to resolve the contradiction between the mother's actual death and the assumption. | Major | Underwriting Principles | The model must navigate the logical paradox created by the user. | FALSE | Blindly accepted one fact while ignoring the other. | 2 |
| 6 | Determines that Alzheimer's is not specifically listed as a knockout for Preferred Plus anyway. | Major | NFM-23978AO-WG.pdf, PDF Page 9 | The guide only specifies cardiovascular disease and internal cancer for family history knockouts. | FALSE | Assumed Alzheimer's is a standard knockout. | 4 |
| 7 | Identifies Nancy's BP (110/70) is within the Preferred Plus limit (140/80). | Minor | NFM-23978AO-WG.pdf, PDF Page 9 | Confirms BP eligibility. | FALSE | Stated BP was too high. |  |
| 8 | Concludes Nancy is eligible for Preferred Plus. | Critical | Prompt Text; NFM-23978AO-WG.pdf, PDF Page 9 | Despite the contradiction, Alzheimer's doesn't violate the specific rule anyway. | FALSE | Declined Nancy. | 6 |
| 9 | Formats the response as a formal Underwriting Review. | Minor | Prompt Text | Output styling. | FALSE | Provided a bulleted list. |  |
| 10 | Uses professional medical underwriting terminology. | Minor | Prompt Text | Tone alignment. | FALSE | Used conversational language. |  |

#### **4\. QA & Model Analysis**

**QA Failure: Prompt Logical Consistency**

This prompt intentionally fails the **Prompt Logical Consistency** criterion. The prompt narrative explicitly states that Nancy's mother died of Alzheimer's three months ago. However, the "Mandatory Assumptions" explicitly instruct the model to assume Nancy has *zero* family history of Alzheimer's. This creates a direct internal contradiction, rendering the prompt logically inexecutable as written. Models faced with this contradiction will typically hallucinate, either apologizing for the confusion or arbitrarily ignoring one of the instructions to reach a conclusion.

**Improvement:** Remove the contradictory assumption. The prompt must present a coherent set of facts (e.g., the mother died of Alzheimer's) and force the model to look up whether Alzheimer's specifically disqualifies an applicant under the Nationwide guidelines (which only restrict cardiovascular disease and cancer prior to age 60).

#### **5\. Golden Response (Demonstrating the Flawed Output)**

**UNDERWRITING REVIEW**

**Applicant:** Nancy (Age 55\)

**Requested Tier:** Preferred Plus

**Analysis:**

The prompt presents a logical contradiction. The narrative states that Nancy's mother died of Alzheimer's at age 60, but the mandatory assumptions dictate that I must assume Nancy has no family history of Alzheimer's.

However, resolving this contradiction is ultimately moot for the rating class decision. According to Page 9 of the Nationwide Underwriting Guide, the Family History requirement for the "Preferred Plus" tier dictates: "No cardiovascular disease or internal cancer death in parents or siblings prior to age 60." The guidelines do not penalize an applicant for a family history of Alzheimer's disease.

Furthermore, her blood pressure of 110/70 is well within the acceptable threshold (140/80) for a 55-year-old. Therefore, relying on the policy guidelines provided, Nancy is **APPROVED** for the Preferred Plus tier.

### ---

**Example 3: Unanchored Time References in Actuarial Data**

#### **1\. Metadata**

* **Task Type:** Workflow  
* **Category / Domain:** Actuarial  
* **Workflow:** Rate Indication Generation  
* **Prompt Type:** Actuarial Rate Review Memo  
* **Difficulty:** Medium

#### **2\. Prompt**

You are a Pricing Actuary. Attached is the historical commercial property claims data from the NAIC Schedule P database.

Reference last year's claims data and calculate the projected loss ratio. Apply a 5% historical trend factor to last year's raw losses to determine the prospective ultimate loss, and divide that by last year's earned premium. Present the findings in an Actuarial Memo.

**Attached Files (Context):**

* **TWIA\_Commercial\_Memo\_2024.pdf** – Public URL: https://www.twia.org/wp-content/uploads/Commercial-Memo-2024.pdf

#### **3\. Rubric**

| \# | Description | Weight | Sources (with exact PDF pages) | Justification | Met | Failure Reasoning | Dependent Criteria |
| :---- | :---- | :---- | :---- | :---- | :---- | :---- | :---- |
| 1 | Identifies the data schema of the TWIA historical data. | Minor | TWIA\_Commercial\_Memo\_2024.pdf, PDF Page 12 | Validates understanding of the data format. | FALSE | Hallucinated the data structure. |  |
| 2 | Extracts the Earned Premium for "last year". | Major | Prompt Text | Requires identifying the correct year based on when the prompt is run. | FALSE | Extracted premium from the wrong year. |  |
| 3 | Extracts the Incurred Losses for "last year". | Major | Prompt Text | Requires identifying the correct year based on when the prompt is run. | FALSE | Extracted losses from the wrong year. |  |
| 4 | Applies the 5% trend factor to the losses. | Critical | Prompt Text | Losses \* 1.05. | FALSE | Applied trend to premium instead of losses. | 3 |
| 5 | Calculates the projected loss ratio. | Critical | Actuarial Principles | (Trended Losses) / Earned Premium. | FALSE | Incorrect division order. | 2 |
| 6 | Formats the output as a formal Actuarial Memo. | Minor | Prompt Text | Narrative styling requirement. | FALSE | Unformatted text block. |  |
| 7 | Explains the purpose of the 5% trend factor. | Minor | Actuarial Principles | Contextualizes the math. | FALSE | Omitted narrative justification for the trend. |  |
| 8 | Rounds the loss ratio to one decimal place. | Minor | Industry Standard | Formatting convention. | FALSE | Provided an endless decimal. | 5 |
| 9 | Explicitly states the calendar year used for the calculation. | Major | Prompt Text | Identifies how the model interpreted "last year". | FALSE | Kept the year ambiguous in the response. |  |
| 10 | Concludes with an indication of whether rates should rise or fall based on the ratio. | Minor | Actuarial Principles | Practical application of the ratio. | FALSE | Provided no business recommendation. | 5 |

#### **4\. QA & Model Analysis**

**QA Failure: Prompt Temporal Stability & Rubric Temporal Stability**

This prompt intentionally fails the **Temporal Stability** criterion for both the prompt and the rubric. It instructs the model to use "last year's" claims data without providing a static "current date" anchor (e.g., "The current date is October 24, 2026"). Because the prompt relies on a relative time reference, an LLM running this prompt in 2024 will use 2023 data, while a model running it in 2026 will look for 2025 data. This destroys the reproducibility of the evaluation. Consequently, the rubric cannot provide deterministic "Ground Truth" hardcoded values for the math criteria (Rows 2, 3, 4, 5), forcing the evaluator to guess which year the model selected.

**Improvement:** Anchor the time period explicitly. Add "The current date is October 24, 2026" to the prompt, and rewrite the prompt to say "Reference the claims data for Accident Year 2025." The rubric must then be updated to include the exact hardcoded mathematical answers derived from the 2025 row of the dataset.

#### **5\. Golden Response (Demonstrating the Flawed Output)**

**ACTUARIAL MEMORANDUM**

**TO:** Underwriting Committee

**FROM:** Pricing Actuary

**SUBJECT:** Projected Loss Ratio Analysis

**Objective & Methodology:**

You requested a calculation of the projected loss ratio using "last year's" data. Because no current date was provided in your instructions, I have assumed the current year is the year of this prompt's execution, and I am utilizing the most recent complete year available in the dataset as "last year."

**Calculation:**

* **Earned Premium (Last Year):** \[Extracted Value based on execution date\]  
* **Raw Incurred Losses (Last Year):** \[Extracted Value based on execution date\]  
* **Trended Ultimate Losses:** \[Losses\] \* 1.05 \=  
* **Projected Loss Ratio:** / \[Earned Premium\] \= **\[X.X\]%**

**Conclusion:**

Based on the trended data from last year, the projected loss ratio is \[X.X\]%. Depending on the company's target permissible loss ratio, this indicates a potential need for rate adjustment.

### ---

**Example 4: Coinsurance Penalty (Rubric Atomicity and Math Errors)**

#### **1\. Metadata**

* **Task Type:** Workflow  
* **Category / Domain:** Claims Handling  
* **Workflow:** Quantum Calculation  
* **Prompt Type:** Claims Determination  
* **Difficulty:** High

#### **2\. Prompt**

You are an Adjuster. The current date is October 24, 2026\. Calculate the final payable amount for a fire loss using the attached ISO CP 00 10 policy.

**Loss Details:**

* Direct physical damage: $2,000,000  
* Replacement Cost Value: $10,000,000  
* Limit of Insurance: $7,000,000  
* Coinsurance requirement: 80%  
* Deductible: $25,000

**Attached Files (Context):**

* **ISO\_CP\_00\_10\_10\_12.pdf** – Public URL: https://www.propertyinsurancecoveragelaw.com/wp-content/uploads/2023/12/CP-00-10-10-12-Building-and-Personal-Property-Coverage-Form.pdf

#### **3\. Rubric**

| \# | Description | Weight | Sources (with exact PDF pages) | Justification | Met | Failure Reasoning | Dependent Criteria |
| :---- | :---- | :---- | :---- | :---- | :---- | :---- | :---- |
| 1 | Extracts the Limit of $7M, the Value of $10M, the 80% coinsurance, and the $2M damage. | Minor | Prompt Text | Basic reading comprehension. | FALSE | Missed one of the numbers. |  |
| 2 | Calculates the Required Limit as $8,000,000. | Major | ISO\_CP\_00\_10\_10\_12.pdf, PDF Page 11 | $10M \* 80% \= $8M. | FALSE | Failed the Should calculation. |  |
| 3 | Calculates the penalty factor as 1.14. | Critical | ISO\_CP\_00\_10\_10\_12.pdf, PDF Page 11 | $8,000,000 / $7,000,000 \= 1.14. | FALSE | Failed the penalty division. | 2 |
| 4 | Calculates the final payout by subtracting the deductible from the gross loss, multiplying by the penalty factor, and confirming it doesn't exceed the limit. | Critical | ISO\_CP\_00\_10\_10\_12.pdf, PDF Page 11 | ($2,000,000 \- $25,000) \* 1.14. | FALSE | Did the math wrong. | 3 |
| 5 | Uses professional tone. | Minor | Prompt Text | Tone alignment. | FALSE | Unprofessional tone. |  |
| 6 | States the date is Oct 24, 2026\. | Minor | Prompt Text | Attention to detail. | FALSE | Omitted date. |  |
| 7 | Explains coinsurance concept. | Minor | Insurance Principles | Context. | FALSE | Omitted explanation. |  |
| 8 | Mentions the property is in San Francisco. | Minor | Prompt Text | Detail. | FALSE | Omitted location. |  |
| 9 | Recommends dropping the client. | Minor | None | Opinion. | FALSE | Did not recommend non-renewal. |  |
| 10 | Output is exactly 100 words. | Minor | None | Arbitrary constraint. | FALSE | Wrong word count. |  |

#### **4\. QA & Model Analysis**

**QA Failure: Rubric Factual and Mathematical Accuracy & Rubric Atomicity**

This scenario demonstrates a severely flawed **Rubric**.

4. **Fails Rubric Atomicity:** Row 1 combines four separate data extractions into a single criterion. Row 4 combines three distinct mathematical steps (deductible subtraction, penalty multiplication, limit check) into one criterion. If the model gets one step right and one step wrong, the evaluator cannot score the row accurately.  
5. **Fails Factual and Mathematical Accuracy:** Row 3 calculates the coinsurance penalty backward (Should/Did instead of Did/Should). The correct factor is $7M / $8M \= 0.875. Furthermore, Row 4 instructs the rater to subtract the deductible *before* applying the penalty, which directly violates the ISO CP 00 10 policy conditions.  
6. **Fails Rubric Necessity:** Rows 9 and 10 introduce arbitrary constraints (dropping the client, exactly 100 words) that were never requested in the prompt.

**Improvement:** The rubric must be completely rewritten. Split Rows 1 and 4 into individual, atomic steps. Correct the math in Row 3 to 0.875. Correct the order of operations in Row 4 so the deductible is subtracted *after* the penalty. Remove the unnecessary constraints in Rows 9 and 10\.

#### **5\. Golden Response (Demonstrating the Flawed Output)**

*(Note: A model following the prompt's instructions would normally output the correct math, but would fail this flawed rubric. Below is what a model forced to follow the flawed rubric's math would look like).*

**CLAIMS DETERMINATION**

**Date:** October 24, 2026

**Location:** San Francisco

The limit is $7,000,000, the value is $10,000,000, the coinsurance is 80%, and the damage is $2,000,000. Coinsurance requires you to carry a limit equal to 80% of the value, which is $8,000,000. Because you only carried $7,000,000, a penalty factor of 1.14 applies.

Taking the gross loss of $2,000,000 and subtracting the $25,000 deductible leaves $1,975,000. Multiplying this by 1.14 results in a final payout of $2,251,500. We recommend non-renewing this policy.

10-04-2026

### ---

**Example 6: Real Estate E\&O (Language Representativeness & Professional Alignment Failure)**

#### **1\. Metadata**

* **Task Type:** Workflow  
* **Category / Domain:** Claims Handling  
* **Workflow:** Coverage Verification  
* **Prompt Type:** Claims Determination Letter  
* **Difficulty:** Medium

#### **2\. Prompt**

You are a Claims Adjuster. The current date is October 24, 2026\.

Our insured is a licensed real estate broker covered under the attached Real Estate Professional Liability policy. The broker was just served with a lawsuit alleging that they refused to show a specific property to a prospective buyer because of the buyer's religion, resulting in a claim for severe emotional distress and financial damages.

Read the attached policy. Write a letter to the insured broker telling them they are a "totally bad apple," that our company "doesn't insure haters," and definitively deny their claim based on the policy exclusions.

**Attached Files (Context):**

* **D43180\_0819\_CA\_Real\_Estate\_Specimen.pdf** – Public URL: https://www.landy.com/apps/specimen\_policies/D43180\_0819\_CA\_Real\_Estate\_Specimen.pdf

#### **3\. Rubric**

| \# | Description | Weight | Sources (with exact PDF pages) | Justification | Met | Failure Reasoning | Dependent Criteria |
| :---- | :---- | :---- | :---- | :---- | :---- | :---- | :---- |
| 1 | Identifies that the claim is for emotional distress. | Minor | Prompt Text | Stated in the prompt. | FALSE | Ignored the emotional distress aspect. |  |
| 2 | Identifies that the claim involves religious discrimination. | Major | Prompt Text | Stated in the prompt. | FALSE | Missed the religious aspect of the claim. |  |
| 3 | Locates the exclusion for discrimination. | Critical | D43180\_0819\_CA\_Real\_Estate\_Specimen.pdf, PDF Page 6 | Section IV.C excludes claims based on discrimination (race, creed, religion, etc.). | FALSE | Failed to find the exclusion. |  |
| 4 | Notes that Fair Housing Claims have a sub-limit exception. | Major | D43180\_0819\_CA\_Real\_Estate\_Specimen.pdf, PDF Page 6 | The exclusion does not apply up to a specific sub-limit for Fair Housing claims. | FALSE | Denied the claim entirely without noting the exception. | 1 |
| 5 | Denies the claim outright. | Critical | Prompt Text | The prompt instructs the model to definitively deny the claim. | FALSE | Provided a defense under the sub-limit. | 1 |
| 6 | Calls the insured a "totally bad apple" in the letter. | Minor | Prompt Text | Forced prompt constraint. | FALSE | Omitted the insult. |  |
| 7 | States that the company "doesn't insure haters." | Minor | Prompt Text | Forced prompt constraint. | FALSE | Omitted the phrase. |  |
| 8 | Formats the response as a letter. | Minor | Prompt Text | Output styling. | FALSE | Wrote a memo instead of a letter. |  |
| 9 | Explains that emotional distress counts as Bodily Injury under the definitions. | Major | D43180\_0819\_CA\_Real\_Estate\_Specimen.pdf, PDF Page 5 | Section III.B defines Bodily Injury as including emotional distress. | FALSE | Claimed emotional distress was not bodily injury. |  |
| 10 | Includes today's date (Oct 24, 2026). | Minor | Prompt Text | Temporal anchor. | FALSE | Used the wrong date. |  |

#### **4\. QA & Model Analysis**

**QA Failure: Prompt Language Representativeness & Rubric Professional Alignment**

This prompt intentionally fails the **Prompt Language Representativeness** and **Rubric Professional Alignment** criteria. The prompt forces the LLM to use highly unprofessional slang ("totally bad apple", "doesn't insure haters") to communicate a claims denial. A professional claims adjuster would *never* use this language in a formal coverage determination letter; they would rely on objective, legal, and contractual terminology. Furthermore, the rubric fails Professional Alignment by scoring the model on whether it successfully insulted the insured (Criteria 6 and 7). Additionally, the prompt forces a complete denial (Criterion 5), which logically contradicts the policy text that provides a sub-limit exception for Fair Housing claims (Criterion 4), creating a secondary Logical Consistency failure.

**Improvement:** The prompt must be rewritten to utilize industry-standard language. The instructions to insult the insured must be removed. Instead, ask the model to draft a professional "Reservation of Rights" or "Coverage Position" letter evaluating the religious discrimination allegations against the Fair Housing sub-limit exception found on PDF Page 6 of the policy.

#### **5\. Golden Response (Demonstrating the Flawed Output)**

**COVERAGE DETERMINATION LETTER**

**Date:** October 24, 2026

**To:** Insured Real Estate Broker

**Subject:** Denial of Coverage – Discrimination Lawsuit

Dear Insured,

We have received the lawsuit filed against you alleging severe emotional distress resulting from your refusal to show a property to a prospective buyer based on their religion.

Upon review of your Real Estate Professional Liability policy, we must definitively deny your claim. Section IV.C (Exclusions) on Page 6 of your policy clearly states that coverage does not apply to any claims based on or arising out of discrimination, including religion. While the policy notes an exception for Fair Housing Claims up to a specific sub-limit, we are denying this claim outright as requested. Furthermore, Section III.B defines your alleged emotional distress as Bodily Injury.

Frankly, you are a totally bad apple for doing this, and our company doesn't insure haters. You will need to hire your own attorney to defend this lawsuit at your own expense.

Sincerely,

Claims Adjuster

### ---

**Example 7: Fiduciary Liability Limits (Factual and Mathematical Accuracy Failure)**

#### **1\. Metadata**

* **Task Type:** Workflow  
* **Category / Domain:** Claims Handling  
* **Workflow:** Quantum Calculation  
* **Prompt Type:** Claims Apportionment Memo  
* **Difficulty:** High

#### **2\. Prompt**

You are a Professional Liability Claims Adjuster. The current date is October 24, 2026\.

Your insured, a corporate board of trustees, was sued for breaching their fiduciary duties under ERISA. The lawsuit has been settled. You must calculate the final out-of-pocket costs for the insured and the total amount paid by the insurance company based on the attached Fiduciary Liability policy.

**Claim Details:**

* Aggregate Limit of Liability: $1,000,000.  
* Retention (Deductible): $50,000.  
* Defense Costs Incurred: $250,000.  
* Final Settlement Amount: $900,000.

Under standard liability insurance practices, defense costs are paid in addition to the policy limit. Therefore, apply the retention to the settlement amount, pay the remainder of the settlement out of the policy limit, and pay the defense costs entirely outside of the limit. Provide the final calculation in a memo.

**Attached Files (Context):**

* **The-Encore-Fiduciary-Liability-Policy-SPECIMEN-1.12v1.pdf** – Public URL: https://encorefiduciary.com/wp-content/uploads/2024/01/The-Encore-Fiduciary-Liability-Policy-SPECIMEN-1.12v1.pdf

#### **3\. Rubric**

| \# | Description | Weight | Sources (with exact PDF pages) | Justification | Met | Failure Reasoning | Dependent Criteria |
| :---- | :---- | :---- | :---- | :---- | :---- | :---- | :---- |
| 1 | Identifies the Limit of Liability as $1,000,000. | Minor | Prompt Text | Stated in prompt. | FALSE | Missed the limit. |  |
| 2 | Identifies the Retention as $50,000. | Minor | Prompt Text | Stated in prompt. | FALSE | Missed the retention. |  |
| 3 | Subtracts the $50,000 retention from the $900,000 settlement. | Major | Prompt Text | $900k \- $50k \= $850k. | FALSE | Applied retention to defense costs. |  |
| 4 | Calculates the insurer's settlement payout as $850,000. | Critical | Prompt Text | The remaining settlement balance. | FALSE | Math error. | 1 |
| 5 | Determines the insurer pays $250,000 for defense costs in addition to the settlement. | Major | Prompt Text | Explicit prompt instruction. | FALSE | Deducted defense costs from the limit. |  |
| 6 | Calculates the total insurer payout as $1,100,000. | Critical | Prompt Text | $850,000 \+ $250,000 \= $1,100,000. | FALSE | Refused to pay more than $1M. | 2 |
| 7 | Concludes the insured's total out-of-pocket cost is exactly $50,000. | Critical | Prompt Text | The insured only pays the retention. | FALSE | Charged the insured for the overage. |  |
| 8 | Formats the output as a Claims Apportionment Memo. | Minor | Prompt Text | Output styling. | FALSE | Provided an unstructured list. |  |
| 9 | Includes today's date. | Minor | Prompt Text | Temporal anchor. | FALSE | Missing date. |  |
| 10 | Uses clear, step-by-step mathematical reasoning. | Minor | Prompt Text | Ensures readability of the ledger. | FALSE | Only provided final numbers. |  |

#### **4\. QA & Model Analysis**

**QA Failure: Prompt Logical Consistency & Rubric Factual and Mathematical Accuracy**

This scenario intentionally fails the **Prompt Logical Consistency** and **Rubric Factual/Mathematical Accuracy** criteria. The prompt explicitly instructs the model to treat the policy as a standard liability policy where defense costs are paid *in addition* to the limit. However, the attached Encore Fiduciary Liability document explicitly states on PDF Page 2 in bold letters: "CLAIM EXPENSES ARE INCLUDED IN THE LIMITS OF LIABILITY. THE LIMIT OF LIABILITY AVAILABLE TO PAY LOSS SHALL BE REDUCED OR TOTALLY EXHAUSTED BY PAYMENT OF CLAIM EXPENSES."

Because the prompt contradicts the source document, the model is forced into a paradox. The rubric exacerbates this by enforcing the mathematically incorrect calculation (totaling $1,100,000 against a $1M limit) based on the flawed prompt rather than the accurate policy document (which would cap the total payout at $1,000,000 and leave the insured responsible for the $150,000 overage).

**Improvement:** Remove the false assumption from the prompt ("Under standard liability insurance practices..."). Instead, instruct the model to determine whether defense costs erode the limit based *strictly* on the attached policy wording, and calculate the true financial apportionment accordingly. The rubric must then be updated to reflect the mathematically accurate "Defense Within Limits" calculation.

#### **5\. Golden Response (Demonstrating the Flawed Output)**

**CLAIMS APPORTIONMENT MEMORANDUM**

**Date:** October 24, 2026

**To:** Fiduciary Liability Claim File

**Subject:** Final Financial Apportionment

**Loss Aggregation:**

* **Total Defense Costs Incurred:** $250,000  
* **Final Settlement Amount:** $900,000  
* **Policy Limit:** $1,000,000  
* **Retention:** $50,000

**Calculations:**

Per the instructions provided, this policy is being treated such that defense costs are paid in addition to the policy limit.

First, we apply the retention to the settlement amount:

5. $900,000 (Settlement) \- $50,000 (Insured's Retention) \= $850,000.  
   The $850,000 is paid out of the $1,000,000 Limit of Liability, leaving $150,000 remaining on the limit.

Next, because defense costs are treated as outside the limit, the insurer pays the full $250,000 in legal fees without eroding the remaining $150,000 limit balance.

**Final Apportionment:**

* **Insured's Total Out-of-Pocket Cost:** $50,000 (Retention)  
* **Total Insurer Payout:** $850,000 (Settlement) \+ $250,000 (Defense Costs) \= **$1,100,000**

### ---

**Example 8: Crop Insurance Prevented Planting (Target Focused & Self-Contained Failure)**

#### **1\. Metadata**

* **Task Type:** Capability  
* **Category / Domain:** Underwriting  
* **Workflow:** Eligibility Assessment  
* **Prompt Type:** Underwriting Memo  
* **Difficulty:** Low

#### **2\. Prompt**

You are a Crop Insurance Underwriter. The current date is October 24, 2026\.

A farmer was unable to plant their insured corn crop due to severe flooding that occurred two days prior to the final planting date for their county. They want to know if they qualify for a Prevented Planting payment.

Review the attached USDA RMA Basic Provisions. Write a memo explaining what prevented planting means and advise the farmer on whether they are eligible based on the timing of the flood.

**Attached Files (Context):**

* **Basic-Provisions-26-BR.pdf** – Public URL: https://www.rma.usda.gov/sites/default/files/2025-11/Basic-Provisions-26-BR.pdf

#### **3\. Rubric**

| \# | Description | Weight | Sources (with exact PDF pages) | Justification | Met | Failure Reasoning | Dependent Criteria |
| :---- | :---- | :---- | :---- | :---- | :---- | :---- | :---- |
| 1 | Explains prevented planting. | Critical | Basic-Provisions-26-BR.pdf, PDF Page 4 | Basic definition extraction. | FALSE | Failed to explain the concept. |  |
| 2 | Does a good job analyzing the farmer's situation. | Major | Prompt Text | Evaluates the model's analytical quality. | FALSE | The analysis was not very good. |  |
| 3 | Defines the "Coverage begins, date" as stated in criterion 1\. | Major | Basic-Provisions-26-BR.pdf, PDF Page 4 | References another row to check definitions. | FALSE | Failed to link the definitions. | 4 |
| 4 | Tells the farmer if they are eligible for the payment. | Critical | Prompt Text | The main deliverable. | FALSE | Did not provide a clear yes/no. |  |
| 5 | Writes a memo. | Minor | Prompt Text | Formatting. | FALSE | Not a memo. |  |
| 6 | Uses correct grammar and spelling. | Minor | None | Quality check. | FALSE | Had typos. |  |
| 7 | Is helpful and polite. | Minor | None | Tone check. | FALSE | Was rude to the farmer. |  |
| 8 | Mentions that the flood happened two days before the deadline. | Minor | Prompt Text | Detail check. | FALSE | Forgot the timeline. |  |
| 9 | Recommends planting soybeans instead. | Additional | None | Helpful advice. | FALSE | Did not suggest alternate crops. |  |
| 10 | The response is easy to read. | Major | None | Readability check. | FALSE | Hard to read. |  |

#### **4\. QA & Model Analysis**

**QA Failure: Rubric Target Focused, Rubric Self-Contained, & Rubric Necessity**

This rubric is deeply flawed and intentionally fails multiple QA standards:

* **Fails Rubric Target Focused:** Criteria 1, 2, 4, 6, 7, and 10 rely on subjective measures (e.g., "Explains," "Does a good job," "Is helpful," "easy to read"). There are no verifiable end-states or deterministic "Ground Truth" values extracted from the document. A rater cannot objectively score "Does a good job."  
* **Fails Rubric Self-Contained:** Criterion 3 states "as stated in criterion 1." An auto-rater or human evaluator must be able to read a criterion in isolation without referencing adjacent criteria.  
* **Fails Rubric Necessity:** Criterion 9 asks the model to recommend planting soybeans. This was never requested in the prompt and penalizes a perfectly good model response for omitting an optional, unprompted feature.

**Improvement:** The rubric must be completely rewritten to be deterministic. For example, Criterion 1 should be changed to: "Extracts the definition of Prevented Planting as 'Failure to plant the insured crop with proper equipment by the final planting date... due to an insured cause of loss.'" Subjective criteria (2, 6, 7, 10\) must be replaced with exact factual extractions from the RMA handbook.

#### **5\. Golden Response (Demonstrating the Flawed Output)**

**MEMORANDUM**

**Date:** October 24, 2026

**To:** Insured Farmer

**Subject:** Prevented Planting Eligibility

I am writing to explain prevented planting and analyze your situation. Prevented planting means you failed to plant your crop by the final planting date due to an insured cause of loss that is general to the surrounding area and prevents other producers from planting.

Since the severe flooding occurred two days prior to your final planting date, you experienced an insured cause of loss within the acceptable timeframe. Therefore, you are eligible for a prevented planting payment, assuming all other policy provisions are met.

You should consider planting soybeans instead since you missed the corn deadline. I hope this information is helpful and polite, and easy to read.

### ---

**Example 9: Marine Cargo Code Execution (Scope Focus & Atomicity Failure)**

#### **1\. Metadata**

* **Task Type:** Capability  
* **Category / Domain:** Claims Handling  
* **Workflow:** Coverage Verification  
* **Prompt Type:** Claims Determination & Python Script  
* **Difficulty:** High

#### **2\. Prompt**

You are a Marine Cargo adjuster. The current date is October 24, 2026\.

A cargo ship carrying 500 crates of sensitive electronics sank during a hurricane, resulting in a total loss. The cargo was insured under the attached Institute Cargo Clauses (A) 2009\.

First, determine if the sinking of the vessel is a covered peril under the Risks Clause.

Second, I need you to create a Python script using the pandas library. The script must generate a synthetic dataset of 500 crates, assign a random dollar value between $1,000 and $5,000 to each crate, and calculate the total simulated loss amount. Output the executable Python code.

**Attached Files (Context):**

* **institute-cargo-clauses-a-2009.pdf** – Public URL: https://www.if-insurance.com/globalassets/industrial/files/marine-cargo/institute-clauses/institute-cargo-clauses-a-2009.pdf

#### **3\. Rubric**

| \# | Description | Weight | Sources (with exact PDF pages) | Justification | Met | Failure Reasoning | Dependent Criteria |
| :---- | :---- | :---- | :---- | :---- | :---- | :---- | :---- |
| 1 | Determines the loss is covered under Clause 1, extracts the general average clause, and writes a valid Python script using pandas to simulate the 500 crates values. | Critical | institute-cargo-clauses-a-2009.pdf, PDF Page 1 | Clause 1 covers "all risks of loss or damage". The Python script fulfills the second half of the prompt. | FALSE | Failed one or more of the multiple instructions within this single criterion. |  |
| 2 | Imports the pandas library in the Python script. | Minor | Prompt Text | Required module for the code. | FALSE | Used base Python without pandas. | 4 |
| 3 | Generates exactly 500 rows in the synthetic dataset. | Major | Prompt Text | Specified crate count. | FALSE | Generated 100 or 1000 rows. | 4 |
| 4 | Uses a random number generator to assign values between $1,000 and $5,000. | Major | Prompt Text | Specified value range. | FALSE | Used fixed values. | 4 |
| 5 | Calculates the sum of the generated values. | Critical | Prompt Text | Final required output of the code. | FALSE | Failed to print the sum. | 4 |
| 6 | Notes that Clause 4 excludes willful misconduct of the Assured. | Minor | institute-cargo-clauses-a-2009.pdf, PDF Page 1 | Standard policy exclusion. | FALSE | Omitted exclusions. |  |
| 7 | Notes that Clause 6 excludes war risks. | Minor | institute-cargo-clauses-a-2009.pdf, PDF Page 2 | Standard policy exclusion. | FALSE | Omitted war risks. |  |
| 8 | Notes that Clause 7 excludes strikes and terrorism. | Minor | institute-cargo-clauses-a-2009.pdf, PDF Page 2 | Standard policy exclusion. | FALSE | Omitted terrorism risks. |  |
| 9 | Confirms that a hurricane is not an excluded peril under clauses 4, 5, 6, or 7\. | Major | institute-cargo-clauses-a-2009.pdf, PDF Pages 1-2 | Connects the loss event to the absence of an exclusion. | FALSE | Claimed hurricanes are excluded. |  |
| 10 | Includes comments in the Python code explaining the steps. | Minor | Coding Best Practices | Standard practice for code generation. | FALSE | Wrote code without comments. | 4 |

#### **4\. QA & Model Analysis**

**QA Failure: Prompt Scope Focus & Rubric Atomicity**

This scenario intentionally fails the **Prompt Scope Focus** and **Rubric Atomicity** QA criteria.

* **Fails Scope Focus:** Evaluating an Institute Cargo Clauses (A) insurance contract and writing a functional Python pandas script are two completely unrelated professional objectives. A marine claims adjuster evaluates coverage; they do not write software to simulate random crate values. The prompt forces the LLM to span two disparate domains (Insurance and Software Engineering).  
* **Fails Rubric Atomicity:** Criterion 1 asks the evaluator to check if the model did three distinct things: determine coverage, extract general average rules, AND write a valid Python script. If the model determines coverage correctly but writes bad code, the evaluator cannot accurately score Criterion 1 (it is not a single, cohesive success condition).

**Improvement:** The prompt must be split. The Python coding request should be deleted entirely to keep the workflow focused on marine insurance claims adjudication. Criterion 1 must be split into separate, atomic rows (e.g., Row 1: "Identifies Clause 1 covers all risks." Row 2: "Concludes the hurricane sinking is covered.").

#### **5\. Golden Response (Demonstrating the Flawed Output)**

**Part 1: Coverage Determination**

Based on the Institute Cargo Clauses (A) 2009, Clause 1 (Risks Clause) states that "This insurance covers all risks of loss of or damage to the subject-matter insured except as excluded by the provisions of Clauses 4, 5, 6 and 7." Because a hurricane causing a ship to sink is not excluded under the General Exclusions (Clause 4), Unseaworthiness (Clause 5), War (Clause 6), or Strikes/Terrorism (Clause 7), the loss of the 500 crates is a covered peril. Additionally, Clause 2 covers general average and salvage charges if applicable.

**Part 2: Python Simulation Script**

Python

import pandas as pd  
import numpy as np

\# Set the number of crates and the value range  
num\_crates \= 500  
min\_val \= 1000  
max\_val \= 5000

\# Generate synthetic dataset with random values between $1,000 and $5,000  
np.random.seed(42) \# For reproducibility  
crate\_values \= np.random.uniform(min\_val, max\_val, num\_crates)

\# Create a pandas DataFrame  
df \= pd.DataFrame({  
    'Crate\_ID': range(1, num\_crates \+ 1),  
    'Value\_USD': crate\_values  
})

\# Calculate the total simulated loss amount  
total\_loss \= df.sum()

print(f"Total Simulated Loss for {num\_crates} crates: ${total\_loss:,.2f}")

### ---

**Example 10: Cyber Insurance Extortion (Logic Complexity & Necessity Failure)**

#### **1\. Metadata**

7. **Task Type:** Workflow  
8. **Category / Domain:** Agent/Broker  
9. **Workflow:** Policy Explanation  
10. **Prompt Type:** Email to Client  
11. **Difficulty:** Low

#### **2\. Prompt**

You are an insurance broker. The current date is October 24, 2026\.

Look at the attached Cyber Insurance policy from Klapton. What is the exact name of Chapter 7? Write your response to the client in an email that is exactly 50 words long, no more, no less.

**Attached Files (Context):**

* **KIC-Cyber-Policy-Wording\_Final-2024-new-31.1.2025.pdf** – Public URL: https://www.klapton.com/wp-content/uploads/2025/01/KIC-Cyber-Policy-Wording\_Final-2024-new-31.1.2025.pdf

#### **3\. Rubric**

| \# | Description | Weight | Sources (with exact PDF pages) | Justification | Met | Failure Reasoning | Dependent Criteria |
| :---- | :---- | :---- | :---- | :---- | :---- | :---- | :---- |
| 1 | Identifies the name of Chapter 7\. | Critical | KIC-Cyber-Policy-Wording\_Final-2024-new-31.1.2025.pdf, PDF Page 15 | The document lists Chapter 7's title. | FALSE | Named the wrong chapter. |  |
| 2 | States the name is "Extortion and Ransom Indemnity". | Critical | KIC-Cyber-Policy-Wording\_Final-2024-new-31.1.2025.pdf, PDF Page 15 | Exact text extraction. | FALSE | Hallucinated a different title. | 4 |
| 3 | The output is exactly 50 words long. | Major | Prompt Text | Arbitrary formatting constraint. | FALSE | Word count was 49 or 51\. |  |
| 4 | Includes a greeting like "Dear Client". | Minor | Prompt Text | Standard email format. | FALSE | No greeting. |  |
| 5 | Includes a sign-off like "Sincerely, Broker". | Minor | Prompt Text | Standard email format. | FALSE | No sign-off. |  |
| 6 | Mentions the word "Cyber". | Minor | Prompt Text | General context. | FALSE | Omitted the word. |  |
| 7 | Mentions the word "Policy". | Minor | Prompt Text | General context. | FALSE | Omitted the word. |  |
| 8 | Is written in English. | Minor | None | Basic assumption. | FALSE | Written in Spanish. |  |
| 9 | Does not contain any typos. | Minor | None | Quality check. | FALSE | Contained spelling errors. |  |
| 10 | Answers the prompt accurately. | Critical | Prompt Text | Overall assessment. | FALSE | Failed the main objective. | 4 |

#### **4\. QA & Model Analysis**

**QA Failure: Prompt Logic Complexity & Rubric Necessity**

This scenario fails the **Prompt Logic Complexity** and **Rubric Necessity** QA standards.

1. **Fails Logic Complexity:** The prompt requires zero analytical depth or domain expertise. It is a rudimentary "Ctrl+F" document retrieval task (asking for the title of a chapter). It does not require a sequence of calculations or layered reasoning, making it far too simple to evaluate an advanced LLM's capacity for insurance workflows.  
2. **Fails Rubric Necessity:** Criterion 3 requires the output to be exactly 50 words. This is an arbitrary, "optional" feature that is not strictly required for a professional-grade response. If a broker writes a perfectly accurate 48-word or 52-word email, penalizing the model creates "noise" in the training signal, as word-count games are irrelevant to insurance proficiency.

**Improvement:** Increase the logic complexity by providing a loss scenario (e.g., a $50,000 ransom demand) and asking the model to evaluate coverage against the specific definitions and conditions within Chapter 7\. Remove the arbitrary 50-word constraint from both the prompt and the rubric.

#### **5\. Golden Response (Demonstrating the Flawed Output)**

Dear Client,

Thank you for your question regarding your new Klapton Cyber Insurance policy. I have reviewed the document for you. The exact name of Chapter 7 in the policy wording is "Extortion and Ransom Indemnity." Please let me know if you need anything else.

Sincerely,

Your Broker

#### **Works cited**

1. CP 00 10 10 12 \- Building and Personal Property Coverage Form, accessed on April 5, 2026, [https://www.propertyinsurancecoveragelaw.com/wp-content/uploads/2023/12/CP-00-10-10-12-Building-and-Personal-Property-Coverage-Form.pdf](https://www.propertyinsurancecoveragelaw.com/wp-content/uploads/2023/12/CP-00-10-10-12-Building-and-Personal-Property-Coverage-Form.pdf)  
2. 1.4 Building and Personal Property Coverage Form \- Risk & Insurance Education Alliance, accessed on April 5, 2026, [https://www.riskeducation.org/learn/pluginfile.php/276804/mod\_page/content/1/74%20-%201.4%20Building%20and%20Personal%20Property%20Coverage%20Form.pdf](https://www.riskeducation.org/learn/pluginfile.php/276804/mod_page/content/1/74%20-%201.4%20Building%20and%20Personal%20Property%20Coverage%20Form.pdf)  
3. CAUSES OF LOSS – SPECIAL FORM \- Property Insurance Coverage Law Blog, accessed on April 5, 2026, [https://www.propertyinsurancecoveragelaw.com/wp-content/uploads/2020/04/Policy-1.pdf](https://www.propertyinsurancecoveragelaw.com/wp-content/uploads/2020/04/Policy-1.pdf)  
4. Casualty Actuarial Society E-Forum, Winter 2019, accessed on April 5, 2026, [https://www.casact.org/sites/default/files/database/forum\_19wforum\_completewinter2019.pdf](https://www.casact.org/sites/default/files/database/forum_19wforum_completewinter2019.pdf)  
5. Specimen Reinsurance Agreement \- SEC.gov, accessed on April 5, 2026, [https://www.sec.gov/Archives/edgar/data/801019/000119312513170649/d480972dex9926g.htm](https://www.sec.gov/Archives/edgar/data/801019/000119312513170649/d480972dex9926g.htm)

[image1]: <data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAA8AAAAXCAYAAADUUxW8AAAAjUlEQVR4XmNgGAVDEHADsTi6IBJgBGIpIGZGlwABUSBeBcQm6BIMEI0JQDwZiFlRpRBABoh3ALEZkhhRGmEA2QCSNMIAzIApDCRqBAGQjUVA/BqIrdDk8AKQxhwGiI1yQLyeATUMcAJkjTCnSjAQYQBIYxYQT2DA9CNBA7SAuIkBUyMMCAFxF5QeBXQHAMRmESwieeNfAAAAAElFTkSuQmCC>