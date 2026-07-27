# Table of contents

* [Home](README.md)
* [Repository roadmap](00-start-here/repository-roadmap.md)
* [Field guide workflow](00-start-here/field-guide-workflow.md)

## Foundations

* [Authentication, authorization, and effective access](01-foundations/authentication-authorization-and-effective-access.md)
* [Kerberos protocol and ticket flow](01-foundations/kerberos-protocol-and-ticket-flow.md)
* [NTLM authentication and relay boundaries](01-foundations/ntlm-authentication-and-relay-boundaries.md)

## Active Directory

* [Section overview](05-active-directory/README.md)
* [Directory structure and discovery](05-active-directory/directory-structure-and-discovery.md)
* [Security descriptors and effective rights](05-active-directory/ad-security-descriptors-and-effective-rights.md)

## Azure and Entra ID

* [Section overview](06-azure-and-entra/README.md)

## Audit playbooks

* [Active Directory forest audit](07-audit-playbooks/active-directory-forest-audit.md)
  * [Coverage and inventory](07-audit-playbooks/active-directory/coverage-and-inventory.md)
  * [Privileged identities](07-audit-playbooks/active-directory/privileged-identities.md)
  * [Trusts and boundaries](07-audit-playbooks/active-directory/trusts-and-boundaries.md)
  * [Kerberos and delegation](07-audit-playbooks/active-directory/kerberos-and-delegation.md)
  * [NTLM exposure](07-audit-playbooks/active-directory/ntlm-exposure.md)
  * [AD CS inventory and template risk](07-audit-playbooks/active-directory/ad-cs-inventory-and-template-risk.md)
  * [Group Policy security](07-audit-playbooks/active-directory/group-policy-security.md)
  * [Credential and service-account hygiene](07-audit-playbooks/active-directory/credential-and-service-account-hygiene.md)
* [Entra ID tenant audit](07-audit-playbooks/entra-id-tenant-audit.md)
* [Azure subscription audit](07-audit-playbooks/azure-subscription-audit.md)
* [Azure IAM and privilege-path audit](07-audit-playbooks/azure-iam-privilege-path-audit.md)

## Reference

* [Source registry](13-reference/source-registry.md)
* [Prioritized backlog](13-reference/prioritized-backlog.md)

## Project standards

* [Contributing](CONTRIBUTING.md)
* [Style guide](STYLE-GUIDE.md)
* [Changelog](CHANGELOG.md)

## Legacy content awaiting migration

* [Active Directory legacy root](windows/active-directory/active-directory-enumeration.md)
  * [AD CS](windows/active-directory/adcs-abuse/README.md)
  * [Relaying](windows/active-directory/relaying/README.md)
  * [Credentials](windows/active-directory/credentials/README.md)
  * [Kerberos](windows/active-directory/kerberos/README.md)
  * [Lateral movement](windows/active-directory/lateral-movement/README.md)
  * [Persistence](windows/active-directory/persistence/README.md)
  * [SCCM](windows/active-directory/sccm.md)
  * [LAPS](windows/active-directory/laps.md)
* [Windows privilege escalation](windows/privilege-escalation.md)
* [Windows persistence](windows/persistence/README.md)
* [Linux placeholder](linux/page-2/README.md)
* [Malware-development placeholder](maldev/page-3.md)
