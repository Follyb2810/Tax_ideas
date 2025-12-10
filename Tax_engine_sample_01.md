Yes! With this tax engine, you (or your employer) can **calculate your net take-home pay** from your gross salary (₦100,000 in your example) **including all statutory deductions** like:

1. **Pension contribution (8%)** → goes to your retirement fund
2. **PAYE (Pay As You Earn)** → income tax
3. **NHF (2.5%)** → optional housing fund
4. **NHIS/HMO (5%)** → health insurance contribution
5. **CRA (Consolidated Relief Allowance)** → reduces taxable income

---

### **Example Calculation: ₦100,000 Gross Salary**

Let’s walk through it using **the rules in our engine**:

1. **Gross Salary:** ₦100,000

2. **Pension (8%):** 100,000 × 0.08 = ₦8,000

   - Salary after pension = 100,000 – 8,000 = ₦92,000

3. **CRA (Relief Allowance):**

   - CRA1 = max(200,000, 1% of 100,000) = ₦200,000 (fixed)
   - CRA2 = 20% × 92,000 = ₦18,400
   - Total CRA = 200,000 + 18,400 = ₦218,400

4. **Taxable Income:** 100,000 – 8,000 – 218,400 = negative → **0**

   - Since taxable income ≤ 0, **no PAYE is charged**.

5. **Other deductions:**

   - NHF = 100,000 × 0.025 = ₦2,500
   - NHIS/HMO = 100,000 × 0.05 = ₦5,000

6. **Net Salary (Take-home pay):**

   ```
   100,000 – (8,000 + 2,500 + 5,000) = 84,500
   ```

✅ So, your **employer should pay you ₦84,500** as take-home salary.

---

If you want, I can make a **tiny calculator snippet in Node.js** where you just input your **gross salary** and it immediately outputs **net salary + deductions breakdown**, so you can check any number quickly.

Do you want me to make that?
