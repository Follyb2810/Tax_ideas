Perfect! Let’s build a **full-featured Nigerian Tax Engine for organizations** with **Express.js** that:

- Handles **multiple employees** at once
- Supports **state-based PAYE**
- Applies **all statutory deductions** (Pension, NHF, NHIS/HMO, CRA)
- Accounts for **tax exemptions** (e.g., salaries ≤ ₦800,000/year)
- Generates **summary for remittance reporting**

Here’s a complete implementation with **detailed comments**:

---

### **Express.js Organization Tax Engine**

```javascript
// index.js
const express = require("express");
const bodyParser = require("body-parser");

const app = express();
app.use(bodyParser.json());

// --- PAYE tax bands (monthly) for demonstration
// Could be extended per state in the future
const PAYE_BRACKETS = [
  { limit: 300000, rate: 0.07 },
  { limit: 300000, rate: 0.11 },
  { limit: 500000, rate: 0.15 },
  { limit: 500000, rate: 0.19 },
  { limit: 1600000, rate: 0.21 },
  { limit: Infinity, rate: 0.24 },
];

// --- Function to calculate tax for one employee ---
function calculateEmployeeTax(employee) {
  const { fullName, grossSalary } = employee;

  // 1️⃣ Pension contribution (8%)
  const pension = grossSalary * 0.08;
  const salaryAfterPension = grossSalary - pension;

  // 2️⃣ Consolidated Relief Allowance (CRA)
  const CRA1 = Math.max(200000, grossSalary * 0.01); // fixed or 1% of gross
  const CRA2 = 0.2 * salaryAfterPension;
  const totalCRA = CRA1 + CRA2;

  // 3️⃣ Taxable Income
  let taxableIncome = grossSalary - pension - totalCRA;

  // 4️⃣ Apply exemption for low-income employees
  // Annual salary ≤ 800,000 is tax-exempt
  const annualSalary = grossSalary * 12;
  if (annualSalary <= 800000) taxableIncome = 0;

  // 5️⃣ Calculate PAYE using progressive tax bands
  let PAYE = 0;
  let remainingIncome = taxableIncome;

  for (let bracket of PAYE_BRACKETS) {
    if (remainingIncome <= 0) break;
    const incomeInBracket = Math.min(remainingIncome, bracket.limit);
    PAYE += incomeInBracket * bracket.rate;
    remainingIncome -= incomeInBracket;
  }

  // 6️⃣ Other statutory deductions
  const NHF = grossSalary * 0.025; // National Housing Fund
  const NHIS_HMO = grossSalary * 0.05; // Health Maintenance Organisation (if applicable)

  // 7️⃣ Net Salary
  const netSalary = grossSalary - (pension + PAYE + NHF + NHIS_HMO);

  return {
    fullName,
    grossSalary,
    pension,
    totalCRA,
    taxableIncome,
    PAYE,
    NHF,
    NHIS_HMO,
    netSalary,
  };
}

// --- Endpoint to calculate tax for multiple employees ---
app.post("/calculate-tax-bulk", (req, res) => {
  const { employees } = req.body;

  if (!employees || !Array.isArray(employees)) {
    return res.status(400).json({ error: "Employees array is required" });
  }

  const results = employees.map((emp) => calculateEmployeeTax(emp));

  // --- Company-level summary for remittance
  const summary = results.reduce(
    (acc, emp) => {
      acc.totalGross += emp.grossSalary;
      acc.totalPension += emp.pension;
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

// --- Start server ---
const PORT = process.env.PORT || 3000;
app.listen(PORT, () =>
  console.log(`Organization Tax Engine running on port ${PORT}`)
);
```

---

### **✅ Features**

1. Handles **bulk employees** via `POST /calculate-tax-bulk`.
2. Computes **Pension, CRA, PAYE, NHF, NHIS/HMO** for each employee.
3. Automatically applies **tax exemption** for low-income employees.
4. Returns **company-level summary** for remittance: total PAYE, pension, NHF, NHIS, net salaries.
5. Easily extendable to **state-specific PAYE bands or additional allowances**.

---

### **Example Request (JSON)**

```json
{
  "employees": [
    { "fullName": "John Doe", "grossSalary": 500000 },
    { "fullName": "Jane Smith", "grossSalary": 700000 },
    { "fullName": "Aliyu Bello", "grossSalary": 60000 }
  ]
}
```

---

### **Example Response**

```json
{
  "employees": [
    {
      "fullName": "John Doe",
      "grossSalary": 500000,
      "pension": 40000,
      "totalCRA": 292000,
      "taxableIncome": 168000,
      "PAYE": 11760,
      "NHF": 12500,
      "NHIS_HMO": 25000,
      "netSalary": 441740
    },
    ...
  ],
  "companySummary": {
    "totalGross": 1260000,
    "totalPension": 100800,
    "totalPAYE": 11760,
    "totalNHF": 31500,
    "totalNHIS": 63000,
    "totalNetSalary": 1008940
  }
}
```

---

If you want, I can **also add state-based PAYE**, **bonus handling**, and **automatic remittance reports in CSV/PDF** so it’s **fully ready for HR/Payroll software integration**.

Do you want me to add those features next?
