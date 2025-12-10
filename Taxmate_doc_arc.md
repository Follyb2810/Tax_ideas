### TaxMate NG MVP Roadmap: From Idea to Launch by February 2026

As a solo software developer in Nigeria, building TaxMate NG (the personal tax assistant app for freelancers and side-hustlers under the 2026 tax reforms) is totally doable with an MVP approach. MVP means focusing on the core features that solve 80% of the user's pain (e.g., calculating and filing PIT without fines) while skipping nice-to-haves like crypto integrations until later.

This roadmap is realistic for you: 8-10 weeks total, assuming 20-30 hours/week. Current date is Dec 09, 2025, so we target a beta launch by mid-Jan 2026 (right when reforms hit) and full launch by Feb. Budget: Under ₦500k if you use free tools (e.g., free tiers of Firebase). I'll break it down into phases, with actionable steps, timelines, and tips.

#### Phase 1: Planning & Research (Week 1: Dec 09-15, 2025)

**Goal**: Nail down exactly what the MVP is, validate demand, and gather resources. Spend time here to avoid mid-build pivots.

- **Define MVP Scope** (What to Build First):

  - Core Problem Solved: Automate PIT calculation and basic filing for users earning >₦800k/year from freelance/side income.
  - Must-Have Features (Keep it to 4-5):
    1. User Signup: Phone/email + auto-TIN check (via NRS/JTB API if available; fallback to manual entry).
    2. Income Tracker: Manual entry of monthly income (e.g., "Upwork: ₦1.2m") + categorize (freelance, crypto, etc.).
    3. Tax Calculator: Input income → output owed PIT using 2026 bands (0% on first ₦800k, etc.) + exemptions (e.g., no tax on remittances).
    4. Filing Assistant: Generate downloadable PDF form + payment instructions (integrate Remita for one-tap pay later).
    5. Reminders: Push notifications/SMS for quarterly deadlines.
  - Out of Scope for MVP: Bank auto-links, audit defense, multi-currency, Pidgin mode (add in v1.1).
  - User Flow: Signup → Add income → See tax estimate → Generate form → Pay/file.

- **Validate Demand**:

  - Post on X (Twitter), LinkedIn, and Nigerian freelancer groups (e.g., "Freelance Nigeria" on Facebook) with a teaser: "Building an app to handle your 2026 tax automatically—DM if interested."
  - Aim for 50-100 signups on a Google Form waitlist. Ask: "What scares you most about the new taxes?" to refine features.
  - Research Competitors: Check apps like Taxaide or FIRS app—yours wins by being freelancer-focused and simple.

- **Legal/Compliance Check**:

  - Read NRS guidelines on their site (nrs.gov.ng) for API access (e.g., e-filing endpoints).
  - Ensure data privacy: Use GDPR-like notices; no storing bank details in MVP.

- **Resources Needed**:
  - Tools: Free VS Code, GitHub for version control.
  - Learning: If new to mobile dev, watch 2-3 YouTube tutorials on Flutter (5 hours total).
  - Budget: ₦50k for domain (taxmateng.com) + Google Play/App Store dev accounts.

**Milestone**: By Dec 15, have a 1-page doc with wireframes (sketch on paper or Figma free tier) and 20+ waitlist signups.

#### Phase 2: Design & Setup (Week 2: Dec 16-22, 2025)

**Goal**: Create simple designs and set up your dev environment. Keep UI clean—think WhatsApp simple.

- **UI/UX Design**:

  - Screens: Welcome, Signup, Dashboard (income summary), Calculator, Filing Page, Settings.
  - Tools: Use Figma or Canva for mockups. Colors: Green/blue for trust (like banking apps).
  - Mobile-First: 90% of users on Android in Nigeria—design for low-data (offline mode for calculations).

- **Tech Stack Setup**:

  - Frontend: Flutter (one code for Android/iOS; fast for solo dev).
  - Backend: Firebase (free for auth, database, notifications—handles user data securely).
  - Database: Firestore for storing user incomes (encrypted).
  - Integrations: Remita API for payments (sign up free); NRS API if public by then.
  - Version Control: GitHub repo—commit daily.

- **Prototype**:
  - Build a clickable prototype in Figma to test with 5 friends/freelancers. Feedback: "Is the calculator intuitive?"

**Milestone**: Working dev environment + basic wireframes. Test signup flow locally.

#### Phase 3: Development (Weeks 3-5: Dec 23, 2025 - Jan 12, 2026)

**Goal**: Code the MVP. Break into sprints—focus on one feature per day.

- **Sprint 1 (Dec 23-29)**: Auth & Onboarding.

  - Code signup with phone OTP (Firebase Auth).
  - TIN integration: Simple form to input TIN; validate format.

- **Sprint 2 (Dec 30-Jan 5)**: Core Calculator.

  - Build income entry form.
  - Hardcode 2026 tax logic in Dart (e.g., if income <= 800000: tax=0; else tax=0.15\*(income-800000)).
  - Add basic deductions UI.

- **Sprint 3 (Jan 6-12)**: Filing & Reminders.

  - Generate PDF (use pdf package in Flutter).
  - Set up Firebase Cloud Messaging for notifications.
  - Basic dashboard to show "Owed: ₦X" and "Next Deadline: March 31".

- **Daily Routine**: Code 4-6 hours/day. Test on your phone emulator. Fix bugs immediately.

- **Security Focus**: Use HTTPS, encrypt data at rest. No storing sensitive info like BVN in MVP.

**Milestone**: Fully functional app on your device. Share APK with 10 beta testers for feedback.

#### Phase 4: Testing & Iteration (Weeks 6-7: Jan 13-26, 2026)

**Goal**: Squash bugs and ensure it works under real conditions.

- **Internal Testing**:

  - Test edge cases: ₦0 income (no tax), ₦50m+ (max rate), exemptions.
  - Simulate reforms: Use sample data from NRS site.

- **Beta Testing**:

  - Release to waitlist (use TestFlight for iOS, Google Play Beta for Android).
  - Collect feedback via Google Forms: "Did it calculate correctly? Any crashes?"
  - Iterate: Fix top 3 issues (e.g., add better error messages).

- **Performance**:
  - Ensure app loads fast on 3G (common in Nigeria).
  - Analytics: Add Firebase Analytics to track usage.

**Milestone**: Stable beta with 50+ testers. 90% satisfaction in feedback.

#### Phase 5: Launch & Go-to-Market (Weeks 8-10: Jan 27-Feb 9, 2026)

**Goal**: Get users and start monetizing. Launch mid-reforms for buzz.

- **Launch Prep**:

  - Submit to App Stores (Google Play: 1-2 days approval; Apple: 1 week).
  - Pricing: Free download; in-app upgrade to Pro (use RevenueCat for subscriptions).

- **Marketing Plan**:

  - **Free Channels**: Post on X with #NigeriaTax2026 (e.g., "Beat the fines—try TaxMate NG free!"). Target freelancer communities.
  - **Paid**: ₦100k on Facebook Ads targeting "Upwork Nigeria" keywords.
  - **Partnerships**: Email Upwork influencers or YouTube creators for shoutouts.
  - **Virality**: Add "Share with friends" for referral discounts.

- **Monetization Rollout**:

  - Start with free tier to get 1k users.
  - Upsell Pro via in-app prompts (e.g., "Unlock auto-filing for ₦4,900/mo").

- **Post-Launch**:
  - Monitor: Use Firebase Crashlytics for bugs.
  - Iterate: Based on reviews, add v1.1 features like bank links by March.

**Milestone**: 500 downloads in first week. Track metrics: Retention (30% at day 7), Conversion to paid (5-10%).

#### Overall Risks & Tips for Success

- **Risks**: NRS APIs delayed? Fallback to manual. Low adoption? Pivot marketing to fear of fines.
- **Budget Breakdown**: ₦100k marketing, ₦50k tools/domains, ₦100k contingencies.
- **Scaling**: If it blows up, hire a part-time dev via Upwork.
- **Why This Wins**: Timely (reforms create urgency), solves real pain, easy for you to build.

This roadmap gets you live before the first quarterly deadline (March 2026). If you follow it, you'll have a revenue-generating app fast. Questions on any phase, or need code snippets for the tax calculator?
