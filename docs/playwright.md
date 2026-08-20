# Playwright

Playwright is useful for browser automation, scraping, end-to-end tests, and AI-assisted browser control through Playwright MCP.

## Used In

- `Swiggy-Instamart` - Python Playwright and `@playwright/test`.
- `Shopping` - Playwright Java dependency alongside Selenium.

## Node Setup

```bash
npm install -D @playwright/test
npx playwright install
npx playwright test
```

## Python Setup

```bash
pip install playwright
python -m playwright install
```

## MCP Setup

```bash
npx -y @playwright/mcp@latest --browser=chrome --caps=vision,pdf,devtools
```

## Good Uses

- Modern web app end-to-end tests.
- Browser automation with reliable selectors.
- Screenshots, PDFs, traces, and visual verification.
- AI-assisted browser exploration through MCP.

## Checklist

- Install browsers in CI.
- Prefer accessible selectors.
- Save traces or screenshots on failure.
- Keep secrets out of browser logs.
- Use explicit waits for app states, not arbitrary sleeps.

