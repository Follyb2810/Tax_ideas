Exactly — this is a key point! Let’s break it down **step by step** so it’s crystal clear how ZeroVAT knows **which items require VAT and which don’t**, and how the system tracks what happens with the invoices.

---

# 🟦 **ZeroVAT – Determining VAT & Tracking Invoices**

---

## **1️⃣ How the app knows which items are taxable**

### **Step 1: Define VAT Rules in the App**

When a business registers:

- App sets **default VAT rules** based on business type:

  - **Standard items:** 7.5% VAT
  - **Zero-rated items:** 0% VAT (e.g., certain foods, exports)
  - **Exempt items:** 0% VAT, cannot reclaim input VAT

- Admin/owner can **customize rules** per item category.

**Example:**

| Item Type             | VAT Rate | Notes        |
| --------------------- | -------- | ------------ |
| Electronics           | 7.5%     | Standard VAT |
| Local Food            | 0%       | Zero-rated   |
| Educational Materials | 0%       | Exempt       |

---

### **Step 2: Assign VAT When Creating Invoice**

- When user adds items, the app **matches the item to a VAT category**:

  - If item is in “standard” → 7.5%
  - If in “zero-rated” → 0%

- Backend calculates **VAT amount = unit_price × quantity × VAT rate**

---

### **Step 3: Validate VAT Eligibility**

- Optional: Use a **centralized item database** (pre-approved by NRS)
- Or let users **tag their items** (app remembers for next invoice)
- Ensures **consistency for audits**

---

## **2️⃣ How we track what happens to invoices**

ZeroVAT needs **full tracking for reporting and compliance**.

### **Invoice Lifecycle**

1. **Created** – invoice is generated in the app
2. **Saved / Sent** – invoice stored in cloud; optionally sent to customer
3. **VAT Calculated** – app calculates output VAT
4. **Included in VAT Report** – end of month, invoice data is aggregated to show total VAT payable or reclaimable
5. **Verified / Audited** – QR code allows customers or NRS to verify authenticity

### **Technical Tracking**

- Each invoice stored in database:

  ```
  invoices table:
  id | business_id | invoice_number | total_amount | total_vat | status (draft/sent/paid) | date
  ```

- Each invoice item stored in **invoice_items table** with VAT applied
- Monthly VAT report queries invoices by **month + business_id**

---

## **3️⃣ How app decides if VAT applies**

- VAT rules = combination of:

  1. **Business type default rules**
  2. **Item category** (standard, zero-rated, exempt)
  3. Optional **manual override by user** if special case

- Then the system automatically calculates:

  - **Output VAT** (from sales)
  - **Input VAT** (from purchases if user enters them)
  - **Net VAT payable/reclaimable**

---

## **4️⃣ Optional Smart Features**

- **Auto-suggest VAT rate** based on item description (using keywords or AI)
- **Flag suspicious invoices** if VAT seems missing or inconsistent
- **Audit trail**: store who created invoice + timestamp + item VAT → useful for NRS audit

---

✅ **Summary**

1. App **knows VAT rates** because it’s pre-configured by business type and item category.
2. Each invoice **automatically calculates VAT** for each line item.
3. App tracks every invoice in a **database** for monthly VAT reports.
4. Optional verification via **QR code** ensures authenticity.
5. The business sees exactly how much VAT they owe or can reclaim — no guesswork.

---

If you want, I can **draw a “VAT decision flow diagram”** showing **how the app decides whether to apply VAT to each item** — it makes this super clear for developers and investors.

Do you want me to do that next?
