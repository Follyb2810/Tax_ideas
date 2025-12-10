Perfect! Let’s create a **visual user flow for ZeroVAT** showing **step-by-step how a user interacts with the app** and how the system handles it. I’ll make it easy to follow for **developers, designers, and investors**.

---

# 🟦 **ZeroVAT – User Flow Diagram (Step-by-Step)**

```mermaid
flowchart TD
    A[User Opens App] --> B{New or Existing User?}
    B -- New --> C[Register Account + Business Info]
    B -- Existing --> D[Login]

    C --> E[Validate TIN with NRS API]
    E --> F[Onboarding Complete → Show Dashboard]
    D --> F

    F --> G[Dashboard]
    G --> H[Create Invoice]
    G --> I[View Invoice List]
    G --> J[View VAT Report]
    G --> K[Settings / Business Info]

    H --> H1[Add Customer (optional)]
    H --> H2[Add Items & Prices]
    H --> H3[Auto-calc VAT]
    H --> H4[Generate Invoice Number & QR Code]
    H --> H5[Save Invoice to Cloud + PDF Export]
    H5 --> G

    I --> I1[Search / Filter Invoices]
    I1 --> I2[View / Download Invoice PDF]
    I2 --> G

    J --> J1[Select Month/Year]
    J1 --> J2[Calculate Output VAT & Input VAT]
    J2 --> J3[Show Net VAT Payable/Reclaimable]
    J3 --> J4[Option to Export CSV/PDF or Send to Accountant]
    J4 --> G

    K --> K1[Update Business Info / VAT Rates / TIN]
    K1 --> G

    %% Optional Notifications / Reminders
    F --> L[Monthly VAT Reminder Notification]
    L --> G
```

---

### **Explanation of Flow**

1. **Start:** User opens the app → decides whether to register or login.
2. **Registration:** User enters business info → app validates TIN with NRS API → user sees dashboard.
3. **Dashboard:** Central hub; user can:

   - Create invoices
   - Check VAT report
   - View past invoices
   - Update settings

4. **Invoice Creation:** One-tap creation → auto-VAT calculation → generate invoice → save & export.
5. **VAT Reports:** Calculates output VAT, input VAT, and net payable automatically → exportable for filing.
6. **Notifications:** Monthly reminders keep the user compliant.
7. **Settings:** Allows updates to business info, VAT rules, or TIN.

---

This flow ensures **user simplicity + system compliance**. From a developer perspective, it’s **modular**, meaning:

- Dashboard → central hub
- Invoice creation → separate module
- VAT reports → separate module
- NRS API interactions → separate service

---

If you want, I can also **turn this flow into a “technical architecture diagram”** showing **frontend → backend → database → NRS API → cloud storage**, so you have a full blueprint for building ZeroVAT.

Do you want me to do that next?
