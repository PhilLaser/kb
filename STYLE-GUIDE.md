# Style guide

## Voice and scope

Write for experienced security practitioners. Lead with the mechanism and boundary, then enumeration, validation, evidence, detection, remediation, and cleanup. Avoid generic filler and tool-only recipes.

## Required metadata

Use YAML front matter from a template. Allowed status values are `verified`, `partially-verified`, `research-note`, `deprecated`, and `needs-retest`. Allowed operational-risk values are `passive`, `low-noise`, `authenticated`, `configuration-changing`, `credential-access`, `high-noise`, `service-impacting`, and `destructive`.

Use ISO dates (`YYYY-MM-DD`). Leave `last-tested` blank when no reproducible test was performed. Do not equate publication with testing.

## Commands

- State the shell and minimum access.
- Use descriptive placeholders such as `<tenant-id>` and variables such as `TENANT_ID`.
- Start with read-only collection.
- Mark untested commands explicitly.
- For state changes, document impact, backup, reversal, and restoration evidence.

## Evidence and uncertainty

For unverified claims use:

```text
Status: Unverified
Reason:
Required validation:
```

Never invent flags, fields, permissions, GUIDs, registry paths, event IDs, or output. Redact secrets and personal/customer data.

## Links and references

Use relative internal links. Prefer standards, official documentation, source code, and primary research. Record title, author or organization, URL, update/publication date when known, access date, relevant section, source type, and independent-verification state.

## Filenames and headings

Use lowercase descriptive hyphenated filenames. Use one `#` heading per page and sentence case below it. Keep canonical concepts in one page and cross-link rather than duplicating them.
