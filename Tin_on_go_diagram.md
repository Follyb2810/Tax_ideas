# TIN-on-the-Go: System Flow Diagram

Below is a clear, simple diagram showing how the system flows from the user to the backend and back.

```
                    ┌────────────────────────┐
                    │       Mobile App       │
                    │ (User / Agent / Verify)│
                    └────────────┬───────────┘
                                 │
                                 ▼
                     ┌───────────────────────┐
                     │     API Gateway       │
                     └────────────┬──────────┘
                                  │
     ┌─────────────────────────────┼───────────────────────────────┐
     ▼                             ▼                               ▼
┌─────────────┐          ┌────────────────┐                ┌─────────────────┐
│ Auth Service│          │ User Service   │                │ TIN Service     │
│ (OTP Login) │          │ (Profiles)     │                │ (TIN App, Cert) │
└──────┬──────┘          └───────┬────────┘                └────────┬────────┘
       │                          │                                 │
       │                          │                                 │
       ▼                          ▼                                 ▼
┌─────────────┐         ┌─────────────────┐               ┌──────────────────┐
│  OTP Store  │         │  User Database  │               │  Gov. API Bridge │
└─────────────┘         └─────────────────┘               └─────────┬────────┘
                                                                      │
                                                                      ▼
                                                          ┌─────────────────────┐
                                                          │ Gov. TIN API System │
                                                          └─────────┬───────────┘
                                                                    │
                                                                    ▼
                                                         ┌────────────────────────┐
                                                         │ Certificate Generator  │
                                                         │     (PDF Service)     │
                                                         └──────────┬────────────┘
                                                                    │
                                                                    ▼
                                                         ┌────────────────────────┐
                                                         │   File Storage (S3)    │
                                                         └──────────┬────────────┘
                                                                    │
                                                                    ▼
                                                         ┌────────────────────────┐
                                                         │  Mobile App Downloads  │
                                                         └────────────────────────┘
```

## Description of Flow

1. **User/Agent opens the mobile app** and sends request through the **API Gateway**.
2. The system routes requests to:

   - **Auth Service** for OTP login.
   - **User Service** for profile and identity data.
   - **TIN Service** for registration, certificate, and verification.

3. **TIN Service** communicates with the government TIN API.
4. Government response returns the TIN.
5. Certificate is generated and stored in **file storage (S3/Spaces)**.
6. User downloads the certificate from the mobile app.
7. Verifiers use the verification endpoint to check TIN.

Let me know if you want this diagram in:

- A more visual **flowchart style**
- A **sequence diagram**
- A **mermaid.js diagram** you can copy into documentation
- Or a **PDF export**
