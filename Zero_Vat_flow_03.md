Perfect! Let’s create a **VAT decision flow diagram** for ZeroVAT showing **how the app decides whether VAT applies to each item**. I’ll make it super clear, step by step.

---

# 🟦 **ZeroVAT – VAT Decision Flow Diagram**

```mermaid
flowchart TD
    A[Add Item to Invoice] --> B{Is item in predefined VAT category?}

    B -- Yes --> C{Category Type?}
    B -- No --> D[Prompt user to select VAT type manually]

    C -- Standard → E[Apply 7.5% VAT]
    C -- Zero-rated → F[Apply 0% VAT]
    C -- Exempt → G[Apply 0% VAT, cannot reclaim input VAT]

    D --> H[User selects: Standard / Zero-rated / Exempt]
    H --> I[Apply selected VAT rate]

    E --> J[Calculate VAT Amount = Qty × Unit Price × VAT Rate]
    F --> J
    G --> J
    I --> J

    J --> K[Add line item to invoice with VAT amount]
    K --> L[Update Invoice Total & Output VAT]
    L --> M[Invoice ready for PDF, cloud storage, and QR code]
```

---

### **Explanation of Flow**

1. **Step 1: Item Added**

   - User enters a product/service on the invoice.

2. **Step 2: Check VAT Category**

   - App first checks if the item is already in the **predefined VAT database** (by category).
   - If yes → go to Step 3
   - If no → ask the user to select the VAT type manually.

3. **Step 3: Apply VAT Rate**

   - Standard → 7.5% VAT
   - Zero-rated → 0% VAT (eligible for audits)
   - Exempt → 0% VAT (cannot reclaim input VAT)

4. **Step 4: Calculate VAT Amount**

   - `VAT = Quantity × Unit Price × VAT Rate`
   - System stores VAT per line item in the database.

5. **Step 5: Update Invoice**

   - Add line item to invoice with calculated VAT.
   - Update invoice total & monthly VAT summary.

6. **Step 6: Ready for Export**

   - Invoice can now be **saved, exported, or sent** to customer with QR code.

---

✅ **Key Points**

- **Automation:** Most items are auto-classified by category → minimal manual input.
- **Flexibility:** User can override VAT type if the item is unusual.
- **Audit-ready:** Each line item stores **VAT rate + calculated VAT**, ensuring compliance.
- **Monthly reporting:** Backend aggregates VAT from invoices automatically for filing.

---

If you want, I can next **create a visual “full invoice + VAT calculation flow”** showing **from user adding an item → VAT calculation → invoice ready → monthly VAT report**. It would combine **user journey + VAT decision + reporting** in one diagram.

Do you want me to do that next?
