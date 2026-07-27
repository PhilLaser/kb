# Contributing

Contributions must be technically useful, attributable, safe by default, and reviewable.

## Workflow

1. Open an issue or describe the intended page and scope.
2. Start from the matching file in `_templates/`.
3. Prefer read-only enumeration and tool-independent explanations.
4. Cite primary sources and record validation limits.
5. Run Markdown, link, navigation, and secret checks.
6. Update `SUMMARY.md` and `CHANGELOG.md` in the same change.

Do not move or delete legacy pages without a migration note and explicit review. Never commit customer data, credentials, tenant identifiers, private IP inventories, or unredacted evidence.

## Review checklist

- Front matter uses allowed status and operational-risk labels.
- Claims distinguish fact, inference, experiment, and speculation.
- Commands name the shell, prerequisites, expected result, failure meaning, and cleanup.
- Configuration-changing steps include state capture, impact, reversal, and restoration checks.
- References include access date and verification state.
- Internal links are relative and present in `SUMMARY.md` where appropriate.

See [STYLE-GUIDE.md](STYLE-GUIDE.md).
