# Playwright + TypeScript Automation Framework

This is an E2E Web UI Automation Framework built with Playwright Test and TypeScript in strict mode.

## Features
- **Page Object Model (POM)** pattern.
- **Strict TypeScript Mode** enabled for robust typing.
- **Custom Fixtures** to inject Page Objects cleanly into tests.
- **Environment Management** via `dotenv`.
- **Allure Report Integration** enabled:
  - Generates highly detailed and interactive HTML reports.
  - Excludes noisy, raw Playwright API steps and shows clean, nested business steps with custom English descriptions.
  - Automatically captures and attaches **screenshots for passed tests** at the final step.
  - Automatically attaches **screenshots, videos, and error logs for failed tests** to simplify debugging.
- **HTML Reporting** included by default.
- **CI/CD Integrated** with GitHub Actions and automatic **GitHub Pages** deployment.
- Uses **Smart Waits** with Playwright's `expect()` assertions.

## Prerequisites
- Node.js (v18 or above)
- npm or yarn

## Setup

1. Install dependencies:
   ```bash
   npm install
   ```

2. Install Playwright browsers:
   ```bash
   npx playwright install --with-deps
   ```

3. Configure environment variables:
   Copy `.env.example` to `.env` and update the values.
   ```bash
   cp .env.example .env
   ```

## Running Tests

### Standard Playwright Executions
- Run all tests in headless mode:
  ```bash
  npm test
  ```
- Run tests in UI mode:
  ```bash
  npm run test:ui
  ```
- Run tests in headed mode:
  ```bash
  npm run test:headed
  ```
- View native Playwright HTML Report:
  ```bash
  npm run report
  ```

### Allure Report Executions
- **Run tests & generate Allure Report** (Recommended command):
  ```bash
  npm run test:allure
  ```
  *This automatically cleans previous results, runs the test suite, and compiles the Allure report.*

- **Open Allure Report** on a local web server to view results:
  ```bash
  npm run allure:open
  ```

- **Generate Allure Report** manually from test results:
  ```bash
  npm run allure:generate
  ```

- **Clean up** previous Allure results and reports:
  ```bash
  npm run allure:clear
  ```

## CI/CD and GitHub Actions

The framework is fully integrated with GitHub Actions (configured in `.github/workflows/playwright.yml`). Upon code push to `main` or `master` branches:
1. Tests are executed on an Ubuntu runner.
2. Allure Report is automatically generated with historical trends.
3. The report is published directly to **GitHub Pages** (on the `gh-pages` branch).
4. Raw artifacts (both Playwright HTML Report and Allure Report files) are uploaded for offline download.

### To view the report online:
1. Go to your GitHub Repository > **Settings** > **Pages**.
2. Under **Build and deployment**, set Source to **Deploy from a branch**.
3. Select the **gh-pages** branch (and `/ (root)` folder) and click **Save**.
4. GitHub will deploy and provide you with a public URL to view the live interactive reports.

### How to configure Environment Variables on GitHub:
To configure the environment variables required for test execution (`BASE_URL`, `ADMIN_USERNAME`, `ADMIN_PASSWORD`):
1. Go to your GitHub Repository > **Settings**.
2. On the left sidebar, go to **Security** > **Secrets and variables** > **Actions**.
3. Under the **Secrets** tab, click **New repository secret** to add sensitive variables (recommended for credentials):
   - **Name**: `ADMIN_USERNAME`, **Value**: *[your admin email]*
   - **Name**: `ADMIN_PASSWORD`, **Value**: *[your admin password]*
4. If you want to configure non-sensitive variables (like `BASE_URL`), you can either add them as secrets or switch to the **Variables** tab and click **New repository variable**:
   - **Name**: `BASE_URL`, **Value**: `https://opensource-demo.orangehrmlive.com`

## Project Structure
- `src/pages/` - Page Object classes encapsulating locators and actions.
- `src/fixtures/` - Custom Playwright fixtures (includes auto-fixture to capture screenshot on test success).
- `src/tests/` - Test specs organized by feature.
- `src/utils/` - Helpers, data generators, and config readers.
- `playwright.config.ts` - Playwright runner configuration (configured with allure-playwright reporter).
