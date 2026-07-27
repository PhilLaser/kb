---
title: Active Directory
platform: windows
technology: active-directory
status: research-note
last-tested:
references: []
---

# Active Directory

Active Directory Domain Services is a distributed identity, authentication, authorization, policy, and service-discovery system. For an assessor, the meaningful unit is not a list of objects but a graph of principals, credentials, delegated rights, hosts, services, trusts, and replication boundaries.

## Trust boundaries

- Forest security boundary and domain administration boundaries within it.
- Domain controllers, directory replication, SYSVOL, and administrative tooling.
- Kerberos realms, NTLM acceptance, trusts, SID filtering, and selective authentication.
- Object ownership, security descriptors, extended rights, and inherited delegation.
- Certificate services, endpoint-management systems, backups, virtualization, and hybrid identity connectors.

## Assessment approach

Start by confirming scope, domains, forests, trusts, sites, and reachable directory services. Collect authenticated directory state with the lowest viable privilege, preserve collection context, then analyze effective paths rather than role names alone. Validate risky paths with the least invasive method and record controls that block theoretical edges.

## Priority playbooks

- [Active Directory forest audit](../07-audit-playbooks/active-directory-forest-audit.md)

## Field foundations

- [Directory structure and discovery](directory-structure-and-discovery.md)
- [Security descriptors and effective rights](ad-security-descriptors-and-effective-rights.md)
- [Kerberos protocol and ticket flow](../01-foundations/kerberos-protocol-and-ticket-flow.md)
- [NTLM authentication and relay boundaries](../01-foundations/ntlm-authentication-and-relay-boundaries.md)

## Legacy material awaiting migration

Current technique notes remain under [`../windows/active-directory/`](../windows/active-directory/). They are not yet certified against the new templates.

## Required foundations

Planned foundations include directory objects and partitions, security descriptors and ACL evaluation, Kerberos, NTLM, trusts, Group Policy, replication, AD CS, and hybrid identity.

## References

References will be added to each canonical foundation page during its validation batch.
