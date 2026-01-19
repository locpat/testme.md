# TESTME.md Specification

**Version 1.0**

A convention for writing human-readable test specifications that AI agents can execute.

---

## Table of Contents

1. [Why TESTME.md?](#why-testmemd)
2. [Quick Example](#quick-example)
3. [File Format](#file-format)
4. [Section Reference](#section-reference)
5. [Examples by Pattern](#examples-by-pattern)
6. [For AI Agents](#for-ai-agents)
7. [Best Practices](#best-practices)

---

## Why TESTME.md?

### The Problem

Traditional test files are:

- **Verbose** — Simple assertions buried in boilerplate
- **Framework-dependent** — Tied to specific testing libraries
- **Hard to read** — Require developer knowledge to understand intent
- **Brittle** — Tightly coupled to implementation details

### The Solution

TESTME.md files are:

- **Human-readable** — Plain Markdown anyone can understand
- **Framework-agnostic** — No dependencies, any agent can execute
- **Intent-focused** — Describe what to test, not how to test it
- **Maintainable** — Easy to update as requirements change

### Benefits

| Audience | Benefit |
|----------|---------|
| **Developers** | Write tests faster, focus on intent |
| **QA Engineers** | Review and write tests without code expertise |
| **AI Agents** | Execute tests without framework knowledge |
| **Product Managers** | Understand test coverage at a glance |

---

## Quick Example

### Before — 47 lines of TypeScript

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

  test('redirects to dashboard with correct credentials', async ({ page }) => {
    await page.goto('/login');
    await page.locator('#email').fill(process.env.ADMIN_EMAIL);
    await page.locator('#password').fill(process.env.ADMIN_PASSWORD);
    await page.locator('button[type="submit"]').click();
    await expect(page).toHaveURL(/\/dashboard/);
  });
});
```

### After — Clear, readable Markdown

```markdown
### Test: Wrong credentials show error

1. Go to `/login`
2. Enter email "wrong@example.com" and password "wrongpassword"
3. Click submit

**Expect:** Error message is visible

---

### Test: Correct credentials redirect to dashboard

1. Go to `/login`
2. Enter email from `ADMIN_EMAIL` env var
3. Enter password from `ADMIN_PASSWORD` env var
4. Click submit

**Expect:** URL contains "/dashboard"
```

---

## File Format

### Filename Conventions

| Pattern | Use Case |
|---------|----------|
| `TESTME.md` | Primary test file for a directory |
| `*.testme.md` | Additional test files (e.g., `auth.testme.md`) |

### File Discovery

AI agents should search for test files in this order:

1. Repository root
2. `tests/` or `test/` directories
3. `e2e/` directory
4. Component or feature directories
5. Any `*.testme.md` files

### Document Structure

A TESTME.md file consists of these sections (all optional except Tests):

```
┌─────────────────────────────────────┐
│ # TESTME: [Suite Name]              │  ← Header
│ Brief description                   │
├─────────────────────────────────────┤
│ ## Prerequisites                    │  ← System requirements
├─────────────────────────────────────┤
│ ## Setup                            │  ← Environment preparation
├─────────────────────────────────────┤
│ ## Environment                      │  ← Variables and config
├─────────────────────────────────────┤
│ ## Tests                            │  ← Test cases (required)
│   ### Test: [Name]                  │
│   ### Test: [Name]                  │
├─────────────────────────────────────┤
│ ## Teardown                         │  ← Cleanup steps
└─────────────────────────────────────┘
```

---

## Section Reference

### 1. Header

The document title and description.

```markdown
# TESTME: User Authentication

End-to-end tests for login, registration, and password reset flows.
```

### 2. Prerequisites

System requirements the agent must verify before running tests.

```markdown
## Prerequisites

- Docker is installed and running
- Port 5432 is available
- File `.env` exists in project root
- Node.js version 18 or higher
```

**Format:** Bulleted list of verifiable conditions.

### 3. Setup

Steps to prepare the environment before running tests.

```markdown
## Setup

1. Copy `env.example` to `.env` if it doesn't exist
2. Run `docker compose up -d`
3. Wait for `http://localhost/health` to return 200
4. Run database migrations: `npm run db:migrate`
```

**Format:** Numbered list of sequential steps.

### 4. Environment

Environment variables and configuration used by tests.

```markdown
## Environment

| Variable | Default | Description |
|----------|---------|-------------|
| BASE_URL | http://localhost:3000 | Application URL |
| ADMIN_EMAIL | admin@example.com | Test admin account |
| ADMIN_PASSWORD | password123 | Admin password |
| TEST_TIMEOUT | 30000 | Default timeout in ms |
```

**Format:** Markdown table with Variable, Default, and Description columns.

### 5. Tests

The core test cases. Each test has a name, steps, and expectations.

```markdown
## Tests

### Test: User can log in with valid credentials

1. Navigate to `/login`
2. Enter email "user@example.com"
3. Enter password "validpassword"
4. Click the "Sign In" button

**Expect:**
- URL changes to `/dashboard`
- Welcome message displays user's name
```

**Format:**

- Test name as H3 heading with `Test:` prefix
- Steps as numbered list or descriptive prose
- Expectations after `**Expect:**` as single line or bulleted list

### 6. Teardown

Cleanup steps to run after all tests complete.

```markdown
## Teardown

1. Run `docker compose down`
2. Delete test database
3. Clear temporary files
```

**Format:** Numbered list of sequential steps.

---

## Examples by Pattern

### Pattern: Form Validation

Testing input validation and error messages.

#### TypeScript (verbose)

```typescript
test('shows error when password is too short', async ({ page }) => {
  await page.goto('/register');
  await page.locator('#firstName').fill('Test');
  await page.locator('#lastName').fill('User');
  await page.locator('#email').fill(`test-${Date.now()}@example.com`);
  await page.locator('#password').fill('short');
  await page.locator('#confirmPassword').fill('short');
  await page.locator('button[type="submit"]').click();
  await expect(page.locator('.error-message')).toBeVisible();
  await expect(page.locator('.error-message')).toHaveText(
    'Password must be at least 8 characters long'
  );
});
```

#### TESTME.md (clear)

```markdown
### Test: Short password shows validation error

1. Navigate to `/register`
2. Fill the registration form:
   - First Name: "Test"
   - Last Name: "User"
   - Email: (any unique email)
   - Password: "short"
   - Confirm Password: "short"
3. Click submit

**Expect:**
- Error message is visible
- Error says "Password must be at least 8 characters long"
```

---

### Pattern: Authentication Required

Testing protected routes and redirects.

#### TypeScript

```typescript
test('dashboard redirects to login when not authenticated', async ({ page }) => {
  await page.goto('/dashboard');
  await expect(page).toHaveURL(/\/login/);
});

test('settings redirects to login when not authenticated', async ({ page }) => {
  await page.goto('/dashboard/settings');
  await expect(page).toHaveURL(/\/login/);
});

test('profile redirects to login when not authenticated', async ({ page }) => {
  await page.goto('/dashboard/profile');
  await expect(page).toHaveURL(/\/login/);
});
```

#### TESTME.md

```markdown
### Test: Protected routes redirect to login

The following pages should redirect unauthenticated users to `/login`:

- `/dashboard`
- `/dashboard/settings`
- `/dashboard/profile`

For each URL: navigate to it without being logged in.

**Expect:** URL changes to contain "/login"
```

---

### Pattern: CRUD Operations

Testing create, read, update, delete workflows.

#### TypeScript (60+ lines)

```typescript
test('create item and verify it appears', async ({ page }) => {
  await page.goto('/dashboard/items');
  const itemName = `Test Item ${Date.now()}`;
  await page.getByRole('button', { name: /Add New/i }).click();
  await page.locator('#itemName').fill(itemName);
  await page.getByRole('button', { name: /Create/i }).click();
  await expect(page.locator('#itemName')).not.toBeVisible({ timeout: 5000 });
  await expect(page.locator('table').getByText(itemName)).toBeVisible();
});

test('delete item removes it from list', async ({ page }) => {
  // ... setup and create item first ...
  await page.locator(`tr:has-text("${itemName}") button.delete`).click();
  await page.getByRole('button', { name: /Confirm/i }).click();
  await expect(page.locator('table').getByText(itemName)).not.toBeVisible();
});
```

#### TESTME.md

```markdown
### Test: Create item and verify it appears

1. Go to `/dashboard/items`
2. Click "Add New" button
3. Enter a unique item name
4. Click "Create"

**Expect:**
- Modal/form closes
- New item appears in the table

---

### Test: Delete item removes it from list

1. Create a new item (as above)
2. Find the item's row in the table
3. Click "Delete" button
4. Confirm the deletion dialog

**Expect:** Item no longer appears in the table
```

---

### Pattern: Async Operations

Testing long-running processes with waits.

#### TypeScript

```typescript
test('shows success after processing', async ({ page }) => {
  test.setTimeout(300000); // 5 minutes
  await page.goto('/dashboard/process');
  await page.locator('#input').fill('test data');
  await page.getByRole('button', { name: /Process/i }).click();
  await waitForProcessingComplete(page, 270000);
  await expect(page.locator('text=Processing')).not.toBeVisible();
  await expect(page.locator('.status-complete')).toBeVisible();
});
```

#### TESTME.md

```markdown
### Test: Processing completes successfully

> **Note:** This test may take up to 5 minutes.

1. Go to `/dashboard/process`
2. Enter test data in the input field
3. Click "Process" button
4. Wait for processing to complete (the "Processing" indicator disappears)

**Expect:**
- No "Processing" status visible
- Item shows completed state
```

---

### Pattern: Environment-Dependent Behavior

Testing with configurable credentials or settings.

#### TypeScript

```typescript
const TEST_ADMIN_EMAIL = process.env.ADMIN_EMAIL || 'admin@example.com';
const TEST_ADMIN_PASSWORD = process.env.ADMIN_PASSWORD || 'password123';

test('login with admin credentials', async ({ page }) => {
  await page.goto('/login');
  await page.locator('#email').fill(TEST_ADMIN_EMAIL);
  await page.locator('#password').fill(TEST_ADMIN_PASSWORD);
  await page.locator('button[type="submit"]').click();
  await expect(page).toHaveURL(/\/dashboard/);
});
```

#### TESTME.md

```markdown
## Environment

| Variable | Default | Description |
|----------|---------|-------------|
| ADMIN_EMAIL | admin@example.com | Admin test account |
| ADMIN_PASSWORD | password123 | Admin password |

---

### Test: Login with admin credentials

1. Go to `/login`
2. Enter the admin email (from `ADMIN_EMAIL` env var)
3. Enter the admin password (from `ADMIN_PASSWORD` env var)
4. Click submit

**Expect:** Redirected to dashboard
```

---

### Pattern: Data Table Verification

Testing table content and sorting.

#### TESTME.md

```markdown
### Test: Table displays correct data

1. Navigate to `/users`
2. Wait for the table to load

**Expect:**
- Table has columns: Name, Email, Role, Status
- At least one row is visible
- Each row has data in all columns

---

### Test: Table sorting works

1. Navigate to `/users`
2. Click the "Name" column header

**Expect:** Rows are sorted alphabetically by name

3. Click the "Name" column header again

**Expect:** Rows are sorted in reverse alphabetical order
```

---

### Pattern: Modal Interactions

Testing dialog and modal workflows.

#### TESTME.md

```markdown
### Test: Confirmation modal prevents accidental deletion

1. Go to `/items`
2. Click "Delete" on any item
3. A confirmation modal appears
4. Click "Cancel"

**Expect:**
- Modal closes
- Item is still in the list

---

### Test: Confirmation modal allows deletion

1. Go to `/items`
2. Click "Delete" on any item
3. Note the item's name
4. Click "Confirm" in the modal

**Expect:**
- Modal closes
- Item is removed from the list
```

---

## For AI Agents

### Discovery Algorithm

```
1. Search for TESTME.md in repository root
2. Search for TESTME.md in tests/, test/, e2e/, spec/ directories
3. Search recursively for *.testme.md files
4. Parse and execute each file found
```

### Execution Flow

```
┌─────────────────────────────────────┐
│ 1. Parse TESTME.md file             │
├─────────────────────────────────────┤
│ 2. Verify Prerequisites             │
│    - Check each requirement         │
│    - Abort if any fail              │
├─────────────────────────────────────┤
│ 3. Execute Setup                    │
│    - Run steps in order             │
│    - Wait for completion            │
├─────────────────────────────────────┤
│ 4. Run Tests                        │
│    - Execute each test              │
│    - Record pass/fail               │
│    - Continue on failure            │
├─────────────────────────────────────┤
│ 5. Execute Teardown                 │
│    - Run cleanup steps              │
│    - Run even if tests failed       │
├─────────────────────────────────────┤
│ 6. Report Results                   │
└─────────────────────────────────────┘
```

### Handling Ambiguity

When steps are unclear, agents should:

1. **Make reasonable assumptions** based on context
2. **Document assumptions** in the test report
3. **Use common patterns** (e.g., "click submit" means click the submit button)
4. **Prefer explicit selectors** when available in the step

### Step Interpretation Guide

| Step Phrase | Interpretation |
|-------------|----------------|
| "Go to `/path`" | Navigate to the URL |
| "Click [button name]" | Find and click element with that text/label |
| "Enter [value]" | Type the value into the focused or specified input |
| "Wait for [element]" | Wait until element is visible |
| "Fill the form" | Enter values into form fields |
| "(any unique value)" | Generate a unique value (e.g., timestamp) |
| "from `VAR` env var" | Read value from environment variable |

### Result Reporting Format

Agents should report results in a clear, consistent format:

```
TESTME Results: [Suite Name]
=====================================
PASS: [Test Name]
PASS: [Test Name]
FAIL: [Test Name]
      Step: [Which step failed]
      Expected: [What was expected]
      Actual: [What happened]
      Screenshot: [path if available]

Summary: X tests, Y passed, Z failed
Duration: [time]
```

### Error Handling

| Scenario | Action |
|----------|--------|
| Prerequisite fails | Abort with clear message |
| Setup step fails | Abort, run teardown |
| Test step fails | Record failure, continue to next test |
| Assertion fails | Record failure, continue to next test |
| Teardown fails | Log warning, complete report |

---

## Best Practices

### Writing Clear Steps

**Do:**

- "Click the blue 'Submit' button"
- "Enter 'test@example.com' in the Email field"
- "Wait for the success message to appear"

**Don't:**

- "Click button"
- "Enter email"
- "Wait"

### Using Unique Test Data

**Do:**

```markdown
3. Enter a unique email (e.g., `test-{timestamp}@example.com`)
```

**Don't:**

```markdown
3. Enter "test@example.com"
```

### Noting Timing Requirements

**Do:**

```markdown
### Test: Large file upload completes

> **Note:** This test may take 2-3 minutes for large files.

1. Select a file larger than 100MB
...
```

### Keeping Tests Independent

Each test should:

- Set up its own preconditions
- Not depend on other tests running first
- Clean up after itself if necessary

**Do:**

```markdown
### Test: Edit user profile

1. Log in as test user
2. Navigate to profile settings
3. Update the display name
...
```

**Don't:**

```markdown
### Test: Edit user profile

(Assumes previous login test passed)
1. Navigate to profile settings
...
```

### Documenting Environment Variables

Always list variables with defaults:

```markdown
## Environment

| Variable | Default | Description |
|----------|---------|-------------|
| API_URL | http://localhost:3000 | Backend API URL |
| TEST_USER | testuser@example.com | Standard test account |
| TIMEOUT | 30000 | Default timeout in milliseconds |
```

### Grouping Related Tests

Use horizontal rules to separate test groups:

```markdown
## Tests

### Authentication

### Test: Valid login succeeds
...

### Test: Invalid login shows error
...

---

### Profile Management

### Test: User can update name
...

### Test: User can change password
...
```

---

## Version History

| Version | Date | Changes |
|---------|------|---------|
| 1.0 | 2024 | Initial specification |

---

## Contributing

This specification is open for community input. Suggest improvements by:

1. Opening an issue with your proposal
2. Providing real-world examples
3. Sharing implementation feedback
