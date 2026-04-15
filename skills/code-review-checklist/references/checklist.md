# Code Review Checklist

Reference checklist used by the `code-review-checklist` skill. Each item is evaluated against the diff and marked Pass / Fail / Warning / N/A.

---

## Security

- [ ] No secrets, API keys, tokens, or passwords hardcoded or committed
- [ ] User input is validated and sanitized before use (especially in SQL, shell commands, file paths)
- [ ] Authentication and authorization checks are present on all protected routes and actions
- [ ] No use of `eval()`, `dangerouslySetInnerHTML`, or similar unsafe patterns without explicit justification
- [ ] Sensitive data (PII, passwords, tokens) is not logged or exposed in error messages
- [ ] CORS configuration is intentional and not set to `*` on sensitive endpoints
- [ ] Dependencies added are well-maintained and have no known critical CVEs
- [ ] SQL queries use parameterized statements or ORM-generated queries (no string concatenation into queries)
- [ ] File uploads validate type, size, and destination path

## Error Handling

- [ ] Errors are caught at appropriate boundaries and not silently swallowed
- [ ] `catch` blocks handle the error meaningfully (log, rethrow, or return error response) — no empty catch blocks
- [ ] API routes return appropriate HTTP status codes for error cases (400 for bad input, 401 for unauth, 404 for not found, 500 for server error)
- [ ] Async errors in Promise chains and async/await functions are properly handled
- [ ] External service failures (APIs, DB) are handled gracefully with useful error messages
- [ ] Error messages shown to users do not leak internal implementation details

## Performance

- [ ] No N+1 query patterns (loops containing database calls)
- [ ] Expensive computations are not run on every render or every request when they could be cached or memoized
- [ ] Large data sets are paginated, not fetched in full
- [ ] Images and assets use appropriate formats and are not unnecessarily large
- [ ] Database queries select only the columns needed (avoid `SELECT *` on wide tables)
- [ ] No blocking synchronous operations in async contexts (e.g., `fs.readFileSync` in a request handler)
- [ ] React components avoid unnecessary re-renders (stable references for callbacks and objects passed as props)

## Correctness

- [ ] Business logic matches the stated requirements or ticket description
- [ ] Edge cases are handled: empty arrays, null/undefined inputs, zero values, maximum/minimum bounds
- [ ] Off-by-one errors checked in loops and array indexing
- [ ] Conditional logic is not inverted or missing branches
- [ ] State mutations do not affect shared or external objects unintentionally
- [ ] Date and time handling accounts for timezones where relevant
- [ ] Numeric operations avoid floating-point pitfalls for currency or precision-sensitive values

## Types and Interfaces (TypeScript)

- [ ] No use of `any` without justification; prefer `unknown` and narrow properly
- [ ] New data shapes have explicit TypeScript interfaces or types defined
- [ ] Return types are declared on exported functions
- [ ] Non-null assertions (`!`) are used only where nullability is genuinely impossible
- [ ] `as` casts have a comment explaining why the cast is safe

## Tests

- [ ] New logic has corresponding unit or integration tests
- [ ] Tests cover the happy path, at least one error/edge case, and boundary conditions
- [ ] Existing tests are not deleted without a clear reason
- [ ] Tests do not depend on execution order or external state
- [ ] Mocks and stubs are reset between tests
- [ ] Test descriptions accurately describe what is being tested

## Readability and Maintainability

- [ ] Functions and variables have clear, descriptive names (no single-letter names outside trivial loops)
- [ ] Functions do one thing and are not longer than ~50 lines without good reason
- [ ] Complex logic has explanatory comments (the "why", not the "what")
- [ ] No dead code, commented-out blocks, or TODO comments left without a linked issue
- [ ] Magic numbers and strings are extracted into named constants
- [ ] File and module organization follows the project's established conventions

## Logging and Observability

- [ ] Significant operations and errors are logged at appropriate levels (info, warn, error)
- [ ] Log messages include enough context to diagnose issues (request ID, user ID where appropriate)
- [ ] No `console.log` debug statements left in production code
- [ ] Structured logging is used where the project has established it

## Dependencies

- [ ] New packages are justified (does the project already have a library that covers this?)
- [ ] Packages are pinned to a specific version or version range that is not overly permissive
- [ ] Dev dependencies are not added to production dependencies and vice versa
- [ ] Lock file (package-lock.json / yarn.lock / pnpm-lock.yaml) is updated and committed

## Documentation

- [ ] Public functions, classes, and modules exported from a package have JSDoc/TSDoc comments
- [ ] README or relevant docs are updated if the change adds a new feature, changes a config option, or modifies a public interface
- [ ] Environment variables introduced are documented (in .env.example or equivalent)
- [ ] Breaking changes are clearly noted in commit message, PR description, or CHANGELOG
