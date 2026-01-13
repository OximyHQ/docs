# Documentation Source Files

These documents are **technical source material** for creating customer-facing product documentation for Oximy Control Plane.

## Purpose

These files capture the complete implementation details, features, and functionality of the Oximy dashboard. They are meant to be consumed by another agent or documentation writer who will:

1. Compile this information with live dashboard screenshots
2. Create polished customer-facing documentation
3. Follow the standard docs cycle: Quickstarts → Setup → Troubleshooting

## Documents

| File | Purpose | Customer Doc Use |
|------|---------|------------------|
| [01-product-overview.md](./01-product-overview.md) | Core concepts, terminology, data flow | Introduction, "What is Oximy?" |
| [02-quickstart.md](./02-quickstart.md) | 5-minute getting started guide | Quickstart tutorial |
| [03-setup-installation.md](./03-setup-installation.md) | Complete setup process, all options | Full setup guide, admin guides |
| [04-features-reference.md](./04-features-reference.md) | All features with details | Feature docs, how-to guides |
| [05-troubleshooting.md](./05-troubleshooting.md) | Common issues and solutions | Troubleshooting section, FAQ |

## How to Use These Documents

### For Customer Documentation Writer

1. **Start with Product Overview** - Understand the core concepts and terminology
2. **Use Quickstart as template** - This is the critical first-time user experience
3. **Reference Setup guide** - For comprehensive installation documentation
4. **Pull from Features Reference** - For individual feature how-tos
5. **Include Troubleshooting** - Critical for user self-service

### Document Structure Suggestion for Final Docs

```
Customer Docs/
├── Getting Started/
│   ├── What is Oximy?          ← from 01-product-overview.md
│   ├── Quickstart              ← from 02-quickstart.md
│   └── Core Concepts           ← from 01-product-overview.md
│
├── Setup/
│   ├── Creating an Account     ← from 03-setup-installation.md
│   ├── Installing the Sensor   ← from 03-setup-installation.md
│   ├── Device Enrollment       ← from 03-setup-installation.md
│   └── Workspace Configuration ← from 03-setup-installation.md
│
├── Features/
│   ├── Activity Feed           ← from 04-features-reference.md
│   ├── Inventory               ← from 04-features-reference.md
│   ├── Device Management       ← from 04-features-reference.md
│   ├── Ask (Analytics)         ← from 04-features-reference.md
│   ├── Command Palette         ← from 04-features-reference.md
│   └── Keyboard Shortcuts      ← from 04-features-reference.md
│
└── Support/
    ├── Troubleshooting         ← from 05-troubleshooting.md
    └── FAQ                     ← extracted from all docs
```

## Key Information

### Product Name
**Oximy Control Plane**

### Tagline
"See Every AI Tool Your Team Uses"

### Core Value Props
- Discover AI tools being used
- Monitor usage in real-time
- Track who, what, when, where
- Understand data exposure

### Target User
Enterprise IT and Security teams

### Trust Messaging
- Open Source sensor
- Privacy-First design
- Encrypted data

### Key URLs
- Dashboard: `app.oximy.com`
- GitHub: `github.com/oximyhq/sensor`

## Screenshots Needed

For final docs, capture screenshots of:

- [ ] Home page with activity feed
- [ ] Home page empty state (scanning)
- [ ] Inventory page with tools
- [ ] Device table
- [ ] Add device modal (each step)
- [ ] Command palette
- [ ] Ask page (empty and with cards)
- [ ] Settings page
- [ ] First detection celebration
- [ ] Activity drawers (trace, feature, metadata, chat)

## Notes

- All keyboard shortcuts assume Mac (Cmd key). Document Ctrl alternatives for Windows.
- Download URLs (`oisp.dev`) are for sensor installers.
- Enrollment codes are 6-digit, expire in 5 minutes.
- Coverage levels vary by tool - not configurable by user.
