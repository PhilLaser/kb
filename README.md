# Pentesting and red team knowledge base

This repository is a practitioner-focused reference for authorized penetration tests, red team engagements, security audits, lab research, and assessment reporting. Markdown in Git is the source of truth and the structure remains usable in GitBook and ordinary Markdown viewers.

## Safety and authorization

All offensive activity described here assumes explicit written authorization and a defined scope. Pages label operational risk and begin with read-only enumeration. Configuration-changing, credential-access, high-noise, service-impacting, and destructive actions require separate review, evidence planning, cleanup, and restoration verification.

## How to use this repository

- Start with the relevant concept page to understand the mechanism and trust boundary.
- Follow an audit playbook for coverage, permissions, evidence, validation, cleanup, and retesting.
- Use technique pages only after prerequisites and operational consequences are understood.
- Treat tool pages as interfaces to protocols and APIs, not as sources of truth.
- Treat pages marked `research-note`, `partially-verified`, or `needs-retest` as requiring independent validation.

## Status labels

`verified`, `partially-verified`, `research-note`, `deprecated`, `needs-retest`.

## Operational-risk labels

`passive`, `low-noise`, `authenticated`, `configuration-changing`, `credential-access`, `high-noise`, `service-impacting`, `destructive`.

## Current state

The numbered structure is the target architecture. Existing material under `windows/`, `linux/`, and `maldev/` is preserved as legacy content pending page-by-page validation and migration. See the [repository roadmap](00-start-here/repository-roadmap.md), [style guide](STYLE-GUIDE.md), and [contribution workflow](CONTRIBUTING.md).

## Scope boundaries

Malware and payload-development research is isolated under `10-malware-development/`. Its implementation details must not be copied into ordinary infrastructure-audit pages.
