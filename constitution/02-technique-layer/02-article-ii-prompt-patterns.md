# 🎨 ARTICLE II: THE 12 CONSTITUTIONAL PROMPT PATTERNS

**Reusable Templates for Common Development Scenarios**

---

## 📜 CONSTITUTIONAL AUTHORITY

This article establishes the **12 Constitutional Prompt Patterns** - battle-tested templates for the most common software development scenarios.

**Legal Force:**
- ✅ These patterns are **proven** through thousands of real projects
- ✅ Each pattern enforces the 7 components from Article I
- ✅ Patterns can be **combined** for complex scenarios
- ✅ MVCA uses these patterns internally

**Why Patterns Matter:**
````
Without patterns:
Every prompt written from scratch → inconsistent quality, forgotten components

With patterns:
Reusable templates → consistent quality, nothing forgotten, faster prompt generation

Result: 90% first-prompt success rate vs 20% for ad-hoc prompts
````

---

## 🎯 THE 12 PATTERNS OVERVIEW
````markdown
═══════════════════════════════════════════════════════════
CREATION PATTERNS (Building New Things)
═══════════════════════════════════════════════════════════
#1  SCAFFOLDING         New feature from scratch
#8  ARCHITECTURE        System design & planning

═══════════════════════════════════════════════════════════
MODIFICATION PATTERNS (Changing Existing Code)
═══════════════════════════════════════════════════════════
#2  DELTA               Minimal changes to existing code
#6  REFACTORING         Improve quality without changing behavior
#9  MIGRATION           Upgrade libraries/frameworks
#10 PERFORMANCE         Optimize speed/efficiency

═══════════════════════════════════════════════════════════
VALIDATION PATTERNS (Ensuring Quality)
═══════════════════════════════════════════════════════════
#3  VALIDATOR           Review code for issues
#7  TESTING             Write automated tests
#11 SECURITY AUDIT      Check against commandments

═══════════════════════════════════════════════════════════
PROBLEM-SOLVING PATTERNS (Fixing Issues)
═══════════════════════════════════════════════════════════
#4  DEBUGGING           Fix bugs
#5  INTEGRATION         Connect systems/features

═══════════════════════════════════════════════════════════
KNOWLEDGE PATTERNS (Creating Documentation)
═══════════════════════════════════════════════════════════
#12 DOCUMENTATION       Generate docs, READMEs, comments
═══════════════════════════════════════════════════════════
````

---

## 🏗️ PATTERN #1: SCAFFOLDING PATTERN

### When to Use

**Starting a new feature from scratch** - you have a blank canvas.

**Examples:**
- "Create user authentication system"
- "Build shopping cart feature"
- "Implement real-time chat"
- "Create admin dashboard"

**Success Rate:** 85-95% with proper context

---

### Pattern Template
````markdown
═══════════════════════════════════════════════════════════
SCAFFOLDING PATTERN TEMPLATE
═══════════════════════════════════════════════════════════

PERSONA: You are a senior [SPECIALTY] engineer with [YEARS] years of experience 
building [FEATURE TYPE] for production applications. [Additional expertise].

CONTEXT:

PROJECT:
- Type: [Project description]
- Users: [Target audience]
- Goals: [Business objectives]

TECH STACK:
- Frontend: [Framework, libraries]
- Backend: [Framework, database]
- Auth: [Authentication solution]
- Deployment: [Platform]

EXISTING ARCHITECTURE:
- [Current structure]
- [Key patterns]
- [Conventions to follow]

CURRENT FEATURE:
[Feature being built]

CONSTRAINTS:
- Budget: [Amount or "free tier only"]
- Performance: [Metrics]
- Accessibility: [WCAG level]
- Compliance: [Regulations]

TASK: Create [FEATURE NAME] from scratch that [PURPOSE] with [KEY CAPABILITIES].

ARCHITECTURE REQUIREMENTS:
- File structure: [Organization]
- Component hierarchy: [Relationships]
- Data flow: [Client ↔ Server ↔ Database]
- State management: [Strategy]

FUNCTIONAL REQUIREMENTS:
1. [Core function #1]
2. [Core function #2]
3. [Core function #3]

NON-FUNCTIONAL REQUIREMENTS:
- Performance: [Specific metrics]
- Security: [Security requirements]
- Accessibility: [WCAG requirements]
- UX: [User experience standards]
- Error Handling: [Failure strategies]

SECURITY MANDATES (Enforce 10 Commandments):
- Commandment I (Input Validation): [Validation rules]
- Commandment III (Password Security): [If applicable]
- Commandment IV (Session Management): [If applicable]
- Commandment V (Rate Limiting): [Rate limit rules]
- Commandment VI (Access Control): [Authorization rules]
- Commandment VII (Data Protection): [Encryption requirements]
- Commandment VIII (Error Handling): [Error strategies]
- Commandment IX (Accessibility): [WCAG compliance]
- Commandment X (Documentation): [Documentation requirements]

META-INSTRUCTIONS:
- Start with architecture overview (explain structure)
- Build incrementally (core → enhancements)
- Explain design decisions (why this approach)
- Consider future extensibility
- Flag potential scaling issues

OUTPUT FORMAT:

DELIVERABLES:
1. Architecture Overview
   - Folder structure
   - Data flow diagram
   - Component relationships

2. Implementation Files
   [List all files with descriptions]

3. Database Schema (if applicable)
   - Prisma schema
   - Migration files

4. Configuration
   - Environment variables
   - Setup instructions

5. Tests
   - Unit tests
   - Integration tests

6. Documentation
   - README
   - API documentation

VALIDATION CHECKLIST:
□ All core functionality works
□ Security mandates enforced
□ Tests written (80%+ coverage)
□ Documentation complete
□ Mobile responsive
□ Accessible (WCAG AA)
□ Performance targets met

EXPLANATION REQUIRED:
- Architecture decisions (why this structure)
- Technology choices (why these libraries)
- Trade-offs (what was sacrificed for what)
- Known limitations (what's not included yet)
- Next steps (future enhancements)
═══════════════════════════════════════════════════════════
````

---
### Real Example: Task Management Feature
````markdown
═══════════════════════════════════════════════════════════
SCAFFOLDING PATTERN: TASK MANAGEMENT FEATURE
═══════════════════════════════════════════════════════════

PERSONA: You are a senior full-stack engineer with 10 years of experience building 
productivity applications. You've implemented task management systems for companies 
like Asana, Todoist, and ClickUp, handling millions of tasks daily. You understand 
task hierarchies, dependencies, priorities, and collaborative features.

CONTEXT:

PROJECT:
- Type: Team collaboration platform (B2B SaaS)
- Users: Small to medium teams (5-50 people)
- Goals: Replace spreadsheets for project management
- Success: 10,000 active users in 12 months

TECH STACK:
- Frontend: Next.js 14 (App Router), React Server Components, TailwindCSS
- Backend: Next.js API routes, Prisma ORM, PostgreSQL 14
- Auth: NextAuth.js v5 (database sessions)
- Real-time: Server-Sent Events (SSE)
- Deployment: Vercel (frontend), Supabase (database)
- Storage: AWS S3 (file attachments)

EXISTING ARCHITECTURE:
- Monorepo: /app, /components, /lib, /prisma
- Component library: /components/ui (shadcn/ui)
- Database: Multi-tenant (companyId on all tables)
- Soft deletes: deletedAt timestamp pattern
- API conventions: RESTful, Zod validation

CURRENT FEATURE:
Building core task management system from scratch. Users exist, auth works, 
company structure exists. This is the first major feature.

CONSTRAINTS:
- Budget: $100/month (Vercel Pro + Supabase free tier)
- Performance: <300ms page load, <100ms API response
- Accessibility: WCAG 2.1 Level AA (enterprise requirement)
- Compliance: SOC 2 (annual audit)
- Mobile: 40% of users on mobile, must work perfectly

TASK: Create a complete task management system that allows teams to create, 
assign, track, and complete tasks with real-time collaboration.

ARCHITECTURE REQUIREMENTS:

File Structure:
```
/app/tasks/
  page.tsx                    (Task list view)
  [taskId]/
    page.tsx                  (Task detail view)
  /api/tasks/
    route.ts                  (GET all, POST create)
    [taskId]/
      route.ts                (GET one, PATCH update, DELETE)
      /comments/
        route.ts              (GET comments, POST comment)

/components/tasks/
  TaskList.tsx                (List component)
  TaskCard.tsx                (Single task card)
  TaskForm.tsx                (Create/edit form)
  TaskDetail.tsx              (Detail view)
  TaskComments.tsx            (Comment thread)
  TaskFilters.tsx             (Filter sidebar)

/lib/tasks/
  queries.ts                  (Database queries)
  mutations.ts                (Create/update/delete)
  validations.ts              (Zod schemas)
  utils.ts                    (Helper functions)
```

Component Hierarchy:
```
TasksPage
├─ TaskFilters (sidebar)
├─ TaskList
│  └─ TaskCard (repeated)
└─ TaskForm (modal)

TaskDetailPage
├─ TaskDetail (main content)
├─ TaskComments (comment thread)
└─ TaskActivity (activity log)
```

Data Flow:
```
Client → API Route → Prisma → PostgreSQL
              ↓
        Validation (Zod)
              ↓
        Authorization (check ownership)
              ↓
        Business Logic
              ↓
        Response
```

State Management:
- Server Components for initial data
- React Query for client-side caching
- Optimistic updates for mutations
- Real-time via SSE (task updates, new comments)

FUNCTIONAL REQUIREMENTS:

1. Task Creation
   - Title (required, 2-200 chars)
   - Description (optional, rich text, max 10,000 chars)
   - Assignee (dropdown, users in company)
   - Due date (date picker, optional)
   - Priority (Low / Medium / High / Urgent)
   - Status (Todo / In Progress / Done)
   - Tags (multi-select, max 10)
   - Attachments (files, max 5 files, 10MB each)

2. Task List View
   - Display all tasks (paginated, 50 per page)
   - Sort by: Date created, Due date, Priority, Status
   - Filter by: Assignee, Status, Priority, Tags, Date range
   - Search: By title, description (debounced, min 3 chars)
   - Views: List view, Kanban board, Calendar view
   - Bulk actions: Mark complete, Delete, Change assignee

3. Task Detail View
   - Full task information
   - Edit inline (title, description, metadata)
   - Comment thread (nested, markdown support)
   - Activity log (who changed what, when)
   - Related tasks (dependencies, subtasks)
   - File attachments (preview, download, delete)

4. Real-time Updates
   - New tasks appear without refresh
   - Status changes reflected live
   - New comments appear instantly
   - Typing indicators in comments
   - Presence (who's viewing this task)

5. Collaboration
   - @mentions in comments (notify mentioned user)
   - Task assignments (notify assignee)
   - Due date reminders (email 1 day before)
   - Activity notifications (in-app + email)

NON-FUNCTIONAL REQUIREMENTS:

Performance:
- Task list: <300ms load time
- Task detail: <200ms load time
- Task creation: <500ms response
- Search: <100ms response
- Real-time latency: <1 second
- Optimistic UI: Show changes immediately

Security:
- Input validation: Zod schemas on all endpoints
- Authorization: Verify user belongs to company
- Rate limiting: 100 requests/min per user
- File validation: Whitelist types (jpg, png, pdf, docx)
- XSS prevention: Sanitize rich text (DOMPurify)

Accessibility:
- Keyboard navigation: Tab through all interactive elements
- Screen readers: ARIA labels on all controls
- Focus management: Return focus after modal close
- Color contrast: WCAG AA (4.5:1 for text)
- Semantic HTML: Proper heading hierarchy

UX:
- Loading states: Skeleton screens during load
- Error messages: Specific, actionable
- Success feedback: Toast notifications
- Undo: 5-second undo for deletions
- Keyboard shortcuts: n (new task), / (search), esc (close)

Error Handling:
- Network errors: Retry with exponential backoff
- Validation errors: Display inline under fields
- Server errors: Log to Sentry, show generic message
- Offline: Queue mutations, sync when online

SECURITY MANDATES:

- Commandment I (Input Validation):
  * Server-side validation with Zod
  * Title: z.string().min(2).max(200)
  * Description: z.string().max(10000).optional()
  * Priority: z.enum(['LOW', 'MEDIUM', 'HIGH', 'URGENT'])
  * Status: z.enum(['TODO', 'IN_PROGRESS', 'DONE'])
  * File validation: Type + size limits

- Commandment V (Rate Limiting):
  * API endpoints: 100 requests/min per user
  * Task creation: 50 tasks/hour per user
  * File uploads: 20 uploads/hour per user
  * Return 429 with Retry-After header

- Commandment VI (Access Control):
  * Verify user authenticated (getServerSession)
  * Verify user belongs to company (task.companyId === session.user.companyId)
  * Only assignee or creator can delete task
  * Return 403 if unauthorized

- Commandment VII (Data Protection):
  * Files uploaded to S3 with presigned URLs
  * Encrypt files at rest (S3 server-side encryption)
  * HTTPS only in production
  * Sanitize rich text (DOMPurify) before display

- Commandment VIII (Error Handling):
  * Generic errors in production
  * Detailed errors in development
  * Never expose: Database schema, S3 bucket names
  * Log all errors to Sentry

- Commandment IX (Accessibility):
  * WCAG AA compliant forms
  * Keyboard navigation (Tab order logical)
  * Screen reader announcements (role="status" for updates)
  * Focus visible (custom outline)
  * High contrast support

- Commandment X (Documentation):
  * JSDoc comments for all functions
  * API documentation (request/response examples)
  * README: How to run locally
  * Explain multi-tenancy strategy

META-INSTRUCTIONS:
- Start with database schema (Task, Comment, Attachment models)
- Build incrementally: Create → List → Detail → Real-time
- Explain state management (why React Query)
- Consider edge cases: Concurrent edits, deleted assignees
- Plan for scale: What happens at 10,000 tasks?

OUTPUT FORMAT:

DELIVERABLES:

1. ARCHITECTURE OVERVIEW
   - Complete folder structure
   - Data flow diagram
   - Real-time architecture (SSE)

2. DATABASE SCHEMA
```prisma
model Task {
  id          String    @id @default(cuid())
  title       String
  description String?   @db.Text
  status      TaskStatus @default(TODO)
  priority    TaskPriority @default(MEDIUM)
  dueDate     DateTime?
  
  creatorId   String
  creator     User      @relation("TaskCreator", fields: [creatorId], references: [id])
  
  assigneeId  String?
  assignee    User?     @relation("TaskAssignee", fields: [assigneeId], references: [id])
  
  companyId   String
  company     Company   @relation(fields: [companyId], references: [id])
  
  comments    Comment[]
  attachments Attachment[]
  
  createdAt   DateTime  @default(now())
  updatedAt   DateTime  @updatedAt
  deletedAt   DateTime? // Soft delete
  
  @@index([companyId])
  @@index([assigneeId])
  @@index([status])
  @@index([dueDate])
}

enum TaskStatus {
  TODO
  IN_PROGRESS
  DONE
}

enum TaskPriority {
  LOW
  MEDIUM
  HIGH
  URGENT
}

model Comment {
  id        String   @id @default(cuid())
  content   String   @db.Text
  
  taskId    String
  task      Task     @relation(fields: [taskId], references: [id], onDelete: Cascade)
  
  authorId  String
  author    User     @relation(fields: [authorId], references: [id])
  
  createdAt DateTime @default(now())
  updatedAt DateTime @updatedAt
  
  @@index([taskId])
}

model Attachment {
  id        String   @id @default(cuid())
  filename  String
  s3Key     String
  size      Int
  mimeType  String
  
  taskId    String
  task      Task     @relation(fields: [taskId], references: [id], onDelete: Cascade)
  
  uploaderId String
  uploader   User    @relation(fields: [uploaderId], references: [id])
  
  createdAt DateTime @default(now())
  
  @@index([taskId])
}
```

3. API ROUTES
   - app/api/tasks/route.ts (GET all, POST create)
   - app/api/tasks/[taskId]/route.ts (GET, PATCH, DELETE)
   - app/api/tasks/[taskId]/comments/route.ts (GET, POST)

4. COMPONENTS
   - components/tasks/TaskList.tsx
   - components/tasks/TaskCard.tsx
   - components/tasks/TaskForm.tsx
   - components/tasks/TaskDetail.tsx
   - components/tasks/TaskComments.tsx

5. UTILITIES
   - lib/tasks/queries.ts (Prisma queries)
   - lib/tasks/validations.ts (Zod schemas)
   - lib/s3.ts (File upload)

6. TESTS
   - __tests__/api/tasks.test.ts (API route tests)
   - __tests__/components/TaskForm.test.ts (Component tests)

7. DOCUMENTATION
   - README.md (Setup guide)
   - docs/tasks-api.md (API documentation)

VALIDATION CHECKLIST:
□ Task creation works (all fields)
□ Task list displays (with filters)
□ Task detail shows (with comments)
□ Real-time updates work
□ File uploads work (S3)
□ Authorization enforced
□ Rate limiting works
□ Accessible (WCAG AA)
□ Mobile responsive
□ Tests pass (80% coverage)
□ Documentation complete

EXPLANATION REQUIRED:
- Why Server Components for list, Client for detail (hydration)
- Why React Query (caching, optimistic updates)
- Why SSE over WebSockets (simplicity, works with Vercel)
- Multi-tenancy strategy (companyId on all queries)
- Soft delete reasoning (audit trail, accidental deletion recovery)
- Known limitations: No subtasks yet, no dependencies, no recurring tasks
- Next steps: Subtasks, task templates, time tracking
═══════════════════════════════════════════════════════════
````

---
---

## ⚡ PATTERN #2: DELTA PATTERN

### When to Use

**Making minimal changes to existing code** - surgical modifications, not rewrites.

**Examples:**
- "Add rate limiting to this API route"
- "Add loading state to this button"
- "Change validation rule for password"
- "Add new field to existing form"

**Success Rate:** 90-95% (higher than Scaffolding because less can go wrong)

---

### Pattern Template
````markdown
═══════════════════════════════════════════════════════════
DELTA PATTERN TEMPLATE
═══════════════════════════════════════════════════════════

PERSONA: You are a senior engineer with expertise in [AREA]. You excel at making 
surgical code changes without breaking existing functionality. You've refactored 
codebases with millions of lines and never introduced regressions.

CONTEXT:
[Standard 5-layer context]

EXISTING CODE:
```[language]
[Paste complete existing code - be thorough]
```

TASK: Modify the existing code to [SPECIFIC CHANGE] while preserving all current 
functionality and maintaining code style.

CHANGE REQUIREMENTS:
1. [Specific change #1]
2. [Specific change #2]
3. [Specific change #3]

CONSTRAINTS:
- **MINIMAL CHANGES ONLY** (do NOT rewrite unless absolutely necessary)
- **PRESERVE EXISTING BEHAVIOR** (all current features must still work)
- **MAINTAIN CODE STYLE** (match existing patterns, naming conventions)
- **NO BREAKING CHANGES** (public API stays same)
- **BACKWARD COMPATIBLE** (if applicable)

SECURITY MANDATES:
[Relevant commandments based on change type]

META-INSTRUCTIONS:
- Analyze existing code FIRST (understand current logic thoroughly)
- Identify EXACT lines to change (be surgical)
- Explain WHY each change is necessary
- Flag potential side effects (what else might be affected)
- Consider backward compatibility (migration path if needed)
- Preserve comments and formatting

OUTPUT FORMAT:

DELIVERABLES:
1. Modified Code (complete file with changes)

2. Diff Summary (line-by-line explanation)
```diff
   - Old line (removed)
   + New line (added)
```
   Why: [Explanation]

3. Migration Notes (if any breaking changes)
   - What changed
   - How to update calling code
   - Deprecation timeline

4. Testing Instructions
   - How to verify change works
   - Regression test checklist

VALIDATION CHECKLIST:
□ Requested change implemented
□ Existing functionality preserved (no regressions)
□ All existing tests still pass
□ New tests added (for new behavior)
□ Code style maintained
□ No breaking changes (or clearly documented)
═══════════════════════════════════════════════════════════
````

---

### Real Example: Add Rate Limiting
````markdown
═══════════════════════════════════════════════════════════
DELTA PATTERN: ADD RATE LIMITING TO API ROUTE
═══════════════════════════════════════════════════════════

PERSONA: You are a senior backend engineer with 12 years of experience securing 
production APIs. You've added rate limiting to systems handling millions of requests 
daily without ever causing downtime. You specialize in making surgical changes that 
preserve existing behavior while adding critical security layers.

CONTEXT:

PROJECT: B2B SaaS invoicing platform
TECH STACK: Next.js 14, Prisma, PostgreSQL, Upstash Redis
DEPLOYMENT: Vercel (serverless functions)
CURRENT: API routes work but no rate limiting (security gap)

EXISTING CODE:
```typescript
// app/api/invoices/route.ts
import { NextResponse } from 'next/server'
import { getServerSession } from 'next-auth'
import { prisma } from '@/lib/prisma'
import { authOptions } from '@/lib/auth'
import { z } from 'zod'

const invoiceSchema = z.object({
  customerId: z.string().cuid(),
  amount: z.number().positive(),
  dueDate: z.string().datetime()
})

export async function GET(request: Request) {
  const session = await getServerSession(authOptions)
  
  if (!session) {
    return NextResponse.json({ error: 'Unauthorized' }, { status: 401 })
  }
  
  const invoices = await prisma.invoice.findMany({
    where: {
      companyId: session.user.companyId
    }
  })
  
  return NextResponse.json(invoices)
}

export async function POST(request: Request) {
  const session = await getServerSession(authOptions)
  
  if (!session) {
    return NextResponse.json({ error: 'Unauthorized' }, { status: 401 })
  }
  
  const body = await request.json()
  const result = invoiceSchema.safeParse(body)
  
  if (!result.success) {
    return NextResponse.json(
      { error: 'Validation failed', details: result.error.format() },
      { status: 400 }
    )
  }
  
  const invoice = await prisma.invoice.create({
    data: {
      ...result.data,
      companyId: session.user.companyId
    }
  })
  
  return NextResponse.json(invoice, { status: 201 })
}
```

TASK: Add comprehensive rate limiting to both GET and POST endpoints using Upstash 
Redis. Limit authenticated users to 100 requests per minute. Return 429 status with 
Retry-After header when limit exceeded.

CHANGE REQUIREMENTS:
1. Import Upstash Ratelimit library
2. Initialize rate limiter with Redis client
3. Check rate limit BEFORE processing request (at top of each handler)
4. Return 429 with Retry-After header if exceeded
5. Use user ID as rate limit key (per-user limits)
6. Handle Redis errors gracefully (fail open in development)

CONSTRAINTS:
- **MINIMAL CHANGES** (only add rate limiting, don't refactor)
- **PRESERVE BEHAVIOR** (same responses, same status codes for existing flows)
- **MAINTAIN STYLE** (match existing code structure, naming)
- **NO BREAKING CHANGES** (API contract unchanged)
- **GRACEFUL DEGRADATION** (work without Redis in development)

SECURITY MANDATES:
- Commandment V: Rate limiting enforced
  * 100 requests/minute per authenticated user
  * 429 status code with Retry-After header
  * Per-user tracking (not global)
  
META-INSTRUCTIONS:
- Add try-catch blocks around rate limiting (Redis might fail)
- Log rate limit events for monitoring
- Consider: Should GET and POST have different limits? (decision: same limit)
- Explain: Why check rate limit BEFORE auth? (prevent auth bypass)

OUTPUT FORMAT:

DELIVERABLES:

1. MODIFIED CODE
```typescript
// app/api/invoices/route.ts
import { NextResponse } from 'next/server'
import { getServerSession } from 'next-auth'
import { prisma } from '@/lib/prisma'
import { authOptions } from '@/lib/auth'
import { z } from 'zod'
import { Ratelimit } from '@upstash/ratelimit'  // ← NEW IMPORT
import { Redis } from '@upstash/redis'          // ← NEW IMPORT

const invoiceSchema = z.object({
  customerId: z.string().cuid(),
  amount: z.number().positive(),
  dueDate: z.string().datetime()
})

// ← NEW: Initialize rate limiter
const ratelimit = new Ratelimit({
  redis: Redis.fromEnv(),
  limiter: Ratelimit.slidingWindow(100, '1 m'),  // 100 requests per minute
  analytics: true,
  prefix: 'ratelimit:invoices'
})

export async function GET(request: Request) {
  // ← NEW: Rate limit check FIRST (before auth)
  const session = await getServerSession(authOptions)
  
  if (!session) {
    return NextResponse.json({ error: 'Unauthorized' }, { status: 401 })
  }
  
  // ← NEW: Rate limiting (after auth, using user ID)
  try {
    const { success, limit, remaining, reset } = await ratelimit.limit(session.user.id)
    
    if (!success) {
      const retryAfter = Math.floor((reset - Date.now()) / 1000)
      return NextResponse.json(
        { 
          error: 'Too many requests. Please try again later.',
          retryAfter 
        },
        { 
          status: 429,
          headers: {
            'Retry-After': String(retryAfter),
            'X-RateLimit-Limit': String(limit),
            'X-RateLimit-Remaining': String(remaining),
            'X-RateLimit-Reset': String(reset)
          }
        }
      )
    }
  } catch (error) {
    // ← NEW: Graceful degradation (Redis failure)
    console.error('Rate limit check failed:', error)
    // Continue processing (fail open in case of Redis issues)
  }
  
  const invoices = await prisma.invoice.findMany({
    where: {
      companyId: session.user.companyId
    }
  })
  
  return NextResponse.json(invoices)
}

export async function POST(request: Request) {
  // ← NEW: Rate limit check
  const session = await getServerSession(authOptions)
  
  if (!session) {
    return NextResponse.json({ error: 'Unauthorized' }, { status: 401 })
  }
  
  // ← NEW: Rate limiting (same as GET)
  try {
    const { success, limit, remaining, reset } = await ratelimit.limit(session.user.id)
    
    if (!success) {
      const retryAfter = Math.floor((reset - Date.now()) / 1000)
      return NextResponse.json(
        { 
          error: 'Too many requests. Please try again later.',
          retryAfter 
        },
        { 
          status: 429,
          headers: {
            'Retry-After': String(retryAfter),
            'X-RateLimit-Limit': String(limit),
            'X-RateLimit-Remaining': String(remaining),
            'X-RateLimit-Reset': String(reset)
          }
        }
      )
    }
  } catch (error) {
    console.error('Rate limit check failed:', error)
    // Continue processing (fail open)
  }
  
  const body = await request.json()
  const result = invoiceSchema.safeParse(body)
  
  if (!result.success) {
    return NextResponse.json(
      { error: 'Validation failed', details: result.error.format() },
      { status: 400 }
    )
  }
  
  const invoice = await prisma.invoice.create({
    data: {
      ...result.data,
      companyId: session.user.companyId
    }
  })
  
  return NextResponse.json(invoice, { status: 201 })
}
```

2. DIFF SUMMARY
```diff
+ import { Ratelimit } from '@upstash/ratelimit'
+ import { Redis } from '@upstash/redis'
```
**Why:** Import rate limiting libraries
```diff
+ const ratelimit = new Ratelimit({
+   redis: Redis.fromEnv(),
+   limiter: Ratelimit.slidingWindow(100, '1 m'),
+   analytics: true,
+   prefix: 'ratelimit:invoices'
+ })
```
**Why:** Initialize rate limiter (100 req/min per user, sliding window algorithm)
```diff
  export async function GET(request: Request) {
    const session = await getServerSession(authOptions)
    
    if (!session) {
      return NextResponse.json({ error: 'Unauthorized' }, { status: 401 })
    }
    
+   try {
+     const { success, limit, remaining, reset } = await ratelimit.limit(session.user.id)
+     
+     if (!success) {
+       // Return 429 with headers
+     }
+   } catch (error) {
+     console.error('Rate limit check failed:', error)
+   }
    
    const invoices = await prisma.invoice.findMany({ ... })
```
**Why:** Add rate limit check AFTER auth (use authenticated user ID as key)

**Same pattern repeated in POST handler.**

**NOT CHANGED:**
- All existing logic preserved
- Response formats unchanged (except new 429 response)
- Validation behavior unchanged
- Database queries unchanged
- Error handling for existing errors unchanged

3. MIGRATION NOTES

**No breaking changes** - this is purely additive.

**New Dependencies:**
```bash
npm install @upstash/ratelimit @upstash/redis
```

**Environment Variables Required:**
```bash
# .env
UPSTASH_REDIS_REST_URL=https://your-redis-url.upstash.io
UPSTASH_REDIS_REST_TOKEN=your_token_here
```

**New Response (429):**
```json
{
  "error": "Too many requests. Please try again later.",
  "retryAfter": 42
}
```

**Headers:**
- `Retry-After`: Seconds until rate limit resets
- `X-RateLimit-Limit`: Max requests allowed
- `X-RateLimit-Remaining`: Requests remaining
- `X-RateLimit-Reset`: Timestamp when limit resets

4. TESTING INSTRUCTIONS

**Manual Test:**
```bash
# 1. Make 100 requests quickly
for i in {1..100}; do
  curl http://localhost:3000/api/invoices \
    -H "Cookie: session_token=your_token"
done

# 2. Make 101st request - should get 429
curl -v http://localhost:3000/api/invoices \
  -H "Cookie: session_token=your_token"

# Expected: 429 status with Retry-After header
```

**Automated Test:**
```typescript
// __tests__/api/invoices-rate-limit.test.ts
import { GET } from '@/app/api/invoices/route'

describe('Rate limiting', () => {
  it('allows 100 requests per minute', async () => {
    // Make 100 requests
    for (let i = 0; i < 100; i++) {
      const response = await GET(mockRequest)
      expect(response.status).toBe(200)
    }
  })
  
  it('returns 429 after 100 requests', async () => {
    // Make 101 requests
    for (let i = 0; i < 100; i++) {
      await GET(mockRequest)
    }
    
    const response = await GET(mockRequest)
    expect(response.status).toBe(429)
    
    const data = await response.json()
    expect(data.error).toContain('Too many requests')
    expect(data.retryAfter).toBeGreaterThan(0)
  })
  
  it('includes rate limit headers', async () => {
    const response = await GET(mockRequest)
    
    expect(response.headers.get('X-RateLimit-Limit')).toBe('100')
    expect(response.headers.get('X-RateLimit-Remaining')).toBeDefined()
  })
})
```

**Redis Failure Test:**
```typescript
it('fails open when Redis unavailable', async () => {
  // Mock Redis failure
  jest.spyOn(ratelimit, 'limit').mockRejectedValue(new Error('Redis down'))
  
  const response = await GET(mockRequest)
  
  // Should still return 200 (fail open, not closed)
  expect(response.status).toBe(200)
})
```

VALIDATION CHECKLIST:
□ Rate limiting works (returns 429 after 100 requests)
□ Retry-After header present in 429 responses
□ Rate limit headers included (X-RateLimit-*)
□ Existing functionality preserved (all tests pass)
□ Redis errors handled gracefully (doesn't crash)
□ Environment variables documented (.env.example)
□ Per-user tracking (not global limit)
□ Both GET and POST endpoints protected
═══════════════════════════════════════════════════════════
````

---
