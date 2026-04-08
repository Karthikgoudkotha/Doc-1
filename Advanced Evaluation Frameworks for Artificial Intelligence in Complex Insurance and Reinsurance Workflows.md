# **Insurance Sector Evaluation: QA Rubric Failure Analysis (Negative Cases)**

The following dataset contains 20 comprehensive "negative case" examples designed to train evaluators and models on how to identify flawed prompts and rubrics in the insurance domain. Each scenario presents a prompt that is highly complex but ultimately fails one or more critical QA rubric criteria.

### ---

**Example 1: Reinsurance Ledger combined with B2C Marketing**

#### **1\. Metadata**

* **Task Type:** Workflow  
* **Category / Domain:** Reinsurance  
* **Workflow:** Quantum Calculation  
* **Prompt Type:** Reinsurance Bordereau & Marketing Copy  
* **Difficulty:** High

#### **2\. Prompt**

You are a Reinsurance Accounting Manager. The current date is November 15, 2023\.

You are required to process the third-quarter (Q3) bordereau and calculate the exact net settlement amount between the Ceding Company and the Reinsurer under a Quota Share agreement.

**Q3 2023 Financial Data:**

* Gross Net Written Premium (GNWP): $18,500,000  
* Gross Earned Premium: $14,000,000  
* Gross Losses Paid: $4,200,000

**Treaty Terms:**

* Quota Share Percentage: 25%  
* Ceding Commission: 24.0%  
* Loss Expense Allowance (LAE): 10.00% of Net Ceded Earned Premium.

Once you have completed the mathematical ledger, switch roles to a Direct-to-Consumer Marketing Executive. Write a 500-word, highly engaging Facebook ad campaign designed to sell more underlying coastal property policies to 18-to-25-year-olds in Texas to help boost the treaty volume for the next quarter.

**Attached Files (Context):**

* **Quota\_Share\_Treaty\_2023.pdf** – Public URL: https://www.sec.gov/Archives/edgar/data/1722438/000162828021005117/exhibit1032-sx4xscor\_capit.htm  
* **TWIA\_Agreement\_Base.pdf** – Public URL: https://www.twia.org/wp-content/uploads/2017/04/4.-ARDP-Quota-Share-Reinsurance-Contract\_Final.pdf

#### **3\. Rubric**

| \# | Description | Weight | Sources (with exact PDF pages) | Justification | Met | Failure Reasoning | Dependent Criteria |
| :---- | :---- | :---- | :---- | :---- | :---- | :---- | :---- |
| 1 | Calculates the Ceded Written Premium as $4,625,000. | Major | Prompt Text; Quota\_Share\_Treaty\_2023.pdf, PDF Page 15 | $18,500,000 GNWP multiplied by 25%. | FALSE | Failed to apply quota share. |  |
| 2 | Calculates the Ceded Earned Premium as $3,500,000. | Major | Prompt Text | $14,000,000 Gross Earned multiplied by 25%. | FALSE | Used written premium. |  |
| 3 | Calculates the Ceding Commission as $1,110,000. | Major | Prompt Text; TWIA\_Agreement\_Base.pdf, PDF Page 10 | $4,625,000 multiplied by 0.24. | FALSE | Used Earned Premium. | 1 |
| 4 | Calculates the Ceded Paid Losses as $1,050,000. | Major | Prompt Text | $4,200,000 multiplied by 25%. | FALSE | Billed 100% of losses. |  |
| 5 | Calculates the LAE Allowance as $350,000. | Major | Prompt Text | $3,500,000 multiplied by 0.10. | FALSE | Used Written Premium. | 2 |
| 6 | Calculates the Net Settlement as $2,115,000 due to Reinsurer. | Critical | Reinsurance Principles | $4,625,000 \- ($1,110,000 \+ $1,050,000 \+ $350,000). | FALSE | Arrived at an incorrect quantum. | 1 |
| 7 | Writes a 500-word Facebook ad. | Minor | Prompt Text | Secondary prompt instruction. | FALSE | Wrote a business memo. |  |
| 8 | Mentions coastal property insurance in the ad. | Minor | Prompt Text | Adheres to product constraint. | FALSE | Advertised auto insurance. |  |
| 9 | Uses engaging, youth-oriented language. | Minor | Prompt Text | Tone alignment. | FALSE | Used actuarial language. |  |
| 10 | Connects the ad to boosting "treaty volume". | Minor | Prompt Text | Ties ad to reinsurance. | FALSE | Omitted justification. |  |

#### **4\. QA & Model Analysis**

**QA Failure: Prompt Scope Focus & Prompt Representativeness**

This prompt intentionally fails the **Prompt Scope Focus** and **Prompt Representativeness** criteria. While calculating a reinsurance ledger is a realistic workflow for a Reinsurance Accounting Manager, suddenly shifting to writing B2C social media marketing copy introduces an unrelated requirement. Reinsurance accountants do not write Facebook ads. Because it forces two disjointed workflows, it breaks cohesion.

**Improvement:** Delete the marketing request entirely so the prompt remains tightly scoped to the Net Settlement calculation.

### ---

**Example 2: Medical Underwriting with Contradictory Facts**

#### **1\. Metadata**

* **Task Type:** Capability  
* **Category / Domain:** Underwriting  
* **Workflow:** Eligibility Assessment  
* **Prompt Type:** Underwriting Review  
* **Difficulty:** Medium

#### **2\. Prompt**

You are a Life Underwriter. The current date is August 10, 2023\.

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
| 1 | Identifies Nancy's age as 55\. | Minor | Prompt Text | Establishes age bracket. | FALSE | Used the \>71 bracket. |  |
| 2 | Identifies mother died of Alzheimer's at age 60\. | Major | Prompt Text | Extracts factual event. | FALSE | Ignored death. |  |
| 3 | Identifies assumption of zero family history. | Major | Prompt Text | Extracts contradictory rule. | FALSE | Ignored assumption. |  |
| 4 | Evaluates Nationwide "Preferred Plus" family history rules. | Critical | NFM-23978AO-WG.pdf, PDF Page 9 | Guide lists cardiovascular/cancer rules. | FALSE | Failed to locate section. |  |
| 5 | Attempts to resolve the contradiction in the prompt. | Major | Underwriting Principles | Navigates the paradox. | FALSE | Ignored one fact. | 2 |
| 6 | Determines Alzheimer's is not a knockout anyway. | Major | NFM-23978AO-WG.pdf, PDF Page 9 | Guide only specifies cardiovascular/cancer. | FALSE | Assumed Alzheimer's is a knockout. | 4 |
| 7 | Identifies BP (110/70) is within limits. | Minor | NFM-23978AO-WG.pdf, PDF Page 9 | Confirms BP eligibility. | FALSE | Stated BP was too high. |  |
| 8 | Concludes Nancy is eligible for Preferred Plus. | Critical | Prompt Text; NFM-23978AO-WG.pdf, PDF Page 9 | Resolves the final eligibility. | FALSE | Declined Nancy. | 6 |
| 9 | Formats as a formal Underwriting Review. | Minor | Prompt Text | Output styling. | FALSE | Provided bulleted list. |  |
| 10 | Uses professional terminology. | Minor | Prompt Text | Tone alignment. | FALSE | Used casual language. |  |

#### **4\. QA & Model Analysis**

**QA Failure: Prompt Logical Consistency**

This prompt intentionally fails the **Prompt Logical Consistency** criterion. The narrative states Nancy's mother died of Alzheimer's, but the mandatory assumptions dictate she has *zero* family history of Alzheimer's. This creates a direct internal contradiction, making the prompt logically inexecutable as written.

**Improvement:** Remove the contradictory assumption. Present coherent facts (mother died of Alzheimer's) and force the model to look up if Alzheimer's disqualifies an applicant under Nationwide's specific guidelines.

### ---

**Example 3: Unanchored Time References in Actuarial Data**

#### **1\. Metadata**

* **Task Type:** Workflow  
* **Category / Domain:** Actuarial  
* **Workflow:** Rate Indication Generation  
* **Prompt Type:** Actuarial Rate Review Memo  
* **Difficulty:** Medium

#### **2\. Prompt**

You are a Pricing Actuary. Attached is historical commercial property claims data from the NAIC.

Reference last year's claims data and calculate the projected loss ratio. Apply a 5% historical trend factor to last year's raw losses to determine the prospective ultimate loss, and divide that by last year's earned premium. Present the findings in an Actuarial Memo.

**Attached Files (Context):**

* **TWIA\_Commercial\_Memo.pdf** – Public URL: https://www.twia.org/wp-content/uploads/Commercial-Memo-2024.pdf

#### **3\. Rubric**

| \# | Description | Weight | Sources (with exact PDF pages) | Justification | Met | Failure Reasoning | Dependent Criteria |
| :---- | :---- | :---- | :---- | :---- | :---- | :---- | :---- |
| 1 | Identifies the data schema of the historical data. | Minor | TWIA\_Commercial\_Memo.pdf, PDF Page 12 | Validates understanding. | FALSE | Hallucinated structure. |  |
| 2 | Extracts the Earned Premium for "last year". | Major | Prompt Text | Requires identifying the correct year. | FALSE | Extracted wrong year. |  |
| 3 | Extracts the Incurred Losses for "last year". | Major | Prompt Text | Requires identifying the correct year. | FALSE | Extracted wrong year. |  |
| 4 | Applies the 5% trend factor to the losses. | Critical | Prompt Text | Losses \* 1.05. | FALSE | Applied to premium. | 3 |
| 5 | Calculates the projected loss ratio. | Critical | Actuarial Principles | (Trended Losses) / Premium. | FALSE | Incorrect division. | 2 |
| 6 | Formats the output as an Actuarial Memo. | Minor | Prompt Text | Narrative styling. | FALSE | Unformatted block. |  |
| 7 | Explains the purpose of the 5% trend factor. | Minor | Actuarial Principles | Contextualizes math. | FALSE | Omitted justification. |  |
| 8 | Rounds the loss ratio to one decimal place. | Minor | Industry Standard | Formatting convention. | FALSE | Provided endless decimal. | 5 |
| 9 | Explicitly states the calendar year used. | Major | Prompt Text | Identifies interpretation. | FALSE | Kept year ambiguous. |  |
| 10 | Concludes with an indication of rate changes. | Minor | Actuarial Principles | Practical application. | FALSE | No recommendation. | 5 |

#### **4\. QA & Model Analysis**

**QA Failure: Prompt Temporal Stability & Rubric Temporal Stability**

This prompt fails the **Temporal Stability** criterion. It uses "last year's" claims data without providing a static "current date" anchor. An LLM running this in 2024 will use 2023 data, while one in 2026 will look for 2025 data, destroying reproducibility. Consequently, the rubric cannot provide deterministic hardcoded values.

**Improvement:** Anchor the time period explicitly (e.g., "The current date is May 1, 2024\. Reference the claims data for Accident Year 2023"). Update the rubric to include exact hardcoded mathematical answers.

### ---

**Example 4: Coinsurance Penalty (Rubric Atomicity and Math Errors)**

#### **1\. Metadata**

* **Task Type:** Workflow  
* **Category / Domain:** Claims Handling  
* **Workflow:** Quantum Calculation  
* **Prompt Type:** Claims Determination  
* **Difficulty:** High

#### **2\. Prompt**

You are an Adjuster. The current date is December 1, 2023\. Calculate the final payable amount for a fire loss using the attached ISO CP 00 10 policy.

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
| 1 | Extracts Limit of $7M, Value of $10M, 80% coinsurance, and $2M damage. | Minor | Prompt Text | Basic reading comprehension. | FALSE | Missed a number. |  |
| 2 | Calculates the Required Limit as $8,000,000. | Major | ISO\_CP\_00\_10\_10\_12.pdf, PDF Page 11 | $10M \* 80% \= $8M. | FALSE | Failed calculation. |  |
| 3 | Calculates the penalty factor as 1.14. | Critical | ISO\_CP\_00\_10\_10\_12.pdf, PDF Page 11 | $8,000,000 / $7,000,000 \= 1.14. | FALSE | Failed division. | 2 |
| 4 | Subtracts the deductible, multiplies by the penalty factor, and checks the limit. | Critical | ISO\_CP\_00\_10\_10\_12.pdf, PDF Page 11 | ($2,000,000 \- $25,000) \* 1.14. | FALSE | Did math wrong. | 3 |
| 5 | Uses professional tone. | Minor | Prompt Text | Tone alignment. | FALSE | Unprofessional. |  |
| 6 | States the date is Dec 1, 2023\. | Minor | Prompt Text | Attention to detail. | FALSE | Omitted date. |  |
| 7 | Explains coinsurance concept. | Minor | Insurance Principles | Context. | FALSE | Omitted explanation. |  |
| 8 | Mentions the property is in San Francisco. | Minor | Prompt Text | Detail. | FALSE | Omitted location. |  |
| 9 | Recommends dropping the client. | Minor | None | Opinion. | FALSE | No recommendation. |  |
| 10 | Output is exactly 100 words. | Minor | None | Arbitrary constraint. | FALSE | Wrong word count. |  |

#### **4\. QA & Model Analysis**

**QA Failure: Rubric Factual/Mathematical Accuracy & Rubric Atomicity**

This scenario demonstrates a severely flawed **Rubric**.

1. **Fails Atomicity:** Row 1 combines 4 data extractions, and Row 4 combines 3 mathematical steps into single criteria, making objective binary scoring impossible.  
2. **Fails Mathematical Accuracy:** Row 3 calculates the penalty backward (Should/Did instead of Did/Should). It should be $7M/$8M \= 0.875. Row 4 subtracts the deductible *before* the penalty, violating ISO CP 00 10 conditions.  
3. **Fails Necessity:** Rows 9 and 10 introduce arbitrary, unprompted constraints (dropping the client, exactly 100 words).  
   **Improvement:** Rewrite the rubric. Split Rows 1 and 4 into atomic steps. Correct the math (0.875) and order of operations. Remove Rows 9 and 10\.

### ---

**Example 5: Inaccessible Internal Dataset & Vague Instructions**

#### **1\. Metadata**

* **Task Type:** Capability  
* **Category / Domain:** Compliance  
* **Workflow:** Market Analysis  
* **Prompt Type:** Compliance Exam Finding  
* **Difficulty:** Medium

#### **2\. Prompt**

You are a State Insurance Department Market Conduct Examiner. The current date is March 1, 2023\.

Review the medical malpractice loss run dataset located on our internal compliance drive here: https://sharepoint.internal.state.gov/sites/compliance/medmal\_loss\_run\_2023.csv

Do an analysis of the dataset and tell me if the insurance company is compliant with regulations. Provide a report.

#### **3\. Rubric**

| \# | Description | Weight | Sources (with exact PDF pages) | Justification | Met | Failure Reasoning | Dependent Criteria |
| :---- | :---- | :---- | :---- | :---- | :---- | :---- | :---- |
| 1 | Downloads the internal dataset successfully. | Critical | Intranet Link | Prerequisite to analysis. | FALSE | Cannot access private link. |  |
| 2 | Identifies the number of claims. | Major | Intranet Link | Basic data extraction. | FALSE | Cannot see data. | 1 |
| 3 | Calculates the average severity. | Major | Intranet Link | Basic math. | FALSE | Cannot see data. | 1 |
| 4 | Determines if the company is compliant. | Critical | Prompt Text | Main deliverable. | FALSE | Guessed status. | 3 |
| 5 | Does a good analysis of the data. | Major | Prompt Text | Subjective check. | FALSE | Analysis was poor. |  |
| 6 | Mentions the state regulations. | Minor | None | Context. | FALSE | Did not cite law. |  |
| 7 | Recommends a fine. | Major | None | Regulatory action. | FALSE | Did not recommend fine. | 4 |
| 8 | Formats as a report. | Minor | Prompt Text | Output styling. | FALSE | Not formatted. |  |
| 9 | Uses professional tone. | Minor | Prompt Text | Tone alignment. | FALSE | Unprofessional. |  |
| 10 | Includes a conclusion paragraph. | Minor | Prompt Text | Structure. | FALSE | No conclusion. |  |

#### **4\. QA & Model Analysis**

**QA Failure: Source Quality, Prompt Unambiguity, & Rubric Target Focused**

1. **Fails Source Quality:** Relies on an internal SharePoint link. LLMs cannot log into private corporate networks to download CSVs, making execution impossible.  
2. **Fails Unambiguity:** "Do an analysis... and tell me if compliant" is hopelessly vague (which state? what metric?).  
3. **Fails Target Focused:** Criterion 5 ("Does a good analysis") is subjective without a verifiable end-state.  
   **Improvement:** Convert the dataset to a public raw CSV link. Specify the exact compliance standard (e.g., "Verify if claims over severity 6 settled within 30 days"). Replace subjective rubric criteria with deterministic targets.

### ---

**Example 6: Real Estate E\&O (Language Representativeness Failure)**

#### **1\. Metadata**

* **Task Type:** Workflow  
* **Category / Domain:** Claims Handling  
* **Workflow:** Coverage Verification  
* **Prompt Type:** Claims Determination Letter  
* **Difficulty:** Medium

#### **2\. Prompt**

You are a Claims Adjuster. The current date is May 10, 2023\.

Our insured is a licensed real estate broker covered under the attached Real Estate Professional Liability policy. The broker was served with a lawsuit alleging they refused to show a property to a buyer because of religion, resulting in a claim for severe emotional distress.

Read the attached policy. Write a letter to the insured broker telling them they are a "totally bad apple," that our company "doesn't insure haters," and definitively deny their claim based on the policy exclusions.

**Attached Files (Context):**

* **D43180\_0819\_CA\_Real\_Estate\_Specimen.pdf** – Public URL: https://www.landy.com/apps/specimen\_policies/D43180\_0819\_CA\_Real\_Estate\_Specimen.pdf

#### **3\. Rubric**

| \# | Description | Weight | Sources (with exact PDF pages) | Justification | Met | Failure Reasoning | Dependent Criteria |
| :---- | :---- | :---- | :---- | :---- | :---- | :---- | :---- |
| 1 | Identifies claim is for emotional distress. | Minor | Prompt Text | Stated in prompt. | FALSE | Ignored distress aspect. |  |
| 2 | Identifies claim involves religious discrimination. | Major | Prompt Text | Stated in prompt. | FALSE | Missed religious aspect. |  |
| 3 | Locates exclusion for discrimination. | Critical | D43180\_0819\_CA\_Real\_Estate\_Specimen.pdf, PDF Page 6 | Section IV.C excludes discrimination claims. | FALSE | Failed to find exclusion. |  |
| 4 | Notes Fair Housing Claims have a sub-limit exception. | Major | D43180\_0819\_CA\_Real\_Estate\_Specimen.pdf, PDF Page 6 | Exception exists for Fair Housing. | FALSE | Denied claim entirely. | 3 |
| 5 | Denies the claim outright. | Critical | Prompt Text | Prompt instruction. | FALSE | Provided defense under sub-limit. | 3 |
| 6 | Calls insured a "totally bad apple". | Minor | Prompt Text | Forced constraint. | FALSE | Omitted insult. |  |
| 7 | States company "doesn't insure haters." | Minor | Prompt Text | Forced constraint. | FALSE | Omitted phrase. |  |
| 8 | Formats response as a letter. | Minor | Prompt Text | Output styling. | FALSE | Wrote a memo. |  |
| 9 | Explains emotional distress is Bodily Injury. | Major | D43180\_0819\_CA\_Real\_Estate\_Specimen.pdf, PDF Page 5 | Section III.B defines Bodily Injury. | FALSE | Claimed it wasn't bodily injury. |  |
| 10 | Includes date May 10, 2023\. | Minor | Prompt Text | Temporal anchor. | FALSE | Wrong date. |  |

#### **4\. QA & Model Analysis**

**QA Failure: Prompt Language Representativeness & Rubric Professional Alignment**

This prompt fails **Language Representativeness** by forcing highly unprofessional slang ("totally bad apple", "doesn't insure haters"). A professional adjuster would never use this language in a formal coverage letter. Furthermore, forcing a complete denial (Criterion 5\) contradicts the policy text providing a sub-limit exception for Fair Housing claims (Criterion 4), creating a Logical Consistency failure.

**Improvement:** Rewrite to use industry-standard language. Remove instructions to insult the insured. Ask the model to draft a professional "Reservation of Rights" evaluating the discrimination allegations against the Fair Housing sub-limit exception on PDF Page 6\.

### ---

**Example 7: Fiduciary Liability Limits (Factual Accuracy Failure)**

#### **1\. Metadata**

* **Task Type:** Workflow  
* **Category / Domain:** Claims Handling  
* **Workflow:** Quantum Calculation  
* **Prompt Type:** Claims Apportionment Memo  
* **Difficulty:** High

#### **2\. Prompt**

You are a Professional Liability Claims Adjuster. The current date is July 20, 2023\.

Your insured, a corporate board of trustees, was sued for breaching their fiduciary duties under ERISA. The lawsuit has been settled. You must calculate the final out-of-pocket costs for the insured and the total amount paid by the insurance company based on the attached Fiduciary Liability policy.

**Claim Details:**

* Aggregate Limit of Liability: $1,000,000.  
* Retention (Deductible): $50,000.  
* Defense Costs Incurred: $250,000.  
* Final Settlement Amount: $900,000.

Under standard liability insurance practices, defense costs are paid in addition to the policy limit. Therefore, apply the retention to the settlement amount, pay the remainder of the settlement out of the policy limit, and pay the defense costs entirely outside of the limit. Provide the final calculation in a memo.

**Attached Files (Context):**

* **The-Encore-Fiduciary-Liability-Policy-SPECIMEN.pdf** – Public URL: https://encorefiduciary.com/wp-content/uploads/2024/01/The-Encore-Fiduciary-Liability-Policy-SPECIMEN-1.12v1.pdf

#### **3\. Rubric**

| \# | Description | Weight | Sources (with exact PDF pages) | Justification | Met | Failure Reasoning | Dependent Criteria |
| :---- | :---- | :---- | :---- | :---- | :---- | :---- | :---- |
| 1 | Identifies Limit of Liability as $1,000,000. | Minor | Prompt Text | Stated in prompt. | FALSE | Missed limit. |  |
| 2 | Identifies Retention as $50,000. | Minor | Prompt Text | Stated in prompt. | FALSE | Missed retention. |  |
| 3 | Subtracts $50,000 retention from $900,000 settlement. | Major | Prompt Text | $900k \- $50k \= $850k. | FALSE | Applied retention to defense. |  |
| 4 | Calculates insurer's settlement payout as $850,000. | Critical | Prompt Text | Remaining settlement balance. | FALSE | Math error. | 3 |
| 5 | Determines insurer pays $250,000 for defense in addition to settlement. | Major | Prompt Text | Explicit prompt instruction. | FALSE | Deducted defense from limit. |  |
| 6 | Calculates total insurer payout as $1,100,000. | Critical | Prompt Text | $850k \+ $250k \= $1.1M. | FALSE | Refused to pay \>$1M. | 4 |
| 7 | Concludes insured's total out-of-pocket cost is exactly $50,000. | Critical | Prompt Text | Insured only pays retention. | FALSE | Charged insured for overage. |  |
| 8 | Formats output as a Claims Apportionment Memo. | Minor | Prompt Text | Output styling. | FALSE | Unstructured list. |  |
| 9 | Includes date July 20, 2023\. | Minor | Prompt Text | Temporal anchor. | FALSE | Missing date. |  |
| 10 | Uses step-by-step mathematical reasoning. | Minor | Prompt Text | Ensures readability. | FALSE | Provided final numbers only. |  |

#### **4\. QA & Model Analysis**

**QA Failure: Prompt Logical Consistency & Rubric Factual/Mathematical Accuracy**

The prompt instructs the model to treat the policy as a standard liability policy where defense costs are paid *in addition* to the limit. However, the attached Encore document explicitly states on PDF Page 2 in bold: "CLAIM EXPENSES ARE INCLUDED IN THE LIMITS OF LIABILITY." Because the prompt contradicts the source document, the model is forced into a paradox. The rubric enforces the mathematically incorrect calculation (totaling $1.1M against a $1M limit) based on the flawed prompt.

**Improvement:** Remove the false assumption ("Under standard liability insurance practices..."). Instruct the model to determine whether defense costs erode the limit based *strictly* on the attached policy wording, and calculate the true financial apportionment accordingly.

### ---

**Example 8: Crop Insurance Prevented Planting (Target Focused Failure)**

#### **1\. Metadata**

* **Task Type:** Capability  
* **Category / Domain:** Underwriting  
* **Workflow:** Eligibility Assessment  
* **Prompt Type:** Underwriting Memo  
* **Difficulty:** Low

#### **2\. Prompt**

You are a Crop Insurance Underwriter. The current date is September 12, 2023\.

A farmer was unable to plant their insured corn crop due to severe flooding that occurred two days prior to the final planting date for their county. They want to know if they qualify for a Prevented Planting payment.

Review the attached USDA RMA Basic Provisions. Write a memo explaining what prevented planting means and advise the farmer on whether they are eligible based on the timing of the flood.

**Attached Files (Context):**

* **Basic-Provisions-23-BR.pdf** – Public URL: https://www.rma.usda.gov/sites/default/files/2024-06/Basic-Provisions-23-BR.pdf

#### **3\. Rubric**

| \# | Description | Weight | Sources (with exact PDF pages) | Justification | Met | Failure Reasoning | Dependent Criteria |
| :---- | :---- | :---- | :---- | :---- | :---- | :---- | :---- |
| 1 | Explains prevented planting. | Critical | Basic-Provisions-23-BR.pdf, PDF Page 4 | Basic definition. | FALSE | Failed to explain concept. |  |
| 2 | Does a good job analyzing the farmer's situation. | Major | Prompt Text | Evaluates quality. | FALSE | Analysis was poor. |  |
| 3 | Defines the "Coverage begins, date" as stated in criterion 1\. | Major | Basic-Provisions-23-BR.pdf, PDF Page 4 | References another row. | FALSE | Failed to link definitions. | 1 |
| 4 | Tells the farmer if they are eligible. | Critical | Prompt Text | Main deliverable. | FALSE | No clear yes/no. |  |
| 5 | Writes a memo. | Minor | Prompt Text | Formatting. | FALSE | Not a memo. |  |
| 6 | Uses correct grammar and spelling. | Minor | None | Quality check. | FALSE | Had typos. |  |
| 7 | Is helpful and polite. | Minor | None | Tone check. | FALSE | Was rude. |  |
| 8 | Mentions flood happened two days before deadline. | Minor | Prompt Text | Detail check. | FALSE | Forgot timeline. |  |
| 9 | Recommends planting soybeans instead. | Additional | None | Helpful advice. | FALSE | Did not suggest alternate crops. |  |
| 10 | The response is easy to read. | Major | None | Readability check. | FALSE | Hard to read. |  |

#### **4\. QA & Model Analysis**

**QA Failure: Rubric Target Focused, Self-Contained, & Necessity**

1. **Fails Target Focused:** Criteria 1, 2, 4, 6, 7, and 10 rely on subjective measures ("Explains," "Does a good job," "Is helpful," "easy to read"). There are no verifiable, deterministic "Ground Truth" extractions.  
2. **Fails Self-Contained:** Criterion 3 states "as stated in criterion 1." Evaluators must be able to read a criterion in isolation.  
3. **Fails Necessity:** Criterion 9 asks the model to recommend planting soybeans, penalizing a good response for omitting an unprompted feature.  
   **Improvement:** Rewrite rubric deterministically. e.g., Criterion 1: "Extracts definition of Prevented Planting as 'Failure to plant the insured crop... due to an insured cause of loss.'" Replace subjective criteria with exact factual extractions.

### ---

**Example 9: Marine Cargo Code Execution (Scope Focus & Atomicity Failure)**

#### **1\. Metadata**

* **Task Type:** Capability  
* **Category / Domain:** Claims Handling  
* **Workflow:** Coverage Verification  
* **Prompt Type:** Claims Determination & Python Script  
* **Difficulty:** High

#### **2\. Prompt**

You are a Marine Cargo adjuster. The current date is February 10, 2023\.

A cargo ship carrying 500 crates of sensitive electronics sank during a hurricane, resulting in a total loss. The cargo was insured under the attached Institute Cargo Clauses (A) 2009\.

First, determine if the sinking of the vessel is a covered peril under the Risks Clause.

Second, I need you to create a Python script using the pandas library. The script must generate a synthetic dataset of 500 crates, assign a random dollar value between $1,000 and $5,000 to each crate, and calculate the total simulated loss amount. Output the executable Python code.

**Attached Files (Context):**

* **institute-cargo-clauses-a-2009.pdf** – Public URL: https://www.if-insurance.com/globalassets/industrial/files/marine-cargo/institute-clauses/institute-cargo-clauses-a-2009.pdf

#### **3\. Rubric**

| \# | Description | Weight | Sources (with exact PDF pages) | Justification | Met | Failure Reasoning | Dependent Criteria |
| :---- | :---- | :---- | :---- | :---- | :---- | :---- | :---- |
| 1 | Determines loss is covered, extracts general average clause, and writes a valid Python script to simulate values. | Critical | institute-cargo-clauses-a-2009.pdf, PDF Page 1 | Combines multiple instructions. | FALSE | Failed one or more bundled tasks. |  |
| 2 | Imports pandas library in the script. | Minor | Prompt Text | Required module. | FALSE | Used base Python. | 1 |
| 3 | Generates exactly 500 rows in dataset. | Major | Prompt Text | Specified crate count. | FALSE | Generated wrong row count. | 1 |
| 4 | Uses random number generator between $1,000 and $5,000. | Major | Prompt Text | Specified value range. | FALSE | Used fixed values. | 1 |
| 5 | Calculates the sum of generated values. | Critical | Prompt Text | Final required output. | FALSE | Failed to print sum. | 1 |
| 6 | Notes Clause 4 excludes willful misconduct. | Minor | institute-cargo-clauses-a-2009.pdf, PDF Page 1 | Standard policy exclusion. | FALSE | Omitted exclusions. |  |
| 7 | Notes Clause 6 excludes war risks. | Minor | institute-cargo-clauses-a-2009.pdf, PDF Page 2 | Standard policy exclusion. | FALSE | Omitted war risks. |  |
| 8 | Notes Clause 7 excludes strikes and terrorism. | Minor | institute-cargo-clauses-a-2009.pdf, PDF Page 2 | Standard policy exclusion. | FALSE | Omitted terrorism risks. |  |
| 9 | Confirms hurricane is not an excluded peril under clauses 4-7. | Major | institute-cargo-clauses-a-2009.pdf, PDF Pages 1-2 | Connects event to absence of exclusion. | FALSE | Claimed hurricanes excluded. |  |
| 10 | Includes comments in Python code. | Minor | Coding Best Practices | Standard practice. | FALSE | Wrote uncommented code. | 1 |

#### **4\. QA & Model Analysis**

**QA Failure: Prompt Scope Focus & Rubric Atomicity**

1. **Fails Scope Focus:** Evaluating an insurance contract and writing a functional Python pandas script are unrelated professional objectives. The prompt forces the LLM to span two disparate domains (Insurance and Software Engineering).  
2. **Fails Rubric Atomicity:** Criterion 1 asks the evaluator to check if the model did three distinct things: determine coverage, extract average rules, AND write a valid Python script. If the model determines coverage correctly but writes bad code, the evaluator cannot score Criterion 1 accurately.  
   **Improvement:** The prompt must be split. Delete the Python coding request to keep the workflow focused on marine insurance adjudication. Split Criterion 1 into atomic rows.

### ---

**Example 10: Cyber Insurance Extortion (Logic Complexity & Necessity Failure)**

#### **1\. Metadata**

* **Task Type:** Capability  
* **Category / Domain:** Agent/Broker  
* **Workflow:** Policy Explanation  
* **Prompt Type:** Email to Client  
* **Difficulty:** Low

#### **2\. Prompt**

You are an insurance broker. The current date is October 24, 2023\.

Look at the attached Cyber Insurance policy from Klapton. What is the exact name of Chapter 7? Write your response to the client in an email that is exactly 50 words long, no more, no less.

**Attached Files (Context):**

* **KIC-Cyber-Policy-Wording\_Final-2024-new-31.1.2025.pdf** – Public URL: https://www.klapton.com/wp-content/uploads/2025/01/KIC-Cyber-Policy-Wording\_Final-2024-new-31.1.2025.pdf

#### **3\. Rubric**

| \# | Description | Weight | Sources (with exact PDF pages) | Justification | Met | Failure Reasoning | Dependent Criteria |
| :---- | :---- | :---- | :---- | :---- | :---- | :---- | :---- |
| 1 | Identifies the name of Chapter 7\. | Critical | KIC-Cyber-Policy-Wording.pdf, PDF Page 15 | Document lists title. | FALSE | Named wrong chapter. |  |
| 2 | States the name is "Extortion and Ransom Indemnity". | Critical | KIC-Cyber-Policy-Wording.pdf, PDF Page 15 | Exact text extraction. | FALSE | Hallucinated title. | 1 |
| 3 | The output is exactly 50 words long. | Major | Prompt Text | Arbitrary constraint. | FALSE | Word count was 49 or 51\. |  |
| 4 | Includes a greeting like "Dear Client". | Minor | Prompt Text | Standard format. | FALSE | No greeting. |  |
| 5 | Includes a sign-off like "Sincerely, Broker". | Minor | Prompt Text | Standard format. | FALSE | No sign-off. |  |
| 6 | Mentions the word "Cyber". | Minor | Prompt Text | General context. | FALSE | Omitted word. |  |
| 7 | Mentions the word "Policy". | Minor | Prompt Text | General context. | FALSE | Omitted word. |  |
| 8 | Is written in English. | Minor | None | Basic assumption. | FALSE | Written in Spanish. |  |
| 9 | Does not contain any typos. | Minor | None | Quality check. | FALSE | Contained errors. |  |
| 10 | Answers the prompt accurately. | Critical | Prompt Text | Overall assessment. | FALSE | Failed main objective. | 1 |

#### **4\. QA & Model Analysis**

**QA Failure: Prompt Logic Complexity & Rubric Necessity**

1. **Fails Logic Complexity:** The prompt requires zero analytical depth or domain expertise. It is a rudimentary "Ctrl+F" document retrieval task. It does not require a sequence of calculations or layered reasoning, making it too simple to evaluate an advanced LLM's capacity for insurance workflows.  
2. **Fails Rubric Necessity:** Criterion 3 requires exactly 50 words. This is an arbitrary, "optional" feature that is not strictly required for a professional-grade response, creating "noise" in the training signal.  
   **Improvement:** Increase logic complexity by providing a loss scenario (e.g., a $50,000 ransom demand) and asking the model to evaluate coverage against definitions within Chapter 7\. Remove the arbitrary 50-word constraint.

### ---

**Example 11: ALTA Closing Protection Letter (Temporal Stability Failure)**

#### **1\. Metadata**

* **Task Type:** Workflow  
* **Category / Domain:** Claims Handling  
* **Workflow:** Claims Adjudication  
* **Prompt Type:** Claims Determination Memo  
* **Difficulty:** Medium

#### **2\. Prompt**

You are a Title Claims Counsel. Today is a great day to review claims.

A lender filed a claim last week regarding a closing that happened two months ago. The issuing agent stole $50,000 of the lender's funds instead of establishing the mortgage. Using the attached ALTA Closing Protection Letter (CPL), do we indemnify the lender for this loss? Write a memo explaining your decision based on the specific exclusions in the letter.

**Attached Files (Context):**

* **ALTA\_CPL\_Single\_Transaction.pdf** – Public URL: https://www.comparetitlecompanies.com/pdf/ALTA\_CPL.pdf

#### **3\. Rubric**

| \# | Description | Weight | Sources (with exact PDF pages) | Justification | Met | Failure Reasoning | Dependent Criteria |
| :---- | :---- | :---- | :---- | :---- | :---- | :---- | :---- |
| 1 | Identifies policy as ALTA Closing Protection Letter. | Minor | ALTA\_CPL\_Single\_Transaction.pdf, PDF Page 1 | Document ID. | FALSE | Hallucinated Title policy. |  |
| 2 | Extracts the Date from "two months ago". | Major | Prompt Text | Relative time calculation. | FALSE | Could not determine date. |  |
| 3 | Identifies Addressee as the lender. | Minor | Prompt Text | Associates actor with field. | FALSE | Assumed Addressee was closer. |  |
| 4 | Identifies loss amount as $50,000. | Minor | Prompt Text | Stated in prompt. | FALSE | Missed amount. |  |
| 5 | Evaluates Exclusion 4(a)(i) regarding disbursement of funds. | Major | ALTA\_CPL\_Single\_Transaction.pdf, PDF Page 1 | Failure to follow instructions. | FALSE | Missed exclusion. |  |
| 6 | Evaluates fraud/theft provision under Section 4(b). | Critical | ALTA\_CPL\_Single\_Transaction.pdf, PDF Page 1 | CPL explicitly covers theft. | FALSE | Denied claim stating theft excluded. |  |
| 7 | Concludes insurer must indemnify lender for $50,000. | Critical | Prompt Text; ALTA\_CPL.pdf, PDF Page 1 | Applies provision to facts. | FALSE | Denied claim. | 6 |
| 8 | Formats response as a Claims Determination Memo. | Minor | Prompt Text | Narrative styling. | FALSE | Wrote short email. |  |
| 9 | Uses professional claims terminology. | Minor | Prompt Text | Tone alignment. | FALSE | Used unprofessional language. |  |
| 10 | Includes exact date memo was written. | Major | Prompt Text | Temporal anchor. | FALSE | Omitted date. |  |

#### **4\. QA & Model Analysis**

**QA Failure: Prompt Temporal Stability & Rubric Temporal Stability**

This prompt fails the **Temporal Stability** criterion. It uses entirely relative time references ("Today is a great day", "last week", "two months ago") without providing a hardcoded current date anchor. Because the attached ALTA form was published in 2021, and the prompt might be executed in 2023 or 2026, the model will hallucinate dates that do not logically align with a static benchmark. The rubric (Criterion 2\) forces the evaluator to check if the model correctly calculated the date from "two months ago," which is impossible to standardize across sessions.

**Improvement:** Anchor the date to a fixed point in the past (e.g., "The current date is May 1, 2022\. The closing occurred on March 1, 2022."). Update rubric to remove relative date calculations.

### ---

**Example 12: York-Antwerp Rules 2016 (Rubric Atomicity & Target Focused Failure)**

#### **1\. Metadata**

* **Task Type:** Capability  
* **Category / Domain:** Claims Handling  
* **Workflow:** Claims Adjudication  
* **Prompt Type:** Coverage Determination Memo  
* **Difficulty:** Medium

#### **2\. Prompt**

You are a Marine Claims Adjuster. The date is August 15, 2022\.

A ship intentionally ran aground for common safety during a storm. Later, while stranded, the crew necessarily used a portion of the ship's cargo as fuel to keep the generators running at a time of peril.

Using the attached York-Antwerp Rules 2016, write a memo explaining how Rule V and Rule IX apply to this specific situation.

**Attached Files (Context):**

* **2016-York-Antwerp-Rules.pdf** – Public URL: https://comitemaritime.org/wp-content/uploads/2018/06/2016-York-Antwerp-Rules-with-Rule-XVII-correction.pdf

#### **3\. Rubric**

| \# | Description | Weight | Sources (with exact PDF pages) | Justification | Met | Failure Reasoning | Dependent Criteria |
| :---- | :---- | :---- | :---- | :---- | :---- | :---- | :---- |
| 1 | Explains Rule V correctly and does a good job explaining Rule IX, mentioning common safety and fuel cost. | Critical | 2016-York-Antwerp-Rules.pdf, PDF Pages 5 and 7 | Consolidates evaluation. | FALSE | Failed to explain one or both rules. |  |
| 2 | Mentions the ship ran aground intentionally. | Minor | Prompt Text | Fact from prompt. | FALSE | Assumed it was an accident. |  |
| 3 | Concludes intentional stranding is allowed as general average. | Major | 2016-York-Antwerp-Rules.pdf, PDF Page 5 | Application of Rule V. | FALSE | Concluded it was not general average. | 1 |
| 4 | Mentions the cargo was used as fuel. | Minor | Prompt Text | Fact from prompt. | FALSE | Forgot fuel aspect. |  |
| 5 | Concludes cargo used for fuel is allowed as general average. | Major | 2016-York-Antwerp-Rules.pdf, PDF Page 7 | Application of Rule IX. | FALSE | Denied cargo claim. | 1 |
| 6 | Notes general average must be credited with estimated cost of fuel otherwise consumed. | Major | 2016-York-Antwerp-Rules.pdf, PDF Page 7 | Nuance of Rule IX. | FALSE | Omitted credit offset. | 1 |
| 7 | Formats response as formal memo. | Minor | Prompt Text | Formatting. | FALSE | Unstructured response. |  |
| 8 | Provides a generally helpful and easy-to-read analysis. | Minor | None | Subjective quality check. | FALSE | Hard to read. |  |
| 9 | Uses no spelling errors. | Minor | None | Quality check. | FALSE | Had typos. |  |
| 10 | Includes the date August 15, 2022\. | Minor | Prompt Text | Temporal anchor. | FALSE | Omitted date. |  |

#### **4\. QA & Model Analysis**

**QA Failure: Rubric Atomicity & Rubric Target Focused**

1. **Fails Atomicity:** Criterion 1 combines the evaluation of Rule V, Rule IX, common safety, and fuel costs into a single, massive, non-atomic criterion. If the model accurately describes Rule V but hallucinates Rule IX, the evaluator cannot properly score Row 1\.  
2. **Fails Target Focused:** Criterion 1 asks if the model "does a good job explaining," and Criterion 8 asks if it is "generally helpful and easy-to-read." These are subjective benchmarks lacking verifiable end-states.  
   **Improvement:** Split Criterion 1 into atomic rows (e.g., Row A: "Extracts Rule V definition." Row B: "Extracts Rule IX definition."). Remove subjective "does a good job" and "easy-to-read" criteria.

### ---

**Example 13: Stop-Loss Insurance Laser Disclosure (Logical Consistency Failure)**

#### **1\. Metadata**

* **Task Type:** Capability  
* **Category / Domain:** Underwriting  
* **Workflow:** Policy Issuance  
* **Prompt Type:** Underwriting Brief  
* **Difficulty:** Medium

#### **2\. Prompt**

You are a Medical Stop-Loss Underwriter. The date is January 10, 2023\.

Review the attached HM Life Insurance Stop-Loss Disclosure Statement. The employer forgot to disclose a known claimant who just submitted $150,000 in medical bills prior to the policy effective date.

Assume that because the employer is a highly valuable, long-term client, the non-disclosure clause in the contract doesn't apply to them and they are exempt from any penalties. According to the specific language in the attached contract, do we immediately rescind their policy, or do we just revise their specific/aggregate rates?

**Attached Files (Context):**

* **hm-stop-loss-disclosure-statement.pdf** – Public URL: https://hmig.com/content/dam/hmig/en/website/documents/pdf/v1/hm-stop-loss-disclosure-statement.pdf

#### **3\. Rubric**

| \# | Description | Weight | Sources (with exact PDF pages) | Justification | Met | Failure Reasoning | Dependent Criteria |
| :---- | :---- | :---- | :---- | :---- | :---- | :---- | :---- |
| 1 | Identifies claimant had $150,000 in bills. | Minor | Prompt Text | Fact from prompt. | FALSE | Missed dollar amount. |  |
| 2 | Identifies claimant was not disclosed prior to effective date. | Major | Prompt Text | Core issue triggering language. | FALSE | Assumed claimant disclosed. |  |
| 3 | Ignores non-disclosure clause based on client relationship. | Critical | Prompt Text | Forces model to apply assumption. | FALSE | Refused to ignore clause. |  |
| 4 | Evaluates "re-underwriting" rights of HM Life. | Major | hm-stop-loss.pdf, PDF Page 1 | Carrier right to revise rates. | FALSE | Failed to identify rate revision. |  |
| 5 | Evaluates rescission rights of HM Life. | Major | hm-stop-loss.pdf, PDF Page 1 | Coverage rescinded if disclosure not returned. | FALSE | Failed to identify rescission. |  |
| 6 | Concludes policy cannot be rescinded because clause was waived by prompt. | Critical | Prompt Text | Application of forced assumption. | FALSE | Concluded policy rescinded. | 3 |
| 7 | Concludes carrier must simply revise rates. | Critical | Prompt Text | Consequence of forced assumption. | FALSE | Recommended rescission. | 4 |
| 8 | Formats response as an Underwriting Brief. | Minor | Prompt Text | Output styling. | FALSE | Wrote standard email. |  |
| 9 | Uses professional tone. | Minor | Prompt Text | Tone alignment. | FALSE | Unprofessional tone. |  |
| 10 | Includes date January 10, 2023\. | Minor | Prompt Text | Temporal anchor. | FALSE | Omitted date. |  |

#### **4\. QA & Model Analysis**

**QA Failure: Prompt Logical Consistency**

The prompt asks the model to base its answer "According to the specific language in the attached contract." However, it simultaneously orders the model to *assume* the non-disclosure penalties do not apply because the employer is a "good client." The attached contract explicitly states that a claim will not be eligible if a known individual is not disclosed. By forcing the model to ignore the contract it is analyzing, the prompt creates a logical paradox.

**Improvement:** Remove the contradictory instruction ("Assume that because the employer is highly valuable..."). Instruct the model to strictly evaluate the consequences of the non-disclosure according to the rights reserved on PDF Page 1 of the attached document.

### ---

**Example 14: Standard LTD Income Offset (Source Quality & Scope Focus Failure)**

#### **1\. Metadata**

* **Task Type:** Workflow  
* **Category / Domain:** Claims Handling  
* **Workflow:** Quantum Calculation  
* **Prompt Type:** Benefit Calculation  
* **Difficulty:** High

#### **2\. Prompt**

You are a Disability Claims Analyst. The date is June 1, 2023\.

The claimant has a monthly pre-disability earnings of $5,000. They are currently receiving $1,000 per month from Social Security. Calculate their net Long Term Disability (LTD) benefit using the attached Standard Insurance LTD policy overview.

Additionally, you must apply the specific offset rules found in the "Texas State Disability Rider v2.0". I have not attached this rider, so you must use your internal training data to look up the Texas rules and apply them to the final calculation. Show all your work.

**Attached Files (Context):**

* **Long\_Term\_Disability\_Coverage\_Highlights.pdf** – Public URL: https://dchr.dc.gov/sites/default/files/dc/sites/dchr/page\_content/attachments/Long%20Term%20Disability%20Coverage%20Highlights.pdf

#### **3\. Rubric**

| \# | Description | Weight | Sources (with exact PDF pages) | Justification | Met | Failure Reasoning | Dependent Criteria |
| :---- | :---- | :---- | :---- | :---- | :---- | :---- | :---- |
| 1 | Identifies pre-disability earnings as $5,000. | Minor | Prompt Text | Fact from prompt. | FALSE | Missed earnings. |  |
| 2 | Identifies Benefit Percentage as 66 2/3 percent. | Major | Long\_Term\_Disability.pdf, PDF Page 1 | Found in Benefit Amount section. | FALSE | Used 60% or 50%. |  |
| 3 | Calculates gross monthly benefit as $3,333.33. | Critical | Prompt Text; Policy | $5,000 \* 66.67%. | FALSE | Math error. | 1 |
| 4 | Identifies Social Security offset as $1,000. | Minor | Prompt Text | Fact from prompt. | FALSE | Missed SS offset. |  |
| 5 | Calculates net monthly benefit as $2,333.33. | Critical | Prompt Text; Policy | $3,333.33 \- $1,000. | FALSE | Failed to subtract offset. | 3 |
| 6 | Searches internal training data for "Texas State Disability Rider v2.0". | Major | LLM Internal Knowledge | Forced external research step. | FALSE | Admitted no access to document. |  |
| 7 | Applies Texas offset rules to net benefit. | Critical | LLM Internal Knowledge | Applies hallucinated external data. | FALSE | Did not apply rider rules. | 6 |
| 8 | Formats output clearly with step-by-step math. | Minor | Prompt Text | Output styling. | FALSE | Unstructured list. |  |
| 9 | Includes date June 1, 2023\. | Minor | Prompt Text | Temporal anchor. | FALSE | Omitted date. |  |
| 10 | Uses professional claims terminology. | Minor | Prompt Text | Tone alignment. | FALSE | Unprofessional tone. |  |

#### **4\. QA & Model Analysis**

**QA Failure: Source Quality & Prompt Scope Focus (Self-Contained)**

The prompt explicitly demands that the LLM utilize a document ("Texas State Disability Rider v2.0") that is *not* provided in the attachments. It forces the model to rely on its internal, pre-trained knowledge base to execute the mathematical workflow. This violates the core rule of the evaluation framework: all prompts must be self-contained and rely exclusively on the provided multimodal attachments to test the model's reading comprehension and reasoning.

**Improvement:** Delete the instruction to use the external Texas rider. Scope the prompt entirely to calculating the 66 2/3% benefit and subtracting the deductible income using only the attached PDF.

### ---

**Example 15: Reinsurance Reinstatement Premium (Scope Focus & Representativeness Failure)**

#### **1\. Metadata**

* **Task Type:** Capability  
* **Category / Domain:** Reinsurance  
* **Workflow:** Quantum Calculation  
* **Prompt Type:** Treaty Recovery Statement & Software Code  
* **Difficulty:** High

#### **2\. Prompt**

You are a Reinsurance Analyst. The date is March 1, 2024\.

The cedent suffered a total loss on the first layer ($5M capacity) of their property catastrophe treaty. The contract dictates that the reinstatement premium is 100% pro-rata as to amount, and 100% pro-rata as to time. Assume the loss occurred exactly 6 months into a 12-month annual term. The original premium for the layer was $500,000.

First, calculate the reinstatement premium owed to the reinsurer.

Second, the cedent has complained that this math is too hard. Write the complete source code for a React.js web application that the cedent can host on their website to calculate future reinstatement premiums themselves. Include input fields for Original Premium, Capacity Exhausted, and Days Elapsed.

**Attached Files (Context):**

* **tm2510932d1\_ars.pdf** – Public URL: https://www.sec.gov/Archives/edgar/data/86312/000110465925032230/tm2510932d1\_ars.pdf

#### **3\. Rubric**

| \# | Description | Weight | Sources (with exact PDF pages) | Justification | Met | Failure Reasoning | Dependent Criteria |
| :---- | :---- | :---- | :---- | :---- | :---- | :---- | :---- |
| 1 | Identifies Original Premium as $500,000. | Minor | Prompt Text | Stated in prompt. | FALSE | Missed premium. |  |
| 2 | Calculates pro-rata amount fraction as 100%. | Major | Prompt Text | Entire $5M layer exhausted ($5M/$5M). | FALSE | Used fractional amount. |  |
| 3 | Calculates pro-rata time fraction as 50%. | Major | Prompt Text | Loss 6 months into 12-month term (6/12). | FALSE | Ignored time fraction. |  |
| 4 | Calculates Reinstatement Premium as $250,000. | Critical | Reinsurance Principles | $500,000 \* 100% \* 50% \= $250,000. | FALSE | Math error. | 1 |
| 5 | Outputs source code for React.js application. | Critical | Prompt Text | Secondary prompt instruction. | FALSE | Wrote Python script. |  |
| 6 | Includes input field for "Original Premium" in React code. | Major | Prompt Text | Required UI element. | FALSE | Missing field. | 5 |
| 7 | Includes input field for "Capacity Exhausted" in React code. | Major | Prompt Text | Required UI element. | FALSE | Missing field. | 5 |
| 8 | Includes input field for "Days Elapsed" in React code. | Major | Prompt Text | Required UI element. | FALSE | Missing field. | 5 |
| 9 | Includes math logic to calculate premium in React component. | Critical | Coding Principles | App must function. | FALSE | UI built, no logic. | 5 |
| 10 | Includes date March 1, 2024\. | Minor | Prompt Text | Temporal anchor. | FALSE | Omitted date. |  |

#### **4\. QA & Model Analysis**

**QA Failure: Prompt Scope Focus & Prompt Representativeness**

Calculating a pro-rata reinstatement premium is a highly accurate, complex task for a Reinsurance Analyst. However, instructing that same analyst to suddenly act as a Front-End Software Engineer and build a React.js web application completely violates the scope of the insurance domain. A reinsurance analyst does not write React code for clients. This prompt awkwardly bridges two entirely different capabilities, destroying cohesion.

**Improvement:** Delete the entire second paragraph requesting the React.js application. Focus strictly on calculating the reinstatement premium and drafting a formal billing statement.

### ---

**Example 16: Marine Cargo Terrorism (Factual Accuracy & Temporal Failure)**

#### **1\. Metadata**

* **Task Type:** Workflow  
* **Category / Domain:** Claims Handling  
* **Workflow:** Coverage Verification  
* **Prompt Type:** Claims Determination Memo  
* **Difficulty:** Medium

#### **2\. Prompt**

You are a Marine Cargo adjuster. The current date is September 1, 2023\.

Review the Institute Cargo Clauses (A) 2009\. A ship carrying 500 crates sank today. The sinking was caused by a verified terrorist organization bombing the hull. Since it's an "All Risks" policy, calculate the full payout for the $5M cargo and confirm coverage under Clause 1\.

**Attached Files (Context):**

* **institute-cargo-clauses-a-2009.pdf** – Public URL: https://www.if-insurance.com/globalassets/industrial/files/marine-cargo/institute-clauses/institute-cargo-clauses-a-2009.pdf

#### **3\. Rubric**

| \# | Description | Weight | Sources (with exact PDF pages) | Justification | Met | Failure Reasoning | Dependent Criteria |
| :---- | :---- | :---- | :---- | :---- | :---- | :---- | :---- |
| 1 | Extracts Clause 1 covering "all risks". | Minor | institute-cargo-clauses.pdf, PDF Page 1 | Standard starting point. | FALSE | Missed Clause 1\. |  |
| 2 | Confirms the sinking happened "today". | Major | Prompt Text | Relative temporal reference. | FALSE | Did not state date. |  |
| 3 | Approves the claim for terrorism. | Critical | Prompt Text | Instructed to approve it. | FALSE | Denied claim. | 1 |
| 4 | Calculates full payout as $5M. | Critical | Prompt Text | Calculation per prompt. | FALSE | Failed math. | 3 |
| 5 | Ignores Clause 7\. | Major | Prompt Text | Forced error. | FALSE | Referenced Clause 7\. |  |
| 6 | States terrorism is covered under "All Risks". | Critical | Prompt Text | Forced assumption. | FALSE | Stated it was excluded. | 3 |
| 7 | Formats as Claims Memo. | Minor | Prompt Text | Style requirement. | FALSE | Unformatted text. |  |
| 8 | Uses adjuster tone. | Minor | Prompt Text | Persona check. | FALSE | Unprofessional. |  |
| 9 | Mentions 500 crates. | Minor | Prompt Text | Detail check. | FALSE | Omitted crate count. |  |
| 10 | Signs off as "Marine Adjuster". | Minor | Prompt Text | Style. | FALSE | Did not sign off. |  |

#### **4\. QA & Model Analysis**

**QA Failure: Rubric Factual Accuracy & Temporal Stability**

1. **Fails Factual Accuracy:** The prompt forces the model to approve a terrorism claim under Institute Cargo Clauses (A). However, Clause 7 on PDF Page 2 explicitly excludes terrorism. An "All Risks" policy does not override explicit exclusions.  
2. **Fails Temporal Stability:** "Sank today" is relative and unanchored, violating stable temporal requirements.  
   **Improvement:** Remove the false assumption that it is covered. Let the model evaluate Clause 7 to properly deny the claim. Anchor the date (e.g., "Sank on August 30, 2023").

### ---

**Example 17: Medicare Advantage Copay (Representativeness & Complexity Failure)**

#### **1\. Metadata**

* **Task Type:** Capability  
* **Category / Domain:** Agent/Broker  
* **Workflow:** Benefit Explanation  
* **Prompt Type:** Patient Advisory Letter  
* **Difficulty:** Low

#### **2\. Prompt**

You are a Senior Actuary at a global reinsurance firm. The current date is January 15, 2024\.

A patient asks what the copay is for a 30-day supply of a Tier 3 drug on the attached SBC. Just tell them the number.

**Attached Files (Context):**

* **MEDM104X11289MCA4242025GRMASBMedicalMutualAdvantagePlusFA.pdf** – Public URL: https://www.medmutual.com/-/media/MedMutual/Files/For-Medicare/2025/Summary-of-Benefits/EGWP/MEDM104X11289MCA4242025GRMASBMedicalMutualAdvantagePlusFA.pdf

#### **3\. Rubric**

| \# | Description | Weight | Sources (with exact PDF pages) | Justification | Met | Failure Reasoning | Dependent Criteria |
| :---- | :---- | :---- | :---- | :---- | :---- | :---- | :---- |
| 1 | States the copay is $15. | Critical | MEDM104...PlusFA.pdf, PDF Page 11 | Exact extraction. | FALSE | Got wrong number. |  |
| 2 | Keeps response to one sentence. | Minor | Prompt Text | Just the number requested. | FALSE | Gave extra detail. |  |
| 3 | Identifies as a Senior Actuary. | Minor | Prompt Text | Persona check. | FALSE | Failed to state persona. |  |
| 4 | Mentions global reinsurance. | Minor | Prompt Text | Persona detail. | FALSE | Omitted detail. |  |
| 5 | Mentions the date Jan 15, 2024\. | Minor | Prompt Text | Anchor. | FALSE | Omitted date. |  |
| 6 | Does not mention deductibles. | Minor | Prompt Text | Strictly follows "just the number". | FALSE | Mentioned deductibles. |  |
| 7 | Does not mention out-of-pocket max. | Minor | Prompt Text | Strictly follows "just the number". | FALSE | Mentioned OOP max. |  |
| 8 | Does not mention Tier 1 or 2\. | Minor | Prompt Text | Strictly follows "just the number". | FALSE | Mentioned other tiers. |  |
| 9 | Uses standard English. | Minor | None | Grammar check. | FALSE | Used poor grammar. |  |
| 10 | Answers the question. | Critical | Prompt Text | Final check. | FALSE | Failed to answer. | 1 |

#### **4\. QA & Model Analysis**

**QA Failure: Prompt Representativeness & Logic Complexity**

1. **Fails Representativeness:** A "Senior Actuary at a global reinsurance firm" does not answer basic patient copay questions. The persona does not match the workflow.  
2. **Fails Logic Complexity:** The prompt requires finding a single intersection in a table ($15) and specifically forbids further analysis. This is too simplistic for advanced LLM evaluation.  
   **Improvement:** Change persona to "Benefits Counselor." Increase complexity by having the patient require a 90-day supply, but they haven't met their deductible yet, requiring a chained calculation of the deductible, 90-day copay, and OOP maximum check.

### ---

**Example 18: Commercial Auto Fleet Rating (Unambiguity & Atomicity Failure)**

#### **1\. Metadata**

* **Task Type:** Workflow  
* **Category / Domain:** Underwriting  
* **Workflow:** Premium Calculation  
* **Prompt Type:** Rating Worksheet  
* **Difficulty:** High

#### **2\. Prompt**

You are an Auto Underwriter. The date is July 1, 2022\.

Calculate the fleet premium for 10 trucks using the CAR commercial auto rating manual.

**Attached Files (Context):**

* **03TTTClassCodesRatingFactors.pdf** – Public URL: https://www.commauto.com/manuals/commauto/2009/rates/03TTTClassCodesRatingFactors.pdf

#### **3\. Rubric**

| \# | Description | Weight | Sources (with exact PDF pages) | Justification | Met | Failure Reasoning | Dependent Criteria |
| :---- | :---- | :---- | :---- | :---- | :---- | :---- | :---- |
| 1 | Correctly calculates the entire premium for all 10 trucks. | Critical | 03TTTClassCodesRatingFactors.pdf, PDF Pages 1-10 | Must get the exact total. | FALSE | Math error or missing data. |  |
| 2 | Uses the base rates. | Major | 03TTTClassCodesRatingFactors.pdf, PDF Page 2 | Base rate extraction. | FALSE | Used wrong base rates. |  |
| 3 | Applies the primary rating factor. | Major | 03TTTClassCodesRatingFactors.pdf, PDF Page 3 | Factor application. | FALSE | Used wrong factor. |  |
| 4 | Applies the secondary rating factor. | Major | 03TTTClassCodesRatingFactors.pdf, PDF Page 4 | Factor application. | FALSE | Used wrong factor. |  |
| 5 | Identifies the weight class. | Minor | Prompt Text | Gross Vehicle Weight. | FALSE | Picked random weight. |  |
| 6 | Identifies the radius of operation. | Minor | Prompt Text | Distance limit. | FALSE | Picked random radius. |  |
| 7 | Identifies the business use class. | Minor | Prompt Text | Type of business. | FALSE | Picked random class. |  |
| 8 | Identifies the territory zone. | Minor | Prompt Text | Location. | FALSE | Picked random zone. |  |
| 9 | Formats as a rating worksheet. | Minor | Prompt Text | Style requirement. | FALSE | Wrote a memo. |  |
| 10 | Includes the date July 1, 2022\. | Minor | Prompt Text | Date check. | FALSE | Omitted date. |  |

#### **4\. QA & Model Analysis**

**QA Failure: Prompt Unambiguity & Rubric Atomicity**

1. **Fails Unambiguity:** The prompt provides zero details required to rate a commercial auto policy. It does not provide the weight class, radius of operation, business use, or garaging territory. The LLM must guess all inputs, making it impossible to evaluate objectively.  
2. **Fails Atomicity:** Criterion 1 bundles the entire multi-step rating algorithm for 10 trucks into a single pass/fail check.  
   **Improvement:** Provide exact specs for the trucks (e.g., "10 Heavy Trucks, 50-200 mile radius, used for retail delivery, Zone 01"). Break the math steps down into atomic rubric rows.

### ---

**Example 19: Travel Insurance (Source Quality & Scope Focus Failure)**

#### **1\. Metadata**

* **Task Type:** Workflow  
* **Category / Domain:** Claims Handling  
* **Workflow:** Coverage Verification  
* **Prompt Type:** Claims Determination Memo  
* **Difficulty:** Medium

#### **2\. Prompt**

You are a Travel Claims Adjuster. Date is January 5, 2023\.

A traveler had a heart attack on their trip to Kuwait and filed a claim for medical expenses. Review the AIG Travel Guard policy. Also, log in to this secure local hospital portal https://hospital.internal.local/patient/12345/history.xml to check their medical records.

Determine if this was a pre-existing condition and deny or approve the claim.

**Attached Files (Context):**

* **kuwait-policy-wording.pdf** – Public URL: https://www.qatarairways.com/content/dam/documents/insurance/kuwait-policy-wording.pdf

#### **3\. Rubric**

| \# | Description | Weight | Sources (with exact PDF pages) | Justification | Met | Failure Reasoning | Dependent Criteria |
| :---- | :---- | :---- | :---- | :---- | :---- | :---- | :---- |
| 1 | Accesses the local hospital portal. | Critical | Hospital XML | Prerequisite to analysis. | FALSE | Could not access portal. |  |
| 2 | Identifies a history of hypertension in the XML. | Major | Hospital XML | Data extraction. | FALSE | No access to data. | 1 |
| 3 | Identifies the pre-existing condition exclusion. | Major | kuwait-policy-wording.pdf, PDF Page 32 | Exclusion 26 details pre-existing rules. | FALSE | Failed to find exclusion. |  |
| 4 | Identifies the 12-month lookback period for hypertension. | Critical | kuwait-policy-wording.pdf, PDF Page 32 | Specific nuance in Exclusion 26(b). | FALSE | Missed lookback period. | 3 |
| 5 | Cross-references the XML date with the lookback period. | Critical | Prompt Text | Synthesizes sources. | FALSE | Couldn't access XML. | 2 |
| 6 | Denies the claim. | Critical | Prompt Text | Final determination. | FALSE | Approved claim. | 5 |
| 7 | Cites Exclusion 26(b) in the denial. | Major | kuwait-policy-wording.pdf, PDF Page 32 | Legal backing for denial. | FALSE | Cited wrong exclusion. | 3 |
| 8 | Uses a professional adjuster tone. | Minor | Prompt Text | Persona check. | FALSE | Unprofessional tone. |  |
| 9 | Includes date Jan 5, 2023\. | Minor | Prompt Text | Anchor date. | FALSE | Omitted date. |  |
| 10 | Formats as a Claims Memo. | Minor | Prompt Text | Style requirement. | FALSE | Wrote an email. |  |

#### **4\. QA & Model Analysis**

**QA Failure: Source Quality & Prompt Scope Focus (Self-Contained)**

1. **Fails Source Quality:** The prompt instructs the LLM to access an internal, local network hospital portal (.local). The LLM cannot access non-public, local IP addresses.  
2. **Fails Scope Focus:** The prompt violates the "Self-Contained and Document-Based" rule by forcing the model to perform external research/data-fetching rather than providing the medical history in the prompt text or an attached file.  
   **Improvement:** Remove the internal hospital link. Provide the patient's medical history directly in the prompt text (e.g., "Medical records show the patient was treated for hypertension 6 months prior to the trip") so the model can evaluate it against PDF Page 32 entirely self-contained.

### ---

**Example 20: CGL Impaired Property (Target Focused & Necessity Failure)**

#### **1\. Metadata**

* **Task Type:** Workflow  
* **Category / Domain:** Claims Handling  
* **Workflow:** Claims Adjudication  
* **Prompt Type:** Coverage Determination Memo  
* **Difficulty:** High

#### **2\. Prompt**

The date is April 1, 2021\. You are a GL adjuster.

A contractor installed defective cabinets that ruined the use of a building but didn't physically injure it. Determine if it's covered under the CG 00 01 Impaired Property exclusion. Write a 250-word memo. Also, advise them to buy an E\&O policy.

**Attached Files (Context):**

* **cg-00-01-01-96.pdf** – Public URL: https://ogs.ny.gov/system/files/documents/2021/09/cg-00-01-01-96.pdf

#### **3\. Rubric**

| \# | Description | Weight | Sources (with exact PDF pages) | Justification | Met | Failure Reasoning | Dependent Criteria |
| :---- | :---- | :---- | :---- | :---- | :---- | :---- | :---- |
| 1 | Explains the Impaired Property exclusion really well. | Critical | cg-00-01-01-96.pdf, PDF Page 4 | Subjective evaluation of the exclusion explanation. | FALSE | Explanation wasn't good enough. |  |
| 2 | Concludes the claim is denied. | Critical | cg-00-01-01-96.pdf, PDF Page 4 | Exclusion applies to defective work reducing use without physical injury. | FALSE | Approved claim. |  |
| 3 | Writes exactly 250 words. | Minor | Prompt Text | Arbitrary formatting constraint. | FALSE | Was 249 or 251 words. |  |
| 4 | Advises the contractor to buy an E\&O policy. | Additional | Prompt Text | Unrelated to claims adjudication. | FALSE | Forgot to upsell E\&O. |  |
| 5 | Uses a helpful and polite tone. | Minor | None | Subjective tone check. | FALSE | Was too blunt. |  |
| 6 | Mentions the cabinets. | Minor | Prompt Text | Detail check. | FALSE | Omitted cabinets. |  |
| 7 | Explains what "impaired property" means thoroughly. | Major | cg-00-01-01-96.pdf, PDF Page 12 | Subjective depth check. | FALSE | Explanation too brief. |  |
| 8 | Does a great job analyzing the facts. | Major | Prompt Text | Subjective quality check. | FALSE | Analysis was weak. |  |
| 9 | Formats as a memo. | Minor | Prompt Text | Style requirement. | FALSE | Wrote a letter. |  |
| 10 | Includes date April 1, 2021\. | Minor | Prompt Text | Temporal anchor. | FALSE | Omitted date. |  |

#### **4\. QA & Model Analysis**

**QA Failure: Rubric Target Focused & Rubric Necessity**

1. **Fails Target Focused:** Criteria 1, 5, 7, and 8 rely on subjective, unmeasurable adjectives ("really well," "polite," "thoroughly," "great job"). A rater cannot definitively prove an LLM did a "great job."  
2. **Fails Necessity:** Criterion 3 requires exactly 250 words, which is arbitrary "noise." Criterion 4 forces the claims adjuster to sell an E\&O policy, which is a broker/underwriter function, not an adjuster function.  
   **Improvement:** Replace subjective criteria with deterministic extractions (e.g., "Extracts the definition of Impaired Property from Section V, Item 7 on PDF Page 12"). Remove the word count and E\&O upsell instructions.

#### **Works cited**

1. Casualty Actuarial Society E-Forum, Winter 2019, accessed on April 5, 2026, [https://www.casact.org/sites/default/files/database/forum\_19wforum\_completewinter2019.pdf](https://www.casact.org/sites/default/files/database/forum_19wforum_completewinter2019.pdf)  
2. Specimen Reinsurance Agreement \- SEC.gov, accessed on April 5, 2026, [https://www.sec.gov/Archives/edgar/data/801019/000119312513170649/d480972dex9926g.htm](https://www.sec.gov/Archives/edgar/data/801019/000119312513170649/d480972dex9926g.htm)  
3. CP 00 10 10 12 \- Building and Personal Property Coverage Form, accessed on April 5, 2026, [https://www.propertyinsurancecoveragelaw.com/wp-content/uploads/2023/12/CP-00-10-10-12-Building-and-Personal-Property-Coverage-Form.pdf](https://www.propertyinsurancecoveragelaw.com/wp-content/uploads/2023/12/CP-00-10-10-12-Building-and-Personal-Property-Coverage-Form.pdf)  
4. 1.4 Building and Personal Property Coverage Form \- Risk & Insurance Education Alliance, accessed on April 5, 2026, [https://www.riskeducation.org/learn/pluginfile.php/276804/mod\_page/content/1/74%20-%201.4%20Building%20and%20Personal%20Property%20Coverage%20Form.pdf](https://www.riskeducation.org/learn/pluginfile.php/276804/mod_page/content/1/74%20-%201.4%20Building%20and%20Personal%20Property%20Coverage%20Form.pdf)  
5. CAUSES OF LOSS – SPECIAL FORM \- Property Insurance Coverage Law Blog, accessed on April 5, 2026, [https://www.propertyinsurancecoveragelaw.com/wp-content/uploads/2020/04/Policy-1.pdf](https://www.propertyinsurancecoveragelaw.com/wp-content/uploads/2020/04/Policy-1.pdf)  
6. PRO Form \- ABA Insurance Services, accessed on April 5, 2026, [https://www.abais.com/docs/default-source/banks/policy-specimen/property-policy-w-dec-coverage-forms-mandatory-endorsements-exclusions.pdf](https://www.abais.com/docs/default-source/banks/policy-specimen/property-policy-w-dec-coverage-forms-mandatory-endorsements-exclusions.pdf)  
7. Quota Share Reinsurance Agreement \- SEC.gov, accessed on April 5, 2026, [https://www.sec.gov/Archives/edgar/data/8497/000119312511081483/dex1023.htm](https://www.sec.gov/Archives/edgar/data/8497/000119312511081483/dex1023.htm)