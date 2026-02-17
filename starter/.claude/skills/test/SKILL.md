---
name: test
description: Run tests and report results
user-invocable: true
context: fork
agent: tester
allowed-tools:
  - Read
  - Grep
  - Glob
  - Bash
---

# Test

Run the project's test suite and report results.

## Instructions

1. **Detect Test Framework**
   - Check package.json for test scripts
   - Identify test framework (Jest, Vitest, Mocha, etc.)
   - Note test configuration files

2. **Run Tests**
   - Execute the test command (usually `npm test`)
   - Capture output including pass/fail counts
   - Note execution time
   - Check for flaky tests (intermittent failures)

3. **Analyze Results**
   - Count passed vs failed tests
   - Identify failed test names and error messages
   - Note any warnings or deprecation notices
   - Check code coverage if available

4. **Report Findings**
   - Summarize test results clearly
   - Highlight failures with specific error messages
   - Suggest next steps for failures
   - Note any flaky or skipped tests

## Test Scope

$ARGUMENTS

## Output Format

Provide results in this format:

```
Test Results
============

Framework: [Jest/Vitest/etc.]
Total Tests: X
Passed: Y
Failed: Z
Skipped: W
Duration: Xs

Failed Tests:
[If any tests failed, list them with error messages]

Coverage:
[If available, show coverage percentages]

Flaky Tests:
[Any tests that sometimes pass/fail]

Summary:
[Brief assessment of test health]

Next Steps:
[Recommended actions]
```

## Completion Criteria

- Test suite has been executed
- Results are clearly reported
- Failed tests are identified with error messages
- Recommendations provided for failures
