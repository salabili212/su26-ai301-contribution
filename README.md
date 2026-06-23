# CodePath AI301 — Contribution README

## Status
Phase III Complete

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
3. Write tests for all the code paths:
   - Returns `'User'` when no attributes exist (default fallback)
   - Returns `'Jane Doe'` when firstName and lastName are both present
   - Returns username when firstName/lastName are missing
   - Returns email when firstName/lastName/username are all missing
   - Returns name when firstName/lastName/username/email are all missing
   - Returns `'User'` when displayAttributes is an empty array
   - Returns correct value when displayAttributes is provided
4. Run `pnpm nx test @asgardeo/vue` to confirm all tests pass

**Review:**
Followed the project's CONTRIBUTING.md — used the `test(vue):` commit message prefix, 
kept changes scoped to the test file only, and completed the full PR checklist including 
security checks.

**Evaluate:**
All 7 tests pass. PR #530 was submitted and passed all 5 automated CodeRabbit checks. 
CodeRabbit also flagged a missing test for the `name` fallback branch, which I went back 
and added.

## Implementation Notes

### Week 3 Progress (June 9–23, 2026)
This week I finished the actual test file and got everything pushed and passing.

**What I built:**
- Created a new test file at `packages/vue/src/__tests__/utils/getDisplayName.test.ts`
- Wrote 7 unit tests covering every code path of the `getDisplayName` utility — the default 
  `'User'` fallback, the firstName + lastName combination, and the username, email, and name 
  fallbacks, plus the displayAttributes cases
- All 7 of my tests pass when I run `pnpm nx test @asgardeo/vue`

**Challenges faced:**
- I'd never used Vitest before, so I had to learn how `vi.mock()` and `mockReturnValueOnce` 
  work before I could test anything. The tricky part was mocking the `getMappedUserProfileValue` 
  dependency so my tests didn't need real user data — I figured it out by reading how other 
  test files in the repo did it.
- After my first push, CodeRabbit pointed out I was missing a test for the `name` fallback 
  branch. It was a fair catch, so I added that 7th test and pushed again.
- When I ran `pnpm install` locally, pnpm quietly added a `packageManager` line to 
  `package.json` that had nothing to do with my issue. I reverted that so my change would 
  stay focused on just the test file.

**Key commits:**
- `2d9ecad`: test(vue): add unit tests for getDisplayName utility
- `6b663e9`: test(vue): add name fallback coverage for getDisplayName
- `9749b93`: chore: revert unintended package.json change

## Code Changes
- **Active branch:** [test-coverage-vue-sdk](https://github.com/salabili212/javascript/tree/test-coverage-vue-sdk)
- **Pull Request:** [PR #530](https://github.com/asgardeo/javascript/pull/530)
- **File changed:** `packages/vue/src/__tests__/utils/getDisplayName.test.ts` (new file, 7 tests)

## Testing Strategy
I used Vitest, which is the framework the project already uses, and wrote isolated unit tests 
so each one checks a single code path. I mocked the `getMappedUserProfileValue` dependency with 
`vi.mock()` so the tests run without needing a real logged-in user, and I matched the repo's 
existing folder layout (`packages/<name>/src/__tests__/utils/`) so my file fits in with everything else.

After every change I ran `pnpm nx test @asgardeo/vue` to make sure nothing broke. All 7 tests in 
my file pass. There are 5 other test suites in the package that fail on my machine, but those 
fail because of an unrelated `@asgardeo/browser` build issue in the monorepo, not because of 
anything I changed — my test file loads and passes cleanly. On the PR itself, all 5 of CodeRabbit's 
automated checks passed.

## Pull Request
[PR #530 — test(vue): add unit tests for getDisplayName utility](https://github.com/asgardeo/javascript/pull/530)
