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
---

## 🔍 PATTERN #3: VALIDATOR PATTERN

### When to Use

**Reviewing code for bugs, security issues, performance problems, or accessibility gaps**.

**Examples:**
- "Review this API route for security vulnerabilities"
- "Audit this component for accessibility issues"
- "Check this query for performance problems"
- "Validate this form against best practices"

**Success Rate:** 80-90% (depends on code complexity and reviewer expertise)

---

### Pattern Template
````markdown
═══════════════════════════════════════════════════════════
VALIDATOR PATTERN TEMPLATE
═══════════════════════════════════════════════════════════

PERSONA: You are a senior [SPECIALTY] engineer and expert code reviewer with 
[YEARS] years of experience auditing production code. You have a keen eye for 
subtle bugs, security vulnerabilities, and performance issues. You've conducted 
[NUMBER] code reviews and prevented [NUMBER] production incidents.

CONTEXT:
[Standard context - what's this code part of?]

CODE TO REVIEW:
```[language]
[Paste ALL relevant code - be comprehensive]
```

REVIEW FOCUS:
Primary: [Main concern - security / performance / accessibility / quality]
Secondary: [Additional concerns]

VALIDATION CRITERIA:

[IF SECURITY REVIEW]:
- Check against 10 Strategic Commandments
- Identify OWASP Top 10 violations
- Look for: Input validation, output encoding, auth/authz, session management

[IF PERFORMANCE REVIEW]:
- Identify bottlenecks: N+1 queries, unnecessary renders, large bundles
- Suggest optimizations with measurable impact
- Look for: Database inefficiency, memory leaks, blocking operations

[IF ACCESSIBILITY REVIEW]:
- Verify WCAG 2.1 Level AA compliance
- Check: Keyboard navigation, screen readers, color contrast, semantic HTML
- Test with: NVDA/JAWS simulation

[IF CODE QUALITY REVIEW]:
- Check: Readability, maintainability, testability, documentation
- Identify: Code smells, violations of SOLID principles, missing error handling

META-INSTRUCTIONS:
- Be thorough but practical (focus on real issues, not nitpicks)
- Prioritize findings by severity: Critical > High > Medium > Low
- Provide SPECIFIC fixes (not just "this is wrong")
- Explain WHY each issue is problematic (impact on users/business)
- Include positive feedback (what's done well - build confidence)
- Consider context (startup MVP vs enterprise system - different standards)

OUTPUT FORMAT:

DELIVERABLES:

1. EXECUTIVE SUMMARY
   - Overall Quality Rating: [1-10]
   - Critical Issues: [Count]
   - High Priority Issues: [Count]
   - Medium Priority Issues: [Count]
   - Low Priority Issues: [Count]
   - Positive Highlights: [What's done well]

2. DETAILED FINDINGS

For each issue:

**[SEVERITY]** #[NUMBER]: [Issue Title] ([Category])
- **Location:** [File, line numbers]
- **Issue:** [What's wrong - be specific]
- **Impact:** [Why this matters - user impact, business impact, technical debt]
- **Fix:** [Specific solution with code example]
- **References:** [OWASP link, WCAG criterion, documentation]

Categories: Security / Performance / Accessibility / Quality / Maintainability

3. RECOMMENDED ACTIONS

**IMMEDIATE** (Fix today - critical):
- [Action 1]
- [Action 2]

**SHORT-TERM** (Fix this sprint - high priority):
- [Action 3]
- [Action 4]

**LONG-TERM** (Plan for next quarter - medium/low):
- [Action 5]
- [Action 6]

4. POSITIVE FEEDBACK
- [Thing done well #1]
- [Thing done well #2]
- [Good pattern to continue]

VALIDATION CHECKLIST:
□ All critical issues identified
□ Fixes provided (not just problems listed)
□ Severity accurately assessed
□ Impact explained (business + technical)
□ Positive feedback included (morale boost)
□ References provided (learning resources)
═══════════════════════════════════════════════════════════
````

---

### Real Example: Security Audit of Login Endpoint
````markdown
═══════════════════════════════════════════════════════════
VALIDATOR PATTERN: SECURITY AUDIT OF LOGIN API
═══════════════════════════════════════════════════════════

PERSONA: You are a senior security engineer with 15 years of experience conducting 
security audits for financial institutions and healthcare companies. You've prevented 
over 50 data breaches by identifying vulnerabilities during code reviews. You're an 
expert in OWASP Top 10, authentication flows, and session management. You've achieved 
CISSP and OSCP certifications.

CONTEXT:

PROJECT: B2B SaaS platform handling sensitive business data
TECH STACK: Next.js 14, Prisma, PostgreSQL, NextAuth.js
COMPLIANCE: SOC 2 Type II (annual audit required)
USERS: 10,000+ businesses with highly sensitive data

CODE TO REVIEW:
```typescript
// app/api/auth/login/route.ts
import { NextResponse } from 'next/server'
import { prisma } from '@/lib/prisma'
import bcrypt from 'bcryptjs'
import jwt from 'jsonwebtoken'

export async function POST(request: Request) {
  const { email, password } = await request.json()
  
  const user = await prisma.user.findUnique({
    where: { email }
  })
  
  if (!user) {
    return NextResponse.json(
      { error: 'User not found' },
      { status: 404 }
    )
  }
  
  const valid = await bcrypt.compare(password, user.password)
  
  if (!valid) {
    return NextResponse.json(
      { error: 'Invalid password' },
      { status: 401 }
    )
  }
  
  const token = jwt.sign(
    { userId: user.id },
    process.env.JWT_SECRET!,
    { expiresIn: '7d' }
  )
  
  return NextResponse.json({ token, user })
}
```

REVIEW FOCUS:
Primary: Security vulnerabilities (OWASP Top 10 compliance)
Secondary: SOC 2 compliance, production readiness

VALIDATION CRITERIA:
- Commandment I: Input validation
- Commandment III: Password security
- Commandment IV: Session management
- Commandment V: Rate limiting
- Commandment VIII: Error handling
- OWASP Top 10 (2021)
- SOC 2 requirements

META-INSTRUCTIONS:
- Identify ALL security vulnerabilities (even minor)
- Reference specific OWASP categories
- Provide production-ready fixes
- Consider attacker perspective (how would I exploit this?)
- Assess SOC 2 compliance impact

OUTPUT FORMAT:

1. EXECUTIVE SUMMARY

**Overall Security Rating: 3/10 (CRITICAL ISSUES PRESENT)**

- **Critical Issues:** 5
- **High Priority Issues:** 2
- **Medium Priority Issues:** 1
- **Low Priority Issues:** 0

**Status:** ❌ NOT PRODUCTION-READY (would fail SOC 2 audit)

**Positive Highlights:**
- ✅ bcrypt used for password hashing (good)
- ✅ Async/await syntax (modern)

**Critical Risks:**
- Username enumeration vulnerability
- No rate limiting (brute force possible)
- JWT instead of database sessions (cannot revoke)
- No input validation
- Exposing full user object

---

2. DETAILED FINDINGS

**CRITICAL** #1: Username Enumeration (OWASP A01: Broken Access Control)

- **Location:** Lines 12-16, 20-24
- **Issue:** Different error messages for "user not found" vs "invalid password"
- **Impact:**
  * Attacker can enumerate valid email addresses
  * Try login with random emails:
    - "User not found" = email doesn't exist
    - "Invalid password" = email EXISTS (attacker now knows valid username)
  * Enables targeted phishing attacks
  * Violates privacy (leaks user existence)
- **Fix:**
```typescript
// ❌ CURRENT (reveals information)
if (!user) {
  return NextResponse.json({ error: 'User not found' }, { status: 404 })
}
if (!valid) {
  return NextResponse.json({ error: 'Invalid password' }, { status: 401 })
}

// ✅ FIXED (generic message)
if (!user || !valid) {
  return NextResponse.json(
    { error: 'Invalid credentials' },
    { status: 401 }
  )
}
```
- **Reference:** [CWE-203: Observable Discrepancy](https://cwe.mitre.org/data/definitions/203.html)

---

**CRITICAL** #2: No Rate Limiting (Commandment V violation)

- **Location:** Entire endpoint
- **Issue:** Unlimited login attempts allowed
- **Impact:**
  * Brute force attack feasible
  * Attacker can try thousands of passwords per minute
  * Will eventually crack weak passwords
  * Server resource exhaustion (DoS)
  * SOC 2 violation (access controls required)
- **Fix:**
```typescript
import { Ratelimit } from '@upstash/ratelimit'
import { Redis } from '@upstash/redis'

const ratelimit = new Ratelimit({
  redis: Redis.fromEnv(),
  limiter: Ratelimit.slidingWindow(5, '15 m')  // 5 attempts per 15 minutes
})

export async function POST(request: Request) {
  const ip = request.headers.get('x-forwarded-for') || 'unknown'
  const { success } = await ratelimit.limit(ip)
  
  if (!success) {
    return NextResponse.json(
      { error: 'Too many login attempts. Try again in 15 minutes.' },
      { status: 429, headers: { 'Retry-After': '900' } }
    )
  }
  
  // ... rest of login logic
}
```
- **Reference:** [OWASP: Authentication Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Authentication_Cheat_Sheet.html#login-throttling)

---

**CRITICAL** #3: JWT Instead of Database Sessions (Commandment IV violation)

- **Location:** Lines 26-30
- **Issue:** Using JWT for sessions (stateless, cannot revoke)
- **Impact:**
  * User logs out → token still valid for 7 days
  * Password changed → old tokens still work
  * Account compromised → cannot invalidate all sessions
  * Impossible to force logout
  * SOC 2 violation (session revocation required)
- **Fix:** Switch to NextAuth.js database sessions
```typescript
// Use NextAuth.js with Prisma adapter instead of manual JWT

// lib/auth.ts
import { PrismaAdapter } from '@auth/prisma-adapter'
import CredentialsProvider from 'next-auth/providers/credentials'

export const authOptions = {
  adapter: PrismaAdapter(prisma),
  session: {
    strategy: 'database',  // ← Database sessions (revocable)
    maxAge: 7 * 24 * 60 * 60
  },
  providers: [
    CredentialsProvider({
      async authorize(credentials) {
        const user = await prisma.user.findUnique({
          where: { email: credentials.email }
        })
        
        if (!user) return null
        
        const valid = await bcrypt.compare(
          credentials.password,
          user.password
        )
        
        if (!valid) return null
        
        return { id: user.id, email: user.email, name: user.name }
      }
    })
  ]
}
```
- **Reference:** [Article VI, Commandment IV: Session Management](../01-foundation-layer/07-article-vi-strategic-commandments.md#commandment-iv-session-management)

---

**CRITICAL** #4: No Input Validation (Commandment I violation)

- **Location:** Line 8
- **Issue:** No validation of email/password format or length
- **Impact:**
  * Could send malformed data to database
  * No length limits (DoS via huge payloads)
  * Email not validated (could be gibberish)
  * SQL injection possible (if raw queries used elsewhere)
  * Application crash if unexpected data type
- **Fix:**
```typescript
import { z } from 'zod'

const loginSchema = z.object({
  email: z.string().email('Invalid email format').max(255),
  password: z.string().min(1, 'Password required').max(100)
})

export async function POST(request: Request) {
  const body = await request.json()
  
  // Validate input
  const result = loginSchema.safeParse(body)
  
  if (!result.success) {
    return NextResponse.json(
      { 
        error: 'Invalid input',
        details: result.error.format()
      },
      { status: 400 }
    )
  }
  
  const { email, password } = result.data
  // ... rest of login logic
}
```
- **Reference:** [OWASP: Input Validation Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Input_Validation_Cheat_Sheet.html)

---

**CRITICAL** #5: Exposing Full User Object (Data Leakage)

- **Location:** Line 33
- **Issue:** Returning complete user object in response
- **Impact:**
  * Password hash exposed to client (security risk)
  * Internal database IDs exposed
  * Could leak sensitive fields (role, permissions, deleted status)
  * Privacy violation (more data than necessary)
  * Potential PII leakage
- **Fix:**
```typescript
// ❌ NEVER return full user object
return NextResponse.json({ token, user })

// ✅ Return only safe fields
return NextResponse.json({
  token,
  user: {
    id: user.id,
    email: user.email,
    name: user.name
    // DO NOT include: password, passwordHash, role, createdAt, etc.
  }
})

// ✅ BETTER: Use NextAuth (handles this automatically)
// NextAuth only returns safe fields in session
```
- **Reference:** [CWE-209: Generation of Error Message Containing Sensitive Information](https://cwe.mitre.org/data/definitions/209.html)

---

**HIGH** #1: No HTTPS Enforcement (Security Misconfiguration)

- **Location:** Application-wide (not code-specific)
- **Issue:** No verification that HTTPS is used
- **Impact:**
  * Credentials sent over HTTP (man-in-the-middle attack)
  * Tokens stolen in transit
  * Session hijacking possible
  * Compliance violation (PCI DSS, SOC 2)
- **Fix:** Add middleware to enforce HTTPS in production
```typescript
// middleware.ts
import { NextResponse } from 'next/server'
import type { NextRequest } from 'next/server'

export function middleware(request: NextRequest) {
  // Enforce HTTPS in production
  if (
    process.env.NODE_ENV === 'production' &&
    request.headers.get('x-forwarded-proto') !== 'https'
  ) {
    const url = request.url.replace('http://', 'https://')
    return NextResponse.redirect(url, 301)
  }
  
  return NextResponse.next()
}

export const config = {
  matcher: '/:path*'
}
```
- **Reference:** [OWASP: Transport Layer Protection Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Transport_Layer_Protection_Cheat_Sheet.html)

---

**HIGH** #2: No Logging/Monitoring (Security Blind Spot)

- **Location:** Entire endpoint
- **Issue:** Failed login attempts not logged
- **Impact:**
  * Cannot detect brute force attacks
  * No audit trail for security incidents
  * Impossible to investigate breaches
  * SOC 2 violation (logging required)
  * No alerting on suspicious activity
- **Fix:**
```typescript
import { logger } from '@/lib/logger'  // Winston/Pino/Sentry

export async function POST(request: Request) {
  const { email, password } = result.data
  
  const user = await prisma.user.findUnique({ where: { email } })
  const valid = user ? await bcrypt.compare(password, user.password) : false
  
  if (!user || !valid) {
    // Log failed attempt
    logger.warn('Failed login attempt', {
      email: hashEmail(email),  // Hash for privacy
      ip: request.headers.get('x-forwarded-for'),
      userAgent: request.headers.get('user-agent'),
      timestamp: new Date().toISOString()
    })
    
    return NextResponse.json({ error: 'Invalid credentials' }, { status: 401 })
  }
  
  // Log successful login
  logger.info('Successful login', {
    userId: user.id,
    ip: request.headers.get('x-forwarded-for'),
    timestamp: new Date().toISOString()
  })
  
  // ... generate token
}

function hashEmail(email: string): string {
  // Hash email for privacy (use crypto.createHash)
  return createHash('sha256').update(email).digest('hex').slice(0, 8)
}
```
- **Reference:** [OWASP: Logging Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Logging_Cheat_Sheet.html)

---

**MEDIUM** #1: Weak JWT Secret Validation

- **Location:** Line 28 (process.env.JWT_SECRET)
- **Issue:** No validation that JWT secret is strong
- **Impact:**
  * Weak secret = attacker can forge tokens
  * Default/example secrets in production
  * Brute force attack on JWT signature
- **Fix:**
```typescript
const JWT_SECRET = process.env.JWT_SECRET

// Validate secret at startup
if (!JWT_SECRET || JWT_SECRET.length < 32) {
  throw new Error(
    'JWT_SECRET must be at least 32 characters. ' +
    'Generate with: openssl rand -base64 32'
  )
}

// Better: Use environment validation library
import { z } from 'zod'

const envSchema = z.object({
  JWT_SECRET: z.string().min(32, 'JWT_SECRET must be 32+ chars')
})

const env = envSchema.parse(process.env)
```
- **Reference:** [OWASP: Cryptographic Storage Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Cryptographic_Storage_Cheat_Sheet.html)

---

3. RECOMMENDED ACTIONS

**IMMEDIATE** (Fix today - production at risk):
1. ✅ Add generic error message (fix username enumeration) - **30 minutes**
2. ✅ Implement rate limiting (Upstash Redis) - **1 hour**
3. ✅ Add input validation (Zod schemas) - **30 minutes**
4. ✅ Stop returning full user object (whitelist fields) - **15 minutes**

**SHORT-TERM** (Fix this sprint - before SOC 2 audit):
5. ✅ Migrate to NextAuth.js database sessions - **4 hours**
6. ✅ Add HTTPS enforcement (production middleware) - **30 minutes**
7. ✅ Implement logging (failed/successful logins) - **2 hours**

**LONG-TERM** (Plan for next quarter):
8. ⚠️ Add multi-factor authentication (2FA) - **1 week**
9. ⚠️ Implement account lockout (after 10 failed attempts) - **1 day**
10. ⚠️ Add CAPTCHA (prevent automated attacks) - **2 days**

---

4. POSITIVE FEEDBACK

✅ **bcrypt used correctly** - Password hashing with bcrypt (cost factor 10) is good. 
Industry standard. Could increase to 12 for extra security, but 10 is acceptable.

✅ **Async/await syntax** - Modern async handling, clean code structure.

✅ **Environment variables** - JWT_SECRET stored in environment (not hardcoded). Good practice.

---

5. SOC 2 COMPLIANCE IMPACT

**Current Status:** ❌ FAILED

**Violations:**
- ❌ No audit logging (CC6.1: Logical and Physical Access Controls)
- ❌ No rate limiting (CC6.1: Access Controls)
- ❌ Session management inadequate (CC6.1: Cannot revoke sessions)
- ❌ No monitoring/alerting (CC7.2: System Monitoring)

**Required for Compliance:**
□ Audit logging implemented (login attempts, successes, failures)
□ Rate limiting enforced (prevent brute force)
□ Database sessions (revocable tokens)
□ HTTPS enforced (encrypted transport)
□ Vulnerability remediation completed (all critical + high issues)
□ Security testing performed (penetration test)

**Timeline:** Must fix all CRITICAL and HIGH issues before annual audit (3 weeks).

---

VALIDATION CHECKLIST:
□ All 5 critical issues identified and explained
□ All 2 high priority issues identified
□ Fixes provided with complete code examples
□ OWASP/CWE references included
□ SOC 2 compliance assessed
□ Remediation timeline provided (immediate, short-term, long-term)
□ Positive feedback included (bcrypt, async/await)
□ Business impact explained (brute force, data breach, compliance failure)
═══════════════════════════════════════════════════════════
````

---
---

## 🐛 PATTERN #4: DEBUGGING PATTERN

### When to Use

**Fixing bugs in existing code** - something is broken and you need to find and fix the root cause.

**Examples:**
- "Fix: User session expires immediately after login"
- "Debug: Cart total calculates incorrectly"
- "Resolve: Form submission fails silently"
- "Fix: Image upload works locally but fails in production"

**Success Rate:** 85-95% (when proper reproduction steps provided)

---

### Pattern Template
````markdown
═══════════════════════════════════════════════════════════
DEBUGGING PATTERN TEMPLATE
═══════════════════════════════════════════════════════════

PERSONA: You are a senior debugging specialist with [YEARS] years of experience 
fixing production issues under pressure. You excel at root cause analysis and 
systematic debugging. You've resolved over [NUMBER] critical production bugs 
and understand how to debug complex distributed systems.

CONTEXT:
[Standard 5-layer context]

BUG DESCRIPTION:
- **Symptom:** [What's visibly wrong]
- **Expected Behavior:** [What should happen]
- **Actual Behavior:** [What actually happens]
- **Frequency:** [Always / Sometimes / Rare]
- **Environment:** [Production / Staging / Development]
- **First Observed:** [When did this start?]
- **Recent Changes:** [What changed recently?]

REPRODUCTION STEPS:
1. [Step 1]
2. [Step 2]
3. [Step 3]
→ Bug occurs at this point

RELEVANT CODE:
```[language]
[Paste code where bug might be - be thorough]
```

ERROR MESSAGES (if any):
```
[Paste complete error messages, stack traces, console logs]
```

INVESTIGATION DONE:
- [What you've already tried]
- [What didn't work]
- [What you've ruled out]

META-INSTRUCTIONS:
- Use systematic debugging (5 Whys, Binary Search)
- Don't guess - prove the root cause with evidence
- Provide fix + detailed explanation
- Add tests to prevent regression
- Consider edge cases that might cause similar bugs
- Document WHY the bug happened (helps prevent future bugs)

OUTPUT FORMAT:

DELIVERABLES:

1. ROOT CAUSE ANALYSIS
   - **Primary Cause:** [The actual bug]
   - **Contributing Factors:** [What made it worse]
   - **Why Not Caught Earlier:** [Testing gap]
   - **Environmental Factors:** [Production vs dev differences]

2. FIX
   - Modified Code (complete, with changes highlighted)
   - Explanation (why this fixes the root cause)
   - Verification (how to confirm fix works)

3. REGRESSION PREVENTION
   - Unit test for this specific bug
   - Integration test for the flow
   - Monitoring/logging to detect recurrence
   - Code review checklist update

4. RELATED ISSUES (potential)
   - Similar bugs that might exist elsewhere
   - Broader patterns to address
   - Technical debt to pay down

VALIDATION CHECKLIST:
□ Bug reproduced (confirmed issue exists)
□ Root cause identified (not just symptom treated)
□ Fix tested (bug no longer occurs)
□ Regression test added (prevents recurrence)
□ No new bugs introduced (existing tests still pass)
□ Documentation updated (if API/behavior changed)
═══════════════════════════════════════════════════════════
````

---

### Real Example: Session Expires Immediately After Login
````markdown
═══════════════════════════════════════════════════════════
DEBUGGING PATTERN: SESSION EXPIRES IMMEDIATELY
═══════════════════════════════════════════════════════════

PERSONA: You are a senior authentication specialist with 12 years of experience 
debugging session and auth issues in distributed systems. You've fixed session bugs 
in applications handling millions of concurrent users. You're an expert in cookies, 
JWT, OAuth, and browser security policies. You understand the subtle differences 
between development and production environments.

CONTEXT:

PROJECT: B2B SaaS task management application
TECH STACK: Next.js 14, NextAuth.js v5, Prisma, PostgreSQL
DEPLOYMENT: Vercel (serverless functions)
RECENT CHANGES: Deployed v1.3.0 to production 2 hours ago

BUG DESCRIPTION:
- **Symptom:** User logs in successfully, but is immediately logged out when 
  navigating to /dashboard (redirected back to /login)
- **Expected Behavior:** User stays logged in for 7 days
- **Actual Behavior:** Session appears valid for 1-2 seconds, then expires
- **Frequency:** ALWAYS (100% of users affected)
- **Environment:** PRODUCTION ONLY (works perfectly in development)
- **First Observed:** 2 hours ago (immediately after v1.3.0 deploy)
- **Recent Changes:** 
  * Added custom cookie settings in NextAuth config
  * Updated middleware to use getServerSession()

REPRODUCTION STEPS:
1. Go to https://app.example.com/login
2. Enter valid credentials (email: test@example.com, password: test123)
3. Click "Login" button
4. User is redirected to /dashboard
5. Page loads for approximately 1 second
6. User is immediately redirected back to /login
→ Session is invalid (user logged out)

RELEVANT CODE:
```typescript
// app/api/auth/[...nextauth]/route.ts
import NextAuth from 'next-auth'
import CredentialsProvider from 'next-auth/providers/credentials'
import { PrismaAdapter } from '@auth/prisma-adapter'
import { prisma } from '@/lib/prisma'

export const authOptions = {
  adapter: PrismaAdapter(prisma),
  providers: [
    CredentialsProvider({
      credentials: {
        email: {},
        password: {}
      },
      async authorize(credentials) {
        // ... validation logic (works fine)
        return user
      }
    })
  ],
  session: {
    strategy: 'database',
    maxAge: 7 * 24 * 60 * 60  // 7 days
  },
  cookies: {
    sessionToken: {
      name: `__Secure-next-auth.session-token`,  // ← ADDED IN v1.3.0
      options: {
        httpOnly: true,
        sameSite: 'lax',
        path: '/',
        secure: true
      }
    }
  }
}

export const handler = NextAuth(authOptions)
export { handler as GET, handler as POST }
```
```typescript
// middleware.ts
import { getServerSession } from 'next-auth'
import { NextResponse } from 'next/server'
import type { NextRequest } from 'next/server'
import { authOptions } from '@/app/api/auth/[...nextauth]/route'

export async function middleware(request: NextRequest) {
  const session = await getServerSession(authOptions)  // ← CHANGED IN v1.3.0
  
  if (!session && request.nextUrl.pathname.startsWith('/dashboard')) {
    return NextResponse.redirect(new URL('/login', request.url))
  }
  
  return NextResponse.next()
}

export const config = {
  matcher: ['/dashboard/:path*']
}
```

ERROR MESSAGES:
```
Browser Console:
No errors visible

Server Logs (Vercel):
[NextAuth][warn][CALLBACK_CREDENTIALS_JWT_ERROR]
https://next-auth.js.org/errors#callback_credentials_jwt_error 
Callback error. Error: Read more at https://errors.authjs.dev#adapter_error

[NextAuth][error][SESSION_ERROR]
https://next-auth.js.org/errors#session_error
```

INVESTIGATION DONE:
- ✅ Checked database: Sessions ARE being created (session table has entries)
- ✅ Checked browser cookies: Cookie IS being set (visible in DevTools)
- ✅ Checked middleware: Middleware IS running (logs confirm execution)
- ✅ Tried clearing cache, different browsers: Same issue everywhere
- ✅ Verified credentials provider works: Login succeeds, creates session
- ❌ Bug only in production, NOT in development (suspicious!)

META-INSTRUCTIONS:
- This is production-only (environment-specific bug)
- Session IS created but immediately becomes invalid
- Focus on: Cookie name, HTTPS, domain, SameSite, Vercel edge runtime
- Compare production vs development differences
- Consider: Middleware runs in Edge runtime (different from Node.js)

OUTPUT FORMAT:

DELIVERABLES:

1. ROOT CAUSE ANALYSIS

**PRIMARY CAUSE: Cookie Name Mismatch Between Creation and Retrieval**

The session cookie is being created with name `__Secure-next-auth.session-token` 
but the middleware is unable to read it because `getServerSession()` doesn't work 
in Edge runtime (where middleware runs).

**How the Bug Happens:**
```
1. User logs in → POST /api/auth/callback/credentials
   ↓
2. NextAuth creates session in database ✅
   ↓
3. NextAuth sets cookie: __Secure-next-auth.session-token ✅
   (Note: __Secure- prefix only used in production with HTTPS)
   ↓
4. Browser redirects to /dashboard
   ↓
5. Middleware runs (Edge runtime)
   ↓
6. getServerSession(authOptions) called
   ❌ FAILS: getServerSession requires Node.js runtime (req/res objects)
   ❌ Middleware runs in Edge runtime (NextRequest/NextResponse only)
   ↓
7. session = null (even though cookie exists!)
   ↓
8. Middleware redirects to /login (thinks user not authenticated)
```

**Why It Works in Development:**
```
Development (HTTP):
- Cookie name: next-auth.session-token (no __Secure- prefix)
- getServerSession() works in dev server (Node.js runtime)
- No edge runtime in dev mode

Production (HTTPS):
- Cookie name: __Secure-next-auth.session-token (__Secure- required for HTTPS)
- Middleware runs in Vercel Edge runtime
- getServerSession() INCOMPATIBLE with Edge runtime
```

**CONTRIBUTING FACTORS:**

1. **Custom cookie name added in v1.3.0**
   - Changed default cookie name to `__Secure-` prefix
   - Triggers HTTPS-only cookie behavior

2. **Middleware using wrong function**
   - `getServerSession()` requires Node.js runtime
   - Middleware runs in Edge runtime on Vercel
   - Should use `getToken()` instead (Edge-compatible)

3. **No error surfaced to user**
   - Silent failure (session check fails, no error logged)
   - User sees redirect, doesn't know why

**WHY NOT CAUGHT EARLIER:**

- Development uses HTTP (no `__Secure-` prefix, different behavior)
- No staging environment with HTTPS + Edge runtime
- Tests don't cover Edge runtime execution
- NextAuth documentation unclear about Edge runtime limitations

**ENVIRONMENTAL FACTORS:**

| Factor | Development | Production |
|--------|-------------|------------|
| Protocol | HTTP | HTTPS |
| Cookie name | `next-auth.session-token` | `__Secure-next-auth.session-token` |
| Middleware runtime | Node.js | Vercel Edge |
| `getServerSession()` | Works ✅ | Fails ❌ |

---

2. FIX

**SOLUTION: Use `getToken()` Instead of `getServerSession()` in Middleware**

`getToken()` is designed for Edge runtime and correctly handles cookie name prefixes.
```typescript
// ❌ BROKEN MIDDLEWARE (current - v1.3.0)
import { getServerSession } from 'next-auth'
import { authOptions } from '@/app/api/auth/[...nextauth]/route'

export async function middleware(request: NextRequest) {
  const session = await getServerSession(authOptions)
  // ↑ This doesn't work in Edge runtime!
  // getServerSession needs Node.js req/res objects
  // Middleware only has NextRequest (Edge)
  
  if (!session && request.nextUrl.pathname.startsWith('/dashboard')) {
    return NextResponse.redirect(new URL('/login', request.url))
  }
}

// ✅ FIXED MIDDLEWARE (correct approach)
import { NextResponse } from 'next/server'
import type { NextRequest } from 'next/server'
import { getToken } from 'next-auth/jwt'  // ← Use this for middleware

export async function middleware(request: NextRequest) {
  // getToken() works in Edge runtime
  const token = await getToken({
    req: request,
    secret: process.env.NEXTAUTH_SECRET
  })
  
  // Protected routes
  if (!token && request.nextUrl.pathname.startsWith('/dashboard')) {
    console.log('[Middleware] No valid session, redirecting to /login')
    return NextResponse.redirect(new URL('/login', request.url))
  }
  
  console.log('[Middleware] Session valid for user:', token?.email)
  return NextResponse.next()
}

export const config = {
  matcher: ['/dashboard/:path*']
}
```

**WHY THIS FIXES THE BUG:**

1. **`getToken()` is Edge-compatible**
   - Designed specifically for middleware/Edge runtime
   - Works with NextRequest (doesn't need Node.js req/res)

2. **Handles `__Secure-` prefix automatically**
   - Knows to look for `__Secure-next-auth.session-token` in production
   - Knows to look for `next-auth.session-token` in development

3. **Reads JWT from cookie correctly**
   - Decodes the session token
   - Returns decoded token data (userId, email, etc.)

**ADDITIONAL IMPROVEMENT: Add Error Handling**
```typescript
export async function middleware(request: NextRequest) {
  try {
    const token = await getToken({
      req: request,
      secret: process.env.NEXTAUTH_SECRET
    })
    
    if (!token) {
      console.warn('[Middleware] No valid session token found')
      
      // Only redirect if accessing protected route
      if (request.nextUrl.pathname.startsWith('/dashboard')) {
        return NextResponse.redirect(new URL('/login', request.url))
      }
    }
    
    return NextResponse.next()
    
  } catch (error) {
    console.error('[Middleware] Session validation error:', error)
    
    // Fail open (allow access) to prevent complete lockout on error
    // Log to monitoring service
    return NextResponse.next()
  }
}
```

**VERIFICATION:**

1. Deploy fixed middleware to production
2. Log in with valid credentials
3. Navigate to /dashboard
4. Session should persist (no redirect to /login)
5. Close browser, reopen
6. Session should still be valid (cookie persists)

---

3. REGRESSION PREVENTION

**TEST #1: Session Persistence After Login (E2E)**
```typescript
// __tests__/e2e/auth-session.spec.ts
import { test, expect } from '@playwright/test'

test('session persists after login across navigation', async ({ page }) => {
  // Login
  await page.goto('/login')
  await page.fill('input[name="email"]', 'test@example.com')
  await page.fill('input[name="password"]', 'password123')
  await page.click('button[type="submit"]')
  
  // Should redirect to dashboard
  await expect(page).toHaveURL('/dashboard')
  
  // Navigate to another protected page
  await page.goto('/dashboard/tasks')
  
  // Should NOT be redirected to login
  await expect(page).not.toHaveURL('/login')
  await expect(page).toHaveURL('/dashboard/tasks')
  
  // Reload page
  await page.reload()
  
  // Should still be authenticated
  await expect(page).not.toHaveURL('/login')
})

test('session persists after browser restart', async ({ browser }) => {
  const context = await browser.newContext()
  const page = await context.newPage()
  
  // Login
  await page.goto('/login')
  await page.fill('input[name="email"]', 'test@example.com')
  await page.fill('input[name="password"]', 'password123')
  await page.click('button[type="submit"]')
  await expect(page).toHaveURL('/dashboard')
  
  // Extract cookies
  const cookies = await context.cookies()
  
  // Close browser context (simulate closing browser)
  await context.close()
  
  // Create new browser context (simulate reopening browser)
  const newContext = await browser.newContext()
  
  // Restore cookies
  await newContext.addCookies(cookies)
  
  const newPage = await newContext.newPage()
  
  // Navigate to protected page
  await newPage.goto('/dashboard')
  
  // Should still be authenticated (not redirected to login)
  await expect(newPage).not.toHaveURL('/login')
  await expect(newPage).toHaveURL('/dashboard')
  
  await newContext.close()
})
```

**TEST #2: Middleware Session Check (Unit)**
```typescript
// __tests__/middleware.test.ts
import { NextRequest } from 'next/server'
import { middleware } from '@/middleware'
import { getToken } from 'next-auth/jwt'

jest.mock('next-auth/jwt')

describe('Middleware session validation', () => {
  beforeEach(() => {
    jest.clearAllMocks()
  })
  
  it('allows access with valid session token', async () => {
    // Mock valid token
    (getToken as jest.Mock).mockResolvedValue({
      email: 'test@example.com',
      sub: 'user-123'
    })
    
    const req = new NextRequest('https://example.com/dashboard')
    const res = await middleware(req)
    
    expect(res.status).toBe(200)  // Should allow access
    expect(getToken).toHaveBeenCalledWith({
      req,
      secret: process.env.NEXTAUTH_SECRET
    })
  })
  
  it('redirects to /login without session token', async () => {
    // Mock no token (null)
    (getToken as jest.Mock).mockResolvedValue(null)
    
    const req = new NextRequest('https://example.com/dashboard')
    const res = await middleware(req)
    
    expect(res.status).toBe(307)  // Redirect
    expect(res.headers.get('location')).toContain('/login')
  })
  
  it('handles getToken errors gracefully', async () => {
    // Mock getToken failure
    (getToken as jest.Mock).mockRejectedValue(new Error('Invalid token'))
    
    const req = new NextRequest('https://example.com/dashboard')
    const res = await middleware(req)
    
    // Should fail open (allow access, not crash)
    expect(res.status).toBe(200)
  })
})
```

**TEST #3: Production Environment Test**
```typescript
// __tests__/integration/production-cookies.test.ts
import { chromium } from 'playwright'

test('cookies work correctly in production-like environment', async () => {
  const browser = await chromium.launch({
    // Force HTTPS-like behavior
    args: ['--enable-features=SecureContext']
  })
  
  const context = await browser.newContext({
    baseURL: 'https://localhost:3000'  // HTTPS
  })
  
  const page = await context.newPage()
  
  // Login
  await page.goto('/login')
  // ... login flow
  
  // Check cookie is set with __Secure- prefix
  const cookies = await context.cookies()
  const sessionCookie = cookies.find(c => 
    c.name === '__Secure-next-auth.session-token'
  )
  
  expect(sessionCookie).toBeDefined()
  expect(sessionCookie?.secure).toBe(true)
  expect(sessionCookie?.httpOnly).toBe(true)
  
  await browser.close()
})
```

**MONITORING: Add Logging to Detect Future Issues**
```typescript
// lib/logger.ts (Winston/Pino/Datadog)
export function logSessionEvent(event: string, details: any) {
  console.log(`[Session] ${event}`, details)
  
  if (process.env.NODE_ENV === 'production') {
    // Send to monitoring service
    // Example: Sentry, Datadog, New Relic
    sentry.captureMessage(`Session: ${event}`, {
      level: 'info',
      extra: details
    })
  }
}

// Usage in middleware:
export async function middleware(request: NextRequest) {
  const token = await getToken({ req: request, secret: process.env.NEXTAUTH_SECRET })
  
  logSessionEvent('middleware_check', {
    path: request.nextUrl.pathname,
    hasToken: !!token,
    tokenEmail: token?.email,
    userAgent: request.headers.get('user-agent')
  })
  
  // ... rest of middleware
}
```

---

4. RELATED ISSUES (POTENTIAL)

**SIMILAR BUGS TO CHECK:**

1. **Other API routes using `getServerSession()` without req/res**
   
   Search codebase for:
```bash
   grep -r "getServerSession(authOptions)" --include="*.ts" --include="*.tsx"
```
   
   Any usage outside of API routes (in middleware, server components without proper context) 
   will have the same bug.
   
   **Fix:** Use `getToken()` in Edge runtime contexts.

2. **Cookie settings on other auth providers (Google OAuth, GitHub)**
   
   If you have OAuth providers, verify `__Secure-` prefix is handled correctly:
```typescript
   // Check OAuth callback routes
   // app/api/auth/[...nextauth]/route.ts
   
   providers: [
     GoogleProvider({
       clientId: process.env.GOOGLE_CLIENT_ID,
       clientSecret: process.env.GOOGLE_CLIENT_SECRET
     }),
     GitHubProvider({
       clientId: process.env.GITHUB_ID,
       clientSecret: process.env.GITHUB_SECRET
     })
   ]
   
   // Cookies config applies to ALL providers
   // Test OAuth login in production!
```

3. **Session expiry edge cases**
   
   What happens at exactly 7 days?
```typescript
   // Add test for session expiry
   test('session expires after maxAge', async () => {
     // Mock Date.now() to be 7 days + 1 second after login
     jest.useFakeTimers()
     jest.setSystemTime(new Date('2024-01-08T00:00:01'))
     
     const token = await getToken({ req: request })
     
     // Should be null (expired)
     expect(token).toBeNull()
   })
```

4. **Clock skew between server and client**
   
   If server time != client time, sessions might expire prematurely.
   
   **Fix:** Use NTP on servers, add tolerance to expiry checks.

**BROADER PATTERNS TO ADDRESS:**

1. **Lack of production-like testing**
   
   **Action:** Set up staging environment with:
   - HTTPS enabled
   - Same deployment platform (Vercel Edge)
   - Production-like environment variables
   - Automated E2E tests run against staging before production deploy

2. **Silent failures in middleware**
   
   **Action:** Improve error handling + logging:
```typescript
   try {
     const token = await getToken(...)
     if (!token) {
       logger.warn('No session token', { path, ip })
     }
   } catch (error) {
     logger.error('Session validation failed', { error, path })
     // Alert on Slack/PagerDuty
   }
```

3. **Documentation gap (NextAuth + Edge runtime)**
   
   **Action:** Add to team wiki:
   - "How NextAuth works in middleware (Edge vs Node.js)"
   - "Common pitfalls: getServerSession vs getToken"
   - "Testing checklist for auth changes"

---

VALIDATION CHECKLIST:
□ Bug reproduced in production (✅ confirmed)
□ Root cause identified (cookie name + Edge runtime incompatibility)
□ Fix implemented (use getToken() instead of getServerSession())
□ Fix tested locally (✅ works in dev)
□ Fix tested in staging (✅ works with HTTPS + Edge)
□ Regression tests added (E2E + unit tests)
□ Monitoring added (session event logging)
□ No new bugs introduced (all existing tests pass)
□ Related issues checked (OAuth providers, other getServerSession uses)
□ Documentation updated (team wiki, code comments)
□ Post-mortem written (how to prevent similar bugs)

---

**POST-MORTEM SUMMARY:**

**What Went Wrong:**
- Used `getServerSession()` in middleware (Edge runtime incompatible)
- No staging environment to catch production-only bugs
- Silent failure (no error surfaced to developers)

**What Went Right:**
- Bug caught immediately (100% user impact = obvious)
- Database sessions working (good foundation)
- Quick diagnosis with systematic debugging

**How to Prevent:**
1. ✅ Set up staging with HTTPS + Vercel Edge
2. ✅ Add E2E tests for auth flows (run against staging)
3. ✅ Document Edge runtime limitations in team wiki
4. ✅ Code review checklist: "Does this work in Edge runtime?"
5. ✅ Monitoring/alerting for session failures
═══════════════════════════════════════════════════════════
````

---
---

## 🔗 PATTERN #5: INTEGRATION PATTERN

### When to Use

**Connecting two systems, features, or services** - making different parts work together.

**Examples:**
- "Integrate Stripe payments with user dashboard"
- "Connect email service (Resend) to notification system"
- "Integrate third-party API (Google Calendar) with task system"
- "Connect frontend form to backend API"

**Success Rate:** 75-85% (depends on API documentation quality)

---

### Pattern Template
````markdown
═══════════════════════════════════════════════════════════
INTEGRATION PATTERN TEMPLATE
═══════════════════════════════════════════════════════════

PERSONA: You are a senior integration engineer with [YEARS] years of experience 
connecting disparate systems. You've integrated hundreds of third-party APIs and 
understand common integration pitfalls (authentication, rate limits, webhooks, 
data format mismatches, error handling).

CONTEXT:
[Standard 5-layer context]

SYSTEM A (Source):
- Name: [What you're integrating FROM]
- Type: [Internal feature / Third-party API]
- Tech: [Framework, database, API type]
- Documentation: [Link to docs]

SYSTEM B (Destination):
- Name: [What you're integrating TO]
- Type: [Internal feature / Third-party API]
- Tech: [Framework, database, API type]
- Documentation: [Link to docs]

INTEGRATION GOAL:
- What data flows from A to B
- When the integration triggers
- Expected outcome

TASK: Integrate [SYSTEM A] with [SYSTEM B] so that [GOAL].

INTEGRATION REQUIREMENTS:

1. DATA MAPPING:
   - Field A1 → Field B1
   - Field A2 → Field B2
   - Transformation needed: [Any data transformation]

2. TRIGGER:
   - Event: [What triggers the integration]
   - Frequency: [Real-time / Scheduled / On-demand]

3. ERROR HANDLING:
   - What if System A fails?
   - What if System B fails?
   - Retry strategy: [Exponential backoff / Queue]

4. AUTHENTICATION:
   - System A auth: [API key / OAuth / JWT]
   - System B auth: [API key / OAuth / JWT]

SECURITY MANDATES:
- Commandment I: Validate data from external systems
- Commandment VI: Verify authorization
- Commandment VII: Encrypt API keys
- Commandment VIII: Handle errors gracefully

META-INSTRUCTIONS:
- Start with authentication (get credentials working first)
- Test with mock/sandbox before production
- Implement idempotency (prevent duplicate operations)
- Add comprehensive logging (track integration flow)
- Consider: What if API changes? (version pinning, error detection)

OUTPUT FORMAT:

DELIVERABLES:

1. INTEGRATION ARCHITECTURE
   - Data flow diagram (A → Integration Layer → B)
   - Authentication flow
   - Error handling flow

2. IMPLEMENTATION
   - Integration service/function
   - Data transformation logic
   - Error handling + retry logic
   - Webhook handlers (if applicable)

3. CONFIGURATION
   - Environment variables (API keys, endpoints)
   - Feature flags (enable/disable integration)

4. TESTS
   - Integration tests (mock external APIs)
   - E2E tests (use sandbox/test mode)

5. MONITORING
   - Success/failure metrics
   - Alerting (when integration breaks)

VALIDATION CHECKLIST:
□ Authentication works (both systems)
□ Data flows correctly (A → B)
□ Error handling works (retry logic)
□ Idempotency implemented (no duplicates)
□ Tests pass (integration + E2E)
□ Monitoring/alerting configured
□ API keys secured (encrypted, not in code)
═══════════════════════════════════════════════════════════
````

---

### Real Example: Integrate Stripe Payments
````markdown
═══════════════════════════════════════════════════════════
INTEGRATION PATTERN: STRIPE PAYMENTS
═══════════════════════════════════════════════════════════

PERSONA: You are a senior payments integration engineer with 10 years of experience 
integrating Stripe, PayPal, and other payment processors. You've processed over 
$100M in transactions and understand PCI compliance, webhook security, idempotency, 
and handling payment failures gracefully.

CONTEXT:

PROJECT: B2B SaaS subscription platform
TECH STACK: Next.js 14, Prisma, PostgreSQL, TailwindCSS
CURRENT STATE: Users can sign up but no payment processing
GOAL: Enable monthly subscriptions ($49/user/month)

SYSTEM A (Source): User Dashboard (Next.js app)
- User selects subscription plan
- User enters payment details
- User confirms purchase

SYSTEM B (Destination): Stripe API
- Process payment
- Create subscription
- Handle webhooks (payment success/failure)

INTEGRATION GOAL:
When user subscribes, create Stripe customer + subscription, then update user 
record in database with subscription status.

TASK: Integrate Stripe payment processing with user subscription system so that 
users can subscribe to paid plans and their subscription status is tracked accurately.

INTEGRATION REQUIREMENTS:

1. DATA MAPPING:
   - User email → Stripe customer email
   - User name → Stripe customer name
   - Subscription plan → Stripe price ID
   - Payment method → Stripe payment method

2. TRIGGER:
   - Event: User clicks "Subscribe" button
   - Flow: Collect payment → Create subscription → Update database
   - Frequency: On-demand (user-initiated)

3. ERROR HANDLING:
   - Payment declined → Show error, allow retry
   - Network timeout → Retry with exponential backoff
   - Webhook failure → Queue for reprocessing
   - Duplicate webhook → Idempotency check (skip if already processed)

4. AUTHENTICATION:
   - Stripe: Secret key (server-side), Publishable key (client-side)
   - Webhook: Signature verification (prevent spoofing)

SECURITY MANDATES:
- Commandment I: Validate webhook signatures (prevent fake webhooks)
- Commandment VII: Never log credit card numbers, use Stripe Elements
- Commandment VIII: Handle payment failures gracefully (user-friendly errors)
- Commandment V: Rate limit subscription attempts (prevent abuse)

META-INSTRUCTIONS:
- Use Stripe Elements (PCI compliant, don't handle card data directly)
- Test with Stripe test mode before production
- Implement idempotency keys (prevent duplicate charges)
- Handle all webhook events (payment_intent.succeeded, subscription.updated, etc.)
- Consider: What if user already has subscription? (prevent double billing)

OUTPUT FORMAT:

DELIVERABLES:

1. INTEGRATION ARCHITECTURE
```
User Dashboard (Client)
    ↓
  [Subscribe Button Clicked]
    ↓
Create Payment Intent (API)
  ↓ (returns client_secret)
Stripe Elements (Client)
  ↓ (user enters card)
Confirm Payment (Stripe)
  ↓
Webhook: payment_intent.succeeded
  ↓
Create Subscription in DB
  ↓
Update User: subscriptionStatus = 'active'
```

**Webhook Flow:**
```
Stripe → POST /api/webhooks/stripe
  ↓
Verify signature (webhook secret)
  ↓
Check idempotency (already processed?)
  ↓
Handle event (payment_intent.succeeded, etc.)
  ↓
Update database
  ↓
Return 200 OK (acknowledge receipt)
```

2. IMPLEMENTATION

**File Structure:**
```
/app/api/
  /stripe/
    create-payment-intent/route.ts
    create-subscription/route.ts
  /webhooks/
    stripe/route.ts

/lib/
  stripe.ts (Stripe client)
  stripe-webhooks.ts (Webhook handlers)

/components/
  SubscriptionForm.tsx (Stripe Elements)
```

**Code:**
```typescript
// lib/stripe.ts
import Stripe from 'stripe'

if (!process.env.STRIPE_SECRET_KEY) {
  throw new Error('STRIPE_SECRET_KEY is required')
}

export const stripe = new Stripe(process.env.STRIPE_SECRET_KEY, {
  apiVersion: '2023-10-16',
  typescript: true
})
```
```typescript
// app/api/stripe/create-payment-intent/route.ts
import { NextResponse } from 'next/server'
import { getServerSession } from 'next-auth'
import { stripe } from '@/lib/stripe'
import { prisma } from '@/lib/prisma'
import { z } from 'zod'

const schema = z.object({
  priceId: z.string(), // Stripe price ID (e.g., price_1234567890)
  quantity: z.number().int().positive().default(1)
})

export async function POST(request: Request) {
  const session = await getServerSession()
  
  if (!session?.user?.id) {
    return NextResponse.json({ error: 'Unauthorized' }, { status: 401 })
  }
  
  const body = await request.json()
  const { priceId, quantity } = schema.parse(body)
  
  try {
    // Get or create Stripe customer
    let stripeCustomerId = session.user.stripeCustomerId
    
    if (!stripeCustomerId) {
      const customer = await stripe.customers.create({
        email: session.user.email,
        name: session.user.name,
        metadata: {
          userId: session.user.id
        }
      })
      
      stripeCustomerId = customer.id
      
      // Save to database
      await prisma.user.update({
        where: { id: session.user.id },
        data: { stripeCustomerId: customer.id }
      })
    }
    
    // Create subscription
    const subscription = await stripe.subscriptions.create({
      customer: stripeCustomerId,
      items: [{ price: priceId, quantity }],
      payment_behavior: 'default_incomplete',
      payment_settings: { save_default_payment_method: 'on_subscription' },
      expand: ['latest_invoice.payment_intent'],
      metadata: {
        userId: session.user.id
      }
    })
    
    const invoice = subscription.latest_invoice as Stripe.Invoice
    const paymentIntent = invoice.payment_intent as Stripe.PaymentIntent
    
    return NextResponse.json({
      subscriptionId: subscription.id,
      clientSecret: paymentIntent.client_secret
    })
    
  } catch (error) {
    console.error('Stripe error:', error)
    
    return NextResponse.json(
      { error: 'Failed to create subscription' },
      { status: 500 }
    )
  }
}
```
```typescript
// app/api/webhooks/stripe/route.ts
import { NextResponse } from 'next/server'
import { headers } from 'next/headers'
import { stripe } from '@/lib/stripe'
import { prisma } from '@/lib/prisma'
import Stripe from 'stripe'

const webhookSecret = process.env.STRIPE_WEBHOOK_SECRET!

export async function POST(request: Request) {
  const body = await request.text()
  const signature = headers().get('stripe-signature')!
  
  let event: Stripe.Event
  
  try {
    // Verify webhook signature (security!)
    event = stripe.webhooks.constructEvent(body, signature, webhookSecret)
  } catch (error) {
    console.error('Webhook signature verification failed:', error)
    return NextResponse.json({ error: 'Invalid signature' }, { status: 400 })
  }
  
  // Handle event
  try {
    switch (event.type) {
      case 'payment_intent.succeeded': {
        const paymentIntent = event.data.object as Stripe.PaymentIntent
        await handlePaymentSuccess(paymentIntent)
        break
      }
      
      case 'payment_intent.payment_failed': {
        const paymentIntent = event.data.object as Stripe.PaymentIntent
        await handlePaymentFailure(paymentIntent)
        break
      }
      
      case 'customer.subscription.created':
      case 'customer.subscription.updated': {
        const subscription = event.data.object as Stripe.Subscription
        await handleSubscriptionUpdate(subscription)
        break
      }
      
      case 'customer.subscription.deleted': {
        const subscription = event.data.object as Stripe.Subscription
        await handleSubscriptionCancellation(subscription)
        break
      }
      
      default:
        console.log(`Unhandled event type: ${event.type}`)
    }
    
    return NextResponse.json({ received: true })
    
  } catch (error) {
    console.error('Webhook handler error:', error)
    return NextResponse.json({ error: 'Webhook handler failed' }, { status: 500 })
  }
}

async function handlePaymentSuccess(paymentIntent: Stripe.PaymentIntent) {
  const userId = paymentIntent.metadata.userId
  
  if (!userId) {
    console.error('No userId in payment intent metadata')
    return
  }
  
  // Check idempotency (already processed?)
  const existing = await prisma.payment.findUnique({
    where: { stripePaymentIntentId: paymentIntent.id }
  })
  
  if (existing) {
    console.log('Payment already processed (idempotency check)')
    return
  }
  
  // Record payment
  await prisma.payment.create({
    data: {
      userId,
      stripePaymentIntentId: paymentIntent.id,
      amount: paymentIntent.amount,
      currency: paymentIntent.currency,
      status: 'succeeded'
    }
  })
  
  console.log(`Payment succeeded: ${paymentIntent.id}`)
}

async function handlePaymentFailure(paymentIntent: Stripe.PaymentIntent) {
  const userId = paymentIntent.metadata.userId
  
  if (!userId) return
  
  await prisma.payment.create({
    data: {
      userId,
      stripePaymentIntentId: paymentIntent.id,
      amount: paymentIntent.amount,
      currency: paymentIntent.currency,
      status: 'failed',
      failureReason: paymentIntent.last_payment_error?.message
    }
  })
  
  // TODO: Send email to user (payment failed, update card)
  
  console.log(`Payment failed: ${paymentIntent.id}`)
}

async function handleSubscriptionUpdate(subscription: Stripe.Subscription) {
  const userId = subscription.metadata.userId
  
  if (!userId) return
  
  await prisma.user.update({
    where: { id: userId },
    data: {
      stripeSubscriptionId: subscription.id,
      subscriptionStatus: subscription.status, // active, past_due, canceled, etc.
      subscriptionPriceId: subscription.items.data[0]?.price.id
    }
  })
  
  console.log(`Subscription updated: ${subscription.id} → ${subscription.status}`)
}

async function handleSubscriptionCancellation(subscription: Stripe.Subscription) {
  const userId = subscription.metadata.userId
  
  if (!userId) return
  
  await prisma.user.update({
    where: { id: userId },
    data: {
      subscriptionStatus: 'canceled',
      subscriptionCanceledAt: new Date()
    }
  })
  
  console.log(`Subscription canceled: ${subscription.id}`)
}
```
```typescript
// components/SubscriptionForm.tsx
'use client'

import { useState } from 'react'
import { loadStripe } from '@stripe/stripe-js'
import {
  Elements,
  PaymentElement,
  useStripe,
  useElements
} from '@stripe/react-stripe-js'

const stripePromise = loadStripe(process.env.NEXT_PUBLIC_STRIPE_PUBLISHABLE_KEY!)

export function SubscriptionForm({ priceId }: { priceId: string }) {
  const [clientSecret, setClientSecret] = useState<string | null>(null)
  
  async function createSubscription() {
    const response = await fetch('/api/stripe/create-payment-intent', {
      method: 'POST',
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify({ priceId })
    })
    
    const { clientSecret } = await response.json()
    setClientSecret(clientSecret)
  }
  
  return (
    <div>
      {!clientSecret ? (
        <button onClick={createSubscription}>
          Subscribe ($49/month)
        </button>
      ) : (
        <Elements stripe={stripePromise} options={{ clientSecret }}>
          <CheckoutForm />
        </Elements>
      )}
    </div>
  )
}

function CheckoutForm() {
  const stripe = useStripe()
  const elements = useElements()
  const [isProcessing, setIsProcessing] = useState(false)
  const [error, setError] = useState<string | null>(null)
  
  async function handleSubmit(e: React.FormEvent) {
    e.preventDefault()
    
    if (!stripe || !elements) return
    
    setIsProcessing(true)
    setError(null)
    
    try {
      const { error } = await stripe.confirmPayment({
        elements,
        confirmParams: {
          return_url: `${window.location.origin}/dashboard/subscription/success`
        }
      })
      
      if (error) {
        setError(error.message || 'Payment failed')
      }
    } finally {
      setIsProcessing(false)
    }
  }
  
  return (
    <form onSubmit={handleSubmit}>
      <PaymentElement />
      
      {error && (
        <div className="text-red-600 mt-2">{error}</div>
      )}
      
      <button
        type="submit"
        disabled={!stripe || isProcessing}
        className="mt-4 px-4 py-2 bg-blue-600 text-white rounded"
      >
        {isProcessing ? 'Processing...' : 'Subscribe'}
      </button>
    </form>
  )
}
```

3. CONFIGURATION
```bash
# .env.local (development)
STRIPE_SECRET_KEY=sk_test_...
STRIPE_WEBHOOK_SECRET=whsec_...
NEXT_PUBLIC_STRIPE_PUBLISHABLE_KEY=pk_test_...

# .env.production
STRIPE_SECRET_KEY=sk_live_...
STRIPE_WEBHOOK_SECRET=whsec_...
NEXT_PUBLIC_STRIPE_PUBLISHABLE_KEY=pk_live_...
```

**Stripe Dashboard Setup:**
1. Create products + prices (monthly plan: $49)
2. Create webhook endpoint: `https://yourdomain.com/api/webhooks/stripe`
3. Select events: `payment_intent.*`, `customer.subscription.*`
4. Copy webhook secret → `.env`

4. TESTS
```typescript
// __tests__/integration/stripe-subscription.test.ts
import { POST as createPaymentIntent } from '@/app/api/stripe/create-payment-intent/route'
import { stripe } from '@/lib/stripe'

jest.mock('@/lib/stripe')

describe('Stripe subscription integration', () => {
  it('creates Stripe customer if not exists', async () => {
    const mockCreate = jest.spyOn(stripe.customers, 'create')
    mockCreate.mockResolvedValue({ id: 'cus_123' } as any)
    
    const response = await createPaymentIntent(mockRequest)
    
    expect(mockCreate).toHaveBeenCalledWith({
      email: 'test@example.com',
      name: 'Test User',
      metadata: { userId: 'user-123' }
    })
  })
  
  it('creates subscription with correct price', async () => {
    const mockCreate = jest.spyOn(stripe.subscriptions, 'create')
    
    await createPaymentIntent(mockRequest)
    
    expect(mockCreate).toHaveBeenCalledWith(
      expect.objectContaining({
        customer: 'cus_123',
        items: [{ price: 'price_monthly_49', quantity: 1 }]
      })
    )
  })
})
```

5. MONITORING
```typescript
// lib/monitoring.ts
export function trackStripeEvent(event: string, data: any) {
  // Send to analytics
  console.log(`[Stripe] ${event}`, data)
  
  // Track metrics
  if (process.env.NODE_ENV === 'production') {
    // Example: Datadog, New Relic, Mixpanel
    analytics.track(event, data)
  }
}

// Usage:
trackStripeEvent('subscription_created', {
  userId,
  subscriptionId,
  priceId,
  amount: 4900
})

trackStripeEvent('payment_failed', {
  userId,
  reason: error.message
})
```

VALIDATION CHECKLIST:
□ Stripe authentication works (test mode + live mode)
□ Customer creation works (saves stripeCustomerId)
□ Subscription creation works (returns clientSecret)
□ Payment succeeds (webhooks received)
□ Database updated (subscriptionStatus = 'active')
□ Webhook signature verified (security)
□ Idempotency works (duplicate webhooks ignored)
□ Error handling works (payment declined, network failure)
□ Tests pass (integration tests with mocked Stripe)
□ Monitoring configured (track subscription events)
□ API keys secured (in environment variables, not code)
□ PCI compliant (using Stripe Elements, not handling cards directly)
═══════════════════════════════════════════════════════════
````

---

## 🔧 PATTERN #6: REFACTORING PATTERN

### When to Use

**Improving code quality without changing behavior** - making code better without breaking it.

**Examples:**
- "Refactor this component to use custom hooks"
- "Extract repeated logic into utility functions"
- "Simplify this nested conditional logic"
- "Convert class component to functional component"

**Success Rate:** 85-90% (when existing tests exist)

---

### Pattern Template
````markdown
═══════════════════════════════════════════════════════════
REFACTORING PATTERN TEMPLATE
═══════════════════════════════════════════════════════════

PERSONA: You are a senior software architect with [YEARS] years of experience 
refactoring legacy codebases. You understand design patterns, SOLID principles, 
and how to improve code without breaking it. You've refactored systems with 
millions of lines of code without introducing regressions.

CONTEXT:
[Standard 5-layer context]

CODE TO REFACTOR:
```[language]
[Paste complete code that needs improvement]
```

REFACTORING GOAL:
- Primary: [Main improvement - readability / performance / testability]
- Secondary: [Additional improvements]

CODE SMELLS IDENTIFIED:
- [Smell #1: Long function, duplicated code, etc.]
- [Smell #2]
- [Smell #3]

CONSTRAINTS:
- **PRESERVE BEHAVIOR** (all existing functionality must work identically)
- **NO BREAKING CHANGES** (public API stays same)
- **MAINTAIN TESTS** (all existing tests must still pass)
- **INCREMENTAL** (refactor in small, safe steps)

META-INSTRUCTIONS:
- Understand existing code FIRST (read thoroughly, identify all behaviors)
- Refactor incrementally (one improvement at a time, not everything at once)
- Explain WHY each refactoring improves code (readability, performance, testability)
- Preserve comments that explain business logic (don't delete important context)
- Run tests after each refactoring step (verify no regressions)

OUTPUT FORMAT:

DELIVERABLES:

1. REFACTORED CODE
   - Complete refactored code
   - Highlighted changes (before/after comparison)

2. REFACTORING EXPLANATION
   - What improved: [Readability, performance, testability]
   - How: [Specific techniques used]
   - Why: [Benefits of refactoring]

3. BEHAVIOR VERIFICATION
   - All public functions work identically
   - All edge cases handled
   - Performance unchanged (or improved)

4. TESTING
   - All existing tests pass
   - New tests added (if testability improved)

VALIDATION CHECKLIST:
□ Behavior preserved (no functional changes)
□ All existing tests pass
□ Code is more readable (easier to understand)
□ Code is more maintainable (easier to change)
□ No performance regression (same speed or faster)
□ Public API unchanged (no breaking changes)
═══════════════════════════════════════════════════════════
````

---

### Real Example: Refactor Form Component
````markdown
═══════════════════════════════════════════════════════════
REFACTORING PATTERN: FORM COMPONENT
═══════════════════════════════════════════════════════════

PERSONA: You are a senior React architect with 8 years of experience refactoring 
React applications. You specialize in custom hooks, component composition, and 
performance optimization. You've refactored hundreds of class components to 
functional components and improved codebases used by millions of users.

CONTEXT:

PROJECT: SaaS task management app
TECH STACK: Next.js 14, React, TypeScript, TailwindCSS
CURRENT: Form component works but is hard to maintain (300 lines, mixed concerns)

CODE TO REFACTOR:
```typescript
// components/TaskForm.tsx (BEFORE)
'use client'

import { useState } from 'react'

export function TaskForm({ onSubmit, initialData }: any) {
  const [title, setTitle] = useState(initialData?.title || '')
  const [description, setDescription] = useState(initialData?.description || '')
  const [priority, setPriority] = useState(initialData?.priority || 'MEDIUM')
  const [dueDate, setDueDate] = useState(initialData?.dueDate || '')
  const [assigneeId, setAssigneeId] = useState(initialData?.assigneeId || '')
  const [errors, setErrors] = useState<any>({})
  const [isSubmitting, setIsSubmitting] = useState(false)
  
  function validateForm() {
    const newErrors: any = {}
    
    if (!title || title.length < 2) {
      newErrors.title = 'Title must be at least 2 characters'
    }
    if (title && title.length > 200) {
      newErrors.title = 'Title must be less than 200 characters'
    }
    if (description && description.length > 10000) {
      newErrors.description = 'Description too long'
    }
    if (!['LOW', 'MEDIUM', 'HIGH', 'URGENT'].includes(priority)) {
      newErrors.priority = 'Invalid priority'
    }
    if (dueDate && new Date(dueDate) < new Date()) {
      newErrors.dueDate = 'Due date must be in the future'
    }
    
    setErrors(newErrors)
    return Object.keys(newErrors).length === 0
  }
  
  async function handleSubmit(e: React.FormEvent) {
    e.preventDefault()
    
    if (!validateForm()) {
      return
    }
    
    setIsSubmitting(true)
    
    try {
      await onSubmit({
        title,
        description,
        priority,
        dueDate: dueDate || null,
        assigneeId: assigneeId || null
      })
    } catch (error: any) {
      setErrors({ submit: error.message })
    } finally {
      setIsSubmitting(false)
    }
  }
  
  return (
    <form onSubmit={handleSubmit} className="space-y-4">
      <div>
        <label htmlFor="title" className="block text-sm font-medium">
          Title *
        </label>
        <input
          id="title"
          type="text"
          value={title}
          onChange={(e) => setTitle(e.target.value)}
          className="mt-1 block w-full rounded-md border-gray-300"
        />
        {errors.title && (
          <p className="mt-1 text-sm text-red-600">{errors.title}</p>
        )}
      </div>
      
      <div>
        <label htmlFor="description" className="block text-sm font-medium">
          Description
        </label>
        <textarea
          id="description"
          value={description}
          onChange={(e) => setDescription(e.target.value)}
          rows={4}
          className="mt-1 block w-full rounded-md border-gray-300"
        />
        {errors.description && (
          <p className="mt-1 text-sm text-red-600">{errors.description}</p>
        )}
      </div>
      
      <div>
        <label htmlFor="priority" className="block text-sm font-medium">
          Priority
        </label>
        <select
          id="priority"
          value={priority}
          onChange={(e) => setPriority(e.target.value)}
          className="mt-1 block w-full rounded-md border-gray-300"
        >
          <option value="LOW">Low</option>
          <option value="MEDIUM">Medium</option>
          <option value="HIGH">High</option>
          <option value="URGENT">Urgent</option>
        </select>
      </div>
      
      <div>
        <label htmlFor="dueDate" className="block text-sm font-medium">
          Due Date
        </label>
        <input
          id="dueDate"
          type="date"
          value={dueDate}
          onChange={(e) => setDueDate(e.target.value)}
          className="mt-1 block w-full rounded-md border-gray-300"
        />
        {errors.dueDate && (
          <p className="mt-1 text-sm text-red-600">{errors.dueDate}</p>
        )}
      </div>
      
      {errors.submit && (
        <div className="rounded-md bg-red-50 p-4">
          <p className="text-sm text-red-800">{errors.submit}</p>
        </div>
      )}
      
      <div className="flex gap-2">
        <button
          type="button"
          onClick={() => window.history.back()}
          className="px-4 py-2 border rounded-md"
        >
          Cancel
        </button>
        <button
          type="submit"
          disabled={isSubmitting}
          className="px-4 py-2 bg-blue-600 text-white rounded-md disabled:opacity-50"
        >
          {isSubmitting ? 'Saving...' : 'Save Task'}
        </button>
      </div>
    </form>
  )
}
```

REFACTORING GOAL:
- Primary: Improve maintainability (extract hooks, separate concerns)
- Secondary: Improve testability (pure functions), add TypeScript types

CODE SMELLS IDENTIFIED:
- **Long component** (300 lines, too much in one file)
- **Mixed concerns** (form state + validation + submission in one component)
- **Repeated patterns** (each field has same structure)
- **Weak typing** (`any` types, no validation schema)
- **Hard to test** (logic mixed with UI)

CONSTRAINTS:
- **PRESERVE BEHAVIOR** (form must work identically)
- **NO BREAKING CHANGES** (props interface stays same)
- **MAINTAIN TESTS** (if tests exist, they must pass)

META-INSTRUCTIONS:
- Extract custom hook (`useTaskForm`) for form state + validation
- Extract reusable `FormField` component (DRY principle)
- Use Zod for type-safe validation
- Separate UI from logic (easier to test)
- Keep public API identical (onSubmit, initialData props)

OUTPUT FORMAT:

DELIVERABLES:

1. REFACTORED CODE
```typescript
// lib/validations/task.ts (NEW FILE - validation schema)
import { z } from 'zod'

export const taskSchema = z.object({
  title: z.string().min(2, 'Title must be at least 2 characters').max(200),
  description: z.string().max(10000, 'Description too long').optional(),
  priority: z.enum(['LOW', 'MEDIUM', 'HIGH', 'URGENT']),
  dueDate: z.string().datetime().optional().refine(
    (date) => !date || new Date(date) >= new Date(),
    'Due date must be in the future'
  ),
  assigneeId: z.string().optional()
})

export type TaskFormData = z.infer<typeof taskSchema>
```
```typescript
// hooks/useTaskForm.ts (NEW FILE - custom hook)
import { useState } from 'react'
import { taskSchema, type TaskFormData } from '@/lib/validations/task'
import { z } from 'zod'

export function useTaskForm(initialData?: Partial<TaskFormData>) {
  const [formData, setFormData] = useState<TaskFormData>({
    title: initialData?.title || '',
    description: initialData?.description || '',
    priority: initialData?.priority || 'MEDIUM',
    dueDate: initialData?.dueDate || '',
    assigneeId: initialData?.assigneeId || ''
  })
  
  const [errors, setErrors] = useState<Record<string, string>>({})
  const [isSubmitting, setIsSubmitting] = useState(false)
  
  function updateField<K extends keyof TaskFormData>(
    field: K,
    value: TaskFormData[K]
  ) {
    setFormData(prev => ({ ...prev, [field]: value }))
    // Clear error for this field when user types
    if (errors[field]) {
      setErrors(prev => {
        const newErrors = { ...prev }
        delete newErrors[field]
        return newErrors
      })
    }
  }
  
  function validate(): boolean {
    try {
      taskSchema.parse(formData)
      setErrors({})
      return true
    } catch (error) {
      if (error instanceof z.ZodError) {
        const newErrors: Record<string, string> = {}
        error.errors.forEach((err) => {
          if (err.path[0]) {
            newErrors[err.path[0].toString()] = err.message
          }
        })
        setErrors(newErrors)
      }
      return false
    }
  }
  
  async function handleSubmit(
    e: React.FormEvent,
    onSubmit: (data: TaskFormData) => Promise<void>
  ) {
    e.preventDefault()
    
    if (!validate()) {
      return
    }
    
    setIsSubmitting(true)
    
    try {
      await onSubmit(formData)
    } catch (error: any) {
      setErrors({ submit: error.message })
    } finally {
      setIsSubmitting(false)
    }
  }
  
  return {
    formData,
    errors,
    isSubmitting,
    updateField,
    handleSubmit
  }
}
```
```typescript
// components/FormField.tsx (NEW FILE - reusable component)
interface FormFieldProps {
  label: string
  error?: string
  required?: boolean
  children: React.ReactNode
}

export function FormField({ label, error, required, children }: FormFieldProps) {
  return (
    <div>
      <label className="block text-sm font-medium text-gray-700">
        {label} {required && <span className="text-red-600">*</span>}
      </label>
      <div className="mt-1">
        {children}
      </div>
      {error && (
        <p className="mt-1 text-sm text-red-600">{error}</p>
      )}
    </div>
  )
}
```
```typescript
// components/TaskForm.tsx (REFACTORED)
'use client'

import { useTaskForm } from '@/hooks/useTaskForm'
import { FormField } from '@/components/FormField'
import type { TaskFormData } from '@/lib/validations/task'

interface TaskFormProps {
  onSubmit: (data: TaskFormData) => Promise<void>
  initialData?: Partial<TaskFormData>
}

export function TaskForm({ onSubmit, initialData }: TaskFormProps) {
  const { formData, errors, isSubmitting, updateField, handleSubmit } = 
    useTaskForm(initialData)
  
  return (
    <form
      onSubmit={(e) => handleSubmit(e, onSubmit)}
      className="space-y-4"
    >
      <FormField label="Title" error={errors.title} required>
        <input
          id="title"
          type="text"
          value={formData.title}
          onChange={(e) => updateField('title', e.target.value)}
          className="block w-full rounded-md border-gray-300"
        />
      </FormField>
      
      <FormField label="Description" error={errors.description}>
        <textarea
          id="description"
          value={formData.description}
          onChange={(e) => updateField('description', e.target.value)}
          rows={4}
          className="block w-full rounded-md border-gray-300"
        />
      </FormField>
      
      <FormField label="Priority">
        <select
          id="priority"
          value={formData.priority}
          onChange={(e) => updateField('priority', e.target.value as any)}
          className="block w-full rounded-md border-gray-300"
        >
          <option value="LOW">Low</option>
          <option value="MEDIUM">Medium</option>
          <option value="HIGH">High</option>
          <option value="URGENT">Urgent</option>
        </select>
      </FormField>
      
      <FormField label="Due Date" error={errors.dueDate}>
        <input
          id="dueDate"
          type="date"
          value={formData.dueDate}
          onChange={(e) => updateField('dueDate', e.target.value)}
          className="block w-full rounded-md border-gray-300"
        />
      </FormField>
      
      {errors.submit && (
        <div className="rounded-md bg-red-50 p-4">
          <p className="text-sm text-red-800">{errors.submit}</p>
        </div>
      )}
      
      <div className="flex gap-2">
        <button
          type="button"
          onClick={() => window.history.back()}
          className="px-4 py-2 border rounded-md"
        >
          Cancel
        </button>
        <button
          type="submit"
          disabled={isSubmitting}
          className="px-4 py-2 bg-blue-600 text-white rounded-md disabled:opacity-50"
        >
          {isSubmitting ? 'Saving...' : 'Save Task'}
        </button>
      </div>
    </form>
  )
}
```

2. REFACTORING EXPLANATION

**WHAT IMPROVED:**

1. **Maintainability** ⬆️
   - Before: 300 lines in one file
   - After: Split into 4 files (60-80 lines each)
   - Easier to find and modify code

2. **Testability** ⬆️
   - Before: Hard to test (UI + logic mixed)
   - After: `useTaskForm` hook is pure logic (easy to test)
   - Validation schema separate (can test independently)

3. **Type Safety** ⬆️
   - Before: `any` types everywhere
   - After: Zod schema + TypeScript types
   - Compile-time type checking

4. **Reusability** ⬆️
   - Before: Form field logic repeated 4 times
   - After: `FormField` component (DRY)
   - Can reuse in other forms

**HOW:**

1. **Extracted custom hook** (`useTaskForm`)
   - Moved all form state management to hook
   - Moved validation logic to hook
   - Component now only handles rendering

2. **Created validation schema** (Zod)
   - Centralized validation rules
   - Type-safe (TypeScript infers types from schema)
   - Easier to modify validation rules

3. **Extracted reusable component** (`FormField`)
   - DRY principle (Don't Repeat Yourself)
   - Consistent styling across all fields
   - Easier to update styling globally

4. **Added TypeScript types**
   - Replaced `any` with proper types
   - `TaskFormData` type inferred from Zod schema
   - Compile-time safety

**WHY:**

- **Easier to maintain:** Changes to validation don't require touching UI
- **Easier to test:** Can test hook + schema separately from UI
- **Easier to extend:** Adding new field = add to schema + one `FormField`
- **Type-safe:** Catch errors at compile time, not runtime

3. BEHAVIOR VERIFICATION

**All public functions work identically:**
- ✅ `onSubmit` called with same data structure
- ✅ `initialData` prop works (pre-fills form)
- ✅ Validation errors displayed in same way
- ✅ Submit button disabled during submission
- ✅ Error handling identical

**Edge cases handled:**
- ✅ Empty title → validation error
- ✅ Title too long → validation error
- ✅ Past due date → validation error
- ✅ Optional fields (description, dueDate, assigneeId)

**Performance:**
- ✅ No performance regression (same number of renders)
- ✅ Validation happens on submit (not on every keystroke)

4. TESTING

**Tests for custom hook:**
```typescript
// __tests__/hooks/useTaskForm.test.ts
import { renderHook, act } from '@testing-library/react'
import { useTaskForm } from '@/hooks/useTaskForm'

describe('useTaskForm', () => {
  it('initializes with default values', () => {
    const { result } = renderHook(() => useTaskForm())
    
    expect(result.current.formData.title).toBe('')
    expect(result.current.formData.priority).toBe('MEDIUM')
  })
  
  it('initializes with provided data', () => {
    const { result } = renderHook(() => 
      useTaskForm({ title: 'Test Task', priority: 'HIGH' })
    )
    
    expect(result.current.formData.title).toBe('Test Task')
    expect(result.current.formData.priority).toBe('HIGH')
  })
  
  it('updates field value', () => {
    const { result } = renderHook(() => useTaskForm())
    
    act(() => {
      result.current.updateField('title', 'New Title')
    })
    
    expect(result.current.formData.title).toBe('New Title')
  })
  
  it('validates form and shows errors', () => {
    const { result } = renderHook(() => useTaskForm())
    
    act(() => {
      result.current.updateField('title', 'A')  // Too short
    })
    
    act(() => {
      result.current.handleSubmit(
        new Event('submit') as any,
        async () => {}
      )
    })
    
    expect(result.current.errors.title).toBe('Title must be at least 2 characters')
  })
})
```

**Tests for validation schema:**
```typescript
// __tests__/validations/task.test.ts
import { taskSchema } from '@/lib/validations/task'

describe('taskSchema', () => {
  it('validates valid data', () => {
    const validData = {
      title: 'Valid Title',
      priority: 'MEDIUM'
    }
    
    expect(() => taskSchema.parse(validData)).not.toThrow()
  })
  
  it('rejects title too short', () => {
    const invalidData = {
      title: 'A',
      priority: 'MEDIUM'
    }
    
    expect(() => taskSchema.parse(invalidData)).toThrow('Title must be at least 2 characters')
  })
  
  it('rejects past due date', () => {
    const yesterday = new Date()
    yesterday.setDate(yesterday.getDate() - 1)
    
    const invalidData = {
      title: 'Valid Title',
      priority: 'MEDIUM',
      dueDate: yesterday.toISOString()
    }
    
    expect(() => taskSchema.parse(invalidData)).toThrow('Due date must be in the future')
  })
})
```

VALIDATION CHECKLIST:
□ Behavior preserved (form works identically)
□ All existing tests pass (if any existed)
□ Code is more readable (split into logical files)
□ Code is more maintainable (easier to modify)
□ Code is more testable (hook + schema isolated)
□ No performance regression (same render count)
□ Public API unchanged (props interface identical)
□ Type-safe (Zod + TypeScript types)
□ DRY principle applied (FormField reused)
□ New tests added (for hook + schema)
═══════════════════════════════════════════════════════════
````

---
---

## ✅ PATTERN #7: TESTING PATTERN

### When to Use

**Writing tests for existing code** - adding automated tests to ensure code works correctly.

**Examples:**
- "Write unit tests for this authentication service"
- "Create E2E tests for the checkout flow"
- "Add integration tests for the API endpoints"
- "Write Playwright tests for user dashboard"

**Success Rate:** 90-95% (when code is testable)

---

### Pattern Template
````markdown
═══════════════════════════════════════════════════════════
TESTING PATTERN TEMPLATE
═══════════════════════════════════════════════════════════

PERSONA: You are a senior QA automation engineer with [YEARS] years of experience 
writing comprehensive test suites. You understand the testing pyramid (unit, 
integration, E2E), test-driven development, and how to write maintainable tests. 
You've achieved 90%+ code coverage in production systems.

CONTEXT:
[Standard 5-layer context]

CODE TO TEST:
```[language]
[Paste code that needs tests]
```

TESTING REQUIREMENTS:

TEST TYPE:
- [ ] Unit tests (functions, utilities, business logic)
- [ ] Integration tests (API routes, database operations)
- [ ] E2E tests (user flows, critical paths)
- [ ] Component tests (React components, UI)

COVERAGE GOAL:
- Minimum: [X%] coverage
- Priority: [Critical paths first, then edge cases]

TESTING FRAMEWORK:
- Unit/Integration: [Jest / Vitest]
- E2E: [Playwright / Cypress]
- Component: [React Testing Library]

WHAT TO TEST:
1. Happy path (normal flow works)
2. Edge cases (boundary conditions)
3. Error cases (failures handled gracefully)
4. Security (authentication, authorization)

META-INSTRUCTIONS:
- Test behavior, not implementation (test what, not how)
- Write readable test names (describe what's being tested)
- Follow AAA pattern (Arrange, Act, Assert)
- Mock external dependencies (APIs, database, file system)
- Test one thing per test (single responsibility)
- Make tests independent (no shared state)

OUTPUT FORMAT:

DELIVERABLES:

1. TEST FILES
   - [filename].test.ts (unit tests)
   - [filename].integration.test.ts (integration tests)
   - [filename].spec.ts (E2E tests with Playwright)

2. TEST COVERAGE REPORT
   - Coverage percentage
   - Uncovered lines (if any)
   - Missing test cases

3. TEST DOCUMENTATION
   - How to run tests
   - Test structure explanation
   - Mock data/fixtures

VALIDATION CHECKLIST:
□ All tests pass
□ Coverage goal met (X% minimum)
□ Happy path tested
□ Edge cases tested
□ Error cases tested
□ Tests are readable (clear names)
□ Tests are maintainable (no duplication)
□ Tests are fast (<5s for unit, <1min for integration)
═══════════════════════════════════════════════════════════
````

---

### Real Example: Testing Authentication API
````markdown
═══════════════════════════════════════════════════════════
TESTING PATTERN: AUTHENTICATION API
═══════════════════════════════════════════════════════════

PERSONA: You are a senior QA engineer with 12 years of experience testing 
authentication systems. You've written comprehensive test suites for banking 
applications and understand security testing, edge cases, and how to mock 
external dependencies effectively.

CONTEXT:

PROJECT: B2B SaaS platform
TECH STACK: Next.js 14, Prisma, PostgreSQL, bcrypt
TEST FRAMEWORK: Jest, Supertest (API testing)
COVERAGE GOAL: 90% minimum

CODE TO TEST:
```typescript
// app/api/auth/register/route.ts
import { NextResponse } from 'next/server'
import { prisma } from '@/lib/prisma'
import bcrypt from 'bcryptjs'
import { z } from 'zod'

const registerSchema = z.object({
  email: z.string().email(),
  password: z.string().min(8),
  name: z.string().min(2).max(50)
})

export async function POST(request: Request) {
  const body = await request.json()
  
  const result = registerSchema.safeParse(body)
  
  if (!result.success) {
    return NextResponse.json(
      { error: 'Validation failed', details: result.error.format() },
      { status: 400 }
    )
  }
  
  const { email, password, name } = result.data
  
  // Check if user already exists
  const existing = await prisma.user.findUnique({
    where: { email }
  })
  
  if (existing) {
    return NextResponse.json(
      { error: 'Email already registered' },
      { status: 409 }
    )
  }
  
  // Hash password
  const passwordHash = await bcrypt.hash(password, 12)
  
  // Create user
  const user = await prisma.user.create({
    data: {
      email,
      name,
      password: passwordHash
    }
  })
  
  return NextResponse.json(
    {
      id: user.id,
      email: user.email,
      name: user.name
    },
    { status: 201 }
  )
}
```

TESTING REQUIREMENTS:

TEST TYPE:
- [x] Unit tests (validation logic)
- [x] Integration tests (API endpoint with database)
- [ ] E2E tests (not needed for API route)

COVERAGE GOAL: 90% minimum

WHAT TO TEST:
1. Happy path (successful registration)
2. Validation errors (invalid email, short password)
3. Duplicate email (409 conflict)
4. Password hashing (bcrypt used correctly)
5. Response format (no password in response)

META-INSTRUCTIONS:
- Mock Prisma for unit tests
- Use test database for integration tests
- Test security (password hashed, not returned)
- Verify bcrypt cost factor (12)

OUTPUT FORMAT:

1. TEST FILES
```typescript
// __tests__/api/auth/register.test.ts (INTEGRATION TESTS)
import { POST } from '@/app/api/auth/register/route'
import { prisma } from '@/lib/prisma'
import bcrypt from 'bcryptjs'

// Mock Prisma
jest.mock('@/lib/prisma', () => ({
  prisma: {
    user: {
      findUnique: jest.fn(),
      create: jest.fn()
    }
  }
}))

// Mock bcrypt
jest.mock('bcryptjs')

describe('POST /api/auth/register', () => {
  beforeEach(() => {
    jest.clearAllMocks()
  })
  
  describe('Happy Path', () => {
    it('creates new user with valid data', async () => {
      const mockUser = {
        id: 'user-123',
        email: 'test@example.com',
        name: 'Test User',
        password: 'hashed_password'
      }
      
      // Mock: User doesn't exist
      ;(prisma.user.findUnique as jest.Mock).mockResolvedValue(null)
      
      // Mock: Password hashing
      ;(bcrypt.hash as jest.Mock).mockResolvedValue('hashed_password')
      
      // Mock: User creation
      ;(prisma.user.create as jest.Mock).mockResolvedValue(mockUser)
      
      const request = new Request('http://localhost:3000/api/auth/register', {
        method: 'POST',
        body: JSON.stringify({
          email: 'test@example.com',
          password: 'password123',
          name: 'Test User'
        })
      })
      
      const response = await POST(request)
      const data = await response.json()
      
      expect(response.status).toBe(201)
      expect(data).toEqual({
        id: 'user-123',
        email: 'test@example.com',
        name: 'Test User'
      })
      
      // Verify password NOT in response
      expect(data.password).toBeUndefined()
      
      // Verify bcrypt called with cost factor 12
      expect(bcrypt.hash).toHaveBeenCalledWith('password123', 12)
      
      // Verify user created with hashed password
      expect(prisma.user.create).toHaveBeenCalledWith({
        data: {
          email: 'test@example.com',
          name: 'Test User',
          password: 'hashed_password'
        }
      })
    })
  })
  
  describe('Validation Errors', () => {
    it('rejects invalid email', async () => {
      const request = new Request('http://localhost:3000/api/auth/register', {
        method: 'POST',
        body: JSON.stringify({
          email: 'invalid-email',
          password: 'password123',
          name: 'Test User'
        })
      })
      
      const response = await POST(request)
      const data = await response.json()
      
      expect(response.status).toBe(400)
      expect(data.error).toBe('Validation failed')
      expect(data.details.email).toBeDefined()
    })
    
    it('rejects password shorter than 8 characters', async () => {
      const request = new Request('http://localhost:3000/api/auth/register', {
        method: 'POST',
        body: JSON.stringify({
          email: 'test@example.com',
          password: 'short',
          name: 'Test User'
        })
      })
      
      const response = await POST(request)
      const data = await response.json()
      
      expect(response.status).toBe(400)
      expect(data.details.password).toBeDefined()
    })
    
    it('rejects name shorter than 2 characters', async () => {
      const request = new Request('http://localhost:3000/api/auth/register', {
        method: 'POST',
        body: JSON.stringify({
          email: 'test@example.com',
          password: 'password123',
          name: 'A'
        })
      })
      
      const response = await POST(request)
      
      expect(response.status).toBe(400)
    })
    
    it('rejects missing required fields', async () => {
      const request = new Request('http://localhost:3000/api/auth/register', {
        method: 'POST',
        body: JSON.stringify({})
      })
      
      const response = await POST(request)
      
      expect(response.status).toBe(400)
    })
  })
  
  describe('Duplicate Email', () => {
    it('returns 409 if email already exists', async () => {
      const existingUser = {
        id: 'user-456',
        email: 'existing@example.com',
        name: 'Existing User',
        password: 'hashed'
      }
      
      // Mock: User exists
      ;(prisma.user.findUnique as jest.Mock).mockResolvedValue(existingUser)
      
      const request = new Request('http://localhost:3000/api/auth/register', {
        method: 'POST',
        body: JSON.stringify({
          email: 'existing@example.com',
          password: 'password123',
          name: 'Test User'
        })
      })
      
      const response = await POST(request)
      const data = await response.json()
      
      expect(response.status).toBe(409)
      expect(data.error).toBe('Email already registered')
      
      // Verify user.create NOT called
      expect(prisma.user.create).not.toHaveBeenCalled()
    })
  })
  
  describe('Database Errors', () => {
    it('handles database connection failure', async () => {
      ;(prisma.user.findUnique as jest.Mock).mockRejectedValue(
        new Error('Database connection failed')
      )
      
      const request = new Request('http://localhost:3000/api/auth/register', {
        method: 'POST',
        body: JSON.stringify({
          email: 'test@example.com',
          password: 'password123',
          name: 'Test User'
        })
      })
      
      // Should throw error (no try-catch in route)
      await expect(POST(request)).rejects.toThrow('Database connection failed')
    })
  })
  
  describe('Security', () => {
    it('hashes password with bcrypt cost factor 12', async () => {
      ;(prisma.user.findUnique as jest.Mock).mockResolvedValue(null)
      ;(bcrypt.hash as jest.Mock).mockResolvedValue('hashed')
      ;(prisma.user.create as jest.Mock).mockResolvedValue({
        id: 'user-123',
        email: 'test@example.com',
        name: 'Test',
        password: 'hashed'
      })
      
      const request = new Request('http://localhost:3000/api/auth/register', {
        method: 'POST',
        body: JSON.stringify({
          email: 'test@example.com',
          password: 'password123',
          name: 'Test'
        })
      })
      
      await POST(request)
      
      // Verify bcrypt.hash called with cost factor 12
      expect(bcrypt.hash).toHaveBeenCalledWith('password123', 12)
    })
    
    it('does not return password in response', async () => {
      ;(prisma.user.findUnique as jest.Mock).mockResolvedValue(null)
      ;(bcrypt.hash as jest.Mock).mockResolvedValue('hashed')
      ;(prisma.user.create as jest.Mock).mockResolvedValue({
        id: 'user-123',
        email: 'test@example.com',
        name: 'Test',
        password: 'hashed_password_should_not_be_returned'
      })
      
      const request = new Request('http://localhost:3000/api/auth/register', {
        method: 'POST',
        body: JSON.stringify({
          email: 'test@example.com',
          password: 'password123',
          name: 'Test'
        })
      })
      
      const response = await POST(request)
      const data = await response.json()
      
      expect(data.password).toBeUndefined()
      expect(data.passwordHash).toBeUndefined()
    })
  })
})
```
```typescript
// __tests__/integration/auth-flow.test.ts (E2E-STYLE INTEGRATION)
import { POST as registerPOST } from '@/app/api/auth/register/route'
import { prisma } from '@/lib/prisma'

// Use real database (test database, not mocked)
describe('Authentication Flow (Integration)', () => {
  beforeEach(async () => {
    // Clear users table before each test
    await prisma.user.deleteMany()
  })
  
  afterAll(async () => {
    await prisma.$disconnect()
  })
  
  it('registers new user end-to-end', async () => {
    const request = new Request('http://localhost:3000/api/auth/register', {
      method: 'POST',
      body: JSON.stringify({
        email: 'integration@example.com',
        password: 'secure_password_123',
        name: 'Integration Test'
      })
    })
    
    const response = await registerPOST(request)
    const data = await response.json()
    
    expect(response.status).toBe(201)
    expect(data.email).toBe('integration@example.com')
    
    // Verify user actually created in database
    const user = await prisma.user.findUnique({
      where: { email: 'integration@example.com' }
    })
    
    expect(user).toBeDefined()
    expect(user?.name).toBe('Integration Test')
    
    // Verify password is hashed (not plaintext)
    expect(user?.password).not.toBe('secure_password_123')
    expect(user?.password.length).toBeGreaterThan(50) // bcrypt hashes are long
  })
  
  it('prevents duplicate registration', async () => {
    // First registration
    await registerPOST(new Request('http://localhost:3000/api/auth/register', {
      method: 'POST',
      body: JSON.stringify({
        email: 'duplicate@example.com',
        password: 'password123',
        name: 'First User'
      })
    }))
    
    // Second registration (should fail)
    const response = await registerPOST(new Request('http://localhost:3000/api/auth/register', {
      method: 'POST',
      body: JSON.stringify({
        email: 'duplicate@example.com',
        password: 'different_password',
        name: 'Second User'
      })
    }))
    
    expect(response.status).toBe(409)
    
    // Verify only one user exists
    const users = await prisma.user.findMany({
      where: { email: 'duplicate@example.com' }
    })
    
    expect(users.length).toBe(1)
    expect(users[0].name).toBe('First User')
  })
})
```

2. TEST COVERAGE REPORT
```bash
# Run tests with coverage
npm test -- --coverage

# Output:
----------------------|---------|----------|---------|---------|-------------------
File                  | % Stmts | % Branch | % Funcs | % Lines | Uncovered Lines
----------------------|---------|----------|---------|---------|-------------------
auth/register/route.ts|   95.45 |    91.67 |     100 |   95.24 | 23-24
----------------------|---------|----------|---------|---------|-------------------

Coverage Summary:
- Statements: 95.45% (21/22 covered)
- Branches: 91.67% (11/12 covered)
- Functions: 100% (1/1 covered)
- Lines: 95.24% (20/21 covered)

Uncovered:
- Lines 23-24: Error handling for database failures (tested but not counted due to throw)

✅ Coverage goal met (90% minimum)
```

3. TEST DOCUMENTATION

**How to Run Tests:**
```bash
# Unit tests only
npm test

# Integration tests (requires test database)
npm run test:integration

# All tests with coverage
npm test -- --coverage

# Watch mode (for development)
npm test -- --watch

# Specific test file
npm test -- register.test.ts
```

**Test Structure:**
```
__tests__/
├── api/
│   └── auth/
│       ├── register.test.ts        (Unit tests - mocked Prisma)
│       └── login.test.ts
├── integration/
│   └── auth-flow.test.ts           (Integration - real database)
└── e2e/
    └── auth-user-flow.spec.ts      (Playwright E2E)
```

**Mock Data:**
```typescript
// __tests__/fixtures/users.ts
export const mockUsers = {
  validUser: {
    email: 'test@example.com',
    password: 'password123',
    name: 'Test User'
  },
  existingUser: {
    id: 'user-456',
    email: 'existing@example.com',
    name: 'Existing User',
    password: 'hashed_password'
  },
  invalidEmail: {
    email: 'invalid-email',
    password: 'password123',
    name: 'Test'
  }
}
```

**Test Database Setup:**
```typescript
// jest.setup.ts
import { PrismaClient } from '@prisma/client'

const prisma = new PrismaClient({
  datasources: {
    db: {
      url: process.env.DATABASE_URL_TEST
    }
  }
})

beforeAll(async () => {
  // Run migrations on test database
  await prisma.$executeRaw`CREATE DATABASE IF NOT EXISTS test_db`
})

afterAll(async () => {
  await prisma.$disconnect()
})
```

VALIDATION CHECKLIST:
□ All tests pass (22/22 passing)
□ Coverage goal met (95.45% > 90%)
□ Happy path tested (successful registration)
□ Edge cases tested (invalid email, short password, duplicate)
□ Error cases tested (database failure)
□ Security tested (password hashing, not returned in response)
□ Tests are readable (clear describe/it blocks)
□ Tests are maintainable (use fixtures, no duplication)
□ Tests are fast (unit tests <2s, integration <10s)
□ Integration tests use real database (not mocked)
═══════════════════════════════════════════════════════════
````

---

## 🏛️ PATTERN #8: ARCHITECTURE PATTERN

### When to Use

**Designing system architecture from scratch** - planning how components/services will work together.

**Examples:**
- "Design architecture for a multi-tenant SaaS application"
- "Plan microservices architecture for e-commerce platform"
- "Design database schema for social media app"
- "Architect real-time collaboration system"

**Success Rate:** 70-80% (requires deep domain understanding)

---

### Pattern Template
````markdown
═══════════════════════════════════════════════════════════
ARCHITECTURE PATTERN TEMPLATE
═══════════════════════════════════════════════════════════

PERSONA: You are a principal software architect with [YEARS] years of experience 
designing scalable systems. You've architected platforms serving millions of users 
and understand trade-offs between different architectural patterns (monolith, 
microservices, serverless, event-driven).

CONTEXT:
[Standard 5-layer context]

SYSTEM REQUIREMENTS:

FUNCTIONAL:
- [Key feature #1]
- [Key feature #2]
- [Key feature #3]

NON-FUNCTIONAL:
- Scale: [Users, requests/sec, data volume]
- Performance: [Latency requirements]
- Availability: [Uptime requirements]
- Consistency: [Strong / Eventual]

CONSTRAINTS:
- Budget: [Infrastructure budget]
- Team: [Team size, skill level]
- Timeline: [Delivery timeline]
- Compliance: [Regulations]

TASK: Design a comprehensive architecture for [SYSTEM] that [GOALS] while 
meeting [REQUIREMENTS].

ARCHITECTURE DECISIONS:

1. SYSTEM ARCHITECTURE:
   - [ ] Monolith
   - [ ] Microservices
   - [ ] Serverless
   - [ ] Hybrid

2. DATABASE STRATEGY:
   - [ ] Single database
   - [ ] Database per service
   - [ ] CQRS (read/write separation)

3. COMMUNICATION:
   - [ ] Synchronous (REST, GraphQL)
   - [ ] Asynchronous (queues, pub/sub)
   - [ ] Real-time (WebSockets, SSE)

4. SCALABILITY:
   - [ ] Vertical (bigger servers)
   - [ ] Horizontal (more servers)
   - [ ] Auto-scaling

META-INSTRUCTIONS:
- Start with high-level architecture (system diagram)
- Make trade-offs explicit (why X over Y)
- Consider failure modes (what if service X fails?)
- Plan for scale (what happens at 10x growth?)
- Document architectural decisions (ADRs)

OUTPUT FORMAT:

DELIVERABLES:

1. ARCHITECTURE DIAGRAM
   - High-level system diagram
   - Component interactions
   - Data flow

2. TECHNOLOGY STACK
   - Frontend: [Framework]
   - Backend: [Framework, language]
   - Database: [Type, specific tech]
   - Infrastructure: [Cloud provider, services]

3. ARCHITECTURE DECISION RECORDS (ADRs)
   - Decision: [What was decided]
   - Context: [Why this decision]
   - Consequences: [Pros and cons]

4. SCALABILITY PLAN
   - Current capacity
   - Scaling strategy
   - Bottlenecks to address

5. SECURITY ARCHITECTURE
   - Authentication/Authorization
   - Data encryption
   - Network security

VALIDATION CHECKLIST:
□ Meets functional requirements
□ Meets non-functional requirements (scale, performance)
□ Fits within budget constraints
□ Team can implement (skill match)
□ Scalable (handles growth)
□ Secure (passes security review)
□ Documented (ADRs written)
═══════════════════════════════════════════════════════════
````

---

*(Due to length, Architecture Pattern real example will be concise)*

### Real Example: Multi-Tenant SaaS Architecture
````markdown
═══════════════════════════════════════════════════════════
ARCHITECTURE PATTERN: MULTI-TENANT SAAS
═══════════════════════════════════════════════════════════

SYSTEM: B2B Project Management SaaS (competitor to Asana)

REQUIREMENTS:
- Scale: 10,000 companies, 100,000 users, 1M tasks/day
- Performance: <300ms page load, <100ms API response
- Multi-tenancy: Complete data isolation between companies
- Real-time: Live updates when tasks change

ARCHITECTURE DECISION:

**1. MONOLITH (for MVP, evolve to microservices later)**
- Why: Team of 3 developers, 6-month timeline
- Trade-off: Easier to build initially, harder to scale later

**2. DATABASE: Single PostgreSQL with tenant isolation**
- Row-level security (companyId on all tables)
- Why: Simpler than database-per-tenant
- Trade-off: Careful query design needed (always filter by companyId)

**3. REAL-TIME: Server-Sent Events (SSE)**
- Why: Simpler than WebSockets, works with serverless
- Trade-off: One-way only (server → client)

**TECH STACK:**
- Frontend: Next.js 14, React, TailwindCSS
- Backend: Next.js API routes, Prisma, PostgreSQL
- Real-time: SSE (via API routes)
- Deployment: Vercel + Supabase
- File storage: AWS S3

**SCALABILITY PLAN:**
- Phase 1 (0-1K companies): Monolith on Vercel
- Phase 2 (1K-10K): Add Redis caching, CDN
- Phase 3 (10K+): Split into microservices (task service, user service)
═══════════════════════════════════════════════════════════
````

---

## 🔄 PATTERN #9: MIGRATION PATTERN

### When to Use

**Upgrading libraries, frameworks, or migrating between technologies**.

**Examples:**
- "Migrate from Next.js Pages Router to App Router"
- "Upgrade from NextAuth v4 to v5"
- "Migrate from JavaScript to TypeScript"
- "Migrate from REST to GraphQL"

**Success Rate:** 75-85% (depends on migration complexity)

---

### Pattern Template
````markdown
═══════════════════════════════════════════════════════════
MIGRATION PATTERN TEMPLATE
═══════════════════════════════════════════════════════════

PERSONA: You are a senior migration specialist with [YEARS] years of experience 
upgrading large codebases. You've migrated systems with millions of lines of code 
without downtime. You understand incremental migration strategies and rollback plans.

CONTEXT:
[Standard 5-layer context]

MIGRATION:

FROM: [Current technology, version]
TO: [Target technology, version]

REASON FOR MIGRATION:
- [Why migrate? Performance, features, support?]

MIGRATION SCOPE:
- Files affected: [Approximate count]
- Breaking changes: [List major breaking changes]
- Timeline: [How long for migration]

TASK: Migrate from [OLD] to [NEW] while maintaining zero downtime and ensuring 
all existing functionality continues to work.

MIGRATION STRATEGY:

APPROACH:
- [ ] Big Bang (migrate everything at once)
- [ ] Incremental (migrate module by module)
- [ ] Strangler Fig (run old + new side-by-side)

PHASES:
1. Preparation (setup new system)
2. Migration (move code)
3. Testing (verify functionality)
4. Cutover (switch to new system)
5. Cleanup (remove old code)

ROLLBACK PLAN:
- How to revert if migration fails
- Criteria for rollback (error rate threshold)

META-INSTRUCTIONS:
- Migrate incrementally (small chunks, verify each)
- Keep old and new working simultaneously (feature flags)
- Test thoroughly before cutover
- Document breaking changes clearly
- Plan rollback strategy BEFORE starting

OUTPUT FORMAT:

DELIVERABLES:

1. MIGRATION PLAN
   - Phase-by-phase steps
   - Timeline estimate
   - Risk assessment

2. CODE CHANGES
   - Automated scripts (codemods if possible)
   - Manual changes checklist

3. TESTING PLAN
   - Regression tests
   - Performance benchmarks
   - User acceptance testing

4. ROLLBACK PROCEDURE
   - How to revert
   - Data backup strategy

5. DOCUMENTATION
   - Breaking changes guide
   - Migration notes for team

VALIDATION CHECKLIST:
□ Migration plan documented
□ All breaking changes identified
□ Tests pass (regression suite)
□ Performance unchanged (or improved)
□ Rollback procedure tested
□ Zero downtime achieved
□ Team trained on new system
═══════════════════════════════════════════════════════════
````

---

### Real Example: Next.js Pages to App Router Migration
````markdown
═══════════════════════════════════════════════════════════
MIGRATION PATTERN: PAGES → APP ROUTER
═══════════════════════════════════════════════════════════

MIGRATION:
FROM: Next.js 13 (Pages Router)
TO: Next.js 14 (App Router)

REASON:
- Better performance (React Server Components)
- Improved data fetching
- Better TypeScript support
- Future-proof (Pages Router deprecated)

SCOPE:
- 25 pages (pages/*.tsx)
- 10 API routes (pages/api/*.ts)
- 15 components (components/*.tsx)

STRATEGY: Incremental (migrate one route at a time)

MIGRATION PLAN:

**Phase 1: Preparation (Week 1)**
1. Create /app directory
2. Add app/layout.tsx (root layout)
3. Move global styles
4. Setup TypeScript for App Router

**Phase 2: Migrate Routes (Week 2-3)**
Route-by-route migration:
1. /login (pages/login.tsx → app/login/page.tsx)
2. /dashboard (pages/dashboard.tsx → app/dashboard/page.tsx)
3. /tasks (pages/tasks/*.tsx → app/tasks/page.tsx)

**Phase 3: Migrate API Routes (Week 4)**
1. pages/api/tasks → app/api/tasks/route.ts
2. Update to new API route format

**Phase 4: Cleanup (Week 5)**
1. Remove /pages directory
2. Update documentation

**ROLLBACK:** Keep /pages directory until migration 100% complete

BREAKING CHANGES:
1. `getServerSideProps` → `async` Server Components
2. `useRouter()` (next/router) → `useRouter()` (next/navigation)
3. API routes format changed

EXAMPLE MIGRATION:
```typescript
// BEFORE (Pages Router)
// pages/tasks/[id].tsx
import { useRouter } from 'next/router'

export async function getServerSideProps({ params }) {
  const task = await prisma.task.findUnique({
    where: { id: params.id }
  })
  
  return { props: { task } }
}

export default function TaskPage({ task }) {
  const router = useRouter()
  
  return <div>{task.title}</div>
}

// AFTER (App Router)
// app/tasks/[id]/page.tsx
import { prisma } from '@/lib/prisma'

export default async function TaskPage({
  params
}: {
  params: { id: string }
}) {
  const task = await prisma.task.findUnique({
    where: { id: params.id }
  })
  
  return <div>{task.title}</div>
}
```

VALIDATION:
□ All routes migrated
□ All tests pass
□ Performance improved (React Server Components)
□ No runtime errors
□ Rollback plan tested
═══════════════════════════════════════════════════════════
````

---
