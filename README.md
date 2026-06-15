[README.md](https://github.com/user-attachments/files/28938810/README.md)
# ConnectorPulse — Enterprise Admin Health Monitor

> An AI-powered admin console tool that diagnoses connector failures and surfaces self-serve resolution steps — reducing mean time to resolution from hours to minutes.

![Status](https://img.shields.io/badge/status-active-00e5a0?style=flat-square) ![Built with Claude](https://img.shields.io/badge/AI-Claude%20Sonnet-blueviolet?style=flat-square) ![License](https://img.shields.io/badge/license-MIT-gray?style=flat-square)

---

## The Problem

Enterprise SaaS admins managing 20–100+ data connectors face a consistent operational gap: **when a connector fails, diagnosing root cause requires expert knowledge most admins don't have**, resulting in:

- High-volume support tickets for issues that have known, self-serve fixes
- 2–4 hour MTTR (mean time to resolution) for common failures like OAuth expiry or rate limiting
- Productivity loss for end users blocked from searching company knowledge

This mirrors a real pattern I observed while interning at **Splunk PMO**, where cross-functional teams spent disproportionate time diagnosing infrastructure issues that had repeatable, documentable solutions.

---

## The Solution

ConnectorPulse is an AI-powered admin health dashboard that:

1. **Visualizes deployment health** across all connectors in real-time
2. **Accepts raw error logs or system alerts** from any connector
3. **Uses Claude AI to diagnose root cause** in plain English (no log-reading expertise required)
4. **Returns prioritized, self-serve resolution steps** with estimated resolution time
5. **Surfaces diagnostic commands** admins can run to verify the fix

The goal: reduce support ticket volume by enabling admins to resolve common failures themselves in under 10 minutes.

---

## Product Design

### User Personas

| Persona | Role | Pain Point |
|---|---|---|
| **IT Admin (Primary)** | Manages connectors and deployment health | Spends 30–60 min per incident diagnosing errors they don't fully understand |
| **Workspace Admin (Secondary)** | Oversees user access and data permissions | Reacts to user complaints about broken search with no visibility into cause |

### Core User Flow

```
Admin notices connector health drop
        ↓
Opens Admin Console Health Dashboard
        ↓
Selects affected connector or pastes error log
        ↓
AI diagnoses root cause + severity
        ↓
Admin follows self-serve resolution steps
        ↓
Confirms resolution / tickets only if escalation needed
```

### Key Design Decisions

**Why AI-powered diagnosis vs. static runbooks?**
Static runbooks require admins to correctly identify the error type first — which is the hard part. LLMs can accept raw, unformatted log output and extract structured insight, removing that prerequisite.

**Why include MTTR estimates?**
Self-serve resolution only succeeds if admins trust it will be faster than opening a ticket. Showing "5–10 min self-serve vs. 2–4 hr ticket" creates the right incentive to try the AI-recommended path first.

**Why pre-loaded scenarios?**
The four most common enterprise connector failure types (auth expiry, sync stalling, permission mismatch, webhook timeout) cover ~80% of real-world incidents. Pre-loading them lowers the barrier to entry and makes the tool useful immediately.

---

## Features

- **Live health score** — single-number deployment health (0–100) across all connectors
- **Connector status sidebar** — per-connector uptime percentage with visual status indicators
- **AI Issue Analyzer** — paste any error log, get structured diagnosis
- **4 pre-loaded failure scenarios** — auth failure, sync stall, permission error, webhook timeout
- **Self-serve resolution steps** — numbered, actionable, admin-executable
- **Diagnostic commands** — CLI/API verification steps post-fix
- **MTTR comparison** — self-serve vs. support ticket time estimates

---

## Technical Architecture

```
Frontend (HTML/CSS/JS)
    ↓ user pastes error log
Prompt Engineering Layer
    ↓ structured prompt with log context
Anthropic Claude API (claude-sonnet-4-6)
    ↓ JSON response: severity, root cause, steps, MTTR
UI Rendering
    ↓ severity badge, action list, code blocks
Admin takes action
```

**Stack:** Vanilla HTML/CSS/JS · Anthropic Claude API · No backend required

---

## Scenarios Covered

| Scenario | Example Trigger | Typical Root Cause |
|---|---|---|
| **Auth Failure** | 401 Unauthorized on sync | OAuth token expired (90-day rotation not configured) |
| **Sync Stalled** | Indexing lag > 6 hours | API rate limit hit; batch size too aggressive |
| **Permission Error** | ACL mismatch post-migration | Service account scope revoked during domain migration |
| **Webhook Timeout** | 504 on event delivery | Firewall rule blocking inbound webhook IP range |

---

## PM Reflection

This project explored a specific product question:

> *Can AI meaningfully reduce the expertise gap for enterprise admins managing complex connector ecosystems?*

The answer is yes — but only if the output is structured and immediately actionable. Early iterations that returned paragraph-form answers saw lower completion rates than the numbered step format. This aligns with established UX patterns for task-completion interfaces: **users in a high-stress diagnostic context need decision reduction, not information density.**

The MTTR comparison was added after thinking through the behavioral incentive: admins will only self-serve if they believe it's faster. Surfacing that comparison directly in the resolution view is the nudge that drives behavior change.

---

## Roadmap

- [ ] Connector-specific resolution playbooks per connector type (Salesforce vs. Zendesk vs. ServiceNow)
- [ ] Historical incident log — track recurring failures by connector to surface systemic issues
- [ ] Ticket deflection analytics — measure how often self-serve resolution succeeds vs. escalates
- [ ] Slack/Teams alert integration — push AI diagnosis to admin channels on health drop
- [ ] Proactive health scoring — predict connector failures before they impact users

---

*Built by [Anne Ononose](https://www.linkedin.com/in/anne-ononose) · Computer Information Systems, University of Houston*
