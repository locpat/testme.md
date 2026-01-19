# TESTME.md

> Human-readable test specifications for AI agents

## What is TESTME.md?

TESTME.md is a convention for writing test specifications in plain Markdown that both humans and AI agents can understand and execute. Instead of verbose test code, you describe tests in natural language with clear steps and expectations.

## Why TESTME.md?

| Traditional Tests | TESTME.md |
|-------------------|-----------|
| Language-specific syntax | Plain Markdown |
| Framework dependencies | No dependencies |
| Verbose boilerplate | Clear, concise steps |
| Requires developer knowledge | Anyone can read and write |
| Tight coupling to implementation | Intent-focused |

## Quick Example

**Before** — 15 lines of TypeScript:

```typescript
import { expect, test } from '@playwright/test';

test.describe('Login Page', () => {
  test('shows error with wrong credentials', async ({ page }) => {
    await page.goto('/login');
    await page.locator('#email').fill('wrong@example.com');
    await page.locator('#password').fill('wrongpassword');
    await page.locator('button[type="submit"]').click();
    await expect(page.locator('.error-message')).toBeVisible();
  });
});
```

**After** — Clear, readable Markdown:

```markdown
### Test: Wrong credentials show error

1. Go to `/login`
2. Enter email "wrong@example.com" and password "wrongpassword"
3. Click submit

**Expect:** Error message is visible
```

## Getting Started

1. Read the [full specification](TESTME-SPEC.md)
2. Browse the [examples](examples/)
3. Create a `TESTME.md` in your project

## File Structure

```
your-project/
├── TESTME.md              # Root-level tests
├── tests/
│   └── TESTME.md          # Test directory specs
├── e2e/
│   ├── TESTME.md          # E2E test specs
│   └── auth.testme.md     # Additional test files
└── components/
    └── Button.testme.md   # Component-specific tests
```

## Documentation

- **[TESTME-SPEC.md](TESTME-SPEC.md)** — Complete specification document
- **[examples/](examples/)** — Real-world examples by pattern

## For AI Agents

AI agents should:

1. Look for `TESTME.md` or `*.testme.md` files in the repository
2. Parse Prerequisites, Setup, Tests, and Teardown sections
3. Execute steps and verify expectations
4. Report results in a clear format

See the [specification](TESTME-SPEC.md#for-ai-agents) for detailed execution guidelines.

## License

This specification is released under the MIT License. Feel free to adopt and adapt it for your projects.
