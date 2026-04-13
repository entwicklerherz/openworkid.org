# RFC: [Title]

- **Author:** [Your name]
- **Date:** [YYYY-MM-DD]
- **Status:** Draft | Under Review | Accepted | Rejected
- **Schema Version:** [Current version, e.g., 0.1.0]
- **Change Type:** Breaking | Non-breaking | Patch

## Summary

One paragraph describing the proposed change.

## Motivation

Why is this change needed? What problem does it solve? Include links to related issues or discussions.

## Specification

### Schema Changes

Describe the exact changes to the schema. Include before/after examples.

```jsonc
// Before
{
  "field": "old_value"
}

// After
{
  "field": "new_value",
  "newField": "added_value"
}
```

### Migration Path

How should existing documents be updated? Is this automatically migratable?

### Impact on Extensions

Does this change affect any existing extensions?

## Alternatives Considered

What other approaches were evaluated? Why were they rejected?

## Backward Compatibility

- [ ] All existing valid documents remain valid (non-breaking)
- [ ] Existing documents may need migration (breaking — requires 30-day comment period)

## Changelog Entry

Draft the CHANGELOG.md entry for this change:

```markdown
### [Changed|Added|Removed]
- Description of change
```
