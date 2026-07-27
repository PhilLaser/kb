---
title: AD credential and service-account hygiene
platform: windows-linux
technology: active-directory
required-access: domain-user
operational-risk: authenticated
status: research-note
last-tested:
references: []
---

# AD credential and service-account hygiene

## Objective

Identify password-policy gaps, stale or risky accounts, service-account key exposure, password-equivalent material, and unsafe credential placement without collecting secrets by default.

## Windows collection

```powershell
$DC = "<dc-fqdn>"
Get-ADDefaultDomainPasswordPolicy -Server $DC | Format-List *
Get-ADFineGrainedPasswordPolicy -Filter * -Server $DC |
    Select-Object Name,Precedence,MinPasswordLength,PasswordHistoryCount,
        MaxPasswordAge,MinPasswordAge,LockoutThreshold,LockoutDuration,
        ReversibleEncryptionEnabled

Get-ADUser -Filter * -Server $DC -Properties Enabled,PasswordNeverExpires,PasswordNotRequired,DoesNotRequirePreAuth,PasswordLastSet,lastLogonTimestamp,servicePrincipalName,userAccountControl |
    Select-Object SamAccountName,Enabled,PasswordNeverExpires,PasswordNotRequired,
        DoesNotRequirePreAuth,PasswordLastSet,lastLogonTimestamp,servicePrincipalName,userAccountControl |
    Export-Csv '.\ad-account-hygiene.csv' -NoTypeInformation -Encoding UTF8

Get-ADServiceAccount -Filter * -Server $DC -Properties * |
    Select-Object Name,SamAccountName,Enabled,HostComputers,PrincipalsAllowedToRetrieveManagedPassword,
        ManagedPasswordIntervalInDays,servicePrincipalName
```

## Linux collection

```bash
DC_FQDN="<dc-fqdn>"
BASE_DN="<domain-distinguished-name>"

ldapsearch -LLL -Y GSSAPI -H "ldap://${DC_FQDN}" -b "${BASE_DN}" \
  '(&(objectCategory=person)(objectClass=user))' sAMAccountName userAccountControl \
  pwdLastSet lastLogonTimestamp servicePrincipalName msDS-ResultantPSO

ldapsearch -LLL -Y GSSAPI -H "ldap://${DC_FQDN}" -b "${BASE_DN}" \
  '(|(objectClass=msDS-GroupManagedServiceAccount)(objectClass=msDS-ManagedServiceAccount))' \
  sAMAccountName servicePrincipalName msDS-ManagedPasswordInterval \
  msDS-GroupMSAMembership
```

The gMSA membership attribute is a security descriptor; decode it to identify principals allowed to retrieve managed passwords. Do not request managed password material merely to prove the right exists.

## Review criteria

- Default and fine-grained password policy coverage and precedence.
- Enabled accounts with password-not-required, no pre-authentication, reversible encryption, non-expiring passwords, or stale keys.
- Service accounts using human-managed passwords where gMSA or another managed identity is feasible.
- SPN accounts with excessive privilege or weak key management.
- gMSA password-retrieval rights granted to overly broad or controllable principals.
- Local administrator password reuse and Windows LAPS coverage.
- Secrets in scripts, SYSVOL, scheduled tasks, service configuration, deployment systems, backups, and documentation.

## Evidence and remediation

Retain account state and relevant attributes, policy source, timestamps with replication caveats, effective gMSA retrieval trustees, privilege relationships, and secret-location metadata without copying secret values unnecessarily. Disable/remove stale identities, adopt managed accounts, scope retrieval rights, rotate exposed credentials, deploy Windows LAPS, remove reversible storage, and retest dependent services.
