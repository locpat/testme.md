# Agent Instructions: Write TESTME.md

You are tasked with creating TESTME.md test specification files for this codebase.

## Specification Reference

Follow the TESTME.md specification:
https://github.com/evilsocket/testme.md/blob/main/SPECS.md

## Your Task

1. **Analyze the codebase** to understand:
   - What the application does
   - Existing test files (if any)
   - Key user flows and features
   - Environment requirements

2. **Create TESTME.md files** that cover:
   - Critical user journeys
   - Authentication flows
   - Core CRUD operations
   - Form validations
   - Edge cases and error handling

3. **Place files appropriately**:
   - `TESTME.md` in the repository root for high-level tests
   - `tests/TESTME.md` or `e2e/TESTME.md` for test directories
   - `*.testme.md` files for specific features or modules

## Analysis Steps

### Step 1: Discover Existing Tests

Look for existing test files to understand what's already tested:

```
- **/*.test.ts, **/*.spec.ts (TypeScript/JavaScript)
- **/*.test.js, **/*.spec.js
- **/test_*.py, **/*_test.py (Python)
- **/tests/, **/test/, **/e2e/, **/spec/
```

### Step 2: Identify Key Features

Examine the codebase for:

- Route definitions (pages, API endpoints)
- Authentication/authorization logic
- Form components and validation
- Database models (entities to CRUD)
- External integrations

### Step 3: Map User Flows

Identify the main user journeys:

- Onboarding (registration, first-time setup)
- Authentication (login, logout, password reset)
- Core workflows (what users primarily do)
- Settings and profile management
- Admin functions (if applicable)

## Writing Guidelines

### Structure Each TESTME.md File

```markdown
# TESTME: [Feature/Area Name]

Brief description of what these tests cover.

## Prerequisites
[Only if needed - system requirements]

## Setup
[Only if needed - preparation steps]

## Environment
[Only if needed - environment variables table]

## Tests

### Test: [Descriptive Name]
[Steps]
**Expect:** [Assertions]

## Teardown
[Only if needed - cleanup steps]
```

### Write Clear Test Cases

**Do:**
- Use specific, descriptive test names
- Write numbered steps that anyone can follow
- Include concrete values in examples
- Note timing for slow operations
- Reference environment variables for secrets

**Don't:**
- Duplicate implementation details from code
- Include framework-specific syntax
- Write overly technical steps
- Assume prior knowledge of the codebase

### Convert Existing Tests

If converting from existing test files:

| Original Code | TESTME.md Equivalent |
|---------------|----------------------|
| `page.goto('/login')` | "Go to `/login`" |
| `page.locator('#email').fill('x')` | "Enter 'x' in the email field" |
| `page.click('button[type="submit"]')` | "Click the submit button" |
| `expect(page).toHaveURL(/dashboard/)` | "**Expect:** URL contains '/dashboard'" |
| `expect(el).toBeVisible()` | "**Expect:** [element] is visible" |
| `expect(el).toHaveText('X')` | "**Expect:** [element] shows 'X'" |

### Group Related Tests

Organize tests logically:

```markdown
## Tests

### Authentication

### Test: Login succeeds with valid credentials
...

### Test: Login fails with wrong password
...

---

### User Profile

### Test: User can update display name
...
```

## Output Checklist

Before finishing, verify:

- [ ] All critical user flows have test coverage
- [ ] Tests are independent (don't depend on each other)
- [ ] Environment variables are documented
- [ ] Setup/teardown included if needed
- [ ] Steps are clear enough for any agent to execute
- [ ] Expectations are specific and verifiable

## Example Output

For a typical web application, you might create:

```
project/
├── TESTME.md                    # Overview + critical paths
├── e2e/
│   ├── TESTME.md               # E2E test index
│   ├── auth.testme.md          # Authentication tests
│   └── checkout.testme.md      # Checkout flow tests
└── api/
    └── TESTME.md               # API endpoint tests
```

## Begin

Analyze this codebase now and create appropriate TESTME.md files following the specification and guidelines above.
