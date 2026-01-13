# Oximy Control Plane - Product Overview

## Technical Documentation Source for Customer Docs

---

## What is Oximy Control Plane?

Oximy Control Plane is an enterprise AI tool usage and security monitoring dashboard. It provides IT and security teams with complete visibility into how AI tools are being used across their organization.

### Core Purpose

- **Discover** which AI tools (ChatGPT, Claude, Copilot, Cursor, etc.) are being used
- **Monitor** usage patterns in real-time
- **Track** who is using which tools, when, and on which devices
- **Understand** data exposure levels for compliance and security

### Target Users

- Enterprise IT administrators
- Security teams
- Compliance officers
- IT managers overseeing AI adoption

---

## Core Concepts

### 1. Devices

A **Device** is any computer (Mac, Windows, or Linux) that has the Oximy sensor installed. Once enrolled, devices send activity data to the Control Plane.

**Key attributes:**
- Device name (e.g., "John's MacBook Pro")
- Owner/assigned user
- Operating system
- Online/Offline status
- Last seen timestamp
- Sensor version

### 2. Tools

A **Tool** represents any AI application or service being monitored. Tools are automatically detected when users interact with them.

**Tool categories:**
- **Apps**: Desktop applications (e.g., Cursor, GitHub Copilot)
- **Websites**: Browser-based AI services (e.g., ChatGPT web, Claude.ai)
- **Direct APIs**: Direct API integrations

**Tool examples:**
- ChatGPT (OpenAI)
- Claude (Anthropic)
- GitHub Copilot
- Cursor
- Perplexity
- Gemini
- And 50+ other AI tools

### 3. Activities

An **Activity** is a single AI tool usage event. Each time a user interacts with an AI tool, an activity is recorded.

**Activity data includes:**
- Timestamp
- User who performed the action
- Tool that was used
- Device the action was performed on
- Coverage level (how much data was captured)

### 4. Coverage Levels

Oximy captures different levels of data depending on the tool and configuration:

| Coverage Level | What's Captured | Use Case |
|---------------|-----------------|----------|
| **Full Trace** | Complete request/response data | Deep analysis, compliance auditing |
| **Features** | Extracted key information (model used, token counts, etc.) | Usage analytics without full content |
| **Metadata** | Basic event info only (tool, time, user) | Minimal footprint monitoring |
| **Chat** | Conversation-style interactions | Chat-based AI tool monitoring |

### 5. Discoveries

A **Discovery** is a milestone event when something is detected for the first time:

- **New Tool**: First time an AI tool is used in the organization
- **New Device**: First time a device sends data
- **New Person**: First time a team member uses an AI tool

Discoveries help track the expansion of AI adoption over time.

### 6. Workspace

A **Workspace** represents your organization's Oximy instance. All devices, users, tools, and activities belong to a workspace.

**Workspace settings:**
- Timezone configuration
- Team member management
- Global preferences

---

## Key Features Summary

| Feature | Description |
|---------|-------------|
| Real-time Activity Feed | Live view of AI tool usage across the organization |
| Tool Inventory | Categorized list of all detected AI tools |
| Device Management | Enroll, monitor, and manage devices |
| Discovery Journal | Track first-time detections |
| Ask (Natural Language Analytics) | Query your data using natural language |
| Keyboard Shortcuts | Power user navigation |
| Command Palette | Quick search and navigation (Cmd+K) |
| Dark/Light Mode | Theme support |

---

## Data Flow Overview

```
[User's Device]
    ↓ (Oximy Sensor)
[Activity Event Generated]
    ↓
[Oximy Backend API]
    ↓
[Control Plane Dashboard]
    ↓
[IT/Security Team Views Data]
```

---

## Authentication

Users authenticate to the Control Plane using:
- Email + password
- SSO (if configured)

Authentication is handled by Clerk, an enterprise-grade auth provider.

---

## API Integration

The Control Plane communicates with the Oximy backend API for:
- Fetching device/tool/activity data
- Enrolling new devices
- Updating workspace settings
- Submitting feedback/requests

All API requests are authenticated using JWT tokens.

---

## Browser Support

The dashboard is optimized for modern browsers:
- Chrome (recommended)
- Firefox
- Safari
- Edge

---

## Notes for Customer Docs

**Quickstart angle**: Focus on getting the first device enrolled and seeing the first activity
**Setup angle**: Step-by-step device enrollment, workspace configuration
**Troubleshooting angle**: Common issues with device enrollment, connectivity, missing data
