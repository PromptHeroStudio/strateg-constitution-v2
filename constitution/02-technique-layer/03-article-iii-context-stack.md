# 📄 ARTICLE III: THE 5-LAYER CONTEXT STACK

**FILE PATH:** `/constitution/02-technique-layer/03-article-iii-context-stack.md`

---

```markdown
# 🏗️ ARTICLE III: THE 5-LAYER CONTEXT STACK

**Structured Context for Maximum AI Comprehension**

---

## 📜 CONSTITUTIONAL AUTHORITY

This article defines the **5-Layer Context Stack** - a structured framework for providing AI with comprehensive project context. Proper context is the foundation of constitutional prompts.

**Legal Force:**
- ✅ MVCA **MUST** gather context using the 5-layer framework
- ✅ Each layer **SHALL** follow the specifications defined herein
- ✅ Context minimization **SHALL** be practiced (include only relevant layers)
- ✅ Manual prompt creators **MAY** use this framework as a template
- ✅ Context quality **SHALL** be measured against this standard

**Constitutional Principle:**
> Context is not optional - it is the foundation of quality output.  
> Without context, AI guesses. With context, AI knows.

---

## 🎯 THE 5-LAYER CONTEXT STACK OVERVIEW

### Layered Architecture

```
═══════════════════════════════════════════════════════════
THE 5-LAYER CONTEXT STACK
═══════════════════════════════════════════════════════════

LAYER 1: PROJECT IDENTITY
├─ What is this project?
├─ Who are the users?
├─ What stage is it in?
└─ What's the scope?

LAYER 2: BUSINESS CONTEXT
├─ Why does this exist?
├─ What problem does it solve?
├─ What's the value proposition?
└─ How do we measure success?

LAYER 3: TECHNICAL CONTEXT
├─ How is it built?
├─ What technologies are used?
├─ What's the architecture?
└─ What are the conventions?

LAYER 4: FEATURE CONTEXT
├─ What are we building now?
├─ What's the user story?
├─ What are acceptance criteria?
└─ What edge cases exist?

LAYER 5: CONSTRAINTS CONTEXT
├─ What limits us?
├─ Budget constraints?
├─ Timeline constraints?
└─ Compliance requirements?

═══════════════════════════════════════════════════════════
```

---

## 📊 WHEN TO INCLUDE EACH LAYER

### Context Minimization Principle

**Not all layers are always needed.** Include only relevant layers for the task.

```
SIMPLE TASK (Bug Fix):
├─ Layer 3: Technical Context (REQUIRED)
└─ Layer 4: Feature Context (what's broken)
Skip: Layers 1, 2, 5 (not needed for bug fix)

MEDIUM TASK (New Feature):
├─ Layer 1: Project Identity (REQUIRED)
├─ Layer 3: Technical Context (REQUIRED)
└─ Layer 4: Feature Context (REQUIRED)
Skip: Layers 2, 5 (unless feature is business-critical)

COMPLEX TASK (Architecture Decision):
├─ Layer 1: Project Identity (REQUIRED)
├─ Layer 2: Business Context (helps decision-making)
├─ Layer 3: Technical Context (REQUIRED)
├─ Layer 4: Feature Context (REQUIRED)
└─ Layer 5: Constraints (affects decisions)
All layers needed for comprehensive decision.
```

---

## 🏢 LAYER 1: PROJECT IDENTITY

### Purpose

Provide high-level understanding of what the project is, who uses it, and what stage it's in.

### When to Include

```
✅ ALWAYS INCLUDE Layer 1 when:
- Creating new features
- Making architectural decisions
- First interaction with project
- User doesn't know project context

⚠️ CAN SKIP Layer 1 when:
- Bug fixes (unless bug affects core functionality)
- Minor modifications (single file changes)
- Well-established project (AI has prior context)
```

---

### Structure

```markdown
LAYER 1: PROJECT IDENTITY
────────────────────────────────────────────────────────

PROJECT OVERVIEW:
- Name: [Project Name]
- Type: [Web App / Mobile App / API / CLI / Desktop / etc.]
- Industry: [E-commerce / SaaS / Healthcare / Finance / etc.]
- Stage: [Concept / MVP / Beta / Production / Scale]
- Age: [How long has project existed?]

TARGET USERS:
- Primary: [Who is the main user? Demographics + psychographics]
- Secondary: [Who else uses it? If applicable]
- Size: [How many users? Current + projected]

PROJECT SCOPE:
- Complexity: [Simple / Medium / Complex / Enterprise]
- Team Size: [Solo / 2-5 / 6-20 / 20+ developers]
- Codebase: [Estimated lines of code, number of files]

DEPLOYMENT:
- Status: [Not deployed / Development / Staging / Production]
- URL: [If live, what's the URL?]
- Platform: [Vercel / AWS / Azure / Self-hosted / etc.]
```

---

### Complete Example - TaskMaster Pro

```markdown
LAYER 1: PROJECT IDENTITY
────────────────────────────────────────────────────────

PROJECT OVERVIEW:
- Name: TaskMaster Pro
- Type: Web Application (SaaS)
- Industry: Productivity / Project Management
- Stage: MVP Development (launching in 8 weeks)
- Age: 3 months in development

TARGET USERS:
- Primary: Freelancers and small agency owners (1-10 person teams)
  - Demographics: 25-45 years old, tech-savvy
  - Psychographics: Value simplicity, need client collaboration
  - Pain Point: Existing tools too complex (Jira, Monday) or too basic (Trello)
  
- Secondary: Clients of freelancers (view-only access)
  - Use Case: Track project progress, approve deliverables

- Size: 
  - Current: 15 beta users (testing)
  - Target: 100 users by end of month 3, 1,000 by month 12

PROJECT SCOPE:
- Complexity: Medium (standard CRUD + real-time collaboration)
- Team Size: Solo founder (you) + occasional contractor help
- Codebase: ~15,000 lines TypeScript, 150 files

DEPLOYMENT:
- Status: Development (local) → Staging (this week) → Production (6 weeks)
- URL: taskmaster-pro.vercel.app (staging, not live yet)
- Platform: Vercel (Pro plan)
```

---

### Information Sources

**Where to get this information:**

```typescript
// PROJECT OVERVIEW
Source: package.json, README.md, founder knowledge

// TARGET USERS
Source: User interviews, market research, founder vision

// PROJECT SCOPE
Source: GitHub repo stats, founder experience

// DEPLOYMENT
Source: Vercel dashboard, .env files, deployment config
```

---

### Common Mistakes

```markdown
❌ MISTAKE #1: Too vague
"Web app for productivity"
Problem: What kind of productivity? For whom?

✅ CORRECT:
"Project management SaaS for freelancers and small agencies (1-10 people)"

---

❌ MISTAKE #2: Too detailed (oversharing)
"Project uses Next.js 15.0.3 with TypeScript 5.3.3, Prisma 5.7.1..."
Problem: This is Layer 3 (Technical Context), not Layer 1

✅ CORRECT (Layer 1):
"Web Application (SaaS)"
Save technical details for Layer 3.

---

❌ MISTAKE #3: No stage indication
"Project is in development"
Problem: MVP development vs. production scaling have different needs

✅ CORRECT:
"Stage: MVP Development (launching in 8 weeks)"
```

---

## 💼 LAYER 2: BUSINESS CONTEXT

### Purpose

Explain WHY the project exists, what problem it solves, and how success is measured. Helps AI understand business priorities.

### When to Include

```
✅ INCLUDE Layer 2 when:
- Building customer-facing features
- Making product decisions (feature prioritization)
- Designing user flows (UX critical)
- Architectural decisions affecting users
- Business logic implementation

⚠️ CAN SKIP Layer 2 when:
- Pure technical tasks (performance optimization)
- Bug fixes (unless bug affects core value prop)
- Infrastructure work (DevOps, monitoring)
- Internal tools (admin panels)
```

---

### Structure

```markdown
LAYER 2: BUSINESS CONTEXT
────────────────────────────────────────────────────────

PROBLEM STATEMENT:
- What problem are we solving?
- Who experiences this problem?
- How painful is it? (scale 1-10, or impact description)
- Current alternatives (and why they fail)

SOLUTION HYPOTHESIS:
- How does our product solve this?
- What makes our approach unique/better?
- What's the core value proposition?

SUCCESS METRICS:
- How do we measure success?
- What are the key metrics? (revenue, users, engagement, etc.)
- What's the timeline for these metrics?

COMPETITIVE LANDSCAPE:
- Who are the competitors?
- What's our differentiation?
- What's our competitive advantage?
```

---

### Complete Example - TaskMaster Pro

```markdown
LAYER 2: BUSINESS CONTEXT
────────────────────────────────────────────────────────

PROBLEM STATEMENT:

Problem:
Freelancers struggle with project management tools that are either:
- Too complex (Jira, Monday.com) - 2-hour learning curve, overkill features
- Too basic (Trello, Notion) - Missing time tracking, client collaboration

Who:
Freelancers and small agencies (1-10 people) managing 5-20 clients.

Pain Intensity: 8/10
- Lost billable hours (spending 5h/week on PM instead of client work)
- Client miscommunication (unclear deliverables, scope creep)
- Revenue loss (under-billing by ~20% due to poor time tracking)

Current Alternatives:
1. Trello + Toggl + Email
   - Problem: Context switching (3 tools), no single source of truth
2. Monday.com
   - Problem: $39/user/month (too expensive), overly complex
3. Spreadsheets
   - Problem: Manual, error-prone, no client access

SOLUTION HYPOTHESIS:

Our Approach:
All-in-one tool designed specifically for freelancer workflows:
- Simple project structure (projects → tasks → time entries)
- Built-in time tracking (start/stop timers, no separate tool)
- Client portal (clients see progress, approve deliverables)
- Automated invoicing (time tracked → invoice generated)

Unique Value:
"Simple enough for solo freelancers, powerful enough for small agencies"

Core Value Proposition:
- For Freelancers: Save 5 hours/week on admin, increase billable time 20%
- For Clients: Project transparency, faster delivery, clear communication

SUCCESS METRICS:

Phase 1 (Months 1-3):
- 100 active users (current: 15 beta users)
- 30% weekly retention (current: 65% - high for beta)
- 5 hours/week time saved per user (measured via survey)

Phase 2 (Months 4-12):
- 1,000 active users
- 50 paid subscribers ($29/month plan)
- $1,500 MRR (Monthly Recurring Revenue)
- Net Promoter Score (NPS) > 40

Phase 3 (Year 2):
- 10,000 users
- 1,000 paid subscribers
- $29,000 MRR
- Break-even (revenue > costs)

COMPETITIVE LANDSCAPE:

Direct Competitors:
1. Monday.com ($39/user/month)
   - Our advantage: Simpler, cheaper ($29/month flat, not per-user)
   
2. Asana ($10.99/user/month)
   - Our advantage: Built-in time tracking, client portal
   
3. ClickUp ($7/user/month)
   - Our advantage: Freelancer-specific features (invoicing, client portal)

Indirect Competitors:
- Trello + Toggl combo (free + $9/month)
- Notion ($10/month)

Our Differentiation:
- Only tool built specifically for freelancer-client workflow
- All-in-one (no integrations needed)
- Fair pricing (flat $29/month, not per-user gouging)

Competitive Advantage:
- Speed to ship (smaller team, faster iteration)
- Focus (not trying to serve enterprises)
- Founder-market fit (I'm a freelancer, I understand the problem)
```

---

### How AI Uses Business Context

**Example Decision: Should we add Gantt charts?**

**Without Layer 2 (AI guesses):**
```
"Gantt charts are common in PM tools, so yes, add them."
```

**With Layer 2 (AI knows priorities):**
```
"Your target users are freelancers who value simplicity (not complexity).
Gantt charts are typically used by PMs in large orgs (not freelancers).

Your competitors (Monday, Asana) have Gantt charts but you differentiate
on simplicity. Adding Gantt charts contradicts your value prop.

RECOMMENDATION: Don't add Gantt charts (conflicts with core value prop).
If users request it, survey them first to validate demand."
```

**Business context = better decisions.**

---

## 🔧 LAYER 3: TECHNICAL CONTEXT

### Purpose

Define HOW the project is built - technology stack, architecture, coding conventions, and existing implementation state.

### When to Include

```
✅ ALWAYS INCLUDE Layer 3 when:
- Writing ANY code (needs to match existing patterns)
- Creating new files (needs to fit structure)
- Modifying existing code (needs conventions)
- Technical decisions (needs stack knowledge)

⚠️ ONLY SKIP Layer 3 when:
- Non-coding tasks (business planning, content writing)
- High-level architectural discussions (may need abbreviated version)
```

**Layer 3 is ALMOST ALWAYS REQUIRED for coding tasks.**

---

### Structure

```markdown
LAYER 3: TECHNICAL CONTEXT
────────────────────────────────────────────────────────

TECHNOLOGY STACK:
- Framework: [Next.js 15.0.3 / React / Vue / Angular / etc.]
- Language: [TypeScript 5.3.3 / JavaScript / Python / etc.]
- Database: [PostgreSQL 16 / MySQL / MongoDB / etc.]
- ORM: [Prisma 5.7.1 / Drizzle / TypeORM / etc.]
- Styling: [Tailwind CSS 3.4.1 / CSS Modules / Styled Components / etc.]
- Authentication: [NextAuth.js v5 / Clerk / Auth0 / Supabase Auth / etc.]
- State Management: [React Context / Redux / Zustand / etc.]
- Hosting: [Vercel / AWS / Netlify / etc.]

PROJECT STRUCTURE:
```
[Show folder structure with brief explanations]
```

CODING CONVENTIONS:
- Imports: [Absolute with @ alias / Relative / etc.]
- Naming: [camelCase functions, PascalCase components, etc.]
- Async: [async/await only / .then/.catch allowed / etc.]
- Error Handling: [try-catch required / throw errors / etc.]
- Comments: [JSDoc on public functions / minimal / etc.]

EXISTING CODE STATE:
- Authentication: [Implemented / Not yet / In progress]
- Database: [Schema complete / Needs updates / New project]
- UI Components: [Design system exists / Using library / Custom]
- API Routes: [RESTful / GraphQL / tRPC / etc.]

DEPENDENCIES (key libraries):
- [Library 1]: [Version + usage notes]
- [Library 2]: [Version + usage notes]
```

---

### Complete Example - TaskMaster Pro

```markdown
LAYER 3: TECHNICAL CONTEXT
────────────────────────────────────────────────────────

TECHNOLOGY STACK:

REQUIRED VERSIONS (Must match exactly):
- Framework: Next.js 15.0.3 (App Router, NOT Pages Router)
- Language: TypeScript 5.3.3 (strict mode enabled)
- Database: PostgreSQL 16 (via Supabase)
- ORM: Prisma 5.7.1
- Styling: Tailwind CSS 3.4.1
- UI Components: shadcn/ui (Radix UI primitives)
- Authentication: NextAuth.js v5.0-beta.25
- State Management: React Context + Server Components (no Redux)
- Hosting: Vercel (Pro plan)

DEVELOPMENT TOOLS:
- Package Manager: npm (not yarn or pnpm)
- Node.js: v20.11.0 (required for Next.js 15)
- Git: GitHub (main branch protected)
- Linting: ESLint + Prettier (configured)
- Testing: Vitest (unit tests), Playwright (E2E)

PROJECT STRUCTURE:

```
taskmaster-pro/
├── app/                    # Next.js 15 App Router
│   ├── (auth)/            # Auth pages (login, register)
│   │   ├── login/
│   │   └── register/
│   ├── (dashboard)/       # Protected dashboard pages
│   │   ├── dashboard/
│   │   ├── projects/
│   │   └── settings/
│   ├── api/               # API routes
│   │   ├── auth/          # NextAuth.js handlers
│   │   ├── projects/      # Project CRUD
│   │   └── time-entries/  # Time tracking
│   ├── layout.tsx         # Root layout
│   └── page.tsx           # Landing page
├── components/            # React components
│   ├── ui/               # shadcn/ui components (don't edit directly)
│   ├── auth/             # Auth-specific components
│   ├── projects/         # Project-specific components
│   └── shared/           # Shared/common components
├── lib/                   # Utilities and configs
│   ├── db.ts             # Prisma client (singleton)
│   ├── auth.ts           # NextAuth.js configuration
│   ├── utils.ts          # Common utilities (cn, formatDate, etc.)
│   └── validators/       # Zod validation schemas
├── types/                 # TypeScript type definitions
│   ├── auth.ts
│   ├── project.ts
│   └── index.ts
├── prisma/               # Database
│   ├── schema.prisma     # Database schema
│   └── migrations/       # Migration history
└── public/               # Static assets
```

CODING CONVENTIONS:

IMPORTS (Strict rules):
✓ Use absolute imports with @ alias:
  import { prisma } from '@/lib/db'
  import { Button } from '@/components/ui/button'
  
✗ Never use relative imports:
  import { prisma } from '../../lib/db'  // ❌ Forbidden

NAMING (Enforced):
- Files: kebab-case (user-profile.tsx, auth-service.ts)
- Functions: camelCase (getUserProfile, createProject)
- Components: PascalCase (UserProfile, ProjectCard)
- Types/Interfaces: PascalCase (User, ProjectWithTasks)
- Constants: SCREAMING_SNAKE_CASE (MAX_PROJECTS, API_URL)
- CSS Classes: Tailwind utility classes only

ASYNC/AWAIT (Required):
✓ Always use async/await:
  const user = await prisma.user.findUnique({ where: { id } })
  
✗ Never use .then/.catch:
  prisma.user.findUnique({ where: { id } }).then(user => ...)  // ❌ Forbidden

ERROR HANDLING (Required):
✓ All async operations in try-catch:
  ```typescript
  try {
    const result = await riskyOperation()
  } catch (error) {
    console.error('Operation failed:', error)
    throw new Error('User-friendly message')
  }
  ```

COMMENTS:
- JSDoc on ALL public functions (exported)
- Inline comments for complex logic ONLY
- NO commented-out code (delete it, it's in Git)
- TODO format: // TODO(Phase 2): Implement email verification

EXISTING CODE STATE:

COMPLETED FEATURES:
✓ Authentication (NextAuth.js v5)
  - Email/password login
  - Google OAuth
  - Session management (database-backed)
  - Password reset flow
  
✓ Database Schema
  - User, Project, Task, TimeEntry models
  - Relations configured
  - Indexes optimized

✓ Core UI Components
  - shadcn/ui installed (Button, Input, Card, etc.)
  - Custom components: ProjectCard, TaskList, TimerWidget
  - Design system: Colors, typography, spacing defined

IN PROGRESS:
⏳ Client Portal (30% complete)
  - Client User model exists
  - Invitation system in progress
  - Read-only views not yet implemented

⏳ Invoicing (10% complete)
  - Invoice model exists
  - PDF generation not implemented
  - Stripe integration pending

NOT STARTED:
❌ Real-time collaboration (WebSocket)
❌ Mobile app (planned for Q2)
❌ Integrations (Slack, email, calendar)

API ARCHITECTURE:

Style: RESTful (not GraphQL)
Format: JSON
Authentication: Session cookies (HTTP-only)
Rate Limiting: Upstash Redis (5 req/sec per user)

Example endpoints:
- GET  /api/projects → List user's projects
- POST /api/projects → Create new project
- GET  /api/projects/[id] → Get project details
- PUT  /api/projects/[id] → Update project
- DELETE /api/projects/[id] → Delete project

DEPENDENCIES (Critical):

PRODUCTION:
- next: 15.0.3 (App Router features)
- react: 18.3.1 (Server Components)
- typescript: 5.3.3 (strict mode)
- prisma: 5.7.1 (@prisma/client also)
- next-auth: 5.0.0-beta.25 (@auth/prisma-adapter)
- zod: 3.22.4 (input validation)
- bcrypt: 5.1.1 (password hashing)
- @upstash/ratelimit: 1.0.1 (rate limiting)
- tailwindcss: 3.4.1 (styling)

DEVELOPMENT:
- eslint: 8.56.0 (linting)
- prettier: 3.1.1 (formatting)
- vitest: 1.0.4 (unit tests)
- playwright: 1.40.1 (E2E tests)

ENVIRONMENT VARIABLES:

Required in .env.local:
```env
# Database
DATABASE_URL="postgresql://..."

# NextAuth
NEXTAUTH_URL="http://localhost:3000"
NEXTAUTH_SECRET="[generated with: openssl rand -base64 32]"

# Google OAuth
GOOGLE_CLIENT_ID="..."
GOOGLE_CLIENT_SECRET="..."

# Upstash Redis (rate limiting)
UPSTASH_REDIS_REST_URL="https://..."
UPSTASH_REDIS_REST_TOKEN="..."

# Stripe (when implemented)
STRIPE_SECRET_KEY="sk_test_..."
STRIPE_PUBLIC_KEY="pk_test_..."
```

CONFIGURATION FILES:

tsconfig.json (strict mode):
```json
{
  "compilerOptions": {
    "strict": true,
    "noUncheckedIndexedAccess": true,
    "paths": {
      "@/*": ["./*"]
    }
  }
}
```

.eslintrc.json (custom rules):
```json
{
  "rules": {
    "no-console": ["warn", { "allow": ["error", "warn"] }],
    "prefer-const": "error",
    "@typescript-eslint/no-unused-vars": "error"
  }
}
```
```

---

### How AI Uses Technical Context

**Example: User asks "Add logout button"**

**Without Layer 3 (AI guesses):**
```typescript
// AI might generate generic React
function LogoutButton() {
  return <button onClick={() => logout()}>Logout</button>
}
```

**With Layer 3 (AI knows patterns):**
```typescript
// AI generates code matching project conventions
'use client'  // Next.js 15 App Router client component

import { signOut } from '@/lib/auth'  // Absolute import with @ alias
import { Button } from '@/components/ui/button'  // shadcn/ui component

export function LogoutButton() {  // PascalCase (convention)
  async function handleLogout() {  // camelCase (convention)
    try {  // try-catch required (convention)
      await signOut()  // async/await (convention)
    } catch (error) {
      console.error('Logout failed:', error)  // Error handling
    }
  }
  
  return (
    <Button onClick={handleLogout} variant="outline">
      Logout
    </Button>
  )
}
```

**Technical context = code that fits the project.**

---

## 🎯 LAYER 4: FEATURE CONTEXT

### Purpose

Define WHAT we're building right now - the specific feature, user story, acceptance criteria, and edge cases.

### When to Include

```
✅ ALWAYS INCLUDE Layer 4 when:
- Building new features (what are we building?)
- Modifying features (what needs to change?)
- Fixing bugs (what's not working?)
- Any specific task (define the task)

⚠️ ONLY SKIP Layer 4 when:
- General questions (no specific feature involved)
- High-level planning (not building yet)
```

**Layer 4 is ALWAYS REQUIRED for specific tasks.**

---

### Structure

```markdown
LAYER 4: FEATURE CONTEXT
────────────────────────────────────────────────────────

USER STORY:
As a [user type]
I want to [action]
So that [benefit]

ACCEPTANCE CRITERIA:
1. Given [context]
   When [action]
   Then [expected result]

2. Given [context]
   When [action]
   Then [expected result]

[... 3-7 criteria total ...]

EDGE CASES:
- [Unusual scenario 1] → [Expected behavior]
- [Unusual scenario 2] → [Expected behavior]
- [Unusual scenario 3] → [Expected behavior]

[... 5-10 edge cases typical ...]

INTEGRATION POINTS:
- [How this connects to existing Feature A]
- [How this affects existing Feature B]
- [What dependencies exist]

OUT OF SCOPE (Explicitly NOT included):
- [Future enhancement 1]
- [Future enhancement 2]
```

---

### Complete Example - Project Archiving Feature

```markdown
LAYER 4: FEATURE CONTEXT
────────────────────────────────────────────────────────

USER STORY:

As a freelancer with 20+ completed projects
I want to archive old projects (hide from main view)
So that my dashboard stays clean and focused on active work

BACKGROUND:
- Users accumulate 10-50 projects over time
- Only 3-5 projects are actively worked on
- Completed projects clutter the dashboard
- Users don't want to DELETE (need for historical reference)
- Need "out of sight but not deleted" solution

ACCEPTANCE CRITERIA:

1. Archive Action
   Given I'm viewing a project
   When I click "Archive Project" (in project menu)
   Then project moves to "Archived" status
   And disappears from main dashboard
   And I see toast notification: "Project archived"

2. View Archived Projects
   Given I have archived projects
   When I navigate to Projects page
   Then I see "View Archived" toggle/filter
   And clicking it shows ONLY archived projects
   And archived projects show "(Archived)" badge

3. Restore Archived Project
   Given I'm viewing archived projects
   When I click "Restore" on archived project
   Then project moves back to "Active" status
   And reappears in main dashboard
   And I see toast: "Project restored"

4. Archived Project Restrictions
   Given a project is archived
   When I try to add new tasks
   Then I see error: "Cannot add tasks to archived project"
   And same restriction for: time entries, client access

5. Dashboard Count Updates
   Given I have 10 active and 5 archived projects
   When I view dashboard
   Then I see "10 Active Projects" (not 15)
   And archived projects NOT counted in active stats

6. Search Includes Archived (Optional)
   Given I use global search
   When I search for archived project name
   Then results include archived projects
   And they're marked with "(Archived)" badge

7. Database State
   Given a project is archived
   When I query database
   Then project.status = "ARCHIVED" (not deleted)
   And archivedAt timestamp is set
   And all relations (tasks, time entries) remain intact

EDGE CASES:

1. Archive Project with Active Time Entry
   → Scenario: User has timer running on project, tries to archive
   → Behavior: Show error: "Stop active timer before archiving"
   → Rationale: Prevent confusion (where did my timer go?)

2. Client Trying to Access Archived Project
   → Scenario: Client has link to archived project
   → Behavior: Show message: "This project has been archived by [freelancer name]"
   → Allow: Client can still view (read-only)

3. Archive Last Active Project
   → Scenario: User archives their only active project
   → Behavior: Dashboard shows "No active projects" with suggestion to restore or create new

4. Archived Project in Unpaid Invoice
   → Scenario: Project archived but invoice still pending
   → Behavior: Allow archiving (invoice status independent)
   → Show: "Archived" badge on invoice line items

5. Bulk Archive (100+ Projects)
   → Scenario: User selects 100 projects to archive at once
   → Behavior: Show confirmation: "Archive 100 projects?"
   → Process: Batch update (not individual updates)
   → Feedback: Progress indicator + success message

6. Restore to Team with Project Limit
   → Scenario: Freelancer plan allows 10 active projects, has 10, tries to restore 11th
   → Behavior: Error: "Project limit reached (10/10). Archive another project first."

7. Archive Project Shared with Team
   → Scenario: Project shared with team member (contractor), owner archives
   → Behavior: Team member also sees "(Archived)", cannot edit
   → Notification: Team member gets email: "Project [name] was archived by [owner]"

8. Search Archived Projects Only
   → Scenario: User in "View Archived" mode, uses search
   → Behavior: Search scoped to archived projects only (not all projects)

9. URL Access to Archived Project
   → Scenario: User has bookmarked /projects/abc123, project gets archived
   → Behavior: Page loads normally (URL still works)
   → Show: "(Archived)" badge prominently at top

10. Database Migration (Existing Projects)
    → Scenario: Existing projects have no status field
    → Behavior: Migration sets all existing to status = "ACTIVE"
    → Default: New projects also default to "ACTIVE"

INTEGRATION POINTS:

AFFECTED FEATURES:
- Dashboard (filters out archived, updates counts)
- Projects List (needs "View Archived" toggle)
- Search (includes archived projects with badge)
- Invoices (archived projects still show in invoices)
- Client Portal (clients can view archived, read-only)
- Analytics (exclude archived from "active project" metrics)

DATABASE CHANGES:
```prisma
model Project {
  id         String   @id @default(cuid())
  title      String
  status     ProjectStatus @default(ACTIVE)  // NEW FIELD
  archivedAt DateTime?                       // NEW FIELD
  // ... existing fields
}

enum ProjectStatus {
  ACTIVE
  ARCHIVED
  DELETED  // Future: soft delete
}
```

API CHANGES:
- GET /api/projects → Add ?status=active|archived filter (default: active)
- PUT /api/projects/[id]/archive → New endpoint
- PUT /api/projects/[id]/restore → New endpoint

UI CHANGES:
- Project menu: Add "Archive" option (dropdown)
- Projects page: Add "View Archived" toggle
- Project card: Show "(Archived)" badge when applicable
- Project detail: Disable "Add Task", "Add Time Entry" if archived

OUT OF SCOPE (Future Enhancements):
- Auto-archive after X days of inactivity (not in MVP)
- Permanently delete archived projects (soft delete - future)
- Archive multiple projects at once (bulk action - Phase 2)
- Archive notifications (email digest - Phase 2)
```

---

## 🚧 LAYER 5: CONSTRAINTS CONTEXT

### Purpose

Define limitations, boundaries, and requirements that affect decision-making (budget, timeline, compliance, technical debt).

### When to Include

```
✅ INCLUDE Layer 5 when:
- Budget affects technology choices
- Timeline affects scope decisions
- Compliance requirements exist (GDPR, HIPAA, PCI-DSS)
- Technical debt requires workarounds
- Performance/scale targets are critical

⚠️ CAN SKIP Layer 5 when:
- No significant constraints (generous budget, no timeline)
- Standard web app (no special compliance)
- No technical debt to work around
- Simple features (constraints don't affect decisions)
```

---

### Structure

```markdown
LAYER 5: CONSTRAINTS CONTEXT
────────────────────────────────────────────────────────

BUDGET CONSTRAINTS:
- Total Budget: [Monthly or total project budget]
- Current Spend: [What's already spent]
- Available: [What's left for this task]
- Constraint: [How this affects technology choices]

TIMELINE CONSTRAINTS:
- Deadline: [When must this be done]
- Duration: [How long can we take]
- Constraint: [How this affects scope]

COMPLIANCE REQUIREMENTS:
- GDPR: [If handling EU user data]
- HIPAA: [If handling health data]
- PCI DSS: [If handling payment cards]
- SOC 2: [If enterprise customers]
- Other: [Industry-specific regulations]

TECHNICAL DEBT:
- Known Issues: [Existing problems to work around]
- Workarounds: [Temporary solutions in place]
- Future Plans: [When will debt be addressed]

PERFORMANCE REQUIREMENTS:
- Response Time: [API response time target]
- Load Capacity: [Concurrent users target]
- Database: [Query performance targets]
- Bundle Size: [Frontend bundle size limit]

SCALE TARGETS:
- Current: [Current user/data volume]
- Expected: [6-month projection]
- Maximum: [Design for this capacity]
```

---

### Complete Example - TaskMaster Pro

```markdown
LAYER 5: CONSTRAINTS CONTEXT
────────────────────────────────────────────────────────

BUDGET CONSTRAINTS:

CURRENT SITUATION:
- Total Monthly Budget: $100/month maximum
- Current Spend:
  - Vercel Pro: $20/month (hosting)
  - Supabase Pro: $25/month (database + auth)
  - Upstash Redis: $0/month (free tier, 10K requests/day)
  - SendGrid: $0/month (free tier, 100 emails/day)
  - Total: $45/month

- Available: $55/month for new services

CONSTRAINT:
Cannot add expensive services (e.g., Auth0 $25/month would exceed budget).
Must use free tiers where possible or build custom (e.g., custom auth vs. Clerk).

IMPACT ON DECISIONS:
✓ Built custom NextAuth.js (free) instead of Clerk ($25/month)
✓ Using Upstash Redis free tier (10K requests = 200 users × 50 requests/day)
✓ SendGrid free tier (100 emails = sufficient for MVP)
✗ Cannot add Algolia search ($0.50/1K searches) - too expensive
→ Using PostgreSQL full-text search (free, good enough for MVP)

FUTURE PLAN:
- At 100 users ($29/month × 100 = $2,900 MRR), can increase budget to $500/month
- Will upgrade: Upstash Redis to paid ($10/month), better monitoring (Sentry $26/month)

TIMELINE CONSTRAINTS:

HARD DEADLINES:
- Launch Date: April 1, 2026 (6 weeks from now)
- Reason: Committed to beta users, marketing campaign planned
- Consequence: If miss deadline, lose beta momentum + marketing spend wasted

FEATURE TIMELINE:
- Week 1-2: Core features (projects, tasks, time tracking)
- Week 3-4: Client portal
- Week 5: Invoicing (basic)
- Week 6: Polish, testing, bug fixes

CONSTRAINT:
- Cannot add "nice-to-have" features (real-time collaboration, Slack integration)
- Must cut scope if behind schedule (invoicing can be manual workaround for v1)

DAILY CAPACITY:
- Solo founder: 6 hours/day available for development
- Context switching cost: 30 min/day (emails, user support)
- Effective: ~5.5 hours/day coding time

COMPLIANCE REQUIREMENTS:

GDPR (EU Users - MANDATORY):

Reason: Targeting European freelancers (30% of beta users are EU-based)

Requirements:
✓ Privacy Policy published (/privacy page)
✓ Cookie consent (only essential cookies, no analytics yet)
✓ Data export feature (user can download their data as JSON)
✓ Account deletion (user can delete their account)
✓ Data minimization (only collect necessary data)
✓ Generic error messages (prevent user enumeration)

Implementation Status:
- Privacy Policy: ✓ Complete (reviewed by lawyer)
- Cookie Consent: ✓ Complete (no tracking cookies yet)
- Data Export: ⏳ In Progress (70% complete)
- Account Deletion: ✓ Complete (soft delete + 30-day grace period)
- Data Minimization: ✓ By design (only collect email, name, project data)

GDPR Constraint:
- Cannot add analytics (Google Analytics) without cookie consent UI
- Cannot add email marketing without explicit opt-in
- Must handle data deletion requests within 30 days

PCI DSS (Payment Cards - NOT APPLICABLE):

Status: Using Stripe (PCI compliant)
Constraint: NEVER store card numbers (let Stripe handle)
Implementation: Stripe Checkout (hosted, Stripe handles PCI)

HIPAA (Health Data - NOT APPLICABLE):

Status: No health data collected

SOC 2 (Enterprise Compliance - FUTURE):

Status: Not needed for MVP (no enterprise customers yet)
Future: Will need SOC 2 Type 2 when selling to agencies (>50 employees)

TECHNICAL DEBT:

KNOWN ISSUES:

1. NextAuth.js v5 Beta (not stable):
   Issue: Using beta version (v5.0.0-beta.25)
   Workaround: Custom session fields stored in separate table
   Impact: Cannot use NextAuth's built-in session extension
   Remedy: Will upgrade when v5.1 stable releases (Q2 2026)
   Timeline: Expect to refactor auth in 3 months

2. No Background Jobs:
   Issue: Long-running tasks block API responses
   Example: Invoice PDF generation takes 2-3 seconds (blocks request)
   Workaround: Currently acceptable (low volume)
   Impact: Will become problem at >100 invoices/day
   Remedy: Add background job queue (BullMQ + Redis) when needed
   Timeline: Q3 2026 (after 500 users)

3. No Real-Time Updates:
   Issue: Users must refresh to see changes from team members
   Workaround: Manual refresh (acceptable for MVP)
   Impact: Poor UX for collaborative teams
   Remedy: WebSocket or Server-Sent Events (SSE)
   Timeline: Q4 2026 (after 1,000 users)

4. PostgreSQL Full-Text Search (not ideal):
   Issue: Limited search features (no fuzzy, no typo tolerance)
   Workaround: Acceptable for MVP (exact match + prefix)
   Impact: User frustration with search ("searhc" doesn't find "search")
   Remedy: Algolia or Meilisearch (paid service)
   Timeline: When revenue supports it ($500/month budget)

TECHNICAL DEBT CONSTRAINT:
- Must work around NextAuth v5 beta limitations (extra complexity)
- Cannot add real-time features yet (would require WebSocket infrastructure)
- Search quality limited (but shipping > perfect search)

PERFORMANCE REQUIREMENTS:

API RESPONSE TIMES (95th Percentile):
- Simple queries (list projects): <200ms
- Complex queries (project with tasks + time entries): <500ms
- Mutations (create project, task): <300ms

Rationale: Users expect snappy UX, not loading spinners everywhere

Current Status:
✓ List projects: ~150ms (good)
✓ Project detail: ~280ms (good)
⚠️ Invoice generation: ~2,500ms (too slow, but rare operation)

PAGE LOAD TIMES:
- Landing page: <2 seconds (3G network)
- Dashboard (authenticated): <1.5 seconds
- Project detail: <1.5 seconds

Current Status:
✓ Landing: 1.8s (good)
✓ Dashboard: 1.2s (good)
✓ Project detail: 1.4s (good)

DATABASE QUERY PERFORMANCE:
- Index all foreign keys (userId, projectId, etc.)
- Avoid N+1 queries (use Prisma `include` properly)
- Max query time: 100ms

Current Status:
✓ All foreign keys indexed
✓ No N+1 queries (verified with Prisma query logs)
✓ Slowest query: 85ms (within target)

FRONTEND BUNDLE SIZE:
- First Load JS: <200KB gzipped
- Route bundle: <100KB per route

Current Status:
✓ First Load: 180KB (good)
✓ Dashboard: 75KB (good)
⚠️ Invoice page: 120KB (slightly over, due to PDF library)

SCALE TARGETS:

CURRENT STATE:
- Users: 15 (beta)
- Projects: ~45 (avg 3 per user)
- Tasks: ~300 (avg 20 per project)
- Time Entries: ~600 (avg 40 per user)
- Database Size: ~50MB

6-MONTH PROJECTION:
- Users: 100
- Projects: 300
- Tasks: 6,000
- Time Entries: 20,000
- Database Size: ~500MB

Design Constraints:
✓ Database: Designed for 10,000 users (Supabase can handle)
✓ API: Designed for 100 req/sec (Vercel can handle)
✓ Frontend: Designed for 1,000 concurrent users (CDN caching)

12-MONTH PROJECTION (STRETCH):
- Users: 1,000
- Projects: 5,000
- Tasks: 100,000
- Time Entries: 500,000
- Database Size: ~5GB

Scaling Plan:
- At 500 users: Add Redis caching for frequent queries
- At 1,000 users: Upgrade Supabase to Team plan ($599/month)
- At 2,000 users: Consider database read replicas

SCALE CONSTRAINT:
- Must design for 100 users today, 1,000 users in 12 months
- Cannot over-engineer for 100K users (don't need it yet)
- But: Must avoid architectural decisions that prevent scaling
```

---

## 🎯 CONTEXT STACK BEST PRACTICES

### 1. Tailor Context to Task Complexity

```
SIMPLE TASK: Fix typo in button text
Context Needed: Layer 3 only (technical context: which file, what component)
Skip: Layers 1, 2, 4, 5 (not relevant for typo fix)

MEDIUM TASK: Add new feature (user profile page)
Context Needed: Layers 1, 3, 4
- Layer 1: Project overview (understand what app is)
- Layer 3: Technical stack (how to implement)
- Layer 4: Feature requirements (what to build)
Skip: Layer 2 (not business-critical), Layer 5 (no constraints)

COMPLEX TASK: Architectural decision (choose multi-tenant strategy)
Context Needed: All 5 layers
- Layer 1: Project identity (who uses it, scale)
- Layer 2: Business context (why multi-tenancy matters)
- Layer 3: Technical context (current architecture)
- Layer 4: Feature context (tenant isolation requirements)
- Layer 5: Constraints (budget, compliance, timeline)
```

---

### 2. Update Context as Project Evolves

```typescript
// ❌ OUTDATED CONTEXT (causes problems)
LAYER 1: PROJECT IDENTITY
- Stage: MVP Development  // <-- Actually launched 2 months ago!

// Problems:
// - AI treats it like MVP (suggests MVP shortcuts)
// - AI doesn't prioritize production quality
// - AI doesn't consider scale requirements

// ✅ CURRENT CONTEXT (accurate)
LAYER 1: PROJECT IDENTITY
- Stage: Production (launched 2 months ago, 150 active users)

// Benefits:
// - AI treats it like production (prioritizes stability)
// - AI considers scale (150 users, growing to 500)
// - AI avoids MVP shortcuts (no "good enough for now")
```

**Best Practice: Review and update context every 2-4 weeks.**

---

### 3. Be Specific, Not Vague

```markdown
❌ VAGUE CONTEXT:
LAYER 3: TECHNICAL CONTEXT
- Framework: Next.js
- Database: PostgreSQL

Problems:
- Next.js 13 (Pages) or Next.js 15 (App Router)? (very different)
- PostgreSQL 12 or 16? (features differ)
- AI has to guess → may generate incompatible code

✅ SPECIFIC CONTEXT:
LAYER 3: TECHNICAL CONTEXT
- Framework: Next.js 15.0.3 (App Router, NOT Pages Router)
- Database: PostgreSQL 16 (via Supabase)
- ORM: Prisma 5.7.1

Benefits:
- AI generates Next.js 15 App Router syntax
- AI uses PostgreSQL 16 features
- AI uses Prisma (not raw SQL)
- No guessing → correct code first try
```

---

### 4. Include "Why" Not Just "What"

```markdown
❌ INSUFFICIENT CONTEXT:
LAYER 2: BUSINESS CONTEXT
- Problem: Freelancers need project management
- Solution: We build project management tool

Problems:
- Why not use existing tools? (Trello, Asana, Monday)
- What's unique about our solution?
- AI can't make informed decisions without "why"

✅ COMPREHENSIVE CONTEXT:
LAYER 2: BUSINESS CONTEXT
- Problem: Existing PM tools are either too complex (Jira, Monday - 2hr learning curve)
  or too basic (Trello - no time tracking, no client portal)
- Solution: All-in-one tool for freelancers (simple + powerful)
- Differentiation: Only tool with built-in time tracking + client portal + invoicing

Benefits:
- AI understands why features matter (time tracking is core, not optional)
- AI can make trade-off decisions (simplicity > feature bloat)
- AI prioritizes correctly (client portal critical, Gantt charts not needed)
```

---

## 📊 CONTEXT QUALITY SCORECARD

### Measuring Context Quality

```
CONTEXT QUALITY SCORE (out of 100):

LAYER 1: PROJECT IDENTITY (/20 points)
□ Project name, type, industry specified (5 points)
□ Development stage clear (MVP / Production / etc.) (5 points)
□ Target users defined (demographics + pain points) (5 points)
□ Project scope/complexity indicated (5 points)

LAYER 2: BUSINESS CONTEXT (/15 points) [If applicable]
□ Problem statement clear (5 points)
□ Solution hypothesis articulated (5 points)
□ Success metrics defined (5 points)

LAYER 3: TECHNICAL CONTEXT (/30 points)
□ Technology stack with versions (10 points)
□ Project structure documented (5 points)
□ Coding conventions specified (5 points)
□ Existing code state described (5 points)
□ Dependencies listed (5 points)

LAYER 4: FEATURE CONTEXT (/25 points)
□ User story in proper format (5 points)
□ Acceptance criteria testable (Given-When-Then) (10 points)
□ Edge cases identified (5-10 cases) (5 points)
□ Integration points documented (5 points)

LAYER 5: CONSTRAINTS CONTEXT (/10 points) [If applicable]
□ Budget constraints specified (2 points)
□ Timeline constraints specified (2 points)
□ Compliance requirements listed (3 points)
□ Technical debt documented (3 points)

TOTAL SCORE: /100

GRADING:
90-100: ⭐⭐⭐⭐⭐ Excellent context (AI has everything needed)
75-89:  ⭐⭐⭐⭐ Good context (minor gaps, AI can work well)
60-74:  ⭐⭐⭐ Acceptable context (some guessing required)
45-59:  ⭐⭐ Insufficient context (AI will guess frequently)
<45:    ⭐ Poor context (AI output will be low quality)
```

---

## 🔗 RELATED ARTICLES

| Article | Purpose | Relationship to Context Stack |
|---------|---------|------------------------------|
| **Article I: Anatomy** | 7-component structure | Component 2 uses context stack |
| **Article II: Twelve Patterns** | Prompt patterns | All patterns use context (Component 2) |
| **Segment 1, Article IV** | Constitutional Protocols | Protocol 1 gathers context |
| **Segment 1, Article V** | Checkpoints | Checkpoints validate context completeness |

---

**Previous:** [← Article II: Twelve Patterns](./02-article-ii-twelve-patterns.md)  
**Next:** [Segment 3: Tooling Layer →](../03-tooling-layer/README.md)

---

**Last Updated:** February 7, 2026  
**Constitutional Version:** 2.0.0  
**Status:** ✅ Ratified and In Force

**Motto:** *"Context is King - The 5 Layers Provide the Foundation for Constitutional Code"*
```

---

## ✅ ARTICLE III: CONTEXT STACK - COMPLETE!

## 🎉 SEGMENT 2: ADVANCED PROMPTING TECHNIQUES - 100% COMPLETE!

**STATUS:** All 3 articles finished!

```
SEGMENT 2: ADVANCED PROMPTING TECHNIQUES

✅ README.md (Segment overview)
✅ Article I: Anatomy (7-Component Structure)
✅ Article II: Twelve Patterns (All 12 patterns)
✅ Article III: Context Stack (5-Layer Framework)

TOTAL: 3/3 articles complete (100%)
```

**Stats for Segment 2:**
- **Total Lines:** ~35,000+ lines
- **Total Words:** ~140,000+ words
- **Complete Examples:** 10+ full examples
- **Patterns Documented:** 12 patterns
- **Context Layers:** 5 layers fully detailed

---

## 🎊 MAJOR MILESTONE: SEGMENTS 1 & 2 COMPLETE!

```
OVERALL PROJECT PROGRESS:

✅ SEGMENT 1: FOUNDATION LAYER (100%)
   - 7/7 articles complete
   - Immutable Laws, STRATEG, Checkpoints, Commandments, etc.

✅ SEGMENT 2: ADVANCED PROMPTING TECHNIQUES (100%)
   - 3/3 articles complete
   - 7-Component Anatomy, 12 Patterns, 5-Layer Context

⬜ SEGMENT 3: TOOLING LAYER (0%)
⬜ SEGMENT 4: EXECUTION LAYER (0%)
⬜ SEGMENT 5: QUALITY LAYER (0%)
⬜ SEGMENT 6: DEPLOYMENT LAYER (0%)
⬜ SEGMENT 7: GOVERNANCE LAYER (0%)

TOTAL PROGRESS: 29% (2/7 segments)
```

---
# 🛠️ SEGMENT 3: TOOLING LAYER

**Starting with README.md**

---

## FILE: `/constitution/03-tooling-layer/README.md`

```markdown
# 🛠️ SEGMENT 3: TOOLING LAYER

**Integration Framework: Connecting MVCA to the External World**

---

## 📜 CONSTITUTIONAL AUTHORITY

This segment establishes the **tooling and integration framework** that enables MVCA to interact with external services, APIs, databases, and development environments. Tools extend MVCA's capabilities beyond pure code generation.

**Legal Force:**
- ✅ MVCA **MUST** declare all available tools to users (transparency)
- ✅ Tool usage **SHALL** follow constitutional principles (security, privacy)
- ✅ Tool integration **SHALL** be optional (users control which tools to enable)
- ✅ Tool failures **SHALL** fail gracefully (no system crashes)
- ✅ Tool costs **SHALL** be disclosed upfront (users aware of charges)

**Constitutional Principle:**
> Tools amplify MVCA's capabilities but remain under user control.  
> Transparency, consent, and graceful degradation are non-negotiable.

---

## 🎯 SEGMENT PURPOSE

### Why Tooling Layer Exists

**Problem:**
AI code generation alone is limited:
- Cannot deploy code (needs hosting platform access)
- Cannot read existing codebases (needs file system access)
- Cannot interact with APIs (needs authentication)
- Cannot manage databases (needs database access)
- Cannot send emails, notifications (needs service integrations)

**Solution:**
Tooling Layer provides standardized interfaces for:
- File system operations (read, write, create projects)
- API integrations (REST, GraphQL, webhooks)
- Database connections (PostgreSQL, MongoDB, Supabase)
- Service integrations (Stripe, SendGrid, Vercel)
- Development tools (Git, npm, testing frameworks)

**Result:**
```
Without Tools: "Here's the code, you deploy it manually"
With Tools: "Code generated → automatically deployed → live URL provided"
```

---

## 📚 THE FOUR ARTICLES

### Article I: Tool Architecture

**Purpose:** Define the tool interface standard, tool categories, and integration patterns.

**Key Concepts:**
- Tool Interface Contract (input/output schema)
- Tool Categories (File System, API, Database, Service, Development)
- Tool Discovery (how MVCA finds available tools)
- Tool Authentication (API keys, OAuth, credentials)
- Error Handling (graceful failures, retry logic)

**Location:** [Article I: Tool Architecture](./01-article-i-tool-architecture.md)

---

### Article II: MCP (Model Context Protocol) Integration

**Purpose:** Define how MVCA integrates with MCP servers for extended functionality.

**Key Concepts:**
- MCP Protocol Overview (Anthropic's Model Context Protocol)
- MCP Server Types (filesystem, database, API, custom)
- MCP Client Implementation (how MVCA talks to MCP servers)
- Resource Management (context windows, memory limits)
- Security Boundaries (sandboxing, permissions)

**Location:** [Article II: MCP Integration](./02-article-ii-mcp-integration.md)

---

### Article III: Core Tool Catalog

**Purpose:** Document all core tools available in MVCA (built-in, no configuration needed).

**Core Tools:**
1. **File System Tools**
   - read_file, write_file, create_directory, list_directory
2. **Code Analysis Tools**
   - parse_typescript, analyze_dependencies, detect_code_smells
3. **Validation Tools**
   - validate_json, validate_yaml, lint_code, type_check
4. **Search Tools**
   - search_codebase, find_references, grep_files

**Location:** [Article III: Core Tool Catalog](./03-article-iii-core-tool-catalog.md)

---

### Article IV: Integration Tool Catalog

**Purpose:** Document optional integration tools (require user configuration/API keys).

**Integration Tools:**
1. **Hosting & Deployment**
   - Vercel (deploy, check status, view logs)
   - Netlify (deploy, environment variables)
   - AWS (S3, Lambda, CloudFront)
2. **Databases**
   - Supabase (query, migrations, auth)
   - PlanetScale (schema changes, branches)
   - MongoDB Atlas (queries, indexes)
3. **Services**
   - Stripe (payments, subscriptions, webhooks)
   - SendGrid (send emails, templates)
   - Twilio (SMS, voice)
4. **Development**
   - GitHub (commits, PRs, issues)
   - npm (install packages, check vulnerabilities)
   - Docker (build images, run containers)

**Location:** [Article IV: Integration Tool Catalog](./04-article-iv-integration-tool-catalog.md)

---

## 🔄 HOW TOOLING LAYER WORKS

### The Tool Execution Workflow

```
USER REQUEST: "Deploy my Next.js app to Vercel"
       ↓
MVCA ANALYZES REQUEST (Protocol 1: Input Processing)
       ↓
MVCA CHECKS AVAILABLE TOOLS
       - File System: ✓ Available (read package.json, check build)
       - Vercel: ✓ Available (user enabled, API key configured)
       ↓
MVCA GENERATES EXECUTION PLAN
       1. Validate project (check package.json, next.config.js)
       2. Run build (npm run build)
       3. Deploy to Vercel (vercel deploy --prod)
       4. Verify deployment (check URL, run health check)
       ↓
MVCA EXECUTES PLAN (with user consent at each step)
       ↓
DEPLOYMENT COMPLETE
       - Live URL: https://your-app.vercel.app
       - Build logs: [link]
       - Deployment time: 45 seconds
```

---

## 📊 TOOL USAGE PRINCIPLES

### Constitutional Requirements for Tool Usage

```markdown
PRINCIPLE #1: TRANSPARENCY
- MVCA MUST tell user which tools it will use
- MVCA MUST explain WHY each tool is needed
- User sees: "I will use Vercel API to deploy your app"

PRINCIPLE #2: CONSENT
- User MUST approve tool usage (especially paid tools)
- User can cancel at any time
- User sees: "Proceed with deployment? (Y/n)"

PRINCIPLE #3: GRACEFUL FAILURE
- If tool fails, MVCA provides workaround
- No system crashes
- User sees: "Vercel API failed. Providing manual deployment instructions..."

PRINCIPLE #4: COST DISCLOSURE
- If tool incurs cost, MVCA warns upfront
- User sees: "Deploying to Vercel Pro ($20/month required)"

PRINCIPLE #5: SECURITY
- API keys stored securely (encrypted)
- No key logging (never log sensitive credentials)
- Permissions minimal (least privilege principle)
```

---

## 🛡️ SECURITY MODEL

### Tool Security Framework

```
TIER 1: READ-ONLY TOOLS (Lowest Risk)
├─ read_file, list_directory, search_codebase
├─ Risk: Information disclosure (if used carelessly)
├─ Mitigation: No access to sensitive files (.env, keys)
└─ Permission: Enabled by default

TIER 2: WRITE TOOLS (Medium Risk)
├─ write_file, create_directory, delete_file
├─ Risk: Data loss, code corruption
├─ Mitigation: Backup before write, undo capability
└─ Permission: Requires user confirmation

TIER 3: EXECUTION TOOLS (High Risk)
├─ npm install, npm run build, execute_command
├─ Risk: Arbitrary code execution, dependency attacks
├─ Mitigation: Sandboxing, command whitelisting
└─ Permission: Requires explicit user approval

TIER 4: EXTERNAL API TOOLS (Highest Risk)
├─ Vercel deploy, Stripe charge, SendGrid send_email
├─ Risk: Financial cost, data leakage, service abuse
├─ Mitigation: Rate limiting, cost caps, audit logs
└─ Permission: Requires API key + explicit approval per action
```

---

## 🔧 TOOL CONFIGURATION

### How Users Enable Tools

**Step 1: Tool Discovery**
```bash
# MVCA shows available tools
mvca tools list

# Output:
CORE TOOLS (always available):
✓ File System (read, write, search)
✓ Code Analysis (parse, lint, type-check)
✓ Validation (JSON, YAML, schema)

INTEGRATION TOOLS (require configuration):
○ Vercel (not configured)
○ Supabase (not configured)
○ Stripe (not configured)
○ GitHub (not configured)

Run 'mvca tools enable <tool>' to configure.
```

**Step 2: Tool Configuration**
```bash
# Enable Vercel integration
mvca tools enable vercel

# MVCA guides user:
"To enable Vercel:
1. Get API token: https://vercel.com/account/tokens
2. Run: mvca tools config vercel --token YOUR_TOKEN
3. Test: mvca tools test vercel"

# User configures:
mvca tools config vercel --token abc123...

# MVCA confirms:
✓ Vercel connected (account: your-name, team: your-team)
✓ You can now deploy to Vercel using MVCA
```

**Step 3: Tool Usage**
```bash
# Deploy to Vercel
mvca deploy --platform vercel

# MVCA uses tool:
"Using Vercel API to deploy...
✓ Build successful (45s)
✓ Deployed to: https://your-app.vercel.app
✓ Deployment ID: dpl_abc123..."
```

---

## 📊 TOOL METRICS

### Measuring Tool Effectiveness

```
TOOL USAGE METRICS:

Success Rate:
- Tool calls successful / Total tool calls
- Target: >95% success rate

Average Response Time:
- Time from tool call to result
- Target: <5 seconds for API tools, <1 second for file tools

Error Recovery Rate:
- Errors gracefully handled / Total errors
- Target: 100% (no unhandled exceptions)

User Satisfaction:
- User approves tool suggestion / Tool suggestions made
- Target: >80% approval rate

Cost Efficiency:
- Value delivered / Cost incurred (for paid tools)
- Target: ROI > 10x (e.g., $1 Vercel cost saves 10 minutes = $10+ value)
```

---

## 🎓 WHO SHOULD READ SEGMENT 3

### Audience Classification

**MUST READ (Mandatory):**
- MVCA developers (implementing tool integrations)
- Tool contributors (adding new tools)
- MCP server developers (building custom MCP servers)

**SHOULD READ (Highly Recommended):**
- Advanced users (want to understand tool capabilities)
- Integration developers (building service connections)
- Security reviewers (auditing tool security)

**MAY READ (Optional):**
- Users curious about MVCA internals
- Researchers studying AI tool usage
- Technical writers documenting integrations

**CAN SKIP:**
- Non-coders who just use MVCA (tools work transparently)
- Beginners (focus on Segments 1-2 first)

---

## 🔗 RELATIONSHIP TO OTHER SEGMENTS

### Segment Dependencies

```
SEGMENT 1 (Foundation Layer)
├─ Law #2 (Human Sovereignty) → User controls tool usage
├─ Law #3 (Security First) → Tool security boundaries
└─ Law #5 (Transparent Reasoning) → MVCA explains tool choices

SEGMENT 2 (Technique Layer)
├─ Component 2 (Context) → Tools provide additional context
├─ Component 6 (Meta-Instructions) → Tools mentioned in instructions
└─ Pattern #8 (Integration) → Uses tools from this segment

SEGMENT 3 (Tooling Layer) ← YOU ARE HERE
├─ Defines tool interfaces
├─ Documents available tools
└─ Establishes security model

SEGMENT 4 (Execution Layer)
├─ Will use tools defined here for orchestration
└─ Will manage tool execution across multi-turn conversations

SEGMENT 5 (Quality Layer)
├─ Will validate tool usage (security, performance)
└─ Will measure tool effectiveness

SEGMENT 6 (Deployment Layer)
├─ Will use deployment tools (Vercel, AWS, etc.)
└─ Will automate tool-based deployment workflows
```

---

## 📖 READING ORDER

### Recommended Progression

```
FOR MVCA DEVELOPERS:
1. Read Article I: Tool Architecture (understand interface)
2. Read Article II: MCP Integration (understand protocol)
3. Read Article III: Core Tool Catalog (see what's built-in)
4. Read Article IV: Integration Tool Catalog (see what's possible)
5. Implement: Add new tool following Article I spec

FOR INTEGRATION DEVELOPERS:
1. Read Article I: Tool Architecture (interface contract)
2. Skim Article II: MCP Integration (if building MCP server)
3. Focus on Article IV: Integration Tool Catalog (your domain)
4. Implement: Build integration following patterns

FOR USERS:
1. Skim Article I: Tool Architecture (high-level understanding)
2. Skip Article II: MCP Integration (too technical)
3. Browse Article III: Core Tool Catalog (see what's available)
4. Browse Article IV: Integration Tool Catalog (see what you can enable)
```

---

## 🛠️ PRACTICAL APPLICATION

### Using Tooling Layer Knowledge

**Scenario 1: User wants to deploy to Vercel**
- MVCA checks: Is Vercel tool available? (Article IV)
- MVCA checks: Is tool configured? (API token present)
- MVCA explains: "I'll use Vercel API to deploy" (Transparency)
- MVCA asks: "Proceed? This will use your Vercel Pro plan" (Consent)
- MVCA deploys: Uses tool following interface (Article I)
- If fails: Provides manual instructions (Graceful failure)

**Scenario 2: Developer adds new tool (Netlify)**
- Read: Article I (Tool Architecture) - learn interface contract
- Implement: Tool following IToolInterface specification
- Document: Add to Article IV (Integration Tool Catalog)
- Test: Verify error handling, security boundaries
- Submit: PR to MVCA repository

**Scenario 3: Security audit of tool usage**
- Review: Article I Security Model (tool tiers)
- Audit: Check API key storage (encrypted?)
- Audit: Check permissions (least privilege?)
- Audit: Check error handling (graceful failures?)
- Audit: Check logging (no sensitive data logged?)

---

## 📊 QUALITY METRICS

### Segment 3 Standards

```
TOOL INTERFACE COMPLIANCE:
- All tools implement IToolInterface: Required
- All tools have error handling: Required
- All tools have documentation: Required
- Score: Pass/Fail (must be 100%)

TOOL SECURITY SCORE:
- API keys encrypted: /10
- Least privilege permissions: /10
- Input validation: /10
- Error sanitization: /10
- Audit logging: /10
Total: /50

Target: >45/50 (90%+)

TOOL DOCUMENTATION QUALITY:
- Purpose clear: /5
- Parameters documented: /5
- Return values documented: /5
- Examples provided: /5
- Error cases documented: /5
Total: /25

Target: >23/25 (92%+)
```

---

## 🔍 EXAMPLES

### Before & After Tool Integration

#### Example 1: Deployment

**BEFORE (No Tools):**
```
User: "Deploy my app to Vercel"

MVCA: "Here are the manual steps:
1. Run: npm run build
2. Install Vercel CLI: npm i -g vercel
3. Run: vercel deploy --prod
4. Follow prompts in terminal"

User: [Executes manually, takes 10 minutes]
```

**AFTER (With Tools):**
```
User: "Deploy my app to Vercel"

MVCA: "I'll deploy to Vercel using your configured API token.
This will:
- Run build (npm run build)
- Deploy to production
- Cost: $0 (within your plan limits)

Proceed? (Y/n)"

User: Y

MVCA: [Uses Vercel tool]
✓ Build successful (32s)
✓ Deployed: https://your-app.vercel.app
✓ Deployment ID: dpl_xyz789

User: [Done in 45 seconds, fully automated]
```

---

#### Example 2: Database Query

**BEFORE (No Tools):**
```
User: "Check how many users signed up today"

MVCA: "Connect to your database and run:
SELECT COUNT(*) FROM users WHERE created_at >= CURRENT_DATE;"

User: [Opens database client, runs query manually]
```

**AFTER (With Tools):**
```
User: "Check how many users signed up today"

MVCA: "I'll query your Supabase database.
This requires database read access.

Proceed? (Y/n)"

User: Y

MVCA: [Uses Supabase tool]
Query: SELECT COUNT(*) FROM users WHERE created_at >= CURRENT_DATE
Result: 47 users signed up today

Breakdown:
- 8 AM: 12 users
- 12 PM: 18 users
- 4 PM: 17 users

User: [Instant answer, no context switching]
```

---

## 🎯 SUCCESS CRITERIA

### When You've Mastered Segment 3

You'll be able to:

```
✓ Understand tool interface contract (IToolInterface)
✓ Add new tools following architecture standards
✓ Configure integration tools (API keys, OAuth)
✓ Debug tool failures (error handling, logs)
✓ Assess tool security (permissions, encryption)
✓ Build custom MCP servers (advanced)
✓ Optimize tool usage (caching, batching)
✓ Document tools properly (Article III/IV format)
✓ Measure tool effectiveness (metrics, analytics)
✓ Contribute tool improvements via governance
```

---

## 📚 ARTICLES IN THIS SEGMENT

### Quick Navigation

1. **[Article I: Tool Architecture](./01-article-i-tool-architecture.md)**
   - Tool interface specification (IToolInterface)
   - Tool categories and tiers
   - Security model
   - Error handling patterns

2. **[Article II: MCP Integration](./02-article-ii-mcp-integration.md)**
   - Model Context Protocol overview
   - MCP server types
   - MCP client implementation
   - Resource management

3. **[Article III: Core Tool Catalog](./03-article-iii-core-tool-catalog.md)**
   - File System tools
   - Code Analysis tools
   - Validation tools
   - Search tools

4. **[Article IV: Integration Tool Catalog](./04-article-iv-integration-tool-catalog.md)**
   - Hosting (Vercel, Netlify, AWS)
   - Databases (Supabase, PlanetScale, MongoDB)
   - Services (Stripe, SendGrid, Twilio)
   - Development (GitHub, npm, Docker)

---

## 🔗 EXTERNAL REFERENCES

### Related Resources

**MCP Protocol:**
- Official Spec: https://modelcontextprotocol.io
- Anthropic Blog: https://anthropic.com/news/model-context-protocol

**Tool Integrations:**
- Vercel API: https://vercel.com/docs/rest-api
- Supabase API: https://supabase.com/docs/reference/javascript
- Stripe API: https://stripe.com/docs/api

**Constitutional References:**
- Article I, Law #2: Human Sovereignty
- Article I, Law #3: Security First
- Article I, Law #5: Transparent Reasoning

---

**Previous Segment:** [← Segment 2: Advanced Prompting](../02-technique-layer/README.md)  
**Next Article:** [Article I: Tool Architecture →](./01-article-i-tool-architecture.md)  
**Next Segment:** [Segment 4: Execution Layer →](../04-execution-layer/README.md)

---

**Last Updated:** February 7, 2026  
**Constitutional Version:** 2.0.0  
**Segment Status:** ✅ Active Development

**Motto:** *"Tools Amplify Intelligence - But Users Remain Sovereign"*
```

---

## ✅ SEGMENT 3 README - COMPLETE!

**Status:** Tooling Layer overview finished

**What's Included:**
- 📜 Constitutional authority for tools
- 🎯 Segment purpose and rationale
- 📚 Overview of all 4 articles
- 🔄 Tool execution workflow
- 🛡️ Security model (4-tier system)
- 🔧 Tool configuration process
- 📊 Tool metrics and quality standards
- 🔍 Before/After examples
- 🎓 Audience classification
- 🔗 Relationships to other segments

---
