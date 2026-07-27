---
title: Active Directory forest audit
platform: windows
technology: active-directory
operational-risk: authenticated
status: research-note
last-tested:
references: []
---

# Active Directory forest audit

## Objective

Establish forest-wide identity and privilege exposure, including cross-domain, certificate, endpoint-management, backup, virtualization, and hybrid paths.

## Workflow

1. Confirm authorization, forest/domain scope, excluded systems, test windows, and evidence handling.
2. Validate DNS, time, domain controllers, Global Catalogs, sites, trusts, and reachable protocols.
3. Record the collection identity, group memberships, privileges, and data visibility.
4. Inventory domains, principals, groups, computers, SPNs, GPOs, delegation, trusts, PKI, privileged systems, and hybrid connectors.
5. Review authentication policy, NTLM exposure, Kerberos encryption/delegation, local-admin reuse, and credential hygiene.
6. Calculate effective ACL and group-based privilege paths, including ownership and inheritance.
7. Manually validate high-impact paths and environmental blockers.
8. Perform only approved, least-invasive abuse validation; capture artefacts and cleanup requirements first.
9. Collect reproducible evidence, coverage gaps, and collection errors.
10. Restore created state, confirm removal, report root causes, and define retests.

## Coverage ledger

Track every domain and DC as `assessed`, `partial`, `unreachable`, `excluded`, or `insufficient-permission`. Record collection time and replication caveats.

## Evidence minimum

Retain sanitized commands/queries, timestamps, source DC, querying principal, raw machine-readable results where permitted, screenshots only as supporting evidence, and a path narrative distinguishing theoretical from validated reachability.

## Known gaps

The permissions matrix, collector recipes, evidence schema, and individual checks are scheduled in the backlog.
