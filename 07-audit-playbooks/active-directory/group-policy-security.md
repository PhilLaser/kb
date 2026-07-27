---
title: AD Group Policy security and control paths
platform: windows-linux
technology: group-policy
required-access: domain-user
operational-risk: authenticated
status: research-note
last-tested:
references: []
---

# AD Group Policy security and control paths

## Objective

Inventory GPOs, links, scope, inheritance, filtering, SYSVOL content, ownership, modification rights, and effective application to privileged systems. A GPO is dangerous when a principal can change policy content or links that reach a higher-trust target.

## Windows collection

```powershell
$Domain = "<domain-fqdn>"
Get-GPO -All -Domain $Domain |
    Select-Object Id,DisplayName,GpoStatus,CreationTime,ModificationTime,Owner |
    Export-Csv '.\gpo-inventory.csv' -NoTypeInformation -Encoding UTF8

Get-GPInheritance -Target "<domain-or-ou-distinguished-name>" |
    Format-List *

Get-GPOReport -All -Domain $Domain -ReportType Xml -Path '.\all-gpos.xml'
```

The GroupPolicy module/RSAT is required. XML contains sensitive configuration and should be protected as evidence.

## Linux collection

```bash
DC_FQDN="<dc-fqdn>"
BASE_DN="<domain-distinguished-name>"

ldapsearch -LLL -Y GSSAPI -H "ldap://${DC_FQDN}" -b "CN=Policies,CN=System,${BASE_DN}" \
  '(objectClass=groupPolicyContainer)' displayName name gPCFileSysPath flags versionNumber nTSecurityDescriptor
```

LDAP returns directory-side metadata. A complete review also needs authorized read access to corresponding SYSVOL paths and ACLs, plus links (`gPLink`), inheritance blocking, security filtering, and WMI filters.

## Review criteria

- GPO and SYSVOL ownership/ACL mismatch or unprivileged modification.
- Rights to create/link GPOs on privileged OUs or the domain.
- Policies affecting DCs, tier-zero servers, administrators, and security tooling.
- Startup/logon scripts, scheduled tasks, services, local groups, registry, user rights, firewall, audit policy, and software deployment.
- Embedded credentials or legacy Group Policy Preference secrets.
- Disabled halves, orphaned GPOs, stale links, enforced links, blocked inheritance, WMI filters, and inaccessible SYSVOL content.

## Evidence and remediation

Retain GPO GUID/name, directory and SYSVOL ACLs, link target/order/enforcement, filtering, settings creating impact, modifying principal, and effective target set. Restrict editing/linking, separate GPO administration, remove secrets and stale content, monitor directory and SYSVOL changes, and validate resultant policy on representative targets (`gpresult.exe /h`).

## References

- Microsoft Learn, [Understand Active Directory Group Policy security settings](https://learn.microsoft.com/en-us/training/modules/understand-active-directory-security-policies/), accessed 2026-07-27.
