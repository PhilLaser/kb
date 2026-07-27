---
title: AD privileged identities and administrative paths
platform: windows-linux
technology: active-directory
required-access: domain-user
operational-risk: authenticated
status: partially-verified
last-tested:
references: []
---

# AD privileged identities and administrative paths

## Objective

Identify direct, nested, delegated, protected, dormant, and service-based privileged identities. Default group membership is only the starting point; object control, GPO rights, replication rights, local administration, and management systems can create equivalent privilege.

## Windows collection

```powershell
$DC = "<dc-fqdn>"
$Domain = Get-ADDomain -Server $DC
$PrivilegedGroups = @(
    "$($Domain.DomainSID)-512",
    "$($Domain.DomainSID)-518",
    "$($Domain.DomainSID)-519",
    'S-1-5-32-544'
)

$PrivilegedMembers = foreach ($Sid in $PrivilegedGroups) {
    try {
        $Group = Get-ADGroup -Identity $Sid -Server $DC
        Get-ADGroupMember -Identity $Group -Recursive -Server $DC |
            Select-Object @{Name='PrivilegedGroup';Expression={$Group.Name}},Name,SamAccountName,ObjectClass,DistinguishedName
    } catch {
        [pscustomobject]@{PrivilegedGroup=$Sid;Name=$null;SamAccountName=$null;ObjectClass=$null;DistinguishedName=$null}
    }
}
$PrivilegedMembers | Export-Csv '.\ad-privileged-members.csv' -NoTypeInformation -Encoding UTF8

Get-ADObject -LDAPFilter '(&(adminCount=1)(|(objectClass=user)(objectClass=group)))' -Server $DC -Properties adminCount,whenChanged |
    Select-Object Name,ObjectClass,DistinguishedName,adminCount,whenChanged |
    Export-Csv '.\ad-admincount-objects.csv' -NoTypeInformation -Encoding UTF8
```

`adminCount=1` is historical/protection-state evidence, not proof of current privilege. Compare current group paths and the object's ACL protection state.

## Linux collection

```bash
DC_FQDN="<dc-fqdn>"
BASE_DN="<domain-distinguished-name>"

ldapsearch -LLL -Y GSSAPI -H "ldap://${DC_FQDN}" -b "${BASE_DN}" \
  '(&(adminCount=1)(|(objectClass=user)(objectClass=group)))' \
  distinguishedName objectSid sAMAccountName memberOf userAccountControl pwdLastSet lastLogonTimestamp

nxc ldap "${DC_FQDN}" -d "<domain-fqdn>" -u "<username>" -p '<password>'
```

Nested membership requires recursive resolution. LDAP matching rule `1.2.840.113556.1.4.1941` can resolve transitive membership on supported AD DS versions, but retain the queried group DN and DC.

## Review criteria

- Direct and nested membership in forest/domain/builtin administrative groups.
- Accounts protected by AdminSDHolder/SDProp and stale `adminCount` state.
- Disabled, stale, shared, or interactive service accounts with privilege.
- Privileged users lacking separation between administrative and daily-use identities.
- Delegated password reset, group modification, GPO, replication, PKI, backup, virtualization, or endpoint-management control.
- Privileged authentication to lower-trust systems and unconstrained delegation exposure.
- Emergency accounts: monitoring, credential custody, exclusions, and testing.

## Evidence and verdict

Record SID, DN, source group/path, whether privilege is direct or transitive, account state, credential age, last-logon caveats, protected status, and the exact boundary-changing operation. Do not use `lastLogonTimestamp` as a precise sign-in time.

## Remediation

Remove unnecessary paths, separate admin identities, use time-bound privilege where available, protect service accounts, restrict administrative logon, monitor emergency accounts, and correct stale protected ACL state only after confirming intended inheritance.

## Retest

Recalculate nested membership and effective rights, confirm removed SIDs no longer appear, and validate that operational owners retain required access.
