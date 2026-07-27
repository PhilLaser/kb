---
title: Entra ID tenant audit
platform: cloud
technology: entra-id
operational-risk: authenticated
status: research-note
last-tested:
references: []
---

# Entra ID tenant audit

## Objective

Assess identities, privileged roles, authentication controls, applications, consent, external collaboration, devices, workloads, and hybrid identity across the complete tenant.

## Workflow

1. Confirm tenant ID, tenant type, domains, licenses, exclusions, and approved identities.
2. Record authentication mode, token audience, delegated/application permissions, Entra roles, and consent state.
3. Inventory users, guests, groups, administrative units, devices, roles, PIM eligibility, applications, service principals, credentials, grants, and federation/synchronization components.
4. Review MFA and passwordless registration, Conditional Access coverage/exclusions, risk policies, legacy authentication, emergency access, and workload policies.
5. Expand effective privilege through groups, ownership, app permissions, consent, role eligibility, and hybrid control.
6. Correlate sign-in and audit activity where authorized and retention permits.
7. Validate high-impact paths manually and label blockers, licensing gaps, and unreadable data.
8. Collect evidence, clean up approved test artefacts, report, and define retests.

## Coverage ledger

Record every Graph endpoint or dataset as `complete`, `partial`, `permission-denied`, `license-unavailable`, `retention-limited`, or `not-applicable`. Never treat an empty response as proof of absence until authorization and pagination are confirmed.

## Permissions matrix fields

Each check must state token audience, delegated or application mode, minimum known Graph permission, admin-consent requirement, required Entra role, API version, and whether licensing changes visibility.
