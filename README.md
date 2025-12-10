# Mini-Bank E2E – Day 27 Capstone

---

## Overview

Day 27 focuses on building a clear, beginner-friendly path to end-to-end testing of the Mini-Bank app. The goal is to run, understand, and begin automating a curated set of 26 tests across core flows (Home, Login, Dashboard, Deposit, Withdrawal, Transfer, Bills, Transactions, Logout), while learning accessibility-first locators and stable test design. Day 28 adds Allure and CI/CD enhancements.

---

## Prerequisites & Setup

- Node.js LTS installed
- Install dependencies and Playwright browsers

```
npm ci
npx playwright install --with-deps
```

- Base URL: `https://mini-bank.testamplify.com/`
- Use a test account provided by the instructor, or create one via the app’s **Create Account** flow before running authenticated tests.

---

## Quick Start

- Run locally: `npx playwright test`
- Debug a failing test: `npx playwright test --debug`
- Optional HTML report: `npx playwright show-report`

---

# Summary Table (Top Section)

| ID               | Title                                      | Page / URL                |
| ---------------- | ------------------------------------------ | ------------------------- |
| **TC-HOME-01**   | Home loads with key landmarks              | `/`                       |
| **TC-HOME-02**   | Login CTA navigates correctly              | `/`                       |
| **TC-HOME-03**   | Footer & Copyright visible                 | `/`                       |
| **TC-LOGIN-01**  | Happy path login                           | `/login`                  |
| **TC-LOGIN-02**  | Invalid user shows error                   | `/login`                  |
| **TC-LOGIN-03**  | Required field validation                  | `/login`                  |
| **TC-DASH-01**   | Account balance visible after login        | `/dashboard`              |
| **TC-DASH-02**   | Start transaction → Deposit                | `/dashboard`              |
| **TC-DASH-03**   | Start transaction → Withdrawal             | `/dashboard`              |
| **TC-DEP-01**    | Deposit rejects 0 amount                   | `/dashboard/deposit`      |
| **TC-DEP-02**    | Deposit rejects negative amount            | `/dashboard/deposit`      |
| **TC-DEP-03**    | Deposit accepts 50,000 (valid)             | `/dashboard/deposit`      |
| **TC-WITH-01**   | Withdrawal rejects 0                       | `/dashboard/withdraw`     |
| **TC-WITH-02**   | Withdrawal rejects negative                | `/dashboard/withdraw`     |
| **TC-WITH-03**   | Withdrawal within available balance        | `/dashboard/withdraw`     |
| **TC-TR-01**     | Transfer: same-bank, sufficient funds      | `/dashboard/transfer`     |
| **TC-TR-02**     | Transfer: same-bank, insufficient funds    | `/dashboard/transfer`     |
| **TC-TR-03**     | Transfer: external bank, sufficient funds  | `/dashboard/transfer`     |
| **TC-BILL-01**   | Pay biller successfully                    | `/dashboard/bills`        |
| **TC-BILL-02**   | Reject invalid bill amount                 | `/dashboard/bills`        |
| **TC-BILL-03**   | Missing required bill fields               | `/dashboard/bills`        |
| **TC-TRX-01**    | Transaction history shows deposit          | `/dashboard/transactions` |
| **TC-TRX-02**    | Transaction history shows withdrawal       | `/dashboard/transactions` |
| **TC-TRX-03**    | Transaction history shows transfer & bills | `/dashboard/transactions` |
| **TC-LOGOUT-01** | Logout from dashboard                      | Header / Global           |
| **TC-LOGOUT-02** | Post-logout route protection               | Global                    |

---

## Total Test Cases Count

- Home: 3
- Login: 3
- Dashboard: 3
- Deposit: 3
- Withdrawal: 3
- Transfer: 3
- Bills: 3
- Transactions: 3
- Logout: 2

### TOTAL = 26 Test Cases

---

## Test Cases – Mini Bank E2E

---

### 1. Home — `https://mini-bank.testamplify.com/`

#### TC-HOME-01 – Home loads with key landmarks (3 assertions)

- **Page:** Home (`/`)
- **Steps:**

  1. Navigate to `https://mini-bank.testamplify.com/`.
  2. Assert the banner landmark (site header) is present.
  3. Assert the primary CTA **“Login”** button is visible and enabled.
  4. Assert the **“Create Account”** (or equivalent) link is visible.
- **Expected Results:**

  - Header/banner landmark is present and shows the app name/logo.
  - “Login” is rendered as a button (role=`button`) with accessible name **“Login”** and is enabled.
  - “Create Account” (or similar) link is visible and clickable.

---

#### TC-HOME-02 – Login CTA navigates correctly

- **Page:** Home (`/`)
- **Steps:**

  1. From the Home page, click the **Login** button (e.g. `getByRole('button', { name: 'Login' })`).
  2. Wait for the URL to change to `/login`.
- **Expected Results:**

  - Browser navigates to the Login page (`/login`).
  - Login form is rendered and visible.

---

#### TC-HOME-03 – Footer & legal (3 assertions)

- **Page:** Home (`/`)
- **Steps:**

  1. From the Home page, scroll down to the footer (landmark `contentinfo`).
  2. Assert the footer is visible.
  3. Assert copyright text is displayed.
- **Expected Results:**

  - Footer landmark is present and visible.
  - Copyright text displays the current year and site/app name.

---

### 2. Login — `https://mini-bank.testamplify.com/login`

#### TC-LOGIN-01 – Happy path login

* **Page:** Login (`/login`)
* **Precondition:** A valid user account exists.
* **Steps:**

  1. Navigate to `/login`.
  2. Enter a valid email in the email field.
  3. Enter the correct password in the password field.
  4. Click **Sign In**.
  5. Wait for redirect to `/dashboard`.
* **Expected Results:**

  * Authentication succeeds.
  * User is redirected to `/dashboard`.
  * Dashboard is displayed with a greeting and account summary (including balance).

---

#### TC-LOGIN-02 – Invalid user shows error

* **Page:** Login (`/login`)
* **Steps:**

  1. Navigate to `/login`.
  2. Enter an invalid email and/or incorrect password.
  3. Click **Sign In**.
* **Expected Results:**

  * An error alert (role=`alert`) is displayed with a meaningful message (e.g. “Invalid email or password”).
  * The URL remains `/login` (no navigation to dashboard).
  * No authenticated content is shown.

---

#### TC-LOGIN-03 – Required field validation

* **Page:** Login (`/login`)
* **Steps:**

  1. Navigate to `/login`.
  2. Leave the email field blank (or leave the password field blank).
  3. Click **Sign In**.
* **Expected Results:**

  * Inline validation is shown for the missing field(s), or the field has `aria-invalid="true"`.
  * Helper/error text is displayed explaining that the field is required.
  * User is not logged in and remains on `/login`.

---

### 3. Dashboard (Overview) — `https://mini-bank.testamplify.com/dashboard`

#### TC-DASH-01 – Account balance visible after login

* **Page:** Dashboard (`/dashboard`)
* **Precondition:** User is logged in (e.g. via TC-LOGIN-01).
* **Steps:**

  1. Login and navigate to `/dashboard`.
  2. Locate the **Account Balance** widget/section.
* **Expected Results:**

  * Account balance is displayed.
  * Balance is formatted as a currency value (e.g. `$1,000.00`).
  * No `NaN`, placeholder, or empty values are shown.

---

#### TC-DASH-02 – Start a transaction → Deposit

* **Page:** Dashboard (`/dashboard`)
* **Steps:**

  1. On the Dashboard, locate and click **Start a transaction** (or equivalent).
  2. From the transaction options, select **Deposit**.
  3. Wait for the URL to become `/dashboard/deposit`.
* **Expected Results:**

  * The app navigates to the Deposit page.
  * Deposit form is visible, including an amount input field and a **Submit** button.

---

#### TC-DASH-03 – Start a transaction → Withdrawal

* **Page:** Dashboard (`/dashboard`)
* **Steps:**

  1. On the Dashboard, locate and click **Start a transaction**.
  2. From the transaction options, select **Withdrawal**.
  3. Wait for the URL to become `/dashboard/withdraw`.
* **Expected Results:**

  * The app navigates to the Withdrawal page.
  * Withdrawal form is visible, including an amount input field and a **Submit** button.

---

### 4. Deposit — `https://mini-bank.testamplify.com/dashboard/deposit`

(BVA: `0`, negative, `50,000`)

#### TC-DEP-01 – Reject 0 amount (BVA)

* **Page:** Deposit (`/dashboard/deposit`)
* **Steps:**

  1. Navigate to `/dashboard/deposit`.
  2. Enter `0` in the amount field.
  3. Click **Submit**.
* **Expected Results:**

  * A validation error is shown (e.g. “Amount must be greater than 0”).
  * Deposit is not processed and balance does not change.

---

#### TC-DEP-02 – Reject negative amount (BVA)

* **Page:** Deposit (`/dashboard/deposit`)
* **Steps:**

  1. Navigate to `/dashboard/deposit`.
  2. Enter `-10` in the amount field.
* **Expected Results:**

  * The **Submit** button remains disabled when a negative amount is entered.
  * User cannot submit the form with a negative value.
  * No deposit is processed and balance does not change.

---

#### TC-DEP-03 – Accept large but valid amount (BVA)

* **Page:** Deposit (`/dashboard/deposit`)
* **Steps:**

  1. Navigate to `/dashboard/deposit`.
  2. Enter `50000` in the amount field.
  3. Click **Submit**.
  4. Confirm success via toast/message.
* **Expected Results:**

  * A success toast/message is displayed.
  * Account balance increases by **50,000**.
  * A deposit transaction record is created and visible in Transaction History.

---

### 5. Withdrawal — `https://mini-bank.testamplify.com/dashboard/withdraw`

#### TC-WITH-01 – Reject 0 amount (BVA)

* **Page:** Withdrawal (`/dashboard/withdraw`)
* **Steps:**

  1. Navigate to `/dashboard/withdraw`.
  2. Enter `0` in the amount field.
* **Expected Results:**

  * The **Submit** button is disabled when amount is `0`.
  * User cannot submit the form with amount `0`.
  * No withdrawal is processed and balance does not change.

---

#### TC-WITH-02 – Reject negative amount

* **Page:** Withdrawal (`/dashboard/withdraw`)
* **Steps:**

  1. Navigate to `/dashboard/withdraw`.
  2. Enter `-5` in the amount field.
* **Expected Results:**

  * The **Submit** button is disabled when a negative amount is entered.
  * User cannot submit the form with a negative value.
  * No withdrawal is processed and balance does not change.

---

#### TC-WITH-03 – Withdraw within available balance

* **Page:** Withdrawal (`/dashboard/withdraw`)
* **Precondition:** Account balance `B` is greater than or equal to withdrawal amount.
* **Steps:**

  1. From Dashboard, note current balance `B`.
  2. Navigate to `/dashboard/withdraw`.
  3. Enter an amount less than `B` (e.g. `50`).
  4. Click **Submit** and confirm success.
* **Expected Results:**

  * Withdrawal succeeds and a success message is shown.
  * Account balance decreases by the withdrawn amount.
  * A withdrawal transaction record is created and visible in Transaction History.

---

### 6. Transfer — `https://mini-bank.testamplify.com/dashboard/transfer`

**Decision Table (reference):**

| Case | Bank Type | Funds        | Expected |
| ---- | --------- | ------------ | -------- |
| A    | Same-bank | Sufficient   | Success  |
| B    | Same-bank | Insufficient | Error    |
| C    | External  | Sufficient   | Success* |
| D    | External  | Insufficient | Error    |

*External success may include additional fee/notice depending on implementation.

We’ll automate three cases (A, B, C).

---

#### TC-TR-01 – Same-bank, sufficient funds (Case A)

* **Page:** Transfer (`/dashboard/transfer`)
* **Precondition:** Account has sufficient funds.
* **Steps:**

  1. Navigate to `/dashboard/transfer`.
  2. Choose **Same-bank** transfer option.
  3. Select a valid recipient account within the same bank.
  4. Enter an amount less than the current balance.
  5. Click **Submit**.
* **Expected Results:**

  * Transfer completes successfully and shows a success message.
  * Account balance decreases by the transfer amount.
  * A transfer transaction record is created and visible in Transaction History.

---

#### TC-TR-02 – Same-bank, insufficient funds (Case B)

* **Page:** Transfer (`/dashboard/transfer`)
* **Precondition:** The chosen transfer amount is greater than available balance.
* **Steps:**

  1. Navigate to `/dashboard/transfer`.
  2. Choose **Same-bank** transfer.
  3. Select a valid same-bank recipient.
  4. Enter an amount greater than the current balance.
  5. Click **Submit**.
* **Expected Results:**

  * An error message is displayed (e.g. “Insufficient funds”).
  * Transfer does not complete; no money is moved.
  * Account balance remains unchanged.

---

#### TC-TR-03 – External, sufficient funds (Case C)

* **Page:** Transfer (`/dashboard/transfer`)
* **Precondition:** Account has sufficient funds.
* **Steps:**

  1. Navigate to `/dashboard/transfer`.
  2. Choose **External bank** transfer.
  3. Enter valid external bank routing and account details.
  4. Enter an amount less than the current balance.
  5. Click **Submit**.
* **Expected Results:**

  * A success message is shown indicating the external transfer was initiated.
  * Account balance decreases by the transfer amount (plus any applicable fee, if implemented).
  * A transfer transaction record is created and visible in Transaction History, optionally flagged as external.

---

### 7. Pay Bills — `https://mini-bank.testamplify.com/dashboard/bills`

#### TC-BILL-01 – Pay a saved biller successfully

* **Page:** Bills (`/dashboard/bills`)
* **Precondition:** At least one saved biller exists and account has sufficient funds.
* **Steps:**

  1. Navigate to `/dashboard/bills`.
  2. Select an existing biller from the list.
  3. Enter a valid amount greater than `0`.
  4. Click **Submit** / **Pay**.
* **Expected Results:**

  * Bill payment succeeds; a success message is displayed.
  * Account balance decreases by the paid amount.
  * A bill payment transaction is created and visible in Transaction History.

---

#### TC-BILL-02 – Reject invalid amount

* **Page:** Bills (`/dashboard/bills`)
* **Steps:**

  1. Navigate to `/dashboard/bills`.
  2. Select a biller.
  3. Enter `0` or a negative amount.
  4. Click **Submit** / **Pay**.
* **Expected Results:**

  * A validation error is shown indicating the amount is invalid.
  * Payment is not processed and account balance does not change.

---

#### TC-BILL-03 – Missing required fields

* **Page:** Bills (`/dashboard/bills`)
* **Steps:**

  1. Navigate to `/dashboard/bills`.
  2. Do **not** select a biller and/or leave the amount field empty.
  3. Click **Submit** / **Pay**.
* **Expected Results:**

  * Inline error messages appear indicating that required field(s) are missing (e.g. biller, amount).
  * Payment is not processed and account balance does not change.

---

### 8. Transactions — `https://mini-bank.testamplify.com/dashboard/transactions`

#### TC-TRX-01 – Deposit line shows `amt_deposited`

* **Page:** Transactions (`/dashboard/transactions`)
* **Precondition:** A successful deposit has been made (e.g. TC-DEP-03).
* **Steps:**

  1. Perform a successful deposit (TC-DEP-03).
  2. Navigate to `/dashboard/transactions`.
  3. Locate the most recent **Deposit** transaction.
* **Expected Results:**

  * A transaction row with type/label “Deposit” is visible.
  * The `amt_deposited` field shows the exact deposited amount.
  * Date/time and (if shown) running balance are consistent with the operation.

---

#### TC-TRX-02 – Withdrawal line shows `amt_withdrawn`

* **Page:** Transactions (`/dashboard/transactions`)
* **Precondition:** A successful withdrawal has been made (TC-WITH-03).
* **Steps:**

  1. Perform a successful withdrawal (TC-WITH-03).
  2. Navigate to `/dashboard/transactions`.
  3. Locate the most recent **Withdrawal** transaction.
* **Expected Results:**

  * A transaction row with type/label “Withdrawal” is visible.
  * The `amt_withdrawn` field shows the exact withdrawn amount.

---

#### TC-TRX-03 – Transfer/Bills amounts visible

* **Page:** Transactions (`/dashboard/transactions`)
* **Precondition:** A successful transfer (TC-TR-01) and bill payment (TC-BILL-01) exist.
* **Steps:**

  1. Perform a successful transfer (TC-TR-01).
  2. Perform a successful bill payment (TC-BILL-01).
  3. Navigate to `/dashboard/transactions`.
  4. Locate the latest **Transfer** and **Bill Payment** rows.
* **Expected Results:**

  * Transfer row shows `amt_transfered` with the correct transfer amount.
  * Bill payment row shows `amt_paid` with the correct payment amount.
  * Timestamps and (if displayed) running balance are consistent and correctly updated.

---

### 9. Logout

#### TC-LOGOUT-01 – Logout from dashboard

* **Page:** Dashboard / Global header
* **Steps:**

  1. Ensure the user is logged in and on `/dashboard`.
  2. In the header/user menu, click **Logout**.
  3. Wait for navigation to complete.
* **Expected Results:**

  * User session is cleared.
  * User is redirected to Home or Login page.
  * Protected routes (e.g. `/dashboard`) are no longer accessible without logging in again.

---

#### TC-LOGOUT-02 – Post-logout route protection

* **Page:** Any (after logout)
* **Precondition:** User has just logged out (TC-LOGOUT-01).
* **Steps:**

  1. After logout, attempt to navigate directly to `/dashboard` (via URL).
* **Expected Results:**

  * User is redirected back to the Login page.
  * No dashboard data or authenticated content is visible.

----

---

# 📦 Day 27 — Capstone Deliverables (Mini-Bank E2E)

This capstone focuses on finalizing the Mini-Bank end-to-end test suite, applying advanced testing concepts, and preparing for Allure Reporting integration on Day 28.

Below are the required deliverables.

---

## ✅ **1. Capstone README (Mini-Bank Overview)**

A clear document containing:

* What we are testing (26 curated E2E test cases across all core flows)
* Why these flows matter (risk-based prioritization, coverage scope)
* Which cases the instructor automated (3–6 examples)
* Which cases the students must automate
* How to run tests locally:
  `npx playwright test`

* How to run tests in CI (GitHub Actions workflow overview)

This README acts as the central reference for the Mini-Bank E2E project.

---

## Instructor vs Student Automation Split

- Instructor automated examples: `TC-HOME-01`, `TC-LOGIN-01`, `TC-DASH-01`, `TC-DEP-03`, `TC-WITH-03`, `TC-TR-01`
- Students automate the remaining cases, following role-first locator and POM guidance below.

---

## ✅ 2. Locator Audit (getByRole Strategy)

A brief analysis of the application’s DOM accessibility with concrete examples:

- Located via roles:
  - Header banner: `page.getByRole('banner')`
  - Login button: `page.getByRole('button', { name: 'Login' })`
  - Footer: `page.getByRole('contentinfo')`
  - Deposit submit: `page.getByRole('button', { name: 'Submit' })`
  - Amount input: `page.getByLabel('Amount')` (prefer labeled inputs)
- Missing semantics to note:
  - Buttons without accessible names
  - Inputs without labels
  - Absent landmarks (banner/contentinfo)
- Recommendations:
  - Ensure buttons have visible names or `aria-label`
  - Label all inputs and associate via `for`/`id`
  - Include ARIA landmarks for header/footer
- Semantic roles lead to more stable, self-healing locators.

---

## ✅ 3. CI Artifact: Report + Annotated Screenshots/Traces

Run the automated tests in CI and attach:

* At least **one Playwright Trace** (trace.zip)
* At least **one screenshot** annotated with notes
  Example:

  * flaky behavior
  * misaligned waiting logic
  * missing DOM roles
* Video artifact (if enabled)

These artifacts demonstrate debugging, observability, and test investigation workflows.

---

## ✅ 4. DOM vs POM Explanation

Include a short section in the README clarifying with a tiny example:

- DOM = the actual rendered HTML elements
- POM = the abstraction we create to access those elements

Rules:

- Stable, role-first locators inside Page Objects
- No selector or logic leakage into test files
- Tests call intent (e.g., `dashboardPage.startDeposit()`)

Example Page Object:

```ts
// dashboard.page.ts
export class DashboardPage {
  constructor(private page) {}
  async startDeposit() {
    await this.page.getByRole('button', { name: 'Start a transaction' }).click();
    await this.page.getByRole('button', { name: 'Deposit' }).click();
  }
  async getBalanceText() {
    return this.page.getByRole('region', { name: 'Account Balance' }).textContent();
  }
}
```

Example Test:

```ts
test('Balance visible after login', async ({ page }) => {
  const dashboardPage = new DashboardPage(page);
  // assume login fixture navigates to /dashboard
  await expect(await dashboardPage.getBalanceText()).toMatch(/\$\d/);
});
```

---

## ✅ 5. Self-Healing Code Strategy

Conceptual section with before/after:

Before (flaky):

```ts
await page.click('text=Login');
await page.waitForTimeout(2000);
```

After (stable):

```ts
await page.getByRole('button', { name: 'Login' }).click();
await expect(page).toHaveURL(/\/login$/);
```

- Prefer semantic locators (`getByRole`, labels)
- Prefer behavior-based waits (URL change, toasts, state)
- Use test IDs only as a last resort
- Avoid custom retries; rely on Playwright auto-waiting

---

## ✅ 6. Refactoring Pass (Before → After)

Document improvements made during refactoring:

- Remove duplicate login steps via fixtures
- Move navigation helpers into reusable utilities
- Extract selectors/actions into POM files
- Replace timeouts with role-based checks
- Simplify test logic for readability

---

## ✅ 7. Debugging & Flake Analysis Walkthrough

Create a short documented flow for debugging failures:

- Reproduce locally with `npx playwright test --debug`
- Use `.only` to isolate failing tests
- Tag async areas prone to race conditions
- Review HAR logs for backend issues
- Open the trace viewer to inspect timing + DOM snapshots

Example flake root cause:

- Symptom: test clicks **Deposit** before the transaction menu finishes rendering.
- Mitigation: wait for `page.getByRole('button', { name: 'Deposit' })` to be visible before clicking; assert URL change.

---

## ✅ 8. CI/CD Contribution (Light Version for Day 27)

Include a minimal GitHub Actions setup:

```yaml
name: E2E
on: [push, pull_request]
jobs:
  tests:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-node@v4
        with:
          node-version: 'lts/*'
      - run: npm ci
      - run: npx playwright install --with-deps
      - run: npx playwright test
      - name: Upload Playwright artifacts on failure
        if: failure()
        uses: actions/upload-artifact@v4
        with:
          name: playwright-artifacts
          path: |
            playwright-report/**
            test-results/**
            traces/**
            videos/**
```

This prepares the pipeline foundation for Day 28’s enhancements.

---

## 🚫 9. Allure Reports & CI/CD Enhancements (Postponed to Day 28)

Allure & advanced CI/CD concepts *intentionally moved to Day 28*:

* Allure results & report artifacts
* Trend history folder
* CI badge
* Understanding “retry passed = still a failure”
* How to read CI reports in daily standups
* CI PR annotations

This keeps Day 27 focused and manageable.

---

## 🎯 Day 27 Exit Criteria

* [ ] 26 E2E test cases listed in README
* [ ] 3–6 sample automated tests written by instructor
* [ ] Remaining tests assigned to students
* [ ] Locator audit completed and documented
* [ ] Self-healing locator strategy section added
* [ ] DOM vs POM explanation included
* [ ] Refactoring notes added
* [ ] Debugging + flake analysis walkthrough documented
* [ ] CI runs Playwright tests and uploads artifacts (traces/screenshots)
* [ ] At least one screenshot/trace annotated and shown in README
* [ ] Allure postponed (saved for Day 28)

---
