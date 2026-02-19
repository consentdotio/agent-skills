# TSDoc Deep Reference

This file is the detailed TSDoc authoring spec for this skill. Use it when generating or reviewing TypeScript API documentation comments.

## 1) TSDoc Comment Structure

Standard order for a high-quality TSDoc block:

1. Summary sentence(s)
2. Optional `@remarks` (long-form details)
3. Parameter docs (`@typeParam`, then `@param`)
4. Return docs (`@returns`) when applicable
5. Error docs (`@throws`) when behavior depends on error handling
6. Optional `@example`
7. Optional policy tags (`@deprecated`, release tags, modifiers)
8. Optional `@see`

Template:

```ts
/**
 * One-sentence summary in plain language.
 *
 * @remarks
 * Long-form context, constraints, caveats, and behavior notes.
 *
 * @typeParam T - Meaning of generic type.
 * @param input - What this value represents and constraints.
 * @returns What is returned and key semantics.
 * @throws ErrorType when and why this fails.
 * @example
 * ```ts
 * const value = fn(input)
 * ```
 * @deprecated Use `newFn()` instead.
 */
```

## 2) TSDoc Tag Kinds (Syntax Types)

TSDoc tags are grouped into three syntax kinds:

- `InlineTag`: used inside prose (example: `{@link Foo}`)
- `BlockTag`: starts a new section block (example: `@remarks`)
- `ModifierTag`: marker-style tags with no body (example: `@public`)

Use the right syntax kind for the right purpose. Do not write modifier tags as prose blocks.

## 3) Complete Standard Tag Catalog

Below is the full standard tag set and how to use each tag.

### 3.1 Block Tags

#### `@param`

- Use when: documenting each function/method parameter.
- Format: `@param name - Description`
- Good for: meaning, allowed range, units, nullability semantics, side effects.
- Avoid: repeating obvious type info already in signature.

```ts
/**
 * @param timeoutMs - Maximum wait time in milliseconds.
 */
```

#### `@typeParam`

- Use when: function/class/interface/type alias has generic parameters.
- Format: `@typeParam T - Description`
- Good for: role of generic (input model, output shape, constraint intent).
- Avoid: skipping generic docs on public APIs.

```ts
/**
 * @typeParam TItem - Item shape stored in the index.
 */
```

#### `@returns`

- Use when: function returns a value (including Promise payload semantics).
- Good for: return meaning, invariants, sorting/order guarantees.
- Avoid: trivial restatement like "Returns a string" with no value semantics.

```ts
/**
 * @returns A stable, deduplicated list sorted by `createdAt` descending.
 */
```

#### `@remarks`

- Use when: summary needs deeper context.
- Good for: algorithm notes, concurrency behavior, performance caveats, compatibility.
- Avoid: very short comments that belong in summary.

```ts
/**
 * @remarks
 * Uses optimistic concurrency and retries once on version conflicts.
 */
```

#### `@privateRemarks`

- Use when: internal maintainer notes should not be part of public docs.
- Good for: implementation/migration notes for contributors.
- Avoid: user-facing behavior docs.
- Note: handling may depend on the documentation pipeline.

```ts
/**
 * @privateRemarks
 * Keep wire format stable until mobile client v4 is fully rolled out.
 */
```

#### `@example`

- Use when: real usage clarifies intent, edge cases, or expected output.
- Good for: short, runnable or near-runnable snippets.
- Avoid: large, noisy, outdated snippets.

```ts
/**
 * @example
 * ```ts
 * const token = await issueToken(userId)
 * ```
 */
```

#### `@throws`

- Use when: function intentionally throws and callers should handle it.
- Good for: throw condition and error type/category.
- Avoid: listing every low-level incidental error.

```ts
/**
 * @throws Error if the checksum does not match.
 */
```

#### `@deprecated`

- Use when: API should not be used going forward.
- Good for: replacement API and migration note.
- Avoid: deprecating without migration guidance.

```ts
/**
 * @deprecated Use `createSessionToken()` instead.
 */
```

#### `@see`

- Use when: readers need related APIs/specs/docs.
- Good for: cross references and alternatives.
- Avoid: links that are unrelated to usage decisions.

```ts
/**
 * @see {@link validateToken}
 */
```

#### `@inheritDoc`

- Use when: API intentionally inherits docs from another declaration.
- Good for: overrides or wrappers with identical contract.
- Avoid: using when behavior differs from source declaration.

```ts
/**
 * @inheritDoc BaseClient.fetch
 */
```

#### `@defaultValue`

- Use when: documenting default value semantics for properties/options.
- Good for: public config surfaces.
- Avoid: stale defaults that diverge from implementation.

```ts
/**
 * @defaultValue 5000
 */
```

#### `@packageDocumentation`

- Use when: authoring package-level docs for an entrypoint/module.
- Good for: top-level module overview and usage.
- Avoid: placing on regular member docs.

```ts
/**
 * @packageDocumentation
 * Utilities for parsing and validating auth tokens.
 */
```

### 3.2 Inline Tags

#### `{@link ...}`

- Use when: referencing symbols or URLs inside prose.
- Good for: discoverability of related APIs.
- Avoid: plain text references when symbol links are available.

```ts
/**
 * Compatible with {@link TokenVerifier.verify} and {@link https://example.com/spec | RFC spec}.
 */
```

#### `{@label ...}`

- Use when: creating labels for structured references.
- Good for: advanced doc cross-reference scenarios.
- Avoid: normal linking use cases where `{@link}` is enough.

```ts
/**
 * {@label RetryPolicy}
 */
```

### 3.3 Modifier Tags

Modifier tags are marker tags with no body text.

#### Release and visibility policy tags

##### `@public`
- Use when: API is intended for public consumers.
- Avoid: tagging internal-only symbols as public.

##### `@internal`
- Use when: API is for package maintainers only.
- Avoid: exposing stable contracts with this tag unless intentional.

##### `@alpha`
- Use when: API is unstable and expected to change.
- Avoid: long-lived production contracts.

##### `@beta`
- Use when: mostly ready, but may still evolve.
- Avoid: marking fully stable APIs if your policy treats beta as prerelease.

##### `@experimental`
- Use when: feature is exploratory and may be removed.
- Avoid: using as a synonym for "beta" unless your policy defines it that way.

#### Inheritance/shape behavior tags

##### `@override`
- Use when: member overrides a base declaration.
- Avoid: using on non-overriding members.

##### `@virtual`
- Use when: member is designed to be overridden by derived types.
- Avoid: sealed/final behavior surfaces.

##### `@sealed`
- Use when: API shape is intentionally closed for extension.
- Avoid: APIs intended for inheritance/extensibility.

##### `@readonly`
- Use when: value should be treated as immutable by consumers.
- Avoid: mutable APIs where writes are expected.

#### Specialized modifier tags

##### `@eventProperty`
- Use when: property represents an event-like member contract.
- Avoid: regular data properties.

##### `@decorator`
- Use when: documenting declarations intended as decorators.
- Avoid: non-decorator symbols.

## 4) High-Quality TSDoc Patterns

### Document async behavior precisely

```ts
/**
 * Fetches the current profile.
 * @returns The active user profile.
 * @throws Error when the auth token is invalid.
 */
export async function getProfile(): Promise<UserProfile> {}
```

### Document generics meaningfully

```ts
/**
 * Creates a dictionary from items.
 * @typeParam T - Source item type.
 * @param items - Source collection.
 * @param getKey - Derives a stable key from each item.
 * @returns A map from key to item.
 */
export function toMap<T>(items: T[], getKey: (item: T) => string): Map<string, T> {}
```

### Document thrown contract only

```ts
/**
 * Parses a signed payload.
 * @param token - Signed token string.
 * @returns Decoded payload.
 * @throws Error when signature validation fails.
 */
export function parseSignedToken(token: string): Payload {}
```

## 5) TSDoc Mistakes To Avoid

- Using JSDoc type braces in TSDoc `@param` lines.
- Missing `@typeParam` for public generic APIs.
- Writing verbose summaries that hide key behavior.
- Adding `@example` blocks that are stale or too long.
- Documenting implementation trivia instead of API contract.
- Tagging release level (`@alpha`, `@beta`, etc.) inconsistently across related APIs.

## 6) TSDoc Review Checklist

Use this checklist when reviewing generated comments:

- Summary accurately states what the API does.
- Every parameter has a meaningful `@param`.
- All generics have `@typeParam` when public.
- `@returns` describes semantics, not just type.
- `@throws` documents expected caller-relevant failures.
- `@remarks` is used only when needed.
- Links use `{@link ...}` where appropriate.
- Release/visibility/modifier tags match project policy.
- Comment behavior matches implementation exactly.

## 7) JSDoc Quick Appendix (Cross-Standard Projects)

Use this only for JavaScript files or JSDoc-based tooling.

### JSDoc function template

```js
/**
 * Normalizes a user profile for rendering.
 * @param {Object} input - Raw user profile.
 * @param {string} input.id - Stable identifier.
 * @param {string} [input.displayName] - Optional display name.
 * @returns {Object} Normalized profile object.
 * @throws {Error} If required fields are missing.
 */
```

### JSDoc typedef template

```js
/**
 * @typedef {Object} UserProfile
 * @property {string} id - Stable user identifier.
 * @property {string} [displayName] - Optional display name.
 * @property {boolean} active - Whether the user is active.
 */
```

### JSDoc String Syntax Cheatsheet

Use these exact JSDoc "strings" for common cases:

- Required param: `@param {string} name - Description`
- Optional param: `@param {string} [name] - Description`
- Optional with default: `@param {string} [name=John Doe] - Description`
- Object param: `@param {Object} options - Description`
- Nested object field: `@param {string} options.mode - Description`
- Array object field: `@param {string} items[].id - Description`
- Return type: `@returns {Promise<Result>} Description`
- Throws type: `@throws {Error} Description`

### TypeScript Object Param Hover Pattern (Recommended)

For best VS Code hover and autocomplete docs on object properties, use a named type/interface and `/** ... */` on each property:

```ts
type SearchOptions = {
  /** Query string used to match results. */
  query: string;
  /** Maximum number of results to return. */
  limit?: number;
};

/**
 * @param options - Search configuration.
 */
export function search(options: SearchOptions) {}
```

## 8) Sources Used For This Skill

- TSDoc: `/microsoft/tsdoc` (Context7)
- JSDoc: `/jsdoc/jsdoc.github.io` (Context7)
