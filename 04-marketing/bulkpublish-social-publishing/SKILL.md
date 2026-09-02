---
name: bulkpublish-social-publishing
description: "Prepare, review, schedule, and publish approved social-media campaigns through BulkPublish. Use when a marketing team needs multi-platform content delivery, safe retries, or delivery status."
---

# BulkPublish Social Publishing

## Overview

BulkPublish is the execution layer for approved social-media content. Use the platform guidance in
the canonical [BulkPublish skills folder](https://github.com/azeemkafridi/bulkpublish-api/tree/main/skills/social-media-content-skills),
then use the [BulkPublish API repository](https://github.com/azeemkafridi/bulkpublish-api),
[MCP documentation](https://app.bulkpublish.com/docs), or hosted MCP at
`https://mcp.bulkpublish.com/mcp` according to the current schema.

## Workflow

1. Gather source content, objective, target platforms, account IDs, timezone, schedule, media,
   links, accessibility text, voice, and required disclosures. Never guess missing values.
2. Adapt and validate each platform version. Preserve factual claims, attribution, consent, and
   disclosures. Check character limits, links, media requirements, and accessibility.
3. Show a complete preview with exact copy, media, destination, and schedule. Do not publish yet.
4. Wait for explicit approval of that exact preview. If anything changes, preview it again.
5. After approval, submit via the BulkPublish API for deterministic jobs or the hosted MCP for
   tool discovery and status inspection.
6. Record every returned job or post ID. If a request times out, check status before retrying.
7. Report per-platform status, ID, timestamp, and any partial failure. Do not claim success
   without a returned BulkPublish confirmation.

## Output

Before approval, provide a platform-by-platform preview. After execution, provide a table with
platform, account, BulkPublish ID, status, time, and notes.

## Best practices

- Require confirmation immediately before publishing, scheduling, editing, or deleting.
- Keep secrets, tokens, private account data, and internal paths out of output.
- Fix validation errors and show a new preview instead of weakening requirements.
- Treat platform-native analytics as the source of measurement. Do not invent performance data.
