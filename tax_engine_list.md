Perfect! Let’s **update our Express.js tax engine** so that it clearly shows:

- **Gross Salary**
- **Pension Contribution**
- **CRA (tax relief)**
- **Taxable Income** (after CRA and pension)
- **PAYE Tax**
- **Other deductions (NHF, NHIS/HMO)**
- **Net Salary**

This way, employees can **see how much is non-taxable vs taxable**.

---

### **Updated Express.js Employee Tax Calculation**

```javascript
// index.js
const express = require("express");
const bodyParser = require("body-parser");

const app = express();
app.use(bodyParser.json());

// PAYE tax bands (monthly)
const PAYE_BRACKETS = [
  { limit: 300000, rate: 0.07 },
  { limit: 300000, rate: 0.11 },
  { limit: 500000, rate: 0.15 },
  { limit: 500000, rate: 0.19 },
  { limit: 1600000, rate: 0.21 },
  { limit: Infinity, rate: 0.24 },
];

// State-specific multipliers (can be extended)
const STATE_PAYER_MULTIPLIER = {
  Lagos: 1.0,
  Abuja: 1.05,
  Kano: 0.95,
  default: 1.0,
};

function applyStateMultiplier(PAYE, state) {
  const multiplier =
    STATE_PAYER_MULTIPLIER[state] || STATE_PAYER_MULTIPLIER.default;
  return PAYE * multiplier;
}

// Calculate tax for one employee
function calculateEmployeeTax(employee) {
  const {
    fullName,
    baseSalary,
    housingAllowance = 0,
    transportAllowance = 0,
    bonus = 0,
    state,
  } = employee;

  // 1️⃣ Total Gross Salary including allowances and bonus
  const grossSalary =
    baseSalary + housingAllowance + transportAllowance + bonus;

  // 2️⃣ Pension contribution (8%)
  const pensionContribution = grossSalary * 0.08;
  const salaryAfterPension = grossSalary - pensionContribution;

  // 3️⃣ Consolidated Relief Allowance (CRA)
  const CRA1 = Math.max(200000, grossSalary * 0.01);
  const CRA2 = 0.2 * salaryAfterPension;
  const totalCRA = CRA1 + CRA2;

  // 4️⃣ Taxable income after CRA and pension
  let taxableIncome = grossSalary - pensionContribution - totalCRA;

  // 5️⃣ Low-income exemption
  const annualSalary = grossSalary * 12;
  if (annualSalary <= 800000) taxableIncome = 0;

  // 6️⃣ Calculate PAYE using progressive tax bands
  let PAYE = 0;
  let remainingIncome = taxableIncome;

  for (let bracket of PAYE_BRACKETS) {
    if (remainingIncome <= 0) break;
    const incomeInBracket = Math.min(remainingIncome, bracket.limit);
    PAYE += incomeInBracket * bracket.rate;
    remainingIncome -= incomeInBracket;
  }

  // 7️⃣ Apply state multiplier
  PAYE = applyStateMultiplier(PAYE, state);

  // 8️⃣ Other statutory deductions
  const NHF = grossSalary * 0.025;
  const NHIS_HMO = grossSalary * 0.05;

  // 9️⃣ Net salary
  const netSalary = grossSalary - (pensionContribution + PAYE + NHF + NHIS_HMO);

  return {
    fullName,
    grossSalary,
    pensionContribution,
    CRA: totalCRA,
    nonTaxableIncome: grossSalary - taxableIncome, // what CRA + pension covers
    taxableIncome,
    PAYE,
    NHF,
    NHIS_HMO,
    netSalary,
    state,
    allowances: { housingAllowance, transportAllowance, bonus },
  };
}

// Endpoint for multiple employees
app.post("/calculate-tax-bulk", (req, res) => {
  const { employees } = req.body;
  if (!employees || !Array.isArray(employees)) {
    return res.status(400).json({ error: "Employees array is required" });
  }

  const results = employees.map((emp) => calculateEmployeeTax(emp));

  // Company summary
  const summary = results.reduce(
    (acc, emp) => {
      acc.totalGross += emp.grossSalary;
      acc.totalPension += emp.pensionContribution;
      acc.totalPAYE += emp.PAYE;
      acc.totalNHF += emp.NHF;
      acc.totalNHIS += emp.NHIS_HMO;
      acc.totalNetSalary += emp.netSalary;
      return acc;
    },
    {
      totalGross: 0,
      totalPension: 0,
      totalPAYE: 0,
      totalNHF: 0,
      totalNHIS: 0,
      totalNetSalary: 0,
    }
  );

  res.json({ employees: results, companySummary: summary });
});

// Start server
const PORT = process.env.PORT || 3000;
app.listen(PORT, () => console.log(`Tax Engine running on port ${PORT}`));
```

---

### **✅ Key Updates**

1. **`nonTaxableIncome` field** shows how much is covered by CRA + pension.
2. Employees can clearly see: **what portion of their salary is taxable vs non-taxable**.
3. **Gross, allowances, PAYE, and net salary** are all visible.
4. Still supports **multiple employees** and **company-level summary**.

---

If you want, I can **also create a visual diagram showing taxable vs non-taxable portions** for any salary, so employees can **see exactly how CRA reduces their tax**.

Do you want me to do that next?
