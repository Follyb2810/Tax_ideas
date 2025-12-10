Great question — and **yes**, the system must support multiple income sources because most freelancers, remote workers, and hustlers earn from more than one stream.

Below is the **user experience** + **technical implementation** for handling multiple incomes.

---

# 🟦 **USER EXPERIENCE — When the User Has More Than One Income**

### **1. Income Setup Screen**

After onboarding, user sees:

> **“Add all your income sources for accurate tax calculation.”**

They can tap **+ Add Income Source**.

---

### **2. Each Income Source Has:**

- Income name
  _e.g., Upwork, Fiverr, Salary, Shop Sales, TikTok creator payout_
- Frequency
  _monthly, weekly, daily, one-off_
- Currency
  _USD, NGN, GBP…_
- Category
  _Freelance, Remote job, Gig work, Small business, Investment income_
- Amount

---

### **3. Dashboard Shows Combined Income**

Example:

| Source       | Amount   | Type           |
| ------------ | -------- | -------------- |
| Upwork       | $450     | freelance      |
| Fiverr       | $150     | freelance      |
| Remote Job   | ₦280,000 | salary         |
| POS Profit   | ₦42,000  | daily business |
| TikTok Gifts | $20      | creator income |

The app automatically converts to NGN and calculates **total monthly income**.

---

### **4. Tax Is Calculated from Total Income**

All incomes → combined → deductible relief applied → PIT computed.

User sees:

> **Your total monthly income: ₦580,400
> Your estimated tax for this month: ₦27,100**

---

### **5. Filing Pack Includes All Incomes**

When user clicks **File Tax Now**, the PDF includes:

- List of all income streams
- Total taxable income
- Calculation sheet
- Final tax due

---

# 🟦 **DEVELOPER IMPLEMENTATION — Multiple Income Support**

Below is how you implement it.

---

## 🧩 **Backend Structure**

### **Income Table Structure**

Each row = 1 income source.

```
Income
---------
id
user_id
source_name (text)
amount (float)
currency (text)
category (enum)
frequency (enum)
month (date or month)
created_at
```

So a user can have:

- 5 income rows in January
- 3 in February
- etc.

---

## 🧩 **How the Tax Engine Works With Multiple Incomes**

### **Step 1 — Fetch all incomes for the month**

```
SELECT * FROM income WHERE user_id = X AND month = '2026-03';
```

### **Step 2 — Convert all to NGN**

(Use exchange rate microservice or static daily rate.)

### **Step 3 — Sum all incomes**

```
totalIncome = sum(all incomes)
```

### **Step 4 — Apply tax rules**

- Consolidated relief
- Allowable deductions
- Progressive PIT brackets

### **Step 5 — Return final tax values**

JSON example:

```json
{
  "total_income": 580400,
  "taxable_income": 450000,
  "tax_due": 27100,
  "income_breakdown": [
    { "source": "Upwork", "amount": 350000 },
    { "source": "Remote Job", "amount": 280000 },
    { "source": "POS", "amount": 42000 }
  ]
}
```

---

## 🧩 **Frontend UX Implementation (Flutter Example)**

### **Add Income Source Button**

User taps → opens modal:

```
TextField: Income name
Dropdown: Category
Dropdown: Frequency
Textfield: Amount
Dropdown: Currency
Save
```

### **Income List**

Cards showing each income item with:

- Name
- Amount
- Frequency
- Edit/Delete buttons

### **“Total Income” UI**

At the top:

> **Total income this month: ₦580,400** > **Estimated tax: ₦27,100**

---

## 🧩 **API Endpoints Needed**

### Add income

`POST /income/add`

Body:

```json
{
  "source_name": "Upwork",
  "amount": 450,
  "currency": "USD",
  "frequency": "monthly",
  "category": "freelance",
  "month": "2026-03"
}
```

---

### Get all incomes for a month

`GET /income/list?month=2026-03`

---

### Calculate tax using all incomes

`GET /tax/calc?month=2026-03`

Backend automatically sums all incomes.

---

# 🟦 **Summary**

### ✔ Users can add unlimited income sources

### ✔ App computes tax from combined income

### ✔ Developer stores incomes as separate rows

### ✔ Tax engine aggregates them for calculations

### ✔ Filing pack includes all income sources

---

If you want, I can now produce:

✅ **User Flow Diagram**
✅ **Database ER Diagram**
✅ **API Spec (Swagger format)**
✅ **Tax Engine Algorithm (full logic)**
Just tell me which one you want next.
