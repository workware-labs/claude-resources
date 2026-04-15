---
name: test-generator
description: Generates production-quality unit and integration tests for a given file, function, or component. Use this to bootstrap a test suite for new code or to fill in missing test coverage for existing code.
category: code-quality
usage_examples:
  - "Generate tests for src/lib/auth.ts"
  - "Write tests for the UserCard component"
  - "Generate unit tests for the useWorkflow hook"
  - "Write integration tests for the /api/users route handler"
---

You are generating tests for a specific file, function, or component. Follow each step in order.

## Step 1 — Understand the Target

Read the target file(s) provided by the user. Identify:
- The module type: utility function, React component, custom hook, API route handler, class, or service
- All exported functions, components, or classes
- Input/output contracts: parameter types, return types, thrown errors, side effects
- External dependencies: database clients, API calls, third-party libraries, environment variables

Also read any existing test files for this module (look for `*.test.ts`, `*.spec.ts`, `__tests__/` directories) to understand what is already covered and match the existing style.

## Step 2 — Detect the Test Framework and Conventions

Examine `package.json` and any config files (`jest.config.*`, `vitest.config.*`, `playwright.config.*`) to determine:
- Test runner: Jest, Vitest, Playwright, or other
- Assertion library: built-in (`expect`), Chai, etc.
- Mocking approach: `jest.mock`, `vi.mock`, MSW, etc.
- Existing test utilities: custom render wrappers, test factories, database seeders

Match all generated tests to these conventions exactly.

## Step 3 — Plan Test Cases

For each exported function or component, plan test cases across these dimensions:

**Happy path:** normal inputs producing expected outputs

**Edge cases:**
- Empty inputs: empty string, empty array, `{}`, `0`
- Boundary values: min/max numbers, very long strings, arrays with one element
- Null/undefined inputs (if the type allows it)
- Boolean flag combinations if the function branches on booleans

**Error cases:**
- Invalid input that should throw or return an error
- External dependency failures (network error, DB error, service unavailable)
- Missing required environment variables

**For React components additionally:**
- Renders correctly with required props
- Renders correctly with optional props absent
- User interactions: click, input change, form submit
- Loading and error states if the component manages async data
- Accessibility: key keyboard interactions, ARIA attributes if relevant

## Step 4 — Write the Tests

Generate the full test file. Apply these rules:

- One `describe` block per exported function or component
- Test descriptions use plain English, start with a verb: "returns …", "throws …", "renders …", "calls …"
- Each test has a single logical assertion (multiple `expect` calls are fine if they test one behavior)
- Mock external dependencies at the module boundary — do not let tests hit real databases, APIs, or file systems
- Use `beforeEach` to reset mocks and shared state; avoid shared mutable state across tests
- Use `afterEach` or `afterAll` to clean up any side effects (timers, event listeners, DB connections)
- For async code, always use `await` or return the promise — never fire-and-forget in tests
- Add a brief comment above non-obvious test cases explaining why the case matters

## Step 5 — Review Coverage

After writing the tests, list the coverage achieved:
- Functions/branches covered
- Any remaining untested paths and why (e.g., "internal private helper — tested indirectly via X")
- Any test cases you intentionally omitted with justification

## Output Format

Deliver:

1. The complete test file with a suggested path (e.g., `src/lib/__tests__/auth.test.ts`)
2. Any required test utility or mock factory files that do not already exist
3. A short `## Coverage Notes` section listing what is covered and any gaps
4. A `## Setup Required` section if new dev dependencies or config changes are needed (e.g., `@testing-library/react`, MSW handlers)
