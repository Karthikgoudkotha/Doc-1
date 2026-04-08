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

### ---

**Example 5: Inaccessible Kaggle Dataset & Vague Instructions**

#### **1\. Metadata**

* **Task Type:** Capability  
* **Category / Domain:** Compliance  
* **Workflow:** Market Analysis  
* **Prompt Type:** Compliance Exam Finding  
* **Difficulty:** Medium

#### **2\. Prompt**

You are a State Insurance Department Market Conduct Examiner. The current date is October 24, 2026\.

Review the medical malpractice loss run dataset located at this Kaggle link: https://www.kaggle.com/datasets/gabrielsantello/medical-malpractice-insurance-dataset

Do an analysis of the dataset and tell me if the insurance company is compliant with regulations. Provide a report.

#### **3\. Rubric**

| \# | Description | Weight | Sources (with exact PDF pages) | Justification | Met | Failure Reasoning | Dependent Criteria |
| :---- | :---- | :---- | :---- | :---- | :---- | :---- | :---- |
| 1 | Downloads the Kaggle dataset successfully. | Critical | Kaggle Link | Prerequisite to analysis. | FALSE | Cannot access the paywalled link. |  |
| 2 | Identifies the number of claims in the dataset. | Major | Kaggle Link | Basic data extraction. | FALSE | Cannot see the data. | 1 |
| 3 | Calculates the average severity of the claims. | Major | Kaggle Link | Basic math. | FALSE | Cannot see the data. | 1 |
| 4 | Determines if the company is compliant. | Critical | Prompt Text | The main deliverable. | FALSE | Guessed at compliance status. | 3 |
| 5 | Does a good analysis of the data. | Major | Prompt Text | Subjective quality check. | FALSE | Analysis was poor. |  |
| 6 | Mentions the state regulations. | Minor | None | Context. | FALSE | Did not cite state law. |  |
| 7 | Recommends a fine for the company. | Major | None | Regulatory action. | FALSE | Did not recommend a fine. | 4 |
| 8 | Formats as a report. | Minor | Prompt Text | Output styling. | FALSE | Not formatted properly. |  |
| 9 | Uses professional tone. | Minor | Prompt Text | Tone alignment. | FALSE | Unprofessional tone. |  |
| 10 | Includes a conclusion paragraph. | Minor | Prompt Text | Structure. | FALSE | No conclusion. |  |

#### **4\. QA & Model Analysis**

**QA Failure: Source Quality, Prompt Unambiguity, & Rubric Target Focused**

This scenario fails on several critical fronts.

1. **Fails Source Quality:** The prompt relies on a Kaggle dataset link. Kaggle requires a user login and often sits behind a paywall/captcha. Large Language Models cannot autonomously log into third-party sites to download CSV files, making the prompt physically impossible to execute.  
2. **Fails Prompt Unambiguity:** The instruction "Do an analysis... and tell me if the company is compliant" is hopelessly vague. It does not specify *which* state's regulations to check, nor does it specify what compliance metric is being tested (e.g., claim denial timelines, reserve adequacy).  
3. **Fails Rubric Target Focused:** Criterion 5 ("Does a good analysis") is entirely subjective and lacks a verifiable end-state.

**Improvement:** The Kaggle dataset must be downloaded, converted to a raw, publicly accessible format (e.g., raw.githubusercontent.com), and attached directly to the prompt. The prompt must be rewritten to specify the exact compliance standard being tested (e.g., "Verify if all claims with a severity over 6 were settled within 30 days per NAIC guidelines"). The rubric must be updated to include hardcoded, deterministic mathematical targets for the data extraction.

#### **5\. Golden Response (Demonstrating the Flawed Output)**

**MARKET CONDUCT EXAM REPORT**

**Date:** October 24, 2026

**Subject:** Compliance Analysis

I am unable to complete this request. As an AI, I cannot access the provided Kaggle URL (https://www.kaggle.com/datasets/gabrielsantello/medical-malpractice-insurance-dataset) because it requires user authentication and is behind a login portal.

Furthermore, without specific details regarding which state's insurance regulations apply to this audit, or which specific compliance metrics (such as claim settlement timelines, fair claims practices, or reserve adequacy) you would like me to test the data against, I cannot provide a definitive determination of the company's regulatory compliance. Please provide the raw CSV data directly and specify the regulatory framework to be applied.

#### **Works cited**

1. Casualty Actuarial Society E-Forum, Winter 2019, accessed on April 5, 2026, [https://www.casact.org/sites/default/files/database/forum\_19wforum\_completewinter2019.pdf](https://www.casact.org/sites/default/files/database/forum_19wforum_completewinter2019.pdf)  
2. Specimen Reinsurance Agreement \- SEC.gov, accessed on April 5, 2026, [https://www.sec.gov/Archives/edgar/data/801019/000119312513170649/d480972dex9926g.htm](https://www.sec.gov/Archives/edgar/data/801019/000119312513170649/d480972dex9926g.htm)  
3. CP 00 10 10 12 \- Building and Personal Property Coverage Form, accessed on April 5, 2026, [https://www.propertyinsurancecoveragelaw.com/wp-content/uploads/2023/12/CP-00-10-10-12-Building-and-Personal-Property-Coverage-Form.pdf](https://www.propertyinsurancecoveragelaw.com/wp-content/uploads/2023/12/CP-00-10-10-12-Building-and-Personal-Property-Coverage-Form.pdf)  
4. 1.4 Building and Personal Property Coverage Form \- Risk & Insurance Education Alliance, accessed on April 5, 2026, [https://www.riskeducation.org/learn/pluginfile.php/276804/mod\_page/content/1/74%20-%201.4%20Building%20and%20Personal%20Property%20Coverage%20Form.pdf](https://www.riskeducation.org/learn/pluginfile.php/276804/mod_page/content/1/74%20-%201.4%20Building%20and%20Personal%20Property%20Coverage%20Form.pdf)  
5. CAUSES OF LOSS – SPECIAL FORM \- Property Insurance Coverage Law Blog, accessed on April 5, 2026, [https://www.propertyinsurancecoveragelaw.com/wp-content/uploads/2020/04/Policy-1.pdf](https://www.propertyinsurancecoveragelaw.com/wp-content/uploads/2020/04/Policy-1.pdf)  
6. PRO Form \- ABA Insurance Services, accessed on April 5, 2026, [https://www.abais.com/docs/default-source/banks/policy-specimen/property-policy-w-dec-coverage-forms-mandatory-endorsements-exclusions.pdf](https://www.abais.com/docs/default-source/banks/policy-specimen/property-policy-w-dec-coverage-forms-mandatory-endorsements-exclusions.pdf)  
7. Quota Share Reinsurance Agreement \- SEC.gov, accessed on April 5, 2026, [https://www.sec.gov/Archives/edgar/data/8497/000119312511081483/dex1023.htm](https://www.sec.gov/Archives/edgar/data/8497/000119312511081483/dex1023.htm)

[image1]: <data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAA8AAAAXCAYAAADUUxW8AAAAjUlEQVR4XmNgGAVDEHADsTi6IBJgBGIpIGZGlwABUSBeBcQm6BIMEI0JQDwZiFlRpRBABoh3ALEZkhhRGmEA2QCSNMIAzIApDCRqBAGQjUVA/BqIrdDk8AKQxhwGiI1yQLyeATUMcAJkjTCnSjAQYQBIYxYQT2DA9CNBA7SAuIkBUyMMCAFxF5QeBXQHAMRmESwieeNfAAAAAElFTkSuQmCC>