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

The Markdown file defines the skill's workflow and behavior. By itself it does not create an agent or configure connectors. With Copilot Studio MCP configured in VS Code, GitHub Copilot can drive the agent lifecycle using your signed-in identity and the operations your account is allowed to use.

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
- Copilot Studio MCP access if you want GitHub Copilot to create, test, and deploy the agent from VS Code. See [Copilot Studio MCP in VS Code](#copilot-studio-mcp-in-vs-code).
- A development environment and the required Copilot Studio authoring, testing, and publishing permissions.

### One flow: voice -> GitHub -> Copilot Studio

This is one guided Copilot Chat session, with pauses for review and approval at consequential steps. GitHub stores the reviewed skill as the source of truth; Copilot Studio MCP carries the same work into the selected Studio environment.

1. Complete the one-time MCP setup below, sign in, and select a development environment.
2. Open this repository in VS Code. Open Copilot Chat with **Ctrl+Alt+I**, select **Agent** mode, and confirm this repository is the active workspace.
3. Focus the chat input, press **Win+H**, and speak the business requirements. Review and correct the transcript before sending it with the single-flow prompt below.
4. Copilot reads the repository, creates or updates the relevant `SKILL.md`, checks its YAML front matter and Markdown, and validates the test prompts. It shows the diff and pauses for approval.
5. After approval, Copilot commits and pushes the reviewed change to the current GitHub branch. It verifies the commit and a clean, synchronized worktree before continuing.
6. In the same chat, Copilot uses Copilot Studio MCP to verify the signed-in identity and target environment, discover current authoring and deployment operations, and create or update the Studio agent as a draft from the reviewed skill.
7. Copilot runs the available tests and shows the exact agent, environment, connected tools, channels, audience, and test results. It pauses again for explicit release approval.
8. Only after that separate approval does Copilot publish/deploy to the named environment and channels. It verifies the service result and reports what was deployed. If an operation is unavailable or its result is uncertain, Copilot stops and reports that instead of claiming success.

Outlook connections and permissions are configured separately. The flow must not access live calendar data or create calendar events unless those connections are ready and the user separately approves the relevant action.

### One-time setup: Copilot Studio MCP in VS Code

This repository includes a workspace-scoped MCP registration in `.vscode/mcp.json` for Microsoft's `@microsoft/copilot-studio-mcp` package.

1. Ensure Node.js, npm, and `npx` are available, and that your organization permits the Microsoft-published MCP package.
2. Open `.vscode/mcp.json`, review the server configuration, then use VS Code's MCP controls to start and trust the server.
3. In Copilot Chat Agent mode, connect with your Microsoft work account and select the intended **development** environment. Review sign-in and consent prompts; never paste credentials or tokens into chat.
4. Keep tool approval enabled. The MCP connection is then available to the one-flow prompt above.

The MCP registration does not grant permissions, select an environment, create Outlook connections, or authorize publishing. Tool availability depends on the signed-in account, environment permissions, service rollout, and VS Code policy. If a required operation is unavailable, stop and report the limitation; use the manual upload steps above as a fallback.

### Single-flow prompt to dictate

```text
Run one guided workflow from my spoken requirements through GitHub to a deployed Copilot Studio agent. Keep all steps in this Copilot Chat session and pause only at the approval gates below.

Business requirements:
[Speak the goal, who will use the agent, what information it can use, the desired outcome, constraints, and actions it must not take.]

First inspect the repository and preserve unrelated content. Create or update only the relevant skill file as Markdown with YAML front matter containing a lowercase hyphenated name and a concise activation description. Include the agent's goal, step-by-step workflow, required tools and permissions, external-action safety rules, fallback behavior, response format, and at least three test prompts including a safety edge case. Validate the Markdown and test prompts, show me the diff, and wait for my approval before committing.

After I approve the file changes, commit and push them to the current GitHub branch. Verify the commit, remote synchronization, and clean worktree. Use the pushed skill as the source of truth for deployment.

Then use the configured Copilot Studio MCP. Read its current authoring guidance and discover the actual available operations. Confirm my signed-in identity and the exact development environment. Create or update the agent as a draft from the pushed skill, attach the skill if supported, and run the available tests. Do not guess operation names or claim success without tool evidence. If the connection or a required operation is unavailable, stop and explain what setup is missing.

Before publishing or deploying, summarize the exact agent, environment, channels, audience, and test results, then ask for a separate explicit approval. Never publish to production or broaden access by default. After approval, publish/deploy to the named target, verify the result, and summarize the deployed version and access. Do not access live calendar data or write calendar events as part of agent deployment.
```

### Voice and licensing notes

- **Win+H is dictation**, not ambient listening: it captures speech while the user invokes voice typing in the focused text field. Always review the transcript before sending it.
- VS Code voice-chat or wake-word availability can vary by product, extension, account, and tenant. The workshop does not depend on those features; use Windows Voice Typing or keyboard input as the fallback.
- Creating a skill file in VS Code and testing it with GitHub Copilot are separate from creating or publishing a Copilot Studio agent. The MCP route requires Copilot Studio access, an approved package, a signed-in work account, a deliberately selected environment, tenant billing, and publishing permissions.
- Saving a draft, testing it, publishing it, and deploying it to channels are separate outcomes. Copilot must report each one accurately and wait for explicit approval before publishing/deploying.
- A skill file does not configure Outlook access. Live calendar behavior requires separately configured and authorized Outlook actions.

## Microsoft documentation

- [Add and manage an uploaded skill](https://learn.microsoft.com/microsoft-copilot-studio/agents-experience/skills-add-existing)
- [Harnesses in Copilot Studio](https://learn.microsoft.com/microsoft-copilot-studio/harnesses-overview)
- [Copilot Studio licensing](https://learn.microsoft.com/microsoft-copilot-studio/billing-licensing)
- [Set up GitHub Copilot in VS Code](https://code.visualstudio.com/docs/copilot/setup)
- [Getting started with GitHub Copilot Chat in VS Code](https://code.visualstudio.com/docs/copilot/getting-started-chat)
- [Connect to Copilot Studio MCP with VS Code](https://learn.microsoft.com/microsoft-copilot-studio/agents-experience/copilot-studio-mcp-vscode-copilot-cli)
- [Use Copilot Studio MCP](https://learn.microsoft.com/microsoft-copilot-studio/agents-experience/copilot-studio-mcp)