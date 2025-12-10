Got it! We can build a **realistic Nigerian tax engine in Express.js** using **actual tax components** like PAYE, Pension, NHF, NHIS/HMO, and CRA, with **clear comments and real variable names**. Here’s a complete, commented version you can use:

---

### **Express.js Nigerian Tax Engine**

```javascript
// index.js
const express = require("express");
const bodyParser = require("body-parser");

const app = express();
app.use(bodyParser.json());

// --- Function to calculate PAYE (Nigeria) ---
function calculateNigerianTax(grossSalary) {
  // 1️⃣ Pension Contribution (8% of gross salary)
  const pensionContribution = grossSalary * 0.08;
  const salaryAfterPension = grossSalary - pensionContribution;

  // 2️⃣ Consolidated Relief Allowance (CRA)
  // CRA1: Max of ₦200,000 or 1% of gross salary
  const CRA1 = Math.max(200000, grossSalary * 0.01);
  // CRA2: 20% of (Gross Salary - Pension)
  const CRA2 = 0.2 * salaryAfterPension;
  const totalCRA = CRA1 + CRA2;

  // 3️⃣ Taxable Income
  let taxableIncome = grossSalary - pensionContribution - totalCRA;
  if (taxableIncome < 0) taxableIncome = 0;

  // 4️⃣ Apply progressive PAYE tax bands (monthly)
  // Note: brackets based on FIRS/State PAYE guide
  let PAYE = 0;
  let remainingIncome = taxableIncome;

  const payeBrackets = [
    { limit: 300000, rate: 0.07 }, // 7% on first ₦300,000
    { limit: 300000, rate: 0.11 }, // 11% on next ₦300,000
    { limit: 500000, rate: 0.15 }, // 15% on next ₦500,000
    { limit: 500000, rate: 0.19 }, // 19% on next ₦500,000
    { limit: 1600000, rate: 0.21 }, // 21% on next ₦1,600,000
    { limit: Infinity, rate: 0.24 }, // 24% on remaining income
  ];

  for (let bracket of payeBrackets) {
    if (remainingIncome <= 0) break;
    const incomeInBracket = Math.min(remainingIncome, bracket.limit);
    PAYE += incomeInBracket * bracket.rate;
    remainingIncome -= incomeInBracket;
  }

  // 5️⃣ Other statutory deductions (if applicable)
  // NHF: 2.5% of basic salary (optional for private employees)
  const NHF = grossSalary * 0.025;

  // NHIS/HMO: 5% of gross salary (if employer deducts)
  const NHIS_HMO = grossSalary * 0.05;

  // 6️⃣ Net Salary = Gross - (Pension + PAYE + NHF + NHIS)
  const netSalary = grossSalary - (pensionContribution + PAYE + NHF + NHIS_HMO);

  return {
    grossSalary,
    pensionContribution,
    totalCRA,
    taxableIncome,
    PAYE,
    NHF,
    NHIS_HMO,
    netSalary,
  };
}

// --- API Endpoint ---
app.post("/calculate-tax", (req, res) => {
  const { grossSalary } = req.body;

  if (!grossSalary) {
    return res.status(400).json({ error: "Gross salary is required" });
  }

  const taxResult = calculateNigerianTax(grossSalary);
  res.json(taxResult);
});

app.post("/calculate-tax-bulk", (req, res) => {
  const { employees } = req.body;

  if (!employees || !Array.isArray(employees)) {
    return res.status(400).json({ error: "Employees array is required" });
  }

  const results = employees.map((emp) => {
    const { grossSalary, state } = emp;
    // You can extend calculateNigerianTax to take "state" for different tax bands
    return { ...emp, taxDetails: calculateNigerianTax(grossSalary) };
  });

  res.json(results);
});

// --- Start Server ---
const PORT = process.env.PORT || 3000;
app.listen(PORT, () => {
  console.log(`Nigerian Tax Engine running on port ${PORT}`);
});
```

---

### **✅ Features of This Tax Engine**

1. Uses **real Nigerian statutory deductions**: Pension (8%), CRA, NHF (2.5%), NHIS/HMO (5%).
2. Implements **progressive PAYE tax bands**.
3. Returns a **detailed breakdown** of all deductions.
4. Ready for **integration into payroll software**.

---

If you want, I can also **add a feature to handle exemptions automatically**, like **employees earning ≤ ₦800,000/year** or **small business rules**, so it fully aligns with **the 2024 Nigerian tax reform**.

Do you want me to add that next?
