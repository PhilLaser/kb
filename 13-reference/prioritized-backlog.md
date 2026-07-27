# Prioritized backlog: next 50 pages

Priority reflects dependency value for audit playbooks, not topic popularity.

## P0 — shared foundations

1. Authentication versus authorization
2. Principals, credentials, tokens, and sessions
3. Security boundaries and effective access
4. Windows access tokens and privileges
5. Windows security descriptors and access checks
6. Active Directory objects, attributes, and partitions
7. Active Directory ACLs, ownership, and extended rights
8. Kerberos architecture and ticket flow
9. Kerberos PAC and authorization data
10. NTLM challenge-response, signing, and channel binding
11. OAuth 2.0 and token anatomy
12. Entra applications and service principals
13. Azure hierarchy and Resource Manager
14. Azure RBAC evaluation and inheritance
15. Azure control plane versus data plane
16. Managed identities and token acquisition
17. Linux users, groups, modes, and ACLs
18. Linux SUID, SGID, capabilities, and execution context

## P1 — audit execution

19. AD forest audit permissions and evidence matrix
20. AD inventory and coverage check
21. AD privileged groups and delegated access check
22. AD trusts and SID filtering check
23. AD Kerberos encryption and delegation check
24. AD NTLM exposure and relay-resistance check
25. AD CS inventory and template-risk check
26. Windows host audit overview
27. Linux host audit overview
28. Entra audit permissions and evidence matrix
29. Entra users, guests, and stale identities check
30. Entra privileged roles and PIM check
31. Entra authentication methods and MFA check
32. Conditional Access coverage and exclusions check
33. Entra applications, credentials, and owners check
34. Entra consent and Graph permissions check
35. Azure subscription permissions and evidence matrix
36. Azure Resource Graph inventory and pagination
37. Azure role assignments and custom roles check
38. Azure public exposure check
39. Azure VM execution and credential paths check
40. Azure Storage exposure and authorization check
41. Key Vault authorization and recovery-control check
42. Azure diagnostic settings and logging coverage check

## P2 — referenced techniques and tools

43. Kerberoasting: protocol, audit, and controlled validation
44. AS-REP roasting: protocol, audit, and controlled validation
45. Resource-based constrained delegation
46. NTLM relay boundary analysis
47. AD CS certificate-template abuse taxonomy
48. Azure managed-identity privilege paths
49. BloodHound and SharpHound tool reference
50. AzureHound and cloud graph collection reference
