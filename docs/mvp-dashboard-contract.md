# MVP Dashboard Contract

## Purpose

This document defines the first public-safe MVP for the Intune Security Copilot Readiness Agent.

The MVP starts with synthetic data and a local dashboard contract before adding Microsoft Graph, Intune reporting APIs, or Security Copilot integrations. This keeps the project safe to publish while still showing a realistic AI/LLM solution pattern for endpoint management.

## Product goal

Help endpoint and security teams understand Intune device-management risk across compliance, patching, applications, configuration profiles, and security posture.

The agent should not only show dashboard tiles. It should also explain the operational meaning of each signal and recommend next actions.

## Evidence base

The MVP dashboard is based on metric patterns that were historically useful in SCCM / Configuration Manager dashboards and reports:

- Client health
- Software update compliance
- Endpoint protection health
- Configuration baseline compliance
- OS and hardware inventory
- Application deployment failures
- Management Insights-style recommendations

The modern Intune version of these signals should use synthetic data first, then map to Microsoft Graph and Intune reporting APIs later.

## Dashboard contract

The MVP dashboard contains eight tiles.

| Tile | What it shows | Why it matters | Initial data source |
|---|---|---|---|
| Overall readiness score | A 0-100 composite score across compliance, patching, security, encryption, and stale-device health | Gives executives a single view of endpoint readiness | Calculated from synthetic summary data |
| Devices by platform | Device counts for Windows, macOS, Android, and iOS/iPadOS | Shows estate shape and helps prioritize platform-specific issues | Synthetic device inventory |
| Compliance breakdown | Compliant, non-compliant, in grace period, not evaluated, and no policy assigned | Avoids inflated compliance views by exposing denominator gaps | Synthetic compliance records |
| Windows patch posture | Last-month patch compliance, failed update rings, and devices missing critical updates | Patching is a top security and audit concern | Synthetic Windows update records |
| App deployment failures | Top failed apps, affected device count, failure code, platform, and last failure date | Helps IT teams triage business app and security agent rollout issues | Synthetic app deployment records |
| Configuration profile failures | Profiles and settings with the highest error or conflict counts | Surfaces configuration drift and policy rollout issues | Synthetic configuration profile records |
| Security health | Defender AV health, encryption gaps, and vulnerability exposure summary | Connects Intune operations to security outcomes | Synthetic security posture records |
| AI recommended actions | Top risks, likely causes, recommended remediation, and suggested owner | Demonstrates the LLM value layer beyond static reporting | Generated from dashboard signals |

## MVP user experience

The first version should run locally and use synthetic data.

Expected flow:

```text
Load synthetic Intune-like data
        |
        v
Calculate dashboard metrics
        |
        v
Generate AI-style findings and recommended actions
        |
        v
Display local dashboard/report
```

## Local-first design

The project should be runnable on a local machine. The first public version should not require customer tenant access.

The later real-world version can add authentication and data retrieval:

```text
Local app
        |
        v
Microsoft Entra ID authentication
        |
        v
Microsoft Graph / Intune reporting APIs
        |
        v
Local processing and optional cache
        |
        v
Dashboard and AI-generated recommendations
```

The app should not connect directly to an Intune database. Intune data should be retrieved through supported Microsoft Graph and Intune reporting APIs.

## Data model requirements

The synthetic data model should include enough fields to support the eight dashboard tiles.

### Device inventory

Minimum fields:

- `deviceId`
- `deviceName`
- `platform`
- `osVersion`
- `ownership`
- `lastCheckInDateTime`
- `primaryUser`
- `enrollmentDateTime`

### Compliance

Minimum fields:

- `deviceId`
- `complianceState`
- `policyName`
- `settingName`
- `failureReason`
- `daysNonCompliant`

### Windows update posture

Minimum fields:

- `deviceId`
- `updateRing`
- `patchStatus`
- `missingCriticalUpdateCount`
- `lastScanDateTime`
- `failureCode`

### App deployment

Minimum fields:

- `appName`
- `appVersion`
- `platform`
- `assignmentGroup`
- `installStatus`
- `failureCode`
- `affectedDeviceCount`
- `lastFailureDateTime`

### Configuration profiles

Minimum fields:

- `profileName`
- `settingName`
- `platform`
- `status`
- `failureCode`
- `affectedDeviceCount`
- `assignmentGroup`

### Security posture

Minimum fields:

- `deviceId`
- `defenderStatus`
- `encryptionStatus`
- `secureBootEnabled`
- `tpmReady`
- `exposureLevel`
- `activeThreatCount`

## AI recommendation contract

The AI layer should generate a concise recommendation object for each major issue.

Recommended shape:

```json
{
  "riskTitle": "Windows patch failures concentrated in Pilot Ring",
  "severity": "High",
  "affectedArea": "Windows patching",
  "evidence": [
    "42 devices failed last-month security updates",
    "Most failures are in the Pilot Ring",
    "Common failure code: 0x80244022"
  ],
  "likelyCause": "Update service or network path issue affecting devices in the ring",
  "recommendedAction": "Review update ring configuration, validate Windows Update connectivity, and remediate the top failing devices first.",
  "suggestedOwner": "Endpoint engineering"
}
```

## MVP acceptance criteria

The first MVP is complete when:

1. Synthetic data can populate all eight dashboard tiles.
2. The dashboard can be run locally without tenant access.
3. The output clearly distinguishes synthetic demo data from real customer data.
4. At least five AI-generated recommendations are produced from the dashboard signals.
5. The project documentation explains how each tile maps to a future Graph/Intune data source.

## Out of scope for the first MVP

- Direct connection to live customer tenants
- Real customer data
- Production Security Copilot plugin packaging
- Tenant-specific remediation automation
- Write-back actions to Intune

## Next implementation steps

1. Create synthetic data files for each data domain.
2. Build a metric calculation layer.
3. Build a local dashboard or HTML report.
4. Add promptbook templates for each scenario.
5. Add a future connector design for Microsoft Graph and Intune reporting APIs.
