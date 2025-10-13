# v0 GitHub Copilot Custom Agent Mode #
This repository provides a foundation for building a custom agent mode with GitHub Copilot in Visual Studio Code. With agent mode, Copilot can autonomously execute complex coding tasks across your workspace using natural language instructions.

## Features

- Custom Copilot Agent Mode
- Autonomous multi-step coding/fixes
- Edit/refactor across multiple files
- Tool and terminal command integration
- Iterative, context-aware workflows

## Getting Started

### Prerequisites

- **Visual Studio Code** (latest version recommended)
- **GitHub Copilot** subscription (Free or Pro for full features)

### Installation

1. Clone this repository:
    ```bash
    git clone https://github.com/All-The-Vibes/v0-chat-mode.git
    cd v0-chat-mode
    ```

2. Open the folder in Visual Studio Code:
    ```bash
    code .
    ```

3. Make sure the GitHub Copilot extension is installed and enabled in VS Code.

## Using Copilot Agent Mode in VS Code

Agent mode enables Copilot to operate autonomously in your workspace.

> **How to use agent mode:**
>
> 1. Open the Copilot Chat window in VS Code.
> 2. Select **Agent** from the mode dropdown.
> 3. Enter your prompt (e.g. “Refactor all API calls to use async/await”).
> 4. Copilot will plan, edit, run commands and iterate on the task until completed.
> 5. Review, accept or reject proposed changes using the editor UI.
>
> See official docs: [Customize chat to your workflow – VS Code](https://code.visualstudio.com/docs/copilot/customization/overview).[1][2][3]

## Custom Instructions for Copilot

You can tailor Copilot agent mode using custom instructions files:

- **Repository-wide instructions:** Create `.github/copilot-instructions.md` in your repo with natural language guidelines for Copilot.
- **Agent mode instructions:** Add `AGENTS.md` to the root of your workspace to guide agent behavior.

> To create repository-wide custom instructions:
> 1. Create `.github/copilot-instructions.md` in your repo.
> 2. Write high-level goals or development rules in Markdown.
> 3. Optionally, use path-specific instructions in `.github/instructions/NAME.instructions.md`.

See: [Adding repository custom instructions for GitHub Copilot](https://docs.github.com/copilot/customizing-copilot/adding-custom-instructions-for-github-copilot).[4][5]

Here’s how you can include **official Visual Studio Code and Microsoft docs instructions** for configuring custom agent mode in your `v0-chat-mode` README, combining the steps referenced in your Perplexity page and authoritative documentation:

***

# Configure Custom Agent Mode in Visual Studio Code

## 1. Enable Agent Mode

- **Update Visual Studio Code** to version 1.99 or newer to access agent mode and advanced chat features.
- Open **Settings** (`Ctrl+,`), search for `"chat.agent.enabled"`, and enable it.  
  This unlocks agent mode for autonomous planning, multi-step tasks, code edits, terminal commands, and tool invocation.[1]

- **Agent Mode Workflow:**
    - Open the **Copilot Chat** window.
    - Select **“Agent”** from the mode dropdown.
    - Enter a high-level prompt (e.g., “Refactor all API methods for async/await”).
    - Copilot agent mode will operate across your codebase, planning, making edits, running tools, and iteratively resolving tasks.

## 2. Configure Chat and Tools

- **Additional Settings:**
    - `chat.tools.autoApprove` (auto-approval for agent-invoked tools)
    - `chat.agent.maxRequests` (limit agent requests per prompt)
    - `chat.tools.terminal.autoApprove` (terminal command approval)
- Example settings for `.vscode/settings.json`:
    ```json
    {
      "chat.agent.enabled": true,
      "chat.tools.autoApprove": true,
      "chat.agent.maxRequests": 10
    }
    ```

## 3. Modes Overview

| Mode   | Description                        | Example Usage                               |
|--------|------------------------------------|---------------------------------------------|
| Ask    | Q&A, explanations, code snippets   | “How does auth work here?”                  |
| Edit   | Multi-file code proposals/changes  | “Add error handling to payment service”      |
| Agent  | Autonomous, multi-step planning    | “Migrate React to Vue”                      |
| Custom | Specialized workflows, orchestration | Feature planning, CI/CD tasks                |

See: [VS Code Modes Documentation][1]

## 4. Create and Manage Custom Agents

- Install the **Agents Toolkit extension** (Visual Studio only)
- Create new agent projects with Microsoft 365 Agents SDK templates or use existing orchestration frameworks (Semantic Kernel, Azure AI Foundry, etc.)
- Configure endpoints, keys, and project settings as required for agent integration

> You can create agents in C#, Python, or JavaScript, define task orchestration, customize endpoints/toolchains via config files, and use MCP servers for advanced integrations.[1]

## 5. Advanced Customization (SDK & Toolkit)

- Select agent templates (Weather Agent, Empty Agent, etc.)
- Integrate semantic kernel, orchestration layers, or any AI/ML platform
- Manually define agent-to-agent communication and orchestration flows
- Secure keys/endpoints with access controls, enable logging and monitoring for debugging

## 6. Security & Best Practices

- Always review terminal commands and file changes proposed by agent mode.
- Use workspace and project settings for granular control of agent behaviors.

## 7. References & Further Reading

- [Agent Mode Documentation (Visual Studio)](https://learn.microsoft.com/en-us/visualstudio/ide/copilot-agent-mode?view=vs-2022)
- [Agent Mode Chat in VS Code](https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode)
- [Microsoft 365 Agents SDK Quickstart](https://learn.microsoft.com/en-us/microsoft-365-copilot/extensibility/create-deploy-agents-sdk)
- [Custom Chat Modes](https://code.visualstudio.com/docs/copilot/customization/custom-chat-modes)
- [Agents Toolkit](https://learn.microsoft.com/en-us/microsoft-365/agents-sdk/create-new-toolkit-project-vs)

***

**Add these instructions directly to the top of your v0-chat-mode README for fully accurate, up-to-date configuration steps direct from official Visual Studio Code and Microsoft Docs sources.**

[1](https://www.perplexity.ai/search/configure-custom-agent-mode-vi-0Dikny8PSW.l8l2RElfCuQ)

## Example Prompts for Agent Mode

- “Refactor all API endpoints to use the latest authentication scheme.”
- “Add end-to-end tests for user registration.”
- “Migrate codebase from Express.js to Fastify.”

The more context you add to your instructions and custom files, the better Copilot agent will perform.[6][7]

## Security Notice

Review all changes before accepting them, especially when using tools or invoking terminal commands. Learn more: [Security documentation for AI in VS Code](https://code.visualstudio.com/docs/copilot/security).

## Resources

- [GitHub Copilot Official Docs](https://docs.github.com/en/copilot)
- [VS Code Copilot Customization](https://code.visualstudio.com/docs/copilot/customization/overview)

***

**Contributing & License**

Feel free to open issues or pull requests to discuss improvements or report bugs.

***

**References:**

- Visual Studio Code – [Customize chat to your workflow][2][7][1]
- GitHub Docs – [Adding custom instructions][5][4]
- GitHub Copilot Blog – [Agent Mode Introduction][6]
- VS Code Copilot Agent Mode – [Official Blog][3]

***

Ready to build and orchestrate development with Copilot Agent Mode – just open this repo in VS Code and try your custom prompts and instructions!
[20](https://docs.github.com/en/copilot/how-tos/use-copilot-extensions/build-a-copilot-agent)
[21](https://learn.microsoft.com/en-us/visualstudio/ide/copilot-chat-context?view=vs-2022)
