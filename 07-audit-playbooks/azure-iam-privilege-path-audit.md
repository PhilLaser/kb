---
title: Azure IAM and privilege-path audit
platform: cloud
technology: azure-entra
operational-risk: authenticated
status: research-note
last-tested:
references: []
---

# Azure IAM and privilege-path audit

## Objective

Determine effective Azure and Entra privilege and identify reachable security-boundary crossings, including indirect control over identities, execution surfaces, credentials, deployments, and authorization state.

## Analysis model

Model principals, groups, applications, service principals, managed identities, scopes, resources, role definitions, assignments, ownership, PIM eligibility, consent grants, and cross-tenant delegation as a graph. Expand custom-role actions and inherited scope. Keep control-plane and data-plane permissions distinct.

For every candidate path label:

- permission present;
- permission usable with current authentication and conditions;
- boundary crossing possible;
- theoretical path;
- manually validated path;
- controlled-exploitation validated path; or
- blocked by a named environmental control.

## Workflow

1. Inventory principals and group expansion, including guests and foreign principals.
2. Collect role definitions and assignments at management-group through resource scope.
3. Collect deny assignments, locks, PIM eligibility/activation controls, Lighthouse delegations, and relevant policy constraints.
4. Correlate application/service-principal ownership, managed identities, federated credentials, and Graph grants.
5. Identify rights to create assignments or modify compute, automation, deployment, credentials, secrets, networking, or identity attachment.
6. Validate token audience, scope, conditions, provider registration, and target existence.
7. Rank paths by reachability, impact, noise, reversibility, and evidence strength.
8. Validate minimally, restore state, and preserve before/after evidence.

## Known gaps

Exact permissions matrices and service-specific privilege edges require separate version-tested pages.
