# MCP Server Specification

OpenWork ID provides a native Model Context Protocol (MCP) server, enabling AI agents to read verified professional data without scraping.

**Server URL:** `https://mcp.upstand.work/sse`
**MCP Version:** 2024-11-05
**REST Fallback:** `https://api.upstand.work/v1`

---

## Authentication

Three non-stacking auth levels:

| Level | Token Format | Access |
|-------|-------------|--------|
| **Public** | None required | `get_profile`, `get_verifications` |
| **User Auth** | `Bearer oiw_usr_xxxxxxxx` | All M1 + M2 tools (own profile) |
| **Client Auth** | `Bearer oiw_api_xxxxxxxx` | M3 tools (read-only, opt-in profiles) |

**User Auth tokens** are obtained via magic-link flow. Valid for 90 days, revocable by the user.

**Client Auth tokens** are API keys for platform integrations.

---

## Rate Limits

| Level | Limit |
|-------|-------|
| Public | 60 requests/minute per IP |
| User Auth (read) | 300 requests/hour per token |
| User Auth (add_experience) | 20 requests/day per user |
| User Auth (request_verification) | 10 requests/week per user |
| Client Auth | Quota-based (per agreement) |

---

## Tools

### Milestone 1 — Available

#### `get_profile`

Returns the complete professional identity document for a public profile.

**Auth:** Public (no token required)

**Parameters:**

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `username` | string | Yes | Profile username |
| `include_extensions` | boolean | No | Include extension data (default: true) |
| `format` | string | No | `"full"` or `"summary"` (default: `"full"`) |

**Returns:**

```json
{
  "identity": { ProfessionalIdentity },
  "verification_count": 5,
  "last_updated": "2026-03-15T10:30:00Z"
}
```

**Example (MCP):**

```json
{
  "method": "tools/call",
  "params": {
    "name": "get_profile",
    "arguments": {
      "username": "maria",
      "format": "full"
    }
  }
}
```

**Example (REST):**

```bash
curl https://api.upstand.work/v1/profile/maria
```

---

#### `get_verifications`

Returns all active peer verifications for a profile.

**Auth:** Public (no token required)

**Parameters:**

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `username` | string | Yes | Profile username |
| `experience_id` | string | No | Filter to a specific experience |
| `include_one_click` | boolean | No | Include one-click verifications (default: true) |

**Returns:**

```json
{
  "verifications": [ PeerVerification ],
  "total": 5,
  "mutual_count": 1
}
```

**Notes:**
- Withdrawn verifications are excluded
- Email addresses are never returned (domain only)
- Mutual verifications are transparently flagged

---

#### `get_suggestions`

AI-generated analysis of the authenticated user's profile, identifying gaps and improvement opportunities.

**Auth:** User Auth

**Parameters:** None (operates on the authenticated user's own profile)

**Returns:**

```json
{
  "profile_strength": "moderate",
  "missing_fields": ["summary", "availableForWork"],
  "unverified_experiences": ["exp/abc123", "exp/def456"],
  "suggested_verifiers": [
    {
      "experience_id": "exp/abc123",
      "suggestion": "Request verification from your team lead at Acme GmbH"
    }
  ]
}
```

---

### Milestone 2 — In Development

#### `get_market_benchmark`

Anonymised rate benchmarks aggregated from opt-in verified profiles.

**Auth:** User Auth

**Parameters:**

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `category` | string | Yes | Extension category (e.g., `"software-engineer"`) |
| `region` | string | No | Geographic filter (e.g., `"DACH"`, `"Berlin"`) |
| `seniority` | string | No | Seniority filter |
| `unit` | string | No | `"hour"` or `"day"` (default: `"hour"`) |

**Returns:**

```json
{
  "p25": 85,
  "p50": 110,
  "p75": 140,
  "currency": "EUR",
  "sample_size": 234,
  "data_source": "internal"
}
```

---

#### `add_experience`

Creates a draft ExperienceRecord. AI writes, human confirms.

**Auth:** User Auth

**Parameters:**

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `experience` | ExperienceRecord | Yes | Draft experience record |

**Returns:**

```json
{
  "draft_id": "draft_abc123",
  "draft": { ExperienceRecord },
  "confirm_url": "https://upstand.work/dashboard?confirm=draft_abc123",
  "status": "draft"
}
```

**Note:** Drafts require explicit user confirmation before going live. No experience is published without human approval.

---

#### `request_verification`

Sends a verification request email for a specific experience.

**Auth:** User Auth

**Parameters:**

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `experience_id` | string | Yes | ID of the experience to verify |
| `verifier_email` | string | Yes | Email address of the verifier |
| `verifier_name` | string | Yes | Full name of the verifier |
| `context_message` | string | Yes | Personal message to the verifier |

**Returns:**

```json
{
  "request_id": "req_xyz789",
  "sent_at": "2026-04-13T14:00:00Z",
  "weekly_remaining": 9
}
```

**Rate limit:** 10 requests/week per user. Human-in-the-loop required; no automation of verification requests.

---

### Milestone 3 — Planned

#### `search_profiles`

Search verified profiles with filters. Only returns profiles with `availableForWork: true` and API discovery enabled.

**Auth:** Client Auth

**Parameters:**

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `extension_id` | string | Yes | Extension to search (e.g., `"software-engineer"`) |
| `fields` | object | No | Field-level filters |
| `min_verifications` | integer | No | Minimum verification count |
| `region` | string | No | Geographic filter |
| `limit` | integer | No | Max results (default: 20) |

**Returns:**

```json
{
  "profiles": [ ProfessionalIdentity ],
  "total": 47,
  "api_credits_used": 1
}
```

**Note:** Returns profile data only. No contact details are exposed through search. Contact must go through the platform's contact request flow.

---

## Integration Examples

### Claude Desktop (MCP)

```json
{
  "mcpServers": {
    "openwork-id": {
      "url": "https://mcp.upstand.work/sse"
    }
  }
}
```

### REST (any HTTP client)

```bash
# Get a profile
curl https://api.upstand.work/v1/profile/maria

# Get verifications
curl https://api.upstand.work/v1/verifications/maria

# Get suggestions (authenticated)
curl https://api.upstand.work/v1/suggestions \
  -H "Authorization: Bearer oiw_usr_xxxxxxxx"
```

### OpenAI Function Calling

```json
{
  "name": "get_openwork_profile",
  "description": "Get a verified professional profile from OpenWork ID",
  "parameters": {
    "type": "object",
    "properties": {
      "username": {
        "type": "string",
        "description": "The OpenWork ID username"
      }
    },
    "required": ["username"]
  }
}
```

---

## Error Handling

| HTTP Status | Meaning |
|-------------|---------|
| 200 | Success |
| 400 | Invalid parameters |
| 401 | Missing or invalid auth token |
| 403 | Insufficient permissions |
| 404 | Profile or resource not found |
| 429 | Rate limit exceeded |
| 500 | Server error |

Error responses follow this format:

```json
{
  "error": {
    "code": "rate_limit_exceeded",
    "message": "Rate limit of 60 requests/minute exceeded. Retry after 23 seconds.",
    "retry_after": 23
  }
}
```
