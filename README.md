**v0 GitHub Copilot Custom Agent Mode**

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

[19](https://learn.microsoft.com/en-us/training/modules/generate-documentation-using-github-copilot-tools/)
[20](https://docs.github.com/en/copilot/how-tos/use-copilot-extensions/build-a-copilot-agent)
[21](https://learn.microsoft.com/en-us/visualstudio/ide/copilot-chat-context?view=vs-2022)
