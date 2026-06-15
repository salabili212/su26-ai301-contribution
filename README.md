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
The `@asgardeo/vue` package had a utility function called `getDisplayName` in 
`packages/vue/src/utils/getDisplayName.ts` with zero unit tests. This means bugs in that 
function could go undetected. The fix was to add unit tests covering all main code paths 
and edge cases.

## Reproduction Process

### Environment Setup
Used GitHub Codespaces to avoid local setup entirely. Ran `pnpm install` to install all 
dependencies, then `pnpm nx test @asgardeo/vue` to confirm the existing test suite ran. 
No major setup issues — Codespaces had the correct Node version pre-configured.

### Steps to Reproduce
1. Fork the `asgardeo/javascript` repository
2. Open the project in GitHub Codespaces
3. Run `pnpm install` to install all dependencies
4. Run `pnpm nx test @asgardeo/vue` to run the existing test suite
5. Navigate to `packages/vue/src/utils/` in the file explorer
6. **Expected:** Unit test files exist for all utility functions
7. **Actual:** No test file exists for `getDisplayName.ts` — zero coverage confirmed

### Branch Link
[https://github.com/salabili212/javascript/tree/test-coverage-vue-sdk](https://github.com/salabili212/javascript/tree/test-coverage-vue-sdk)

## Solution Approach

### Implementation Plan

**Understand:**
The `@asgardeo/vue` package contained a utility function `getDisplayName` in 
`packages/vue/src/utils/getDisplayName.ts` with zero unit tests. This function builds 
a display name for a logged-in user by checking fields like firstName, lastName, username, 
and email in order — but none of those code paths were tested. A bug could be silently 
introduced with no automated detection.

**Match:**
Other packages in the repo follow the pattern of placing tests in 
`packages/<name>/src/__tests__/utils/`. I followed this same structure and created 
`packages/vue/src/__tests__/utils/getDisplayName.test.ts`.

**Plan:**
1. Create the test file at `packages/vue/src/__tests__/utils/getDisplayName.test.ts`
2. Mock the `getMappedUserProfileValue` dependency using `vi.mock()` from Vitest so 
   tests run in isolation without needing real user data
3. Write tests for all 6 code paths:
   - Returns `'User'` when no attributes exist (default fallback)
   - Returns `'Jane Doe'` when firstName and lastName are both present
   - Returns username when firstName/lastName are missing
   - Returns email when firstName/lastName/username are all missing
   - Returns `'User'` when displayAttributes is an empty array
   - Returns correct value when displayAttributes is provided
4. Run `pnpm nx test @asgardeo/vue` to confirm all 6 tests pass

**Review:**
Followed the project's CONTRIBUTING.md — used the `test(vue):` commit message prefix, 
kept changes scoped to one new file only, and completed the full PR checklist including 
security checks.

**Evaluate:**
All 6 tests pass. PR #530 was submitted and passed all 5 automated CodeRabbit checks. 
CodeRabbit also flagged a missing test for the `name` fallback branch — a valid improvement 
that could be addressed in a follow-up.

## Pull Request
[PR #530 — test(vue): add unit tests for getDisplayName utility](https://github.com/asgardeo/javascript/pull/530)
