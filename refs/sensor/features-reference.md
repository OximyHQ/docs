# Features & Configuration Reference

> **Purpose**: Technical reference for creating customer-facing documentation. Contains feature descriptions, configuration options, and user-facing capabilities for both platforms.

---

## Core Features Overview

Oximy captures and analyzes AI API traffic from applications on your device. Key capabilities:

| Feature | Description |
|---------|-------------|
| AI Traffic Capture | Intercepts API calls to AI providers (OpenAI, Anthropic, etc.) |
| Local Storage | All captured data stored locally on device |
| Cloud Sync | Optional sync to Oximy dashboard (when logged in) |
| Auto-Updates | Automatic app updates (platform-native) |
| OISP Detection | Uses open standard bundle for AI service detection |

---

## Monitoring Feature

### Status States

| State | Icon | Description |
|-------|------|-------------|
| Monitoring Active | Green shield checkmark | Proxy running, traffic being captured |
| Monitoring Paused | Orange shield | Certificate installed but proxy stopped |
| Setup Required | Gray slashed shield | Certificate not installed |

### Controls

**Start Monitoring**:
- Starts mitmproxy process
- Enables system proxy
- Begins capturing traffic

**Stop Monitoring**:
- Disables system proxy
- Stops mitmproxy process
- Existing data preserved

### Information Displayed

**Home Screen** (both platforms):
- Current monitoring status (Active/Paused/Setup Required)
- Port number (when active)
- Device name
- Organization name (if connected)
- Events pending sync count
- Last sync time

---

## Workspace Connection

### Enrollment Flow

1. User gets 6-digit code from Oximy web dashboard
2. Enter code in app
3. App registers device with API
4. Device receives:
   - Device token (for authentication)
   - Workspace name
   - Device ID

### Enrollment Codes

- Format: 6 numeric digits
- Expiration: Time-limited (dashboard generates fresh codes)
- Single-use: Each code can only be used once

### Error Messages

| Error | Meaning |
|-------|---------|
| "Invalid code" | Code doesn't match any active enrollment |
| "Code expired" | Code has passed its expiration time |
| "Device already registered" | This device already enrolled |
| "Connection failed" | Network or API issue |

---

## Certificate Management

### What the Certificate Does

The Oximy CA certificate enables HTTPS traffic inspection:
- Acts as a trusted Certificate Authority on the device
- Allows mitmproxy to decrypt/re-encrypt HTTPS traffic
- Required for capturing AI API traffic (all modern APIs use HTTPS)

### Certificate Details

| Property | Value |
|----------|-------|
| Common Name | Oximy CA |
| Organization | Oximy Inc |
| Country | US |
| Key Size | RSA 4096-bit |
| Validity | 10 years |
| Hash Algorithm | SHA-256 |

### User Actions

**Install Certificate**:
- Generates new CA if not exists
- Installs to system trust store
- Prompts for admin/password authorization

**Uninstall Certificate**:
- Removes from trust store
- Certificate files remain on disk
- Proxy must be disabled first

**Reinstall Certificate**:
- Toggle off then on in Settings
- Regenerates completely new certificate
- Useful if certificate becomes corrupted

---

## Proxy Configuration

### How It Works

1. App starts mitmproxy on localhost
2. System proxy configured to route through mitmproxy
3. All HTTP/HTTPS traffic passes through proxy
4. Proxy inspects traffic, captures AI API calls
5. Non-AI traffic passes through unmodified

### Port Configuration

| Setting | Value |
|---------|-------|
| Preferred Port | 1030 |
| Search Range | ±100 (930-1130) |
| Listen Address | 127.0.0.1 (localhost only) |

Port selection:
- Tries port 1030 first
- If unavailable, searches nearby ports
- Final port shown in app Settings/Status

### Bypass List

Traffic to these destinations bypasses the proxy:

| macOS | Windows |
|-------|---------|
| localhost | localhost |
| 127.0.0.1 | 127.0.0.1 |
| *.local | `<local>` |
| 169.254/16 | - |

---

## Detection Bundle (OISP)

### What It Is

OISP (Open Intelligence & Services Protocol) is an open standard that defines:
- Known AI service endpoints (URLs)
- Request/response parsing rules
- Traffic classification patterns

### Supported AI Providers

The bundle includes detection for 60+ AI providers including:
- OpenAI (API and ChatGPT web)
- Anthropic (API and Claude web)
- Google AI / Gemini
- Perplexity
- Azure OpenAI
- AWS Bedrock
- And many more

### Bundle Updates

- Auto-refreshes every 30 minutes when proxy is running
- Manual refresh: Settings > Detection Bundle > "Refresh Now"
- Bundle cached locally for offline operation

---

## Data Storage

### Trace Files

**Format**: JSONL (JSON Lines - one JSON object per line)

**Naming**: `traces_YYYY-MM-DD.jsonl` (daily files)

**Location**:
- macOS: `~/.oximy/traces/`
- Windows: `%USERPROFILE%\.oximy\traces\`

**Content** (per event):
- Event ID (UUIDv7)
- Timestamp
- Source (process attribution)
- Trace level (full_trace, identifiable, metadata_only)
- Timing information
- Client process info
- Interaction details:
  - Messages
  - Model
  - Temperature
  - Token usage
  - Finish reason
  - Response content

### Storage Management

**View in Settings**:
- Number of trace files
- Total storage size
- Pending events count

**Actions**:
- "Open Folder" - Opens traces directory in file browser
- "Clear Data" - Deletes all local trace files

**Clear Data Confirmation**:
- Shows count of pending events and files
- Warns this cannot be undone
- Synced events not affected

---

## Sync Feature

### When Sync Occurs

- Automatic periodic sync when logged in
- Manual "Sync Now" trigger (macOS)
- Events uploaded to Oximy cloud dashboard

### Sync Requirements

- User must be logged in to workspace
- Internet connection required
- Valid device token

### Sync Status Indicators

- Events Pending: Count of events awaiting sync
- Last Sync: Time of most recent successful sync

---

## Auto-Updates

### macOS (Sparkle)

| Setting | Description |
|---------|-------------|
| Check for Updates | Manual check button |
| Automatic Updates | Toggle on/off |
| Check Interval | Every 24 hours (when enabled) |

Update process:
1. App checks for new version
2. Downloads update in background
3. Prompts user to install
4. Restarts with new version

### Windows (Velopack)

| Setting | Description |
|---------|-------------|
| Check for Updates | Manual check button |
| Progress Bar | Shows download progress |

Update process:
1. User clicks "Check for Updates"
2. Downloads from GitHub Releases
3. Applies update on next restart

---

## Settings Reference

### macOS Settings Tab

| Section | Options |
|---------|---------|
| **Updates** | Check for Updates, Auto-update toggle, Version display |
| **Detection Bundle** | OISP Bundle refresh, Last update time |
| **Certificate** | Status, Install/Uninstall toggle, Show in Finder, Open Keychain |
| **Account** | Workspace name, Login status, Log Out |
| **Local Data** | Storage info, Pending events, Open Folder, Clear Data, Sync Now |
| **Advanced** | Port number, Config directory, Reset All Settings |

### Windows Settings Window

| Section | Options |
|---------|---------|
| **About** | App icon, Version info |
| **Updates** | Check for Updates, Download progress |
| **Support** | Help Center, Send Feedback |
| **Certificate** | Status, Generate/Install/Uninstall |
| **Advanced** | Open Logs Folder, Open Traces Folder, Launch at Startup |
| **Legal** | Terms of Service, Privacy Policy |
| **Account** | Workspace name, Log Out |
| **Quit** | Quit Oximy button |

---

## Privacy & Data Handling

### What Oximy Captures

- AI API request/response content
- Model parameters (temperature, max_tokens, etc.)
- Token usage statistics
- Originating application (process attribution)

### What Oximy Does NOT Capture

- Non-AI traffic (passes through unmodified)
- Traffic to bypassed hosts
- Raw credentials (API keys are typically in headers, which are captured but should be treated as sensitive)

### Data Location

All data stored locally in `~/.oximy/` (macOS) or `%USERPROFILE%\.oximy\` (Windows):
- User controls their data
- Can delete anytime via Settings
- Only synced to cloud if logged in

---

## Support Features

### macOS Support Tab

- Help documentation link
- Contact support (pre-filled email)
- Feedback options

### Windows Support Section

- Help Center link (opens browser)
- Send Feedback link (opens GitHub issues)

### Support Email Template (macOS)

Pre-filled with:
- Subject: "Oximy Mac Support Request"
- App Version & Build Number
- macOS Version
- Device Model
- Oximy Directory path

---

## Account Management

### Login (Enrollment)

- Enter 6-digit code from dashboard
- Validates with API
- Stores device token and workspace info

### Logout

- Clears device token
- Clears workspace info
- Does NOT delete local data
- Does NOT uninstall certificate

### Reset All Settings

Available in Advanced settings:
- Clears all app preferences
- Returns to initial setup state
- Does NOT delete trace files
- Does NOT uninstall certificate

---

## Keyboard Shortcuts & Quick Actions

### macOS

| Action | Method |
|--------|--------|
| Open Oximy | Click menu bar icon |
| Switch tabs | Click tab bar buttons |
| Quit | Click menu bar icon > Quit, or Settings > Reset |

### Windows

| Action | Method |
|--------|--------|
| Open Oximy | Click system tray icon |
| Open Settings | Click Settings in popup |
| Quit | Right-click tray > Quit, or Settings > Quit |

---

## Technical Specifications

### Network Usage

| Purpose | Destination |
|---------|-------------|
| API Enrollment | api.oximy.com |
| Event Sync | api.oximy.com |
| OISP Bundle | oisp.dev |
| Updates (macOS) | GitHub Releases |
| Updates (Windows) | GitHub Releases API |

### File System Usage

| Directory | Purpose |
|-----------|---------|
| ~/.oximy/ | Main configuration directory |
| ~/.oximy/traces/ | Captured event JSONL files |
| ~/.oximy/logs/ | Application log files |
| ~/.oximy/bundle_cache.json | OISP bundle cache |
| ~/.oximy/mitmproxy-ca.pem | CA certificate + key |
| ~/.oximy/mitmproxy-ca-cert.pem | CA certificate only |

### Process Architecture

```
Oximy App (Swift/C#)
    ├── mitmproxy process (Python)
    │   └── oximy-addon (Python)
    │       └── Writes to traces/*.jsonl
    └── System proxy configuration
```

---

## Version Information

### How to Find Version

**macOS**:
- Settings > Updates section shows version
- Or: About menu > Version

**Windows**:
- Settings window header shows version
- About section shows version

### Version Format

`MAJOR.MINOR.PATCH` (e.g., 1.0.0)

Current version defined in:
- macOS: Bundle version in Info.plist
- Windows: Constants.cs (`Version = "1.0.0"`)
