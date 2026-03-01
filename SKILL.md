---
name: Chrono Ward
category: planning
purpose: Planning and scheduling automation for projects with task decomposition, deadline tracking, and automated reminders
tags: [planning, scheduling, automation, tasks, deadlines, project-management]
version: 1.0.0
requires:
  - openclaw >= 0.8.0
  - cron CLI (optional for scheduled tasks)
---

# Chrono Ward - Planning & Scheduling Automation

Chrono Ward is OpenClaw's planning subagent for decomposing complex tasks, tracking deadlines, setting up scheduled reminders, and managing project timelines across multiple workstreams.

## Quick Reference

| Command | Description |
|---------|-------------|
| `/plan <goal>` | Decompose a goal into actionable tasks |
| `/schedule <task> <date>` | Set deadline for a task |
| `/timeline <project>` | Show project timeline and milestones |
| `/remind <message> <interval>` | Create recurring reminder |
| `/block <time> <activity>` | Reserve time block |
| `/standup <project>` | Daily standup report |
| `/retrospective <project>` | Generate retrospective summary |

## Core Commands

### /plan — Task Decomposition

Decompose complex goals into actionable sub-tasks with dependencies.

```
/plan "Implement user authentication system"
```

**Output Example:**
```
├── Research OAuth providers
│   ├── Compare Auth0 vs Supabase vs Firebase
│   └── Document pros/cons for each
├── Design authentication flow
│   ├── Create login/register wireframes
│   └── Define session management strategy
├── Implement backend auth
│   ├── Set up JWT handling
│   ├── Create password reset flow
│   └── Add rate limiting
├── Implement frontend auth
│   ├── Build login/register forms
│   ├── Add social login buttons
│   └── Create protected route wrapper
└── Testing
    ├── Write unit tests for auth utils
    └── E2E login flow tests

Estimated total: 3-5 days
```

### /schedule — Deadline Management

Assign deadlines to tasks and integrate with calendar.

```
/schedule "Implement backend auth" "2026-03-15"
/schedule "Complete code review" "friday 5pm"
/schedule "Deploy to staging" "next monday"
```

**Supported Date Formats:**
- `YYYY-MM-DD` — Exact date
- `friday 5pm`, `monday 9am` — Relative weekdays
- `next monday`, `tomorrow 3pm` — Relative dates
- `in 3 days`, `in 2 weeks` — Relative intervals

### /timeline — Project Visualization

Display Gantt-style timeline for project milestones.

```
/timeline flickclaw-saas
/timeline rpgclaw --days 30
/timeline "feature: user-dashboard" --verbose
```

**Output Format:**
```
RPGCLAW Timeline (Mar 1 - Mar 31)
================================================
Mar 01 ████████░░░░░░░░░░░░░░░░░░░ 25%  Backend API
Mar 05 ┌──────────────────────────────────────┐
Mar 10 │         Database Schema             │
Mar 15 └──────────────────────────────────────┘
                    ↓
Mar 18 ┌──────────────────────────────────────┐
Mar 25 │       Authentication Flow            │
Mar 30 └──────────────────────────────────────┘

Milestones:
  ✓ Schema Review (Mar 05)
  → Auth Implementation (Mar 25) - ON TRACK
  ⚠ Frontend Integration (Mar 28) - AT RISK
  ○ Deploy to Prod (Mar 31)
```

### /remind — Automated Reminders

Set up recurring reminders for task follow-ups.

```
/remind "Review PRs" "every weekday 10am"
/remind "Check staging health" "every 4 hours"
/remind "Team standup" "mon-fri 09:00"
/remind "Update changelog" "every friday 4pm"
```

**Interval Syntax:**
- `every <number> hours/minutes/days`
- `every weekday <time>`
- `mon-fri <time>`, `weekends <time>`
- `cron "0 9 * * 1-5"` — Standard cron format

### /block — Time Protection

Reserve focused time blocks for deep work.

```
/block "2h" "Fix authentication bug"
/block "friday 2pm-4pm" "Code review session"
/block "90m" "Write documentation" --interruptions false
```

### /standup — Daily Standups

Generate or log daily standup entries.

```
/standup flickclaw
/standup rpgclaw --yesterday --today --blockers
```

**Output:**
```
📋 Daily Standup - FlickClaw (Mar 01, 2026)
==========================================
✅ Yesterday:
   - Fixed payment webhook timeout
   - Reviewed PR #142 (user-dashboard)
   - Updated API documentation

🎯 Today:
   - Implement subscription renewal flow
   - Write unit tests for payment service
   - Pair with OPS on deployment

🚧 Blockers:
   - Waiting on AWS credentials for staging
   - Need design mockups for pricing page
```

### /retrospective — Project Analysis

Generate retrospective summaries with velocity metrics.

```
/retrospective flickclaw --sprint 12
/retrospective rpgclaw --range "last month"
```

**Output:**
```
📊 Sprint 12 Retrospective - FlickClaw
======================================
Velocity: 34 story points (+12% vs avg)

✅ What Went Well:
   - Authentication feature completed ahead of schedule
   - Code review turnaround improved to <4 hours
   - Test coverage increased to 78%

⚠ What Could Improve:
   - Underestimated complexity of payment integration
   - Too many context switches between features

🔧 Action Items:
   - Create payment integration guide
   - Schedule design sync for Q2 roadmap
   - Add integration tests for webhook handlers
```

## Use Cases

### Use Case 1: Feature Planning Sprint

**Scenario:** Starting a new feature development cycle for FlickClaw.

```
/plan "Implement subscription management dashboard"
/schedule "Create subscription data models" "tomorrow"
/schedule "Build billing UI components" "in 3 days"
/schedule "Integrate Stripe webhooks" "in 5 days"
/schedule "QA and deploy" "in 7 days"
/block "90m" "Focus: data modeling" --interruptions false
/block "90m" "Focus: UI implementation" --interruptions false
/remind "Sprint sync" "every weekday 2pm"
```

### Use Case 2: Bug Triage Session

**Scenario:** Managing multiple bug reports with varying priorities.

```
/plan "Fix login timeout issue"
/priority --high "Login timeout affects 15% of users"
/schedule "Root cause analysis" "today 4pm"
/schedule "Implement fix" "tomorrow"
/schedule "Deploy hotfix" "tomorrow evening"
/block "2h" "Debug and fix login issue"
```

### Use Case 3: Release Coordination

**Scenario:** Coordinating a production release across teams.

```
/plan "Release v2.5.0"
/timeline "Release v2.5.0" --days 7
/schedule "Code freeze" "wednesday noon"
/schedule "QA sign-off" "thursday 5pm"
/schedule "Production deploy" "friday 10am"
/remind "Release checklist" "thursday 9am"
/standup rpgclaw --focus "release prep"
```

### Use Case 4: Onboarding New Developer

**Scenario:** Setting up a new team member with tasks and schedule.

```
/plan "Onboard @newdev to FlickClaw"
/schedule "Environment setup" "day 1"
/schedule "Codebase walkthrough" "day 1 afternoon"
/schedule "First task: fix typo in docs" "day 2"
/schedule "Second task: add unit test" "day 3"
/block "3h" "Onboarding session"
/remind "Check-in with new dev" "daily 3pm"
```

### Use Case 5: Technical Debt Management

**Scenario:** Tracking and scheduling technical debt items.

```
/plan "Address technical debt - Q1"
/schedule "Upgrade Node.js version" "week 8"
/schedule "Migrate from REST to GraphQL" "week 10"
/schedule "Add integration tests for payments" "week 9"
/schedule "Refactor auth middleware" "week 11"
/priority --medium "Technical debt items"
/timeline "Technical Debt Q1"
```

## Configuration

### Global Settings

Create `~/.openclaw/chronoward.json` for default behaviors:

```json
{
  "default_estimate": "2h",
  "reminder_channels": ["slack", "terminal"],
  "timezone": "Europe/Madrid",
  "default_calendar": "google",
  "working_hours": {
    "start": "09:00",
    "end": "18:00"
  },
  "weekend_protection": true,
  "default_reminder_lead": "15m"
}
```

### Project-Specific Config

Add `.chronoward.json` in project root:

```json
{
  "project": "flickclaw-saas",
  "milestones": [
    {"name": "Alpha", "date": "2026-04-01"},
    {"name": "Beta", "date": "2026-06-01"},
    {"name": "Launch", "date": "2026-09-01"}
  ],
  "sprint_length": "2 weeks",
  "default_assignee": "team"
}
```

## Integrations

### Calendar Sync

Connect to Google Calendar for bidirectional sync:

```
/connect calendar google --auth
```

### Slack Notifications

```
/connect slack --workspace "flickclaw-team"
/remind "Deploy to production" "friday 2pm" --slack "#ops"
```

### GitHub Integration

Automatically link tasks to issues:

```
/connect github --repo "smouj/flickclaw-saas"
/schedule "Fix issue #142" "tomorrow" --link-issue
```

## Troubleshooting

### Issue: Tasks Not Appearing in Timeline

**Cause:** Missing date assignments on tasks.

**Solution:**
```bash
# Check task dates
/timeline myproject --verbose

# Add dates to undated tasks
/schedule "My task" "2026-03-10"
```

### Issue: Reminders Not Firing

**Cause:** Cron daemon not running or permission issues.

**Solution:**
```bash
# Verify cron is active
crontab -l

# Check Chrono Ward reminder service
/openclaw status

# Restart reminder service
/openclaw service restart chronoward
```

### Issue: Calendar Sync Failures

**Cause:** OAuth token expired or invalid credentials.

**Solution:**
```bash
# Re-authenticate calendar
/connect calendar google --reauth

# Check connection status
/openclaw integrations status
```

### Issue: Time Blocks Being Overwritten

**Cause:** Conflicting block requests.

**Solution:**
```bash
# List current blocks
/block --list

# Remove conflicting block
/block cancel "2h Fix login"

# Use unique names for blocks
/block "2h" "Fix login bug - high priority"
```

### Issue: /plan Generates Too Many Tasks

**Cause:** Goal is too broad or ambiguous.

**Solution:**
```bash
# Break down into smaller goals
/plan "Implement login form UI"
/plan "Implement login API endpoint"
/plan "Add session handling"

# Or limit decomposition depth
/plan "Implement auth" --depth 2
```

## Advanced Patterns

### Chained Dependencies

```
/plan "Full auth implementation" --chain
# Creates: research → design → backend → frontend → test → deploy
```

### Estimation with Confidence

```
/schedule "Complex feature" "2026-03-15" --estimate "3-5 days" --confidence "70%"
```

### Recurring Task Templates

```
/template "bug-fix"
  /priority --high
  /estimate "2h"
  /block "1h"
  /schedule "today"

/use "bug-fix" "Fix memory leak"
```

### Workload Balancing

```
/workload --project flickclaw --view week
/workload --balance --max-daily 6h
```

## Keyboard Shortcuts

| Shortcut | Action |
|----------|--------|
| `Ctrl+p` | Quick plan |
| `Ctrl+d` | Set deadline |
| `Ctrl+r` | Add reminder |
| `Ctrl+b` | Time block |
| `Ctrl+s` | Show standup |
| `Ctrl+t` | Show timeline |

## Exit Codes

| Code | Meaning |
|------|---------|
| 0 | Success |
| 1 | Invalid command syntax |
| 2 | Task not found |
| 3 | Scheduling conflict |
| 4 | Integration error |
| 5 | Permission denied |

## Examples Gallery

### Example 1: Complete Sprint Setup

```bash
# Morning: Plan the sprint
/plan "Sprint 13 - FlickClaw Payments"
/schedule "Payment models" "day 1"
/schedule "Stripe integration" "day 2-3"
/schedule "Webhook handlers" "day 3-4"
/schedule "Frontend payment UI" "day 4-5"
/schedule "Testing" "day 5"

/block "2h" "Deep work: models" --daily
/block "2h" "Deep work: integration" --daily

# Set reminders
/remind "Sprint standup" "mon-fri 9am"
/remind "Check progress" "wed 2pm"

/# View timeline
/timeline flickclaw --sprint 13
```

### Example 2: Emergency Hotfix

```bash
# Quick decomposition
/plan "Hotfix: Database connection leak"

# High priority
/priority --critical "Affects production"

# Immediate schedule
/schedule "Identify leak source" "today 2pm"
/schedule "Implement fix" "today 4pm"
/schedule "Deploy" "today 5pm"

/# Block focused time
/block "90m" "Debug connection leak"

/# Alert team
/remind "Hotfix in progress" "now" --slack "#incidents"
```

### Example 3: Weekly Review

```bash
# Generate reports
/retrospective flickclaw --sprint 12

# Check upcoming
/timeline flickclaw --days 7

# View workload
/workload --view week

# Archive completed
/archive --project flickclaw --last-week
```

---

**Remember:** Chrono Ward works best when tasks are small, deadlines are realistic, and time blocks are protected. Use `/help <command>` for detailed syntax on any specific command.
```