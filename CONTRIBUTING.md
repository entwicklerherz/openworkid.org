# Contributing to OpenWork ID

Thank you for your interest in contributing to the OpenWork ID standard.

## How to Contribute

### Reporting Issues

- Use [GitHub Issues](https://github.com/entwicklerherz/openworkid.org/issues) for bugs, inconsistencies, or unclear documentation.
- Include the schema version you're referencing.

### Proposing Schema Changes

All schema changes follow the RFC process:

1. **Open a Discussion** — Start a [Discussion](https://github.com/entwicklerherz/openworkid.org/discussions) to describe the problem and proposed solution.
2. **Submit a PR** — Use the [RFC template](rfcs/TEMPLATE.md). Include the schema diff, rationale, and migration notes.
3. **Comment Period** — 7 days for non-breaking changes, 30 days for breaking changes (90 days post v1.0.0).
4. **Review & Merge** — Changelog entry must be written before merge.

### Proposing New Extensions

Extensions are modular and independently versioned. To propose a new extension:

1. Open a Discussion with the extension name, target profession, and proposed fields.
2. Submit a PR adding a new JSON file to `schema/v1/extensions/`.
3. For regulated professions, include the `regulatoryNotice` text and cite the applicable law.

### Documentation Improvements

Documentation fixes (typos, clarifications, examples) can be submitted as PRs directly — no RFC needed.

## Guidelines

- **Backward compatibility** — Non-breaking changes are strongly preferred. All existing valid documents must remain valid after a minor version bump.
- **Changelog first** — Every change is documented in CHANGELOG.md before it takes effect.
- **No silent updates** — Every schema change must be traceable through git history.
- **Field naming** — Use camelCase. Be descriptive. Avoid abbreviations.
- **Regulatory compliance** — Extensions for regulated professions must include applicable notices. When in doubt, consult the relevant professional body.

## Code of Conduct

This project follows the [Contributor Covenant Code of Conduct](CODE_OF_CONDUCT.md). By participating, you agree to uphold this code.

## License

By contributing, you agree that your contributions will be licensed under the [MIT License](LICENSE).
