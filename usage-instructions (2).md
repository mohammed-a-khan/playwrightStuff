# QAF BDD to Playwright TypeScript Converter

This utility converts QAF BDD framework tests (using Selenium and Java) to Playwright tests in TypeScript. It handles the transformation of locators, assertions, wait conditions, BDD patterns, and Page Object Models with custom CSWebElement implementations.

## Prerequisites

- Node.js (v14 or later)
- TypeScript
- Your existing QAF BDD framework tests

## Installation

1. Save the converter script to a file named `qaf-to-playwright-converter.ts`
2. Install the required dependencies:

```bash
npm init -y
npm install typescript ts-node @types/node --save-dev
npm install @playwright/test --save-dev
npx playwright install
```

## Usage

### Converting a single file

```bash
npx ts-node qaf-to-playwright-converter.ts path/to/your/JavaTest.java path/to/output/JavaTest.spec.ts
```

### Converting a directory of files

```bash
npx ts-node qaf-to-playwright-converter.ts path/to/your/test/directory path/to/output/directory
```

 Playwright test.step() calls
- Java variable declarations → TypeScript declarations

## Post-Conversion Steps

After conversion, you may need to:

1. Review the generated TypeScript files for any manual adjustments
2. Update selectors if needed (the converter maintains existing selectors)
3. Customize the generated `playwright.config.ts` file with your project's specific configurations
4. Run the tests to verify that the conversion worked as expected:

```bash
npx playwright test
```

## Example Conversion

The converter transforms QAF BDD and Selenium patterns into their Playwright equivalents:

| QAF BDD / Selenium (Java) | Playwright (TypeScript) |
|---------------------------|-------------------------|
| `driver.get(url)` | `await page.goto(url)` |
| `driver.findElement(By.id("element")).click()` | `await page.click('#element')` |
| `driver.findElement(By.id("element")).sendKeys(text)` | `await page.fill('#element', text)` |
| `WebDriverWait(driver, 10).until(ExpectedConditions.visibilityOfElementLocated(By.id("element")))` | `await page.waitForSelector('#element', { timeout: 10 * 1000 })` |
| `Assert.assertEquals(actual, expected)` | `expect(actual).toBe(expected)` |
| `@QAFTestStep(description = "step description")` | `// @step: step description` |
| `@Test` | `test('testName', async ({ page }) => { ... })` |

## Limitations

Some aspects of QAF BDD tests might require manual adjustments:

1. Complex custom locators
2. Custom wait conditions
3. QAF configuration settings
4. QAF-specific utility methods
5. Integration with custom frameworks

## Troubleshooting

If you encounter issues during conversion:

1. Check that your Java files follow standard QAF BDD patterns
2. Examine the console output for any error messages
3. Review the generated TypeScript files for syntax errors
4. Run the Playwright tests with debug mode: `npx playwright test --debug`
