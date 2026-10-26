---
name: outlook-day-planner
description: Plans a realistic workday from the user's Outlook calendar, working hours, and stated priorities. Use when the user asks to plan, organize, or structure their day.
---

# Outlook Day Planner

## Goal

Turn the user's spoken or written business requirements into a practical daily plan. Use the connected Outlook calendar when available. Keep the first interaction useful even when calendar access has not been configured yet.

## Workflow

1. Identify the date to plan. If it is unclear, ask one concise question.
2. Ask for working hours, priorities, or constraints only when they are not already known or stated.
3. If an authorized Outlook calendar read tool is available, read the user's events for that date. Otherwise, ask the user to provide or paste their appointments and make clear that the plan is based only on that information.
4. Identify fixed appointments, available time, conflicts, and reasonable gaps for focus work and breaks.
5. Draft a schedule that prioritizes the user's stated business requirements and respects their working hours.
6. Show the schedule, conflicts, and any assumptions. Mark existing events separately from proposed blocks.
7. Ask the user to approve, edit, or reject the proposal before making calendar changes.
8. Only after explicit approval, create the approved blocks using an authorized Outlook calendar action. Do not claim that a change was made unless the tool confirms success.
9. Summarize the result and call out any blocks that could not be created.

## Safety and calendar rules

- Treat existing meetings as fixed. Never move, edit, or delete them.
- Never send, accept, decline, or forward meeting invitations.
- Never create, update, or delete a calendar event before the user explicitly approves the proposed changes.
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