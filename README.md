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

### Workshop workflow

1. Open this repository in VS Code and open Copilot Chat with **Ctrl+Alt+I**.
2. Select **Agent** mode and confirm the repository is the active workspace.
3. Focus the chat input and press **Win+H**. Speak the requirements as business language: the user's goal, inputs, expected result, constraints, and what the agent must never do.
4. Review and correct the transcript. Then submit it as the prompt below, replacing the bracketed section with the dictated requirements.
5. Let Copilot inspect the existing skill and README, then ask it to create or update only the relevant `SKILL.md`. The skill must have YAML front matter with `name` and `description`, plus clear workflow steps, safety rules, response format, and test prompts.
6. Review the proposed diff. Confirm that the skill uses only tools that will actually be configured, asks before external writes, and states its fallback when a tool is unavailable.
7. Ask Copilot to validate the Markdown and check the test prompts against the requirements. Fix any mismatch before accepting the changes.
8. Ask GitHub Copilot to use Copilot Studio MCP to confirm the signed-in identity, discover current capabilities, and show the selected development environment. Do not let it guess or select an environment implicitly.
9. Ask Copilot to create or update the agent as a draft from the reviewed skill. Have it inspect the available authoring schema and report any unsupported component rather than claiming it was added.
10. Ask Copilot to run the available tests against the skill's test prompts, then review the results and the exact agent, environment, tools, and channels that would be released.
11. Give a separate explicit approval to publish/deploy that reviewed agent to the named environment and channels. Copilot must verify the deployment result and report what is available to users.
12. Configure and authorize Outlook calendar actions separately before demonstrating live calendar reads or writes. Commit and push repository changes only after the workshop owner approves them.

### Copilot Studio MCP in VS Code

This repository includes a workspace-scoped MCP registration in `.vscode/mcp.json` for Microsoft's `@microsoft/copilot-studio-mcp` package.

1. Open this repository in VS Code. Ensure Node.js, npm, and `npx` are available, and that your organization permits the Microsoft-published MCP package.
2. Open `.vscode/mcp.json`, review the server configuration, then use VS Code's MCP controls to start and trust the server.
3. In Copilot Chat Agent mode, connect with your Microsoft work account and select the intended **development** environment. Review sign-in and consent prompts; never paste credentials or tokens into chat.
4. Ask Copilot to show the available Copilot Studio capabilities and confirm the account and environment before allowing changes.
5. Keep tool approval enabled. Ask Copilot to save a draft and test first. Publishing/deployment requires a separate approval after Copilot summarizes the agent, environment, channels, and test results.

The MCP registration does not grant permissions, select an environment, create Outlook connections, or authorize publishing. Tool availability depends on the signed-in account, environment permissions, service rollout, and VS Code policy. If a required operation is unavailable, stop and report the limitation; use the manual upload steps above as a fallback.

### Voice prompt to dictate

```text
Create or update an uploadable Copilot Studio skill in this repository from the business requirements below.

Business requirements:
[Speak the goal, who will use the agent, what information it can use, the desired outcome, constraints, and actions it must not take.]

First inspect the existing repository files and preserve unrelated content. Write the skill as Markdown with YAML front matter containing a lowercase hyphenated name and a concise activation description. Include the agent's goal, an explicit step-by-step workflow, required tools and permissions, safety rules for external actions, fallback behavior when a tool is unavailable, response format, and at least three test prompts including one safety edge case.

For this stage, only create or update the relevant Markdown file. Do not access live customer data or perform external actions. Show me the diff and validation results first.

After I approve the skill, use the configured Copilot Studio MCP to inspect its current authoring guidance and available operations. Confirm my signed-in account and the exact development environment. Create or update the agent as a draft, add the skill if the available schema supports it, and run the available tests. Do not invent operations or claim success without tool evidence.

Before publishing or deploying, summarize the exact agent, environment, channels, audience, and test results, then ask for a separate explicit approval. Never publish to production or broaden access by default. Commit or push GitHub changes only after I separately approve those changes.
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