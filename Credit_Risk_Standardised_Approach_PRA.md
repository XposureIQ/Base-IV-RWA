# Credit Risk — Standardised Approach (PRA Rulebook)

**Regulatory basis:** PRA Rulebook — Credit Risk: Standardised Approach (CRR) Part, including the Basel 3.1 final rules published in PS1/26.

**Target effective date:** 1 January 2027  
**Document status:** Implementation / design specification  
**Reference date:** 30 September 2026  
**Jurisdiction:** United Kingdom (PRA-regulated CRR firms)

> **Important:** This document is an implementation-oriented summary, not legal advice and not a substitute for the PRA Rulebook, applicable CRR provisions retained in UK law, PRA supervisory statements, permissions, reporting instructions, or firm policy. The implementation should be validated against the rule text applicable to the firm's scope and reporting date.

---

## 1. Purpose

Define a standardized, auditable approach for calculating **credit-risk risk-weighted exposure amounts (RWEAs)** under the PRA's Standardised Approach (SA).

The intended implementation should:

- classify each exposure into the correct exposure class;
- calculate the regulatory exposure value (including applicable credit conversion factors for off-balance-sheet items);
- determine the applicable risk weight from the PRA rules, including external credit assessment where permitted;
- recognize eligible credit risk mitigation (CRM) consistently with the Credit Risk Mitigation (CRR) Part;
- apply specific treatments for real estate, defaulted exposures, specialised lending, covered bonds, CIUs, equity and other items;
- calculate RWEA at exposure level and aggregate to portfolio / reporting level;
- retain enough data and evidence to reproduce every regulatory result.

---

## 2. Regulatory hierarchy

Use the following order of precedence when implementing or resolving a rule:

1. **PRA Rulebook / applicable UK CRR provisions** in force for the calculation date.
2. **PRA permissions and firm-specific supervisory requirements.**
3. **PRA supervisory statements** explaining supervisory expectations.
4. **PRA reporting and disclosure instructions** for regulatory submissions.
5. Internal policy, methodology and system rules, provided they do not override the regulatory requirements.

For the Basel 3.1 target state, the PRA's final rules in **PRA2026/1** come into force on **1 January 2027**. The July 2026 amendments do not change the underlying Basel 3.1 policy and are also reflected in the final supervisory material.

---

## 3. Regulatory source map

| Topic | Primary PRA source |
|---|---|
| Standardised Approach rules | Credit Risk: Standardised Approach (CRR) Part, PRA2026/1, Annex D |
| Basel 3.1 final policy | PS1/26 — Implementation of Basel 3.1: Final rules |
| Supervisory expectations | SS10/13 — Credit risk — standardised approach |
| Credit risk mitigation | Credit Risk Mitigation (CRR) Part; SS17/13 where applicable |
| ECAI / credit assessment mapping | Articles 135–141 and applicable PRA / retained technical standards |
| Reporting | PRA regulatory reporting templates and instructions applicable to the reporting date |

Official sources:

- https://www.bankofengland.co.uk/prudential-regulation/publication/2026/january/implementation-of-the-basel-3-1-final-rules-policy-statement
- https://www.prarulebook.co.uk/-/media/pra/files/legal-instruments/2026/pra2026-1.pdf
- https://www.bankofengland.co.uk/prudential-regulation/publication/2013/credit-risk-standardised-approach-ss

---

## 4. End-to-end calculation flow

```text
Source exposure
      |
      v
1. Validate / enrich exposure data
      |
      v
2. Perform required due diligence
      |
      v
3. Determine exposure value
      |  - on-balance-sheet value
      |  - off-balance-sheet CCF
      |  - derivatives / SFT / long-settlement rules
      v
4. Classify exposure
      |
      +--> Sovereign / central bank
      +--> RGLA
      +--> PSE
      +--> MDB
      +--> International organisation
      +--> Institution
      +--> Corporate / specialised lending
      +--> Retail
      +--> Real estate
      +--> Defaulted
      +--> Covered bond
      +--> CIU
      +--> Equity / subordinated debt / own-funds instrument
      +--> Other item
      |
      v
5. Determine base risk weight
      |  - ECAI / credit quality step where permitted
      |  - unrated methodology where applicable
      |  - LTV / real-estate dependency where applicable
      |
      v
6. Apply CRM / risk-weight substitution or other permitted adjustment
      |
      v
7. Apply currency-mismatch treatment where applicable
      |
      v
8. Calculate RWEA
      |
      v
9. Aggregate + reconcile + report
```

Core formula:
**RWEA = Exposure Value × Applicable Risk Weight**

For exposures subject to CRM, the exposure value and/or risk weight must be modified using the applicable CRM method before the final RWEA is determined.

---

## 5. Due diligence requirements

The PRA Standardised Approach requires the institution to understand the **risk profile, creditworthiness and characteristics** of exposures at obligor and portfolio level.

Due diligence should be proportionate to the firm's nature, scale and complexity.

Operationally, the process should include:

- assessment of the obligor's operating and financial condition;
- controls ensuring the correct regulatory risk-weight assignment;
- due diligence before the exposure is incurred and at least annually thereafter;
- exposure-level due diligence where reasonably practicable;
- consideration of the effect of corporate-group membership on risk profile / creditworthiness.

The Rulebook contains specific exceptions for certain sovereign, MDB and international-organisation exposures.

### Minimum evidence set

Store at least:

- obligor ID and legal entity;
- group / connected-client identifier;
- exposure ID and product;
- exposure class and rationale;
- financial / regulatory information used in due diligence;
- external credit assessment, provider, date and mapping;
- internal classification where an unrated methodology is used;
- collateral / guarantee information;
- LTV and property value evidence where applicable;
- final risk weight;
- overrides and approvals;
- calculation timestamp and methodology version.

---

## 6. Exposure value

### 6.1 On-balance-sheet assets

For an asset item, exposure value is generally based on the accounting value after applicable specific credit risk adjustments, additional value adjustments and relevant own-funds reductions as specified by the Rulebook.

### 6.2 Off-balance-sheet items

For items in the PRA conversion-factor table:

**Exposure Value = Nominal Amount after applicable adjustments × CCF**

The 2027 target rule contains, among others, conversion-factor treatments for:

| Off-balance-sheet category | CCF / treatment |
|---|---:|
| Certain direct credit substitutes / guarantees | 100% |
| Certain commitments | 50% or 40%, depending on the rule category |
| Certain trade-related documentary items | 20% |
| Unconditionally cancellable commitments meeting the rule | 10% |

**Implementation note:** Do not hard-code CCF solely from product name. Use a regulatory eligibility decision tree based on the exact characteristics of the item and the applicable Article 111 category.

### 6.3 Derivatives

Derivative exposure values are determined using the applicable counterparty-credit-risk methodology rather than treating the accounting carrying amount as the regulatory exposure value.

### 6.4 Securities financing transactions / long settlement transactions

Where CRM is incorporated into the exposure-value calculation, use the relevant provisions in the Credit Risk Mitigation and Counterparty Credit Risk parts.

---

## 7. Exposure classification

The 2027 Standardised Approach contains the following major exposure classes:

1. Central governments or central banks
2. Regional governments or local authorities
3. Public sector entities
4. Multilateral development banks
5. International organisations
6. Institutions
7. Corporates
8. Retail
9. Real estate
10. Covered bonds
11. Exposures in default
12. CIUs
13. Subordinated debt, equity and other own-funds instruments
14. Other items

Classification must be deterministic, documented and mutually consistent with downstream risk-weight logic.

---

## 8. Sovereign / central-government / central-bank exposures

For the 2027 PRA rules, exposures to central governments or central banks generally receive **100%** unless another specified treatment applies.

Where a nominated ECAI assessment is available, the credit-quality-step table applies:

| Credit Quality Step | 1 | 2 | 3 | 4 | 5 | 6 |
|---|---:|---:|---:|---:|---:|---:|
| Risk weight | 0% | 20% | 50% | 100% | 100% | 150% |

A specific UK treatment applies to exposures to the UK central government and Bank of England that are denominated and funded in sterling: **0%**.

Export-credit-agency assessments may be used where the conditions in Article 137 are met. The corresponding MEIP risk-weight table is 0%, 0%, 20%, 50%, 100%, 100%, 100%, 150% for MEIP 0–7.

---

## 9. Regional governments / local authorities / PSEs / MDBs / international organisations

These exposure classes require rule-specific treatment. They must not be collapsed into a generic "government" bucket.

Key implementation principles:

- identify whether the counterparty is UK or third-country;
- determine whether the exposure can be treated as sovereign-equivalent under the relevant article;
- use the applicable ECAI / credit-quality-step mapping where required;
- apply short-maturity preferential treatments only when the precise conditions are met;
- maintain a reference table for zero-weight MDBs and international organisations.

Examples of MDBs receiving 0% under Article 117(2) include the World Bank / IBRD, IFC, Inter-American Development Bank, Asian Development Bank, African Development Bank, EIB, EIF, Islamic Development Bank and Asian Infrastructure Investment Bank, subject to the Rulebook's precise eligibility wording.

International organisations listed in Article 118(1), including the EU, IMF and BIS, receive 0% subject to the article.

---

## 10. Institutions

### 10.1 Rated institutions

Use the appropriate Article 120 treatment based on credit-quality step, maturity and whether the relevant short-term assessment applies.

### 10.2 Unrated institutions

Unrated institutions are classified into **Grade A, Grade B or Grade C** using the regulatory criteria.

For exposures with original maturity **greater than three months**:

| Grade | Risk weight |
|---|---:|
| A | 40% |
| B | 75% |
| C | 150% |

For exposures with original maturity **three months or less**, and specified qualifying trade exposures of up to six months:

| Grade | Risk weight |
|---|---:|
| A | 20% |
| B | 50% |
| C | 150% |

A further **30%** risk weight may be available for qualifying Grade A exposures with original maturity greater than three months where the additional CET1 and leverage-ratio requirements in Article 121(5) are satisfied.

External assessments must be subjected to the required due-diligence test. Where due diligence indicates higher risk than implied by the assessment, the risk weight must be increased by at least one credit-quality step as required by Article 120(4).

---

## 11. Corporates

### 11.1 Rated corporate exposures

For an available nominated-ECAI assessment:

| Credit Quality Step | 1 | 2 | 3 | 4 | 5 | 6 |
|---|---:|---:|---:|---:|---:|---:|
| Risk weight | 20% | 50% | 75% | 100% | 150% | 150% |

Short-term assessments use the separate Article 122 short-term table.

### 11.2 Unrated corporates

The default treatment for unrated corporate exposures is **100%**, unless an applicable PRA permission permits the alternative investment-grade approach.

Where that permission is held:

| Internal assessment | Risk weight |
|---|---:|
| Investment grade | 65% |
| Not investment grade | 135% |

The investment-grade assessment must satisfy the regulatory criteria and should incorporate the firm's internal credit assessment system.

For an SME that is not retail and has no nominated-ECAI assessment, the specified risk weight is **85%**.

---

## 12. Specialised lending

A corporate exposure (other than real estate) is a specialised lending exposure when the characteristics in Article 122A are met, including:

- financing / operating specific physical assets;
- little or no independent repayment capacity beyond the financed assets;
- substantial lender control over assets and related income; and
- primary repayment from the asset-generated cash flows.

Sub-categories:

- object finance;
- commodities finance;
- project finance.

Without a relevant issue-specific ECAI assessment:

| Specialised lending | Risk weight |
|---|---:|
| Object finance | 100% |
| Commodities finance | 100% |
| Project finance — pre-operational | 130% |
| Project finance — operational, subject to conditions | 100% |

Do not classify real-estate or ordinary corporate lending as specialised lending merely because repayment depends on a specific project; all Article 122A conditions need to be considered.

---

## 13. Retail exposures

An exposure may qualify as retail where the applicable natural-person / SME criteria are met and, for SME retail, the relevant product, aggregation, materiality and granularity conditions are satisfied.

A key threshold in the 2027 rules is **GBP 880,000** for the specified aggregate amount owed by the obligor / connected clients, excluding residential real estate exposures.

### Retail risk weights

| Retail category | Risk weight |
|---|---:|
| Regulatory retail — transactor | 45% |
| Regulatory retail — non-transactor | 75% |
| Other retail not qualifying as regulatory retail | 100% |
| Certain qualifying employee / pension-linked lending | 35% |

Real-estate exposures are excluded from the general retail class and must be routed through the real-estate rules.

---

## 14. Currency mismatch multiplier

For qualifying **unhedged retail and residential real-estate exposures**, apply a **1.5x multiplier** to the applicable risk weight, subject to a maximum risk weight of **150%**, where the regulatory currency-mismatch conditions are met.

For natural-person obligors, the principal test is whether the lending currency differs from the currency of the obligor's source of income.

A qualifying hedge requires the natural and/or financial hedge to cover at least **90% of the relevant instalment** under the conditions in Article 123B.

Implementation should calculate and persist:

```text
Base risk weight
        x
Currency mismatch multiplier (1.0 or 1.5)
        =
Adjusted risk weight, capped at 150%
```

---

## 15. Real-estate exposures

Real-estate exposures require a separate decision tree covering:

1. regulatory vs other real estate;
2. residential vs commercial;
3. acquisition, development and construction (ADC);
4. material dependence on property-generated cash flows;
5. LTV;
6. priority and amount of other charges;
7. counterparty category.

### 15.1 Regulatory residential real estate — not materially dependent on property cash flows

For the exposure portion up to **55% of property value**, the risk weight is **20%**. The residual exposure receives the applicable counterparty risk weight under Article 124L, subject to the detailed charge-priority rules.

### 15.2 Regulatory residential real estate — materially dependent on property cash flows

| LTV | Risk weight |
|---|---:|
| LTV <= 50% | 30% |
| 50% < LTV <= 60% | 35% |
| 60% < LTV <= 70% | 40% |
| 70% < LTV <= 80% | 50% |
| 80% < LTV <= 90% | 60% |
| 90% < LTV <= 100% | 75% |
| LTV > 100% | 105% |

Where the applicable charge-priority condition is present, a **1.25 multiplier** applies when LTV is greater than 50%.

### 15.3 Regulatory commercial real estate — not materially dependent

For qualifying natural-person / SME exposures, the portion up to 55% of property value receives **60%**, with the residual portion assigned the relevant counterparty risk weight.

For certain other counterparties, the Rulebook applies a higher-of / lower-of combination involving the counterparty risk weight and the corresponding dependent-property treatment.

### 15.4 Regulatory commercial real estate — materially dependent

| Condition | Risk weight |
|---|---:|
| LTV <= 80% | 100% |
| LTV > 80% | 110% |
| Certain prior-charge cases, LTV <= 60% | 100% |
| Certain prior-charge cases, 60% < LTV <= 80% | 125% |
| Certain prior-charge cases, LTV > 80% | 137.5% |

### 15.5 Other real estate

- materially dependent on property cash flows: **150%**;
- qualifying residential exposure not materially dependent: counterparty risk weight under Article 124L;
- qualifying commercial exposure not materially dependent: **higher of 60% and the applicable counterparty risk weight**.

### 15.6 ADC exposures

Default risk weight: **150%**.

A **100%** risk weight may be available for qualifying residential ADC exposures where prudent underwriting and the relevant pre-sale / pre-lease or substantial borrower-equity condition is met.

### 15.7 Real-estate cash-flow dependency assessment

The system should maintain explicit boolean / categorical fields for:

- primary residence;
- number of qualifying properties;
- social housing;
- association / cooperative status;
- self-build status;
- own-business-use test for commercial property;
- rental-income dependency;
- ADC status;
- property value and qualifying valuation date;
- LTV;
- prior / pari-passu charges.

The assessment must be refreshed at least annually where the Article requires annual reassessment, and more frequently where new information requires it.

---

## 16. Exposures in default

For the unsecured / unprotected part of a defaulted exposure:

| Specific credit risk adjustments as % of outstanding amount | Risk weight |
|---|---:|
| < 20% | 150% |
| >= 20% | 100% |

A defaulted residential real-estate exposure that is not materially dependent on the property's cash flows receives **100%** under Article 127(3).

Recognised collateral and unfunded credit protection must be incorporated using the permitted CRM method before applying the residual defaulted-exposure treatment.

---

## 17. Particularly high-risk exposures

Exposures associated with particularly high risk receive **150%**.

The assessment should consider whether there is:

- a high risk of loss following obligor default; or
- insufficient information to assess whether the exposure has such a high risk of loss.

The classification decision and evidence must be recorded.

---

## 18. Covered bonds

Eligible covered bonds receive preferential risk weights subject to the detailed eligibility, cover-pool and information requirements in Article 129.

For unrated eligible covered bonds, the risk weight is derived from the risk weight of the issuing institution according to the Article 129 correspondence table, including:

| Issuer institution risk weight | Covered-bond risk weight |
|---:|---:|
| 20% | 10% |
| 30% | 15% |
| 40% | 20% |
| 50% | 25% |
| 75% | 35% |
| 100% | 50% |
| 150% | 100% |

Portfolio information must include the specified cover-pool and asset characteristics and be received at least semi-annually where required.

---

## 19. Collective investment undertakings (CIUs)

For qualifying CIUs, the institution may use:

1. **Look-through approach** — risk-weight the underlying exposures as though directly held.
2. **Mandate-based approach** — calculate based on investment limits and the methodology specified by the Rulebook.
3. **Fallback approach** — **1,250%** where neither permitted approach is used.

Minimum information and reporting conditions must be satisfied. Under the look-through approach, underlying exposure information is subject to the required verification conditions.

For implementation, retain:

- CIU identifier;
- management company;
- mandate / prospectus;
- underlying exposure data;
- reporting date;
- chosen approach;
- leverage limits;
- derived RWEA;
- evidence of any independent verification.

---

## 20. Equity, subordinated debt and own-funds instruments

An instrument meeting the equity-exposure definition is assigned:

| Equity treatment | Risk weight |
|---|---:|
| Standard equity exposure | 250% |
| Higher-risk equity exposure | 400% |
| Subordinated debt / own-funds / equity instrument not classified as equity exposure | 150% |

Certain exposures are instead deducted from own funds or receive a prescribed **1,250% / 250%** treatment under the Own Funds (CRR) Part.

The implementation must therefore check **capital-deduction rules before simply assigning an SA risk weight**.

---

## 21. Other items

Examples include:

| Other item | Risk weight / treatment |
|---|---:|
| Tangible assets | 100% |
| Unallocable prepayments / accrued income | 100% |
| Cash in process of collection | 20% |
| Cash in hand / cash equivalents | 0% |
| Qualifying gold bullion | 0% |
| Lease residual value | Special Article 134 treatment |

Where the rules do not provide a specific risk-weight calculation, the general residual rule is **100%**, subject to the relevant deductions / exclusions.

---

## 22. ECAI and credit-assessment framework

The engine must distinguish:

- issuer-level vs issue-specific assessment;
- long-term vs short-term assessment;
- domestic-currency vs foreign-currency assessment;
- nominated ECAI eligibility;
- mapping to credit quality step;
- due-diligence override;
- issue ranking / seniority where relevant;
- whether the external assessment already reflects CRM.

Do not use an assessment mechanically simply because a rating exists in the source system. The regulatory conditions for use must be satisfied.

### Required ECAI data structure

```text
ECAI_ID
Assessment_ID
Obligor_ID / Issue_ID
Assessment_Type
Long_Term_or_Short_Term
Currency
Credit_Quality_Step
Effective_Date
Expiry_or_Review_Date
Nominated_ECAI_Flag
Issue_Specific_Flag
CRM_Already_Reflected_Flag
Due_Diligence_Result
Regulatory_Methodology_Version
```

---

## 23. Credit Risk Mitigation (CRM)

CRM should be implemented as a separate regulatory calculation component rather than embedded independently in every product rule.

Potential mitigation types include:

- eligible financial collateral;
- guarantees;
- credit derivatives;
- netting arrangements where recognized;
- other eligible protection mechanisms under the Credit Risk Mitigation (CRR) Part.

The engine should determine:

```text
Gross exposure value
        |
        +--> CRM eligibility
        |
        +--> Legal certainty
        |
        +--> Collateral / protection value
        |
        +--> Volatility / haircut requirements
        |
        +--> Maturity / currency mismatch adjustments
        v
CRM-adjusted exposure / risk-weight treatment
```

The Standardised Approach and the CRM methodology must use the exact method permitted for the exposure and for the firm's permissions.

---

## 24. Risk-weight engine decision logic

Recommended priority sequence:

```text
A. Is the item required to be deducted from own funds?
    -> Yes: apply deduction / prescribed treatment.
    -> No: continue.

B. Is it a securitisation position?
    -> Yes: route to securitisation framework.
    -> No: continue.

C. Is it a derivative / SFT / CCR exposure requiring a special exposure-value method?
    -> Yes: calculate regulatory exposure value first.
    -> No: continue.

D. Determine exposure class under Article 112.

E. Determine whether defaulted.

F. Determine whether a real-estate / ADC / specialised-lending / CIU / covered-bond rule applies.

G. Determine whether ECAI assessment is available and legally usable.

H. Perform required due diligence.

I. Determine base risk weight.

J. Apply CRM.

K. Apply currency-mismatch rule where applicable.

L. Apply caps / floors / special residual rules.

M. Calculate RWEA.

N. Produce reason code + evidence + methodology version.
```

---

## 25. Core data model

### Exposure table

| Field | Purpose |
|---|---|
| exposure_id | Unique regulatory exposure identifier |
| obligor_id | Counterparty identifier |
| group_id | Group / connected-client aggregation |
| product_type | Product classification |
| balance_type | On-balance / off-balance / derivative / SFT |
| nominal_amount | Original regulatory amount |
| accounting_value | Accounting value |
| specific_cra | Specific credit risk adjustment |
| exposure_value | Final regulatory exposure value before RW |
| exposure_class | Article 112 class |
| sub_class | Regulatory subclass |
| default_flag | Default status |
| real_estate_flag | Real-estate routing |
| property_value | Qualifying property value |
| ltv | Loan-to-value |
| cash_flow_dependency | Material dependence assessment |
| adc_flag | ADC status |
| specialised_lending_type | Object / commodity / project |
| ciu_flag | CIU exposure |
| equity_flag | Equity exposure |
| ecaI_id | Credit-assessment provider |
| credit_quality_step | Mapped CQS |
| internal_grade | Internal grade where used |
| crm_flag | CRM present |
| crm_method | Applied CRM method |
| currency_mismatch_flag | Article 123B indicator |
| base_risk_weight | Pre-adjustment RW |
| adjusted_risk_weight | Final RW |
| rwea | Risk-weighted exposure amount |
| rule_article | Main regulatory rule applied |
| methodology_version | Calculation version |
| override_flag | Manual override |
| override_reason | Controlled reason |
| evidence_reference | Source / document trace |

---

## 26. Calculation controls

### Control 1 — Exposure reconciliation

```text
Source GL / trade exposure population
        =
SA calculation population
        +
Documented exclusions
```

### Control 2 — Exposure-value reconciliation

For each class, reconcile accounting values / nominal values to regulatory exposure values after adjustments.

### Control 3 — Risk-weight reasonableness

Review distributions of final risk weights by:

- exposure class;
- product;
- geography;
- rating bucket;
- LTV bucket;
- default status.

### Control 4 — Rating validation

No ECAI assessment should be used unless the ECAI is approved / nominated and the assessment is within its permitted scope.

### Control 5 — Real-estate validation

Require evidence for:

- valuation date;
- property type;
- qualifying charge;
- LTV;
- cash-flow dependency;
- priority charges.

### Control 6 — CRM validation

Every CRM benefit must have an eligibility flag, legal-evidence reference, valuation date and method.

### Control 7 — Override governance

Every manual override must capture:

- original value;
- override value;
- regulatory article;
- reason;
- approver;
- effective date;
- expiry / review date.

---

## 27. Sample worked calculations

### Example A — Standard corporate

```text
Exposure value = GBP 10,000,000
ECAI CQS       = 3
Corporate RW   = 75%

RWEA = 10,000,000 x 75%
     = GBP 7,500,000
```

### Example B — Regulatory retail, non-transactor

```text
Exposure value = GBP 500,000
Risk weight    = 75%

RWEA = 500,000 x 75%
     = GBP 375,000
```

### Example C — Unrated SME corporate

```text
Exposure value = GBP 2,000,000
SME, not retail, no nominated-ECAI assessment
Risk weight    = 85%

RWEA = 2,000,000 x 85%
     = GBP 1,700,000
```

### Example D — Unhedged currency-mismatch retail

```text
Base RW                 = 75%
Currency mismatch      = Yes
Multiplier              = 1.5x
Adjusted RW             = 112.5%
Exposure value          = GBP 100,000

RWEA = 100,000 x 112.5%
     = GBP 112,500
```

### Example E — Regulatory residential real estate, property-dependent

```text
Exposure value = GBP 600,000
Property value = GBP 1,000,000
LTV            = 60%
Base RW        = 35%

RWEA = 600,000 x 35%
     = GBP 210,000
```

These examples illustrate the calculation mechanics only; real production calculations must execute every applicable eligibility and CRM condition.

---

## 28. Output dataset / regulatory result

Recommended result record:

```text
exposure_id
calculation_date
rulebook_version
methodology_version
exposure_value
exposure_class
exposure_subclass
credit_quality_step
rating_source
crm_method
crm_adjusted_value
base_risk_weight
currency_mismatch_multiplier
other_adjustments
final_risk_weight
rwea
rule_article
reason_code
override_flag
evidence_reference
```

The result must be reproducible from source data and the rulebook / methodology version used at the time of calculation.

---

## 29. Versioning and effective-date control

The application must be capable of running rules by **calculation date**.

Recommended pattern:

```text
Rulebook version
    |
    +--> Effective from
    +--> Effective to
    +--> Methodology version
    +--> Article / table version
```

For the Basel 3.1 target implementation described in this document:

```text
PRA2026/1
Effective: 2027-01-01
```

The system should not replace the currently applicable rule set before its legally effective date merely because the future rules have already been published.

---

## 30. Implementation checklist

- [ ] Exposure population reconciled to source systems.
- [ ] Article 112 classification implemented.
- [ ] Article 110A due-diligence controls implemented.
- [ ] Article 111 exposure-value / CCF logic implemented.
- [ ] Articles 114–134 risk-weight rules implemented.
- [ ] Articles 135–141 ECAI logic implemented.
- [ ] Real-estate LTV and cash-flow-dependency logic implemented.
- [ ] Currency-mismatch multiplier implemented.
- [ ] Defaulted exposure logic implemented.
- [ ] Specialised lending routing implemented.
- [ ] CIU look-through / mandate / fallback logic implemented.
- [ ] Equity and own-funds deduction checks implemented.
- [ ] CRM integration completed.
- [ ] Manual overrides governed and audited.
- [ ] Regulatory reporting mappings completed.
- [ ] Regression test pack created.
- [ ] 2026-to-2027 effective-date switch tested.
- [ ] Reconciliation and management controls signed off.

---

## 31. Regulatory test scenarios

At minimum, the automated regression suite should include:

1. Rated sovereign — CQS 1–6.
2. UK sterling central government / Bank of England exposure.
3. Rated and unrated institution — maturity <= 3 months / > 3 months.
4. Institution Grade A with and without 30% eligibility conditions.
5. Rated corporate — CQS 1–6.
6. Unrated corporate — standard 100% treatment.
7. Permitted investment-grade / non-investment-grade corporate treatment.
8. Unrated SME corporate — 85%.
9. Specialised lending — object / commodities / project, pre-op / operational.
10. Retail transactor / non-transactor / non-regulatory retail.
11. Currency mismatch — hedged / unhedged.
12. Residential property — LTV bands.
13. Residential property — material cash-flow dependence.
14. Commercial property — dependent / non-dependent.
15. ADC — ordinary and qualifying residential ADC.
16. Defaulted exposure — specific adjustments below / above 20%.
17. Particularly high-risk exposure.
18. Covered bond — rated / unrated issuer mapping.
19. CIU — look-through / mandate / fallback.
20. Equity / higher-risk equity / subordinated debt.
21. Off-balance-sheet items — each CCF bucket.
22. CRM — collateral / guarantee / credit-derivative cases.
23. Prior-ranking / pari-passu property charges.
24. ECAI due-diligence uplift.
25. Calculation-date boundary: 31-Dec-2026 vs 1-Jan-2027.

---

## 32. Key implementation cautions

### Do not assume "rating = risk weight"

The assessment must be legally usable, correctly mapped and consistent with due-diligence requirements.

### Do not classify solely by product code

Regulatory classification depends on counterparty, legal form, product characteristics and specific regulatory conditions.

### Do not treat collateral as automatically reducing RWEA

Only eligible CRM recognized under the applicable method provides regulatory benefit.

### Do not use accounting value as universal exposure value

Derivatives, SFTs, off-balance-sheet items, leases and other products have specific regulatory exposure-value rules.

### Do not hard-code the future rules as current rules

The Basel 3.1 final rules described here are effective from 1 January 2027. The engine must use effective-date versioning.

---

## 33. Sources and audit trail

Primary regulatory references used for this document:

1. **PRA2026/1 — PRA Rulebook: CRR Firms: (CRR) Instrument 2026**, Annex D — Credit Risk: Standardised Approach (CRR) Part. Effective 1 January 2027.
2. **PS1/26 — Implementation of Basel 3.1: Final rules**, Bank of England / PRA, published 20 January 2026.
3. **SS10/13 — Credit risk — standardised approach**, including the July 2026 future version effective 1 January 2027.
4. **PRA July 2026 low-impact amendments (LIAF02/26)** relating to corrections to the Standardised Approach provisions and SS10/13.

This document intentionally summarizes regulatory requirements rather than reproducing the legislative text.

---

## 34. Change log

| Version | Date | Change |
|---|---|---|
| 1.0 | 2026-09-30 | Initial implementation-oriented PRA Standardised Approach document aligned to the Basel 3.1 final rule set effective 1 January 2027. |

