# Inkubit Business AI

Workshop-ready agent skills for Microsoft Copilot Studio.

## Outlook Day Planner

The first skill turns spoken or written business priorities into a proposed workday plan based on the user's Outlook calendar, then tracks agreed work in a selected Microsoft Planner Kanban plan. It treats existing meetings and tasks as fixed and requires explicit approval before calendar or board changes.

Skill file: [`.github/skills/outlook-day-planner/SKILL.md`](.github/skills/outlook-day-planner/SKILL.md)

## Manual skill upload (optional)

1. Create or open an agent powered by the **GitHub Copilot harness**.
2. Open **Build** and choose **Skills** > **Add skill** > **Upload a skill**.
3. Upload `.github/skills/outlook-day-planner/SKILL.md`.
4. Open **Preview** and test with a prompt such as: “Plan my day tomorrow. My top priorities are the customer proposal and the team review.”

This is a manual alternative. The GitHub Actions deployment below packages the CLI-authored agent from `copilot-agent/settings.mcs.yml`. It does not upload `.github/skills/outlook-day-planner/SKILL.md` as a separate item in the Studio Skills panel; the agent instructions are maintained in the CLI workspace as well.

## Outlook connection

To plan from live calendar data, configure an authorized Outlook calendar read action for the agent. To create approved focus blocks, also configure an Outlook calendar create action. Test both actions in the target environment and verify the signed-in identity and permissions.

Without calendar access, the skill must ask the user to provide their appointments and clearly label the result as a plan based only on that information. Calendar writes must remain approval-gated.

## Planner Kanban board

The board is a standard Microsoft Planner plan opened in the Planner app, not a custom board embedded inside Copilot Studio. In the target Studio environment, add the Microsoft **Planner** connector to the agent and authorize a connection. Enable the operations the workflow needs: **List plans for a group**, **List buckets**, **List tasks**, **Create a task**, and **Update a task**. The connector currently supports basic plans.

Choose an existing Planner plan and keep its bucket names. The agent reads tasks before suggesting new cards, shows proposed titles, known due dates, and status, then waits for approval before creating or updating tasks. It does not create plans or buckets, delete tasks, or assign other people without separate approval. If the connected update action cannot move a card between buckets, it must say so and must not claim the card moved.

Planner connections are environment-specific and are not provisioned by the current GitHub Actions solution package. Add and authorize the Planner connector in Copilot Studio after the first deployment; verify the connection works before using board actions. Without it, the agent can still propose a task list but cannot update the board.

## Licensing and harness

The **GitHub Copilot harness in Copilot Studio** is an authoring and orchestration framework; it is not the GitHub Copilot service. Microsoft documents Microsoft 365 Copilot licensing as a way to access Copilot Studio authoring for this harness. Agent use can consume Copilot Credits, depending on the configuration. Confirm the tenant's current licensing, billing, and publishing permissions before the workshop.

Other Copilot Studio harnesses have different licensing and publishing options. A Copilot Studio trial can be used to build and test in the test chat, but does not allow publishing.

## Suggested workshop flow

1. Collect a participant's business requirements in natural language.
2. Upload this skill to the agent.
3. Preview the proposed day plan using sample or authorized calendar data.
4. Review conflicts and assumptions, then approve or edit the proposal.
5. Demonstrate calendar or Planner writes only if the relevant connector actions, permissions, and approval flow are configured in advance.

## Voice-first agent generation from VS Code

This workflow turns spoken business requirements into a GitHub change that automatically packages and deploys the agent to a protected Copilot Studio development environment. It uses **Windows Voice Typing** to dictate into VS Code; it does not require or imply an always-on wake-word listener. The speaker reviews the transcript before submitting it to Copilot.

### Prerequisites

- VS Code with GitHub Copilot Chat installed and signed in.
- This repository cloned and opened as the VS Code workspace.
- A working microphone and Windows Voice Typing. In the Copilot Chat input, press **Win+H** to dictate; if voice typing is unavailable, type or paste the same prompt.
- A Dataverse development environment with a database, and a service principal allowed to import solutions and publish agents there.
- A GitHub Actions environment named `copilot-studio-dev`, configured with a required reviewer and the repository variables/secrets listed below.

### One flow: voice -> VS Code -> GitHub -> Copilot Studio

GitHub is the deployment bridge. The skill file guides GitHub Copilot while the CLI workspace contains the agent instructions that PAC packages into a Dataverse solution. A push to `main` starts GitHub Actions; the protected GitHub environment provides the release approval before import and publish.

1. Open this repository in VS Code. Open Copilot Chat with **Ctrl+Alt+I**, select **Agent** mode, and confirm this repository is the active workspace.
2. Focus the chat input, press **Win+H**, and speak the business requirements. Review and correct the transcript before sending it with the single-flow prompt below.
3. Copilot updates the authoring skill in `.github/skills/outlook-day-planner/SKILL.md` and mirrors the runtime behavior in `copilot-agent/settings.mcs.yml`. It validates the Markdown and packages the workspace locally with `pac copilot pack` before showing the diff.
4. After you approve the changes, Copilot commits and pushes them to `main`. The push triggers `.github/workflows/deploy-copilot-studio.yml`.
5. GitHub pauses the deployment at the protected `copilot-studio-dev` environment for its required reviewer approval.
6. After approval, the workflow authenticates with the scoped service principal, imports the packaged solution into the configured Dataverse development environment, publishes the agent, and checks its deployment status.
7. Review the GitHub Actions run for the exact commit and deployment result. A failed or unverified operation is reported as a failure, not as a completed deployment.

Outlook connections and permissions are configured separately in the target environment. The deployment workflow does not create connector connections or calendar events.

### One-time setup: GitHub deployment environment

The deploy workflow is `.github/workflows/deploy-copilot-studio.yml`. Before the first push that changes `copilot-agent/`, configure these items in GitHub:

- Create a GitHub Actions environment named `copilot-studio-dev` and add a required reviewer so pushes cannot publish without approval.
- Add repository variable `POWERPLATFORM_ENVIRONMENT_URL` with the Dataverse development environment URL. The deployment job stays skipped until this variable is set.
- Add environment secrets `POWERPLATFORM_APP_ID`, `POWERPLATFORM_TENANT_ID`, and `POWERPLATFORM_CLIENT_SECRET` for a Microsoft Entra service principal. Grant that application user only the environment permissions needed to import the solution and publish the agent.
- After the first deployment, add and authorize the Planner connector in Copilot Studio, enable the required plan/bucket/task actions, and select the plan for the workshop. Planner connections are environment-specific and are not created by this workflow.
- Confirm the environment has a Dataverse database and the service principal is registered as an application user in it.
- Keep production as a separate environment and workflow gate; this repository's workflow targets development only.

The action does not create environments or grant permissions. Never put secrets in YAML or source files. A push to `main` triggers deployment only for changes under `copilot-agent/` or to the workflow itself. The `workflow_dispatch` event is also available after setup.

### Single-flow prompt to dictate

```text
Use this repository's GitHub Actions path to turn my spoken requirements into an Outlook Day Planner agent deployed to Copilot Studio. Keep all authoring in this workspace and do not call Copilot Studio MCP directly.

Business requirements:
[Speak the goal, who will use the agent, working hours, priorities, Planner plan if known, desired outcome, constraints, and actions it must not take.]

First inspect the repository and preserve unrelated content. Update `.github/skills/outlook-day-planner/SKILL.md` with YAML front matter and guidance for Outlook scheduling plus the Planner Kanban board. Mirror the runtime behavior in `copilot-agent/settings.mcs.yml`, including selected-plan handling, duplicate checks, bucket/status proposals, and separate approval gates for calendar and Planner writes. Do not change the solution schema name or publisher prefix without asking. Validate both files and run `pac copilot pack` locally. Show me the diff and package result, and wait for my approval before committing.

After I approve the file changes, commit and push them to `main`. Verify the commit and remote synchronization. The GitHub Actions workflow will then package and deploy the CLI-authored agent; do not deploy directly from the workstation.

The GitHub Actions deployment must wait for the required reviewer approval on the `copilot-studio-dev` environment. It must authenticate using GitHub environment secrets, run `pac solution import --publish-changes`, publish `inkubit_OutlookDayPlanner`, and verify with `pac copilot status`. Never deploy to production or broaden agent access by default. Do not access live calendar or Planner data or write calendar events or tasks as part of agent deployment.
```

### Voice and licensing notes

- **Win+H is dictation**, not ambient listening: it captures speech while the user invokes voice typing in the focused text field. Always review the transcript before sending it.
- VS Code voice-chat or wake-word availability can vary by product, extension, account, and tenant. The workshop does not depend on those features; use Windows Voice Typing or keyboard input as the fallback.
- `.github/skills/outlook-day-planner/SKILL.md` is the authoring source; `copilot-agent/settings.mcs.yml` is the deployable CLI-authored agent definition. Keep their behavior synchronized.
- GitHub Actions packages and imports the CLI-authored agent through a Dataverse solution. This is not an upload of the `SKILL.md` as a separate Studio Skills panel component.
- GitHub Actions must use an approved service principal and a protected GitHub environment. The successful package, import, publish, and status check are separate outcomes; inspect the action logs for each.
- A skill file does not configure Outlook access. Live calendar behavior requires separately configured and authorized Outlook actions.
- The Planner connector supports basic plans. A selected Planner plan and its authorized connector connection must exist before the agent can read or change Kanban tasks.

## Microsoft documentation

- [Add and manage an uploaded skill](https://learn.microsoft.com/microsoft-copilot-studio/agents-experience/skills-add-existing)
- [Harnesses in Copilot Studio](https://learn.microsoft.com/microsoft-copilot-studio/harnesses-overview)
- [Copilot Studio licensing](https://learn.microsoft.com/microsoft-copilot-studio/billing-licensing)
- [Set up GitHub Copilot in VS Code](https://code.visualstudio.com/docs/copilot/setup)
- [Getting started with GitHub Copilot Chat in VS Code](https://code.visualstudio.com/docs/copilot/getting-started-chat)
- [PAC CLI Copilot commands](https://learn.microsoft.com/power-platform/developer/cli/reference/copilot)
- [GitHub Actions for Microsoft Power Platform](https://learn.microsoft.com/power-platform/alm/devops-github-actions)
- [Configure GitHub Actions service principal access](https://learn.microsoft.com/power-platform/alm/tutorials/github-actions-start)
- [Planner connector reference](https://learn.microsoft.com/connectors/planner/)
- [Planner task board and Graph overview](https://learn.microsoft.com/graph/planner-concept-overview)