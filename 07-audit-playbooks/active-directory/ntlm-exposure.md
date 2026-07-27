---
title: AD NTLM exposure and relay resistance
platform: windows-linux
technology: ntlm
required-access: domain-user-plus-host-read
operational-risk: authenticated
status: partially-verified
last-tested:
references: []
---

# AD NTLM exposure and relay resistance

## Objective

Measure actual NTLM use and the controls that prevent capture, downgrade, and relay. Do not reduce the verdict to SMB signing alone.

## Windows collection

```powershell
$Targets = Get-Content '.\authorized-windows-targets.txt'
$Results = foreach ($Target in $Targets) {
    try {
        $Session = New-CimSession -ComputerName $Target
        $Server = Get-SmbServerConfiguration -CimSession $Session
        [pscustomobject]@{
            Target = $Target
            RequireSmbSigning = $Server.RequireSecuritySignature
            EnableSmbSigning = $Server.EnableSecuritySignature
            EncryptData = $Server.EncryptData
            Status = 'queried'
        }
        Remove-CimSession $Session
    } catch {
        [pscustomobject]@{Target=$Target;Status='collection-failed';Error=$_.Exception.Message}
    }
}
$Results | Export-Csv '.\smb-security-settings.csv' -NoTypeInformation -Encoding UTF8
```

CIM access requires additional host rights and network policy. A failed query is a coverage gap.

On authorized hosts/DCs, review NTLM operational logs and effective policy rather than registry keys alone:

```powershell
Get-WinEvent -FilterHashtable @{LogName='Microsoft-Windows-NTLM/Operational'} -ErrorAction SilentlyContinue |
    Select-Object TimeCreated,Id,MachineName,Message |
    Export-Csv '.\ntlm-operational-events.csv' -NoTypeInformation -Encoding UTF8
```

## Linux and NetExec collection

```bash
TARGETS="<authorized-target-or-file>"
nxc smb "${TARGETS}" --gen-relay-list relay-candidates.txt
nxc smb --help
```

Confirm this option against the installed version. Supplement candidates with LDAP signing/channel-binding policy, HTTP/IIS EPA, MSSQL and other protocol settings, TLS termination, coercion surfaces, and the privileges of identities likely to authenticate.

## Verdict dimensions

- NTLM versions permitted and observed.
- Inbound/outbound/domain NTLM audit or restriction policy.
- SMB signing required by client and server.
- LDAP signing and channel binding.
- EPA on HTTP-integrated authentication endpoints.
- Name-resolution poisoning exposure.
- Coercion sources and network reachability.
- Local administrator password uniqueness.
- Privileged authentication restrictions.

## Evidence and remediation

Retain per-target protocol settings, telemetry window, observed NTLM consumers, candidate identity classes, and untested controls. Inventory dependencies before restriction; then require signing/binding, deploy EPA, correct SPNs so Kerberos can work, remove poisoning/coercion exposure, use Windows LAPS, and restrict privileged logon. Retest representative applications and relay preconditions separately.

## References

- Microsoft, [NTLM overview](https://learn.microsoft.com/en-us/windows-server/security/kerberos/ntlm-overview), accessed 2026-07-27.
