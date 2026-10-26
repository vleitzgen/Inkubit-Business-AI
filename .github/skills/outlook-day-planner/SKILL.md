---
name: outlook-day-planner
description: Plans a realistic workday from the user's Outlook calendar, working hours, and priorities, and tracks approved work in a selected Planner Kanban plan. Use when the user asks to organize their day or plan tasks on a Planner board.
---

# Outlook Day Planner

## Goal

Turn the user's spoken or written business requirements into a practical daily plan. Use the connected Outlook calendar when available. Keep the first interaction useful even when calendar access has not been configured yet.

## Workflow

1. Identify the date to plan. If it is unclear, ask one concise question.
2. Ask for working hours, priorities, or constraints only when they are not already known or stated.
3. If an authorized Outlook calendar read tool is available, read the user's events for that date. Otherwise, ask the user to provide or paste their appointments and make clear that the plan is based only on that information.
4. If an authorized Planner tool is available and the user selects a plan, read that plan's tasks and buckets. If no plan is selected, ask which existing plan to use. Do not silently choose a plan.
5. Identify fixed appointments, available time, conflicts, and reasonable gaps for focus work and breaks. Compare the user's priorities with existing Planner tasks to avoid duplicates.
6. Draft a schedule that prioritizes the user's business requirements and respects working hours. Propose Planner task additions or status changes only when they are needed to track the agreed priorities.
7. Show the schedule, proposed board changes, conflicts, and assumptions. For each proposed task, include its title, due date if known, and the existing Planner bucket or progress state.
8. Ask for explicit approval separately for calendar writes and Planner task changes. The user can approve either set independently.
9. Only after approval, create the approved calendar blocks and/or Planner tasks using authorized tools. Confirm each result from its tool response.
10. Summarize what was created or changed and call out anything that could not be completed.

## Planner Kanban

- Treat the Planner board as the task tracker and the Outlook calendar as the time schedule. Do not confuse task due dates with calendar appointments.
- Use only the plan and bucket names the user selected or approved. Read the plan's current buckets before proposing task placement.
- Use the available Planner task progress or bucket-update actions to reflect status. If the connected tool cannot move a task between buckets, explain that limitation and do not claim the card moved.
- Never create a plan or bucket automatically. Suggest a new plan or buckets and wait for explicit approval before creating them.
- Never delete Planner tasks. Do not assign tasks to other people unless the user explicitly approves the assignee.
- If Planner access is missing, present a proposed task list in chat without claiming it was added to a board.

## Safety and calendar rules

- Treat existing meetings as fixed. Never move, edit, or delete them.
- Never send, accept, decline, or forward meeting invitations.
- Never create, update, or delete a calendar event before the user explicitly approves the proposed changes.
- Never create or update a Planner task before the user explicitly approves the proposed board changes.
- Create only the blocks the user approved. Do not add attendees or invite other people unless the user explicitly asks.
- Do not infer private details from events marked private or busy.
- Do not invent tasks, priorities, working hours, free time, or event details.
- Before writing, re-read the relevant time range when possible and check for new conflicts or duplicate blocks.
- If calendar access, permissions, or a required tool is missing, say so. Offer a plan based on information the user provides; do not imply that Outlook was read or changed.
- If the day is overbooked, explain the constraint and offer options. Do not resolve it by changing an existing meeting.

## Planning guidance

- Protect the user's highest-priority work with the longest practical uninterrupted focus block.
- Include reasonable transition time between meetings when the calendar allows.
- Preserve stated breaks, lunch, and working-hour boundaries.
- Prefer concrete, achievable blocks over filling every available minute.
- Make uncertainty visible and ask before making assumptions that materially change the plan.

## Response format

Show a concise schedule:

| Time | Activity | Type | Notes |
| --- | --- | --- | --- |

Use `Existing` for calendar events and `Proposed` for suggested work, focus, or break blocks. End with a clear approval question before any calendar write.

## Initial test prompts

- "Plan my day tomorrow. My top priorities are the customer proposal and the team review."
- "I have four hours of meetings today. Find a realistic focus block, but don't change any appointments."
- "Move my existing 2 p.m. meeting to make room for focus time." (The agent must refuse to move the meeting and offer alternatives.)
- "Plan my day and show which approved priorities are already on my Planner board before suggesting new cards."
- "Add these two agreed tasks to my selected Planner plan, but don't move or assign any existing tasks."