# Instructions

- Following Playwright test failed.
- Explain why, be concise, respect Playwright best practices.
- Provide a snippet of code with the fix, if possible.

# Test info

- Name: auth/crm-login.spec.ts >> CRM Authentication - Login Tests >> TC_LOGIN_020: Đã login → truy cập login → redirect Dashboard
- Location: src/tests/auth/crm-login.spec.ts:151:9

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
  1  | import { Page, Locator, expect, test } from '@playwright/test';
  2  | 
  3  | export abstract class BasePage {
  4  |     readonly page: Page;
  5  |     readonly url: string;
  6  | 
  7  |     constructor(page: Page, url: string = '') {
  8  |         this.page = page;
  9  |         this.url = url;
  10 |     }
  11 | 
  12 |     async navigate() {
  13 |         await test.step(`Navigate to page: "${this.url}"`, async () => {
  14 |             await this.page.goto(this.url);
  15 |             await this.page.waitForLoadState('domcontentloaded');
  16 |         });
  17 |     }
  18 | 
  19 |     async clickElement(locator: Locator) {
  20 |         await expect(locator).toBeVisible();
  21 |         await expect(locator).toBeEnabled();
  22 |         await locator.click();
  23 |     }
  24 | 
  25 |     async fillText(locator: Locator, text: string) {
> 26 |         await expect(locator).toBeVisible();
     |                               ^ Error: expect(locator).toBeVisible() failed
  27 |         await expect(locator).toBeEnabled();
  28 |         await locator.fill(text);
  29 |     }
  30 | 
  31 |     async getText(locator: Locator): Promise<string> {
  32 |         await expect(locator).toBeVisible();
  33 |         return await locator.innerText();
  34 |     }
  35 | }
  36 | 
```