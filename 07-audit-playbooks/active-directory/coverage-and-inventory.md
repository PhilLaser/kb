---
title: AD forest coverage and inventory
platform: windows-linux
technology: active-directory
required-access: domain-user
operational-risk: authenticated
status: partially-verified
last-tested:
references:
  - https://learn.microsoft.com/en-us/powershell/module/activedirectory/get-adforest
---

# AD forest coverage and inventory

## Objective

Prove which forests, domains, domain controllers, sites, trusts, and naming contexts were assessed. This check prevents a successful query against one DC from being mistaken for forest-wide coverage.

## Minimum access

A normal domain account usually reads the required directory metadata. Network access, DNS, time synchronization, and LDAP policy can still limit collection.

## Windows collection

```powershell
$SeedDC = "<dc-fqdn>"
$OutputDirectory = ".\evidence-ad-coverage"
New-Item -ItemType Directory -Path $OutputDirectory -Force | Out-Null

$RootDSE = Get-ADRootDSE -Server $SeedDC -Properties *
$Forest = Get-ADForest -Server $SeedDC
$Forest | ConvertTo-Json -Depth 5 | Set-Content "$OutputDirectory\forest.json"

$Coverage = foreach ($DomainName in $Forest.Domains) {
    try {
        $Domain = Get-ADDomain -Identity $DomainName -Server $SeedDC
        $Controllers = Get-ADDomainController -Filter * -Server $DomainName
        foreach ($DC in $Controllers) {
            [pscustomobject]@{
                Domain = $DomainName
                DomainMode = $Domain.DomainMode
                DC = $DC.HostName
                Site = $DC.Site
                IPv4Address = $DC.IPv4Address
                GlobalCatalog = $DC.IsGlobalCatalog
                ReadOnly = $DC.IsReadOnly
                OperatingSystem = $DC.OperatingSystem
                Status = "discovered"
                Error = $null
            }
        }
    } catch {
        [pscustomobject]@{Domain=$DomainName;Status="collection-failed";Error=$_.Exception.Message}
    }
}
$Coverage | Export-Csv "$OutputDirectory\domain-controllers.csv" -NoTypeInformation -Encoding UTF8
```

**Verification:** PowerShell syntax parsed; target execution untested. `New-Item` and evidence writes change only the assessor workstation.

## Linux collection

```bash
DOMAIN="<domain-fqdn>"
DC_FQDN="<dc-fqdn>"
BASE_DN="<domain-distinguished-name>"

dig +short SRV "_ldap._tcp.dc._msdcs.${DOMAIN}"
ldapsearch -LLL -Y GSSAPI -H "ldap://${DC_FQDN}" -s base -b "" \
  defaultNamingContext configurationNamingContext schemaNamingContext rootDomainNamingContext
ldapsearch -LLL -Y GSSAPI -H "ldap://${DC_FQDN}" -b "CN=Partitions,CN=Configuration,${BASE_DN}" \
  '(objectClass=crossRef)' nCName dnsRoot nETBIOSName systemFlags
```

The configuration naming context is not always `CN=Configuration,<current-domain-DN>` in a child domain. Prefer the value returned by RootDSE rather than constructing it; the command above must substitute the actual configuration DN.

NetExec context check:

```bash
nxc ldap "${DC_FQDN}" -d "${DOMAIN}" -u "<username>" -p '<password>'
```

## Evidence and verdict

Maintain one row per domain and DC with status `assessed`, `partial`, `unreachable`, `excluded`, or `insufficient-permission`. Record queried DC, collection identity, authentication method, time, and errors. An absent record from a failed domain query is a coverage gap, not proof of absence.

## Remediation and retest

This is a methodology control. Resolve DNS, routing, credentials, LDAP policy, or scope ambiguity; rerun collection and confirm every authorized domain has an explicit status.

## References

- Microsoft, [Get-ADForest](https://learn.microsoft.com/en-us/powershell/module/activedirectory/get-adforest), accessed 2026-07-27.
- Microsoft, [Get-ADRootDSE](https://learn.microsoft.com/en-us/powershell/module/activedirectory/get-adrootdse), accessed 2026-07-27.
