# Context7 Integration Design

## Overview

Add proactive Context7 usage guidance across all superpowers skills, ensuring LLMs fetch current library documentation before writing any code that uses external dependencies.

## Approach

Direct MCP tool guidance (not a skill wrapper). Context7 is a tool, not a workflow - the value is in triggering usage, not explaining how to use it.

## Trigger Condition

**Ultra-aggressive:** Before writing ANY code that uses external libraries, frameworks, or APIs - always fetch current docs first.

## Tool References

Use short names for clarity:
- Context7's `resolve-library-id` tool
- Context7's `query-docs` tool

## Files to Modify

### 1. `skills/using-superpowers/SKILL.md`

Add new section after "Skill Priority":

```markdown
## External Documentation (Context7)

Before writing ANY code using external libraries, frameworks, or APIs:
1. Use Context7's `resolve-library-id` to find the library
2. Use Context7's `query-docs` to fetch current documentation

This is not optional. Training data becomes stale. Even if you "know" the library, fetch current docs. The cost of a lookup is trivial compared to debugging outdated code.
```

### 2. `skills/brainstorming/SKILL.md`

Add new section after "Domain-Specific Skills":

```markdown
## External Documentation

Before exploring technical approaches for any feature involving external libraries:
- Use Context7's `resolve-library-id` and `query-docs` to fetch current documentation
- Base design decisions on current APIs, not training data
- This applies even for well-known libraries (React, Express, etc.)
```

### 3. `skills/writing-plans/SKILL.md`

Add new section after domain-specific skills:

```markdown
## External Documentation

Before writing implementation steps that use external libraries:
- Use Context7 to fetch current documentation for ALL dependencies involved
- Include version-specific APIs in task specifications
- Never assume training data reflects current library behavior
```

### 4. `skills/subagent-driven-development/SKILL.md`

Add to "Available Skills" section (or create parallel "External Tools" subsection):

```markdown
### External Tools

- **Context7** - Fetch current library documentation. Use `resolve-library-id` then `query-docs` BEFORE writing any code using external dependencies. This is mandatory, not optional.
```

### 5. `skills/subagent-driven-development/implementer-prompt.md`

Add near the top with other mandatory behaviors:

```markdown
## External Documentation

Before writing ANY code that uses external libraries, frameworks, or APIs:
1. Use Context7's `resolve-library-id` to find the library
2. Use Context7's `query-docs` to fetch current documentation

Do this EVERY time, even for libraries you "know". Training data is stale. APIs change. Fetch first, code second.
```

### 6. `skills/dispatching-parallel-agents/SKILL.md`

Add to the subagent prompt template:

```markdown
**External Documentation:** Before writing code using any external library, use Context7's
`resolve-library-id` and `query-docs` to fetch current documentation. This is mandatory.
```

## Rationale

- **Ultra-aggressive trigger:** User explicitly requested "even thinks a little" threshold
- **Direct MCP guidance:** Context7 is a tool, not a workflow - no skill wrapper needed
- **Same locations as frontend-design:** Consistent pattern across the codebase
- **Imperative tone:** "Not optional" - matches superpowers' mandatory workflow philosophy
