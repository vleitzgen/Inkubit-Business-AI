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

## Microsoft documentation

- [Add and manage an uploaded skill](https://learn.microsoft.com/microsoft-copilot-studio/agents-experience/skills-add-existing)
- [Harnesses in Copilot Studio](https://learn.microsoft.com/microsoft-copilot-studio/harnesses-overview)
- [Copilot Studio licensing](https://learn.microsoft.com/microsoft-copilot-studio/billing-licensing)