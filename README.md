<div align="center">

# `TESTME.md`

<i>Human-readable test specifications for AI agents</i>

[![Release](https://img.shields.io/github/release/evilsocket/testme.md.svg?style=flat-square)](https://github.com/evilsocket/testme.md/releases/latest)
[![License](https://img.shields.io/badge/license-GPL3-brightgreen.svg?style=flat-square)](https://github.com/evilsocket/testme.md/blob/master/LICENSE.md)

</div>

TESTME.md is a convention for writing test specifications in plain Markdown that both humans and AI agents can understand and execute. Instead of verbose test code, you describe tests in natural language with clear steps and expectations.

## Why TESTME.md?

| Traditional Tests | TESTME.md |
|-------------------|-----------|
| Language-specific syntax | Plain Markdown |
| Framework dependencies | No dependencies |
| Verbose boilerplate | Clear, concise steps |
| Requires developer knowledge | Anyone can read and write |
| Tight coupling to implementation | Intent-focused |

## Quick Start

Generate TESTME.md files for your codebase:

```bash
curl -s https://raw.githubusercontent.com/evilsocket/testme.md/main/WRITE.md | cursor
```

Execute existing TESTME.md files:

```bash
curl -s https://raw.githubusercontent.com/evilsocket/testme.md/main/QA.md | cursor
```

> **Note:** These examples use [Cursor](https://cursor.sh), but any AI agent works. You can pipe to other CLI agents, or copy/paste the file contents into ChatGPT, Claude, or any AI assistant with access to your codebase.

## Example

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

| File | Purpose |
|------|---------|
| [SPECS.md](SPECS.md) | The complete specification (v1.0.0a) |
| [WRITE.md](WRITE.md) | Agent instructions to generate TESTME.md files |
| [QA.md](QA.md) | Agent instructions to execute TESTME.md files |
| [example/](example/) | Complete example for an e-commerce application |

### Workflow

1. **Read the spec** — Understand the format via [SPECS.md](SPECS.md)
2. **Generate tests** — Give an AI agent [WRITE.md](WRITE.md) to analyze your codebase and create TESTME.md files
3. **Run tests** — Give an AI agent [QA.md](QA.md) to find and execute TESTME.md files

## Manual Setup

If you prefer not to use the curl commands, you can set things up manually.

### Writing Tests Manually

1. Read the [full specification](SPECS.md)
2. Browse the [example](example/)
3. Create a `TESTME.md` in your project

### Generating Tests with AI

1. Copy the contents of [WRITE.md](WRITE.md)
2. Paste it into your AI agent (with codebase access)
3. The agent will analyze your code and generate TESTME.md files

### Running Tests with AI

1. Copy the contents of [QA.md](QA.md)
2. Paste it into your AI agent (with codebase access)
3. The agent will find all TESTME.md files and execute them

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
| [WRITE.md](WRITE.md) | Agent prompt for generating TESTME.md files |
| [QA.md](QA.md) | Agent prompt for executing TESTME.md files |
| [example/](example/) | Example TESTME.md files for an e-commerce app |

## For AI Agents

### Writing Tests

To generate TESTME.md files for a codebase, use [WRITE.md](WRITE.md) as your instruction set.

### Executing Tests

To find and execute TESTME.md files, use [QA.md](QA.md) as your instruction set.

See the [specification](SPECS.md#for-ai-agents) for detailed guidelines.

## License

This project is released under the [GNU General Public License v3.0](LICENSE).

[![Star History Chart](https://api.star-history.com/svg?repos=evilsocket/testme.md&type=Date)](https://star-history.com/#evilsocket/testme.md&Date)
