# CodePath AI301 — Contribution README

## Status
Phase II Complete

## Issue
- **Repository:** asgardeo/javascript
- **Issue #:** [#170](https://github.com/asgardeo/javascript/issues/170)
- **Issue Title:** chore: add unit test coverage for untested code paths in @asgardeo/vue SDK

## Why I Chose This Issue
I chose issue #170 because the @asgardeo/vue SDK had utility functions with zero test coverage. 
It was labeled "good first issue" and matched my goal of learning how real open source testing works. 
The scope was clear — find untested code, write tests, submit a PR. I also wanted experience 
contributing to a real TypeScript/Vue project.

## Understanding the Issue
The `@asgardeo/vue` package had several utility functions that had never been tested. 
This means bugs in those functions could go undetected. The fix is to add unit tests 
that cover all the main code paths and edge cases.

## Reproduction Process
1. Forked asgardeo/javascript repository
2. Opened GitHub Codespaces
3. Created branch: `test-coverage-vue-sdk`
4. Ran `pnpm install` to install dependencies
5. Ran `pnpm nx test @asgardeo/vue` to confirm existing tests
6. Found that `src/utils/` had zero test coverage

## Solution Approach
Added unit tests for the `getDisplayName` utility function which had no tests at all. 
Wrote 6 tests covering all code paths including fallback behavior.

## Testing Strategy
- Mocked the `getMappedUserProfileValue` dependency
- Tested each fallback case individually
- All 6 tests pass successfully

## Pull Request
[PR #530](https://github.com/asgardeo/javascript/pull/530)
