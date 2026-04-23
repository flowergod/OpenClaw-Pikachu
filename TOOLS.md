# TOOLS.md - Local Notes

Skills define _how_ tools work. This file is for _your_ specifics — the stuff that's unique to your setup.

## What Goes Here

Things like:

- Camera names and locations
- SSH hosts and aliases
- Preferred voices for TTS
- Speaker/room names
- Device nicknames
- Anything environment-specific

## Feishu Configuration

- **Daily task Bitable:**
  - app_token: `YmMcb4PUlaTIAmshS6EcFqPenff`
  - table_id: `tbllwg2t4sEJ64i1`
  - url: https://my.feishu.cn/base/YmMcb4PUlaTIAmshS6EcFqPenff?table=tbllwg2t4sEJ64i1&view=vewBOvxLXA

## Cron Jobs

- **Daily morning notification:** ID `76d2eb5a-6c3d-4b2e-8479-59e30d0c8323`
  - Schedule: `0 0 * * *` (UTC) → 08:00 Beijing time
  - Event: `发送每日晨间通知`
  - Skill: `daily-calendar-reminder`

## Custom Skills

- **daily-calendar-reminder:** Location `/app/skills/daily-calendar-reminder/`
  - Function: Daily morning notification with 5 sections:
    1. Today's calendar events
    2. Today's non-calendar todos
    3. Unassigned high-priority tasks
    4. In-progress project tracking
    5. Overdue unfinished tasks reminder
