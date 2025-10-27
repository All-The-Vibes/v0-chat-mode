# v0 Custom Chat Mode for GitHub Copilot

**Bring Vercel v0's Power Directly Into VS Code**

Build high-fidelity Next.js prototypes and production-ready frontends without leaving your editor—no API calls, no copy-paste from web UIs, just pure development flow.

---

## What Is This?

`v0-chat-mode` is a custom agent mode for GitHub Copilot that replicates the intelligent code generation, styling conventions, and Next.js best practices from **Vercel's v0** product. Instead of toggling between the v0 web interface and your code editor, you get v0's capabilities directly in VS Code through GitHub Copilot's Agent Mode.

### Why Use This?

- **No API Dependencies**: Generate v0-quality code without calling v0 or AI SDK APIs
- **Zero Context Switching**: Stay in your editor—no copying and pasting from web UIs
- **Visual-to-Code**: Paste screenshots, wireframes, or design mockups directly in chat to generate matching implementations
- **Production-Grade Code**: Get the same high-quality, modern Next.js patterns that v0 produces
- **Rapid Prototyping**: Build complete features with natural language prompts
- **Full Customization**: Extend or modify the agent mode to fit your workflow

### What Can It Do?

This custom mode generates:
- **Next.js App Router** components with best practices
- **Responsive UI** with Tailwind CSS and shadcn/ui
- **Type-safe TypeScript** code with proper patterns
- **Accessible components** following modern standards
- **Integration code** for popular services (Supabase, Stripe, etc.)
- **Visual-to-Code**: Paste screenshots, wireframes, or design mockups and the agent will implement matching frontends

---

## Quick Start

### Prerequisites

- **VS Code** version 1.99 or higher
- **GitHub Copilot** extension installed and active
- **GitHub Copilot Chat** enabled

### Installation Steps

1. **Ensure VS Code is up to date**
   - Check: `Help > About` should show version 1.99+
   - Update if needed: `Help > Check for Updates`

2. **Open GitHub Copilot Chat**
   - Click the chat icon in the top center of VS Code
   - Or use keyboard shortcut: `Ctrl+Alt+I` (Windows/Linux) or `Cmd+Shift+I` (Mac)

3. **Access Agent Mode**
   - Look at the bottom left of the chat panel
   - Click **"Agent"** to reveal the mode selector
   - Click **"Configure Modes..."**

4. **Create Custom Chat Mode**
   - Select **"Create new custom chat mode file"**
   - This creates a `.github/chatmodes` directory in your workspace

5. **Add v0 Chat Mode**
   - **Option A**: Copy the contents of `v0.chatmode.md` from this repository and paste it into the new file
   - **Option B**: Download `v0.chatmode.md` and drag-and-drop it into your `.github/chatmodes` directory

6. **Activate the Mode**
   - Return to GitHub Copilot Chat
   - Click the mode selector at the bottom left
   - Select your newly created v0 chat mode

You're ready to build! 🚀

---

## Usage Examples

Once activated, interact with the v0 chat mode using natural language:

```
"Create a responsive pricing page with three tiers"
```

```
"Build a dashboard layout with sidebar navigation and data cards"
```

```
"Add a contact form with validation using React Hook Form and Zod"
```

```
"Generate a blog post list with pagination and search"
```

The agent will generate complete, production-ready code following v0's conventions.

### Working with Visual Designs

One of the most powerful features is the ability to work from visual references:

**Paste screenshots or designs directly into the chat**, such as:
- Figma/Sketch design mockups
- Screenshots from other websites or apps
- Hand-drawn wireframes or diagrams
- Design inspiration from Dribbble or Behance

Then simply ask:
```
"Implement this design using shadcn/ui components"
```

```
"Create a React component matching this screenshot"
```

```
"Build this dashboard layout with the same visual hierarchy"
```

The agent will analyze the visual elements, layout patterns, color schemes, and component structure, then generate matching code that faithfully reproduces the design while following modern best practices.

---

## Recommended Configuration

For optimal performance, configure your VS Code settings (`Ctrl+,` or `Cmd+,`):

```json
{
  "chat.agent.enabled": true,
  "chat.tools.autoApprove": true,
  "chat.agent.maxRequests": 15
}
```

### Recommended AI Model

For the best results with this custom mode, use **Claude Sonnet 4.5** (if available in your GitHub Copilot settings). Claude Sonnet 4.5 provides:
- Superior reasoning for complex component architecture
- Better understanding of modern Next.js patterns
- More accurate multi-step code generation
- Excellent TypeScript inference

---

## Core Principles

This chat mode follows v0's established conventions:

### Code Organization
- **Modular components**: Single responsibility, composable pieces
- **File naming**: kebab-case for all files (`my-component.tsx`)
- **Type safety**: Full TypeScript with proper type definitions
- **Project structure**: Organized by feature, not file type

### Design Standards
- **Mobile-first**: Responsive layouts by default
- **Accessibility**: Semantic HTML and ARIA attributes
- **Styling**: Tailwind CSS v4 with consistent design tokens
- **Components**: shadcn/ui for common UI patterns

### Development Workflow
- **Surgical edits**: Update only relevant code sections
- **Clear reasoning**: Explains changes before implementation
- **Best practices**: Follows Next.js 14+ App Router conventions
- **No terminal**: All operations through VS Code UI and agent tools

### Supported Integrations
- Next.js App Router
- shadcn/ui components
- Tailwind CSS v4
- TypeScript
- React Server Components
- Popular services: Supabase, Stripe, Vercel, and more

---

## Features

### Intelligent Code Generation
- Generate entire features from natural language descriptions
- Automatic file creation, editing, and organization
- Context-aware suggestions based on your existing codebase

### Visual Design Implementation
- **Paste images directly**: Drop screenshots, mockups, or wireframes into chat
- **Design analysis**: Automatically identifies layouts, components, and styling patterns
- **Faithful reproduction**: Generates code that matches the visual design
- **Design tool agnostic**: Works with Figma, Sketch, Adobe XD, hand-drawn sketches, or any visual reference

### Design System Consistency
- Limited color palettes (5 tokens max)
- Maximum 2 font families per project
- Consistent spacing and layout patterns

### Quality Assurance
- TypeScript type checking
- Accessibility best practices
- Performance optimization patterns
- Error handling and validation

---

## Documentation

For detailed guidelines, coding standards, and advanced usage, see the complete specification in `v0.chatmode.md`.

---

## Examples & Templates

Check out the `/examples` directory (coming soon) for:
- Common component patterns
- Integration examples
- Full page templates
- Custom hook implementations

---

## Contributing

Contributions are welcome! If you have improvements to the chat mode or want to add new patterns:

1. Fork the repository
2. Create a feature branch
3. Update `v0.chatmode.md` with your changes
4. Submit a pull request with examples

---

## Resources

- [GitHub Copilot Chat Documentation](https://docs.github.com/en/copilot/using-github-copilot/asking-github-copilot-questions-in-your-ide)
- [VS Code Agent Mode Guide](https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode)
- [Next.js Documentation](https://nextjs.org/docs)
- [Vercel v0](https://v0.dev)

---

## License

MIT License - see [LICENSE](LICENSE) for details

---

## Acknowledgments

This project is inspired by and follows the conventions established by Vercel's v0 product. It is not officially affiliated with Vercel.

---

**Built with ❤️ for developers who want v0 power in their editor**
