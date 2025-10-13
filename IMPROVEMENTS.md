# Implementation Guide: v0 Chat Mode Improvements

This document provides specific, actionable changes to improve the v0 chat mode configuration based on the deep review.

---

## Priority 1: Critical Fixes (Implement Immediately)

### 1.1 Fix Formatting Errors

**Location:** `v0.chatmode.md` line 47

**Current:**
```markdown
Edited blog posts pagev2IMPORTANT:
```

**Fix to:**
```markdown
Edited blog posts page

## IMPORTANT:
```

### 1.2 Update Version References

**Location:** `README.md` lines 13-16, 59

**Changes needed:**
```markdown
# Old (lines 13-16):
For optimal agent workflow, **Claude Sonnet 4.5** is recommended...

# New:
For optimal agent workflow, **Claude 3.5 Sonnet** (or latest available model) is recommended as the underlying AI model for this repository's agent mode.

# Old (line 59):
1. Ensure you have VS Code (v1.99+) and the GitHub Copilot extension enabled.

# New:
1. Ensure you have VS Code (latest version) and the GitHub Copilot extension enabled.
```

### 1.3 Clarify Terminal Usage

**Location:** `README.md` and `v0.chatmode.md`

**Add to README.md after line 50:**
```markdown
### Terminal & Command Line

**For Users:**
- No terminal usage or command-line instructions—everything is managed in-IDE or via agent actions.
- All file operations, deployments, and configurations are handled through the v0 UI.

**For v0 Agent:**
- The agent does not use terminal commands when interacting with the v0 workspace.
- All operations are performed through provided tools (edit, new, runCommands as needed).
```

### 1.4 Add Missing Section: Error Handling

**Location:** `v0.chatmode.md` - Insert after line 236 (after "DO NOT fetch inside useEffect")

**New section:**
```markdown
## Error Handling & Recovery

### Client-Side Error Handling

**Form Submissions:**
```typescript
// Use try-catch with specific error handling
try {
  const response = await fetch('/api/endpoint', {
    method: 'POST',
    body: JSON.stringify(data)
  })
  
  if (!response.ok) {
    throw new Error(`API error: ${response.status}`)
  }
  
  const result = await response.json()
  // Handle success
} catch (error) {
  // Show user-friendly error message
  console.log("[v0] Error submitting form:", error)
  setError("Failed to submit. Please try again.")
}
```

**React Error Boundaries:**
```typescript
// Create error boundary for robust error handling
export default function ErrorBoundary({ 
  children 
}: { 
  children: React.ReactNode 
}) {
  return (
    <ErrorBoundary
      fallback={<ErrorFallback />}
      onError={(error) => console.log("[v0] Caught error:", error)}
    >
      {children}
    </ErrorBoundary>
  )
}
```

### Server-Side Error Handling

**API Routes:**
```typescript
// Always wrap route handlers in try-catch
export async function POST(request: Request) {
  try {
    const body = await request.json()
    
    // Validate input
    if (!body.email) {
      return Response.json(
        { error: "Email is required" },
        { status: 400 }
      )
    }
    
    // Process request
    const result = await processData(body)
    
    return Response.json({ success: true, data: result })
    
  } catch (error) {
    console.log("[v0] API error:", error)
    return Response.json(
      { error: "Internal server error" },
      { status: 500 }
    )
  }
}
```

**Server Actions:**
```typescript
'use server'

export async function submitForm(formData: FormData) {
  try {
    // Validate and process
    const data = {
      name: formData.get('name'),
      email: formData.get('email')
    }
    
    // Database operation
    await db.insert(users).values(data)
    
    return { success: true }
    
  } catch (error) {
    console.log("[v0] Server action error:", error)
    return { 
      success: false, 
      error: "Failed to save data" 
    }
  }
}
```

### Error Recovery Patterns

**Retry Logic:**
```typescript
async function fetchWithRetry(
  url: string, 
  maxRetries = 3
) {
  for (let i = 0; i < maxRetries; i++) {
    try {
      const response = await fetch(url)
      if (response.ok) return response
    } catch (error) {
      console.log(`[v0] Retry ${i + 1}/${maxRetries}`)
      if (i === maxRetries - 1) throw error
      await new Promise(r => setTimeout(r, 1000 * (i + 1)))
    }
  }
}
```

**Graceful Degradation:**
- Always provide fallback UI when features fail
- Show cached data when fresh data unavailable
- Allow users to proceed with limited functionality

### Integration Error Handling

**Database Errors:**
```typescript
try {
  await supabase.from('users').insert(data)
} catch (error) {
  if (error.code === '23505') {
    // Duplicate key error
    return { error: "Email already exists" }
  }
  // Generic error
  return { error: "Failed to create user" }
}
```

**External API Errors:**
```typescript
try {
  const result = await stripe.customers.create(data)
} catch (error) {
  if (error.type === 'StripeCardError') {
    return { error: "Card declined" }
  } else if (error.type === 'StripeRateLimitError') {
    return { error: "Too many requests, try again" }
  }
  return { error: "Payment processing failed" }
}
```

### Guidelines

- **ALWAYS** wrap async operations in try-catch blocks
- **ALWAYS** validate user input before processing
- **ALWAYS** return user-friendly error messages (never expose internal errors)
- **ALWAYS** log errors with `[v0]` prefix for debugging
- **NEVER** show stack traces or sensitive info to users
- **NEVER** assume external services will succeed
```

### 1.5 Add Missing Section: Security Best Practices

**Location:** `v0.chatmode.md` - Insert after Error Handling section

**New section:**
```markdown
## Security Best Practices

### Authentication & Authorization

**Row Level Security (RLS) - REQUIRED for Supabase:**
```sql
-- Enable RLS on all tables
ALTER TABLE users ENABLE ROW LEVEL SECURITY;

-- Example: Users can only read their own data
CREATE POLICY "Users can view own data"
ON users FOR SELECT
USING (auth.uid() = id);

-- Example: Users can only update their own data
CREATE POLICY "Users can update own data"
ON users FOR UPDATE
USING (auth.uid() = id);
```

**Server-Side Auth Checks:**
```typescript
// ALWAYS verify auth on server
export async function GET(request: Request) {
  const supabase = createServerClient(/* ... */)
  
  const { data: { user }, error } = await supabase.auth.getUser()
  
  if (!user || error) {
    return Response.json(
      { error: "Unauthorized" },
      { status: 401 }
    )
  }
  
  // Proceed with authorized request
}
```

### Input Validation & Sanitization

**Server-Side Validation (REQUIRED):**
```typescript
import { z } from 'zod'

const userSchema = z.object({
  email: z.string().email(),
  name: z.string().min(2).max(100),
  age: z.number().min(18).max(120)
})

export async function POST(request: Request) {
  try {
    const body = await request.json()
    
    // Validate input
    const validData = userSchema.parse(body)
    
    // Process validated data
    await processUser(validData)
    
  } catch (error) {
    if (error instanceof z.ZodError) {
      return Response.json(
        { error: "Invalid input", details: error.errors },
        { status: 400 }
      )
    }
    throw error
  }
}
```

**SQL Injection Prevention:**
```typescript
// ALWAYS use parameterized queries
// ✅ GOOD: Using ORM or query builder
await db.select().from(users).where(eq(users.id, userId))

// ✅ GOOD: Using prepared statements
const { data } = await supabase
  .from('users')
  .select()
  .eq('id', userId)

// ❌ BAD: String concatenation
await db.execute(`SELECT * FROM users WHERE id = '${userId}'`) // NEVER DO THIS
```

### XSS Prevention

**Content Rendering:**
```typescript
// React automatically escapes content
// ✅ SAFE:
<div>{userInput}</div>

// ⚠️ DANGEROUS (avoid unless necessary):
<div dangerouslySetInnerHTML={{ __html: sanitizedHTML }} />

// If you MUST render HTML, sanitize first:
import DOMPurify from 'isomorphic-dompurify'
const clean = DOMPurify.sanitize(dirtyHTML)
```

### CSRF Protection

**Form Submissions:**
```typescript
// Next.js Server Actions have built-in CSRF protection
// API routes should verify origin
export async function POST(request: Request) {
  const origin = request.headers.get('origin')
  const host = request.headers.get('host')
  
  if (origin !== `https://${host}`) {
    return Response.json(
      { error: "Invalid origin" },
      { status: 403 }
    )
  }
  
  // Process request
}
```

### Environment Variables

**Sensitive Data:**
```typescript
// ✅ GOOD: Server-only env vars
const secret = process.env.SECRET_KEY // Only accessible on server

// ⚠️ PUBLIC: Client-accessible (use sparingly)
const publicKey = process.env.NEXT_PUBLIC_API_KEY // Exposed to browser

// Guidelines:
// - NEVER commit .env files to git
// - NEVER expose secrets to client
// - ALWAYS use NEXT_PUBLIC_ prefix for client vars
// - ALWAYS validate env vars on startup
```

**Environment Variable Validation:**
```typescript
// lib/env.ts
const envSchema = z.object({
  DATABASE_URL: z.string().url(),
  SUPABASE_KEY: z.string().min(1),
  STRIPE_SECRET: z.string().min(1),
})

export const env = envSchema.parse(process.env)
```

### Rate Limiting

**API Route Protection:**
```typescript
import { Ratelimit } from '@upstash/ratelimit'
import { Redis } from '@upstash/redis'

const ratelimit = new Ratelimit({
  redis: Redis.fromEnv(),
  limiter: Ratelimit.slidingWindow(10, '10 s'),
})

export async function POST(request: Request) {
  const ip = request.headers.get('x-forwarded-for')
  const { success } = await ratelimit.limit(ip ?? 'anonymous')
  
  if (!success) {
    return Response.json(
      { error: "Too many requests" },
      { status: 429 }
    )
  }
  
  // Process request
}
```

### Security Checklist

Before deploying any code, verify:

- [ ] All database tables have RLS policies enabled
- [ ] Server actions verify user authentication
- [ ] User input is validated on server-side
- [ ] Parameterized queries used (no string concatenation)
- [ ] Environment variables not exposed to client
- [ ] Rate limiting on public API endpoints
- [ ] HTTPS enforced in production
- [ ] Security headers configured (CSP, HSTS, etc.)
- [ ] No sensitive data logged to console in production
- [ ] Dependencies regularly updated (no known vulnerabilities)

### Security Headers

**next.config.mjs:**
```javascript
/** @type {import('next').NextConfig} */
const nextConfig = {
  async headers() {
    return [
      {
        source: '/(.*)',
        headers: [
          {
            key: 'X-Frame-Options',
            value: 'DENY',
          },
          {
            key: 'X-Content-Type-Options',
            value: 'nosniff',
          },
          {
            key: 'Referrer-Policy',
            value: 'strict-origin-when-cross-origin',
          },
        ],
      },
    ]
  },
}

export default nextConfig
```
```

---

## Priority 2: High-Value Improvements

### 2.1 Streamline Tool Descriptions

**Strategy:** Reduce verbose descriptions by moving detailed content to dedicated sections.

**Example - GrepRepo (current: ~1000 chars, target: ~400 chars):**

```markdown
# Current (too verbose):
"Searches for regex patterns within file contents across the repository. Returns matching lines with file paths and line numbers, perfect for code exploration and analysis.

Primary use cases:
• Find function definitions: 'function\\s+myFunction' or 'const\\s+\\w+\\s*='
• Locate imports/exports: 'import.*from' or 'export\\s+(default|\\{)'
..."

# Improved (concise):
"Searches repository file contents using regex patterns. Returns matching lines with paths and line numbers.

Use for: function definitions, imports/exports, class declarations, API calls, configuration keys, usage tracking.

Supports: glob patterns (*.ts, src/**), path filtering, case-insensitive matching. Max 200 results returned."
```

### 2.2 Add Table of Contents

**Location:** `v0.chatmode.md` - Insert after line 5 (after YAML frontmatter and intro)

```markdown
## Table of Contents

1. [CodeProject](#codeproject)
   - [Write To File](#write-to-file)
   - [Delete Files](#delete-files)
   - [Rename or Move Files](#rename-or-move-files)
   - [Image and Assets](#image-and-assets-in-code-projects)
   - [Executable Scripts](#executable-scripts)
   - [Debugging](#debugging)

2. [Thinking](#thinking)

3. [Math](#math)

4. [Coding Guidelines](#coding-guidelines)
   - [Context Gathering](#context-gathering)
   - [Integration Support](#integration-support)
   - [Error Handling](#error-handling--recovery)
   - [Security Best Practices](#security-best-practices)

5. [Design Guidelines](#design-guidelines)
   - [Color System](#color-system)
   - [Typography](#typography)
   - [Layout Structure](#layout-structure)
   - [Tailwind Implementation](#tailwind-implementation)
   - [Visual Elements & Icons](#visual-elements--icons)

6. [Integration Guidelines](#integration-guidelines)
   - [Supabase](#supabase-integration-guidelines)
   - [Neon](#neon-integration-guidelines)
   - [Upstash](#upstash-for-redis-integration-guidelines)
   - [Blob, AI Providers, Stripe](#integration-guidelines)

7. [v0 Capabilities](#v0-capabilities)

8. [Refusals](#refusals)

9. [Alignment](#alignment)

10. [Tools](#current-project)
    - [FetchFromWeb](#fetchfromweb)
    - [GrepRepo](#greprepo)
    - [LSRepo](#lsrepo)
    - [ReadFile](#readfile)
    - [InspectSite](#inspectsite)
    - [SearchWeb](#searchweb)
    - [TodoManager](#todomanager)
    - [SearchRepo](#searchrepo)
    - [GenerateDesignInspiration](#generatedesigninspiration)
    - [GetOrRequestIntegration](#getorrequestintegration)

---
```

### 2.3 Enhance Integration Guidelines

**Add to Neon section (line 373):**

```markdown
## Neon Integration Guidelines

- v0 MUST use the `@neondatabase/serverless` package to interact with a Neon database.
- v0 MUST use the `neon(...)` function to create a reusable SQL client.
- v0 NEVER uses the `@vercel/postgres` package to interact with a Neon database.

**Basic Setup:**
```typescript
import { neon } from '@neondatabase/serverless'

// Create SQL client
const sql = neon(process.env.DATABASE_URL!)

// Query example
const users = await sql`
  SELECT * FROM users 
  WHERE email = ${email}
`
```

**Connection Pooling:**
```typescript
import { Pool } from '@neondatabase/serverless'

const pool = new Pool({ 
  connectionString: process.env.DATABASE_URL 
})

// Use for multiple queries
const client = await pool.connect()
try {
  await client.query('BEGIN')
  await client.query('INSERT INTO users ...')
  await client.query('INSERT INTO profiles ...')
  await client.query('COMMIT')
} finally {
  client.release()
}
```

**Transactions:**
```typescript
// Use the sql client for simple transactions
await sql`BEGIN`
try {
  await sql`INSERT INTO users VALUES (${data})`
  await sql`INSERT INTO profiles VALUES (${profile})`
  await sql`COMMIT`
} catch (error) {
  await sql`ROLLBACK`
  throw error
}
```

**Best Practices:**
- Always use parameterized queries (template literals)
- Enable connection pooling for high-traffic apps
- Use transactions for related operations
- Handle connection errors gracefully
```

### 2.4 Add Testing Guidelines Section

**Location:** `v0.chatmode.md` - Insert after Security section

```markdown
## Testing Guidelines

### Test File Structure

```
app/
  page.tsx
  page.test.tsx        # Unit tests for page component
  
components/
  button.tsx
  button.test.tsx      # Unit tests for component
  
api/
  route.ts
  route.test.ts        # Unit tests for API route
  
e2e/
  user-flow.spec.ts    # End-to-end tests
```

### Component Testing

**React Component Tests:**
```typescript
import { render, screen } from '@testing-library/react'
import { Button } from './button'

describe('Button', () => {
  it('renders with correct text', () => {
    render(<Button>Click me</Button>)
    expect(screen.getByText('Click me')).toBeInTheDocument()
  })
  
  it('calls onClick when clicked', () => {
    const handleClick = jest.fn()
    render(<Button onClick={handleClick}>Click</Button>)
    screen.getByText('Click').click()
    expect(handleClick).toHaveBeenCalledTimes(1)
  })
})
```

### API Route Testing

**Next.js API Route Tests:**
```typescript
import { POST } from './route'

describe('POST /api/users', () => {
  it('creates a user successfully', async () => {
    const request = new Request('http://localhost/api/users', {
      method: 'POST',
      body: JSON.stringify({ 
        email: 'test@example.com',
        name: 'Test User'
      })
    })
    
    const response = await POST(request)
    const data = await response.json()
    
    expect(response.status).toBe(200)
    expect(data.success).toBe(true)
  })
  
  it('validates required fields', async () => {
    const request = new Request('http://localhost/api/users', {
      method: 'POST',
      body: JSON.stringify({})
    })
    
    const response = await POST(request)
    
    expect(response.status).toBe(400)
  })
})
```

### Integration Testing

**Database Integration Tests:**
```typescript
import { createClient } from '@supabase/supabase-js'

describe('User CRUD operations', () => {
  let supabase: ReturnType<typeof createClient>
  
  beforeAll(() => {
    supabase = createClient(
      process.env.TEST_SUPABASE_URL!,
      process.env.TEST_SUPABASE_KEY!
    )
  })
  
  it('creates and retrieves user', async () => {
    const { data: user } = await supabase
      .from('users')
      .insert({ email: 'test@example.com' })
      .select()
      .single()
    
    expect(user.email).toBe('test@example.com')
    
    // Cleanup
    await supabase.from('users').delete().eq('id', user.id)
  })
})
```

### E2E Testing

**Playwright E2E Tests:**
```typescript
import { test, expect } from '@playwright/test'

test('user can sign up and log in', async ({ page }) => {
  await page.goto('http://localhost:3000')
  
  // Sign up
  await page.click('text=Sign Up')
  await page.fill('input[name="email"]', 'test@example.com')
  await page.fill('input[name="password"]', 'password123')
  await page.click('button[type="submit"]')
  
  // Verify redirect to dashboard
  await expect(page).toHaveURL('/dashboard')
  await expect(page.locator('h1')).toContainText('Welcome')
})
```

### Testing Best Practices

- **Unit Tests:** Test individual components and functions in isolation
- **Integration Tests:** Test interactions between components and services
- **E2E Tests:** Test complete user workflows
- **Coverage Goals:** Aim for 80%+ coverage on critical paths
- **Test Naming:** Use descriptive names that explain what is being tested
- **AAA Pattern:** Arrange → Act → Assert structure
- **Mock External Services:** Don't hit real APIs in tests
- **Clean Up:** Always clean up test data after tests run

### When to Write Tests

- **Always:** Critical business logic, authentication, payments
- **Often:** Complex components, API routes, data transformations
- **Sometimes:** Simple presentational components, utilities
- **Rarely:** Third-party integrations (trust their tests)

v0 will write tests when:
- User explicitly requests tests
- Building critical features (auth, payments, data operations)
- Project already has testing infrastructure in place
```

---

## Priority 3: Medium-Term Enhancements

### 3.1 Add Performance Optimization Section

**Location:** After Testing Guidelines

```markdown
## Performance Optimization

### Image Optimization

**Next.js Image Component:**
```typescript
import Image from 'next/image'

// ✅ GOOD: Use Next.js Image component
<Image
  src="/hero.jpg"
  alt="Hero image"
  width={1200}
  height={600}
  priority // For above-the-fold images
  placeholder="blur"
/>

// ❌ BAD: Regular img tag
<img src="/hero.jpg" alt="Hero" />
```

### Code Splitting

**Dynamic Imports:**
```typescript
// Lazy load heavy components
import dynamic from 'next/dynamic'

const HeavyComponent = dynamic(() => import('./HeavyComponent'), {
  loading: () => <div>Loading...</div>,
  ssr: false // Disable SSR if not needed
})
```

### Bundle Size Optimization

**Tree Shaking:**
```typescript
// ✅ GOOD: Named imports (tree-shakeable)
import { Button, Card } from '@/components/ui'

// ❌ BAD: Default imports of entire library
import * as UI from '@/components/ui'
```

**Analyze Bundle:**
```bash
# Add to package.json scripts
"analyze": "ANALYZE=true next build"
```

### Data Fetching Optimization

**Static Generation:**
```typescript
// Generate at build time for static content
export async function generateStaticParams() {
  const posts = await getPosts()
  return posts.map((post) => ({ slug: post.slug }))
}
```

**Streaming:**
```typescript
// Stream data for faster perceived performance
import { Suspense } from 'react'

export default function Page() {
  return (
    <Suspense fallback={<Skeleton />}>
      <DataComponent />
    </Suspense>
  )
}
```

### Caching Strategies

**SWR for Client State:**
```typescript
import useSWR from 'swr'

function Profile() {
  const { data, error, isLoading } = useSWR(
    '/api/user',
    fetcher,
    {
      revalidateOnFocus: false,
      revalidateOnReconnect: false,
      refreshInterval: 60000 // Refresh every minute
    }
  )
}
```

### Performance Checklist

- [ ] Images optimized (WebP, proper sizing)
- [ ] Heavy components lazy loaded
- [ ] Bundle analyzed (no unnecessary dependencies)
- [ ] Static pages pre-rendered
- [ ] API responses cached appropriately
- [ ] Database queries optimized (indexes, select only needed fields)
- [ ] CSS purged (unused styles removed)
- [ ] Fonts preloaded
- [ ] Third-party scripts loaded efficiently
```

### 3.2 Improve Example Quality

**Replace Example 1 (philosophical question) with practical example:**

```markdown
<example>
[User] 
Can you add a loading state to my button component?

[Assistant]

<Thinking>
This is a straightforward enhancement to an existing component. I should:
1. First read the current button component to understand its structure
2. Add loading state with prop and disabled behavior
3. Show loading indicator (spinner or text)
4. Maintain existing functionality
</Thinking>

Let me first check your current button implementation.

*Calls ReadFile with path to button component*

I can see your button component. I'll add a loading state with a spinner and disabled behavior.

*Edits button component to add isLoading prop, disabled state, and spinner icon*

Your button now supports a loading state! Use it like this:
```tsx
<Button isLoading={isSubmitting}>
  Submit Form
</Button>
```

When `isLoading` is true, the button shows a spinner and becomes disabled to prevent multiple submissions.

</example>
```

---

## Priority 4: Long-Term Enhancements

### 4.1 Create Validation Schema

**New file:** `validation-schema.json`

```json
{
  "$schema": "http://json-schema.org/draft-07/schema#",
  "title": "v0 Chat Mode Validation",
  "type": "object",
  "required": ["codeEdits", "designSystem", "integrations"],
  "properties": {
    "codeEdits": {
      "type": "object",
      "properties": {
        "readBeforeWrite": {
          "type": "boolean",
          "const": true,
          "description": "Must read file before editing"
        },
        "surgicalEdits": {
          "type": "boolean",
          "const": true,
          "description": "Only edit necessary sections"
        },
        "preserveExisting": {
          "type": "boolean",
          "const": true,
          "description": "Use // ... existing code ... marker"
        }
      },
      "required": ["readBeforeWrite", "surgicalEdits", "preserveExisting"]
    },
    "designSystem": {
      "type": "object",
      "properties": {
        "colorCount": {
          "type": "integer",
          "minimum": 3,
          "maximum": 5,
          "description": "Total colors used (3-5)"
        },
        "fontFamilies": {
          "type": "integer",
          "maximum": 2,
          "description": "Maximum font families (1-2)"
        },
        "mobileFirst": {
          "type": "boolean",
          "const": true,
          "description": "Design mobile-first"
        }
      },
      "required": ["colorCount", "fontFamilies", "mobileFirst"]
    },
    "integrations": {
      "type": "object",
      "properties": {
        "rlsEnabled": {
          "type": "boolean",
          "description": "Row Level Security enabled for Supabase"
        },
        "envVarsValidated": {
          "type": "boolean",
          "description": "Environment variables validated"
        },
        "errorHandling": {
          "type": "boolean",
          "const": true,
          "description": "Proper error handling implemented"
        }
      },
      "required": ["errorHandling"]
    }
  }
}
```

### 4.2 Add CI/CD Documentation

**New section in README.md:**

```markdown
## Deployment & CI/CD

### Vercel Deployment

1. **Connect GitHub Repository**
   - Go to vercel.com/new
   - Import your repository
   - Vercel automatically detects Next.js configuration

2. **Configure Environment Variables**
   - Add all required env vars in Project Settings → Environment Variables
   - Set different values for Preview and Production environments
   - Use Vercel Integrations for auto-configuration

3. **Deploy**
   - Every push to main triggers production deployment
   - Pull requests create preview deployments automatically
   - Review deployments in Vercel dashboard

### Environment-Specific Configuration

**Development (.env.local):**
```bash
DATABASE_URL="postgresql://localhost:5432/dev"
NEXT_PUBLIC_APP_URL="http://localhost:3000"
```

**Production (Vercel Dashboard):**
```bash
DATABASE_URL="postgresql://prod.neon.tech/prod"
NEXT_PUBLIC_APP_URL="https://myapp.com"
```

### Build Optimization

**package.json:**
```json
{
  "scripts": {
    "build": "next build",
    "build:analyze": "ANALYZE=true next build",
    "postbuild": "next-sitemap"
  }
}
```

### Monitoring

- Enable Vercel Analytics in project settings
- Set up error tracking with Sentry or similar
- Monitor Core Web Vitals in Vercel dashboard
- Set up uptime monitoring for critical endpoints
```

### 4.3 Create Quick Reference Guide

**New file:** `QUICK_REFERENCE.md`

```markdown
# v0 Chat Mode Quick Reference

## Most Common Operations

### Code Editing
```
1. Read file first: ReadFile → 
2. Edit surgically: Only change what's needed →
3. Use markers: // ... existing code ...
```

### Context Gathering
```
Quick search: GrepRepo (regex patterns)
List files: LSRepo (directory structure)
Read file: ReadFile (full content or chunks)
Deep dive: SearchRepo (comprehensive analysis)
```

### Design Constraints
```
Colors: 3-5 total
Fonts: Maximum 2 families
Layout: Mobile-first, Flexbox → Grid
```

### Security Checklist
```
✓ RLS enabled on all tables
✓ Server-side validation
✓ Parameterized queries
✓ No secrets in client code
✓ Rate limiting on public APIs
```

### Error Handling Template
```typescript
try {
  // Operation
} catch (error) {
  console.log("[v0] Error:", error)
  return { error: "User-friendly message" }
}
```

## Tool Selection Guide

| Need | Tool | Why |
|------|------|-----|
| Find function | GrepRepo | Fast pattern matching |
| List directory | LSRepo | See structure |
| Read full file | ReadFile | Complete content |
| Architecture overview | SearchRepo | Comprehensive analysis |
| Check integration | GetOrRequestIntegration | Env vars + schema |
| Web documentation | SearchWeb | Latest info + sources |
| Screenshot | InspectSite | Visual verification |
| Design ideas | GenerateDesignInspiration | Creative direction |
| Task planning | TodoManager | Multi-step projects |

## Integration Quick Reference

### Supabase
```typescript
import { createBrowserClient } from '@supabase/ssr'
const supabase = createBrowserClient(url, key)
await supabase.from('table').select()
```

### Neon
```typescript
import { neon } from '@neondatabase/serverless'
const sql = neon(process.env.DATABASE_URL!)
const users = await sql`SELECT * FROM users`
```

### Stripe
```typescript
import Stripe from 'stripe'
const stripe = new Stripe(process.env.STRIPE_SECRET_KEY!)
await stripe.customers.create({ email })
```

## Common Patterns

### Form with Validation
```typescript
'use server'
import { z } from 'zod'

const schema = z.object({
  email: z.string().email()
})

export async function submitForm(formData: FormData) {
  try {
    const data = schema.parse({
      email: formData.get('email')
    })
    await saveToDb(data)
    return { success: true }
  } catch (error) {
    return { error: "Invalid input" }
  }
}
```

### Authenticated API Route
```typescript
export async function GET(request: Request) {
  const supabase = createServerClient(/* ... */)
  const { data: { user } } = await supabase.auth.getUser()
  
  if (!user) {
    return Response.json({ error: "Unauthorized" }, { status: 401 })
  }
  
  // Process request
}
```

### Loading State Component
```typescript
export function DataComponent() {
  const { data, isLoading, error } = useSWR('/api/data')
  
  if (isLoading) return <Skeleton />
  if (error) return <Error message={error} />
  
  return <Display data={data} />
}
```
```

---

## Implementation Checklist

Use this checklist to track implementation of improvements:

### Phase 1: Critical Fixes (Week 1)
- [ ] Fix formatting error on line 47
- [ ] Update version references (VS Code, Claude)
- [ ] Clarify terminal usage guidance
- [ ] Add Error Handling section
- [ ] Add Security Best Practices section

### Phase 2: High-Value Improvements (Week 2-3)
- [ ] Streamline tool descriptions (reduce by 40%)
- [ ] Add table of contents
- [ ] Enhance Neon integration guidelines
- [ ] Add Testing Guidelines section
- [ ] Improve example quality (replace philosophical example)

### Phase 3: Medium-Term Enhancements (Week 4-6)
- [ ] Add Performance Optimization section
- [ ] Create Quick Reference Guide
- [ ] Add more practical examples
- [ ] Expand integration documentation
- [ ] Add troubleshooting guide

### Phase 4: Long-Term Enhancements (Month 2-3)
- [ ] Create validation schema
- [ ] Add CI/CD documentation
- [ ] Build automated testing
- [ ] Create metrics/analytics
- [ ] Add advanced features (task dependencies, etc.)

---

## Success Metrics

Track these metrics to measure improvement impact:

1. **Documentation Quality**
   - Time to find information (target: <2 minutes)
   - User satisfaction score (target: 8+/10)
   - Number of clarifying questions needed (target: <2 per task)

2. **Code Quality**
   - Security issues detected (target: 0 critical, <5 medium)
   - Test coverage (target: >80% on critical paths)
   - Performance score (target: >90 Lighthouse score)

3. **Developer Experience**
   - Setup time for new developers (target: <15 minutes)
   - Time to first successful generation (target: <10 minutes)
   - Error rate (target: <10% of generations)

---

**Document Version:** 1.0  
**Last Updated:** October 13, 2025  
**Next Review:** January 2026
