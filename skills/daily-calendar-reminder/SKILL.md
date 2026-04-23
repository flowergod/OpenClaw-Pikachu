---
name: daily-calendar-reminder
description: 每日晨间通知技能，自动查询飞书日历今日日程 + 多维表格任务，分类发送提醒。用于定时任务每日触发发送。Use when: 需要每日自动发送晨间通知、定时查询今日任务和日程。
---

# 每日晨间通知 Skill

## 功能
每日早间自动发送综合晨间通知，包含：飞书日历今日日程 + 多维表格任务管理（今日待办、高优先级、进行中项目、过期提醒）。

## 使用场景
- 由 OpenClaw cron 定时触发（每日 00:00 UTC / 08:00 北京时间）
- 触发后自动执行，无需人工干预
- 结果发送到绑定的用户会话

## 配置信息
任务数据存储在飞书多维表格：
- **app_token**: `YmMcb4PUlaTIAmshS6EcFqPenff`
- **table_id**: `tbllwg2t4sEJ64i1`

表格字段：
- 任务名称：任务标题
- 计划日期：任务计划完成日期
- 状态：待规划/待执行/进行中/已完成/暂停
- 优先级：高/中/低
- 分组：日程表/工作/生活/个人/其他
- 项目：所属项目

## 输出格式
严格遵循以下格式输出：

## Daily Morning Notification

1. **Today's calendar events** (飞书日历今日日程)
   - 按照开始时间排序
   - 格式：`- [HH:MM] 日程标题`（如有地点附加在括号内）
   - 如果没有：`_无日程_`

2. **Today's non-calendar todos** (今日计划任务)
   - 从多维表格查询 **计划日期为今天** 且 **状态不是已完成** 的任务
   - 格式：`- 任务名称 (优先级)`
   - 如果没有：`_无_`

3. **Unassigned high-priority tasks** (未安排高优先级任务)
   - 从多维表格查询 **优先级=高** 且 **状态=待执行** 且 **计划日期为空或不是今天** 的任务
   - 如果没有：`_无_`

4. **In-progress project tracking** (进行中项目任务)
   - 从多维表格查询 **状态=进行中** 的任务，按项目分组显示
   - 如果没有：`_无_`

5. **Overdue unfinished tasks reminder** (过期未完成任务提醒)
   - 从多维表格查询 **计划日期 < 今天** 且 **状态 != 已完成** 的任务
   - 如果没有：`_无_`

## 执行步骤

1. **计算今日时间范围**（时区：Asia/Shanghai）
   - 今日开始时间戳：`今天 00:00:00` → 转换为毫秒时间戳
   - 今日结束时间戳：`今天 23:59:59` → 转换为毫秒时间戳
   - 日历查询开始：`YYYY-MM-DDT00:00:00+08:00`（ISO 8601）
   - 日历查询结束：`YYYY-MM-DDT23:59:59+08:00`（ISO 8601）

2. **查询飞书日历今日日程**
   ```json
   {
     "name": "feishu_calendar_event",
     "parameters": {
       "action": "instance_view",
       "start_time": "<今日开始时间 ISO 格式>",
       "end_time": "<今日结束时间 ISO 格式>"
     }
   }
   ```

3. **分类查询多维表格任务**
   使用 `feishu_bitable_app_table_record` 工具，`app_token: "YmMcb4PUlaTIAmshS6EcFqPenff"`, `table_id: "tbllwg2t4sEJ64i1"`

   - **Today's non-calendar todos**: 筛选条件：
     - `计划日期` = `["Today"]`
     - `状态` 不包含 `已完成`

   - **Unassigned high-priority tasks**: 筛选条件：
     - `优先级` = `["高"]`
     - `状态` = `["待执行"]`
     - (`计划日期` isEmpty OR `计划日期` is not Today)

   - **In-progress project tracking**: 筛选条件：
     - `状态` = `["进行中"]`

   - **Overdue unfinished tasks**: 筛选条件：
     - `计划日期` < Today
     - `状态` != `已完成`

   每个分类单独查询，按照排序规则排序（计划日期升序）。

4. **整理并发送**
   - 按照上面指定的 `## Daily Morning Notification` 格式输出
   - 使用 message 工具发送到绑定的用户会话

## 参数
本技能不需要额外参数，触发时自动计算当前日期。
