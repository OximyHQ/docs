# Setup & Installation Guide

## Technical Documentation Source for Customer Docs

---

## Overview

This guide covers the complete setup process for Oximy Control Plane, including account creation, device enrollment, and workspace configuration.

---

## Part 1: Account Setup

### Creating Your Account

1. Navigate to [app.oximy.com/sign-up](https://app.oximy.com/sign-up)
2. Enter your work email address
3. Create a strong password
4. Verify your email via the confirmation link
5. Complete your profile (name, organization)

**After sign-up:**
- A workspace is automatically created for your organization
- You're redirected to the onboarding flow (`/setup`)
- Your workspace starts in "first-time user" mode

### Signing In

1. Navigate to [app.oximy.com/sign-in](https://app.oximy.com/sign-in)
2. Enter your email and password
3. You'll be redirected to the Home page

---

## Part 2: Device Enrollment

### Overview

Each computer that will be monitored needs the Oximy sensor installed and enrolled with your workspace.

### Method 1: During Onboarding

New users go through a guided onboarding flow:

**Step 1: Welcome**
- Introduction to Oximy
- Trust badges shown: Open Source, Privacy-First, Encrypted
- Auto-advances or click "Get Started"

**Step 2: Platform Selection & Download**
- Select Mac or Windows
- For Mac: Select chip type (Apple Silicon or Intel)
- Download the appropriate installer

**Step 3: Detection**
- Sensor scans for AI tools
- Progress shown: "Detecting tools..."
- Auto-advances when detection complete

**Step 4: Inventory Review**
- See list of detected AI tools
- Review what will be monitored

**Step 5: Complete**
- Confirmation of successful setup
- Redirected to Home page

### Method 2: Adding Devices Later

From anywhere in the dashboard:

1. Click the **"+ Add device"** button in the sidebar
2. Or navigate to Devices page and click **"Add device"**

**Device Enrollment Flow:**

1. **Choose OS** - Select Mac or Windows
2. **Install & Connect:**
   - Download sensor installer
   - Install and open the app
   - Grant required permissions
   - Enter the 6-digit enrollment code

3. **Success** - Name your device and optionally set owner email

### Enrollment Code Details

| Property | Value |
|----------|-------|
| Format | 6 digits (e.g., `158 221`) |
| Display | Shown with space for readability |
| Expiration | 5 minutes from generation |
| Copy | Click copy button to clipboard |
| Retry | Generate new code if expired |

### Download URLs

| Platform | URL |
|----------|-----|
| Mac (Apple Silicon) | `https://oisp.dev/download/mac-arm` |
| Mac (Intel) | `https://oisp.dev/download/mac-intel` |
| Windows | `https://oisp.dev/download/windows` |

---

## Part 3: Sensor Installation

### Mac Installation

**Requirements:**
- macOS 11.0 (Big Sur) or later
- Admin privileges for installation
- Internet connection

**Steps:**

1. Open the downloaded `.dmg` file
2. Drag Oximy to the Applications folder
3. Open Oximy from Applications
4. **Grant permissions when prompted:**
   - **Accessibility**: Required for AI tool detection
   - **Network monitoring**: Required for network traffic analysis
5. Enter your enrollment code when prompted
6. Sensor starts running in the menu bar

**Menu bar icon:**
- Green indicator: Connected and running
- Yellow indicator: Connecting
- Red indicator: Connection issue

### Windows Installation

**Requirements:**
- Windows 10 or later
- Admin privileges for installation
- Internet connection

**Steps:**

1. Run the downloaded `.exe` installer
2. Follow installation wizard
3. Click "Install" when prompted by Windows Security
4. Open Oximy from Start menu
5. Enter your enrollment code when prompted
6. Sensor runs in system tray

**System tray icon:**
- Shows connection status
- Right-click for options

---

## Part 4: Workspace Configuration

### Accessing Settings

1. Click your profile avatar in the sidebar
2. Select "Settings" from the dropdown
3. Or navigate directly to `/settings`

### Available Settings

#### Timezone

Configure the workspace timezone to control how dates and times display throughout the dashboard.

**Options include:**
- **Americas**: ET, CT, MT, PT (New York, Chicago, Denver, LA, etc.)
- **Europe**: GMT, CET (London, Paris, Berlin, etc.)
- **Asia/Pacific**: Singapore, Hong Kong, Tokyo, Sydney, etc.
- **UTC**: For global teams

**How to change:**
1. Open Settings
2. Click the Timezone dropdown
3. Select your preferred timezone
4. Click "Save changes"
5. Success message appears: "Settings saved"

---

## Part 5: Managing Devices

### Viewing Devices

Navigate to the **Devices** page to see all enrolled devices.

**Device information shown:**
- Device name
- Owner (email)
- Operating system (with icon)
- Status (Online/Offline)
- Last seen timestamp
- Sensor version

### Device Table Features

| Feature | Description |
|---------|-------------|
| Sorting | Click column headers to sort |
| Selection | Click row to view device details |
| Keyboard navigation | Use j/k to navigate, Enter to select |
| Status indicator | Green = Online, Gray = Offline |

### Keyboard Shortcuts (Device Page)

| Key | Action |
|-----|--------|
| `j` | Move to next device |
| `k` | Move to previous device |
| `Enter` | Open device details |
| `Esc` | Deselect current device |

### Device Detail Page

Click on any device to see:
- Device information
- Activity history
- Analytics (usage over time)
- Health status

---

## Part 6: Team Member Management

### Inviting Team Members

Team members can be managed through workspace settings (managed via backend/admin).

**Each member can:**
- Access the dashboard
- View all devices and activities
- Enroll new devices
- Configure settings

---

## Part 7: Privacy & Security Setup

### Understanding Coverage Levels

Different tools provide different levels of data:

| Level | What's Captured | When to Use |
|-------|-----------------|-------------|
| Full Trace | Complete request/response | Deep analysis, compliance |
| Features | Key metadata (model, tokens) | Usage analytics |
| Metadata | Basic event info only | Minimal monitoring |
| Chat | Conversation structure | Chat-based tools |

### Open Source Verification

The sensor is fully open-source:
- GitHub: [github.com/oximyhq/sensor](https://github.com/oximyhq/sensor)
- Audit the code before deployment
- Review what data is collected

### Privacy Guarantees

- **No file access**: Sensor cannot read your files
- **No message access**: Sensor cannot read your private messages
- **Local processing**: Data processed on-device before transmission
- **Encrypted transit**: All data encrypted in transit
- **Encrypted storage**: All data encrypted at rest

---

## Setup Checklist

### First-Time Setup

- [ ] Create account at app.oximy.com
- [ ] Complete email verification
- [ ] Go through onboarding flow
- [ ] Download sensor for your OS
- [ ] Install sensor application
- [ ] Grant required permissions
- [ ] Enter enrollment code
- [ ] Verify device appears in dashboard
- [ ] Trigger first AI activity
- [ ] See activity in feed

### Adding Additional Devices

- [ ] Click "Add device" button
- [ ] Select operating system
- [ ] Download installer
- [ ] Install on target device
- [ ] Enter enrollment code within 5 minutes
- [ ] Name the device
- [ ] Verify it appears online

### Workspace Configuration

- [ ] Set timezone
- [ ] Invite team members
- [ ] Configure notification preferences (if available)

---

## Common Setup Scenarios

### Scenario 1: IT Admin Setting Up for Team

1. Create admin account
2. Configure workspace timezone
3. Generate enrollment tokens for each device
4. Distribute installer + codes to team
5. Team members install on their devices
6. Verify all devices enrolled

### Scenario 2: Individual Developer

1. Create account
2. Complete onboarding with your device
3. Start using AI tools normally
4. Review activity in dashboard

### Scenario 3: Remote Team

1. Admin creates workspace
2. Share sign-up link with team
3. Each member installs sensor on their device
4. All devices report to same workspace

---

## Technical Details

### API Endpoints Used During Setup

| Endpoint | Purpose |
|----------|---------|
| `POST /api/v1/devices/enroll` | Create enrollment token |
| `GET /api/v1/devices/enrollment/{token}/status` | Poll enrollment status |
| `GET /api/v1/bootstrap` | Initial data load after login |
| `GET /api/v1/workspace` | Get workspace settings |
| `PATCH /api/v1/workspace` | Update workspace settings |

### Enrollment Flow Sequence

```
1. Dashboard: POST /devices/enroll
   → Returns: { enrollmentToken: "123456", expiresAt: "...", downloadUrl: {...} }

2. User downloads and installs sensor

3. Sensor: POST /devices/register
   → Sends: enrollment token + device info

4. Dashboard polls: GET /devices/enrollment/123456/status
   → Returns: { status: "pending" }

5. Backend processes registration

6. Dashboard polls: GET /devices/enrollment/123456/status
   → Returns: { status: "completed", deviceId: "...", device: {...} }

7. Dashboard shows success state
```
