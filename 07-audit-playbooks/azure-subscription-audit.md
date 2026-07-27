---
title: Azure subscription audit
platform: cloud
technology: azure
operational-risk: authenticated
status: research-note
last-tested:
references: []
---

# Azure subscription audit

## Objective

Assess governance, IAM, public exposure, compute, storage, secrets, applications, data services, containers, automation, monitoring, backup, and deployment paths across every accessible subscription.

## Workflow

1. Enumerate accessible tenants, management groups, and subscriptions; preserve context in every record.
2. Record inaccessible or excluded subscriptions and the reason.
3. Inventory resources in bulk with Resource Graph, then supplement types and properties unavailable there through ARM or service APIs.
4. Review policy, locks, diagnostic settings, Defender settings, networking, public endpoints, identities, role assignments, encryption, backup, and recovery controls.
5. Assess service-specific control-plane and data-plane access independently.
6. Identify indirect execution or credential paths through extensions, automation, deployment, identities, disks, snapshots, app settings, and pipelines.
7. Manually validate high-risk exposures and record environmental blockers.
8. Perform approved controlled validation, clean up, confirm restoration, report, and retest.

## Coverage ledger

For each subscription record tenant, subscription ID, display name, collection principal, accessible APIs, query failures, pagination state, collection time, and status. Distinguish `no matching resources` from `not authorized to enumerate`.

## Data-source rule

Every check must identify whether Resource Graph, Azure CLI, Az PowerShell, ARM/REST, Microsoft Graph, portal, or a specialist collector is authoritative or merely convenient.
