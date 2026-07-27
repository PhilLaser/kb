---
title: AD CS inventory and certificate-template risk
platform: windows-linux
technology: active-directory-certificate-services
required-access: domain-user
operational-risk: authenticated
status: partially-verified
last-tested:
references: []
---

# AD CS inventory and certificate-template risk

## Objective

Inventory enterprise CAs, enrollment endpoints, published templates, template/CA ACLs, issuance requirements, subject construction, authentication uses, and relay protections. An ESC label is a taxonomy shortcut; the report must retain the underlying conditions.

## Windows collection

```powershell
$DC = "<dc-fqdn>"
$RootDSE = Get-ADRootDSE -Server $DC
$ConfigurationDN = $RootDSE.configurationNamingContext
$EnrollmentBase = "CN=Enrollment Services,CN=Public Key Services,CN=Services,$ConfigurationDN"
$TemplateBase = "CN=Certificate Templates,CN=Public Key Services,CN=Services,$ConfigurationDN"

Get-ADObject -LDAPFilter '(objectClass=pKIEnrollmentService)' -SearchBase $EnrollmentBase -Server $DC -Properties * |
    Select-Object Name,dNSHostName,certificateTemplates,flags,DistinguishedName

Get-ADObject -LDAPFilter '(objectClass=pKICertificateTemplate)' -SearchBase $TemplateBase -Server $DC -Properties * |
    Select-Object Name,displayName,msPKI-Enrollment-Flag,msPKI-Certificate-Name-Flag,
        pKIExtendedKeyUsage,msPKI-RA-Signature,msPKI-Template-Schema-Version,
        msPKI-Private-Key-Flag,nTSecurityDescriptor
```

Resolve template and CA security descriptors using the effective-rights workflow. Reading every property can be large; export raw objects to protected evidence storage.

Native CA discovery from an authorized Windows host:

```powershell
certutil.exe -config - -ping
certutil.exe -template
```

These commands may prompt or depend on installed components and current domain context.

## Linux collection

```bash
certipy find -h
certipy find -u '<username>@<domain-fqdn>' -p '<password>' -dc-ip '<dc-ip>' -enabled -stdout
```

**Verification:** version-sensitive example; confirm installed Certipy help. Prefer Kerberos or another protected secret-input method where supported. Do not interpret `Vulnerable: True` without retaining the condition, principals with enroll/control rights, issuing CA, and target authentication impact.

## Required analysis

- Enterprise versus standalone and root versus subordinate CA role.
- CA host, service identity, key protection, backup, web/RPC enrollment, and publication points.
- Published/enabled templates and which CAs issue them.
- Read, Enroll, Autoenroll, ownership, DACL modification, and template modification rights.
- Subject/SAN supply, authentication EKUs/application policies, manager approval, authorized signatures, issuance policy, and key archival/export settings.
- CA officer/manager rights and CA configuration flags.
- HTTP enrollment relay protection and RPC packet privacy.
- Revocation, auditing, certificate lifetime, and issued-certificate review.

## Evidence and remediation

Record raw flags plus decoded meaning, template and CA ACLs, effective enrolling principals, published CA, enrollment transport, exact boundary impact, and validation state. Remove unnecessary enrollment/control rights, require approval/signatures where appropriate, constrain subject construction and authentication purposes, protect CA administration/keys, require transport protections, enable auditing, and retest both enrollment and dependent applications.

## References

- Microsoft, [Certificate template concepts](https://learn.microsoft.com/en-us/windows-server/identity/ad-cs/certificate-template-concepts), accessed 2026-07-27.
- Microsoft, [Manage certificate templates](https://learn.microsoft.com/en-us/windows-server/identity/ad-cs/manage-certificate-templates), accessed 2026-07-27.
- Microsoft Defender for Identity, [Security assessment: Certificates](https://learn.microsoft.com/en-us/defender-for-identity/security-assessment-insecure-adcs-certificate-enrollment), accessed 2026-07-27.
