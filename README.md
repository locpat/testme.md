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

## This Repository

This repo contains everything you need to adopt TESTME.md:

| File | Purpose |
|------|---------|
| [SPECS.md](SPECS.md) | The complete specification (v1.0) |
| [WRITE_TESTME.md](WRITE_TESTME.md) | Instructions for AI agents to write TESTME.md files |
| [example/](example/) | Complete example for an e-commerce application |

### Workflow

1. **Read the spec** — Understand the format via [SPECS.md](SPECS.md)
2. **Generate tests** — Give an AI agent [WRITE_TESTME.md](WRITE_TESTME.md) to analyze your codebase and create TESTME.md files
3. **Run tests** — Have an AI agent execute the TESTME.md files and report results

## Getting Started

### Option 1: Write Tests Manually

1. Read the [full specification](SPECS.md)
2. Browse the [example](example/)
3. Create a `TESTME.md` in your project

### Option 2: Generate Tests with AI

Pipe the instructions directly to your AI agent:

```bash
curl -s https://raw.githubusercontent.com/evilsocket/testme.md/main/WRITE_TESTME.md | cursor
```

Or manually:

1. Copy the contents of [WRITE_TESTME.md](WRITE_TESTME.md)
2. Give it to an AI agent along with your codebase
3. The agent will analyze your code and generate TESTME.md files

## File Structure

In your project:

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

| Document | Description |
|----------|-------------|
| [SPECS.md](SPECS.md) | Complete format specification |
| [WRITE_TESTME.md](WRITE_TESTME.md) | Agent prompt for generating TESTME.md files |
| [example/](example/) | Example TESTME.md files for an e-commerce app |

## For AI Agents

### Executing Tests

AI agents should:

1. Look for `TESTME.md` or `*.testme.md` files in the repository
2. Parse Prerequisites, Setup, Tests, and Teardown sections
3. Execute steps and verify expectations
4. Report results in a clear format

See the [specification](SPECS.md#for-ai-agents) for detailed execution guidelines.

### Writing Tests

To generate TESTME.md files for a codebase, use [WRITE_TESTME.md](WRITE_TESTME.md) as your instruction set.

## License

This specification is released under the MIT License. Feel free to adopt and adapt it for your projects.
