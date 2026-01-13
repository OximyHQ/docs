# Windows Technical Setup Reference

> **Purpose**: Technical reference document for creating customer-facing documentation. Contains implementation details, user flows, and troubleshooting context for the Oximy Windows application.

---

## Overview

The Oximy Windows app is a native C# WPF application that runs in the system tray. It captures AI API traffic by running a local mitmproxy instance and routing system traffic through it.

**Key Technologies**:
- C# .NET WPF application
- Bundled Python environment with mitmproxy
- Velopack framework for auto-updates
- Windows Certificate Store integration
- Windows Registry for proxy configuration

---

## Installation

### Download & Install
1. Download the installer from the official distribution channel
2. Run the installer (may require administrator approval)
3. Complete installation wizard
4. Oximy launches automatically and appears in system tray

### First Launch Behavior
- App appears in the Windows system tray (bottom-right, near clock)
- Clicking the tray icon shows the main popup interface
- First-time users see the onboarding flow

### System Requirements
- Windows 10/11
- .NET Runtime (bundled or system)
- Internet connection for initial enrollment
- Administrator privileges may be needed for certificate installation

---

## Setup Flow (User Journey)

The app has a 3-step onboarding flow:

### Step 1: Welcome Onboarding
**View**: `OnboardingView`

Three welcome screens explaining the app:
1. "Welcome to Oximy" - Monitor and analyze your AI API usage
2. "Your Privacy Matters" - All data is stored locally
3. "Ready to Start" - Let's set up Oximy

**Navigation**: Back/Next buttons, page indicators (dots)

### Step 2: Login (if applicable)
**View**: `LoginView`

User connects to their workspace (similar to macOS enrollment).

### Step 3: Permissions Setup
**View**: `PermissionsView`

Two permissions required in sequence:

#### 3a. Security Certificate
**Action**: "Install" button

What happens:
1. Generates RSA 4096-bit CA certificate using .NET cryptography APIs
2. Certificate details:
   - Common Name: "Oximy CA"
   - Organization: "Oximy Inc"
   - Country: "US"
   - Validity: 10 years (3650 days)
3. Creates PEM files in `%USERPROFILE%\.oximy\`
4. Installs to Windows Certificate Store:
   - First attempts: LocalMachine\Root (system-wide, requires elevation)
   - Fallback 1: Uses certutil.exe with elevation prompt
   - Fallback 2: CurrentUser\Root (user-only, no elevation)

**Files Created**:
- `%USERPROFILE%\.oximy\mitmproxy-ca-cert.pem` (certificate only)
- `%USERPROFILE%\.oximy\mitmproxy-ca.pem` (certificate + private key combined)

#### 3b. Network Proxy
**Action**: "Enable" button (only available after certificate is installed)

What happens:
1. Starts mitmproxy process on preferred port 1030 (or finds available port)
2. Configures Windows system proxy via Registry:
   - Sets `HKCU\Software\Microsoft\Windows\CurrentVersion\Internet Settings\ProxyEnable` to 1
   - Sets `ProxyServer` to `127.0.0.1:PORT`
   - Sets `ProxyOverride` to bypass list
3. Notifies Windows of settings change:
   - Calls `InternetSetOption` with INTERNET_OPTION_SETTINGS_CHANGED
   - Broadcasts WM_SETTINGCHANGE message

**Proxy Bypass List**:
```
localhost;127.0.0.1;<local>
```

### Completion
After both permissions are granted, user clicks "Continue" to complete setup.

---

## Main Interface (Post-Setup)

### System Tray
- Oximy icon in Windows system tray
- Left-click shows the main popup
- Right-click shows context menu

### Tray Popup
**View**: `TrayPopup`

Quick access interface showing:
- Current monitoring status
- Today's captured events
- Quick toggle on/off
- Settings link

### Status View
**View**: `StatusView`

Shows detailed status information:
- Proxy status (running/stopped)
- Certificate status (installed/not installed)
- Event count
- Connection status

### Settings Window
**View**: `SettingsWindow` (400x700 separate window)

Sections:
1. **About** - App version info
2. **Updates** - Check for updates button, download progress
3. **Support** - Help Center, Send Feedback links
4. **Certificate** - Status, Generate/Install/Uninstall buttons
5. **Advanced** - Open Logs/Traces folders, Launch at Startup toggle
6. **Legal** - Terms of Service, Privacy Policy links
7. **Account** - Workspace name, Log Out button
8. **Quit** - Quit Oximy button

---

## Configuration & Data Storage

### Directory Structure
```
%USERPROFILE%\.oximy\
├── traces\                     # Captured traffic (JSONL files)
│   └── traces_YYYY-MM-DD.jsonl
├── logs\                       # Application logs
├── cache\                      # Temporary cache
├── mitmproxy-ca-cert.pem      # Certificate only
└── mitmproxy-ca.pem           # Certificate + private key (combined)
```

### Bundled Resources
Located in app installation directory:
```
Resources\
├── python-embed\              # Bundled Python environment
│   ├── python.exe
│   └── Scripts\
│       └── mitmdump.exe
└── oximy-addon\               # Python addon files
    └── addon.py
```

### Settings Storage
Settings keys (stored in app settings):
- `OnboardingComplete` - Boolean
- `WorkspaceName` - String
- `DeviceToken` - String
- `DeviceName` - String

### Network Configuration
- **Default Port**: 1030
- **Port Range**: 930-1130 (preferred ±100)
- **Max Restart Attempts**: 3

### Registry Keys Modified
```
HKCU\Software\Microsoft\Windows\CurrentVersion\Internet Settings\
├── ProxyEnable     (DWORD: 0 or 1)
├── ProxyServer     (String: "127.0.0.1:PORT")
└── ProxyOverride   (String: bypass list)
```

---

## Auto-Updates

### Framework
Uses Velopack for Windows native updates

### Update Behavior
- Checks GitHub Releases API for new versions
- Downloads update packages in background
- Shows progress bar during download
- Applies update on next restart

### Update Settings
- Button: "Check for Updates" in Settings
- Progress indicator during download

---

## Logging & Diagnostics

### Log Location
`%USERPROFILE%\.oximy\logs\`

### Traces Location
`%USERPROFILE%\.oximy\traces\`

### Access from Settings
- "Open Logs Folder" button opens Windows Explorer to logs
- "Open Traces Folder" button opens Windows Explorer to traces

### Support Links
| Resource | Action |
|----------|--------|
| Help Center | Opens https://oximy.com/help |
| Send Feedback | Opens https://github.com/oximy/oximy/issues |

---

## Certificate Management

### Certificate Store Locations

**LocalMachine\Root** (preferred):
- System-wide trust
- Requires administrator privileges
- All users on the machine trust the certificate

**CurrentUser\Root** (fallback):
- User-specific trust
- No elevation required
- Only current user trusts the certificate

### Certificate Operations

**Generate Certificate**:
```csharp
// Uses .NET's RSA.Create(4096) and CertificateRequest
// Creates self-signed X509 certificate with CA extensions
```

**Install Certificate**:
1. Try X509Store LocalMachine\Root
2. If access denied, try certutil.exe with runas
3. Last resort: X509Store CurrentUser\Root

**Uninstall Certificate**:
- Searches both LocalMachine and CurrentUser stores
- Removes all certificates matching "Oximy CA" subject name

---

## Proxy Configuration Details

### Enabling Proxy
```csharp
// Registry modification
key.SetValue("ProxyEnable", 1, RegistryValueKind.DWord);
key.SetValue("ProxyServer", "127.0.0.1:PORT", RegistryValueKind.String);
key.SetValue("ProxyOverride", "localhost;127.0.0.1;<local>", RegistryValueKind.String);

// Notify system
InternetSetOption(INTERNET_OPTION_SETTINGS_CHANGED);
InternetSetOption(INTERNET_OPTION_REFRESH);
SendMessageTimeout(WM_SETTINGCHANGE, "Internet Settings");
```

### Disabling Proxy
```csharp
key.SetValue("ProxyEnable", 0, RegistryValueKind.DWord);
// Same notification calls
```

### Important Notes
- The WinINet notification is **critical** - without it, browsers won't pick up the change
- WM_SETTINGCHANGE broadcast notifies other applications
- App stores original values for potential rollback

---

## Cleanup & Uninstallation

### From Settings
1. **Uninstall Certificate**: Settings > Certificate > "Uninstall Certificate"
2. **Log Out**: Settings > Account > "Log Out"
3. **Quit**: Settings > "Quit Oximy"

### App Termination Behavior
- Proxy is automatically disabled when app quits
- This ensures system isn't left in broken proxy state

### Full Uninstall Steps
1. Quit Oximy from system tray or Settings
2. Use Windows "Add or Remove Programs" to uninstall
3. Delete `%USERPROFILE%\.oximy\` directory (optional, for complete cleanup)
4. Open certmgr.msc, find "Oximy CA" in Trusted Root CAs, delete (if still present)
5. Check Internet Options > Connections > LAN Settings, ensure proxy is disabled

---

## Key URLs & Resources

| Resource | URL |
|----------|-----|
| Sign Up | https://oximy.com/signup |
| Help Center | https://oximy.com/help |
| Terms of Service | https://oximy.com/terms |
| Privacy Policy | https://oximy.com/privacy |
| GitHub | https://github.com/oximy/oximy |
| Feedback | https://github.com/oximy/oximy/issues |
| Support Email | support@oximy.com |
| OISP Bundle | https://oisp.dev/spec/v0.1/oisp-spec-bundle.json |

---

## Technical Implementation Notes

### Certificate Generation Flow
```
1. RSA.Create(4096) - generate key pair
2. CertificateRequest with:
   - BasicConstraints: CA=true, pathLength=0
   - KeyUsage: KeyCertSign | CrlSign
   - SubjectKeyIdentifier
3. CreateSelfSigned with 10-year validity
4. Export to PEM format (Base64 with headers)
5. Save combined key+cert to mitmproxy-ca.pem
```

### Proxy Configuration Flow
```
1. Start mitmproxy process (find available port)
2. Modify Registry HKCU\...\Internet Settings
3. Call InternetSetOption(SETTINGS_CHANGED)
4. Call InternetSetOption(REFRESH)
5. Broadcast WM_SETTINGCHANGE
```

### mitmproxy Process
- Python at `Resources\python-embed\python.exe`
- Mitmdump at `Resources\python-embed\Scripts\mitmdump.exe`
- Addon at `Resources\oximy-addon\addon.py`
- Runs on localhost:PORT
- Captures matching AI traffic to JSONL files

### P/Invoke for System Notification
```csharp
[DllImport("wininet.dll")]
InternetSetOption(IntPtr, int option, IntPtr, int)

[DllImport("user32.dll")]
SendMessageTimeout(HWND_BROADCAST, WM_SETTINGCHANGE, IntPtr, "Internet Settings", ...)
```

---

## Differences from macOS

| Feature | macOS | Windows |
|---------|-------|---------|
| App Type | Menu bar app | System tray app |
| UI Framework | SwiftUI | WPF/XAML |
| Update Framework | Sparkle | Velopack |
| Certificate Store | Keychain | Windows Cert Store |
| Proxy Config | networksetup CLI | Registry + WinINet |
| Settings Storage | UserDefaults | App Settings |
| Startup | LaunchAgent | Registry Run key |

---

## Common Error Scenarios

### Certificate Installation Failures
- "Access denied" - Need elevation, will try certutil
- "Certificate already exists" - Previous installation still present
- "Cannot write to store" - Antivirus may be blocking

### Proxy Enable Failures
- "Cannot access registry" - Permission issue
- "Port already in use" - Another app using port 1030
- "mitmproxy failed to start" - Python environment issue

### Network Issues
- "Browsers not using proxy" - WinINet notification failed
- "SSL errors" - Certificate not trusted by application
- "Connection refused" - mitmproxy process crashed
