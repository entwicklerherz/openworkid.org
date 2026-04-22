# Changelog

All notable changes to the OpenWork ID standard are documented in this file.

Entries are permanent and never edited or deleted. Breaking changes require an RFC.

The format follows [Keep a Changelog](https://keepachangelog.com/).

---

## [0.1.0] — 2026-04-13

### Added

- Core schema with five concepts: ProfessionalIdentity, ExperienceRecord, PeerVerification, HumanProof, Extension
- JSON-LD @context at `https://openworkid.org/v1`
- PortableIdentityExport envelope for cross-platform transfer
- 11 profession-specific extensions (4 technical, 3 business, 4 regulated)
  - Technical: software-engineer, data-scientist, product-manager, ux-designer
  - Business: finance-controller, management-consultant, creative-director
  - Regulated: psychotherapist, physician, lawyer, architect
- MCP server specification (7 tools, 3 auth levels)
  - M1 tools available: get_profile, get_verifications, get_suggestions
  - M2 tools in development: get_market_benchmark, add_experience, request_verification
  - M3 tools planned: search_profiles
- RFC process and contribution guidelines
- MIT license

### Reference Implementation

- [upstand.work](https://upstand.work) — live platform implementing this standard
