---
title: AD trusts and forest boundaries
platform: windows-linux
technology: active-directory-trusts
required-access: domain-user
operational-risk: authenticated
status: partially-verified
last-tested:
references: []
---

# AD trusts and forest boundaries

## Objective

Inventory trust direction, transitivity, authentication scope, SID filtering, trust keys, and reachable resources. A trust direction describes whose principals may be accepted; arrows in tools are frequently misread, so document the statement in words.

## Windows collection

```powershell
$DC = "<dc-fqdn>"
Get-ADTrust -Filter * -Server $DC -Properties * |
    Select-Object Name,Source,Target,Direction,TrustType,TrustAttributes,
        ForestTransitive,IntraForest,SelectiveAuthentication,
        SIDFilteringForestAware,SIDFilteringQuarantined,TGTDelegation |
    Export-Csv '.\ad-trusts.csv' -NoTypeInformation -Encoding UTF8

nltest.exe /domain_trusts /all_trusts /v
```

## Linux collection

```bash
DC_FQDN="<dc-fqdn>"
BASE_DN="<domain-distinguished-name>"

ldapsearch -LLL -Y GSSAPI -H "ldap://${DC_FQDN}" -b "CN=System,${BASE_DN}" \
  '(objectClass=trustedDomain)' cn trustPartner trustDirection trustType trustAttributes securityIdentifier
```

Raw numeric trust fields must be decoded against Microsoft schema/protocol documentation. Do not infer SID-filter or selective-authentication behavior from direction alone.

## Manual validation

For each trust answer:

1. Which side trusts identities from the other side?
2. Is the path domain, forest, external, realm, shortcut, or other type?
3. Is it transitive and over which namespace?
4. Are selective authentication and SID filtering configured?
5. Which resources grant `Allowed to authenticate` or other access?
6. Can DNS, Kerberos referrals, LDAP, and the target service be reached?
7. Are privileged groups, SID history, delegation, or shared management systems involved?

## Evidence and verdict

Retain both sides' observations where authorized. Label a trust path `configured`, `reachable`, `authenticated`, `authorized`, or `boundary-crossing`; these are separate states. Record unreachable or out-of-scope partner forests explicitly.

## Remediation and retest

Remove obsolete trusts, narrow direction/transitivity, use selective authentication where suitable, enforce appropriate SID filtering, constrain DNS/network paths, protect trust credentials, and monitor changes. Retest from both sides with representative low-privilege identities.
