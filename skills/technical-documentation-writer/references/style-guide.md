# Technical Documentation Style Guide

This guide defines writing conventions for all documentation produced by the `technical-documentation-writer` skill. Apply every rule consistently.

---

## Voice and Tone

- **Use active voice.** Prefer "The function returns an array" over "An array is returned by the function."
- **Use second person** ("you") when addressing the reader in guides and READMEs. Use third person for module references that describe what code does.
- **Be direct.** Start sentences with the subject. Avoid filler phrases like "It is important to note that…" or "Please be aware that…"
- **Present tense** for describing how code behaves. "The hook returns a loading state" not "The hook will return a loading state."
- **Imperative mood** for steps and instructions. "Run `npm install`" not "You should run `npm install`."

## Structure

- Use **H2 (`##`)** for top-level sections. Never use H1 inside a document body — the document title is H1.
- Use **H3 (`###`)** for sub-sections. Avoid H4 unless the hierarchy genuinely requires it.
- Lead each section with 1–2 sentences that state what the section covers before diving into details.
- Use **numbered lists** for sequential steps. Use **bullet lists** for non-ordered items.
- Keep lists parallel: if the first item starts with a verb, all items start with a verb.

## Code Examples

- Every function, hook, or API endpoint documented must include at least one realistic code example.
- Code blocks must specify the language: ` ```ts `, ` ```bash `, ` ```json `, etc.
- Examples must be self-contained enough that a reader can copy and run them with minimal changes.
- Show the import statement at the top of TypeScript/JavaScript examples.
- Use realistic variable names in examples — not `foo`, `bar`, `test123`.
- Highlight the most important line with a comment if the example is long.

## Parameters and Return Values

- Document parameters in a Markdown table with columns: **Name | Type | Required | Default | Description**
- Mark required parameters clearly. Do not omit defaults.
- For TypeScript, use the actual TypeScript type (e.g. `string | null`, `User[]`) not prose descriptions.
- Document thrown errors in a separate **Throws** subsection: **Error Type | Condition**

## Formatting Conventions

- Use backtick inline code for: function names, variable names, file paths, environment variables, HTTP methods, status codes, and any literal value the reader might type.
- Use **bold** to introduce a term the first time it appears or to call out a key concept.
- Use *italics* sparingly — only for emphasis where bold would be too strong.
- Use `>` blockquote for important notes or warnings, preceded by **Note:** or **Warning:** in bold.
- Use a horizontal rule (`---`) to separate major logical groupings only if headers alone are insufficient.

## Environment Variables

- Document every environment variable in a table: **Variable | Required | Default | Description | Example Value**
- Never include real secrets or production values in examples. Use placeholder values like `your-api-key-here`.
- Note which variables are required at build time vs. runtime.

## Versioning

- Note the version or date the documentation was written for when it may change.
- If documenting a breaking change, use a prominent **Breaking Change in vX.Y** callout.

## What to Avoid

- Do not repeat the code in prose. If you have a code block, don't also write "This code imports X, then calls Y, then returns Z."
- Do not document implementation details that are internal and not part of the public interface.
- Do not use jargon without defining it on first use.
- Do not write documentation for code that does not yet exist — only document what is currently in the codebase.
- Do not pad sections with obvious statements. If a parameter named `userId` is a `string` representing a user's ID, "A string representing the user's ID" adds no value.
