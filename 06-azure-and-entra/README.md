---
title: Azure and Microsoft Entra ID
platform: cloud
technology: azure-entra
status: research-note
last-tested:
references: []
---

# Azure and Microsoft Entra ID

Azure and Entra assessments cross several independent authorization systems. Directory roles, Microsoft Graph permissions, Azure RBAC, resource-provider actions, data-plane roles, application ownership, and token audience must be evaluated separately and then correlated into effective privilege paths.

## Trust boundaries

- Tenant, management-group, subscription, resource-group, and resource scopes.
- Entra directory objects versus Azure Resource Manager resources.
- Control-plane authorization versus service-specific data planes.
- Human, guest, application, service-principal, managed, workload, and device identities.
- Consent, token issuance, Conditional Access, privileged activation, federation, synchronization, and cross-tenant access.

## Assessment approach

Enumerate all accessible tenants and subscriptions before choosing context. Preserve tenant, subscription, principal, token audience, scope, and timestamp with every result. Distinguish an absent resource from an unreadable resource and report partial coverage explicitly. Prefer bulk collection through Resource Graph, Microsoft Graph, ARM/REST, CLI, or PowerShell before portal validation.

## Priority playbooks

- [Entra ID tenant audit](../07-audit-playbooks/entra-id-tenant-audit.md)
- [Azure subscription audit](../07-audit-playbooks/azure-subscription-audit.md)
- [Azure IAM and privilege-path audit](../07-audit-playbooks/azure-iam-privilege-path-audit.md)

## Planned foundations

Azure hierarchy, Azure RBAC, Entra roles, Graph permission models, applications and service principals, managed identities, OAuth tokens, Conditional Access, PIM, hybrid identity, Resource Graph, and control-plane/data-plane separation.

## References

Canonical pages will use Microsoft documentation, protocol specifications, official API references, tool source code, and primary security research.
