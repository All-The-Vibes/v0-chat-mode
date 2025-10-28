# v0 Custom Chat Mode for GitHub Copilot

<div align="center">
  <img src="https://media1.giphy.com/media/v1.Y2lkPTc5MGI3NjExbzBxMXJndjdmaWVic3ExdmI0aHBjanNua3ltMHRheG0zMTVia2RjdyZlcD12MV9pbnRlcm5hbF9naWZfYnlfaWQmY3Q9Zw/TFO2mwVPIFoOJcuTSC/giphy.gif" width="250" alt="Coding magic in action"/>
  
  <h2>Bring _Vercel's v0_ Directly Into VS Code</h2>
  
  <p>Build Next.js prototypes and front end apps without leaving your editor—no API calls, no copy-paste from web UIs, just pure development flow in your existing codebase.</p>
</div>



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

### What Can It Generate?

This custom mode creates **modern, production-ready web applications** across a wide range of categories:

#### **Application Types**

**E-commerce & Business**
- Shopping carts and product catalogs
- Checkout flows with Stripe integration
- Pricing pages and landing pages
- Business websites with lead generation

**Content & Media Platforms**
- Blog platforms with posts and pagination
- Marketing and promotional websites
- Portfolio and showcase sites
- Documentation sites and knowledge bases

**SaaS & Dashboards**
- Admin dashboards with data visualization
- Analytics and metrics dashboards
- User management interfaces
- Settings and configuration panels

**Authentication & User Systems**
- Login and registration flows
- User profile pages and account management
- OAuth integrations (via Supabase)
- Protected routes and role-based access control

**Database-Driven Applications**
- Full CRUD applications (Supabase, Neon, Upstash)
- Real-time collaborative apps
- Search functionality (Upstash Search)
- Redis-cached high-performance apps

**AI-Powered Applications**
- Chatbots and conversational interfaces
- AI content generation tools
- Image generation apps (via fal, Deep Infra)
- LLM-powered features (Groq, Grok/xAI)

#### **Technical Capabilities**

- **Next.js App Router** with server and client components
- **shadcn/ui** component library integration
- **Tailwind CSS v4** with design tokens
- **TypeScript** with full type safety
- **Responsive design** with mobile-first approach
- **Form handling** with validation (React Hook Form, Zod)
- **API integration** (RESTful and real-time)
- **File storage** (Vercel Blob)
- **Payment processing** (Stripe)
- **Visual-to-Code**: Paste screenshots, wireframes, or design mockups and the agent will implement matching frontends

---

## Quick Start

### Prerequisites

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

## Supported Integrations

The v0 chat mode has built-in support for popular services and can automatically check integration status, retrieve environment variables, and access live database schemas:

### **Databases & Storage**
- **Supabase** - PostgreSQL database, authentication, real-time subscriptions
- **Neon** - Serverless PostgreSQL
- **Upstash Redis** - Serverless Redis for caching and session storage
- **Vercel Blob** - File and media storage

### **AI & Machine Learning**
- **Groq** - Fast LLM inference
- **Grok (xAI)** - AI model access
- **fal** - AI image generation and models
- **Deep Infra** - AI model hosting and inference

### **Search & Performance**
- **Upstash Search** - Vector search and semantic search

### **Payments & Commerce**
- **Stripe** - Payment processing, subscriptions, and checkout

The agent can automatically request these integrations from you if they're needed for your project, and will access live database schemas to generate accurate queries and ORM code.

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
- Popular services: Supabase, Neon, Upstash, Stripe, Groq, Grok, fal, Deep Infra, and more (see [Supported Integrations](#supported-integrations) for full list)

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
