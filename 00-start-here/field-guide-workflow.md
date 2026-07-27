---
title: Field guide workflow
status: verified
last-tested: 2026-07-27
references: []
---

# Field guide workflow

Use this workflow whenever a technology or service is encountered during an authorized assessment. Individual pages may add protocol-specific steps but should not omit these decisions.

## 1. Establish context

Record the target, authorization boundary, source host, source identity, network path, time window, evidence location, and prohibited actions. Preserve exact tenant, domain, subscription, forest, host, and service context. Never let a tool's current context silently define scope.

## 2. Identify the service

Confirm the service independently of the port number. Capture DNS names, certificates, banners, protocol negotiation, authentication schemes, and redirects. A reachable listener is not proof that the expected application owns it.

## 3. Select an access lane

| Lane | Typical purpose | Examples |
|---|---|---|
| Passive or unauthenticated | Establish exposure without credentials | DNS, TLS metadata, RootDSE fields permitted anonymously |
| Authenticated low privilege | Measure normal-user visibility and effective controls | AD LDAP queries, SMB policy collection, Graph inventory |
| Privileged audit | Read configuration unavailable to ordinary principals | SACLs, protected settings, tenant-wide policy |
| Controlled validation | Prove a reachable boundary crossing | Approved temporary object, reversible role change, test credential |

Do not use privileged access as the only lane when the engagement needs to describe what an attacker with lower access could observe.

## 4. Enumerate in layers

1. Confirm name resolution, routing, time, and TLS.
2. Identify protocol and authentication support.
3. Enumerate the smallest stable inventory.
4. Expand configuration and effective authorization.
5. Correlate identities, credentials, permissions, and execution surfaces.
6. Identify gaps caused by permissions, filtering, paging, timeouts, or unsupported APIs.

## 5. Use equivalent command lanes

Pages should provide, where the protocol permits:

- Windows-native or PowerShell commands;
- Linux-native protocol clients or Python tooling;
- a common assessment tool such as NetExec;
- API or raw-protocol alternatives when they improve coverage.

Equivalent does not mean identical. Record differences in defaults, paging, authentication, signing, TLS validation, and returned attributes.

## 6. Interpret before validating

For every candidate issue distinguish:

- configuration observed;
- permission present;
- permission usable from the assessment context;
- security boundary crossing possible;
- path theoretical;
- path manually validated;
- path validated through controlled exploitation;
- path blocked by a named control.

## 7. Preserve evidence

Record command or query, tool and version, source identity, target, timestamp, exit status, raw output location, relevant excerpt, interpretation, and coverage limitation. Prefer structured formats. Screenshots support evidence but should not replace machine-readable output.

## 8. Restore and retest

Inventory intended artefacts before validation. Capture the prior state, make the minimum approved change, remove it, query the resulting state independently, and document anything that could not be restored.

## Command verification labels

- **Tested:** executed in the stated version and environment.
- **Syntax-verified:** checked against current official help or documentation but not executed against a target.
- **Untested:** requires a representative environment or remains research work.

Never describe a command as tested merely because its syntax parses locally.
