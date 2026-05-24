# Instructions

- Following Playwright test failed.
- Explain why, be concise, respect Playwright best practices.
- Provide a snippet of code with the fix, if possible.

# Test info

- Name: auth/crm-login.spec.ts >> CRM Authentication - Login Tests >> TC_LOGIN_001: Verify trang Login hiển thị đầy đủ
- Location: src/tests/auth/crm-login.spec.ts:15:9

# Error details

```
Error: expect(locator).toBeVisible() failed

Locator: locator('#email')
Expected: visible
Timeout: 5000ms
Error: element(s) not found

Call log:
  - Expect "toBeVisible" with timeout 5000ms
  - waiting for locator('#email')

```

# Page snapshot

```yaml
- generic [active] [ref=e1]:
  - heading "Not Found" [level=1] [ref=e2]
  - paragraph [ref=e3]: The requested URL was not found on this server.
```

# Test source

```ts
  1   | import { test, expect } from '../../fixtures/base.fixture';
  2   | import { EnvConfig } from '../../utils/env.config';
  3   | 
  4   | test.describe('CRM Authentication - Login Tests', () => {
  5   | 
  6   |     const VALID_EMAIL = EnvConfig.ADMIN_USERNAME;
  7   |     const VALID_PASSWORD = EnvConfig.ADMIN_PASSWORD;
  8   | 
  9   |     test.beforeEach(async ({ loginPage }) => {
  10  |         await loginPage.navigate();
  11  |     });
  12  | 
  13  |     // ==================== MOD-01: Login Form UI ====================
  14  | 
  15  |     test('TC_LOGIN_001: Verify trang Login hiển thị đầy đủ', async ({ loginPage }) => {
  16  |         await test.step('Verify all UI elements on Login page are visible', async () => {
> 17  |             await expect(loginPage.inputEmail).toBeVisible();
      |                                                ^ Error: expect(locator).toBeVisible() failed
  18  |             await expect(loginPage.inputPassword).toBeVisible();
  19  |             await expect(loginPage.btnLogin).toBeVisible();
  20  |             await expect(loginPage.lblHeading).toHaveText('Login');
  21  |             await expect(loginPage.divRememberMeWrapper).toBeVisible();
  22  |             await expect(loginPage.lnkForgotPassword).toBeVisible();
  23  |         });
  24  |     });
  25  | 
  26  |     test('TC_LOGIN_002: HTML5 validation khi email không có @', async ({ loginPage, page }) => {
  27  |         await loginPage.login('invalidemail', VALID_PASSWORD);
  28  |         
  29  |         await test.step('Verify HTML5 validation message for missing "@"', async () => {
  30  |             const validationMsg = await loginPage.getEmailValidationMessage();
  31  |             expect(validationMsg).toContain('@');
  32  |             expect(page.url()).toContain('authentication');
  33  |         });
  34  |     });
  35  | 
  36  |     test('TC_LOGIN_003: HTML5 validation khi email thiếu domain', async ({ loginPage, page }) => {
  37  |         await loginPage.login('test@', VALID_PASSWORD);
  38  |         
  39  |         await test.step('Verify HTML5 validation message for missing domain', async () => {
  40  |             const validationMsg = await loginPage.getEmailValidationMessage();
  41  |             expect(validationMsg).not.toBe('');
  42  |             expect(page.url()).toContain('authentication');
  43  |         });
  44  |     });
  45  | 
  46  |     test('TC_LOGIN_004: Password field hiển thị masked', async ({ loginPage }) => {
  47  |         await test.step('Verify Password field type is masked as "password"', async () => {
  48  |             await expect(loginPage.inputPassword).toHaveAttribute('type', 'password');
  49  |         });
  50  |     });
  51  | 
  52  |     test('TC_LOGIN_005: Remember me mặc định unchecked', async ({ loginPage }) => {
  53  |         await test.step('Verify Remember Me checkbox is unchecked by default', async () => {
  54  |             await expect(loginPage.chkRememberMe).not.toBeChecked();
  55  |         });
  56  |     });
  57  | 
  58  |     // ==================== MOD-02A: Happy Path ====================
  59  | 
  60  |     test('TC_LOGIN_009: Đăng nhập thành công', async ({ loginPage, dashboardPage, page }) => {
  61  |         await loginPage.login(VALID_EMAIL, VALID_PASSWORD);
  62  |         
  63  |         await test.step('Verify Dashboard page loaded successfully', async () => {
  64  |             await expect(dashboardPage.lblWrapper).toBeVisible();
  65  |             expect(page.url()).toContain('/admin');
  66  |             await expect(page).toHaveTitle(/Dashboard/);
  67  |         });
  68  |     });
  69  | 
  70  |     // ==================== MOD-02C: Empty Fields ====================
  71  | 
  72  |     test('TC_LOGIN_010: Lỗi server-side khi trống Email', async ({ loginPage }) => {
  73  |         await loginPage.login('', 'AnyPass@123');
  74  |         
  75  |         await test.step('Verify server-side error message for missing Email', async () => {
  76  |             await expect(loginPage.lblAlertDanger).toBeVisible();
  77  |             await expect(loginPage.lblAlertDanger).toContainText('Email Address field is required');
  78  |         });
  79  |     });
  80  | 
  81  |     test('TC_LOGIN_011: Lỗi server-side khi trống Password', async ({ loginPage }) => {
  82  |         await loginPage.login(VALID_EMAIL, '');
  83  |         
  84  |         await test.step('Verify server-side error message for missing Password', async () => {
  85  |             await expect(loginPage.lblAlertDanger).toBeVisible();
  86  |             await expect(loginPage.lblAlertDanger).toContainText('Password field is required');
  87  |         });
  88  |     });
  89  | 
  90  |     test('TC_LOGIN_012: Lỗi khi trống cả Email và Password', async ({ loginPage }) => {
  91  |         await test.step('Click login button without inputting credentials', async () => {
  92  |             await loginPage.clickElement(loginPage.btnLogin);
  93  |         });
  94  |         
  95  |         await test.step('Verify validation errors for both missing Email and Password', async () => {
  96  |             await expect(loginPage.lblAlertDanger.first()).toBeVisible();
  97  |             const texts = await loginPage.lblAlertDanger.allInnerTexts();
  98  |             const combinedText = texts.join(' ');
  99  |             expect(combinedText).toContain('Email Address field is required');
  100 |             expect(combinedText).toContain('Password field is required');
  101 |         });
  102 |     });
  103 | 
  104 |     // ==================== MOD-02B: Invalid Credentials ====================
  105 | 
  106 |     test('TC_LOGIN_013: Email đúng, Password sai', async ({ loginPage, page }) => {
  107 |         await loginPage.login(VALID_EMAIL, 'WrongPass@999');
  108 |         
  109 |         await test.step('Verify danger alert for wrong Password', async () => {
  110 |             await expect(loginPage.lblAlertDanger).toBeVisible();
  111 |             await expect(loginPage.lblAlertDanger).toContainText('Invalid email or password');
  112 |             expect(page.url()).toContain('authentication');
  113 |         });
  114 |     });
  115 | 
  116 |     test('TC_LOGIN_014: Email sai, Password đúng', async ({ loginPage }) => {
  117 |         await loginPage.login('hacker@gmail.com', VALID_PASSWORD);
```