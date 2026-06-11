# Synthetic Data Schema

## Purpose

This document defines the synthetic Intune-like data model for the public demo version of the Intune Security Copilot Readiness Agent.

The schema is designed to support the MVP dashboard without requiring access to a real tenant. All sample data created from this schema must be fictional and safe to publish.

## Design principles

- Use realistic Intune-style fields without copying real tenant records.
- Keep the schema simple enough for a local demo.
- Preserve clear mappings to future Microsoft Graph and Intune reporting API sources.
- Support both dashboard metrics and AI-generated recommendations.
- Avoid names, device IDs, users, tenant IDs, or values copied from real environments.

## Dataset overview

| Dataset | File | Primary purpose |
|---|---|---|
| Device inventory | `devices.json` | Device estate, platform counts, OS/build distribution, stale device detection |
| Compliance | `compliance.json` | Compliance breakdown, failed policies, failed settings, non-compliance age |
| Windows updates | `windows-updates.json` | Patch posture, update ring failures, missing critical updates |
| App deployments | `app-deployments.json` | Latest app deployment failures and error clusters |
| Configuration profiles | `configuration-profiles.json` | Policy/profile success, error, conflict, and high-impact failures |
| Security posture | `security-posture.json` | Defender health, encryption, TPM, Secure Boot, exposure level |

## Shared conventions

### Identifiers

Use fictional IDs with stable prefixes:

```text
demo-device-001
demo-policy-001
demo-app-001
demo-profile-001
```

### Dates

Use ISO 8601 timestamps:

```text
2026-06-10T09:15:00Z
```

### Platforms

Allowed platform values:

```text
Windows
macOS
iOS
iPadOS
Android
ChromeOS
Linux
Unknown
```

### Severity

Allowed severity values:

```text
Critical
High
Medium
Low
Informational
```

## Device inventory schema

File:

```text
data/synthetic/devices.json
```

Purpose:

Supports platform counts, operating system distribution, stale device checks, ownership segmentation, and future readiness scoring.

Fields:

| Field | Type | Required | Description |
|---|---|---:|---|
| `deviceId` | string | Yes | Synthetic stable device identifier |
| `deviceName` | string | Yes | Fictional device name |
| `platform` | string | Yes | Device platform |
| `osVersion` | string | Yes | Operating system version or build |
| `ownership` | string | Yes | `Corporate`, `Personal`, or `Unknown` |
| `enrollmentType` | string | Yes | Example: `Autopilot`, `UserEnrollment`, `AndroidEnterprise`, `AppleADE`, `Manual` |
| `primaryUser` | string | No | Fictional user alias or `null` |
| `department` | string | No | Fictional department for grouping |
| `lastCheckInDateTime` | string | Yes | Last device check-in timestamp |
| `enrollmentDateTime` | string | Yes | Enrollment timestamp |
| `managementState` | string | Yes | `Managed`, `RetirePending`, `WipePending`, `Unmanaged`, `Unknown` |

Example:

```json
{
  "deviceId": "demo-device-001",
  "deviceName": "DEMO-WIN11-001",
  "platform": "Windows",
  "osVersion": "10.0.26100.6584",
  "ownership": "Corporate",
  "enrollmentType": "Autopilot",
  "primaryUser": "alex.demo",
  "department": "Finance",
  "lastCheckInDateTime": "2026-06-10T09:15:00Z",
  "enrollmentDateTime": "2025-11-04T03:30:00Z",
  "managementState": "Managed"
}
```

## Compliance schema

File:

```text
data/synthetic/compliance.json
```

Purpose:

Supports compliance breakdown, non-compliance triage, failed policy analysis, and AI recommendations.

Fields:

| Field | Type | Required | Description |
|---|---|---:|---|
| `deviceId` | string | Yes | Synthetic device identifier matching `devices.json` |
| `complianceState` | string | Yes | `Compliant`, `NonCompliant`, `InGracePeriod`, `NotEvaluated`, `NoPolicyAssigned`, `Unknown` |
| `policyId` | string | No | Synthetic compliance policy identifier |
| `policyName` | string | No | Fictional compliance policy name |
| `settingName` | string | No | Failed setting name |
| `failureReason` | string | No | Human-readable failure reason |
| `daysNonCompliant` | number | No | Number of days the device has been non-compliant |
| `lastEvaluationDateTime` | string | Yes | Last compliance evaluation timestamp |
| `severity` | string | No | Risk severity |

Example:

```json
{
  "deviceId": "demo-device-001",
  "complianceState": "NonCompliant",
  "policyId": "demo-policy-001",
  "policyName": "Windows Baseline Compliance",
  "settingName": "Require BitLocker",
  "failureReason": "Encryption is not enabled",
  "daysNonCompliant": 12,
  "lastEvaluationDateTime": "2026-06-10T09:20:00Z",
  "severity": "High"
}
```

## Windows updates schema

File:

```text
data/synthetic/windows-updates.json
```

Purpose:

Supports patch compliance, last-month update posture, failed update rings, and missing critical update analysis.

Fields:

| Field | Type | Required | Description |
|---|---|---:|---|
| `deviceId` | string | Yes | Synthetic Windows device identifier |
| `updateRing` | string | Yes | Fictional update ring name |
| `patchStatus` | string | Yes | `Compliant`, `MissingUpdates`, `Failed`, `PendingReboot`, `Unknown` |
| `qualityUpdateMonth` | string | Yes | Patch month in `YYYY-MM` format |
| `missingCriticalUpdateCount` | number | Yes | Count of missing critical/security updates |
| `lastScanDateTime` | string | Yes | Last update scan timestamp |
| `failureCode` | string | No | Synthetic or common-looking update failure code |
| `failureSummary` | string | No | Human-readable failure summary |
| `severity` | string | No | Risk severity |

Example:

```json
{
  "deviceId": "demo-device-001",
  "updateRing": "Windows Pilot Ring",
  "patchStatus": "Failed",
  "qualityUpdateMonth": "2026-05",
  "missingCriticalUpdateCount": 3,
  "lastScanDateTime": "2026-06-09T22:10:00Z",
  "failureCode": "0x80244022",
  "failureSummary": "Windows Update service endpoint unavailable",
  "severity": "High"
}
```

## App deployments schema

File:

```text
data/synthetic/app-deployments.json
```

Purpose:

Supports top failed app deployments, app install success rate, error-code grouping, and impacted platform analysis.

Fields:

| Field | Type | Required | Description |
|---|---|---:|---|
| `appId` | string | Yes | Synthetic app identifier |
| `appName` | string | Yes | Fictional or generic app name |
| `appVersion` | string | No | App version |
| `platform` | string | Yes | Target platform |
| `assignmentGroup` | string | Yes | Fictional assignment group |
| `installStatus` | string | Yes | `Installed`, `Failed`, `Pending`, `NotApplicable`, `Unknown` |
| `failureCode` | string | No | App deployment error code |
| `failureSummary` | string | No | Human-readable failure summary |
| `affectedDeviceCount` | number | Yes | Number of impacted devices |
| `lastFailureDateTime` | string | No | Most recent failure timestamp |
| `severity` | string | No | Risk severity |

Example:

```json
{
  "appId": "demo-app-001",
  "appName": "Contoso VPN Client",
  "appVersion": "5.4.2",
  "platform": "Windows",
  "assignmentGroup": "All Corporate Windows Devices",
  "installStatus": "Failed",
  "failureCode": "0x87D1041C",
  "failureSummary": "App requirement rule not met",
  "affectedDeviceCount": 34,
  "lastFailureDateTime": "2026-06-10T04:45:00Z",
  "severity": "High"
}
```

## Configuration profiles schema

File:

```text
data/synthetic/configuration-profiles.json
```

Purpose:

Supports configuration drift detection, top failing profile analysis, conflict detection, and recommended remediation.

Fields:

| Field | Type | Required | Description |
|---|---|---:|---|
| `profileId` | string | Yes | Synthetic configuration profile identifier |
| `profileName` | string | Yes | Fictional configuration profile name |
| `settingName` | string | No | Setting with failure or conflict |
| `platform` | string | Yes | Target platform |
| `assignmentGroup` | string | Yes | Fictional assignment group |
| `status` | string | Yes | `Success`, `Error`, `Conflict`, `NotApplicable`, `Pending`, `Unknown` |
| `failureCode` | string | No | Configuration failure code |
| `failureSummary` | string | No | Human-readable failure summary |
| `affectedDeviceCount` | number | Yes | Number of impacted devices |
| `lastStatusDateTime` | string | Yes | Last status timestamp |
| `severity` | string | No | Risk severity |

Example:

```json
{
  "profileId": "demo-profile-001",
  "profileName": "Windows Security Baseline",
  "settingName": "Block legacy authentication",
  "platform": "Windows",
  "assignmentGroup": "All Corporate Windows Devices",
  "status": "Conflict",
  "failureCode": "PolicyConflict",
  "failureSummary": "Another assigned profile configures a different value",
  "affectedDeviceCount": 27,
  "lastStatusDateTime": "2026-06-10T07:30:00Z",
  "severity": "High"
}
```

## Security posture schema

File:

```text
data/synthetic/security-posture.json
```

Purpose:

Supports security health, Defender status, encryption gaps, hardware security readiness, and vulnerability exposure summaries.

Fields:

| Field | Type | Required | Description |
|---|---|---:|---|
| `deviceId` | string | Yes | Synthetic device identifier matching `devices.json` |
| `defenderStatus` | string | Yes | `Healthy`, `Unhealthy`, `Disabled`, `Unknown`, `NotApplicable` |
| `encryptionStatus` | string | Yes | `Encrypted`, `NotEncrypted`, `EncryptionInProgress`, `NotApplicable`, `Unknown` |
| `secureBootEnabled` | boolean | No | Secure Boot state |
| `tpmReady` | boolean | No | TPM readiness |
| `exposureLevel` | string | Yes | `Critical`, `High`, `Medium`, `Low`, `None`, `Unknown` |
| `activeThreatCount` | number | Yes | Count of active threats |
| `topRisk` | string | No | Human-readable top risk |
| `lastUpdatedDateTime` | string | Yes | Last security posture timestamp |

Example:

```json
{
  "deviceId": "demo-device-001",
  "defenderStatus": "Unhealthy",
  "encryptionStatus": "NotEncrypted",
  "secureBootEnabled": true,
  "tpmReady": true,
  "exposureLevel": "High",
  "activeThreatCount": 0,
  "topRisk": "Device is not encrypted and Defender health is degraded",
  "lastUpdatedDateTime": "2026-06-10T10:00:00Z"
}
```

## AI recommendation schema

File:

```text
data/synthetic/ai-recommendations.json
```

Purpose:

Stores generated or precomputed recommendation examples for the public demo. These can be produced by the local agent from synthetic dashboard signals.

Fields:

| Field | Type | Required | Description |
|---|---|---:|---|
| `recommendationId` | string | Yes | Synthetic recommendation identifier |
| `riskTitle` | string | Yes | Short title of the risk |
| `severity` | string | Yes | Risk severity |
| `affectedArea` | string | Yes | Example: `Compliance`, `Patching`, `Applications`, `Configuration`, `Security` |
| `evidence` | array | Yes | Supporting evidence strings from synthetic data |
| `likelyCause` | string | Yes | AI-style likely cause |
| `recommendedAction` | string | Yes | Recommended remediation |
| `suggestedOwner` | string | Yes | Example: `Endpoint engineering`, `Security operations`, `Application owner` |

Example:

```json
{
  "recommendationId": "demo-rec-001",
  "riskTitle": "Windows patch failures concentrated in the pilot ring",
  "severity": "High",
  "affectedArea": "Patching",
  "evidence": [
    "34 Windows devices failed the 2026-05 quality update",
    "Most failures are assigned to the Windows Pilot Ring",
    "Common failure code: 0x80244022"
  ],
  "likelyCause": "Windows Update service connectivity or ring configuration issue",
  "recommendedAction": "Validate update ring settings, confirm Windows Update connectivity, and remediate the top failing devices first.",
  "suggestedOwner": "Endpoint engineering"
}
```

## Future Microsoft Graph mapping

The public MVP uses synthetic data. A future lab or customer connector can map Microsoft Graph and Intune reporting API responses into this same schema.

| Synthetic dataset | Future Graph / Intune source pattern |
|---|---|
| `devices.json` | Microsoft Graph managed devices and Intune device inventory reports |
| `compliance.json` | Intune device compliance and policy compliance reports |
| `windows-updates.json` | Intune Windows update reports and Windows update for Business reporting |
| `app-deployments.json` | Intune app install status and app reporting exports |
| `configuration-profiles.json` | Intune device configuration profile status reports |
| `security-posture.json` | Intune endpoint security reports and Microsoft Defender signals |

## Validation rules

Synthetic sample data should follow these rules:

1. Every `deviceId` referenced by compliance, update, or security posture records must exist in `devices.json`.
2. At least four platforms must be represented: Windows, macOS, iOS/iPadOS, and Android.
3. At least 20 percent of devices should be non-compliant so the demo has meaningful findings.
4. At least three app deployment failures should exist.
5. At least three configuration profile failures or conflicts should exist.
6. At least five AI recommendations should be generated or included as examples.
7. No record should contain real tenant names, real device names, real user names, real customer names, or real IDs.
