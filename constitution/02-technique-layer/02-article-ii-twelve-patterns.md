# 🎨 ARTICLE II: THE TWELVE PATTERNS

**Proven Prompt Patterns for Constitutional Code Generation**

---

## 📜 CONSTITUTIONAL AUTHORITY

This article defines the **12 canonical prompt patterns** used by MVCA to generate constitutional code for different development scenarios. Each pattern is optimized for specific tasks and follows the 7-component structure defined in Article I.

**Legal Force:**
- ✅ MVCA **MUST** select appropriate pattern based on request type
- ✅ Each pattern **SHALL** follow the specifications defined herein
- ✅ Pattern selection **SHALL** follow the decision tree (see Pattern Selection)
- ✅ Manual prompt creators **MAY** use these patterns as templates
- ✅ Pattern modifications **SHALL** require evidence-based justification (Segment 7)

**Constitutional Principle:**
> Different tasks require different approaches.  
> The right pattern ensures the right outcome.

---

## 🎯 THE TWELVE PATTERNS OVERVIEW

### Pattern Categories
```
CREATION PATTERNS (Building New Code):
├─ Pattern #1: Scaffolding (structure without implementation)
├─ Pattern #2: Implementation (complete feature building)
└─ Pattern #7: Testing (test suite creation)

MODIFICATION PATTERNS (Changing Existing Code):
├─ Pattern #3: Delta Editing (surgical code changes)
├─ Pattern #6: Refactoring (improve without changing behavior)
└─ Pattern #10: Performance Optimization (make it faster)

PROBLEM-SOLVING PATTERNS (Fixing Issues):
├─ Pattern #4: Debugging (find and fix bugs)
├─ Pattern #5: Chain of Thought (complex reasoning)
└─ Pattern #9: Code Review (quality assessment)

INTEGRATION PATTERNS (Connecting Systems):
├─ Pattern #8: Integration (connect external services)
└─ Pattern #12: Migration (move between technologies)

AUDIT PATTERNS (Quality Verification):
├─ Pattern #11: Security Audit (OWASP compliance check)
└─ Pattern #13: Accessibility Audit (WCAG compliance check)

Note: Pattern #2 (Implementation) is implied in the overview above
Note: Pattern #13 is typically combined with Pattern #11 as "Quality Audit"
```

---

## 📊 PATTERN SELECTION DECISION TREE

### How MVCA Chooses the Right Pattern
```
USER REQUEST → CLASSIFY → SELECT PATTERN

REQUEST TYPE CLASSIFICATION:

"Create [feature]" OR "Build [feature]" OR "Add [feature]"
├─ Is it NEW code? (not modifying existing)
│  ├─ YES: Is it complex? (>500 lines estimated)
│  │  ├─ YES: Pattern #1 Scaffolding → Then Pattern #2 Implementation
│  │  └─ NO: Pattern #2 Implementation (direct)
│  └─ NO: Go to modification patterns below

"Fix [bug]" OR "Debug [issue]" OR "Why doesn't [X] work?"
├─ Pattern #4 Debugging

"Refactor [code]" OR "Improve [code]" OR "Clean up [code]"
├─ Is goal: Make it faster?
│  ├─ YES: Pattern #10 Performance Optimization
│  └─ NO: Pattern #6 Refactoring (code quality)

"Change [X] to [Y]" OR "Update [feature]" OR "Modify [code]"
├─ Pattern #3 Delta Editing

"Connect [service]" OR "Integrate [API]" OR "Add [third-party]"
├─ Pattern #8 Integration

"Write tests for [feature]"
├─ Pattern #7 Testing

"Review [code]" OR "Is this code good?"
├─ Pattern #9 Code Review

"Audit security" OR "Check for vulnerabilities"
├─ Pattern #11 Security Audit

"Check accessibility" OR "Is this WCAG compliant?"
├─ Pattern #13 Accessibility Audit

"Migrate from [X] to [Y]"
├─ Pattern #12 Migration

"Explain [complex problem]" OR "How would you approach [complex task]?"
├─ Pattern #5 Chain of Thought

DEFAULT (ambiguous):
├─ Pattern #2 Implementation (safe default)
```

---

## 🏗️ PATTERN #1: SCAFFOLDING

### Purpose

Generate project structure, file skeletons, and type definitions WITHOUT implementing actual logic. Used for complex features (>500 lines) as Phase 1 before implementation.

### When to Use
```
✅ USE SCAFFOLDING WHEN:
- Feature is complex (>500 lines estimated)
- Multiple files need to be created
- Type definitions need careful planning
- Database schema needs to be designed first
- User wants to review structure before implementation

✅ EXAMPLES:
- "Create authentication system" (complex, many files)
- "Build payment integration" (complex, sensitive)
- "Add multi-tenant architecture" (complex, database-heavy)

❌ DON'T USE SCAFFOLDING WHEN:
- Feature is simple (<100 lines)
- Single file modification
- Time is critical (scaffolding adds extra step)
- User explicitly wants immediate implementation

❌ EXAMPLES:
- "Add a logout button" (too simple)
- "Fix typo in component" (trivial change)
```

---

### Pattern Structure

**7-Component Adaptation:**
```markdown
COMPONENT 1: PERSONA
"You are a senior architect specializing in [domain]..."
Focus: Planning, structure, design (not implementation)

COMPONENT 2: CONTEXT
Include: Layers 1, 3, 4 (skip 2 & 5 - not needed for structure)

COMPONENT 3: TASK
Task: "Generate SCAFFOLDING ONLY (no implementation)"
IN SCOPE:
- File structure (folders, files, locations)
- Type definitions (interfaces, types, enums)
- Function signatures (name, params, return type, JSDoc)
- Database schema (models, relations, indexes)
- TODO comments (mark implementation points)

OUT OF SCOPE:
- Actual implementation (Phase 2)
- Complex logic (Phase 2)
- Error handling (Phase 2)
- Testing (Phase 4)

SUCCESS CRITERIA:
- All files created in correct locations
- TypeScript compiles (no errors)
- Clear TODO comments mark implementation points
- Ready for Phase 2 (Implementation)

COMPONENT 4: REQUIREMENTS
Focus: Structural requirements (file organization, types, schema)
Skip: Detailed implementation requirements (those go in Phase 2)

COMPONENT 5: SECURITY MANDATES
Include: Schema-level security (indexes, constraints, cascade deletes)
Skip: Implementation-level security (that's Phase 2)

COMPONENT 6: META-INSTRUCTIONS
THINKING PROCESS: "Start with types → Schema → File structure → Function signatures"
CONSTRAINTS: "DO NOT implement logic, DO NOT add complex error handling"

COMPONENT 7: OUTPUT
DELIVERABLES: File list, complete scaffolding code, TODO markers
NEXT STEPS: "After validation, request Phase 2: Implementation"
```

---

### Complete Example - Authentication Scaffolding
```markdown
═══════════════════════════════════════════════════════════
CONSTITUTIONAL PROMPT: AUTHENTICATION SCAFFOLDING
Pattern #1: Scaffolding
═══════════════════════════════════════════════════════════

COMPONENT 1: PERSONA ASSIGNMENT
────────────────────────────────────────────────────────

You are a senior software architect specializing in authentication 
systems and Next.js application structure with 10+ years experience.

Your expertise includes:
- Application architecture and file organization
- TypeScript type system and interface design
- Database schema design (Prisma ORM)
- NextAuth.js v5 architecture patterns
- Separation of concerns and modular design

You write well-structured scaffolding that prioritizes:
1. Clear separation of concerns (auth, database, types)
2. Type safety (strict TypeScript interfaces)
3. Scalability (easy to extend in future phases)
4. Maintainability (clear structure, good naming)

COMPONENT 2: CONTEXT INJECTION
────────────────────────────────────────────────────────

LAYER 1: PROJECT IDENTITY
- Name: TaskMaster Pro
- Type: Web Application (SaaS)
- Industry: Project Management / Productivity
- Stage: MVP Development
- Target Users: Freelancers and small teams

LAYER 3: TECHNICAL CONTEXT
- Framework: Next.js 15.0.3 (App Router)
- Language: TypeScript 5.3.3 (strict mode)
- Database: PostgreSQL 16
- ORM: Prisma 5.7.1
- Authentication: NextAuth.js v5 (to be scaffolded)
- Hosting: Vercel

PROJECT STRUCTURE:
```
app/
├── (auth)/
├── (dashboard)/
└── api/
lib/
├── db.ts (Prisma client)
types/
components/
```

CODING CONVENTIONS:
- Imports: Absolute with @ alias
- Naming: camelCase functions, PascalCase components/types
- Async: async/await only

LAYER 4: FEATURE CONTEXT

USER STORY:
As a new user
I want to register and login with email/password or Google
So that I can access the platform securely

ACCEPTANCE CRITERIA:
1. User can register with email + password
2. User can login with email + password
3. User can login with Google OAuth
4. Sessions persist across browser restarts
5. User can logout (session destroyed)

COMPONENT 3: TASK DEFINITION
────────────────────────────────────────────────────────

TASK: Generate authentication system SCAFFOLDING ONLY

SCOPE:

IN SCOPE (This Phase - Scaffolding):
- Complete file structure (all necessary files created)
- TypeScript type definitions (User, Session, Account interfaces)
- Prisma schema (User, Session, Account models with relations)
- Function signatures with TODO comments
- NextAuth.js configuration skeleton
- Environment variable template

OUT OF SCOPE (Defer to Phase 2 - Implementation):
- Actual NextAuth configuration logic
- Password hashing implementation
- Google OAuth provider setup
- Session callback implementations
- Rate limiting middleware
- Login/Register UI components

SUCCESS CRITERIA:
- All files created in correct locations
- TypeScript compiles successfully (npm run type-check passes)
- No actual implementation (TODO comments mark future work)
- Prisma schema valid (npx prisma validate passes)
- Clear path for Phase 2 (Implementation)

PHASE: 1/4 - Scaffolding
NEXT PHASE: Phase 2 - Implementation

ESTIMATED TIME: 10 minutes

COMPONENT 4: REQUIREMENTS SPECIFICATION
────────────────────────────────────────────────────────

STRUCTURAL REQUIREMENTS:

FILE STRUCTURE:
```
lib/
├── auth.ts (NextAuth configuration skeleton)
├── password.ts (password hashing utilities - signatures only)
app/
├── api/
│   └── auth/
│       └── [...nextauth]/
│           └── route.ts (API route handlers)
types/
├── auth.ts (TypeScript interfaces)
prisma/
├── schema.prisma (updated with auth models)
.env.example (environment variables template)
TYPE DEFINITIONS (types/auth.ts):

AuthUser interface (extends Prisma User)
AuthSession interface
AuthConfig type (NextAuth configuration)

FUNCTION SIGNATURES:
lib/auth.ts:
typescriptexport const { handlers, auth, signIn, signOut } = NextAuth({ ... })
// TODO (Phase 2): Complete NextAuth configuration
lib/password.ts:
typescriptexport async function hashPassword(password: string): Promise<string>
// TODO (Phase 2): Implement bcrypt hashing (cost 12)

export async function verifyPassword(password: string, hash: string): Promise<boolean>
// TODO (Phase 2): Implement bcrypt verification
DATA REQUIREMENTS (Prisma Schema):
prismamodel User {
  id            String    @id @default(cuid())
  email         String    @unique
  password      String?   // Nullable (OAuth users)
  name          String
  emailVerified DateTime?
  image         String?
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
  type              String
  provider          String
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

COMPONENT 5: SECURITY MANDATES
────────────────────────────────────────────────────────

SCHEMA-LEVEL SECURITY (Scaffolding Phase):

1. CASCADE DELETES:
   - When User deleted → Sessions deleted
   - When User deleted → Accounts deleted
   - Prevents orphaned data

2. INDEXES:
   - User.email (unique + indexed for fast lookup)
   - Session.userId (indexed for fast user session lookup)
   - Session.sessionToken (unique + indexed for validation)
   - Account.userId (indexed for user account lookup)

3. CONSTRAINTS:
   - User.email UNIQUE (no duplicate accounts)
   - Session.sessionToken UNIQUE (no duplicate sessions)
   - Account [provider, providerAccountId] UNIQUE (no duplicate OAuth links)

4. NULLABLE FIELDS:
   - User.password NULLABLE (OAuth users have no password)
   - User.emailVerified NULLABLE (not verified on registration)

IMPLEMENTATION-LEVEL SECURITY (Phase 2):
- Password hashing (bcrypt cost 12)
- Session management (database-backed)
- Rate limiting (5 attempts / 15 min)
- CSRF protection (NextAuth.js default)

COMPONENT 6: META-INSTRUCTIONS
────────────────────────────────────────────────────────

THINKING PROCESS:

1. CREATE TYPE DEFINITIONS FIRST:
   - Define AuthUser, AuthSession interfaces in types/auth.ts
   - Establish type contracts before implementation
   - Use Prisma-generated types where possible

2. UPDATE PRISMA SCHEMA:
   - Add User, Session, Account models
   - Define relations (user → sessions, user → accounts)
   - Add indexes for performance
   - Add constraints for data integrity

3. CREATE FILE STRUCTURE:
   - lib/auth.ts (NextAuth config skeleton)
   - lib/password.ts (password utilities signatures)
   - app/api/auth/[...nextauth]/route.ts (handlers)
   - .env.example (environment variables)

4. WRITE FUNCTION SIGNATURES:
   - Include JSDoc comments
   - Add TODO comments for Phase 2
   - Define parameter types
   - Define return types

5. VALIDATE SCAFFOLDING:
   - TypeScript compiles (no errors)
   - Prisma schema valid
   - All imports resolve
   - Ready for Phase 2

CONSTRAINTS:

DO NOT (Scaffolding Phase):
❌ Implement actual authentication logic
❌ Add password hashing code
❌ Configure OAuth providers
❌ Add error handling logic
❌ Create UI components
❌ Add rate limiting

DO (Scaffolding Phase):
✓ Create all file structures
✓ Define all type interfaces
✓ Write function signatures with JSDoc
✓ Update Prisma schema with models
✓ Add TODO comments marking implementation points
✓ Create .env.example template

BEST PRACTICES:

- TODO comments format: "// TODO (Phase 2): [specific task]"
- Function signatures include JSDoc with @param and @returns
- All types explicitly defined (no 'any')
- Imports use @ alias (absolute imports)

DEPENDENCIES:

MUST EXIST:
✓ Next.js 15 project initialized
✓ Prisma installed and configured
✓ TypeScript configured (strict mode)

WILL BE INSTALLED (Phase 2):
- next-auth
- @auth/prisma-adapter
- bcrypt
- @upstash/ratelimit

REFERENCES:

- NextAuth.js v5 Docs: https://next-auth.js.org
- Prisma Schema: https://www.prisma.io/docs/reference/api-reference/prisma-schema-reference

COMPONENT 7: OUTPUT FORMAT & VALIDATION
────────────────────────────────────────────────────────

DELIVERABLES:

1. FILE LISTING:
```
   ✓ types/auth.ts
   ✓ lib/auth.ts
   ✓ lib/password.ts
   ✓ app/api/auth/[...nextauth]/route.ts
   ✓ prisma/schema.prisma (additions)
   ✓ .env.example

COMPLETE SCAFFOLDING CODE (with TODO markers)
VALIDATION CHECKLIST (below)
NEXT STEPS (Phase 2 guidance)

VALIDATION CHECKLIST:
After receiving scaffolding:
□ All 6 files created in correct locations
□ TypeScript compiles: npm run type-check (no errors)
□ Prisma schema valid: npx prisma validate
□ All imports resolve (no red squiggles)
□ TODO comments present (mark implementation points)
□ No actual implementation present (scaffolding only)
□ Function signatures include types and JSDoc
□ Ready to request Phase 2
SETUP INSTRUCTIONS:

Run Prisma migration:

bash   npx prisma migrate dev --name add-auth-scaffolding

Copy environment template:

bash   cp .env.example .env.local

Verify TypeScript:

bash   npm run type-check
```
   Expected: ✓ No errors

NEXT STEPS:

CURRENT: Phase 1 (Scaffolding) - COMPLETE ✓

NEXT: Request "Phase 2: Implementation"

Phase 2 will add:
- Complete NextAuth.js configuration
- Password hashing (bcrypt)
- Google OAuth provider
- Session callbacks
- Error handling

Estimated: 25 minutes

═══════════════════════════════════════════════════════════
END OF SCAFFOLDING PROMPT
═══════════════════════════════════════════════════════════
```

---

### Pattern #1 Validation Checklist
```markdown
SCAFFOLDING PATTERN QUALITY CHECKLIST:

STRUCTURE:
□ All necessary files identified
□ File paths correct (Next.js conventions)
□ Folder structure logical

TYPE DEFINITIONS:
□ All interfaces defined
□ No 'any' types
□ Extends Prisma types where appropriate

FUNCTION SIGNATURES:
□ All public functions have signatures
□ JSDoc comments present (@param, @returns)
□ TODO comments mark implementation points

PRISMA SCHEMA:
□ All models defined
□ Relations correct (foreign keys)
□ Indexes on foreign keys
□ Cascade deletes configured

TODO MARKERS:
□ Format: "// TODO (Phase 2): [specific task]"
□ Every unimplemented function has TODO
□ TODOs reference next phase

NO IMPLEMENTATION:
□ No actual logic (only signatures)
□ No complex error handling
□ No complete configurations

GRADING:
All checks pass: ⭐⭐⭐⭐⭐ Perfect scaffolding
1-2 issues: ⭐⭐⭐⭐ Good scaffolding
3-4 issues: ⭐⭐⭐ Acceptable
>4 issues: ⭐⭐ Needs improvement
```

---

## 🔨 PATTERN #2: IMPLEMENTATION

### Purpose

Generate complete, production-ready implementation with full logic, error handling, and security features. Can be used standalone (simple features) or after Pattern #1 (complex features).

### When to Use
```
✅ USE IMPLEMENTATION WHEN:
- Building complete feature from scratch
- Feature is simple-to-medium complexity (<500 lines)
- After scaffolding phase (Phase 2 of multi-phase)
- Need production-ready code immediately

✅ EXAMPLES:
- "Implement user profile page" (straightforward feature)
- "Complete the authentication logic" (after scaffolding)
- "Build password reset flow" (medium complexity)

❌ DON'T USE IMPLEMENTATION WHEN:
- Just need structure (use Pattern #1 Scaffolding)
- Modifying existing code (use Pattern #3 Delta Editing)
- Debugging (use Pattern #4 Debugging)
```

---

### Pattern Structure

**7-Component Adaptation:**
```markdown
COMPONENT 1: PERSONA
"You are a senior full-stack developer specializing in [domain]..."
Focus: Complete, production-ready implementation

COMPONENT 2: CONTEXT
Include: ALL 5 layers (complete context needed)

COMPONENT 3: TASK
Task: "Implement COMPLETE [feature] with full logic and error handling"
IN SCOPE:
- Complete implementation (not just signatures)
- All business logic
- Error handling (try-catch, validation)
- Edge case handling
- Security features (validation, sanitization)

OUT OF SCOPE:
- UI components (if backend feature)
- Testing (separate phase)
- Documentation (inline comments sufficient)

SUCCESS CRITERIA:
- Feature works end-to-end
- All acceptance criteria met
- Error handling complete
- TypeScript compiles
- Manual test passes

COMPONENT 4: REQUIREMENTS
COMPLETE requirements:
- Functional (all user actions)
- Technical (all implementation details)
- Non-functional (performance, security)
- Edge cases (all unusual scenarios)
- Data (complete schema if needed)

COMPONENT 5: SECURITY MANDATES
Include: ALL relevant commandments (full security implementation)

COMPONENT 6: META-INSTRUCTIONS
THINKING PROCESS: Full implementation sequence
CONSTRAINTS: No shortcuts, production-ready quality required

COMPONENT 7: OUTPUT
DELIVERABLES: Complete, working code
VALIDATION: Manual test checklist
NEXT STEPS: Testing phase or next feature
```

---

### Complete Example - Password Reset Implementation
```markdown
═══════════════════════════════════════════════════════════
CONSTITUTIONAL PROMPT: PASSWORD RESET IMPLEMENTATION
Pattern #2: Implementation
═══════════════════════════════════════════════════════════

COMPONENT 1: PERSONA ASSIGNMENT
────────────────────────────────────────────────────────

You are a senior full-stack developer specializing in authentication 
security and Next.js with 10+ years experience.

Your expertise includes:
- Secure password reset flows (token-based)
- Email integration (SendGrid, Resend)
- Cryptographic token generation (crypto.randomBytes)
- Token storage and validation (database-backed)
- OWASP A07 (Authentication Failures) mitigation

You write production-ready code that prioritizes:
1. Security (constitutional mandates, OWASP compliance)
2. User experience (clear feedback, helpful errors)
3. Reliability (handles edge cases, fails gracefully)
4. Maintainability (clean code, documented, testable)

COMPONENT 2: CONTEXT INJECTION
────────────────────────────────────────────────────────

LAYER 1: PROJECT IDENTITY
- Name: TaskMaster Pro
- Type: Web Application (SaaS)
- Stage: MVP Development
- Target Users: Freelancers and small teams

LAYER 3: TECHNICAL CONTEXT
- Framework: Next.js 15.0.3 (App Router)
- Language: TypeScript 5.3.3 (strict mode)
- Database: PostgreSQL 16 + Prisma 5.7.1
- Email: SendGrid (configured, API key in env)
- Authentication: NextAuth.js v5

EXISTING CODE:
- User model exists (email, password fields)
- bcrypt configured (cost 12)
- NextAuth configured (database sessions)

LAYER 4: FEATURE CONTEXT

USER STORY:
As a registered user who forgot my password
I want to reset it via email link
So that I can regain access to my account

ACCEPTANCE CRITERIA:
1. User clicks "Forgot Password" → enters email → receives reset email
2. Email contains unique reset link (valid 24 hours)
3. User clicks link → enters new password → password updated
4. New password hashed with bcrypt (cost 12)
5. User automatically logged in after successful reset
6. Expired links show error with option to request new reset

EDGE CASES:
- Email not registered → Generic message (no user enumeration)
- Multiple reset requests → Only latest token valid
- Token already used → Error: "Link already used"
- Weak password → Specific validation errors
- Token expired (>24h) → Error: "Link expired"

LAYER 5: CONSTRAINTS
- Budget: Using SendGrid free tier (100 emails/day limit)
- Timeline: Complete in 2 hours
- Compliance: GDPR (EU users) - generic error messages required

COMPONENT 3: TASK DEFINITION
────────────────────────────────────────────────────────

TASK: Implement complete password reset flow with email verification

SCOPE:

IN SCOPE (Complete Implementation):
- "Forgot Password" API endpoint (POST /api/auth/forgot-password)
- Reset token generation (crypto.randomBytes, 32 bytes)
- Reset token storage (ResetToken table in database)
- Email sending (SendGrid integration)
- Password reset page (app/(auth)/reset-password/page.tsx)
- Password reset API (POST /api/auth/reset-password)
- Token validation (expiry, single-use)
- Password update (bcrypt hash)
- Automatic login after reset
- Rate limiting (3 reset requests per hour per email)

OUT OF SCOPE:
- SMS-based reset (future enhancement)
- Security questions (not recommended)
- Password reset from settings (logged-in flow, separate)

SUCCESS CRITERIA:
- User can request reset (receives email within 1 min)
- Email link works (loads reset page)
- Can set new password (meets validation)
- Old password no longer works
- Token expires after 24 hours
- Token single-use (cannot reuse)
- Rate limited (3 requests / hour / email)

PHASE: Implementation (Complete Feature)
NEXT PHASE: Testing & Evaluation

ESTIMATED TIME: 2 hours

COMPONENT 4: REQUIREMENTS SPECIFICATION
────────────────────────────────────────────────────────

[... Full requirements specification as shown in Component 4 section of Article I ...]

COMPONENT 5: SECURITY MANDATES
────────────────────────────────────────────────────────

COMMANDMENT I: INPUT VALIDATION

[... Full commandment details as shown in Component 5 section ...]

COMMANDMENT III: PASSWORD SECURITY

[... Full commandment details ...]

COMMANDMENT V: RATE LIMITING

[... Full commandment details ...]

COMMANDMENT VII: DATA PROTECTION

[... Full commandment details ...]

COMMANDMENT VIII: ERROR HANDLING

[... Full commandment details ...]

COMPONENT 6: META-INSTRUCTIONS
────────────────────────────────────────────────────────

THINKING PROCESS:

1. CREATE DATABASE MODEL:
   - Add ResetToken model to Prisma schema
   - Fields: id, userId, hashedToken, expiresAt, usedAt
   - Relations: user (foreign key)
   - Indexes: hashedToken (unique), userId
   - Migration: npx prisma migrate dev --name add-reset-tokens

2. IMPLEMENT REQUEST RESET ENDPOINT:
   - POST /api/auth/forgot-password
   - Validate email (Zod)
   - Check user exists (don't reveal if not)
   - Generate token (crypto.randomBytes(32))
   - Hash token (SHA-256 before storage)
   - Store in database (userId, hashedToken, expiresAt)
   - Send email (SendGrid)
   - Rate limit (3 per hour per email)

3. IMPLEMENT EMAIL SENDING:
   - Create lib/email.ts utility
   - SendGrid template or HTML email
   - Include reset link with token
   - Handle failures gracefully (retry logic)

4. IMPLEMENT RESET PAGE:
   - app/(auth)/reset-password/page.tsx
   - Extract token from URL (?token=abc123)
   - Validate token (API call)
   - Show form if valid, error if not
   - Password input with requirements
   - Submit → API call

5. IMPLEMENT RESET ENDPOINT:
   - POST /api/auth/reset-password
   - Validate token (exists, not expired, not used)
   - Validate new password (Zod)
   - Hash password (bcrypt cost 12)
   - Update User.password
   - Mark token as used (usedAt = now)
   - Create session (auto-login)
   - Return success

6. TEST COMPLETE FLOW:
   - Request reset → Receive email
   - Click link → See reset form
   - Submit new password → Password updated
   - Try old password → Fails
   - Try reset link again → Already used error

CONSTRAINTS:

DO NOT:
❌ Store reset token in plaintext (hash with SHA-256)
❌ Allow unlimited reset requests (rate limit: 3/hour)
❌ Skip email validation (always validate server-side)
❌ Expose "user not found" (use generic message)
❌ Allow weak passwords (enforce requirements)
❌ Allow expired tokens (check expiresAt)
❌ Allow token reuse (check usedAt)

DO:
✓ Hash tokens before storage (SHA-256)
✓ Set expiration (24 hours from creation)
✓ Mark tokens as used (single-use enforcement)
✓ Rate limit reset requests (prevent abuse)
✓ Generic error messages (prevent user enumeration)
✓ Validate password strength (Zod schema)
✓ Hash passwords (bcrypt cost 12)
✓ Auto-login after successful reset

BEST PRACTICES:

[... Full best practices section ...]

DEPENDENCIES:

MUST EXIST:
✓ User model (email, password fields)
✓ bcrypt configured
✓ SendGrid API key in environment

WILL BE CREATED:
- ResetToken model (Prisma)
- Email utility (lib/email.ts)
- API routes (forgot-password, reset-password)
- Reset page UI

REFERENCES:

- SendGrid API: https://docs.sendgrid.com/api-reference
- crypto.randomBytes: https://nodejs.org/api/crypto.html
- bcrypt: https://github.com/kelektiv/node.bcrypt.js

COMPONENT 7: OUTPUT FORMAT & VALIDATION
────────────────────────────────────────────────────────

DELIVERABLES:

1. PRISMA SCHEMA UPDATE (ResetToken model)
2. EMAIL UTILITY (lib/email.ts)
3. FORGOT PASSWORD API (POST /api/auth/forgot-password)
4. RESET PASSWORD API (POST /api/auth/reset-password)
5. RESET PASSWORD PAGE (app/(auth)/reset-password/page.tsx)
6. RATE LIMIT CONFIGURATION

VALIDATION CHECKLIST:

FUNCTIONALITY:
□ Can request reset (email sent)
□ Email contains valid reset link
□ Reset link loads reset page
□ Can submit new password
□ Password updated in database (bcrypt hashed)
□ Automatically logged in after reset
□ Old password no longer works

SECURITY:
□ Token stored hashed (not plaintext)
□ Token expires after 24 hours
□ Token single-use (cannot reuse)
□ Rate limited (3 requests / hour)
□ Generic errors (no user enumeration)
□ Password validated (strength requirements)

EDGE CASES:
□ Email not registered → Generic message
□ Multiple requests → Latest token valid
□ Expired token → Clear error message
□ Used token → Clear error message
□ Weak password → Specific validation errors

NEXT STEPS:

CURRENT: Password Reset Implementation - COMPLETE ✓

READY FOR: Manual Testing

Test these scenarios:
1. Happy path (request → receive → reset → login)
2. Expired token (wait 24h or manually expire in DB)
3. Used token (use once, try again)
4. Email not registered
5. Weak password
6. Rate limit (make 4 requests in 1 hour)

After testing passes, proceed to Phase E (Evaluation).

═══════════════════════════════════════════════════════════
END OF IMPLEMENTATION PROMPT
═══════════════════════════════════════════════════════════
```

---

**[CONTINUING WITH REMAINING PATTERNS IN NEXT RESPONSE...]**

**Current Progress:** 2/12 patterns documented

Should I continue with Patterns #3-12? 🚀
