# macOS Technical Setup Reference

> **Purpose**: Technical reference document for creating customer-facing documentation. Contains implementation details, user flows, and troubleshooting context for the Oximy macOS application.

---

## Overview

The Oximy macOS app is a native Swift/SwiftUI application that runs as a menu bar utility. It captures AI API traffic by running a local mitmproxy instance and routing system traffic through it.

**Key Technologies**:
- Swift/SwiftUI native app
- Bundled Python environment with mitmproxy
- Sparkle framework for auto-updates
- System Keychain integration for CA certificates
- networksetup CLI for proxy configuration

---

## Installation

### Download & Install
1. Download the DMG from the official distribution channel
2. Open the DMG file
3. Drag the Oximy app to the Applications folder
4. Launch Oximy from Applications or Spotlight

### First Launch Behavior
- On first launch, macOS may show a security dialog ("Oximy is from an identified developer")
- The app opens as a menu bar item (top-right of screen)
- Clicking the menu bar icon shows the main interface

### System Requirements
- macOS (version requirements TBD from build configuration)
- Internet connection for initial enrollment
- Administrator privileges for certificate installation

---

## Setup Flow (User Journey)

The app has a 2-step onboarding flow:

### Step 1: Workspace Enrollment
**View**: `EnrollmentView`

1. User sees "Connect Your Workspace" screen
2. User enters 6-digit enrollment code from their Oximy dashboard
3. Code is validated via API (`APIClient.registerDevice`)
4. On success:
   - Device token is stored
   - Workspace name is saved
   - User proceeds to Step 2

**Error Scenarios**:
- Invalid code: "Invalid code. Please check and try again."
- Expired code: "Code expired. Get a new one from your dashboard."
- Already registered: "This device is already registered."
- Connection failed: "Connection failed. Please try again."

**Alternative Path**:
- "Need an account? Sign up free" link opens: `https://app.oximy.com`

### Step 2: Permissions Setup
**View**: `SetupView`

Two permissions required in sequence:

#### 2a. Certificate Installation
**Action**: "Install Certificate" button

What happens:
1. Generates RSA 4096-bit CA certificate using OpenSSL
2. Certificate details:
   - Common Name: "Oximy CA"
   - Organization: "Oximy Inc"
   - Country: "US"
   - Validity: 10 years (3650 days)
3. Creates combined PEM file (key + cert) at `~/.oximy/mitmproxy-ca.pem`
4. Creates cert-only file at `~/.oximy/mitmproxy-ca-cert.pem`
5. Installs to System Keychain via `security add-trusted-cert` command
6. **User will see macOS password prompt** to authorize Keychain modification

**Files Created**:
- `~/.oximy/mitmproxy-ca.pem` (private key + certificate, permissions 0600)
- `~/.oximy/mitmproxy-ca-cert.pem` (certificate only, permissions 0644)
- `~/.oximy/mitmproxy-ca.p12` (PKCS12 format for Keychain)

#### 2b. Proxy Enablement
**Action**: "Enable Proxy" button (only available after certificate is installed)

What happens:
1. Starts mitmproxy process on preferred port 1030 (or finds available port in ±100 range)
2. Configures system proxy via `networksetup`:
   - Sets HTTP proxy to 127.0.0.1:PORT for all network services
   - Sets HTTPS proxy to 127.0.0.1:PORT for all network services
3. Enables proxy for all detected network interfaces (Wi-Fi, Ethernet, etc.)

**Proxy Bypass List** (default):
- localhost
- 127.0.0.1
- *.local
- 169.254/16

### Completion
After both permissions are granted, user clicks "Start Monitoring" to complete setup.

**Alternative**: User can click "Set Up Later" to skip and configure later from Settings.

---

## Main Interface (Post-Setup)

### Menu Bar Icon
- Shows Oximy icon in system menu bar
- Clicking reveals the main window (340x420 pixels)

### Navigation Tabs
Three tabs accessible from bottom navigation:

1. **Home Tab** (`HomeTab`)
   - Shows current monitoring status
   - Displays today's captured events count
   - Quick enable/disable toggle

2. **Settings Tab** (`SettingsTab`)
   - Updates section (check for updates, auto-update toggle)
   - Detection Bundle section (OISP bundle refresh)
   - Certificate section (install/uninstall CA, show in Finder, open Keychain)
   - Account section (workspace info, logout)
   - Local Data section (storage info, sync status, clear data)
   - Advanced section (port info, config directory, reset all settings)

3. **Support Tab** (`SupportTab`)
   - Help documentation links
   - Contact support
   - Feedback options

---

## Configuration & Data Storage

### Directory Structure
```
~/.oximy/
├── traces/                    # Captured traffic (JSONL files)
│   └── traces_YYYY-MM-DD.jsonl
├── logs/                      # Application logs
├── bundle_cache.json          # OISP bundle cache
├── mitmproxy-ca.pem          # Private key + certificate (combined)
├── mitmproxy-ca-cert.pem     # Certificate only
└── mitmproxy-ca.p12          # PKCS12 format
```

### UserDefaults Keys
- `setupComplete` - Boolean, setup wizard completed
- `workspaceName` - String, connected workspace name
- `deviceToken` - String, authentication token
- `autoStartEnabled` - Boolean, launch at login
- `deviceId` - String, unique device identifier
- `workspaceId` - String, workspace identifier

### Network Configuration
- **Default Port**: 1030
- **Port Range**: 930-1130 (preferred ±100)
- **Listen Host**: 127.0.0.1 (localhost only)

---

## Auto-Updates

### Framework
Uses Sparkle 2.x for macOS native updates

### Update Behavior
- Automatic checks every 24 hours (configurable)
- Feed URL: GitHub Releases appcast.xml
- Signatures: EdDSA + Apple code signing
- User can manually check from Settings

### Update Settings
- Toggle: "Automatic Updates" on/off
- Button: "Check for Updates" manual check

---

## Logging & Diagnostics

### Log Location
`~/.oximy/logs/`

### Support Email
Pre-filled email template includes:
- App Version and Build Number
- macOS Version
- Device Model
- Oximy Directory path

### Error Reporting
- Sentry integration for crash reporting
- Anonymous device ID for correlation
- Breadcrumbs for state tracking

---

## Cleanup & Uninstallation

### From Settings
1. **Remove Certificate**: Toggle off in Settings > Certificate section
2. **Clear Local Data**: Settings > Local Data > "Clear Data"
3. **Reset All Settings**: Settings > Advanced > "Reset All Settings"

### Full Uninstall Steps
1. Quit Oximy from menu bar
2. Remove Oximy.app from Applications
3. Delete `~/.oximy/` directory
4. Open Keychain Access, search "Oximy CA", delete certificate
5. Open System Preferences > Network > Advanced > Proxies, disable if still set

### App Termination Behavior
- Proxy is automatically disabled when app quits (sync method `disableProxySync`)
- This ensures system isn't left in broken proxy state

---

## Key URLs & Resources

| Resource | URL |
|----------|-----|
| Sign Up | https://app.oximy.com |
| Documentation | https://docs.oximy.com |
| Terms of Service | https://oximy.com/terms |
| Privacy Policy | https://oximy.com/privacy |
| GitHub | https://github.com/oximyhq/sensor |
| Support Email | support@oximy.com |

---

## Technical Implementation Notes

### Certificate Installation Flow
```
1. OpenSSL generates RSA 4096-bit key + self-signed cert
2. Files saved to ~/.oximy/
3. security add-trusted-cert -d -r trustRoot -k /Library/Keychains/System.keychain
   └─> Prompts for admin password
4. Fallback: tries user keychain if system keychain fails
```

### Proxy Configuration Flow
```
1. networksetup -listallnetworkservices (get all interfaces)
2. For each service (Wi-Fi, Ethernet, etc.):
   - networksetup -setwebproxy SERVICE 127.0.0.1 PORT
   - networksetup -setwebproxystate SERVICE on
   - networksetup -setsecurewebproxy SERVICE 127.0.0.1 PORT
   - networksetup -setsecurewebproxystate SERVICE on
```

### mitmproxy Process
- Bundled Python environment at `Resources/python-embed/`
- Addon at `Resources/oximy-addon/addon.py`
- Runs on localhost:PORT
- Captures matching AI traffic to JSONL files
