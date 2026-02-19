# Agent Skills

A collection of skills for AI coding agents following the Agent Skills format.

## Available Skills

### [`tsdoc-jsdoc-authoring`](./tsdoc-jsdoc-authoring)

Comprehensive TSDoc and JSDoc authoring skill for TypeScript and JavaScript codebases. Includes rule-based guidance, incorrect/correct examples, and VS Code hover-friendly patterns for object parameter properties.

## Installation

```bash
npx skills add consentdotio/agent-skills
```

## Usage

Skills are automatically activated when relevant tasks are detected. Example prompts:

- "Add TSDoc comments to this TypeScript API file."
- "Write JSDoc for this JavaScript utility module."
- "Document each object parameter property so VS Code hover shows descriptions."
- "Review these docs and fix missing @param/@returns tags."

## Skill Structure

- `tsdoc-jsdoc-authoring/SKILL.md`: Main skill behavior, triggers, and workflow.
- `tsdoc-jsdoc-authoring/AGENTS.md`: Full compiled rule index and category guide.
- `tsdoc-jsdoc-authoring/reference.md`: Deep reference and syntax cheatsheets.
- `tsdoc-jsdoc-authoring/rules/**`: Focused rule files with incorrect/correct examples.

## Validation

```bash
npm run validate
```

This runs:

```bash
bunx skills-ref validate ./tsdoc-jsdoc-authoring
```

## Prerequisites

- A Cursor/Codex environment with Agent Skills support
- `bunx` available for local validation

## License

MIT
