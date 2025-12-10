# 🟦 **PART 1 — USER JOURNEY (How users will use TaxMate NG)**

This is written from the perspective of a normal user.

---

## **1. Onboarding**

### **Flow**

1. User downloads the app (or visits web app).
2. They sign up using:

   - Email
   - Phone number
   - Google or Apple login

### **User Goal**

“To quickly know how much tax I’m supposed to pay.”

---

## **2. Income Setup**

### **Flow**

1. User selects their income type(s):

   - Freelancer
   - Remote worker
   - Small business
   - Gig worker (Bolt/Uber rider, etc.)

2. User enters:

   - Monthly income
   - Extra side income (optional)
   - Foreign income (optional)

3. The app shows:

   - **Estimated tax for the month**
   - **Estimated total annual tax**
   - **Due date**

### **User Goal**

“Make it easy and clear so I don’t need to be an accountant.”

---

## **3. Monthly Tracking**

### **Flow**

- User gets reminders:

  - 1 week before PIT deadline
  - 24 hours before
  - Same-day reminder

- User opens the app and updates income if needed.
- App recalculates tax in real time.

### **User Goal**

“Don’t let me miss deadlines.”

---

## **4. Filing Support**

### **Flow**

1. User taps **‘File Now’**.
2. App generates:

   - Tax summary sheet
   - Breakdown of income
   - Breakdown of allowable deductions

3. App asks user to upload:

   - Payment receipts to tax authority
   - Any bank statements if needed

4. App stores everything in their dashboard.

### **User Goal**

“Help me file properly so I don’t get fined.”

---

## **5. Audit Defense**

### **Flow**

If the NRS flags their bank account or requests info:

- User taps **‘Audit Mode’**
- App instantly generates:

  - Income summary
  - Proof of declared income
  - All tax receipts
  - Exportable PDF pack

### **User Goal**

“Give me documents to defend myself immediately.”

---

## **6. Subscription & Payment**

### **Flow**

Users choose:

- Free plan (basic calculator)
- ₦2,500 – ₦15,000/month for filing + audit defense

### **User Goal**

“Affordable and easy.”

---

# 🟦 **PART 2 — DEVELOPER IMPLEMENTATION (How to build TaxMate NG)**

This is now technical — your real development guide.

---

# 🧩 **System Architecture Overview**

### **Frontend**

- Flutter (mobile-first)
- React (web dashboard)
- Clean UI, offline support

### **Backend (API)**

- Node.js (Express or NestJS) OR Go (faster)
- REST + optional GraphQL layer

### **Database**

- PostgreSQL (main)
- Redis (caching)
- S3/GCP for file storage (receipts, PDF reports)

### **Authentication**

- JWT auth
- OAuth (Google, Apple)

---

# 🧩 **Core Backend Modules**

### **1. User Module**

- Register/login
- Email/phone verification
- Profile setup
- Store user preference (income type, reminders)

### **2. Income Module**

- Add income streams
- Monthly income data
- Currency conversion for foreign income
- Validation rules (e.g., no negative values)

### **3. Tax Engine Module (the brain)**

- Computes PIT using 2026 tax rules:

  - Consolidated relief
  - Allowable deductions
  - Progressive tax bands

- Auto-calc monthly + annual tax
- Auto-adjust when income changes

### **4. Filing Module**

- Generates “Filing Pack” PDF:

  - Income summary
  - Declared tax
  - Payable calculations

- Upload payment receipts
- Mark filing as completed
- API endpoints for downloading documents

### **5. Audit Defense Module**

- Auto-generate PDF with:

  - All receipts
  - All income
  - All tax calculations

- One-click export or email

### **6. Notification Module**

- Cron jobs for monthly reminders
- Push notifications
- Email reminders
- SMS fallback (optional)

### **7. Subscription Module**

- Paystack or Flutterwave
- Recurring billing
- Trial periods
- Webhooks for payment verification

---

# 🧩 **API Endpoints (Core)**

### **Auth**

- `POST /auth/register`
- `POST /auth/login`
- `POST /auth/verify`

### **Income**

- `POST /income/add`
- `GET /income/monthly/:month`
- `PUT /income/update/:id`

### **Tax Engine**

- `GET /tax/calc?month=JAN`
- `GET /tax/annual`
- `POST /tax/compute`

### **Filing**

- `POST /filing/create`
- `POST /filing/upload-receipt`
- `GET /filing/pdf/:id`

### **Audit**

- `GET /audit/pdf`
- `POST /audit/send-to-email`

### **Subscriptions**

- `POST /subscription/initialize`
- `POST /subscription/webhook`

---

# 🧩 **Database Schema (Simplified)**

### **Users**

- id
- name
- email
- phone
- password_hash
- subscription_status
- created_at

### **Income**

- id
- user_id
- amount
- category (freelance, remote, gig)
- month
- description
- created_at

### **Tax Records**

- id
- user_id
- month
- taxable_income
- tax_due
- reliefs
- deductions
- created_at

### **Filing**

- id
- user_id
- period
- pdf_url
- receipts_url[]
- status
- created_at

---

# 🧩 **Admin Dashboard (Optional)**

Admin can:

- Manage users
- Handle disputes
- Update tax rules
- Generate global reports
- Manage chatbot FAQ
- View error logs

---

# 🟦 **PART 3 — DOCUMENTATION EXPLAINS BOTH SIDES**

You now have:

### ✔ User Flow Document

### ✔ Developer Architecture Document

### ✔ API Overview

### ✔ Database Structure

If you want, I can create:

### **📘 Full Product Requirements Document (PRD)**

### **📗 Full System Architecture Document (with diagrams)**

### **📙 API Documentation (Swagger spec)**

### **📕 UX Wireframes**

### **📘 30-Day Development Plan**

Just tell me **which document to generate next**.
