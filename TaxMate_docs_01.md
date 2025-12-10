### TaxMate NG – Deep Dive (Everything You Need to Confidently Talk About It)

#### 1. WHAT is TaxMate NG? (The Product in One Sentence)

TaxMate NG is a mobile app that automatically calculates, files, and pays the correct Personal Income Tax (PIT) for freelancers, remote workers, content creators, crypto traders, Bolt/Uber drivers, and anyone earning side income in Nigeria — so they never get fined or harassed by the Nigeria Revenue Service (NRS) starting January 2026.

Think of it as “QuickBooks Self-Employed + TurboTax” but built only for Nigeria’s 2026 tax rules and for people who earn money outside a normal salary.

#### 2. WHY does this product NEED to exist in 2026? (The Real Pain)

From January 1, 2026, these new rules hit millions of Nigerians for the first time:

| New 2026 Rule                                                               | What it means in real life                                      | How it scares people                                |
| --------------------------------------------------------------------------- | --------------------------------------------------------------- | --------------------------------------------------- |
| First ₦800k per year is tax-free, but everything above is taxed 15–25%      | If you earn $1,000/month on Upwork (≈₦1.6m), you now owe tax    | Most freelancers have never filed tax in their life |
| Foreign income (Upwork, Fiverr, YouTube, Binance) is now taxable in Nigeria | Platforms will start reporting your earnings to NRS             | People are panicking on Twitter right now           |
| Banks will auto-report any account receiving >₦5m/month                     | Lifestyle audit: “You declared ₦800k but spent ₦20m — explain!” | Fear of account freeze or jail                      |
| You must file quarterly or pay ₦50,000–₦500,000 fine + possible jail        | Deadlines: March, June, Sept, Dec every year                    | Nobody knows the deadlines                          |
| TIN + NIN + BVN must be linked, or you can’t bank                           | Many people still don’t have TIN                                | Queues at tax offices will be madness               |

Result: In Q1 2026, at least 3–5 million Nigerians will suddenly need to understand and pay tax correctly — most of them young, digital, and scared.

That’s your market.

#### 3. WHO exactly will pay you money?

Primary users (your paying customers):

1. Upwork/Fiverr Nigerian freelancers (50,000–150,000 people earning $500+/month)
2. YouTube/TikTok creators earning dollars
3. Crypto traders who made money in 2025 bull run
4. Bolt/Uber drivers doing ₦800k+ per year
5. Airbnb hosts, Instagram vendors, graphics designers, programmers with foreign clients
6. Nigerian remote workers hired by US/UK companies (the new “digital nomads”)

These people are already online, already have smartphones, and are willing to pay ₦5,000–₦15,000 per month to avoid a ₦500,000 fine or frozen account.

#### 4. HOW does TaxMate NG actually work? (Simple User Journey)

Step-by-step what the user experiences:

| Step | What the user does                                                                            | What the app does behind the scenes                                             |
| ---- | --------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------- |
| 1    | Downloads app & signs up with phone number                                                    | Auto-pulls NIN → gets or creates TIN                                            |
| 2    | Links bank accounts (Moniepoint, Kuda, Opay, GTB, etc.) OR connects Upwork/PayPal/Stripe      | Securely reads inflows using Open Banking (Paystack/Flutterwave)                |
| 3    | Every month the app says: “You earned ₦2.3m this quarter. After exemptions, you owe ₦187,000” | Applies exact 2026 tax bands + deductions                                       |
| 4    | User taps “File & Pay”                                                                        | Generates official NRS form, user pays via card/USSD, app submits to NRS portal |
| 5    | Done! App saves proof + sends reminder 3 months later                                         | User sleeps peacefully                                                          |

Extra magic features that make people love it:

- “Audit Shield” button → instantly download a PDF defending your numbers if NRS calls
- Crypto wallet connect → auto-calculate gains (anything under ₦800k total is tax-free)
- “Safe to Spend” meter → shows how much you can withdraw without triggering lifestyle audit
- Pidgin English mode → “Oga, you never file tax o, deadline na next week!”

#### 5. HOW you make money (Pricing that users happily pay)

| Plan    | Price                        | Who buys it                  | What they get                                                      |
| ------- | ---------------------------- | ---------------------------- | ------------------------------------------------------------------ |
| Free    | ₦0                           | People earning <₦5m/year     | Basic calculator + reminders                                       |
| Pro     | ₦4,900/month or ₦49,000/year | Most freelancers             | Auto-filing, bank connect, audit defense                           |
| Premium | ₦9,900/month                 | High earners, crypto traders | Multi-bank, foreign currency, priority support, accountant hotline |

Average revenue per paying user ≈ ₦60,000–₦100,000 per year.

If you get just 5,000 paying users in year 1 → ₦300m–₦500m revenue.

#### 6. Why YOU (a software developer) can build this fast

MVP (Minimum Viable Product) you can launch by February 2026:

1. User auth + TIN lookup
2. Manual income entry + tax calculator (2026 bands hardcoded)
3. Generate PDF tax form + Remita payment link
4. Push reminders

That’s it for version 1.0 — already solves 80% of the pain.

Later add bank integrations, Upwork API, etc.

Tech stack a single Nigerian dev can handle:

- Flutter or React Native (one codebase for Android + iOS)
- Firebase or Supabase (free tier to start)
- Paystack/Flutterwave for payments
- Simple Node.js backend on Render or Railway

#### 7. One-liner you can use when pitching or posting on X

“TaxMate NG is the app that files your 2026 tax for you in one tap — so Upwork money no go put you for tax wahala.”

Now you fully understand TaxMate NG inside out.

Ready for the next one? Just say “Next” and I’ll do the same deep breakdown for ZeroVAT (#2). Or tell me if you want the full MVP roadmap + wireframes for TaxMate NG first.
