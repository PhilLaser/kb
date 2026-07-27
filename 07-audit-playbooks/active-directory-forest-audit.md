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

## Executable checks

1. [Forest coverage and inventory](active-directory/coverage-and-inventory.md)
2. [Privileged identities and administrative paths](active-directory/privileged-identities.md)
3. [Trusts and forest boundaries](active-directory/trusts-and-boundaries.md)
4. [Kerberos policy and delegation](active-directory/kerberos-and-delegation.md)
5. [NTLM exposure and relay resistance](active-directory/ntlm-exposure.md)
6. [AD CS inventory and certificate-template risk](active-directory/ad-cs-inventory-and-template-risk.md)
7. [Group Policy security and control paths](active-directory/group-policy-security.md)
8. [Credential and service-account hygiene](active-directory/credential-and-service-account-hygiene.md)

## Minimum permissions matrix

| Data source | Typical minimum | Limitations |
|---|---|---|
| RootDSE, domains, trusts, users, groups, computers | Authenticated domain user | ACL changes or hardened directory permissions can reduce visibility |
| Security descriptors (DACL/owner) | Read control on target object | SACL requires additional privilege; effective access still requires group/GUID resolution |
| Remote SMB/CIM configuration | Host-specific remote-management rights | Failure is a coverage gap, not a secure result |
| Security and NTLM operational logs | Event-log access on each target | Retention and audit policy bound the conclusion |
| CA runtime configuration and issued requests | CA-specific administrative/auditor rights | Directory-only collection cannot prove runtime CA state |
| GPO directory and SYSVOL content | Read on both AD object and SYSVOL files | Effective application also requires links, filtering, inheritance, and target state |

Use separate low-privilege and privileged collection identities when the engagement must characterize attacker-visible data and complete configuration coverage.

## Coverage ledger

Track every domain and DC as `assessed`, `partial`, `unreachable`, `excluded`, or `insufficient-permission`. Record collection time and replication caveats.

## Evidence minimum

Retain sanitized commands/queries, timestamps, source DC, querying principal, raw machine-readable results where permitted, screenshots only as supporting evidence, and a path narrative distinguishing theoretical from validated reachability.

## Remaining gaps

Host-local administrator coverage, replication health, DNS, LAPS, SCCM/MECM, Exchange, backup, virtualization, endpoint security, and hybrid identity checks remain separate batches. Commands in this batch are syntax-verified but require representative lab execution before `verified` status.
