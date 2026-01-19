# Agent Instructions: Execute TESTME.md Tests

You are tasked with finding and executing TESTME.md test specifications in this codebase.

## What is TESTME.md?

TESTME.md is a convention for writing human-readable test specifications that AI agents can execute. Tests are written in plain Markdown with clear steps and expectations.

**Project:** https://github.com/evilsocket/testme.md
**Specification:** https://github.com/evilsocket/testme.md/blob/main/SPECS.md

## Your Task

1. **Find all TESTME.md files** in the codebase
2. **Parse each file** according to the specification
3. **Execute the tests** following the steps described
4. **Report results** clearly

## Discovery

Search for test files in this order:

1. `TESTME.md` in repository root
2. `TESTME.md` in `tests/`, `test/`, `e2e/`, `spec/` directories
3. Any `*.testme.md` files recursively

## Execution Flow

For each TESTME.md file found:

```
1. Read and parse the file
2. Verify Prerequisites (abort if any fail)
3. Execute Setup steps in order
4. Run each Test:
   - Perform the steps
   - Verify expectations
   - Record pass/fail
5. Execute Teardown steps
6. Report results
```

## File Structure

A TESTME.md file contains these sections:

| Section | Required | Purpose |
|---------|----------|---------|
| Header | Yes | Suite name and description |
| Prerequisites | No | System requirements to verify |
| Setup | No | Environment preparation steps |
| Environment | No | Variables and configuration |
| Tests | Yes | Test cases with steps and expectations |
| Teardown | No | Cleanup steps |

## Interpreting Steps

| Step Phrase | Action |
|-------------|--------|
| "Go to `/path`" | Navigate to the URL |
| "Navigate to `/path`" | Navigate to the URL |
| "Click [button/link]" | Find and click the element |
| "Enter [value]" | Type into the focused or specified input |
| "Fill [field] with [value]" | Enter value in the named field |
| "Wait for [element/condition]" | Wait until condition is met |
| "Select [option]" | Choose from dropdown/select |
| "(any unique value)" | Generate a unique value (use timestamp) |
| "from `VAR` env var" | Read from environment variable |

## Interpreting Expectations

| Expectation | Verification |
|-------------|--------------|
| "is visible" | Element exists and is displayed |
| "is not visible" | Element hidden or not present |
| "URL contains X" | Current URL includes the string |
| "URL changes to X" | Navigation occurred to that path |
| "shows [text]" | Element contains the text |
| "error message appears" | Error UI is displayed |

## Handling Ambiguity

When steps are unclear:

1. Make reasonable assumptions based on context
2. Use common UI patterns (submit buttons, form fields, etc.)
3. Document any assumptions in your report
4. If truly blocked, note it and continue to next test

## Result Reporting

Report results in this format:

```
TESTME Results: [Suite Name]
=====================================
File: [path/to/TESTME.md]

Prerequisites: PASSED (or FAILED with details)
Setup: PASSED (or FAILED with details)

Tests:
  PASS: [Test Name]
  PASS: [Test Name]
  FAIL: [Test Name]
        Step: [Which step failed]
        Expected: [What was expected]
        Actual: [What happened]

Teardown: PASSED (or SKIPPED)

Summary: X tests, Y passed, Z failed
=====================================
```

## Error Handling

| Scenario | Action |
|----------|--------|
| Prerequisite fails | Report failure, skip this file |
| Setup fails | Report failure, run teardown, skip tests |
| Test step fails | Record failure, continue to next test |
| Assertion fails | Record failure, continue to next test |
| Teardown fails | Log warning, complete report |

## Environment Variables

If an Environment section exists, use the specified defaults when variables are not set:

```markdown
## Environment

| Variable | Default | Description |
|----------|---------|-------------|
| BASE_URL | http://localhost:3000 | App URL |
| ADMIN_EMAIL | admin@example.com | Admin account |
```

Use `http://localhost:3000` if `BASE_URL` is not set.

## Begin

Find all TESTME.md files in this codebase and execute them now. Report results for each file found.
