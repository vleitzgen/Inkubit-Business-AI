# Inkubit Business AI

Workshop-ready agent skills for Microsoft Copilot Studio.

## Outlook Day Planner

The first skill turns spoken or written business priorities into a proposed workday plan based on the user's Outlook calendar. It treats existing meetings as fixed and requires explicit approval before creating calendar blocks.

Skill file: [`.github/skills/outlook-day-planner/SKILL.md`](.github/skills/outlook-day-planner/SKILL.md)

## Add the skill to Copilot Studio

1. Create or open an agent powered by the **GitHub Copilot harness**.
2. Open **Build** and choose **Skills** > **Add skill** > **Upload a skill**.
3. Upload `.github/skills/outlook-day-planner/SKILL.md`.
4. Open **Preview** and test with a prompt such as: “Plan my day tomorrow. My top priorities are the customer proposal and the team review.”

The Markdown file provides the agent's workflow and behavior. It does not create the Copilot Studio agent, configure connectors, grant permissions, or publish the agent.

## Outlook connection

To plan from live calendar data, configure an authorized Outlook calendar read action for the agent. To create approved focus blocks, also configure an Outlook calendar create action. Test both actions in the target environment and verify the signed-in identity and permissions.

Without calendar access, the skill must ask the user to provide their appointments and clearly label the result as a plan based only on that information. Calendar writes must remain approval-gated.

## Licensing and harness

The **GitHub Copilot harness in Copilot Studio** is an authoring and orchestration framework; it is not the GitHub Copilot service. Microsoft documents Microsoft 365 Copilot licensing as a way to access Copilot Studio authoring for this harness. Agent use can consume Copilot Credits, depending on the configuration. Confirm the tenant's current licensing, billing, and publishing permissions before the workshop.

Other Copilot Studio harnesses have different licensing and publishing options. A Copilot Studio trial can be used to build and test in the test chat, but does not allow publishing.

## Suggested workshop flow

1. Collect a participant's business requirements in natural language.
2. Upload this skill to the agent.
3. Preview the proposed day plan using sample or authorized calendar data.
4. Review conflicts and assumptions, then approve or edit the proposal.
5. Demonstrate calendar writes only if the Outlook actions, permissions, and approval flow are configured in advance.

## Voice-first agent generation from VS Code

This workflow turns spoken business requirements into a reviewable skill file in this repository. It uses **Windows Voice Typing** to dictate into VS Code; it does not require or imply an always-on wake-word listener. The speaker reviews the transcript before submitting it to Copilot.

### Prerequisites

- VS Code with GitHub Copilot Chat installed and signed in.
- This repository cloned and opened as the VS Code workspace.
- A working microphone and Windows Voice Typing. In the Copilot Chat input, press **Win+H** to dictate; if voice typing is unavailable, type or paste the same prompt.
- Copilot Studio access if you want to continue from the generated file to an agent preview.

### Workshop workflow

1. Open this repository in VS Code and open Copilot Chat with **Ctrl+Alt+I**.
2. Select **Agent** mode and confirm the repository is the active workspace.
3. Focus the chat input and press **Win+H**. Speak the requirements as business language: the user's goal, inputs, expected result, constraints, and what the agent must never do.
4. Review and correct the transcript. Then submit it as the prompt below, replacing the bracketed section with the dictated requirements.
5. Let Copilot inspect the existing skill and README, then ask it to create or update only the relevant `SKILL.md`. The skill must have YAML front matter with `name` and `description`, plus clear workflow steps, safety rules, response format, and test prompts.
6. Review the proposed diff. Confirm that the skill uses only tools that will actually be configured, asks before external writes, and states its fallback when a tool is unavailable.
7. Ask Copilot to validate the Markdown and check the test prompts against the requirements. Fix any mismatch before accepting the changes.
8. In Copilot Studio, create or open an agent using the **GitHub Copilot harness**, then upload the generated skill under **Build** > **Skills** > **Add skill** > **Upload a skill**.
9. Test the agent in **Preview**. Configure and authorize Outlook calendar actions separately before demonstrating live calendar reads or writes.
10. Commit and push the reviewed Markdown changes only after the workshop owner approves them.

### Voice prompt to dictate

```text
Create or update an uploadable Copilot Studio skill in this repository from the business requirements below.

Business requirements:
[Speak the goal, who will use the agent, what information it can use, the desired outcome, constraints, and actions it must not take.]

First inspect the existing repository files and preserve unrelated content. Write the skill as Markdown with YAML front matter containing a lowercase hyphenated name and a concise activation description. Include the agent's goal, an explicit step-by-step workflow, required tools and permissions, safety rules for external actions, fallback behavior when a tool is unavailable, response format, and at least three test prompts including one safety edge case.

Do not create or modify backend services, connect external accounts, access live customer data, or execute actions in external systems. Only create or update the relevant Markdown file. Show me the diff and validation results, and wait for my approval before committing or pushing.
```

### Voice and licensing notes

- **Win+H is dictation**, not ambient listening: it captures speech while the user invokes voice typing in the focused text field. Always review the transcript before sending it.
- VS Code voice-chat or wake-word availability can vary by product, extension, account, and tenant. The workshop does not depend on those features; use Windows Voice Typing or keyboard input as the fallback.
- Creating a skill file in VS Code and testing it with GitHub Copilot are separate from creating or publishing a Copilot Studio agent. Copilot Studio access, the selected harness, tenant billing, and publishing permissions must be checked in advance.
- A skill file does not configure Outlook access. Live calendar behavior requires separately configured and authorized Outlook actions.

## Microsoft documentation

- [Add and manage an uploaded skill](https://learn.microsoft.com/microsoft-copilot-studio/agents-experience/skills-add-existing)
- [Harnesses in Copilot Studio](https://learn.microsoft.com/microsoft-copilot-studio/harnesses-overview)
- [Copilot Studio licensing](https://learn.microsoft.com/microsoft-copilot-studio/billing-licensing)
- [Set up GitHub Copilot in VS Code](https://code.visualstudio.com/docs/copilot/setup)
- [Getting started with GitHub Copilot Chat in VS Code](https://code.visualstudio.com/docs/copilot/getting-started-chat)