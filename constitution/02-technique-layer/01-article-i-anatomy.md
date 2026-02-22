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
## 📝 COMPONENT 4: REQUIREMENTS SPECIFICATION

### Purpose

Transform vague feature descriptions into precise, testable requirements. Bridge the gap between "what we want" (business goal) and "what to build" (implementation).

### Structure
````markdown
COMPONENT 4: REQUIREMENTS SPECIFICATION
────────────────────────────────────────────────────────

FUNCTIONAL REQUIREMENTS:
[What the feature must DO]

1. [Requirement 1]
   - [Sub-requirement 1a]
   - [Sub-requirement 1b]

2. [Requirement 2]
   - [Sub-requirement 2a]

TECHNICAL REQUIREMENTS:
[HOW to implement]

1. [Technical decision 1]
2. [Technical decision 2]

NON-FUNCTIONAL REQUIREMENTS:
[Quality attributes: performance, security, accessibility]

- Performance: [response time, load capacity]
- Security: [OWASP compliance, encryption]
- Accessibility: [WCAG level, keyboard navigation]
- Scalability: [concurrent users, data volume]

EDGE CASES:
[Unusual scenarios that must be handled]

1. [Edge case 1] → [Expected behavior]
2. [Edge case 2] → [Expected behavior]

DATA REQUIREMENTS:
[Database schema, data models]
```prisma
[Prisma schema or data structure]
```
````

---

### Functional Requirements

**Purpose:** Define what the user can DO with the feature

**Format:** Action-oriented statements

**Example - Password Reset:**
````markdown
FUNCTIONAL REQUIREMENTS:

1. Request Password Reset:
   - User can click "Forgot Password" link on login page
   - User enters their registered email address
   - System validates email format (Zod schema)
   - System sends password reset email within 1 minute
   - User sees confirmation: "If email exists, reset link sent"

2. Receive Reset Email:
   - Email contains unique reset link (valid 24 hours)
   - Link format: https://app.com/reset-password?token=abc123...
   - Email explains link expires in 24 hours
   - Email includes "Didn't request this?" security notice

3. Reset Password:
   - User clicks reset link in email
   - System validates token (not expired, not used, exists)
   - User sees password reset form (if token valid)
   - User enters new password (must meet requirements)
   - System validates password strength (Zod schema)
   - System updates password (bcrypt hash, cost 12)
   - System marks token as used (single-use enforcement)
   - User automatically logged in (new session created)

4. Password Changed Notification:
   - System sends "Password Changed" email to user
   - Email includes time, date, IP address (security audit)
   - Email includes "Wasn't you?" link to report unauthorized access
````

**Key Principles:**
- ✅ User-centric (describes user actions)
- ✅ Specific (not "user can reset password" - too vague)
- ✅ Complete (covers entire user journey)
- ✅ Testable (can verify each requirement)

---

### Technical Requirements

**Purpose:** Define HOW to implement (technology choices, architectural decisions)

**Example - Password Reset:**
````markdown
TECHNICAL REQUIREMENTS:

1. Token Generation:
   - Use crypto.randomBytes(32) for token generation
   - Cryptographically secure random (not Math.random())
   - 32 bytes = 256 bits (sufficient entropy)

2. Token Storage:
   - Store SHA-256 hash of token (not plaintext)
   - Include userId, hashedToken, expiresAt, usedAt
   - Index on hashedToken for fast lookup
   - Cascade delete when user deleted

3. Email Sending:
   - SendGrid Transactional Email API
   - Template: password-reset-v1
   - From: noreply@app.com
   - Subject: "Reset Your Password - TaskMaster Pro"

4. Password Validation:
   - Zod schema: min 8 chars, uppercase, number, special char
   - Server-side validation (never trust client)
   - Error messages specific (not generic "invalid password")

5. Password Hashing:
   - bcrypt with cost factor 12 (constitutional minimum)
   - Async hashing (bcrypt.hash, not bcrypt.hashSync)
   - Replace old hash (don't keep history)

6. Session Creation:
   - NextAuth.js signIn() after successful reset
   - Database-backed session (not JWT)
   - 7-day expiration (constitutional maximum)
````

**Key Principles:**
- ✅ Technology-specific (names libraries, APIs)
- ✅ Explains WHY (not just WHAT)
- ✅ Includes version numbers where critical
- ✅ References constitutional mandates

---

### Non-Functional Requirements

**Purpose:** Define quality attributes (the "-ilities")

**Categories:**
1. **Performance:** Speed, response time, throughput
2. **Security:** OWASP compliance, encryption, authentication
3. **Accessibility:** WCAG compliance, keyboard navigation, screen readers
4. **Scalability:** Concurrent users, data volume, growth capacity
5. **Reliability:** Uptime, error rates, recovery time
6. **Usability:** Ease of use, learning curve, user satisfaction
7. **Maintainability:** Code quality, documentation, testability

**Example - Password Reset:**
````markdown
NON-FUNCTIONAL REQUIREMENTS:

PERFORMANCE:
- Password reset request: < 500ms response time (P95)
- Email sent: < 60 seconds from request
- Reset form load: < 2 seconds (3G network)
- Password update: < 1 second (bcrypt is expensive, acceptable)

SECURITY:
- OWASP A02 (Cryptographic Failures):
  ✓ Tokens stored hashed (SHA-256)
  ✓ Passwords hashed (bcrypt cost 12)
  
- OWASP A07 (Authentication Failures):
  ✓ Rate limiting: 3 reset requests per hour per email
  ✓ Token expiry: 24 hours maximum
  ✓ Single-use tokens (cannot reuse after password changed)
  
- OWASP A08 (Data Integrity):
  ✓ CSRF protection (NextAuth.js handles)
  ✓ HTTPS enforced (all reset links use https://)

- Generic error messages (prevent user enumeration):
  ✓ "If email exists, reset link sent" (not "Email not found")

ACCESSIBILITY (WCAG 2.1 AA):
- Form labels: All inputs have <label htmlFor> association
- Error messages: aria-live="polite" announces errors
- Keyboard navigation: Can Tab through entire flow
- Focus indicators: Visible focus ring on all interactive elements
- Color contrast: ≥ 4.5:1 for all text
- Screen reader: "Reset Password" form properly announced

SCALABILITY:
- Can handle 1,000 concurrent password reset requests
- Token table can store 100K+ tokens (with indexes)
- Email queue can handle 10K emails/hour (SendGrid limit)

RELIABILITY:
- Email delivery: 99% success rate (SendGrid SLA)
- Token generation: 100% success (crypto.randomBytes never fails)
- Database operations: Retry on transient failures (Prisma retry logic)

USABILITY:
- Clear instructions: "Check your email for reset link"
- Password requirements visible: Show requirements before user types
- Success feedback: "Password changed successfully, logging you in..."
- Error recovery: "Link expired? Request a new one" with link

MAINTAINABILITY:
- TypeScript strict mode (no 'any' types)
- Unit tests: Token generation, validation, expiry logic
- Integration tests: Complete password reset flow
- Documentation: JSDoc comments on all public functions
````

---

### Edge Cases

**Purpose:** Ensure robustness by handling unusual scenarios

**Categories:**
1. **User behavior:** Unexpected actions, corner cases
2. **System failures:** Network errors, database timeouts
3. **Security:** Attack attempts, malicious input
4. **Timing:** Race conditions, concurrent operations
5. **Data:** Empty states, large data, special characters

**Example - Password Reset:**
````markdown
EDGE CASES:

1. Email Not Registered:
   → Show generic message: "If email exists, reset link sent"
   → Prevents user enumeration (attacker can't discover registered emails)
   → Still rate limit (prevent abuse)

2. Multiple Reset Requests:
   → Invalidate previous tokens (only latest token valid)
   → User receives new email: "You requested another reset link"
   → Old links show: "Token expired or already used"

3. Token Expired (>24 hours):
   → Show error: "This reset link has expired"
   → Offer: "Request a new password reset link" (button)
   → Log security event (potential attack if many expired attempts)

4. Token Already Used:
   → Show error: "This reset link has already been used"
   → Offer: "Request a new password reset link"
   → Security note: "If you didn't change your password, contact support"

5. User Already Logged In:
   → Redirect to dashboard with message: "You're already logged in"
   → Or: Allow password change from settings (different flow)

6. Reset Requested While Password Reset Pending:
   → Invalidate old token, generate new one
   → User gets new email: "New reset link sent (previous link canceled)"

7. Weak Password Submitted:
   → Reject with specific errors:
     ✗ "Password must be at least 8 characters"
     ✗ "Password must contain an uppercase letter"
     ✗ "Password must contain a number"
     ✗ "Password must contain a special character (!@#$%^&*)"

8. User Deletes Account During Reset Process:
   → Token becomes invalid (cascade delete in database)
   → Reset link shows: "This reset link is no longer valid"

9. Network Timeout During Email Sending:
   → Retry email sending (max 3 attempts)
   → If all retries fail, log error (notify support team)
   → User sees: "Reset link sent" (don't expose email failure)

10. Concurrent Password Resets (Same User, Multiple Devices):
    → Only latest token valid (previous tokens invalidated)
    → First to use valid token wins
    → Others get: "Token expired or already used"

11. SQL Injection Attempt in Email Input:
    → Zod validation rejects (not valid email format)
    → Prisma parameterized queries prevent injection anyway
    → Log security event (potential attack)

12. Token Manipulation (User Modifies Token in URL):
    → SHA-256 hash won't match any stored token
    → Show: "Invalid reset link"
    → Rate limit token validation attempts (prevent brute force)

13. User Clicks Reset Link Multiple Times (Impatient):
    → First click: Show reset form
    → Subsequent clicks (token not used yet): Show same form
    → After password changed: Show "already used" error

14. Email Delivery Failure (SendGrid Down):
    → Retry with exponential backoff (1s, 2s, 4s)
    → If 3 retries fail, add to background job queue
    → User still sees: "If email exists, reset link sent"
    → Background job retries every 5 minutes for 1 hour

15. User Enters Same Password as Current Password:
    → Allow it (constitutional principle: user sovereignty)
    → But: Show warning "This is your current password"
    → Reason: User might not remember, forcing change is annoying
````

**Key Principles:**
- ✅ Anticipate misuse (security edge cases)
- ✅ Handle failures gracefully (don't crash)
- ✅ Maintain security (even in edge cases)
- ✅ User-friendly error messages

---

### Data Requirements

**Purpose:** Define database schema, relationships, constraints

**Example - Password Reset:**
````markdown
DATA REQUIREMENTS:

PRISMA SCHEMA:
```prisma
model User {
  id                String   @id @default(cuid())
  email             String   @unique
  password          String?  // Nullable (OAuth users have no password)
  name              String
  emailVerified     DateTime?
  createdAt         DateTime @default(now())
  updatedAt         DateTime @updatedAt
  
  // Relations
  resetTokens       ResetToken[]
  sessions          Session[]
  
  @@index([email])
}

model ResetToken {
  id                String   @id @default(cuid())
  hashedToken       String   @unique  // SHA-256 hash of actual token
  userId            String
  expiresAt         DateTime
  usedAt            DateTime?  // Null = not used, DateTime = used
  createdAt         DateTime @default(now())
  
  // Relations
  user              User     @relation(fields: [userId], references: [id], onDelete: Cascade)
  
  // Indexes for performance
  @@index([hashedToken])  // Fast token lookup
  @@index([userId])       // Fast user token lookup
  @@index([expiresAt])    // Clean up expired tokens
}
```

SCHEMA NOTES:

1. ResetToken.hashedToken:
   - Store SHA-256 hash (not plaintext token)
   - Unique constraint (no duplicate tokens)
   - Index for O(1) lookup performance

2. ResetToken.expiresAt:
   - Timestamp when token becomes invalid
   - Index for cleanup job (delete expired tokens)
   - Checked on every token validation

3. ResetToken.usedAt:
   - Null = token not used yet (can still be used)
   - DateTime = token used (cannot reuse)
   - Single-use enforcement

4. Cascade Delete:
   - When User deleted → All ResetTokens deleted
   - Prevents orphaned tokens in database

5. User.password Nullable:
   - OAuth users (Google, GitHub) have no password
   - Allow null to support social login

DATA FLOW:

1. User requests reset:
   → Create ResetToken (hashedToken, userId, expiresAt)
   → Send email with plaintext token (not stored)

2. User clicks link:
   → Extract token from URL query param
   → Hash with SHA-256
   → Look up ResetToken by hashedToken
   → Validate: expiresAt > now, usedAt == null

3. User resets password:
   → Update User.password (bcrypt hash)
   → Update ResetToken.usedAt = now()
   → Token now invalid (single-use enforced)

4. Cleanup (daily cron job):
   → Delete ResetToken where expiresAt < now - 7 days
   → Keep recent expired tokens for audit trail (7 days)
````

---

### Complete Example - Authentication Feature
````markdown
COMPONENT 4: REQUIREMENTS SPECIFICATION
────────────────────────────────────────────────────────

FUNCTIONAL REQUIREMENTS:

1. User Registration:
   - User can access registration page (/register)
   - User enters email, password, name
   - System validates email format (Zod: email())
   - System validates password strength (8+ chars, complexity)
   - System checks email uniqueness (not already registered)
   - System creates user account (bcrypt hash password, cost 12)
   - System sends verification email (with verification link)
   - User sees: "Account created. Check email to verify."

2. User Login (Email/Password):
   - User can access login page (/login)
   - User enters email and password
   - System validates credentials (bcrypt.compare)
   - System rate limits (5 attempts per 15 minutes per IP)
   - On success: Create session, redirect to /dashboard
   - On failure: Generic error "Invalid credentials" (no user enumeration)

3. User Login (Google OAuth):
   - User can click "Continue with Google" button
   - System initiates Google OAuth flow (NextAuth.js)
   - User authenticates with Google
   - System creates user if new (or links to existing by email)
   - System creates session, redirects to /dashboard

4. Session Management:
   - Session stored in database (not JWT)
   - HTTP-only cookie (JavaScript cannot access)
   - SameSite=Strict (CSRF protection)
   - 7-day expiration (constitutional maximum)
   - Session renewed on activity (sliding window)

5. User Logout:
   - User can click "Logout" button
   - System deletes session from database
   - System clears session cookie
   - User redirected to login page

TECHNICAL REQUIREMENTS:

1. Authentication Framework:
   - NextAuth.js v5.2.0 (latest stable)
   - Prisma adapter for database sessions
   - Google OAuth provider configured

2. Password Hashing:
   - bcrypt with cost factor 12
   - Async hashing (bcrypt.hash, not hashSync)
   - Validate with bcrypt.compare (constant-time)

3. Rate Limiting:
   - Upstash Redis for rate limit storage
   - @upstash/ratelimit library
   - 5 attempts per 15 minutes per IP (login)
   - 3 attempts per hour per IP (registration)

4. Email Service:
   - SendGrid Transactional Email API
   - Template: registration-verification-v1
   - From: noreply@taskmaster.app

5. Database Models:
   - User (id, email, password, name, emailVerified)
   - Session (id, userId, sessionToken, expires)
   - Account (id, userId, provider, providerAccountId) [for OAuth]

NON-FUNCTIONAL REQUIREMENTS:

PERFORMANCE:
- Login response: < 500ms (P95) [bcrypt is expensive, acceptable]
- Session validation: < 50ms (database lookup)
- OAuth redirect: < 1 second total flow

SECURITY:
- Commandment III: bcrypt cost 12 (password security)
- Commandment IV: Database sessions, HTTP-only cookies, CSRF protection
- Commandment V: Rate limiting (5 attempts / 15 min)
- Commandment VI: No user enumeration (generic error messages)
- OWASP A01: Authorization checks on all protected routes
- OWASP A02: Passwords hashed, sessions encrypted
- OWASP A07: Rate limiting, session timeout

ACCESSIBILITY (WCAG 2.1 AA):
- Login form: Labels on all inputs (htmlFor + id)
- Error messages: aria-live="polite" announces errors
- Password visibility toggle: aria-label="Show password"
- Keyboard navigation: Tab through form, Enter submits
- Focus indicators: Visible 2px outline on all inputs
- Color contrast: ≥ 4.5:1 for all text

SCALABILITY:
- Can handle 10,000 concurrent sessions
- Can handle 1,000 login attempts per minute
- Redis rate limiter scales horizontally

RELIABILITY:
- Email delivery: 99% (SendGrid SLA)
- Session storage: 99.9% uptime (database SLA)
- OAuth: 99.9% uptime (Google SLA)

EDGE CASES:

1. Email Already Registered:
   → Error: "Email already in use" (409 Conflict)
   → Suggest: "Try logging in or reset password"

2. Weak Password Submitted:
   → Specific errors: "Must contain uppercase", "Must be 8+ chars"
   → Client-side validation (UX) + server-side (security)

3. OAuth Email Matches Existing Credentials Account:
   → Link accounts (same email = same user)
   → Update User.emailVerified = now() (OAuth = verified)

4. Rate Limit Exceeded:
   → 429 status with Retry-After header
   → Message: "Too many attempts. Try again in 15 minutes."
   → Exponential backoff on repeated violations

5. Session Expired During Use:
   → Redirect to /login with message: "Session expired, please log in"
   → Preserve original destination (return URL)

6. User Closes Browser (Session Persistence):
   → Session cookie Max-Age = 7 days (persists browser close)
   → "Remember me" checkbox (optional): 30 days instead of 7

7. Concurrent Logins (Multiple Devices):
   → Allow (user can be logged in on phone + laptop)
   → Optional: Limit to N concurrent sessions (configurable)

8. Account Deleted During Active Session:
   → Session validation fails (user not found)
   → Redirect to login: "Account no longer exists"

9. Google OAuth Failure (Google Down):
   → Show error: "Google login temporarily unavailable"
   → Fallback: "Try email/password login instead"

10. Email Verification Link Expired (>24 hours):
    → Show: "Verification link expired"
    → Offer: "Resend verification email" button

DATA REQUIREMENTS:
```prisma
model User {
  id            String    @id @default(cuid())
  email         String    @unique
  password      String?   // Nullable (OAuth users)
  name          String
  emailVerified DateTime?
  image         String?   // Profile picture (from OAuth)
  createdAt     DateTime  @default(now())
  updatedAt     DateTime  @updatedAt
  
  sessions      Session[]
  accounts      Account[]
  
  @@index([email])
}

model Session {
  id           String   @id @default(cuid())
  sessionToken String   @unique
  userId       String
  expires      DateTime
  createdAt    DateTime @default(now())
  
  user User @relation(fields: [userId], references: [id], onDelete: Cascade)
  
  @@index([userId])
  @@index([sessionToken])
}

model Account {
  id                String  @id @default(cuid())
  userId            String
  type              String  // "oauth" | "email"
  provider          String  // "google" | "github" | "credentials"
  providerAccountId String
  refresh_token     String?
  access_token      String?
  expires_at        Int?
  token_type        String?
  scope             String?
  id_token          String?
  session_state     String?
  
  user User @relation(fields: [userId], references: [id], onDelete: Cascade)
  
  @@unique([provider, providerAccountId])
  @@index([userId])
}
```
````

---

### Validation Checklist
````markdown
REQUIREMENTS COMPONENT QUALITY CHECKLIST:

FUNCTIONAL REQUIREMENTS:
□ All user actions documented (what user can DO)
□ Complete user journey (registration → login → logout)
□ 5-10 functional requirements specified
□ Each requirement has sub-requirements (detailed)

TECHNICAL REQUIREMENTS:
□ Technology choices specified (NextAuth.js v5, bcrypt, etc.)
□ Version numbers included (where critical)
□ Explains WHY (not just WHAT)
□ References constitutional mandates

NON-FUNCTIONAL REQUIREMENTS:
□ Performance targets specified (< 500ms, etc.)
□ Security compliance documented (OWASP, Commandments)
□ Accessibility requirements (WCAG AA)
□ Scalability limits defined (concurrent users, data volume)

EDGE CASES:
□ 10+ edge cases identified and handled
□ Security edge cases included (SQL injection, etc.)
□ User behavior edge cases (multiple attempts, etc.)
□ System failure edge cases (email down, database timeout)

DATA REQUIREMENTS:
□ Complete Prisma schema provided
□ Relationships defined (foreign keys)
□ Indexes specified (performance optimization)
□ Constraints documented (unique, nullable)
□ Schema notes explain design decisions

GRADING:
All 5 sections complete with detail: ⭐⭐⭐⭐⭐ Excellent
4 sections complete: ⭐⭐⭐⭐ Good
3 sections complete: ⭐⭐⭐ Acceptable
<3 sections: ⭐⭐ Insufficient
````

---

## 🛡️ COMPONENT 5: SECURITY MANDATES

### Purpose

Inject constitutional security and quality requirements directly into the prompt to ensure AI generates compliant code.

### Structure
````markdown
COMPONENT 5: SECURITY MANDATES (Constitutional Requirements)
────────────────────────────────────────────────────────

Source: STRATEG CONSTITUTION v2.0
- Article I, Law #3 (Security First)
- Article VI, Strategic Commandments

MANDATORY SECURITY REQUIREMENTS:

[For each relevant commandment:]

COMMANDMENT [NUMBER]: [NAME]

Mandate:
[Specific requirement from constitution]

Rationale:
[Why this is required - business/security justification]

Implementation:
```[language]
[Code example showing HOW to implement]
```

Violations Prohibited:
❌ [What NOT to do - example 1]
❌ [What NOT to do - example 2]

OWASP Reference: [A0X:2021 - Category Name]

────────────────────────────────────────────────────────

CONSTITUTIONAL VALIDATION CHECKLIST:

Before considering this implementation complete, verify:
□ [Checkpoint 1]
□ [Checkpoint 2]
□ [Checkpoint 3]
...

FAILURE TO COMPLY:
Violating these mandates is a CRITICAL constitutional violation.
Code that violates security mandates will NOT pass evaluation phase.
````

---

### Which Commandments to Include?

**Decision Tree:**
````
FEATURE = Authentication?
├─ Commandment I: Input Validation (email, password)
├─ Commandment III: Password Security (bcrypt)
├─ Commandment IV: Session Management
├─ Commandment V: Rate Limiting
└─ Commandment VIII: Error Handling (no user enumeration)

FEATURE = API Endpoint?
├─ Commandment I: Input Validation (Zod)
├─ Commandment V: Rate Limiting
├─ Commandment VI: Access Control (authorization)
└─ Commandment VIII: Error Handling

FEATURE = Form/UI?
├─ Commandment I: Input Validation (client + server)
├─ Commandment II: Output Encoding (XSS prevention)
├─ Commandment IX: Accessibility (WCAG AA)
└─ Commandment X: Documentation (form labels)

FEATURE = Data Handling?
├─ Commandment I: Input Validation
├─ Commandment VII: Data Protection (encryption, GDPR)
└─ Commandment VIII: Error Handling (no PII in logs)

FEATURE = File Upload?
├─ Commandment I: Input Validation (file type, size)
├─ Commandment VII: Data Protection (virus scan, secure storage)
└─ Commandment VIII: Error Handling

ALL FEATURES (Always Include):
└─ Commandment VIII: Error Handling (generic errors, logging)
````

---

### Example - Authentication Feature
````markdown
COMPONENT 5: SECURITY MANDATES (Constitutional Requirements)
────────────────────────────────────────────────────────

Source: STRATEG CONSTITUTION v2.0
- Article I, Law #3 (Security First)
- Article VI, Strategic Commandments #1, 3, 4, 5, 8

MANDATORY SECURITY REQUIREMENTS:

────────────────────────────────────────────────────────

COMMANDMENT I: INPUT VALIDATION

Mandate:
ALL user input MUST be validated on the server side using Zod schemas.
Client-side validation is for UX only, never for security.

Rationale:
90% of security vulnerabilities stem from improper input validation.
Client-side validation can be bypassed by attackers.

Implementation:
```typescript
import { z } from 'zod'

const loginSchema = z.object({
  email: z.string().email('Invalid email format'),
  password: z.string().min(8, 'Password must be at least 8 characters')
})

export async function POST(request: Request) {
  const body = await request.json()
  
  // Validate input
  const result = loginSchema.safeParse(body)
  
  if (!result.success) {
    return Response.json(
      { error: 'Invalid input', details: result.error.format() },
      { status: 400 }
    )
  }
  
  // Proceed with validated data
  const { email, password } = result.data
  // ...
}
```

Violations Prohibited:
❌ Trusting client-side validation alone
❌ No server-side validation
❌ String concatenation in SQL queries (use Prisma)

OWASP Reference: A03:2021 - Injection

────────────────────────────────────────────────────────

COMMANDMENT III: PASSWORD SECURITY

Mandate:
Passwords MUST be hashed with bcrypt (cost factor 12 minimum) or Argon2.
NEVER store passwords in plaintext, not even temporarily.

Rationale:
Plaintext passwords expose ALL accounts in event of database breach.
Average breach cost: $4.5M (IBM 2023).

Implementation:
```typescript
import bcrypt from 'bcrypt'

// Registration
const hashedPassword = await bcrypt.hash(password, 12)
await prisma.user.create({
  data: { email, password: hashedPassword }
})

// Login
const user = await prisma.user.findUnique({ where: { email } })
const valid = await bcrypt.compare(password, user.password)
```

Violations Prohibited:
❌ Storing plaintext passwords
❌ Using MD5, SHA-1, or SHA-256 for passwords
❌ bcrypt cost factor < 12
❌ Logging passwords (even hashed)

OWASP Reference: A02:2021 - Cryptographic Failures

────────────────────────────────────────────────────────

COMMANDMENT IV: SESSION MANAGEMENT

Mandate:
Sessions MUST be database-backed (not JWT for sensitive apps).
Cookies MUST be HTTP-only, Secure, and SameSite=Strict.
Session timeout: 7 days maximum.

Rationale:
JWT sessions cannot be revoked (logout doesn't work).
HTTP-only cookies prevent XSS theft.
SameSite=Strict prevents CSRF attacks.

Implementation:
```typescript
import NextAuth from 'next-auth'
import { PrismaAdapter } from '@auth/prisma-adapter'

export const { handlers, auth } = NextAuth({
  adapter: PrismaAdapter(prisma),
  session: {
    strategy: 'database',  // NOT "jwt"
    maxAge: 7 * 24 * 60 * 60  // 7 days (constitutional maximum)
  },
  cookies: {
    sessionToken: {
      options: {
        httpOnly: true,      // JavaScript cannot access
        sameSite: 'strict',  // CSRF protection
        secure: process.env.NODE_ENV === 'production'  // HTTPS only
      }
    }
  }
})
```

Violations Prohibited:
❌ localStorage for sessions (vulnerable to XSS)
❌ JWT for sensitive applications (cannot revoke)
❌ No CSRF protection
❌ Session in URL parameters

OWASP Reference: A07:2021 - Identification and Authentication Failures

────────────────────────────────────────────────────────

COMMANDMENT V: RATE LIMITING

Mandate:
Authentication endpoints MUST be rate limited:
- Login: 5 attempts per 15 minutes per IP
- Registration: 3 attempts per hour per IP

Rationale:
Without rate limiting, attackers can brute force passwords.
2021 incident: 47 accounts compromised in 2 hours (no rate limiting).

Implementation:
```typescript
import { Ratelimit } from '@upstash/ratelimit'
import { Redis } from '@upstash/redis'

const rateLimit = new Ratelimit({
  redis: new Redis({
    url: process.env.UPSTASH_REDIS_REST_URL!,
    token: process.env.UPSTASH_REDIS_REST_TOKEN!
  }),
  limiter: Ratelimit.slidingWindow(5, '15 m')
})

export async function POST(request: Request) {
  const ip = request.headers.get('x-forwarded-for') || 'unknown'
  const { success, reset } = await rateLimit.limit(ip)
  
  if (!success) {
    return Response.json(
      { error: 'Too many attempts. Try again later.' },
      { 
        status: 429,
        headers: { 'Retry-After': Math.ceil((reset - Date.now()) / 1000).toString() }
      }
    )
  }
  
  // Proceed with login
}
```

Violations Prohibited:
❌ No rate limiting on authentication
❌ Client-side rate limiting only
❌ Same limit for all operations

OWASP Reference: A07:2021 - Identification and Authentication Failures

────────────────────────────────────────────────────────

COMMANDMENT VIII: ERROR HANDLING

Mandate:
Error messages to users MUST be generic (no system details exposed).
Detailed errors logged server-side only.
No user enumeration (same error for "user not found" and "wrong password").

Rationale:
Detailed errors help attackers (reveal system structure, database schema).
User enumeration allows discovering registered emails.

Implementation:
```typescript
export async function POST(request: Request) {
  try {
    const { email, password } = await request.json()
    
    const user = await prisma.user.findUnique({ where: { email } })
    
    if (!user || !await bcrypt.compare(password, user.password)) {
      // Generic error (no user enumeration)
      return Response.json(
        { error: 'Invalid credentials' },  // Same for both cases
        { status: 401 }
      )
    }
    
    // Success...
    
  } catch (error) {
    // Log full details server-side
    console.error('Login error:', error)
    
    // Generic error to user
    return Response.json(
      { error: 'An error occurred. Please try again later.' },
      { status: 500 }
    )
  }
}
```

Violations Prohibited:
❌ Exposing stack traces to users
❌ Different messages: "User not found" vs "Wrong password"
❌ Database error details in response
❌ PII in error logs

OWASP Reference: A05:2021 - Security Misconfiguration

────────────────────────────────────────────────────────

CONSTITUTIONAL VALIDATION CHECKLIST:

Before considering this authentication implementation complete:

□ Passwords hashed with bcrypt (cost ≥ 12)
□ Sessions stored in database (not JWT)
□ HTTP-only cookies configured (httpOnly: true)
□ CSRF protection enabled (NextAuth.js default)
□ Rate limiting implemented (5 attempts / 15 min)
□ Input validation (Zod schemas on all endpoints)
□ Generic error messages (no user enumeration)
□ No PII logged (no passwords, even hashed)
□ HTTPS enforced in production

FAILURE TO COMPLY:

Violating these mandates is a CRITICAL constitutional violation.

This code will NOT pass Phase E (Evaluation & Testing).
Security vulnerabilities will be flagged during OWASP Top 10 audit.

I CANNOT generate code that violates these mandates.
This is non-negotiable (Article I, Law #3: Security First).
````

---

### Validation Checklist
````markdown
SECURITY MANDATES COMPONENT QUALITY CHECKLIST:

□ All relevant commandments included (3-6 commandments typical)
□ Each mandate has clear requirement statement
□ Rationale provided (WHY it matters)
□ Implementation code example provided (HOW to do it)
□ Violations explicitly prohibited (WHAT NOT to do)
□ OWASP reference included (traceability)
□ Constitutional validation checklist present
□ Failure consequences stated clearly

GRADING:
8/8 checked: ⭐⭐⭐⭐⭐ Comprehensive security mandate
6-7 checked: ⭐⭐⭐⭐ Good security coverage
4-5 checked: ⭐⭐⭐ Acceptable
<4 checked: ⭐⭐ Insufficient security guidance
````

---

**[TO BE CONTINUED - Components 6-7 in next response...]**

**Current Progress: 5/7 components complete**

Should I continue with Components 6-7 (final components)? 🚀
