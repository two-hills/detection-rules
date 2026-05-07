# Sigma → SIEM Translation Notes

## Tool: sigconverter.io

Web frontend for `sigma-cli`. Paste Sigma → pick target → get SIEM-specific query.

## Key concept: Pipelines are field-mapping translators

Sigma uses generic field names (`Image`, `CommandLine`, `ParentImage`).
Real SIEMs use vendor-specific field names. Pipelines translate between them.

| Pipeline | Target | Field naming |
|----------|--------|--------------|
| `microsoft_xdr` | Defender XDR | `DeviceProcessEvents`, `FolderPath`, `ProcessCommandLine` |
| `windows-audit` | Native Windows Event 4688 | `NewProcessName`, `CommandLine`, `ParentProcessName` |
| `sysmon` | Sysmon-instrumented hosts | `EventID=1`, `Image`, `ParentImage` |
| (none) | Generic Sigma fields | Won't work in any real SIEM without mapping |

## Lesson: A Sigma rule + the right pipeline = a working detection.
A Sigma rule alone = abstract logic that does nothing.

## KQL vs SPL — style differences

KQL (Kusto) — verbose, piped, explicit:
- Each operation on its own `| where` line
- Each operator named (`contains`, `endswith`)
- Easier to read for newcomers

SPL (Splunk) — terse, dense:
- Spaces between conditions = AND (implicit)
- Wildcards `*` baked into syntax
- `IN` operator for compact OR lists
- Steeper learning curve, faster to write

Neither is "better." Pick based on team familiarity.

## Common gotchas

- KQL column names are case-sensitive (`FileName`, not `Filename`)
- SPL escapes backslashes in patterns (`*\\mstsc.exe`)
- Auto-conversion is a starting point, not a finished product — always tune
- Pipeline-less output looks correct but won't match real data

## Data path mental model

The detection engineer thinks about the full path:
- Sentinel path: Windows → Azure Monitor Agent → Log Analytics workspace → KQL
- Splunk path: Windows → Universal Forwarder → Splunk indexer → SPL

Sigma works as the abstract source of truth because it doesn't care
which path the data took.

## Why this matters for AI labs

AI labs (OpenAI, Anthropic) typically run mixed environments:
- Sentinel/Defender for Microsoft 365 + Azure
- Splunk for SOC operations
- Falco for Kubernetes

Detection engineers who only know one query language are vendor-locked.
Detection engineers who write Sigma + know multiple targets are vendor-portable.

This is the philosophy behind "detection-as-code" — write once, deploy many.