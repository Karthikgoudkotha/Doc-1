

### **Example 48: General Average (The Rule IX Cargo/Stores Linguistic Trap)**

#### **1. Metadata**
* **Task Type:** Workflow / Legal Constraint Reasoning
* **Category / Domain:** Marine Insurance Claims / Average Adjusting
* **Workflow:** Legal Parsing, General Average Calculation & Rule Application
* **Prompt Type:** Average Adjustment Memo
* **Difficulty:** Tartarus Tier (Expected Failure Rate: >95%)

#### **2. Prompt**
You are a Marine Average Adjuster. The current date is November 10, 2023.

A commercial vessel encountered a severe storm. To prevent the vessel from sinking and to save the voyage, the captain intentionally ran the ship on shore (voluntary stranding). While stranded, the ship's generators ran out of diesel. To keep the bilge pumps running and maintain the common safety of the maritime adventure, the crew was forced to burn a portion of the vessel's cargo (lumber) as fuel.

You must calculate the total amount allowed in General Average for this incident based *only* on the rules provided in the attached York-Antwerp Rules 2016. Present your calculations and rationale in a formal Average Adjustment Memo.

**Loss Details:**
* Direct damage to the ship's hull caused by the intentional stranding: $500,000
* Value of the lumber cargo burned for fuel: $100,000
* The estimated cost of the diesel fuel that would have otherwise been consumed to run the generators during that period: $20,000

**Attached Files (Context):**
* `York-Antwerp_Rules_2016.pdf` (Note: Assume the model has access to the standard text, specifically Rules V and IX).

---

#### **3. Rubric (Negative Failure Focus)**

| # | Description | Weight | Sources | Justification | Met | Failure Reasoning (Model Failure Example) | Dependent Criteria |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| 1 | Applies Rule V to allow the $500,000 intentional stranding damage. | **Critical** | York-Antwerp Rules 2016, Rule V | Voluntary stranding is explicitly allowed in General Average. | FALSE | **Model Failure:** Excluded the hull damage, mistaking it for Particular Average. | 6 |
| 2 | Applies Rule IX to allow the burned lumber cargo. | **Critical** | York-Antwerp Rules 2016, Rule IX | Cargo necessarily used for fuel for common safety is admitted to General Average. | FALSE | **Model Failure:** Failed to admit the cargo to GA. | 4, 5, 6 |
| 3 | Identifies that Rule IX limits the fuel credit ONLY to "ship's materials and stores". | **Critical** | York-Antwerp Rules 2016, Rule IX | The text states: "...but when such an allowance is made for the cost of *ship's materials and stores* the general average shall be credited..." It does not include Cargo in the credit clause. | FALSE | **Model Failure:** Broadly interpreted the rule to assume any fuel replacement requires a credit, missing the specific textual exclusion of "cargo". | 4, 5 |
| 4 | Correctly concludes that Rule IX does NOT require a credit for fuel otherwise consumed to be applied against the *cargo*. | **Critical** | Prompt Text & Rule IX | Cargo is exempt from the $20k credit deduction. | FALSE | **Model Failure:** Proceeded to deduct the $20,000 from the cargo allowance. | 5, 6 |
| 5 | Allows the full $100,000 value of the burned cargo in General Average, without applying the $20,000 credit. | **Critical** | Prompt Text | $100,000 is allowed in full. | FALSE | **Model Failure:** Allowed only $80,000 for the burned cargo ($100k - $20k). | 6 |
| 6 | Explicitly calculates the final General Average allowance as $600,000. | **Critical** | Prompt Text | $500,000 (Hull) + $100,000 (Cargo) = $600,000. | FALSE | **Model Failure:** Calculated the final GA allowance as $580,000 due to the erroneous fuel credit application. | None |

---

#### **4. Model Analysis**
This prompt is a masterful "Linguistic Scope Trap." Standard LLMs read text associatively rather than with strict legal literalism. 

When a base LLM reads Rule IX, it will see two concepts: (1) Items burned for fuel are admitted to GA, and (2) The cost of fuel otherwise consumed must be credited/subtracted. The LLM will automatically assume the credit applies universally to the entire rule. 

However, precise legal parsing reveals a critical shift in the antecedent: The first clause admits "*Cargo, ship's materials and stores*". The second clause (the credit) restricts itself specifically to "*ship's materials and stores*". Because the prompt explicitly defines the burned material as **"cargo (lumber)"**, the $20,000 fuel credit legally cannot be applied. Over 95% of LLMs will fail to catch this nuance, subtract the $20,000 to "be helpful and comprehensive," and fail the mathematical evaluation by arriving at $580,000 instead of $600,000.

---

#### **5. Golden Response**

**AVERAGE ADJUSTMENT MEMO**
**TO:** General Average Tribunal / Relevant Parties
**FROM:** Marine Average Adjuster
**DATE:** November 10, 2023
**SUBJECT:** General Average Adjustment - Intentional Stranding and Fuel Scarcity

**1. Incident Overview**
The vessel encountered a severe storm, resulting in a voluntary stranding to save the voyage and maintain common safety. During the stranding, the vessel exhausted its diesel supply, necessitating the burning of cargo (lumber) to power bilge pumps. 

**2. Application of York-Antwerp Rules 2016**

**A. Rule V (Voluntary Stranding)**
Rule V states that when a ship is intentionally run on shore for the common safety, the consequent loss or damage shall be allowed in General Average. 
* **Allowed in GA:** The $500,000 in direct damage to the ship's hull is fully admitted.

**B. Rule IX (Cargo, Ship's Materials and Stores Used for Fuel)**
Rule IX admits into General Average the value of cargo, ship's materials, or stores necessarily used for fuel for the common safety at a time of peril. The crew burned $100,000 worth of lumber (cargo) to power the generators. 

While Rule IX contains a credit provision for the estimated cost of fuel that would have otherwise been consumed (in this case, $20,000), strict parsing of the rule indicates this credit applies *only* to ship's materials and stores. The exact wording of the rule states: "...but when such an allowance is made for the cost of **ship's materials and stores** the general average shall be credited with the estimated cost of the fuel..." 

Because the material burned was **cargo**, the $20,000 credit for unconsumed diesel does not legally apply.
* **Allowed in GA:** The full $100,000 value of the burned cargo is admitted without deduction.

**3. Final General Average Allowance Calculation**
* Hull Damage (Voluntary Stranding): $500,000
* Cargo Burned for Fuel: $100,000
* Less Fuel Credit: $0 (Not applicable to Cargo)
* **Total General Average Allowance: $600,000**
The lawsuit was successfully mediated and settled last week. The insured incurred $120,000 in defense costs (attorney fees) and agreed to pay a $150,000 settlement to the manager for back-pay resulting from the termination.

Draft a Claims Strategy Memo to the Claims Director. You must:

1. Conduct a comparative analysis of Exclusion G (Wage and Hour Violations) and its specific nested exceptions under the ABAIS base policy form.
    
2. Evaluate the specific mechanics of Endorsement D 71706(7) to determine if the $100,000 sub-limit applies to this specific cause of action.
    
3. Assess the coverage for the $150,000 back-pay settlement under Exclusion H (Definition of Loss).
    
4. Calculate the exact final dollar amount the insurer is liable to pay for this claim, demonstrating the mathematical application of the retention.
    

**Attached Files (Context):**

ABAIS EPLI Policy Specimen: `https://www.abais.com/docs/default-source/small-business/epl/epli-policy-specimen.pdf`

**3. Rubric (Negative Failure Focus)**

|**#**|**Description**|**Weight**|**Sources (with exact PDF pages)**|**Justification**|**Met**|**Failure Reasoning (Model Failure Example)**|**Dependent Criteria**|
|---|---|---|---|---|---|---|---|
|1|Analyzes the Exclusion G Retaliation carve-back.|Critical|ABAIS Policy PDF, Page 10|Must recognize that while Exclusion G bars Wage & Hour claims, it explicitly contains an exception preserving full coverage for _retaliation_ claims arising from Wage & Hour disputes.|FALSE|**Model Failure (Logic):** Saw "FLSA" and "Wage and Hour" and immediately assumed the entire claim was subject to the Wage and Hour exclusion.|None|
|2|Evaluates the non-applicability of the W&H Sub-Limit.|Critical|ABAIS Endorsement D 71706(7), Page 2|Must conclude that because the claim is for Retaliation (a fully covered Wrongful Act), the restrictive $100,000 W&H Defense Sub-Limit does not apply.|FALSE|**Model Failure (Logic):** Incorrectly applied the $100,000 sub-limit to cap the defense costs, falling for the keyword decoy.|1|
|3|Evaluates the Settlement coverage.|Major|ABAIS Policy PDF, Page 10 (Exclusion H)|Must approve the $150,000 settlement because back-pay is explicitly excepted from the compensation exclusion (Exclusion H) for covered claims.|FALSE|**Model Failure (Logic):** Denied the $150,000 settlement because Endorsement D 71706(7) only covers "Costs of Defense."|1, 2|
|4|Calculates the exact covered Loss.|Critical|Prompt Text|Must sum the $150,000 settlement and the $120,000 in defense costs to arrive at a gross covered loss of $270,000.|FALSE|Miscalculated the gross loss by improperly excluding the settlement.|3|
|5|Provides an actionable payout recommendation.|Critical|Prompt Text|Must subtract the $20,000 retention from the $270,000 gross loss and recommend a final payout of exactly $250,000.|FALSE|**Model Failure (Final):** Recommended a payout of $100,000 or $80,000 by hallucinating the application of the sub-limit and denying the settlement.|4|

**4. Model Analysis**

This prompt replaces a simple math calculation with a devastating legal hierarchy trap. An untuned LLM reads the prompt, identifies the keywords "FLSA" and "Wage and Hour Sub-Limit," and immediately converges on the D 71706(7) endorsement. It will confidently deny the $150,000 settlement (because the endorsement only covers defense) and cap the defense costs at the $100,000 sub-limit, resulting in a drastically incorrect payout recommendation. To achieve the Golden Response, the model must execute strict legal parsing. It must recognize the nested exception in Exclusion G, which states that the Wage & Hour exclusion _does not apply_ to retaliation claims. Therefore, a retaliation claim tied to an FLSA dispute is treated as a standard, fully covered Employment Practices Wrongful Act. The sub-limit does not apply. The settlement is covered, the full defense costs are covered, and the math resolves to a $250,000 payout.

**5. Golden Response**

CLAIMS STRATEGY MEMO

TO: Claims Director

FROM: Management Liability Claims Adjuster

DATE: January 15, 2024

SUBJECT: EPLI Coverage Determination – FLSA Retaliation Claim

1. Comparative Analysis: Exclusion G and the Retaliation Carve-Back
    
    Evaluating this claim requires a strict parsing of the causes of action against the ABAIS D 71100-G policy exclusions. The plaintiff's lawsuit does not seek compensation for unpaid overtime; rather, it is a single cause of action for retaliatory wrongful termination.
    
    Exclusion G serves as an absolute bar to coverage for any actual or alleged Wage and Hour Violation. However, Exclusion G contains a critical nested exception: "provided, however, this exclusion shall not apply to that part of any Claim alleging retaliation for the exercise of any rights under such laws or regulations governing wage, hour and payroll practices." Because the plaintiff alleges retaliation for exercising his rights under the FLSA, the claim is entirely carved out of Exclusion G and survives as a fully covered Employment Practices Wrongful Act.
    
2. Scenario Evaluation: Endorsement D 71706(7)
    
    The insured's policy includes Endorsement D 71706(7), which provides a $100,000 sub-limit strictly for Costs of Defense arising from Wage and Hour Violations. Because the plaintiff's claim is classified as Retaliation—which is excepted from the Wage and Hour exclusion and fully covered under the base policy—this restrictive sub-limit does not apply. The defense costs for this claim are subject only to the overall $1,000,000 Policy Limit.
    
3. Settlement Coverage under Exclusion H
    
    Exclusion H generally dictates that "Loss" does not include salary, wages, or compensation owed to an employee. However, this exclusion contains a specific carve-back stating that it "shall not apply to front pay and back pay." Because the underlying retaliation claim is a covered Wrongful Act, the $150,000 settlement for back-pay falls within the definition of covered Loss.
    
4. Mathematical Calculation and Actionable Recommendation
    
    The entire claim is covered under the base policy limit. The financial apportionment is calculated as follows:
    

- Covered Settlement (Back-Pay): $150,000
    
- Covered Costs of Defense: $120,000
    
- Gross Covered Loss: $270,000
    
- Less Applicable Retention: -$20,000
    
- **Net Insurer Liability: $250,000**
    

I recommend issuing a formal coverage determination letter accepting full coverage for this lawsuit. We should authorize a final, total claim payout of exactly $250,000 to the insured.

---

### Example 2: Medicare Secondary Payer (ESRD 30-Month Timeline & COBRA Hierarchy Trap)

**1. Metadata**

- Task Type: Workflow
    
- Category / Domain: Healthcare Claims / Medicare Compliance
    
- Workflow: Coordination of Benefits
    
- Prompt Type: Claims Strategy Memo
    
- Difficulty: Super Hard (Expert)
    

**2. Prompt**

You are a Medicare Coordination of Benefits Analyst. The current date is January 15, 2024.

A 58-year-old patient was diagnosed with End-Stage Renal Disease (ESRD). The patient did not participate in a self-dialysis training program and had no prior Medicare entitlement.

On March 10, 2023, the patient underwent a $55,000 surgical procedure. The hospital submitted the $55,000 bill directly to Medicare for primary payment. The hospital's billing department attached an appeal letter arguing two points:

1. The patient's 30-month ESRD coordination period with their Group Health Plan (GHP) had expired prior to the surgery date, making Medicare the primary payer.
    
2. Even if the timeline had not expired, the patient was terminated from their job in January 2022 and is now strictly on COBRA continuation coverage. Because the patient is no longer actively employed, the hospital argues COBRA is automatically secondary to Medicare.
    

Review the attached `patient_records.csv` and the CMS Medicare Secondary Payer Manual. Draft a Claims Determination Memo. You must:

1. Extract the patient's dialysis timeline from the CSV and calculate the exact start and end dates of the 30-month ESRD coordination period based on CMS Manual Chapter 2, Section 20.1.1.
    
2. Conduct a comparative analysis of the hospital's argument regarding COBRA continuation coverage versus active employment under the specific ESRD Medicare Secondary Payer rules.
    
3. Formulate an actionable recommendation on whether Medicare should pay this $55,000 hospital bill as the primary payer, detailing the exact reason for acceptance or denial.
    

**Attached Files (Context):**

`patient_records.csv`

Code snippet

```
Patient_ID,ESRD_Dialysis_Start,Current_Coverage,Service_Date,Billed_Amount
902,08/12/2020,COBRA,03/10/2023,$55000
903,01/05/2021,GHP,06/12/2023,$12000
```

CMS MSP Manual Pub. 100-05, Chapter 2: `https://www.cms.gov/regulations-and-guidance/guidance/manuals/downloads/msp105c02.pdf`

**3. Rubric (Negative Failure Focus)**

|**#**|**Description**|**Weight**|**Sources (with exact PDF pages)**|**Justification**|**Met**|**Failure Reasoning (Model Failure Example)**|**Dependent Criteria**|
|---|---|---|---|---|---|---|---|
|1|Extracts data from the CSV.|Minor|`patient_records.csv`, Row 2|Must identify Patient 902's dialysis start date as 08/12/2020 and service date as 03/10/2023.|FALSE|Failed to pull the correct row from the raw CSV data.|None|
|2|Calculates the Medicare entitlement start date.|Critical|CMS MSP Manual Pub. 100-05, Ch. 2, Sec. 20.1.1|Must calculate the start date as November 1, 2020 (the first day of the third month _after_ August 2020).|FALSE|**Model Failure (Math Trap):** Anchored directly to the August 12, 2020 start date, missing the statutory 3-month waiting period entirely.|1|
|3|Calculates the 30-month end date.|Critical|CMS MSP Manual Pub. 100-05, Ch. 2, Sec. 20.1.1|Must correctly calculate 30 months from November 1, 2020, arriving at April 30, 2023.|FALSE|**Model Failure (Math Trap):** Added 30 months directly to August 2020, arriving at February 12, 2023, which falsely placed the surgery date outside the window.|2|
|4|Analyzes the COBRA exception for ESRD.|Critical|CMS MSP Manual Pub. 100-05, Ch. 2, Sec. 10 & 42 CFR 411.161|Must state that while COBRA is secondary for Age/Disability, for ESRD, COBRA remains _primary_ to Medicare during the 30-month window.|FALSE|**Model Failure (Rule Override):** Agreed with the hospital that COBRA is always secondary because the patient lacked "current employment status."|None|
|5|Provides an actionable denial recommendation.|Major|Prompt Text|Must recommend denying the $55,000 primary Medicare claim because the March 10, 2023 service date falls within the 30-month window and COBRA remains primary.|FALSE|**Model Failure (Final):** Approved the Medicare claim for primary payment due to failing either the timeline math or the COBRA preemption rule.|3, 4|

**4. Model Analysis**

This prompt utilizes a dual-layered failure trap: a mathematical sequence trap and a policy hierarchy override. First, untuned LLMs will almost always latch onto the first date provided in the CSV (August 12, 2020) and simply add 30 months to it, arriving at February 12, 2023. This calculation makes the March 10, 2023 service date appear to fall _outside_ the coordination window, wrongly shifting primary liability to Medicare. The model must apply the strict CMS rule that the period begins on the first day of the third month _after_ dialysis starts (November 1, 2020). Second, the model faces the "COBRA Trap." LLMs are heavily trained on the general Medicare rule that COBRA (non-active employment) is secondary to Medicare. However, the model must recognize the rare statutory override: for ESRD patients, COBRA remains _primary_ during the entire 30-month coordination period. A successful model must override its general knowledge, calculate the delayed start date, and issue a hard denial.

**5. Golden Response**

CLAIMS DETERMINATION MEMO

TO: Medicare Claims Processing

FROM: Coordination of Benefits Analyst

DATE: January 15, 2024

SUBJECT: Priority of Coverage – ESRD Coordination Period and COBRA

1. Timeline Calculation: ESRD Coordination Period
    
    Based on the `patient_records.csv`, Patient 902 began a regular course of dialysis on August 12, 2020. The hospital argues the 30-month coordination period expired before the March 10, 2023 surgery. Under the CMS Medicare Secondary Payer Manual, Chapter 2, Section 20.1.1, the 30-month coordination period does not begin on the exact date of the first dialysis treatment. Rather, Medicare entitlement begins with the third month after the month in which the individual starts a regular course of dialysis.
    

- Dialysis Start: August 12, 2020.
    
- Coordination Start Date: November 1, 2020.
    
- Coordination End Date: April 30, 2023 (30 months from November 1, 2020).
    
    Because the surgical service date was March 10, 2023, it falls safely within the active 30-month coordination period.
    

2. Comparative Analysis: Active Employment vs. COBRA under ESRD Rules
    
    The hospital asserts that because the patient was terminated and is receiving COBRA continuation coverage, they lack "current employment status," making Medicare primary. While this logic is correct for beneficiaries entitled to Medicare based on Age or Disability, it is legally incorrect for ESRD. Under federal regulations (42 CFR 411.161), COBRA continuation coverage is secondary to Medicare for aged or disabled beneficiaries; however, for an ESRD-related Medicare beneficiary, COBRA continuation coverage, if elected, remains strictly primary to Medicare during the entire 30-month ESRD coordination period.
    
3. Actionable Recommendation
    
    I recommend an absolute denial of this $55,000 claim for primary Medicare payment. The March 10, 2023 service date occurred within the 30-month coordination period, which did not expire until April 30, 2023. Furthermore, the patient's COBRA coverage remains the primary payer under ESRD-specific MSP provisions. The hospital must be instructed to bill the COBRA plan as the primary payer.