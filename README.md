# v0 GitHub Copilot Custom Agent Mode

**Advanced Frontend Agent Mode Template for Next.js & Modern Web Tooling**

***

## Purpose

`v0-chat-mode` provides a best-practices automation and code generation agent for web app development with Next.js (App Router) and modern tool integrations.  This assistant leverages Visual Studio Code’s Agent Mode and orchestrates code generation, scaffolding, editing, and integration tasks—built for rapid prototyping and deployment in any compatible workspace.

***

## Recommended Model Selection

For optimal agent workflow, **Claude 3.5 Sonnet** (or latest available model) is recommended as the underlying AI model for this repository’s agent mode.  
Claude 3.5 Sonnet provides advanced reasoning, code understanding, and multi-step orchestration capability, ensuring best-in-class results for frontend and automation tasks.

***

## Key Usage Principles

### Code Project Management

- Group all React/Next.js component files within Code Projects.
- Edit files surgically: update only relevant sections, showing placeholders as needed (e.g., `// ... existing code ...`).
- Prefer kebab-case for filenames: `my-component.tsx`.
- Create, rename/move, and delete files using built-in agent tool actions—never manual/terminal commands.

### Supported Tooling & Integrations

- Full support for Next.js (App Router), shadcn/ui, and other popular cloud/AI tools (Azure, Stripe, Supabase, Groq, xAI).
- All configuration is handled via workspace UI or agent mode actions—no direct `.env` editing, terminal commands, or blob asset URLs.
- Scripts for migration, seeding, or task automation go under `/scripts`.

### Debugging & Reasoning

- Use `console.log("[v0] ...")` for all debug and trace output.
- Reason “out loud” before code generation, and summarize planned changes and rationale in postambles.

### Coding & Design Standards

- Mobile-first layouts, semantic HTML, Tailwind CSS v4 tokens.
- Use max 2 font families, 5 color tokens; always split code into modular components.
- For AI features, use only approved providers—see your workspace requirements.
- Environment variables should be set according to recommended project best practices.

### User Experience

- All user interaction is through the workspace—support drag/drop, file previews, GitHub push, ZIP download, deploy processes.
- No terminal usage or command-line instructions—everything is managed in-IDE or via agent actions.
- For support, direct users to project maintainers or workspace customer service.

### Terminal & Command Line

**For Users:**
- No terminal usage or command-line instructions—everything is managed in-IDE or via agent actions.
- All file operations, deployments, and configurations are handled through the v0 UI.

**For v0 Agent:**
- The agent does not use terminal commands when interacting with the v0 workspace.
- All operations are performed through provided tools (edit, new, runCommands as needed).

***

## Visual Studio Code Agent Mode Setup

**Quickstart for Agent Mode Automation**

1. Ensure you have VS Code (latest version) and the GitHub Copilot extension enabled.
2. In VS Code settings (`Ctrl+,`), enable agent mode:
    ```json
    {
      "chat.agent.enabled": true,
      "chat.tools.autoApprove": true,
      "chat.agent.maxRequests": 15
    }
    ```
3. Open Copilot Chat, select **Agent** mode.
4. Enter high-level requests:
    - “Refactor all API calls to use async/await”
    - “Add end-to-end tests for user registration”
    - “Migrate app from Express.js to Fastify”
5. Agent mode will plan, edit, run tools, and iterate on requests autonomously—review proposed changes and approve/reject in editor.

See [VS Code Agent Mode Docs](https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode) for full configuration and best practices.

***

## Best Practices Summary

- Use Code Projects for grouping/editing all code.
- Apply mobile-first and modular design.
- Respect all alignment and coding guidelines (see `/v0.chatmode.md` for deep detail).
- Use agent mode and tooling for all edits, integrations, and deployments.
- Never access terminal—use integrated agents and UI only.

***
