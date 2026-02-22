# 🔬 ARTICLE I: ANATOMY OF A CONSTITUTIONAL PROMPT

**The 7-Component Structure That Generates Production-Ready Code**

---

## 📜 CONSTITUTIONAL AUTHORITY

This article defines the **mandatory 7-component structure** that EVERY constitutional prompt must follow. This structure is the foundation of MVCA's prompt generation and can be used manually by advanced users.

**Legal Force:**
- ✅ MVCA **MUST** include all 7 components in every generated prompt
- ✅ Each component **SHALL** follow the specifications defined herein
- ✅ Components **MUST** appear in the specified order
- ✅ Manual prompts **MAY** follow this structure (optional but recommended)
- ✅ Component omission **SHALL** trigger prompt quality score reduction

**Constitutional Principle:**
> A well-structured prompt is a constitutional contract between user and AI.  
> Each component serves a specific purpose in ensuring quality output.

---

## 🎯 THE 7-COMPONENT STRUCTURE

### Overview
```
═══════════════════════════════════════════════════════════
CONSTITUTIONAL PROMPT STRUCTURE
═══════════════════════════════════════════════════════════

COMPONENT 1: PERSONA ASSIGNMENT
├─ Define AI's role and expertise
├─ Set expectations for output quality
└─ Establish domain knowledge context

COMPONENT 2: CONTEXT INJECTION
├─ Project Identity (what is this?)
├─ Business Context (why does it exist?)
├─ Technical Context (how is it built?)
├─ Feature Context (what are we building?)
└─ Constraints Context (what limits us?)

COMPONENT 3: TASK DEFINITION
├─ Clear, specific task description
├─ Scope boundaries (IN/OUT)
├─ Success criteria
└─ Phase identification

COMPONENT 4: REQUIREMENTS SPECIFICATION
├─ Functional requirements (what it must do)
├─ Technical requirements (how to implement)
├─ Non-functional requirements (quality attributes)
├─ Edge cases (unusual scenarios)
└─ Data requirements (schema, models)

COMPONENT 5: SECURITY MANDATES
├─ Constitutional commandments (I-X)
├─ OWASP Top 10 compliance
├─ WCAG 2.1 AA compliance
├─ Implementation guidelines
└─ Violation warnings

COMPONENT 6: META-INSTRUCTIONS
├─ Thinking process (step-by-step approach)
├─ Constraints (what NOT to do)
├─ Best practices (patterns to follow)
├─ Dependencies (what must exist first)
└─ References (documentation links)

COMPONENT 7: OUTPUT FORMAT & VALIDATION
├─ Deliverables list (specific files)
├─ Code format expectations
├─ Validation checklist
├─ Next steps guidance
└─ Expected output structure

═══════════════════════════════════════════════════════════
```

---

## 🎭 COMPONENT 1: PERSONA ASSIGNMENT

### Purpose

Define the AI's role, expertise level, and domain knowledge to set appropriate expectations for output quality.

### Rationale

**Why this matters:**
- AI adapts its language and technical depth based on assigned persona
- Expertise level determines code complexity and best practices applied
- Domain specialization ensures relevant patterns and solutions

**Without persona:**
```
"Create authentication" → Generic, potentially outdated or insecure solution
```

**With persona:**
```
"You are a senior full-stack developer specializing in Next.js authentication 
with 10+ years experience and deep OWASP knowledge"
→ Production-grade NextAuth.js implementation with security best practices
```

---

### Structure
```markdown
COMPONENT 1: PERSONA ASSIGNMENT
────────────────────────────────────────────────────────

You are a [role] specializing in [domain] with [experience level].

Your expertise includes:
- [Specific skill 1]
- [Specific skill 2]
- [Specific skill 3]
- [Specific skill 4]

You write production-ready code that prioritizes:
1. [Priority 1] (constitutional mandate)
2. [Priority 2] (constitutional mandate)
3. [Priority 3] (best practice)
4. [Priority 4] (best practice)
```

---

### Examples

#### Example 1: Authentication Feature
```markdown
COMPONENT 1: PERSONA ASSIGNMENT
────────────────────────────────────────────────────────

You are a senior full-stack developer specializing in Next.js 
authentication and security with 10+ years of experience.

Your expertise includes:
- NextAuth.js v5 (latest patterns and best practices)
- OWASP Top 10 compliance (especially A01, A02, A07, A08)
- Production-grade authentication systems (100K+ users)
- Prisma ORM + PostgreSQL for session storage
- Secure OAuth flows (Google, GitHub, Microsoft)

You write production-ready code that prioritizes:
1. Security (constitutional mandate - Commandment III, IV, V)
2. Accessibility (WCAG 2.1 AA - Commandment IX)
3. Performance (fast, optimized, scalable)
4. Maintainability (clean, documented, testable)
```

#### Example 2: Database Schema Design
```markdown
COMPONENT 1: PERSONA ASSIGNMENT
────────────────────────────────────────────────────────

You are a database architect with expertise in Prisma ORM, 
PostgreSQL optimization, and schema design with 8+ years experience.

Your expertise includes:
- Relational database modeling (normalization, denormalization)
- Prisma schema best practices (indexes, constraints, relations)
- Query optimization and performance tuning
- Data integrity and referential integrity enforcement
- Migration strategies for production databases

You write production-ready schemas that prioritize:
1. Data integrity (foreign keys, constraints, validation)
2. Query performance (strategic indexing)
3. Scalability (handles growth without restructuring)
4. Maintainability (clear naming, documentation)
```

#### Example 3: UI Component
```markdown
COMPONENT 1: PERSONA ASSIGNMENT
────────────────────────────────────────────────────────

You are a frontend developer specializing in React, accessibility 
(WCAG AA), and user experience with 7+ years of experience.

Your expertise includes:
- React 18+ (hooks, server components, suspense)
- Tailwind CSS (utility-first styling)
- Accessibility standards (WCAG 2.1 Level AA)
- Semantic HTML and ARIA attributes
- Responsive design (mobile-first approach)
- TypeScript strict mode

You write production-ready components that prioritize:
1. Accessibility (keyboard nav, screen readers - Commandment IX)
2. Performance (optimized rendering, lazy loading)
3. User experience (intuitive, responsive, fast)
4. Code quality (TypeScript, documented, testable)
```

---

### Persona Selection Guide

**Decision Tree:**
```
FEATURE TYPE = Authentication?
├─ Persona: "Senior full-stack developer specializing in authentication"
└─ Expertise: NextAuth.js, OWASP, bcrypt, session management

FEATURE TYPE = Database?
├─ Persona: "Database architect"
└─ Expertise: Prisma, PostgreSQL, indexing, optimization

FEATURE TYPE = UI Component?
├─ Persona: "Frontend developer"
└─ Expertise: React, Tailwind, WCAG AA, responsive design

FEATURE TYPE = API Endpoint?
├─ Persona: "Backend engineer"
└─ Expertise: Next.js API routes, validation, error handling

FEATURE TYPE = Performance?
├─ Persona: "Performance engineer"
└─ Expertise: Lighthouse, Core Web Vitals, optimization

FEATURE TYPE = Security?
├─ Persona: "Security engineer"
└─ Expertise: OWASP Top 10, penetration testing, threat modeling

DEFAULT:
├─ Persona: "Senior full-stack developer"
└─ Expertise: General web development best practices
```

---

### Common Mistakes
```markdown
❌ MISTAKE #1: Too generic
"You are a developer"
Problem: AI doesn't know what level of expertise to apply

✅ CORRECT:
"You are a senior full-stack developer with 10+ years experience..."

---

❌ MISTAKE #2: No domain specification
"You are an expert programmer"
Problem: Expert in what? Python? Java? React? AI guesses.

✅ CORRECT:
"You are a senior full-stack developer specializing in Next.js..."

---

❌ MISTAKE #3: Missing priorities
Persona defined but no indication of what matters most

✅ CORRECT:
"You write production-ready code that prioritizes:
1. Security (constitutional mandate)
2. Accessibility (WCAG AA)..."

---

❌ MISTAKE #4: Conflicting expertise
"You are a React expert specializing in Vue.js"
Problem: Contradictory specializations confuse AI

✅ CORRECT:
Choose ONE primary specialization, others as secondary
```

---

### Validation Checklist
```markdown
PERSONA COMPONENT QUALITY CHECKLIST:

□ Role clearly defined (senior/mid/junior + domain)
□ Experience level specified (years or project scale)
□ 3-5 specific expertise areas listed
□ 4 priorities listed (2 constitutional, 2 best practice)
□ No contradictions in expertise claims
□ Appropriate for the feature being built
□ Sets realistic expectations (not claiming omniscience)

GRADING:
7/7 checked: ⭐⭐⭐⭐⭐ Excellent persona
5-6 checked: ⭐⭐⭐⭐ Good persona
3-4 checked: ⭐⭐⭐ Acceptable
<3 checked: ⭐⭐ Needs improvement
```

---

## 🌍 COMPONENT 2: CONTEXT INJECTION

### Purpose

Provide the AI with comprehensive context about the project, business, technology, feature, and constraints using the 5-Layer Context Stack.

### The 5-Layer Context Stack
```
LAYER 1: PROJECT IDENTITY
├─ What: Project name, type, industry
├─ Who: Target users, user personas
├─ Stage: MVP, Production, Scale
└─ Scope: App complexity, scale expectations

LAYER 2: BUSINESS CONTEXT
├─ Problem: What problem does this solve?
├─ Solution: How does it solve it?
├─ Value Proposition: Why will users adopt?
└─ Success Metrics: How do we measure success?

LAYER 3: TECHNICAL CONTEXT
├─ Stack: Framework, language, database, ORM
├─ Architecture: Folder structure, conventions
├─ Existing Code: Current implementation state
└─ Coding Standards: Style guide, naming conventions

LAYER 4: FEATURE CONTEXT
├─ User Story: As a [user], I want [action], so that [benefit]
├─ Acceptance Criteria: Given-When-Then scenarios
├─ Edge Cases: Unusual scenarios to handle
└─ Integration Points: How this connects to existing features

LAYER 5: CONSTRAINTS CONTEXT
├─ Budget: Hosting/service costs limits
├─ Timeline: When must this be complete?
├─ Compliance: GDPR, HIPAA, PCI DSS requirements?
└─ Technical Debt: Known issues to work around
```

---

### Structure
```markdown
COMPONENT 2: CONTEXT INJECTION
────────────────────────────────────────────────────────

LAYER 1: PROJECT IDENTITY
- Name: [Project Name]
- Type: [Web App / Mobile App / API / CLI / etc.]
- Industry: [E-commerce / SaaS / Healthcare / etc.]
- Stage: [MVP / Production / Scale]
- Target Users: [Who uses this? Demographics/psychographics]

LAYER 2: BUSINESS CONTEXT (optional - include if relevant)
- Problem: [What pain point are we solving?]
- Solution: [How does our app solve it?]
- Value Proposition: [Why will users choose us?]
- Success Metrics: [How do we measure success?]

LAYER 3: TECHNICAL CONTEXT
- Framework: [Next.js 15.0.3 / React / etc.]
- Language: [TypeScript 5.3.3 / JavaScript / etc.]
- Database: [PostgreSQL 16 / MongoDB / etc.]
- ORM: [Prisma 5.7.1 / Drizzle / etc.]
- Hosting: [Vercel / AWS / etc.]
- Authentication: [NextAuth.js v5 / Clerk / etc.]

PROJECT STRUCTURE:
```
app/
├── (auth)/
│   ├── login/
│   └── register/
├── (dashboard)/
│   └── dashboard/
├── api/
│   └── [endpoints]
lib/
├── db.ts
└── [utilities]
```

CODING CONVENTIONS:
- Imports: Absolute with @ alias (e.g., @/lib/db)
- Naming: camelCase functions, PascalCase components
- Async: Always async/await (never .then/.catch)
- Error handling: try/catch blocks required

LAYER 4: FEATURE CONTEXT
User Story:
"As a [user type], I want to [action], so that [benefit]"

Acceptance Criteria:
1. Given [context], When [action], Then [result]
2. Given [context], When [action], Then [result]
3. [...]

Edge Cases:
- [Unusual scenario 1]
- [Unusual scenario 2]

LAYER 5: CONSTRAINTS CONTEXT (optional - include if applicable)
- Budget: [e.g., <$50/month for all services]
- Timeline: [e.g., Complete in 2 days]
- Compliance: [e.g., GDPR for EU users, HIPAA for health data]
- Known Issues: [e.g., Workaround needed for X]
```

---

### Layer-by-Layer Deep Dive

#### Layer 1: Project Identity

**Purpose:** Help AI understand the project at a high level

**Required Information:**
```markdown
✓ Project Name: TaskMaster Pro
✓ Type: Web Application (SaaS)
✓ Industry: Productivity / Project Management
✓ Stage: MVP Development (launching in 8 weeks)
✓ Target Users: Freelancers and small teams (1-10 people)
```

**Why this matters:**
- Industry context influences UI/UX decisions (B2B vs B2C)
- Stage determines complexity (MVP = simple, Production = robust)
- Target users influence accessibility and UX priorities

---

#### Layer 2: Business Context

**Purpose:** Connect technical decisions to business goals

**When to include:**
- ✓ When building customer-facing features
- ✓ When business logic is non-obvious
- ✓ When prioritization needs justification
- ✗ Skip for purely technical tasks (performance optimization)

**Example:**
```markdown
LAYER 2: BUSINESS CONTEXT

Problem: Freelancers lose projects to competitors on Upwork due to 
race-to-the-bottom pricing. Quality work undervalued.

Solution: Curated marketplace where only vetted freelancers can join.
Minimum rate $75/hour enforced. Focus on quality over price.

Value Proposition: 
- For Freelancers: Higher rates, quality-focused clients
- For Clients: Vetted talent, predictable quality, less risk

Success Metrics:
- 100 vetted freelancers by month 3
- 50 projects posted by month 6
- $10K GMV (Gross Merchandise Value) by month 6
```

**How AI uses this:**
- Understands why $75/hour minimum is enforced (validation logic)
- Knows vetting is critical (prioritizes admin approval features)
- Focuses on quality indicators (reviews, portfolios) over quantity

---

#### Layer 3: Technical Context

**Purpose:** Ensure AI generates code that fits existing codebase

**Critical Information:**
```markdown
✓ Framework + Version: Next.js 15.0.3 (App Router)
✓ Language + Version: TypeScript 5.3.3 (strict mode)
✓ Database + Version: PostgreSQL 16
✓ ORM: Prisma 5.7.1
✓ Styling: Tailwind CSS 3.4.1
✓ Hosting: Vercel Pro
```

**Why versions matter:**
```
"Next.js" → AI might use Pages Router (old)
"Next.js 15" → AI uses App Router (new)
```

**Coding Conventions:**
```markdown
CODING CONVENTIONS:
- Imports: Use absolute imports with @ alias
  ✓ import { prisma } from '@/lib/db'
  ✗ import { prisma } from '../../lib/db'

- Naming:
  ✓ camelCase: functions (getUserProfile)
  ✓ PascalCase: components (UserProfile)
  ✓ SCREAMING_SNAKE_CASE: constants (DATABASE_URL)

- Async/Await:
  ✓ async/await (always)
  ✗ .then/.catch (never)

- Error Handling:
  ✓ try/catch blocks (required)
  ✗ Unhandled promises (never)
```

**Project Structure:**
```
app/
├── (auth)/           # Authentication pages
├── (dashboard)/      # Protected dashboard
├── api/              # API routes
lib/
├── db.ts            # Prisma client
├── auth.ts          # NextAuth config
components/
├── ui/              # Reusable UI components
└── [feature]/       # Feature-specific components
```

**How AI uses this:**
- Generates code matching existing patterns
- Uses correct import paths
- Follows naming conventions
- Puts files in correct locations

---

#### Layer 4: Feature Context

**Purpose:** Define exactly what we're building right now

**User Story Format:**
```markdown
USER STORY: Password Reset

As a registered user who forgot my password
I want to reset it via email link
So that I can regain access to my account

ACCEPTANCE CRITERIA:
1. Given I'm on the login page
   When I click "Forgot Password"
   Then I see an email input form

2. Given I enter my registered email
   When I submit the form
   Then I receive a password reset email within 1 minute

3. Given I click the reset link in the email
   When I enter a new valid password (meeting requirements)
   Then my password is updated (bcrypt hashed)
   And I'm logged in automatically

4. Given the reset link is >24 hours old
   When I try to use it
   Then I see error: "Link expired, request new reset"

EDGE CASES:
- Email not registered → Generic message (prevent user enumeration)
- Already used reset link → Error: "Link already used"
- Multiple reset requests → Only latest link valid, previous invalidated
- User tries to reset while already logged in → Redirect to dashboard

INTEGRATION POINTS:
- Email service: SendGrid (transactional email)
- Database: ResetToken table (store tokens with expiry)
- Authentication: Updates User.password field
```

**How AI uses this:**
- Generates code covering ALL acceptance criteria
- Handles ALL edge cases
- Creates necessary database models (ResetToken)
- Integrates with existing systems (User model, auth)

---

#### Layer 5: Constraints Context

**Purpose:** Define limitations and requirements

**When to include:**
- ✓ Budget constraints affect technology choices
- ✓ Timeline constraints affect scope
- ✓ Compliance requirements affect implementation
- ✓ Known technical debt requires workarounds

**Example:**
```markdown
LAYER 5: CONSTRAINTS CONTEXT

BUDGET:
- Maximum $50/month for all services combined
- Current spend: $20 Vercel + $25 Supabase = $45/month
- Available: $5/month for new services
- Constraint: Cannot add expensive services (e.g., Auth0 $20/mo would exceed budget)

TIMELINE:
- Feature must be complete in 2 days (48 hours)
- Constraint: Prioritize speed over perfection (MVP approach)
- Defer: Advanced features (2FA, social login) to Phase 2

COMPLIANCE:
- Users in EU (GDPR applies)
- Must implement: Data export, account deletion, cookie consent
- No health data (HIPAA not required)
- No payment card storage (using Stripe, PCI DSS handled)

TECHNICAL DEBT:
- Known issue: NextAuth.js v5 beta (custom fields not supported yet)
- Workaround: Store extra user data in separate table, fetch separately
- TODO: Consolidate when NextAuth.js v5.1 releases (Q2 2024)
```

**How AI uses this:**
- Chooses free/cheap solutions (bcrypt over paid auth service)
- Prioritizes scope appropriately (core features only)
- Includes GDPR-required features (data export, deletion)
- Implements documented workarounds

---

### Context Minimization

**Principle:** Include only relevant layers for the task
```
SIMPLE TASK (fix bug):
├─ Layer 3: Technical Context (required)
└─ Layer 4: Feature Context (what's broken)

MEDIUM TASK (new feature):
├─ Layer 1: Project Identity (required)
├─ Layer 3: Technical Context (required)
└─ Layer 4: Feature Context (required)

COMPLEX TASK (business-critical feature):
├─ Layer 1: Project Identity (required)
├─ Layer 2: Business Context (helps AI prioritize)
├─ Layer 3: Technical Context (required)
├─ Layer 4: Feature Context (required)
└─ Layer 5: Constraints (affects decisions)
```

**Example - Bug Fix (minimal context):**
```markdown
COMPONENT 2: CONTEXT INJECTION
────────────────────────────────────────────────────────

LAYER 3: TECHNICAL CONTEXT
- Framework: Next.js 15.0.3 (App Router)
- Bug: Rate limiting not triggering on /api/auth/login
- Expected: 5 attempts / 15 min should block requests
- Actual: Unlimited login attempts possible

LAYER 4: FEATURE CONTEXT
The rate limiting middleware exists but isn't being called.
Rate limiter configured in lib/rate-limit.ts.
Need to identify why middleware isn't executing.
```

---

### Validation Checklist
```markdown
CONTEXT COMPONENT QUALITY CHECKLIST:

LAYER 1 (Project Identity):
□ Project name specified
□ App type specified (web/mobile/API/etc.)
□ Industry/domain specified
□ Stage specified (MVP/Production/Scale)
□ Target users defined

LAYER 2 (Business Context) - if included:
□ Problem statement clear
□ Solution explained
□ Value proposition articulated
□ Success metrics defined

LAYER 3 (Technical Context):
□ Framework + version specified
□ Language + version specified
□ Database + ORM specified
□ Project structure provided
□ Coding conventions documented

LAYER 4 (Feature Context):
□ User story in correct format
□ Acceptance criteria testable (Given-When-Then)
□ Edge cases identified (3+ cases)
□ Integration points specified

LAYER 5 (Constraints) - if applicable:
□ Budget constraints specified
□ Timeline constraints specified
□ Compliance requirements listed
□ Known technical debt documented

GRADING:
Layer 1 + 3 + 4 complete: Minimum acceptable
All relevant layers complete: ⭐⭐⭐⭐⭐ Excellent
```

---

## 📋 COMPONENT 3: TASK DEFINITION

### Purpose

Define exactly what needs to be built, with clear scope boundaries and success criteria.

### Structure
```markdown
COMPONENT 3: TASK DEFINITION
────────────────────────────────────────────────────────

TASK: [One-sentence task description]

SCOPE:

IN SCOPE (what to build/fix):
- [Deliverable 1]
- [Deliverable 2]
- [Deliverable 3]

OUT OF SCOPE (explicitly NOT included - defer to later):
- [Future enhancement 1]
- [Future enhancement 2]

SUCCESS CRITERIA:
- [Measurable outcome 1]
- [Measurable outcome 2]
- [Measurable outcome 3]

PHASE: [X/N] - [Phase Name]
NEXT PHASE: [What comes after this?]

ESTIMATED TIME: [X minutes/hours]
```

---

### Examples

#### Example 1: Scaffolding Authentication
```markdown
COMPONENT 3: TASK DEFINITION
────────────────────────────────────────────────────────

TASK: Generate scaffolding for NextAuth.js v5 authentication system

SCOPE:

IN SCOPE (This Phase):
- File structure for authentication (all necessary files)
- TypeScript type definitions (User, Session, Account models)
- NextAuth.js configuration skeleton (lib/auth.ts)
- API route handlers (app/api/auth/[...nextauth]/route.ts)
- Prisma schema updates (User, Session, Account models)
- Environment variable template (.env.example)
- Function signatures with TODO comments (no implementation yet)

OUT OF SCOPE (Defer to Phase 2):
- Actual implementation (that's Phase 2: Implementation)
- Google OAuth setup (Phase 2)
- Login UI components (Phase 4: UI + Testing)
- Rate limiting (Phase 3: Security Hardening)

SUCCESS CRITERIA:
- All files created in correct locations (follows Next.js 15 App Router conventions)
- TypeScript compiles successfully (npm run type-check passes)
- No runtime implementation yet (scaffolding only)
- TODO comments mark where implementation will go
- Prisma schema includes all auth-related models
- Ready for Phase 2 (implementation)

PHASE: 1/4 - Scaffolding
NEXT PHASE: Phase 2 - Core Implementation (NextAuth config + Google OAuth)

ESTIMATED TIME: 10 minutes
```

#### Example 2: Bug Fix
```markdown
COMPONENT 3: TASK DEFINITION
────────────────────────────────────────────────────────

TASK: Fix rate limiting middleware not triggering on login endpoint

SCOPE:

IN SCOPE (This Fix):
- Identify why rate limiting middleware isn't executing
- Fix middleware configuration or Next.js middleware.ts
- Verify rate limiting triggers correctly (5 attempts / 15 min)
- Add test to confirm fix works

OUT OF SCOPE:
- Implementing rate limiting (already exists, just not working)
- Changing rate limit thresholds (configuration is correct)
- Rate limiting other endpoints (only fixing /api/auth/login)

SUCCESS CRITERIA:
- After 5 failed login attempts, 6th attempt returns 429 status
- Rate limit resets after 15 minutes
- Existing rate limiting code unchanged (only middleware config fixed)
- Manual test confirms fix works

PHASE: N/A (Bug Fix)
NEXT PHASE: N/A

ESTIMATED TIME: 30 minutes
```

#### Example 3: Feature Implementation
```markdown
COMPONENT 3: TASK DEFINITION
────────────────────────────────────────────────────────

TASK: Implement complete password reset flow with email verification

SCOPE:

IN SCOPE (This Feature):
- "Forgot Password" link on login page
- Email input form (request password reset)
- Email sending via SendGrid (with reset link)
- Reset token generation (cryptographically secure, 32 bytes)
- Reset token storage in database (hashed with SHA-256, expires 24h)
- Password reset page (accessible via token link)
- New password form (with validation: 8+ chars, complexity)
- Password update (bcrypt hash, cost 12)
- Automatic login after successful reset
- Email notification (password was changed)

OUT OF SCOPE (Future Enhancements):
- SMS-based password reset (defer to Phase 2 if needed)
- Security questions (not recommended, defer indefinitely)
- Account recovery without email (defer to Phase 3)

SUCCESS CRITERIA:
- User can request password reset (receives email within 1 min)
- Reset link works when clicked (loads reset form)
- Can set new password (meets validation requirements)
- Old password no longer works (new password required)
- Reset link expires after 24 hours (shows error if used after)
- Reset link single-use (cannot reuse after password changed)
- Rate limited (3 reset requests per hour per email)

PHASE: Implementation (Feature Complete)
NEXT PHASE: Testing (Phase E - Evaluation)

ESTIMATED TIME: 2 hours
```

---

### Scope Boundaries Best Practices

**Clear IN SCOPE:**
```
✓ Specific deliverables
✓ File names (lib/auth.ts, app/api/auth/route.ts)
✓ Functionality (can login, can reset password)
✓ What gets created/modified

✗ Vague statements ("make it work", "handle auth")
```

**Clear OUT OF SCOPE:**
```
✓ Explicit deferrals with reason
✓ "Not included in MVP" or "Phase 2 feature"
✓ Prevents scope creep

✗ Leaving things ambiguous (AI might include them)
```

**Example - Good vs Bad Scope:**

**❌ BAD (Vague):**
```
IN SCOPE:
- Authentication

OUT OF SCOPE:
- Other stuff
```

**✅ GOOD (Specific):**
```
IN SCOPE:
- Email/password login (POST /api/auth/login)
- Google OAuth login (NextAuth.js Google provider)
- Session management (database-backed sessions)
- Logout (DELETE /api/auth/logout)

OUT OF SCOPE:
- Password reset (defer to Sprint 2)
- Email verification (defer to Sprint 2)
- 2FA/MFA (defer to Sprint 3 - not MVP)
- Social login (GitHub, Facebook) - only Google for MVP
```

---

### Success Criteria Best Practices

**SMART Criteria:**
- **S**pecific: "User can login" not "Auth works"
- **M**easurable: "5 failed attempts → 429 status" not "Rate limiting works"
- **A**chievable: Realistic given constraints
- **R**elevant: Directly related to task
- **T**estable: Can verify with checklist

**Examples:**

**❌ BAD (Not Measurable):**
```
- Feature works
- No bugs
- Good performance
```

**✅ GOOD (Measurable):**
```
- User can login with valid email + password (200 status returned)
- Invalid credentials return 401 with generic error message
- After 5 failed attempts, 6th returns 429 (rate limited)
- Session persists across browser close (7 days max)
- TypeScript compiles with no errors (npm run type-check passes)
```

---

### Validation Checklist
```markdown
TASK DEFINITION COMPONENT QUALITY CHECKLIST:

□ Task described in one clear sentence
□ IN SCOPE: 3-7 specific deliverables listed
□ OUT OF SCOPE: Explicit deferrals (prevents scope creep)
□ SUCCESS CRITERIA: 3-5 measurable, testable outcomes
□ Phase identified (if part of multi-phase project)
□ Next phase specified (if applicable)
□ Estimated time provided (realistic)
□ No ambiguous language ("make it work", "handle X")

GRADING:
8/8 checked: ⭐⭐⭐⭐⭐ Crystal clear task
6-7 checked: ⭐⭐⭐⭐ Clear task
4-5 checked: ⭐⭐⭐ Acceptable
<4 checked: ⭐⭐ Too vague, needs improvement
```

---

**Current Progress: 3/7 components complete**

Should I continue with Components 4-7? 🚀
