---
title: AD Kerberos policy and delegation audit
platform: windows-linux
technology: kerberos
required-access: domain-user
operational-risk: authenticated
status: partially-verified
last-tested:
references:
  - https://learn.microsoft.com/en-us/windows/win32/adschema/a-msds-allowedtoactonbehalfofotheridentity
---

# AD Kerberos policy and delegation audit

## Objective

Identify weak encryption, pre-authentication exceptions, risky SPN ownership, and unconstrained, constrained, or resource-based constrained delegation. Determine which identities and target services make each configuration meaningful.

## Windows collection

```powershell
$DC = "<dc-fqdn>"

Get-ADUser -LDAPFilter '(|(servicePrincipalName=*)(userAccountControl:1.2.840.113556.1.4.803:=4194304)(userAccountControl:1.2.840.113556.1.4.803:=524288))' -Server $DC -Properties servicePrincipalName,userAccountControl,msDS-SupportedEncryptionTypes,msDS-AllowedToDelegateTo,AccountNotDelegated,ProtectedFromAccidentalDeletion |
    Select-Object SamAccountName,Enabled,userAccountControl,AccountNotDelegated,msDS-SupportedEncryptionTypes,servicePrincipalName,msDS-AllowedToDelegateTo

Get-ADComputer -LDAPFilter '(|(userAccountControl:1.2.840.113556.1.4.803:=524288)(msDS-AllowedToDelegateTo=*)(msDS-AllowedToActOnBehalfOfOtherIdentity=*))' -Server $DC -Properties userAccountControl,msDS-AllowedToDelegateTo,msDS-AllowedToActOnBehalfOfOtherIdentity,msDS-SupportedEncryptionTypes,servicePrincipalName |
    Select-Object Name,userAccountControl,msDS-SupportedEncryptionTypes,servicePrincipalName,msDS-AllowedToDelegateTo,msDS-AllowedToActOnBehalfOfOtherIdentity
```

The bitwise LDAP matching rule is AD-specific. Decode `userAccountControl` flags rather than treating the integer as self-explanatory.

## Linux collection

```bash
DC_FQDN="<dc-fqdn>"
BASE_DN="<domain-distinguished-name>"

ldapsearch -LLL -Y GSSAPI -H "ldap://${DC_FQDN}" -b "${BASE_DN}" \
  '(|(userAccountControl:1.2.840.113556.1.4.803:=524288)(msDS-AllowedToDelegateTo=*)(msDS-AllowedToActOnBehalfOfOtherIdentity=*))' \
  distinguishedName objectSid userAccountControl servicePrincipalName \
  msDS-AllowedToDelegateTo msDS-AllowedToActOnBehalfOfOtherIdentity msDS-SupportedEncryptionTypes
```

`msDS-AllowedToActOnBehalfOfOtherIdentity` is a binary security descriptor. A text LDAP client may not render it usefully; preserve the raw value with a capable collector and resolve trustees.

## Interpretation

- **Unconstrained delegation:** determine whether the host/service can receive forwardable user TGTs and whether protected identities are exposed.
- **Constrained delegation:** resolve every allowed target SPN, protocol-transition setting, source service account, and target service boundary.
- **RBCD:** decode the target object's security descriptor and identify principals allowed to act; then evaluate who controls those principals.
- **No pre-authentication:** identify enabled accounts, key strength, privilege, and monitoring; do not request crackable material by default.
- **SPN accounts:** evaluate password/key management, encryption type, privilege, and service reachability.

## Evidence

Record source principal, target SPN/object, delegation type, raw attributes/security descriptor, resolved trustees, encryption settings, protected-user/account-not-delegated state, theoretical versus reachable path, and environmental blockers.

## Remediation and retest

Remove obsolete delegation, prefer narrowly scoped resource-based designs where operationally suitable, protect privileged identities, use managed service accounts, remove unnecessary SPNs, require pre-authentication, and phase out weak encryption. Requery attributes and validate service operation after change.

## References

- Microsoft, [ms-DS-Allowed-To-Act-On-Behalf-Of-Other-Identity attribute](https://learn.microsoft.com/en-us/windows/win32/adschema/a-msds-allowedtoactonbehalfofotheridentity), accessed 2026-07-27.
- Microsoft, [Kerberos authentication overview](https://learn.microsoft.com/en-us/windows-server/security/kerberos/kerberos-authentication-overview), accessed 2026-07-27.
