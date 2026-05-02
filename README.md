# rest-api-automation-newman

Automated API test suite built with Newman and Allure, integrated with a GitHub Actions CI/CD pipeline that publishes test reports and sends Slack notifications on every run.

---

## Live Report

[View Allure Report](https://emilianoqa.github.io/rest-api-automation-newman/)

![CI](https://github.com/EmilianoQA/rest-api-automation-newman/actions/workflows/newman-tests.yml/badge.svg)

---

## Stack

| Tool | Purpose |
|---|---|
| Newman | CLI runner for Postman collections |
| Allure | Visual test report generation |
| newman-reporter-htmlextra | Standalone HTML report |
| GitHub Actions | CI/CD pipeline |
| GitHub Pages | Report hosting |
| Slack Incoming Webhooks | Test result notifications |

---

## API Under Test

**Frankfurter** — open-source exchange rates API backed by the European Central Bank.
Base URL: `https://api.frankfurter.dev/v2`
No authentication required.

---

## Test Coverage

20 test cases across 5 modules:

| Module | Cases | What is validated |
|---|---|---|
| Currencies | 4 | List structure, required fields, single currency detail, invalid code handling |
| Latest Rates | 4 | Default base currency, custom base, filtered quotes, invalid base error |
| Historical Rates | 5 | Valid date, weekend behavior, earliest available date, out-of-range date, future date |
| Time Series | 4 | Monthly range, filtered currency, inverted range, weekly range |
| Single Rate | 3 | Valid pair, business logic assertion, same-currency error |

---

## Pipeline

Every push to `main` triggers the following sequence:

1. Install dependencies
2. Run Newman test suite
3. Install Allure CLI
4. Generate Allure report
5. Publish report to GitHub Pages
6. Send Slack notification with result and links

The pipeline runs even when tests fail, ensuring the report is always published and the team is always notified.

---

## Run Locally

**Requirements:** Node.js 18+, Allure CLI

```bash
# Clone the repository
git clone https://github.com/EmilianoQA/rest-api-automation-newman.git
cd rest-api-automation-newman

# Install dependencies
npm install

# Run tests with full reporting
npm test

# Run tests in CLI only mode
npm run test:cli

# Open Allure report in browser
npm run report
```

---

## Project Structure

```
rest-api-automation-newman/
├── .github/
│   └── workflows/
│       └── newman-tests.yml        # CI/CD pipeline definition
├── collections/
│   └── frankfurter.postman_collection.json   # 20 test cases
├── environments/
│   └── qa.postman_environment.json           # Environment variables
├── results/                        # Generated HTML report (gitignored)
├── allure-results/                 # Generated Allure data (gitignored)
├── .gitignore
├── package.json
└── README.md
```

---

## Slack Notification

The pipeline sends a notification to a Slack channel after every run. The message includes the test result, branch name, commit message, a link to the Allure report and a link to the Actions run.

The webhook URL is stored as a GitHub Secret and never exposed in the codebase.
