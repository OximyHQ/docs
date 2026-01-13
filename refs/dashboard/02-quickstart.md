# Quickstart Guide - Get Your First AI Detection

## Technical Documentation Source for Customer Docs

---

## Goal

Get from zero to seeing your first AI tool activity in under 5 minutes.

---

## Prerequisites

- A Mac (Intel or Apple Silicon) or Windows computer
- Admin access to install software
- An Oximy account (sign up at oximy.com)

---

## Step 1: Sign Up and Access the Dashboard

1. Go to [oximy.com](https://oximy.com) and sign up for an account
2. Verify your email address
3. You'll be redirected to the onboarding flow at `/setup`

**What happens:**
- Account created via Clerk authentication
- New workspace automatically provisioned
- Onboarding wizard starts

---

## Step 2: Download the Sensor

During onboarding (or anytime via the "Add Device" button):

### For Mac:

1. Select "Mac" as your platform
2. Choose your chip type:
   - **Apple Silicon** (M1, M2, M3, M4) - most Macs from 2020+
   - **Intel** - older Macs before 2020

   *Not sure? Go to  → About This Mac*

3. Download URLs:
   - Apple Silicon: `https://oisp.dev/download/mac-arm`
   - Intel: `https://oisp.dev/download/mac-intel`

### For Windows:

1. Select "Windows" as your platform
2. Download URL: `https://oisp.dev/download/windows`

**Trust Note:**
> The sensor is fully open-source. Audit the code on GitHub before installing: https://github.com/oximyhq/sensor

---

## Step 3: Install and Grant Permissions

### Mac Installation:

1. Open the downloaded `.dmg` file
2. Drag the Oximy app to your Applications folder
3. Open the app from Applications
4. **Grant Required Permissions:**
   - Accessibility permission (required for detection)
   - Network monitoring permission
5. App will prompt you to enter your enrollment code

### Windows Installation:

1. Run the downloaded `.exe` installer
2. Follow the installation wizard
3. Open Oximy from the Start menu
4. App will prompt you to enter your enrollment code

---

## Step 4: Enter Enrollment Code

When adding a device, you'll receive a 6-digit enrollment code:

1. The dashboard generates a unique code (e.g., `158 221`)
2. Enter this code in the sensor app
3. Code expires after 10 minutes - if it expires, generate a new one

**How enrollment works:**
- Dashboard creates token via `POST /api/v1/devices/enroll`
- Sensor sends token to backend to register
- Dashboard polls `GET /api/v1/devices/enrollment/{token}/status` every 5 seconds
- When status is "completed", device appears in dashboard

---

## Step 5: Trigger Your First Detection

Once the sensor is running:

1. Open any AI tool:
   - **ChatGPT** (web or app)
   - **Claude** (web or app)
   - **GitHub Copilot**
   - **Cursor**
   - Any other supported AI tool

2. Make a request (ask a question, generate code, etc.)

3. **See it in the dashboard!**
   - Activity appears in the real-time feed on the Home page
   - "Signal Acquired!" celebration modal appears on first detection
   - Tool is added to your Inventory
   - Discovery is logged in the Journal sidebar

---

## What You Should See

### Home Page Activity Feed:

```
[Tool Icon] ChatGPT
2 seconds ago · John's MacBook Pro · john@company.com
```

### First Detection Celebration:

A modal appears with:
- "Signal Acquired!" headline
- "Your first AI activity has been detected" message
- Confetti animation

### Discovery Journal:

- "New Tool: ChatGPT" discovery card
- "New Device: John's MacBook Pro" discovery card
- "New Person: john@company.com" discovery card

---

## Quick Verification Checklist

| Check | Expected Result |
|-------|-----------------|
| Device appears in Devices page | Shows online status, OS, sensor version |
| Activity appears in Home feed | Shows tool name, device, user, timestamp |
| Tool appears in Inventory | Grouped by category (Apps/Websites/APIs) |
| Discovery cards appear | In Journal sidebar on Home page |

---

## Troubleshooting Quick Fixes

### Enrollment code expired
- Click "Generate new token" to get a fresh 6-digit code
- Enter it within 10 minutes

### Sensor not connecting
- Check your internet connection
- Make sure firewall isn't blocking Oximy
- Verify the enrollment code matches exactly

### No activity detected
- Make sure the sensor app is running (check menu bar/system tray)
- Make sure AI tool is actually making network requests
- Wait a few seconds - there's minimal latency in detection

### Device shows "Offline"
- Sensor app may have been quit
- Computer may be asleep
- Re-open the Oximy sensor app

---

## Next Steps

After your first detection:

1. **Enroll more devices** - Add team members' computers
2. **Explore the Inventory** - See all detected AI tools
3. **Use Ask** - Query your data with natural language
4. **Configure Settings** - Set workspace timezone

---

## Time Estimates for Customer Docs

| Step | Typical Time |
|------|--------------|
| Sign up & login | 1-2 minutes |
| Download sensor | 30 seconds |
| Install sensor | 1-2 minutes |
| Enter code & connect | 30 seconds |
| First detection | Instant (once AI tool used) |
| **Total** | **~5 minutes** |

---

## Key URLs

| Resource | URL |
|----------|-----|
| Dashboard | `https://app.oximy.com` |
| Sign up | `https://app.oximy.com/sign-up` |
| Mac ARM download | `https://oisp.dev/download/mac-arm` |
| Mac Intel download | `https://oisp.dev/download/mac-intel` |
| Windows download | `https://oisp.dev/download/windows` |
| GitHub (open source) | `https://github.com/oximyhq/sensor` |
