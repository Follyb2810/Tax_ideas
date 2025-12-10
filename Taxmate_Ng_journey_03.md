# TaxMate NG — User Flow Diagram & Tax Engine Algorithm

This document contains:

1. A **User Flow Diagram** (Mermaid) that maps the user experience from onboarding to audit defense.
2. A **Full Tax Engine Algorithm**: inputs, rules configuration, step-by-step pseudocode, edge cases, unit-test examples, and performance notes.

---

# 1) User Flow Diagram

```mermaid
flowchart TD
  A[Start: Open App / Web] --> B[Sign Up / Login]
  B --> C{New or Returning User}
  C -->|New| D[Onboarding: choose income profile]
  C -->|Returning| E[Dashboard]
  D --> E

  E --> F[Add Income Source]
  F --> G[Income Modal: name, amount, frequency, currency, category]
  G --> H[Save Income Source]
  H --> E

  E --> I[View Tax Summary]
  I --> J{Review or Edit incomes}
  J -->|Edit| F
  J -->|OK| K[Monthly Reminders & Notifications]

  I --> L[File Now]
  L --> M[Generate Filing Pack (PDF)]
  M --> N[Upload receipts/payments]
  N --> O[Filing Complete]

  E --> P[Audit Mode]
  P --> Q[Generate Audit PDF Pack]
  Q --> R[Export / Email / Share]

  E --> S[Subscription Management]
  S --> T[Payments / Webhooks]

  style A fill:#f9f,stroke:#333,stroke-width:1px
  style R fill:#cfc,stroke:#333,stroke-width:1px
```

_Notes:_

- The diagram is intentionally compact — consider expanding into separate flows for _Onboarding_, _Income Management_, _Filing_, and _Audit_ when creating UI wireframes.
- Notifications are driven by a background scheduler and user preferences.

---

# 2) TAX ENGINE: Full Logic

This section defines a configurable tax engine you can run on the backend. It is written to be **data-driven** so legal changes (bands, reliefs, thresholds) can be updated in configuration rather than code.

## 2.1 Definitions & Inputs

**Inputs (per user, per period):**

- `user_id`
- `period` (YYYY-MM, e.g., `2026-03`)
- `incomes[]` — list of income objects: `{source_name, amount, currency, frequency, category, date_received}`
- `deductions[]` — list of deductible items: `{type, amount, proof_url}`
- `reliefs[]` — user-claimed reliefs (if any) `{type, amount}`
- `exemptions[]` — items permanently zero-rated for the user
- `exchange_rates` — mapping for currency -> NGN for the day or period
- `tax_rules_config` — config object containing: `bands`, `consolidated_relief`, `standard_relief`, `allowable_deduction_types`, `tax_free_threshold`, `tax_year_start`, `tax_year_end`, and `special_exemptions`

**Outputs:**

- `total_income_ngn`
- `taxable_income_ngn`
- `tax_liability_breakdown` (by band)
- `tax_due_for_period` (monthly/quarterly/annual as configured)
- `breakdown_by_source`
- `fiscal_report_pdf_url`

## 2.2 Configuration Example (JSON)

```json
{
  "tax_year_start": "2026-01-01",
  "tax_year_end": "2026-12-31",
  "tax_free_threshold": 0,
  "consolidated_relief": {
    "type": "percentage_plus_fixed",
    "percent": 0.01,
    "fixed": 200000
  },
  "bands": [
    { "from": 0, "to": 300000, "rate": 0.07 },
    { "from": 300001, "to": 600000, "rate": 0.11 },
    { "from": 600001, "to": 1100000, "rate": 0.15 },
    { "from": 1100001, "to": 1600000, "rate": 0.19 },
    { "from": 1600001, "to": 3200000, "rate": 0.21 },
    { "from": 3200001, "to": null, "rate": 0.24 }
  ],
  "allowable_deduction_types": ["pension", "business_expense", "insurance"]
}
```

> **Important:** The `bands` and `consolidated_relief` fields above are placeholders. Put authoritative tax rates and rules from the local NRS in `tax_rules_config` in production.

## 2.3 High-level Algorithm (Steps)

1. **Normalize incomes**

   - For each income in `incomes[]`: convert `amount` to NGN using `exchange_rates` (if `currency != NGN`) and normalize `frequency` to monthly or annual values depending on `period`.

2. **Aggregate incomes**

   - Sum all normalized incomes for the period → `gross_income_ngn`.

3. **Apply tax-free threshold**

   - `income_after_threshold = max(gross_income_ngn - tax_free_threshold, 0)`

4. **Apply allowable deductions**

   - Filter `deductions[]` by `allowable_deduction_types` and sum → `total_deductions`.
   - `income_after_deductions = max(income_after_threshold - total_deductions, 0)`

5. **Apply consolidated relief**

   - If `consolidated_relief.type == 'percentage_plus_fixed'`: `cr = max(income_after_deductions * percent, fixed)` (or apply official formula)
   - `taxable_income = max(income_after_deductions - cr, 0)`

6. **Compute tax per bands**

   - For each band in `bands` (ordered low→high):

     - Compute `taxable_amount_in_band = clamp(taxable_income, band.from, band.to) - band.from`
     - `tax_for_band = taxable_amount_in_band * band.rate`
     - accumulate `tax_for_band` into `total_tax` and record breakdown

7. **Apply tax credits/withholding**

   - Subtract already withheld tax (if any) for the period from `total_tax` → `tax_due` (ensure not negative)

8. **Round and produce outputs**

   - Round currency to nearest naira (or smallest unit)
   - Produce `tax_liability_breakdown`, `effective_tax_rate = total_tax / gross_income_ngn`

## 2.4 Pseudocode (detailed)

```python
# Pseudocode for tax calculation

def calculate_tax(user_id, period, incomes, deductions, reliefs, exchange_rates, config):
    # 1) Normalize incomes to NGN for the period
    normalized = []
    for inc in incomes:
        amount_ngn = convert_to_ngn(inc.amount, inc.currency, exchange_rates)
        monthly_amount_ngn = normalize_frequency(amount_ngn, inc.frequency, period)
        normalized.append({**inc, 'amount_ngn': monthly_amount_ngn})

    # 2) Aggregate
    gross_income = sum(x['amount_ngn'] for x in normalized)

    # 3) Threshold
    income_after_threshold = max(gross_income - config['tax_free_threshold'], 0)

    # 4) Deductions
    allowable = [d['amount'] for d in deductions if d['type'] in config['allowable_deduction_types']]
    total_deductions = sum(allowable)
    income_after_deductions = max(income_after_threshold - total_deductions, 0)

    # 5) Consolidated relief example
    cr_conf = config['consolidated_relief']
    if cr_conf['type'] == 'percentage_plus_fixed':
        cr_calc = max(income_after_deductions * cr_conf['percent'], cr_conf['fixed'])
    else:
        cr_calc = cr_conf.get('fixed', 0)

    taxable_income = max(income_after_deductions - cr_calc, 0)

    # 6) Band calculation
    total_tax = 0
    breakdown = []
    remaining = taxable_income
    for band in config['bands']:
        lower = band['from']
        upper = band['to'] if band['to'] is not None else float('inf')
        if taxable_income <= lower:
            break
        taxable_in_this_band = min(taxable_income, upper) - lower
        tax_in_band = taxable_in_this_band * band['rate']
        breakdown.append({'band': f"{lower}-{upper}", 'taxable': taxable_in_this_band, 'tax': tax_in_band})
        total_tax += tax_in_band

    # 7) Subtract withheld/credits
    withheld = sum(r.get('amount',0) for r in reliefs if r.get('type') == 'withheld')
    tax_due = max(total_tax - withheld, 0)

    return {
        'gross_income': gross_income,
        'taxable_income': taxable_income,
        'total_tax': round(total_tax),
        'tax_due': round(tax_due),
        'breakdown': breakdown,
        'effective_tax_rate': total_tax / gross_income if gross_income>0 else 0
    }
```

## 2.5 Frequency Normalization

You must normalize incomes of varying frequencies (daily, weekly, monthly, annual) into the period unit (monthly or yearly) that the engine uses.

**Rules:**

- `daily_amount * business_days_in_month` (assume 22 or configurable)
- `weekly_amount * 52 / 12` → monthly
- `annual_amount / 12` → monthly
- `one-off` incomes should be allocated to the month of `date_received`

## 2.6 Exchange Rate Handling

- Prefer **per-transaction** exchange rate if available (best accuracy for tax defense).
- If not, use a configured `daily` rate service (caching in Redis for that day).
- Store `amount_original` and `amount_ngn` in `income` row for auditability.

## 2.7 Edge Cases & Rules

1. **Negative incomes**: reject or treat as correction entries (flag for manual review).
2. **Multiple currencies**: convert and store both values.
3. **Partial months**: prorate annual salaries based on days active in month.
4. **Large one-offs**: support smoothing options (user opt-in to spread over months) — but always show both options in UI.
5. **Zero/negative tax**: show `tax_due = 0` and store `tax_credit` if withheld > tax.
6. **Rounding differences**: keep unrounded internal values and round only for display & PDF to avoid small reconciling differences.

## 2.8 Auditability & Logging

- Write an immutable `tax_calculation` record every time the engine computes for a period:

  - `calculation_id`, `user_id`, `period`, `inputs_hash`, `config_version`, `computed_values`, `timestamp`

- Store proof of inputs (receipt URLs, exchange rate used) for each income item.
- Allow `recompute` endpoint that indicates `recompute_reason` (e.g., updated income, policy change) and keeps previous calculation record.

## 2.9 Unit Tests / Examples

**Example Test Case 1 (multiple incomes)**

- incomes: `[{Upwork, $500, monthly}, {Salary, ₦200000, monthly}]`
- exchange rate: `1 USD = 1500 NGN`
- deductions: `[{pension, 10000}]`
- config: (use config above)

**Expected flow:**

- Convert $500 → 750000 NGN
- gross_income = 750000 + 200000 = 950000 NGN
- apply deductions, relief, bands → return `tax_due`

Include a suite of tests that assert intermediate values: `gross_income`, `total_deductions`, `cr_calc`, `taxable_income`, `tax_per_band`, and `tax_due`.

## 2.10 Performance & Scaling

- Single calculation is O(B + N) where B = number of bands, N = number of income items.
- Typical user N is small (<20). Keep tax rules in memory or in a fast KV store for fast access.
- Cache last computed result for the (user_id, period, config_version) triplet to avoid recomputation.
- For bulk recalculations (e.g., tax-law update), run batched workers with concurrency and rate-limiting.

## 2.11 API Mappings

- `POST /tax/compute` — compute tax for given user and period (idempotent: returns cached calculation if inputs/config unchanged)
- `GET /tax/:user_id/:period` — fetch last computed tax record
- `POST /tax/recompute` — force recompute with reason
- `GET /tax/config` — fetch active tax config

---

# Appendix: Tips for Implementation

1. Keep tax rules **config-driven** and versioned. Each time rules change, bump `config_version`.
2. Keep every monetary conversion and intermediate step recorded to a `calculation_audit` table for defense in audits.
3. Ensure timezone and date handling is consistent across frontend/backends.
4. Provide 'what-if' mode in UI to allow users test scenarios (e.g., add one-off income) without saving.

---

If you want, I can now:

- Export this doc as a **PDF** or **Swagger** spec for API endpoints
- Generate **ER diagrams** for the income/tax tables
- Produce **unit test code** in Python/JS for the tax engine

Tell me which next.
