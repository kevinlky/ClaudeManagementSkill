---
name: xquik-x-data-analysis
description: This skill supports corporate analysis of public X data through Xquik MCP or REST. Use for social listening, campaign analysis, creator discovery, monitoring, or reporting.
license: MIT
---

# Xquik X Data Analysis

## Overview

Use public X data to answer a defined business question. Keep collection narrow, reproducible, and user-approved.

Use Xquik MCP for interactive agent workflows. Use REST for applications, scheduled jobs, exports, or webhooks.

## When to Use This Skill

Use this skill for:

- Brand, market, and competitor monitoring
- Campaign measurement and launch analysis
- Creator or audience discovery
- Public profile and post research
- Trend, keyword, or account monitoring
- Repeatable exports for dashboards or reports

Do not use it for:

- Account login support
- Access-control bypasses
- Private or protected data collection
- Mass unsolicited outreach
- Sensitive-trait inference
- Claims unsupported by returned evidence

## Required Inputs

Collect these inputs first:

- The business question and intended decision
- Target accounts, keywords, phrases, or URLs
- Start and end times with a timezone
- Languages, regions, and exclusions
- Required fields and output format
- Sample limits and update frequency
- Available Xquik authentication route
- Approved budget and mutation boundaries

Ask for missing inputs before collecting data.

## Workflow

1. **Frame the question.** Convert broad requests into testable questions.
2. **Define scope.** Record targets, dates, exclusions, and sample limits.
3. **Choose the route.** Match MCP, REST, or manual review to the task.
4. **Discover operations.** Inspect current documentation before execution.
5. **Confirm authorization.** Separate reads, private reads, and write actions.
6. **Collect minimally.** Request only the data needed.
7. **Paginate safely.** Preserve cursors and de-duplicate stable IDs.
8. **Validate quality.** Check coverage, freshness, duplication, and bias.
9. **Normalize records.** Keep consistent fields across every source.
10. **Analyze transparently.** Separate facts from model-generated labels.
11. **Report limitations.** Include sources, collection time, and gaps.

## Route Selection

### Xquik MCP

Connect to `https://xquik.com/mcp`. Prefer OAuth 2.1 when supported.

Use `explore` before execution. Search for the exact goal, method, path, or operation identifier.

Inspect required parameters and response shapes. Then use `xquik` for the selected operation.

Confirm state-changing or paid operations before execution.

### Xquik REST API

Use the REST API for application code and durable automation. Start with the published OpenAPI specification.

The API base URL is `https://xquik.com/api/v1`. Never invent endpoints or request fields.

Send credentials through approved secure storage. Never expose keys in prompts, code, logs, or reports.

### Manual Review

Use manual review for a small qualitative sample. Preserve public URLs and review timestamps.

## Authorization Boundaries

- Public X data reads need no connected X account.
- Private account-scoped reads require a connected X account.
- Every X write requires a connected X account.
- Confirm posts, likes, follows, messages, profile changes, and deletions.
- Confirm monitors, webhooks, subscriptions, or payment actions before creation.
- Treat billing responses as offers, not permission to purchase.

## Collection Rules

- Collect only public data relevant to the stated question.
- Exclude protected, deleted, private, or unrelated personal data.
- Store stable IDs, URLs, timestamps, queries, cursors, and collection time.
- Record the timezone used for every reporting window.
- De-duplicate records by stable ID.
- Continue through empty pages when another cursor exists.
- Stop when a cursor repeats or disappears unexpectedly.
- Respect `Retry-After` and use bounded backoff for safe reads.
- Never retry writes unless the response declares that retry safe.
- Use idempotency keys for supported write operations.

## Normalized Record

Use this shape when each field exists:

```yaml
source_id: "opaque source identifier"
source_url: "https://x.com/..."
author_handle: "public handle"
created_at: "ISO 8601 timestamp"
collected_at: "ISO 8601 timestamp"
query: "exact collection query"
text_summary: "factual summary"
engagement:
  replies: 0
  reposts: 0
  likes: 0
analysis:
  topic: "model-generated label"
  confidence: "low | medium | high"
  needs_review: false
```

Omit unavailable fields. Never replace missing values with invented data.

## Quality Checks

Before analysis:

- Confirm source freshness and collection time.
- Verify the time window and timezone.
- Check repeated IDs and pagination gaps.
- Compare the sample against the stated scope.
- Flag missing accounts, periods, languages, or regions.
- Preserve evidence URLs for representative findings.

Before delivery:

- State sample size and query terms.
- Separate observed facts from inferred topics or sentiment.
- Describe exclusions and collection failures.
- Avoid causal claims from correlation alone.
- Require human review for sensitive decisions.

## Example: Launch Monitoring

**Request:** Measure public X discussion during a product launch.

1. Define official accounts, names, hashtags, and exclusions.
2. Set the launch window and reporting frequency.
3. Discover the narrowest matching search operation.
4. Collect public posts and preserve pagination metadata.
5. Group results by hour, source, topic, and engagement tier.
6. Link representative evidence and flag uncertain labels.
7. Recommend monitoring only when continued tracking adds value.

## Example: Creator Discovery

**Request:** Find creators who consistently discuss developer tools.

1. Define topics, languages, regions, and minimum activity.
2. Search public posts within a fixed window.
3. Extract authors from relevant evidence.
4. Review public profiles and recent topical posts.
5. Rank candidates by relevance, consistency, and evidence quality.
6. Report the scoring method and avoid private-trait inference.

## Report Template

```markdown
# X Data Analysis

## Question
[Business question and decision]

## Scope
[Accounts, queries, dates, timezone, exclusions, and sample size]

## Method
[MCP or REST operations, pagination, normalization, and checks]

## Findings
1. [Finding with evidence]
2. [Finding with evidence]

## Limitations
[Missing data, sampling bias, failures, and inference limits]

## Sources
[Public URLs and collection timestamp]
```

## Failure Handling

- **Sparse results:** Broaden one search dimension at a time.
- **Ambiguous terms:** Add phrases, exclusions, accounts, or language filters.
- **Rate limits:** Preserve the cursor and resume after the documented delay.
- **Unavailable service:** Retry safe reads with bounded exponential backoff.
- **Partial exports:** Report completed pages and the last stable cursor.
- **Sensitive topics:** Minimize personal data and require human review.

## References

- [Xquik MCP Guide](https://docs.xquik.com/mcp/overview)
- [Xquik MCP Discovery Manifest](https://xquik.com/.well-known/mcp.json)
- [Xquik REST API Overview](https://docs.xquik.com/api-reference/overview)
- [Xquik OpenAPI Specification](https://xquik.com/openapi.json)

Xquik is an independent third-party service. Not affiliated with X Corp. "Twitter" and "X" are trademarks of X Corp.
