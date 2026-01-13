# Technical Documentation for Customer Docs

> **Purpose**: These technical reference documents provide source material for creating customer-facing documentation. They contain implementation details, user flows, UI copy, and troubleshooting context extracted from the actual codebase.

## Documents

### Platform Setup Guides

| Document | Description |
|----------|-------------|
| [macos-setup-reference.md](./macos-setup-reference.md) | macOS app installation, setup flow, configuration, and data locations |
| [windows-setup-reference.md](./windows-setup-reference.md) | Windows app installation, setup flow, configuration, and data locations |

### Cross-Platform References

| Document | Description |
|----------|-------------|
| [features-reference.md](./features-reference.md) | Feature descriptions, settings options, and user-facing capabilities |
| [troubleshooting-reference.md](./troubleshooting-reference.md) | Common issues, diagnostics, and solutions for both platforms |

---

## How to Use These Documents

These documents are intended as **source material** for another agent/person to create final customer documentation. They include:

1. **Technical accuracy** - Details extracted directly from source code
2. **User journeys** - Step-by-step flows as implemented
3. **UI copy** - Actual text shown to users
4. **Error messages** - Real error scenarios and their meanings
5. **Configuration details** - File paths, settings keys, default values
6. **Platform differences** - Where macOS and Windows implementations differ

### Recommended Customer Doc Structure

Based on these technical docs, the final customer documentation should include:

**Quickstart Guides**:
- Getting Started with Oximy (macOS)
- Getting Started with Oximy (Windows)

**Setup & Configuration**:
- Installing and Setting Up Oximy
- Connecting to Your Workspace
- Certificate Installation Guide
- Configuring Proxy Settings

**Using Oximy**:
- Understanding the Dashboard
- Managing Your Data
- Syncing to the Cloud
- Updating Oximy

**Troubleshooting**:
- Common Issues and Solutions
- Certificate Problems
- Proxy Configuration Issues
- Getting Help

---

## Key Information Summary

### What Oximy Does
Oximy captures AI API traffic (OpenAI, Anthropic, etc.) from applications on your device by running a local proxy and storing interaction data for analysis.

### Setup Requirements
1. Enrollment code from Oximy dashboard
2. Certificate installation (requires admin password)
3. Proxy enablement (configures system network settings)

### Data Storage
- All data stored locally in `~/.oximy/` (macOS) or `%USERPROFILE%\.oximy\` (Windows)
- Optional cloud sync when logged into workspace

### Key URLs
| Purpose | URL |
|---------|-----|
| Sign Up | https://app.oximy.com |
| Documentation | https://docs.oximy.com |
| Support | support@oximy.com |

---

## Source Code References

These docs were created by analyzing:

**macOS App** (`/OximyMac/`):
- `Views/EnrollmentView.swift` - Enrollment flow
- `Views/MainView.swift` - Setup and dashboard views
- `Views/Tabs/SettingsTab.swift` - Settings UI
- `Services/CertificateService.swift` - Certificate handling
- `Services/ProxyService.swift` - Proxy configuration
- `App/Constants.swift` - Configuration values

**Windows App** (`/OximyWindows/`):
- `Views/OnboardingView.xaml(.cs)` - Welcome flow
- `Views/PermissionsView.xaml(.cs)` - Permission setup
- `Views/SettingsWindow.xaml` - Settings UI
- `Services/CertificateService.cs` - Certificate handling
- `Services/ProxyService.cs` - Proxy configuration
- `Constants.cs` - Configuration values

---

## Updating These Documents

When the implementation changes, these documents should be updated to reflect:
- New features or settings
- Changed UI flows
- Modified error messages
- Updated file paths or configuration
- New troubleshooting scenarios

Last updated: January 2026
