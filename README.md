# OpenWork ID

**Open standard for portable, peer-verified professional identity.**

[![License: MIT](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE)
[![Schema Version](https://img.shields.io/badge/schema-v0.1.0-blue.svg)](schema/v1/)
[![MCP](https://img.shields.io/badge/MCP-2024--11--05-purple.svg)](docs/mcp-server.md)

---

## What is OpenWork ID?

OpenWork ID defines an open, JSON-LD-based schema for professional identity that is:

- **Portable** — Export your complete professional identity as structured data. Zero lock-in.
- **Peer-verified** — Real people publicly vouch for your work, staking their own reputation.
- **AI-agent ready** — Native MCP (Model Context Protocol) support. Agents read verified data without scraping.
- **Free forever** — No paywalls, no premium tiers. Professionals pay nothing.

The standard does not own identity. It defines how identity travels between systems.

> **Website:** [openworkid.org](https://openworkid.org)
> **Reference implementation:** [openid.work](https://openid.work)

---

## Core Concepts

The schema defines five interdependent concepts:

### 1. Experience Record

A structured narrative of professional work — not a CV line item. Each record captures context, challenge, contribution, result, and learning.

```jsonc
{
  "@type": "ExperienceRecord",
  "id": "https://openid.work/maria/exp/abc123",
  "organisation": "Acme GmbH",
  "role": "Senior Backend Engineer",
  "startDate": "2022-03",
  "endDate": "2024-01",
  "context": "Legacy payment system processing 2M transactions/month...",
  "challenge": "Migration to event-driven architecture without downtime...",
  "contribution": "I designed the dual-write strategy and led a team of 4...",
  "result": "Zero-downtime migration completed in 8 weeks, processing latency reduced by 62%.",
  "learning": "Learned that incremental rollouts with feature flags reduce risk more than big-bang releases."
}
```

### 2. Peer Verification

Public confirmation by a named individual. Verifiers stake their professional identity — name, role, and company are visible to everyone.

```jsonc
{
  "@type": "PeerVerification",
  "verifier": {
    "name": "Thomas Weber",
    "role": "CTO",
    "organisation": "Acme GmbH",
    "emailDomain": "acme.de",
    "emailDomainType": "corporate"
  },
  "statement": "Maria led this migration with remarkable clarity. The dual-write approach was her idea and it worked flawlessly.",
  "statementType": "text",
  "mutualFlag": false,
  "withdrawnAt": null
}
```

### 3. Human Proof

Evidence that a real person controls the profile — not an AI agent claiming credentials.

| Method | Strength | Description |
|--------|----------|-------------|
| `email-magic-link` | Minimum | Email ownership verified |
| `peer-verification-chain` | Strong | Multiple active peer verifications |
| `document-verified` | Strongest | Government ID verification (future) |

### 4. Portable Identity

Complete export envelope for cross-platform transfer. Includes all experience records, verifications, extensions, and export metadata.

### 5. Extensions

Modular, profession-specific field sets. Each extension has a canonical URI, is independently versioned, and attaches to any `ProfessionalIdentity`. The core schema stays universal — extensions make a Psychotherapist's profile structurally different from a Software Engineer's.

---

## Extension Library

**11 extensions** across three categories, all at v1.0.0:

### Technical

| Extension | Key Fields |
|-----------|------------|
| [Software Engineer](schema/v1/extensions/software-engineer.json) | languages, frameworks, cloud, databases, seniority, AI tools |
| [Data Scientist](schema/v1/extensions/data-scientist.json) | ML frameworks, specialisations, cloud ML, publications |
| [Product Manager](schema/v1/extensions/product-manager.json) | product types, methodologies, team size, tech affinity |
| [UX Designer](schema/v1/extensions/ux-designer.json) | disciplines, tools, project types, portfolio |

### Business

| Extension | Key Fields |
|-----------|------------|
| [Finance Controller](schema/v1/extensions/finance-controller.json) | specialisations (FP&A, Treasury, M&A...), ERP systems, certifications |
| [Management Consultant](schema/v1/extensions/management-consultant.json) | strategy, transformation, engagement length, methodologies |
| [Creative Director](schema/v1/extensions/creative-director.json) | disciplines, industries, project types, portfolio |

### Regulated

| Extension | Key Fields |
|-----------|------------|
| [Psychotherapist](schema/v1/extensions/psychotherapist.json) | approbation, therapeutic approach, setting, Kassenzulassung |
| [Physician](schema/v1/extensions/physician.json) | specialty, subspecialty, setting, certifications |
| [Lawyer](schema/v1/extensions/lawyer.json) | legal areas, bar admission, Fachanwaltstitel |
| [Architect](schema/v1/extensions/architect.json) | Leistungsphasen (HOAI), building types, BIM, chamber membership |

> Regulated extensions include a mandatory `regulatoryNotice` field. Implementing platforms **must** display this notice during profile editing.

---

## MCP Server

Native Model Context Protocol support. AI agents can read verified professional data without scraping.

**Server:** `https://mcp.openid.work/sse` (MCP 2024-11-05)
**REST fallback:** `https://api.openid.work/v1`

| Tool | Auth | Status | Description |
|------|------|--------|-------------|
| `get_profile` | Public | Available | Full professional identity document |
| `get_verifications` | Public | Available | Peer verifications for a profile |
| `get_suggestions` | User | Available | AI-generated profile improvement suggestions |
| `get_market_benchmark` | User | In development | Anonymised rate benchmarks |
| `add_experience` | User | In development | Create draft experience record |
| `request_verification` | User | In development | Send verification request |
| `search_profiles` | Client | Planned | Search verified profiles with filters |

See [MCP Server Documentation](docs/mcp-server.md) for full tool signatures, auth levels, and rate limits.

---

## Versioning

**Current version: v0.1.0** (April 2026)

The standard follows semantic versioning:

- **MAJOR** — Breaking changes (field removals, type changes). Existing documents may fail validation.
- **MINOR** — Additive changes (new optional fields, enum values, extensions). Existing documents remain valid.
- **PATCH** — Documentation and example fixes. Zero schema changes.

### Roadmap

| Version | Milestone | Key Changes |
|---------|-----------|-------------|
| v0.1.0 | M1 | Initial release. Core schema, 11 extensions, 3 MCP tools |
| v0.2.0 | M2 | M2 MCP tools, VerifiedEmployer schema, ContactRequest, RFC process opens |
| v1.0.0 | M3 | Stable release. search_profiles, VerifierReputation, Foundation governance |

---

## Governance

### Three Immovable Principles

1. **Professionals pay nothing. Ever.** No paywalls, no premium tiers.
2. **Your data is yours. Always.** Export as JSON anytime. Delete within 30 days.
3. **The standard belongs to no one.** MIT-licensed. Governance transfers to a neutral foundation when adoption reaches critical mass.

### RFC Process

All schema changes go through GitHub:

1. Open a [Discussion](https://github.com/entwicklerherz/openworkid.org/discussions)
2. Submit a PR using the [RFC template](rfcs/TEMPLATE.md)
3. Comment period: 7 days (non-breaking) or 30 days (breaking)
4. Changelog entry written before merge

---

## Repository Structure

```
openworkid.org/
├── schema/
│   └── v1/
│       ├── professional-identity.jsonld   # Core schema
│       ├── context.jsonld                  # JSON-LD @context
│       └── extensions/                     # 11 profession-specific extensions
│           ├── software-engineer.json
│           ├── data-scientist.json
│           ├── product-manager.json
│           ├── ux-designer.json
│           ├── finance-controller.json
│           ├── management-consultant.json
│           ├── creative-director.json
│           ├── psychotherapist.json
│           ├── physician.json
│           ├── lawyer.json
│           └── architect.json
├── docs/
│   └── mcp-server.md                      # MCP server specification
├── rfcs/
│   └── TEMPLATE.md                        # RFC template for schema changes
├── CHANGELOG.md
├── CONTRIBUTING.md
├── CODE_OF_CONDUCT.md
└── LICENSE
```

---

## Quick Start

### Read a profile (MCP)

```bash
# Using any MCP-compatible client
curl -X POST https://mcp.openid.work/sse \
  -H "Content-Type: application/json" \
  -d '{"method": "tools/call", "params": {"name": "get_profile", "arguments": {"username": "maria"}}}'
```

### Read a profile (REST)

```bash
curl https://api.openid.work/v1/profile/maria
```

### Export as JSON-LD

```bash
curl https://api.openid.work/v1/export/maria \
  -H "Accept: application/ld+json"
```

---

## Contributing

See [CONTRIBUTING.md](CONTRIBUTING.md) for how to propose changes, submit extensions, and participate in the RFC process.

---

## License

MIT. See [LICENSE](LICENSE).

---

Built and maintained by [entwicklerherz](https://github.com/entwicklerherz).
Reference implementation at [openid.work](https://openid.work).
