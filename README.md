# Mini-Bank E2E – Day 27 (Repo Walkthrough & Capstone Summary)

Day 27 is about **understanding everything we’ve built so far**.

Not just writing tests — but knowing:
- what every folder does,
- what every file is responsible for,
- how tests are structured,
- how to run, debug, and read results,
- and how students can extend the project safely.

This README is the **single source of truth** for the Mini-Bank E2E project.

Base URL: https://mini-bank.testamplify.com/

---

## 1) What This Project Is

This is a **Playwright End-to-End (E2E) automation project** for the Mini-Bank web application.

We validate the most important user flows:
- Home
- Login
- Dashboard
- Deposit
- Withdrawal
- Transfer
- Bills
- Transactions
- Logout

By Day 27, we focus on:
- clean project structure
- accessibility-first locators
- Page Object Model (POM)
- stable, self-healing tests
- debugging & reports

Day 28 will extend this with **Allure + deeper CI/CD concepts**.

---

## 2) Prerequisites & Setup

### Node.js
Make sure **Node.js LTS** is installed.

### Install dependencies
`npm ci` installs dependencies **exactly** as locked in `package-lock.json`.

```bash
npm ci
````

### Install Playwright browsers

Downloads Chromium, Firefox, WebKit **and** required system dependencies.

```bash
npx playwright install --with-deps
```

---

## 3) How To Run Tests

### Run all tests

```bash
npx playwright test
```

### Debug mode (step through tests)

```bash
npx playwright test --debug
```

### Open HTML report

```bash
npx playwright show-report
```

---

## 4) Repository Structure (What Everything Means)

Below is what you see in VS Code and **why each part exists**.

---

### ✅ `node_modules/`

All installed npm packages live here.

* Generated automatically
* Never edited manually
* Excluded from git

---

### ✅ `pages/`

This folder contains **shared page-level code** used by the test suite.

Most commonly, this is where **Page Objects (POM)** live.

Purpose:

* Hide DOM details (selectors, roles, labels)
* Expose intent-based actions:

  * `loginPage.login(email, password)`
  * `dashboardPage.startDeposit()`

Tests should **not** contain raw selectors when POMs exist.

---

### ✅ `tests/`

This is where **Playwright test files** live.

* Files end with `.spec.js`
* Playwright automatically discovers these
* Tests describe **user behavior**, not DOM mechanics

Good tests read like:

> “Login → Deposit money → Verify balance updated”

---

### ✅ `utils/`

Reusable helper logic that is **not page-specific**.

Examples:

* parsing currency values
* generating random test data
* shared navigation helpers
* common setup helpers

If logic is reused but doesn’t belong to one page → it goes here.

---

### ✅ `test-data/`

Static or reusable input data for tests.

Examples:

* sample users (non-secret)
* recipients / billers
* predefined amounts

Rules:

* Reusable data → yes
* Secrets / passwords → ❌ never here

---

### ✅ `playwright-report/`

HTML report generated after test runs.

Open it with:

```bash
npx playwright show-report
```

Useful for:

* viewing passed/failed tests
* screenshots
* error messages

---

### ✅ `test-results/`

Raw Playwright artifacts:

* screenshots
* traces
* videos (if enabled)

This folder is auto-generated and often uploaded as CI artifacts.

---

### ✅ `.gitignore`

Defines what **should not be committed**:

* node_modules
* reports
* test results
* artifacts

---

### ✅ `package.json`

Project metadata and dependencies.

Common scripts you may add:

```json
"scripts": {
  "test": "playwright test",
  "test:debug": "playwright test --debug",
  "report": "playwright show-report"
}
```

---

### ✅ `package-lock.json`

Locks dependency versions.

Why it matters:

* Everyone installs the **same versions**
* `npm ci` relies on this file

---

### ✅ `playwright.config.js`

Global Playwright configuration:

* baseURL
* reporters
* browser settings
* timeouts & retries

Centralizes config so tests stay clean.

---

### ✅ `README.md`

This file.

Acts as:

* onboarding guide
* project reference
* Day 27 summary

---

## 5) Where To Work (Quick Guide)

If you’re a student:

* Write tests → `tests/`
* Add page actions/locators → `pages/`
* Share helpers → `utils/`
* Store inputs → `test-data/`
* View results → `playwright-report/`, `test-results/`

---

## 6) DOM vs POM (Simple Explanation)

### DOM

The real HTML elements rendered in the browser.

### POM (Page Object Model)

A layer we create to **hide DOM details**.

❌ Bad (DOM leakage in tests):

```js
await page.click('text=Login');
await page.waitForTimeout(2000);
```

✅ Good (POM + self-healing):

```js
await homePage.clickLogin();
await expect(page).toHaveURL(/\/login$/);
```

---

## 7) Self-Healing Test Rules

Prefer:

* `getByRole`
* `getByLabel`
* `toHaveURL`
* `toBeVisible`

Avoid:

* `waitForTimeout`
* brittle CSS selectors
* hard-coded delays

Playwright already waits intelligently — let it.

---

## 8) Debugging Workflow

When a test fails:

1. Re-run in debug mode

```bash
npx playwright test --debug
```

2. Run a single test file

```bash
npx playwright test tests/<file>.spec.js
```

3. Temporarily isolate a test

```js
test.only('...', async ({ page }) => {})
```

4. Open HTML report

```bash
npx playwright show-report
```

5. Inspect screenshots / traces in `test-results/`

---

## 9) Test Coverage Summary Example

This project defines **26 E2E test cases** covering all major Mini-Bank flows:

* Home: 3
* Login: 3
* Dashboard: 3
* Deposit: 3
* Withdrawal: 3
* Transfer: 3
* Bills: 3
* Transactions: 3
* Logout: 2

**Total: 26 test cases**

---


### Student responsibility:

* Automate remaining cases
* Follow the same folder structure
* Use POM + role-first locators
* Avoid time-based waits

---

## 10) What’s Next (Day 28)

Intentionally postponed to keep Day 27 focused:

* Allure reports
* CI trend history
* advanced CI annotations
* retry analysis

Day 27 is about **clarity, structure, and stability**.

---

## ✅ Day 27 Exit Criteria

* [ ] Repo structure understood
* [ ] Tests run locally
* [ ] HTML report reviewed
* [ ] POM concept understood
* [ ] Self-healing locators applied
* [ ] You know where to add tests
* [ ] Ready for Allure & CI upgrades (Day 28)
