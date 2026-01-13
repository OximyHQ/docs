# Features Reference Guide

## Technical Documentation Source for Customer Docs

---

## Dashboard Navigation

### Main Navigation (Sidebar)

The sidebar provides access to all main features:

| Icon | Page | Description |
|------|------|-------------|
| Home | `/home` | Real-time activity feed and discovery journal |
| Inventory | `/inventory` | All detected AI tools by category |
| Devices | `/devices` | Manage enrolled devices |
| Ask | `/ask` | Natural language analytics |
| Settings | `/settings` | Workspace configuration |

### Sidebar Actions

- **Search button**: Opens command palette (Cmd+K)
- **Add Device**: Primary CTA to enroll new devices
- **Help & Support**: Quick access to support resources
- **Profile dropdown**: Theme toggle, settings, sign out

### Sidebar Behavior

- Collapsible via button or Cmd+[
- Shows icons only when collapsed
- Remembers collapsed state

---

## Home Page

### Activity Feed (Left Column)

The activity feed shows real-time AI tool usage as a narrative timeline.

**Activity Card Information:**
- Tool name and icon
- Timestamp (relative: "2 seconds ago", "5 minutes ago")
- Device name
- User email/name
- Coverage indicator (what data was captured)

**Activity Interaction:**
- Click any activity to open its detail drawer
- Appropriate drawer opens based on coverage level:
  - **Chat Drawer**: For chat interactions
  - **Trace Drawer**: For full request/response data
  - **Feature Drawer**: For extracted features
  - **Metadata Drawer**: For basic metadata only

**Empty State:**
- Time-aware messaging:
  - Morning: "Good Morning. Listening..."
  - Afternoon: "Scanning for Signals..."
  - Evening: "Evening Watch Active"
  - Night: "Night Watch Active"
- Scanning animation displayed
- Prompt to open any AI tool

### Journal Sidebar (Right Column)

The journal sidebar shows discoveries and landscape overview.

**Sections:**

1. **Recent Discoveries**
   - New tools detected
   - New devices connected
   - New people using AI
   - Milestones reached

2. **Tool Landscape**
   - Most used AI tools
   - Activity count per tool
   - Quick access to tool details

3. **Device Landscape**
   - Devices in workspace
   - Online/offline status
   - Quick access to device details

4. **Workspace Stats**
   - Total activities
   - Active devices
   - Detected tools
   - Team members

### First Detection Celebration

When the first AI activity is detected, a celebration modal appears:
- "Signal Acquired!" headline
- Confetti animation
- One-time display (marks as seen via API)

---

## Inventory Page

### Purpose

View and manage all AI tools detected across your organization.

### Layout

**Header Section:**
- Page title with tool count
- Search bar (filters by name, domain, provider)
- "Missing an app?" button (request new tool support)

**Tool Categories:**

| Category | Description | Examples |
|----------|-------------|----------|
| **Apps** | Desktop applications | Cursor, GitHub Copilot |
| **Websites** | Browser-based AI tools | ChatGPT, Claude.ai |
| **Direct APIs** | Direct API integrations | OpenAI API calls |

### Tool Card Information

Each tool card shows:
- Tool icon
- Tool name
- Provider
- Activity count
- Activity sparkline (7-day trend)
- Coverage level badge
- Last seen timestamp

### Search Functionality

- Real-time filtering as you type
- Searches tool name, domain, and provider
- Shows "No Tools Found" with option to request

### Missing App Request

Click "Missing an app?" to request support for:
- New AI tools not currently detected
- Full trace access for existing tools

---

## Devices Page

### Purpose

View and manage all enrolled devices.

### Device Table

| Column | Description |
|--------|-------------|
| Device name | User-assigned name |
| Owner | Email of device owner |
| OS | Operating system icon (Mac/Windows/Linux) |
| Status | Online (green) / Offline (gray) |
| Last seen | Relative timestamp |
| Version | Sensor version number |

### Table Features

- Click row to view device details
- Sortable columns
- Keyboard navigation (j/k)

### Empty State

When no devices enrolled:
- Scanning animation
- "No Devices Connected" message
- Mac and Windows install buttons

### Adding Devices

Via "Add device" button:
1. Select OS (Mac/Windows)
2. Download installer
3. Install and grant permissions
4. Enter 6-digit enrollment code
5. Name the device

---

## Ask Page

### Purpose

Query your AI usage data using natural language.

### How It Works

Type questions like:
- "Who uses ChatGPT the most?"
- "What new tools were discovered this week?"
- "Show me activity by tool"
- "What percentage of activity has full trace?"

### Input Features

- Text input with dynamic placeholder
- Suggested questions based on your data
- Enter to submit
- Loading state during processing

### Response Cards

Questions return interactive visualization cards:

| Card Type | Description | Example Question |
|-----------|-------------|------------------|
| **List Card** | Ranked list with counts | "Who uses ChatGPT most?" |
| **Stat Card** | Single metric display | "How many tools detected?" |
| **Bar Chart** | Horizontal bar visualization | "Activity by tool" |
| **Donut Card** | Percentage visualization | "Coverage breakdown" |
| **New Tools Card** | Recently discovered tools | "New tools this week?" |
| **Text Card** | Simple text response | Fallback for complex queries |

### Card Features

- Timestamp of when asked
- Dismiss button (X)
- Action buttons to drill down
- Persistent storage (cards saved locally)

### Layout

- Empty state: Centered input with suggestions
- With cards: Masonry grid layout (responsive columns)
- Input stays at bottom when cards present

---

## Command Palette

### Purpose

Quick search and navigation across the entire dashboard.

### Opening

- Press `Cmd+K` (Mac) or `Ctrl+K` (Windows)
- Press `/` from anywhere
- Click search icon in sidebar

### Default View (No Query)

Shows three sections:
1. **Recent**: Recent prompts/activities
2. **Actions**: Add device, toggle theme, view coverage gaps
3. **Go to**: Quick navigation to pages

### Search Mode

When typing, searches across:
- Activities (by prompt content, tool name)
- Tools (by name, domain)
- Devices (by name, owner email)

### Result Categories

| Category | Shows |
|----------|-------|
| Prompts | Matching activities with prompt previews |
| Tools | Matching tools with activity counts |
| Devices | Matching devices with status |

### Keyboard Navigation

| Key | Action |
|-----|--------|
| `↑` `↓` | Navigate results |
| `Enter` | Select current item |
| `Esc` | Close palette |

---

## Keyboard Shortcuts

### Global Navigation

| Shortcut | Action |
|----------|--------|
| `g h` | Go to Home |
| `g i` | Go to Inventory |
| `g d` | Go to Devices |
| `g a` | Go to Ask |
| `g p` | Go to Profile |
| `g ?` | Go to Help |

### Quick Actions

| Shortcut | Action |
|----------|--------|
| `Cmd+K` | Open command palette |
| `/` | Open command palette |
| `n` | Add new device |
| `?` | Show keyboard shortcuts |
| `Esc` | Close modal/drawer/clear selection |

### Interface Controls

| Shortcut | Action |
|----------|--------|
| `Cmd+[` | Toggle sidebar |
| `t` | Toggle light/dark theme |

### List Navigation (Tables/Lists)

| Shortcut | Action |
|----------|--------|
| `j` | Move down |
| `k` | Move up |
| `↓` | Move down |
| `↑` | Move up |
| `Enter` | Select/open item |
| `o` | Open item |
| `g g` | Go to first item |
| `Shift+G` | Go to last item |

### Viewing Shortcuts

Press `?` anywhere to view all available shortcuts in a modal.

---

## Settings Page

### Workspace Settings

**Timezone Configuration**
- Select from 40+ global timezones
- Affects all date/time displays in dashboard
- Changes apply workspace-wide

**Available Timezones:**
- Americas: ET, CT, MT, PT (various cities)
- Europe: GMT, CET (London, Paris, Berlin, etc.)
- Asia/Pacific: Singapore, Hong Kong, Tokyo, Sydney, etc.
- UTC

**Saving Changes:**
- Click "Save changes" button
- Success message: "Settings saved"
- Cancel button to revert unsaved changes

---

## Detail Drawers

### Chat Drawer

For chat-type interactions:
- Conversation thread view
- User messages vs AI responses
- Model information
- Timestamps

### Trace Drawer

For full trace activities:
- Request headers
- Request body (payload)
- Response headers
- Response body
- Timing information

### Feature Drawer

For feature extraction activities:
- Extracted model info
- Token counts (if available)
- Key metadata
- Tool-specific features

### Metadata Drawer

For metadata-only activities:
- Basic event info
- Tool name
- Timestamp
- Device info

### Common Drawer Features

All drawers include:
- Close button (X)
- Escape key to close
- Activity header with tool/device info
- Copy functionality for data

---

## Theme Support

### Light Mode

- Light backgrounds
- Dark text
- Optimized for daytime use

### Dark Mode

- Dark backgrounds
- Light text
- Optimized for low-light environments
- Reduces eye strain

### Toggle Methods

1. Press `t` key
2. Click theme toggle in profile dropdown
3. Command palette: "Switch to dark/light mode"

### Persistence

Theme preference is saved and persists across sessions.

---

## Help & Support

### Accessing Help

- Click "Help & Support" in sidebar
- Press `g ?` keyboard shortcut

### Available Options

- Documentation links
- Report a bug
- Request a feature
- Contact support

### Feedback Submission

Users can submit:
- Bug reports
- Feature requests
- App/tool support requests
- Trace access requests

---

## Real-Time Features

### Live Activity Feed

- Activities appear as they're detected
- No manual refresh needed
- "now" indicator for very recent activities
- Smooth animations for new items

### Device Status

- Real-time online/offline status
- Last seen updates automatically

### Discovery Journal

- New discoveries appear immediately
- Milestone celebrations shown in real-time

---

## Empty States

### Smart Empty States

Each page has contextual empty states:

| Page | Empty State Message | Action |
|------|---------------------|--------|
| Home | "Good Morning. Listening..." (time-aware) | Open any AI tool |
| Inventory | "Awaiting First Detection" | Sensor will detect tools |
| Devices | "No Devices Connected" | Install sensor |
| Ask | Centered input with suggestions | Ask a question |

### Scanning Animation

Empty states show a visual scanning animation to indicate the system is actively monitoring.

---

## API Response Times

The dashboard is optimized for fast response times:
- Initial page load: Bootstrap data fetched
- Activity feed: Cursor-paginated for efficiency
- Search: Client-side filtering for instant results
- Ask: API-powered with local fallback

---

## Browser Compatibility

| Browser | Support Level |
|---------|---------------|
| Chrome | Full (recommended) |
| Firefox | Full |
| Safari | Full |
| Edge | Full |

### Recommended

- Latest browser versions
- JavaScript enabled
- Cookies enabled (for auth)
