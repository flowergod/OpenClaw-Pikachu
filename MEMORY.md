# MEMORY.md - Long-Term Memory

This is your curated long-term memory. Only load this in main sessions.

## Significant Events

- **2026-04-23**: Created `daily-calendar-reminder` custom skill for daily morning notification
- **2026-04-23**: Configured cron job to send daily notification at 8:00 Beijing time
- **2026-04-23**: Integrated with Feishu Bitable for task data: `app_token=YmMcb4PUlaTIAmshS6EcFqPenff`, `table_id=tbllwg2t4sEJ64i1`
- **2026-04-23**: Reorganized workspace structure to `~/.openclaw/workspace/`
- **2026-04-23**: Marked 9 expired tasks as completed in Bitable

## Key Configuration

- **Daily notification cron ID: `76d2eb5a-6c3d-4b2e-8479-59e30d0c8323`
- **Custom skill location**: `/app/skills/daily-calendar-reminder/
- **Feishu Bitable URL**: https://my.feishu.cn/base/YmMcb4PUlaTIAmshS6EcFqPenff?table=tbllwg2t4sEJ64i1&view=vewBOvxLXA

## Lessons Learned

- OpenClaw skills are automatically loaded by description matching when system events trigger
- Feishu Bitable date fields require millisecond timestamps, not ISO strings
- Cron uses UTC time, Beijing time = UTC +8
