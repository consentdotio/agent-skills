---
name: tsdoc-jsdoc-authoring
description: Write and review API documentation comments using TSDoc and JSDoc best practices. Use when the user asks for docs, doc comments, TSDoc, JSDoc, @param/@returns help, or documentation quality improvements in JavaScript or TypeScript code.
metadata:
  author: custom
  version: "1.0.0"
  argument-hint: <file-or-pattern>
---

# TSDoc and JSDoc Authoring

Create high-quality documentation comments for functions, classes, interfaces, types, modules, and exported APIs.

## When to Apply

Use this skill when the user asks to:
- add or improve code comments/docs
- write TSDoc or JSDoc
- document params, return values, thrown errors, or examples
- standardize documentation style across files

## Rule 1: Choose the Correct Standard

- Use **TSDoc** for TypeScript code and typed public APIs.
- Use **JSDoc** for JavaScript code or projects using JSDoc tooling.
- Do not mix tag syntaxes from both standards in one comment block.

## Workflow

1. Read the target file and identify the symbol's purpose and side effects.
2. Prefer documenting exported/public APIs first.
3. Write a one-line summary in plain language.
4. Add only meaningful tags (no empty or redundant tags).
5. Validate tag names and formatting against the relevant standard.
6. Ensure comments match actual behavior (inputs, outputs, errors, async behavior).

## Authoring Rules (Both Standards)

- Describe intent and behavior, not obvious implementation details.
- Keep summaries concise and action-oriented ("Returns...", "Creates...", "Parses...").
- Document every parameter and explain what each parameter does.
- If a parameter is an object, document each object property and what it does.
- Use imperative, consistent phrasing for `@param` descriptions.
- Document thrown errors with `@throws` when behavior depends on error handling.
- Add `@example` only when it clarifies non-obvious usage.
- Do not restate TypeScript types in prose unless it adds semantic meaning.

## TSDoc Rules

Use these tags by default when relevant:
- `@param` for each function argument
- `@typeParam` for generic parameters
- `@returns` for return semantics
- `@remarks` for longer contextual details
- `@throws` for exceptional behavior
- `@example` for practical usage
- `@deprecated` for migration guidance

TSDoc formatting reminders:
- Use `@param name - Description` with a hyphen separator.
- Prefer inline links with `{@link SymbolName}`.
- Keep release tags (`@alpha`, `@beta`, `@public`, `@internal`) aligned with project policy.
- For object parameters in TypeScript, prefer a named `type` or `interface` and add `/** ... */` comments on each property so VS Code hovers/autocomplete show per-property docs.

## JSDoc Rules

Use these tags by default when relevant:
- `@param {Type} name` (with optional/default forms when needed)
- `@returns {Type}` (or `@return`)
- `@throws {Type}` when known
- `@typedef` and `@property` for reusable object shapes
- `@example` for usage snippets
- `@async` and `@yields` for async/generator behavior when needed

JSDoc formatting reminders:
- Optional params: `@param {string} [name]`
- Optional with default: `@param {string} [name=John Doe]`
- Nested object props: `@param {Object} options` and `@param {string} options.mode`

## Output Expectations

When updating comments:
- keep existing project conventions unless user requests a migration
- preserve behavior accuracy over verbosity
- avoid adding comments to private/internal symbols unless requested

## Additional Reference

For templates and tag cheat sheets, see [reference.md](reference.md).

## Rule Files

Read focused rule files in `rules/` for concrete "incorrect vs correct" patterns:

- Core contract: `tsdoc-summary`, `tsdoc-remarks`, `tsdoc-param`, `tsdoc-typeparam`, `tsdoc-returns`, `tsdoc-throws`, `tsdoc-example`, `tsdoc-deprecated`
- Cross-reference/reuse: `tsdoc-link`, `tsdoc-see`, `tsdoc-inheritdoc`, `tsdoc-label`
- Package/internal: `tsdoc-package-documentation`, `tsdoc-private-remarks`, `tsdoc-default-value`
- Policy: `tsdoc-release-tags`, `tsdoc-modifier-tags`, `tsdoc-no-jsdoc-braces`
- JSDoc core: `jsdoc-summary`, `jsdoc-param`, `jsdoc-optional-default`, `jsdoc-property-namepaths`, `jsdoc-returns`, `jsdoc-throws`, `jsdoc-example`
- JSDoc advanced: `jsdoc-typedef-property`, `jsdoc-async`, `jsdoc-yields`, `jsdoc-class-constructor`, `jsdoc-module`

## Full Compiled Document

For the full rule index and category guide: `AGENTS.md`
