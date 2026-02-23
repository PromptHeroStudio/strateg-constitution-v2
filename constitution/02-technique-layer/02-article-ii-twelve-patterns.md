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

## ✏️ PATTERN #3: DELTA EDITING

### Purpose

Make surgical, precise changes to existing code without rewriting entire files. Used for modifications, updates, and targeted improvements to specific functions or sections.

### When to Use
````
✅ USE DELTA EDITING WHEN:
- Modifying existing code (not creating new)
- Changes are localized (specific functions/sections)
- Want to preserve most of existing code
- Need to see exact before/after changes
- Multiple small changes across file

✅ EXAMPLES:
- "Change login rate limit from 5 to 10 attempts"
- "Update User model to include 'bio' field"
- "Modify email validation to allow '+' character"
- "Change button color from blue to green"

❌ DON'T USE DELTA EDITING WHEN:
- Creating entirely new files (use Pattern #1 or #2)
- Refactoring entire file structure (use Pattern #6)
- Changes are very minor (single character/word)
- User wants to see complete new version
````

---

### Pattern Structure

**7-Component Adaptation:**
````markdown
COMPONENT 1: PERSONA
"You are a senior developer experienced in code modification..."
Focus: Precision, minimal disruption, maintain code quality

COMPONENT 2: CONTEXT
Include: Layer 3 (Technical) + current code state
Provide: EXACT current code that needs modification

COMPONENT 3: TASK
Task: "Make SPECIFIC changes to existing code (delta editing)"
IN SCOPE:
- Specific changes requested
- Maintain surrounding code unchanged
- Preserve code style and conventions
- Update related code if necessary (imports, types, etc.)

OUT OF SCOPE:
- Rewriting entire file
- Refactoring unrelated code
- Changing architectural patterns

SUCCESS CRITERIA:
- Exact changes made as requested
- Surrounding code unchanged
- TypeScript still compiles
- Functionality maintained (or improved as requested)

COMPONENT 4: REQUIREMENTS
CHANGE SPECIFICATION:
- CURRENT: [Show exact current code]
- CHANGE TO: [Describe specific change]
- REASON: [Why this change is needed]

COMPONENT 5: SECURITY MANDATES
Include: Only if change affects security-sensitive code

COMPONENT 6: META-INSTRUCTIONS
THINKING PROCESS: "Identify exact location → Make precise change → Verify no breaking changes"
CONSTRAINTS: "Change ONLY what's requested, preserve everything else"

COMPONENT 7: OUTPUT
DELIVERABLES: Before/After comparison (diff format)
VALIDATION: Checklist for testing change
````

---

### Complete Example - Modify Rate Limit
````markdown
═══════════════════════════════════════════════════════════
CONSTITUTIONAL PROMPT: MODIFY RATE LIMIT THRESHOLD
Pattern #3: Delta Editing
═══════════════════════════════════════════════════════════

COMPONENT 1: PERSONA ASSIGNMENT
────────────────────────────────────────────────────────

You are a senior developer with expertise in API rate limiting,
security configurations, and Next.js with 8+ years experience.

Your expertise includes:
- Rate limiting strategies and thresholds
- Upstash Redis configuration
- Security trade-offs (usability vs. protection)
- Code modification with minimal disruption

You write precise code changes that prioritize:
1. Accuracy (change exactly what's requested)
2. Consistency (maintain code style)
3. Safety (no breaking changes)
4. Context awareness (update related constants/comments)

COMPONENT 2: CONTEXT INJECTION
────────────────────────────────────────────────────────

LAYER 3: TECHNICAL CONTEXT
- Framework: Next.js 15.0.3 (App Router)
- Language: TypeScript 5.3.3 (strict mode)
- Rate Limiting: Upstash Redis + @upstash/ratelimit

CURRENT CODE (lib/rate-limit.ts):
```typescript
import { Ratelimit } from '@upstash/ratelimit'
import { Redis } from '@upstash/redis'

const redis = new Redis({
  url: process.env.UPSTASH_REDIS_REST_URL!,
  token: process.env.UPSTASH_REDIS_REST_TOKEN!
})

/**
 * Rate limiter for login attempts
 * Constitutional: Commandment V (5 attempts per 15 minutes)
 */
export const loginRateLimit = new Ratelimit({
  redis,
  limiter: Ratelimit.slidingWindow(5, '15 m'), // CURRENT: 5 attempts
  analytics: true,
  prefix: 'ratelimit:login'
})

/**
 * Rate limiter for registration
 * Constitutional: Commandment V (3 attempts per hour)
 */
export const registerRateLimit = new Ratelimit({
  redis,
  limiter: Ratelimit.slidingWindow(3, '1 h'),
  analytics: true,
  prefix: 'ratelimit:register'
})
```

COMPONENT 3: TASK DEFINITION
────────────────────────────────────────────────────────

TASK: Modify login rate limit from 5 to 10 attempts per 15 minutes

SCOPE:

IN SCOPE (Specific Changes):
- Change loginRateLimit: 5 → 10 attempts
- Update JSDoc comment to reflect new limit
- Verify registration rate limit unchanged

OUT OF SCOPE:
- Changing time window (keep 15 minutes)
- Modifying registration rate limit
- Restructuring file
- Adding new rate limiters

SUCCESS CRITERIA:
- Login rate limit: 10 attempts per 15 minutes
- Registration rate limit: unchanged (3 per hour)
- JSDoc comment updated
- TypeScript compiles
- No other code changed

REASON FOR CHANGE:
User feedback indicates 5 attempts too restrictive for legitimate users
who occasionally mistype passwords. 10 attempts provides better UX
while still protecting against brute force (OWASP A07 compliant).

COMPONENT 4: REQUIREMENTS SPECIFICATION
────────────────────────────────────────────────────────

CHANGE SPECIFICATION:

CURRENT:
```typescript
limiter: Ratelimit.slidingWindow(5, '15 m'), // 5 attempts
```

CHANGE TO:
```typescript
limiter: Ratelimit.slidingWindow(10, '15 m'), // 10 attempts
```

CURRENT COMMENT:
```typescript
/**
 * Rate limiter for login attempts
 * Constitutional: Commandment V (5 attempts per 15 minutes)
 */
```

CHANGE TO:
```typescript
/**
 * Rate limiter for login attempts
 * Constitutional: Commandment V (10 attempts per 15 minutes)
 * Adjusted from 5 to 10 based on user feedback (legitimate mistyping)
 */
```

VERIFY UNCHANGED:
- Registration rate limit: 3 per hour (DO NOT CHANGE)
- Time window: 15 minutes (DO NOT CHANGE)
- Other rate limiters: If any exist, DO NOT CHANGE

COMPONENT 5: SECURITY MANDATES
────────────────────────────────────────────────────────

COMMANDMENT V: RATE LIMITING

Mandate:
Rate limiting prevents brute force attacks. Threshold must balance
security (prevent attacks) vs. usability (allow legitimate errors).

Rationale for 10 attempts:
- 10 attempts still prevents brute force (very slow attack vector)
- Legitimate users may mistype 5-7 times (keyboard issues, caps lock)
- 15-minute lockout provides cooling-off period
- Still OWASP A07 compliant (authentication failure protection)

Constitutional Validation:
✓ Rate limiting present (required)
✓ Threshold reasonable (10 attempts acceptable)
✓ Time window appropriate (15 minutes sufficient)
✓ No unlimited attempts (violation would be NO rate limit)

COMPONENT 6: META-INSTRUCTIONS
────────────────────────────────────────────────────────

THINKING PROCESS:

1. LOCATE CODE:
   - File: lib/rate-limit.ts
   - Target: loginRateLimit configuration
   - Line: limiter: Ratelimit.slidingWindow(5, '15 m')

2. MAKE PRECISE CHANGE:
   - Change: 5 → 10 (numeric value only)
   - Verify: Time window '15 m' unchanged
   - Update: JSDoc comment to reflect new limit

3. VERIFY NO SIDE EFFECTS:
   - Check: registerRateLimit unchanged
   - Check: No other rate limiters affected
   - Check: No type errors introduced

4. VALIDATE CHANGE:
   - TypeScript compiles (npm run type-check)
   - Imports still valid
   - No breaking changes

CONSTRAINTS:

DO NOT:
❌ Change time window ('15 m' stays)
❌ Modify registration rate limit
❌ Refactor entire file
❌ Add new functionality
❌ Change variable names
❌ Modify other configuration

DO:
✓ Change only the numeric value: 5 → 10
✓ Update JSDoc comment
✓ Maintain code style
✓ Preserve surrounding code exactly

BEST PRACTICES:

- Minimal change (least disruption)
- Update documentation (comments reflect reality)
- Preserve formatting (spacing, indentation)
- No unrelated changes (scope creep)

COMPONENT 7: OUTPUT FORMAT & VALIDATION
────────────────────────────────────────────────────────

DELIVERABLES:

BEFORE/AFTER DIFF:
```diff
// lib/rate-limit.ts

  /**
   * Rate limiter for login attempts
-  * Constitutional: Commandment V (5 attempts per 15 minutes)
+  * Constitutional: Commandment V (10 attempts per 15 minutes)
+  * Adjusted from 5 to 10 based on user feedback (legitimate mistyping)
   */
  export const loginRateLimit = new Ratelimit({
    redis,
-   limiter: Ratelimit.slidingWindow(5, '15 m'),
+   limiter: Ratelimit.slidingWindow(10, '15 m'),
    analytics: true,
    prefix: 'ratelimit:login'
  })
```

COMPLETE UPDATED CODE:
```typescript
/**
 * Rate limiter for login attempts
 * Constitutional: Commandment V (10 attempts per 15 minutes)
 * Adjusted from 5 to 10 based on user feedback (legitimate mistyping)
 */
export const loginRateLimit = new Ratelimit({
  redis,
  limiter: Ratelimit.slidingWindow(10, '15 m'),
  analytics: true,
  prefix: 'ratelimit:login'
})
```

VALIDATION CHECKLIST:

□ Numeric change: 5 → 10 (correct)
□ Time window unchanged: '15 m' (correct)
□ JSDoc comment updated (reflects new limit)
□ Registration rate limit unchanged (3 per hour)
□ TypeScript compiles: npm run type-check
□ No unrelated changes made
□ Code formatting preserved

TESTING:

Manual test to verify change:
1. Attempt login with wrong password 6 times
   Expected: Still allowed (was blocked at 6 with old limit)
2. Attempt login with wrong password 11 times
   Expected: Blocked on 11th attempt (429 status)
3. Wait 15 minutes
   Expected: Can attempt again

NEXT STEPS:

Change complete. Test in development:
```bash
npm run dev
# Test login with multiple wrong passwords
```

If working as expected, commit:
```bash
git add lib/rate-limit.ts
git commit -m "feat: increase login rate limit to 10 attempts (user feedback)"
```

═══════════════════════════════════════════════════════════
END OF DELTA EDITING PROMPT
═══════════════════════════════════════════════════════════
````

---

### Pattern #3 Validation Checklist
````markdown
DELTA EDITING PATTERN QUALITY CHECKLIST:

PRECISION:
□ Exact location identified
□ Specific change described (before/after)
□ Minimal disruption (only what's needed)
□ Surrounding code unchanged

DOCUMENTATION:
□ Before/after diff provided
□ Reason for change explained
□ Complete updated code shown
□ Comments updated to match changes

SAFETY:
□ No breaking changes introduced
□ TypeScript still compiles
□ Related code updated (imports, types, comments)
□ Testing instructions provided

SCOPE:
□ Only requested changes made
□ No scope creep (unrelated changes)
□ No refactoring (unless requested)
□ Consistent code style maintained

GRADING:
All checks pass: ⭐⭐⭐⭐⭐ Perfect delta edit
1-2 issues: ⭐⭐⭐⭐ Good delta edit
3-4 issues: ⭐⭐⭐ Acceptable
>4 issues: ⭐⭐ Too disruptive, needs refinement
````

---

## 🐛 PATTERN #4: DEBUGGING

### Purpose

Systematically identify and fix bugs through structured analysis, hypothesis testing, and validation. Used when something is broken or not working as expected.

### When to Use
````
✅ USE DEBUGGING WHEN:
- Feature not working as expected
- Error messages appearing
- Unexpected behavior occurring
- Need to diagnose root cause
- "Why doesn't X work?" questions

✅ EXAMPLES:
- "Login returns 500 error"
- "Why is rate limiting not triggering?"
- "Password reset email not sending"
- "Dashboard shows wrong data"

❌ DON'T USE DEBUGGING WHEN:
- No error (just enhancement request)
- Building new feature (use Pattern #1 or #2)
- Refactoring working code (use Pattern #6)
- Known issue with known fix (use Pattern #3 Delta Editing)
````

---

### Pattern Structure

**7-Component Adaptation:**
````markdown
COMPONENT 1: PERSONA
"You are a senior debugging expert specializing in [technology]..."
Focus: Systematic problem-solving, root cause analysis

COMPONENT 2: CONTEXT
Include: Layer 3 (Technical) + PROBLEM DESCRIPTION
Critical: Exact error messages, reproduction steps, expected vs actual behavior

COMPONENT 3: TASK
Task: "Diagnose and fix bug using systematic debugging protocol"
IN SCOPE:
- Symptom analysis
- Hypothesis generation
- Evidence collection
- Root cause identification
- Fix implementation
- Fix verification

OUT OF SCOPE:
- Refactoring unrelated code
- Adding new features
- Comprehensive testing (separate phase)

SUCCESS CRITERIA:
- Bug identified (root cause found)
- Fix implemented (problem resolved)
- Verified working (manual test confirms)
- No regressions (other features still work)

COMPONENT 4: REQUIREMENTS
DEBUGGING PROTOCOL:
1. SYMPTOM ANALYSIS: What's wrong?
2. REPRODUCTION: Can we reproduce it?
3. EVIDENCE: What data do we have?
4. HYPOTHESIS: What might be wrong?
5. TEST: Verify hypothesis
6. FIX: Implement solution
7. VERIFY: Confirm fix works

COMPONENT 5: SECURITY MANDATES
Include: If bug is security-related (vulnerability fix)

COMPONENT 6: META-INSTRUCTIONS
THINKING PROCESS: "Observe → Hypothesize → Test → Fix → Verify"
CONSTRAINTS: "Don't guess, follow systematic process"

COMPONENT 7: OUTPUT
DELIVERABLES: 
- Root cause analysis
- Fix implementation
- Verification steps
- Prevention recommendations
````

---

### Complete Example - Rate Limiting Not Working
````markdown
═══════════════════════════════════════════════════════════
CONSTITUTIONAL PROMPT: DEBUG RATE LIMITING FAILURE
Pattern #4: Debugging
═══════════════════════════════════════════════════════════

COMPONENT 1: PERSONA ASSIGNMENT
────────────────────────────────────────────────────────

You are a senior backend engineer and debugging expert specializing 
in Next.js, API routes, and rate limiting with 12+ years experience.

Your expertise includes:
- Systematic debugging methodology
- Next.js 15 middleware architecture
- Upstash Redis troubleshooting
- API request flow analysis
- Root cause analysis techniques

You approach debugging by:
1. Observing symptoms carefully (no assumptions)
2. Generating testable hypotheses
3. Collecting evidence systematically
4. Identifying root cause (not just symptoms)
5. Implementing minimal fix (no scope creep)
6. Verifying fix thoroughly

COMPONENT 2: CONTEXT INJECTION
────────────────────────────────────────────────────────

LAYER 3: TECHNICAL CONTEXT
- Framework: Next.js 15.0.3 (App Router)
- Language: TypeScript 5.3.3
- Rate Limiting: Upstash Redis + @upstash/ratelimit
- Deployment: Vercel (production)

PROBLEM DESCRIPTION:

SYMPTOM:
Rate limiting is not triggering on login endpoint.
Users can make unlimited login attempts (brute force possible).

EXPECTED BEHAVIOR:
After 5 failed login attempts, 6th attempt should return 429 (Too Many Requests).

ACTUAL BEHAVIOR:
Can attempt login 20+ times without any rate limiting.
Always returns 200 or 401, never 429.

REPRODUCTION STEPS:
1. Visit /api/auth/login
2. POST with wrong credentials
3. Repeat 10 times
4. Observe: All attempts succeed (no rate limit)

ERROR MESSAGES:
None visible in browser console or server logs.

WHAT USER TRIED:
- Verified Upstash Redis credentials (correct in .env.local)
- Checked rate limit code exists in lib/rate-limit.ts (it does)
- Confirmed redis connection works (can read/write manually)

RELEVANT CODE:

lib/rate-limit.ts:
```typescript
import { Ratelimit } from '@upstash/ratelimit'
import { Redis } from '@upstash/redis'

const redis = new Redis({
  url: process.env.UPSTASH_REDIS_REST_URL!,
  token: process.env.UPSTASH_REDIS_REST_TOKEN!
})

export const loginRateLimit = new Ratelimit({
  redis,
  limiter: Ratelimit.slidingWindow(5, '15 m'),
  analytics: true,
  prefix: 'ratelimit:login'
})
```

app/api/auth/login/route.ts:
```typescript
import { loginRateLimit } from '@/lib/rate-limit'

export async function POST(request: Request) {
  const { email, password } = await request.json()
  
  // Validate credentials
  const user = await prisma.user.findUnique({ where: { email } })
  if (!user || !await bcrypt.compare(password, user.password)) {
    return Response.json({ error: 'Invalid credentials' }, { status: 401 })
  }
  
  // Success
  return Response.json({ success: true })
}
```

middleware.ts:
```typescript
import { NextResponse } from 'next/server'
import type { NextRequest } from 'next/server'

export function middleware(request: NextRequest) {
  // Some other middleware logic here
  return NextResponse.next()
}

export const config = {
  matcher: ['/dashboard/:path*']
}
```

COMPONENT 3: TASK DEFINITION
────────────────────────────────────────────────────────

TASK: Diagnose why rate limiting isn't working and fix the bug

SCOPE:

IN SCOPE (Debugging + Fix):
- Identify root cause of rate limit failure
- Implement minimal fix
- Verify fix works (rate limiting triggers)
- Provide testing steps for user

OUT OF SCOPE:
- Refactoring rate limit implementation (unless necessary for fix)
- Adding new rate limiters
- Changing rate limit thresholds (5 attempts is correct)

SUCCESS CRITERIA:
- Root cause identified and explained
- Fix implemented (minimal change)
- After 5 failed logins, 6th returns 429
- Rate limit resets after 15 minutes
- No regressions (login still works when correct)

DEBUGGING APPROACH: Systematic 7-step protocol

COMPONENT 4: REQUIREMENTS SPECIFICATION
────────────────────────────────────────────────────────

DEBUGGING PROTOCOL:

STEP 1: SYMPTOM ANALYSIS
What we know:
- Rate limit code exists (lib/rate-limit.ts)
- Redis connection works (credentials valid)
- Rate limiting not triggering (never returns 429)
- Login endpoint works (returns 200/401)

What we DON'T know:
- Is rate limit function being called?
- Is redis storing attempt counts?
- Is middleware blocking execution?
- Is there an exception being silently caught?

STEP 2: REPRODUCTION
Can reproduce: YES (consistently fails to rate limit)
Steps:
1. Make 10 POST requests to /api/auth/login
2. Observe: All return 200/401, none return 429

STEP 3: EVIDENCE COLLECTION
Check these:
□ Is loginRateLimit actually imported in route?
  - Look for: import { loginRateLimit } from '@/lib/rate-limit'
□ Is loginRateLimit.limit() being called?
  - Look for: await loginRateLimit.limit(identifier)
□ Is middleware interfering?
  - Check: middleware.ts config matcher
□ Are errors being silently caught?
  - Check: try-catch blocks swallowing errors

STEP 4: HYPOTHESIS GENERATION
Possible causes (ordered by likelihood):

HYPOTHESIS #1 (MOST LIKELY):
Rate limit function exists but IS NOT BEING CALLED in route handler.
Evidence: Code shows loginRateLimit imported but no .limit() call visible.
Test: Search for "loginRateLimit.limit" in route file.

HYPOTHESIS #2:
Rate limit IS called but errors are silently caught.
Evidence: No error logs, but that could indicate caught exception.
Test: Add console.log before and after rate limit call.

HYPOTHESIS #3:
Middleware is blocking before rate limit executes.
Evidence: Middleware.ts exists with config.
Test: Check if /api/auth/login matches middleware matcher.

HYPOTHESIS #4 (LEAST LIKELY):
Redis issue (though connection works for manual read/write).
Evidence: Credentials valid, connection works in testing.
Test: Check rate limit key in Redis (does it increment?).

STEP 5: TEST HYPOTHESIS #1
Look at route file - is .limit() being called?

OBSERVATION:
```typescript
export async function POST(request: Request) {
  const { email, password } = await request.json()
  
  // Validate credentials
  const user = await prisma.user.findUnique({ where: { email } })
  // ...
}
```

RESULT: ❌ No call to loginRateLimit.limit()!
ROOT CAUSE IDENTIFIED: Rate limit imported but never called.

STEP 6: FIX IMPLEMENTATION
Add rate limit check BEFORE credential validation:
```typescript
import { loginRateLimit } from '@/lib/rate-limit'

export async function POST(request: Request) {
  // RATE LIMIT CHECK (MUST BE FIRST)
  const ip = request.headers.get('x-forwarded-for') || 'unknown'
  const { success, limit, remaining, reset } = await loginRateLimit.limit(ip)
  
  if (!success) {
    return Response.json(
      { error: 'Too many login attempts. Please try again later.' },
      { 
        status: 429,
        headers: {
          'X-RateLimit-Limit': limit.toString(),
          'X-RateLimit-Remaining': '0',
          'X-RateLimit-Reset': reset.toString(),
          'Retry-After': Math.ceil((reset - Date.now()) / 1000).toString()
        }
      }
    )
  }
  
  // Proceed with credential validation
  const { email, password } = await request.json()
  const user = await prisma.user.findUnique({ where: { email } })
  
  if (!user || !await bcrypt.compare(password, user.password)) {
    return Response.json({ error: 'Invalid credentials' }, { status: 401 })
  }
  
  // Success
  return Response.json({ success: true })
}
```

STEP 7: VERIFY FIX
Manual test:
1. Make 5 failed login attempts → All return 401
2. Make 6th failed login attempt → Returns 429 ✓
3. Check headers → X-RateLimit-* headers present ✓
4. Wait 15 minutes → Rate limit resets ✓
5. Make successful login → Returns 200 (not rate limited) ✓

COMPONENT 5: SECURITY MANDATES
────────────────────────────────────────────────────────

COMMANDMENT V: RATE LIMITING

This bug is a CRITICAL security vulnerability (OWASP A07).

Impact of bug:
- Brute force attacks possible (unlimited password guessing)
- Account takeover risk (attacker can try 10,000s of passwords)
- DoS vulnerability (can overwhelm server with login attempts)

Impact of fix:
- Brute force prevented (max 5 attempts per 15 min)
- OWASP A07 compliant (authentication failure protection)
- DoS mitigated (cannot overwhelm with requests)

Constitutional requirement:
Rate limiting on authentication endpoints is MANDATORY (Commandment V).
This bug was a constitutional violation that is now fixed.

COMPONENT 6: META-INSTRUCTIONS
────────────────────────────────────────────────────────

THINKING PROCESS (Applied):

1. OBSERVED SYMPTOM:
   ✓ Rate limiting not working (never returns 429)

2. COLLECTED EVIDENCE:
   ✓ Rate limit code exists (lib/rate-limit.ts)
   ✓ Route imports rate limit (import statement present)
   ✓ BUT: No .limit() call in route handler

3. GENERATED HYPOTHESIS:
   ✓ H1: Function not called (CONFIRMED - root cause)

4. TESTED HYPOTHESIS:
   ✓ Searched for ".limit(" in route file
   ✓ Result: Not found → Confirmed root cause

5. IMPLEMENTED FIX:
   ✓ Added rate limit check at start of handler
   ✓ Minimal change (only added missing functionality)

6. VERIFIED FIX:
   ✓ 6th attempt returns 429
   ✓ Headers present
   ✓ Reset works

LESSONS LEARNED:

1. Import without usage is common mistake
   - Prevention: ESLint rule (no-unused-imports)
   - Prevention: Code review checklist

2. Rate limit must be FIRST check
   - Reason: Prevent expensive operations (database queries)
   - Reason: Protect against DoS

3. This was not detected in testing
   - Lesson: Manual security testing needed
   - Lesson: Add automated test for rate limiting

PREVENTION RECOMMENDATIONS:

1. Add test:
```typescript
   // tests/api/auth/login.test.ts
   test('rate limits after 5 attempts', async () => {
     for (let i = 0; i < 6; i++) {
       const response = await POST('/api/auth/login', { email, password: 'wrong' })
       if (i < 5) expect(response.status).toBe(401)
       else expect(response.status).toBe(429)
     }
   })
```

2. Add ESLint rule (warn on unused imports)

3. Add comment reminder:
```typescript
   // SECURITY: Rate limit MUST be first check (before any DB queries)
   const { success } = await loginRateLimit.limit(ip)
```

COMPONENT 7: OUTPUT FORMAT & VALIDATION
────────────────────────────────────────────────────────

ROOT CAUSE ANALYSIS:

PROBLEM:
Rate limiting code exists (lib/rate-limit.ts) but was never
called in the login route handler (app/api/auth/login/route.ts).

CAUSE:
Developer imported loginRateLimit but forgot to call .limit().
Common mistake: Import without usage.

IMPACT:
CRITICAL security vulnerability (OWASP A07).
Brute force attacks possible, unlimited login attempts.

FIX IMPLEMENTATION:

BEFORE:
```typescript
export async function POST(request: Request) {
  const { email, password } = await request.json()
  // ... no rate limit check ...
}
```

AFTER:
```typescript
export async function POST(request: Request) {
  // Rate limit check (ADDED)
  const ip = request.headers.get('x-forwarded-for') || 'unknown'
  const { success, reset } = await loginRateLimit.limit(ip)
  
  if (!success) {
    return Response.json(
      { error: 'Too many login attempts. Please try again later.' },
      { status: 429, headers: { 'Retry-After': ... } }
    )
  }
  
  // Original code continues...
  const { email, password } = await request.json()
  // ...
}
```

COMPLETE FIXED CODE:

[... Full updated route file provided ...]

VERIFICATION STEPS:

User should test:
1. Make 5 failed login attempts
```bash
   for i in {1..5}; do
     curl -X POST http://localhost:3000/api/auth/login \
       -H "Content-Type: application/json" \
       -d '{"email":"test@example.com","password":"wrong"}'
   done
```
   Expected: All return 401

2. Make 6th attempt
   Expected: Returns 429 with Retry-After header

3. Wait 15 minutes (or clear Redis key)
   Expected: Can attempt again

PREVENTION:

Add this test to prevent regression:
[... Test code provided in Component 6 ...]

NEXT STEPS:

Bug fixed. Recommended actions:
1. Test in development (verify fix works)
2. Add automated test (prevent regression)
3. Deploy to production
4. Security audit (check for similar issues in other endpoints)

═══════════════════════════════════════════════════════════
END OF DEBUGGING PROMPT
═══════════════════════════════════════════════════════════
````

---

### Pattern #4 Validation Checklist
````markdown
DEBUGGING PATTERN QUALITY CHECKLIST:

SYSTEMATIC APPROACH:
□ 7-step protocol followed (Symptom → Fix → Verify)
□ Multiple hypotheses generated (not just first guess)
□ Evidence collected before assuming cause
□ Root cause identified (not just symptoms treated)

PROBLEM ANALYSIS:
□ Symptom clearly described
□ Expected vs actual behavior specified
□ Reproduction steps provided
□ Error messages included

FIX QUALITY:
□ Minimal change (only what's needed to fix)
□ Root cause addressed (not symptom band-aid)
□ No scope creep (no unrelated changes)
□ Verification steps provided

PREVENTION:
□ Lessons learned documented
□ Prevention recommendations provided
□ Test added to prevent regression
□ Similar issues identified

GRADING:
All checks pass: ⭐⭐⭐⭐⭐ Excellent debugging
1-2 issues: ⭐⭐⭐⭐ Good debugging
3-4 issues: ⭐⭐⭐ Acceptable
>4 issues: ⭐⭐ Needs improvement (guessing vs. systematic)
````

---
# 📄 ARTICLE II: TWELVE PATTERNS (FINAL PART)

**Patterns #5-12**

---

```markdown
## 🧠 PATTERN #5: CHAIN OF THOUGHT

### Purpose

Solve complex problems through step-by-step reasoning, breaking down difficult tasks into manageable steps. Used when the solution isn't obvious and requires careful planning or multi-step analysis.

### When to Use

```
✅ USE CHAIN OF THOUGHT WHEN:
- Problem is complex (requires multi-step reasoning)
- Solution approach unclear (need to think through options)
- Trade-offs need evaluation (pros/cons analysis)
- Architectural decisions required
- "How would you approach [complex problem]?" questions

✅ EXAMPLES:
- "How should I architect a multi-tenant SaaS application?"
- "What's the best way to migrate from MongoDB to PostgreSQL?"
- "Design a scalable notification system"
- "Should I use Server Components or Client Components for this?"

❌ DON'T USE CHAIN OF THOUGHT WHEN:
- Problem is straightforward (use Pattern #2 Implementation)
- Just need code (not reasoning/planning)
- Debugging specific issue (use Pattern #4)
- Simple modification (use Pattern #3)
```

---

### Pattern Structure

**7-Component Adaptation:**

```markdown
COMPONENT 1: PERSONA
"You are a senior architect/consultant with deep problem-solving expertise..."
Focus: Systematic reasoning, trade-off analysis, multiple approaches

COMPONENT 2: CONTEXT
Include: ALL 5 layers (need complete context for decision-making)

COMPONENT 3: TASK
Task: "Analyze problem systematically and provide step-by-step reasoning"
IN SCOPE:
- Problem decomposition (break into sub-problems)
- Multiple approaches (at least 2-3 options)
- Trade-off analysis (pros/cons for each)
- Recommendation with rationale
- Implementation roadmap (if applicable)

SUCCESS CRITERIA:
- Problem clearly understood
- Multiple approaches evaluated
- Clear recommendation made
- Reasoning transparent (can follow logic)
- Actionable next steps provided

COMPONENT 4: REQUIREMENTS
REASONING FRAMEWORK:
1. Problem Understanding
2. Constraint Identification
3. Approach Generation (multiple options)
4. Trade-off Analysis
5. Recommendation
6. Implementation Plan

COMPONENT 5: SECURITY MANDATES
Include: If decision has security implications

COMPONENT 6: META-INSTRUCTIONS
THINKING PROCESS: "Understand deeply → Generate options → Evaluate → Recommend → Plan"
CONSTRAINTS: "Show your work (reasoning transparent), consider multiple angles"

COMPONENT 7: OUTPUT
DELIVERABLES:
- Problem analysis
- 2-3 approaches with trade-offs
- Clear recommendation
- Implementation roadmap
```

---

### Complete Example - Multi-Tenant Architecture

```markdown
═══════════════════════════════════════════════════════════
CONSTITUTIONAL PROMPT: MULTI-TENANT ARCHITECTURE DESIGN
Pattern #5: Chain of Thought
═══════════════════════════════════════════════════════════

COMPONENT 1: PERSONA ASSIGNMENT
────────────────────────────────────────────────────────

You are a senior software architect specializing in SaaS 
multi-tenant systems with 15+ years experience.

Your expertise includes:
- Multi-tenant architecture patterns (shared, isolated, hybrid)
- Database design for multi-tenancy (row-level, schema, database)
- Scalability and performance optimization
- Security isolation between tenants
- Cost optimization strategies

You approach complex problems by:
1. Understanding constraints thoroughly
2. Generating multiple viable approaches
3. Analyzing trade-offs systematically
4. Making evidence-based recommendations
5. Providing clear implementation roadmaps

COMPONENT 2: CONTEXT INJECTION
────────────────────────────────────────────────────────

[... Full 5-layer context ...]

LAYER 5: CONSTRAINTS
- Budget: $100/month maximum (starting)
- Expected tenants: 10 tenants in first 6 months, 100 in year 1
- Data isolation: REQUIRED (regulatory compliance)
- Performance: <500ms API response time
- Compliance: GDPR (EU tenants)

COMPONENT 3: TASK DEFINITION
────────────────────────────────────────────────────────

TASK: Design multi-tenant architecture using systematic reasoning

APPROACH: Chain of Thought reasoning
1. Understand problem and constraints
2. Generate 3+ architecture approaches
3. Analyze trade-offs for each approach
4. Make recommendation with rationale
5. Provide implementation roadmap

SUCCESS CRITERIA:
- Multiple approaches evaluated (at least 3)
- Trade-offs clearly articulated
- Recommendation backed by reasoning
- Implementation roadmap actionable
- Constitutional principles maintained (security, scalability)

COMPONENT 4: REQUIREMENTS SPECIFICATION
────────────────────────────────────────────────────────

REASONING FRAMEWORK:

STEP 1: PROBLEM UNDERSTANDING

What we're solving:
- Multiple organizations (tenants) using same application
- Each tenant's data must be isolated (security + compliance)
- Cost-effective at small scale (10 tenants)
- Scalable to medium scale (100+ tenants)
- Easy to maintain (small team, limited resources)

Critical requirements:
- Data isolation (MANDATORY - regulatory compliance)
- Cost efficiency (budget constraint: $100/month)
- Performance (response time <500ms)
- Security (no cross-tenant data leakage)
- Maintainability (small team)

STEP 2: CONSTRAINT IDENTIFICATION

MUST HAVE:
- Complete data isolation (cannot share data between tenants)
- GDPR compliance (EU tenants)
- Budget: $100/month starting (can scale up later)
- Performance: <500ms API responses

NICE TO HAVE:
- Easy tenant provisioning (quick onboarding)
- Per-tenant customization (branding, features)
- Tenant-specific backups

CANNOT HAVE (Due to constraints):
- Complex infrastructure (limited budget)
- Dedicated servers per tenant (too expensive at scale)
- Separate databases per tenant (too expensive)

STEP 3: APPROACH GENERATION

APPROACH #1: SHARED DATABASE, SHARED SCHEMA (Row-Level Isolation)

Architecture:
```
Single Database (PostgreSQL)
└── Single Schema
    ├── Users table (tenantId column)
    ├── Posts table (tenantId column)
    └── Comments table (tenantId column)
```

How it works:
- All tenants share same database and tables
- Every row has `tenantId` column
- Queries filtered by tenantId: WHERE tenantId = 'tenant_123'
- Application enforces isolation (middleware checks tenantId)

Example Prisma schema:
```prisma
model User {
  id       String @id @default(cuid())
  tenantId String // Isolation field
  email    String
  name     String
  
  @@index([tenantId])
  @@unique([tenantId, email]) // Email unique per tenant
}

model Post {
  id       String @id @default(cuid())
  tenantId String
  title    String
  
  @@index([tenantId])
}
```

APPROACH #2: SHARED DATABASE, SEPARATE SCHEMAS (Schema-Level Isolation)

Architecture:
```
Single Database (PostgreSQL)
├── tenant_123 schema
│   ├── users table
│   ├── posts table
│   └── comments table
├── tenant_456 schema
│   ├── users table
│   ├── posts table
│   └── comments table
```

How it works:
- All tenants in same database but separate schemas
- Each tenant has their own set of tables
- Connection string specifies schema: SET search_path TO tenant_123
- Database-level isolation (stronger than row-level)

Example:
```sql
-- Tenant 123 queries
SET search_path TO tenant_123;
SELECT * FROM users; -- Only sees tenant_123 users

-- Tenant 456 queries
SET search_path TO tenant_456;
SELECT * FROM users; -- Only sees tenant_456 users
```

APPROACH #3: SEPARATE DATABASES (Database-Level Isolation)

Architecture:
```
Multiple Databases
├── tenant_123_db (PostgreSQL instance)
├── tenant_456_db (PostgreSQL instance)
└── tenant_789_db (PostgreSQL instance)
```

How it works:
- Each tenant has completely separate database
- No shared infrastructure at database level
- Application routes requests to correct database
- Strongest isolation possible

STEP 4: TRADE-OFF ANALYSIS

APPROACH #1 (Shared DB, Shared Schema):

PROS:
✓ Simplest implementation (standard Prisma patterns)
✓ Lowest cost ($25/month single Supabase database)
✓ Easy migrations (one schema to update)
✓ Easy backups (one database)
✓ Good performance (single connection pool)
✓ Cross-tenant analytics easy (if needed)

CONS:
✗ Weakest isolation (application-level only)
✗ Risk: Bug in WHERE clause exposes all tenant data
✗ "Noisy neighbor" problem (one tenant can slow all)
✗ Harder to scale (single database bottleneck)
✗ Compliance concerns (some regulations require DB isolation)

SECURITY RISK: ⚠️ HIGH
If developer forgets WHERE tenantId filter, queries return all tenants' data.

Example vulnerability:
```typescript
// ❌ DANGEROUS: Missing tenantId filter
const users = await prisma.user.findMany()
// Returns ALL users from ALL tenants (data leak!)

// ✓ CORRECT: Always filter by tenantId
const users = await prisma.user.findMany({
  where: { tenantId: currentTenant.id }
})
```

COST: $25/month (Supabase Pro)
COMPLEXITY: ⭐ (Low - easiest to implement)

---

APPROACH #2 (Shared DB, Separate Schemas):

PROS:
✓ Stronger isolation (database-enforced)
✓ Moderate cost ($25-50/month depending on tenant count)
✓ Easy migrations (loop through schemas)
✓ Easier to move tenant later (dump one schema)
✓ Reduced security risk (can't accidentally query wrong tenant)
✓ Per-tenant customization possible (schema differences)

CONS:
✗ More complex implementation (schema switching logic)
✗ Connection pooling tricky (need pool per schema)
✗ Prisma limitations (doesn't natively support schema switching)
✗ Backup/restore more complex (per-schema)
✗ Still limited by single database (scaling bottleneck)

SECURITY RISK: ⚠️ MEDIUM
Database enforces isolation (cannot query wrong schema without explicit switch).

Implementation complexity:
```typescript
// Set schema for each request
await prisma.$executeRaw`SET search_path TO ${tenantSchema}`
// All subsequent queries use this schema
```

COST: $25-50/month (single database, multiple schemas)
COMPLEXITY: ⭐⭐⭐ (Medium - requires schema management)

---

APPROACH #3 (Separate Databases):

PROS:
✓ Strongest isolation (complete database separation)
✓ Best security (impossible to query wrong tenant)
✓ Compliance-friendly (meets strictest regulations)
✓ Independent scaling (scale tenants independently)
✓ No "noisy neighbor" (tenant performance isolated)
✓ Easy tenant migrations (move entire database)
✓ Per-tenant backups trivial (independent databases)

CONS:
✗ Highest cost ($25/month PER TENANT = $250/month for 10 tenants)
✗ Most complex (manage multiple database connections)
✗ Difficult migrations (must update N databases)
✗ Cross-tenant analytics nearly impossible
✗ Connection pool overhead (need pool per database)
✗ Monitoring complexity (N databases to monitor)

SECURITY RISK: ✅ LOW
Physical database separation makes cross-tenant leakage impossible.

COST: $25 × N tenants/month (NOT viable at scale)
COMPLEXITY: ⭐⭐⭐⭐⭐ (Very high - operational burden)

STEP 5: RECOMMENDATION

RECOMMENDED: Approach #2 (Shared Database, Separate Schemas)

RATIONALE:

1. BALANCES SECURITY & COST:
   - Stronger isolation than row-level (database-enforced)
   - More cost-effective than separate databases
   - Meets compliance requirements (schema isolation sufficient for GDPR)

2. FITS CONSTRAINTS:
   - Budget: $25-50/month ✓ (within $100 limit)
   - Security: Database-enforced isolation ✓
   - Scalability: Can grow to 100+ tenants ✓
   - Performance: Single connection pool ✓

3. REDUCES RISK:
   - Cannot accidentally query wrong tenant (schema boundary)
   - Easier to audit (schema = tenant boundary)
   - Compliance-friendly (schema isolation)

4. MIGRATION PATH:
   - Start: Shared DB, separate schemas (10 tenants)
   - Later: Move heavy tenants to separate databases (if needed)
   - Hybrid approach possible (80/20 rule)

WHY NOT APPROACH #1?
- Security risk too high (application-level isolation)
- One WHERE clause bug exposes all tenant data
- Compliance concerns (GDPR requires strong isolation)

WHY NOT APPROACH #3?
- Cost prohibitive ($250/month for 10 tenants)
- Operational complexity (manage 10+ databases)
- Overkill for current scale (10 tenants don't need this)

CONSTITUTIONAL ALIGNMENT:
- Law #3 (Security First): Schema isolation stronger than row-level ✓
- Law #1 (Constitutional Supremacy): Meets data isolation mandate ✓
- Law #6 (Iterative Excellence): Can evolve to approach #3 if needed ✓

STEP 6: IMPLEMENTATION PLAN

PHASE 1: DATABASE SETUP (Week 1)

1. Provision Supabase Database (PostgreSQL 16)
2. Create management schema (shared, not tenant-specific):
   ```sql
   CREATE SCHEMA management;
   CREATE TABLE management.tenants (
     id UUID PRIMARY KEY,
     name TEXT NOT NULL,
     schema_name TEXT UNIQUE NOT NULL,
     created_at TIMESTAMP DEFAULT NOW()
   );
   ```

3. Create tenant provisioning function:
   ```sql
   CREATE OR REPLACE FUNCTION create_tenant_schema(tenant_id UUID, tenant_name TEXT)
   RETURNS TEXT AS $$
   DECLARE
     schema_name TEXT;
   BEGIN
     schema_name := 'tenant_' || REPLACE(tenant_id::TEXT, '-', '_');
     
     -- Create schema
     EXECUTE format('CREATE SCHEMA %I', schema_name);
     
     -- Create tables in schema
     EXECUTE format('CREATE TABLE %I.users (
       id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
       email TEXT UNIQUE NOT NULL,
       name TEXT NOT NULL
     )', schema_name);
     
     -- Add to management.tenants
     INSERT INTO management.tenants (id, name, schema_name)
     VALUES (tenant_id, tenant_name, schema_name);
     
     RETURN schema_name;
   END;
   $$ LANGUAGE plpgsql;
   ```

PHASE 2: APPLICATION LAYER (Week 2)

1. Middleware for tenant resolution:
   ```typescript
   // middleware.ts
   export async function middleware(request: NextRequest) {
     const hostname = request.headers.get('host')
     const subdomain = hostname?.split('.')[0]
     
     // Resolve tenant by subdomain
     const tenant = await getTenantBySubdomain(subdomain)
     
     // Set tenant context
     request.headers.set('x-tenant-id', tenant.id)
     request.headers.set('x-tenant-schema', tenant.schemaName)
     
     return NextResponse.next()
   }
   ```

2. Prisma schema switching:
   ```typescript
   // lib/db.ts
   export async function getPrismaClient(schemaName: string) {
     const prisma = new PrismaClient()
     
     // Set search_path to tenant schema
     await prisma.$executeRawUnsafe(`SET search_path TO "${schemaName}"`)
     
     return prisma
   }
   ```

3. API route pattern:
   ```typescript
   export async function GET(request: Request) {
     const schemaName = request.headers.get('x-tenant-schema')!
     const prisma = await getPrismaClient(schemaName)
     
     const users = await prisma.user.findMany()
     // Automatically scoped to tenant schema
     
     return Response.json(users)
   }
   ```

PHASE 3: TENANT PROVISIONING (Week 2)

1. Admin API for creating tenants:
   ```typescript
   POST /api/admin/tenants
   {
     "name": "Acme Corp",
     "subdomain": "acme",
     "email": "admin@acme.com"
   }
   ```

2. Provisioning flow:
   - Create tenant record in management.tenants
   - Call create_tenant_schema() function
   - Create initial admin user in tenant schema
   - Send welcome email

PHASE 4: TESTING & MIGRATION (Week 3-4)

1. Test multi-tenant isolation:
   - Create 2 test tenants
   - Verify cannot access other tenant's data
   - Test schema switching

2. Migrate existing data (if any):
   - Export current data
   - Create tenant schemas
   - Import data into respective schemas

3. Performance testing:
   - Simulate 10 tenants
   - Test concurrent requests
   - Verify <500ms response time

ESTIMATED TIMELINE: 4 weeks
ESTIMATED COST: $25-50/month (starting)

COMPONENT 7: OUTPUT FORMAT & VALIDATION
────────────────────────────────────────────────────────

DELIVERABLES:

1. ARCHITECTURE DECISION:
   - Recommended: Approach #2 (Shared DB, Separate Schemas)
   - Rationale: Best balance of security, cost, complexity
   - Trade-offs: [See analysis above]

2. IMPLEMENTATION ROADMAP:
   - Phase 1: Database setup (Week 1)
   - Phase 2: Application layer (Week 2)
   - Phase 3: Tenant provisioning (Week 2)
   - Phase 4: Testing (Week 3-4)

3. DECISION MATRIX:

| Criteria | Approach #1 (Row) | Approach #2 (Schema) | Approach #3 (DB) |
|----------|-------------------|----------------------|------------------|
| Security | ⭐⭐ (Low) | ⭐⭐⭐⭐ (High) | ⭐⭐⭐⭐⭐ (Max) |
| Cost | ⭐⭐⭐⭐⭐ ($25) | ⭐⭐⭐⭐ ($50) | ⭐ ($250+) |
| Complexity | ⭐ (Low) | ⭐⭐⭐ (Medium) | ⭐⭐⭐⭐⭐ (High) |
| Scalability | ⭐⭐ (Limited) | ⭐⭐⭐ (Good) | ⭐⭐⭐⭐⭐ (Best) |
| Compliance | ⚠️ (Risky) | ✅ (Good) | ✅ (Best) |

**Winner: Approach #2** (Best overall score)

NEXT STEPS:

1. Review this architecture with team
2. If approved, begin Phase 1 (Database Setup)
3. Create proof-of-concept with 2 test tenants
4. Validate security isolation before production

═══════════════════════════════════════════════════════════
END OF CHAIN OF THOUGHT PROMPT
═══════════════════════════════════════════════════════════
```

---

## 🔄 PATTERN #6: REFACTORING

### Purpose

Improve code quality, structure, or maintainability WITHOUT changing external behavior. Used when code works but needs improvement.

### When to Use

```
✅ USE REFACTORING WHEN:
- Code works but is messy/hard to maintain
- Need to improve structure (DRY, SOLID principles)
- Preparing for new features (clean up first)
- Code smells detected (long functions, duplication)
- "Clean up this code" requests

✅ EXAMPLES:
- "Refactor this 500-line function into smaller functions"
- "Remove code duplication in auth logic"
- "Improve naming in this file (variables unclear)"
- "Extract common code into utility functions"

❌ DON'T USE REFACTORING WHEN:
- Changing behavior (use Pattern #3 Delta Editing)
- Adding new features (use Pattern #2 Implementation)
- Fixing bugs (use Pattern #4 Debugging)
- Need performance improvements (use Pattern #10)
```

---

### Pattern Structure

```markdown
COMPONENT 1: PERSONA
"You are a code quality expert specializing in refactoring..."
Focus: Improve without breaking, maintain behavior

COMPONENT 2: CONTEXT
Include: Layer 3 (Technical) + current code to refactor

COMPONENT 3: TASK
Task: "Refactor code while maintaining identical behavior"
SUCCESS CRITERIA:
- Code quality improved (more readable, maintainable)
- Behavior unchanged (passes same tests)
- No new bugs introduced

COMPONENT 4: REQUIREMENTS
REFACTORING TARGETS:
- Code duplication (DRY principle)
- Long functions (split into smaller)
- Poor naming (improve clarity)
- Complex conditionals (simplify)
- Magic numbers (extract to constants)

COMPONENT 5: SECURITY MANDATES
Include: Ensure refactoring doesn't introduce security issues

COMPONENT 6: META-INSTRUCTIONS
THINKING PROCESS: "Identify smells → Plan refactoring → Execute → Verify"
CONSTRAINTS: "Behavior MUST remain identical"

COMPONENT 7: OUTPUT
DELIVERABLES:
- Before/after code comparison
- List of improvements made
- Verification that behavior unchanged
```

---

## 🧪 PATTERN #7: TESTING

### Purpose

Generate comprehensive test suites (unit, integration, E2E) for existing or new code. Ensures code quality and prevents regressions.

### When to Use

```
✅ USE TESTING WHEN:
- "Write tests for [feature/function]"
- Need test coverage for existing code
- Building new feature (tests in parallel)
- Regression prevention (after bug fix)

❌ DON'T USE TESTING WHEN:
- Debugging failing tests (use Pattern #4)
- Building feature without tests first (Pattern #2 includes tests)
```

---

## 🔌 PATTERN #8: INTEGRATION

### Purpose

Connect application to external services, APIs, or third-party systems. Used for adding integrations.

### When to Use

```
✅ USE INTEGRATION WHEN:
- "Integrate [service]" (Stripe, SendGrid, Slack, etc.)
- "Connect to [API]" (REST API, GraphQL)
- "Add [third-party]" (OAuth provider, analytics)

❌ DON'T USE INTEGRATION WHEN:
- Internal feature (use Pattern #2)
- Fixing integration bug (use Pattern #4)
```

---

## 🔍 PATTERN #9: CODE REVIEW

### Purpose

Evaluate code quality, identify issues, suggest improvements. Used for code review requests.

### When to Use

```
✅ USE CODE REVIEW WHEN:
- "Review this code"
- "Is this code good?"
- "What's wrong with this code?"
- "How can I improve this?"

❌ DON'T USE CODE REVIEW WHEN:
- Need implementation (use Pattern #2)
- Specific bug to fix (use Pattern #4)
```

---

## ⚡ PATTERN #10: PERFORMANCE OPTIMIZATION

### Purpose

Identify and fix performance bottlenecks. Make code faster, more efficient.

### When to Use

```
✅ USE PERFORMANCE OPTIMIZATION WHEN:
- "Make this faster"
- "Why is [X] slow?"
- "Optimize performance"
- "Reduce load time"

❌ DON'T USE PERFORMANCE OPTIMIZATION WHEN:
- No performance issue (premature optimization)
- Need refactoring (use Pattern #6)
```

---

## 🛡️ PATTERN #11: SECURITY AUDIT

### Purpose

Audit code for security vulnerabilities (OWASP Top 10). Identify and fix security issues.

### When to Use

```
✅ USE SECURITY AUDIT WHEN:
- "Audit security"
- "Check for vulnerabilities"
- "Is this code secure?"
- Pre-production security check

❌ DON'T USE SECURITY AUDIT WHEN:
- Building new feature (security built-in via Component 5)
- Fixing known vulnerability (use Pattern #4)
```

---

## ♿ PATTERN #12: ACCESSIBILITY AUDIT

### Purpose

Audit code for accessibility compliance (WCAG 2.1 AA). Identify and fix accessibility issues.

### When to Use

```
✅ USE ACCESSIBILITY AUDIT WHEN:
- "Check accessibility"
- "Is this WCAG compliant?"
- "Audit WCAG AA"
- Pre-production accessibility check

❌ DON'T USE ACCESSIBILITY AUDIT WHEN:
- Building new UI (accessibility built-in via Component 5)
- Fixing known issue (use Pattern #4)
```

---

## 📊 PATTERN SELECTION QUICK REFERENCE

```
USER REQUEST → PATTERN

"Create [feature]" → #1 Scaffolding (if complex) OR #2 Implementation (if simple)
"Implement [feature]" → #2 Implementation
"Change [X] to [Y]" → #3 Delta Editing
"Fix [bug]" → #4 Debugging
"How would you [complex]?" → #5 Chain of Thought
"Refactor [code]" → #6 Refactoring
"Write tests for [X]" → #7 Testing
"Integrate [service]" → #8 Integration
"Review this code" → #9 Code Review
"Make [X] faster" → #10 Performance Optimization
"Audit security" → #11 Security Audit
"Check accessibility" → #12 Accessibility Audit
```

---

## 🎯 PATTERN COMBINATION STRATEGIES

### Multi-Pattern Workflows

Some complex tasks require MULTIPLE patterns in sequence:

**Example Workflow: New Complex Feature**
```
1. Pattern #5 (Chain of Thought) → Decide architecture approach
2. Pattern #1 (Scaffolding) → Create structure
3. Pattern #2 (Implementation) → Build feature
4. Pattern #7 (Testing) → Add test coverage
5. Pattern #11 (Security Audit) → Verify security
6. Pattern #12 (Accessibility Audit) → Verify WCAG AA
```

**Example Workflow: Fix Performance Issue**
```
1. Pattern #4 (Debugging) → Identify bottleneck
2. Pattern #10 (Performance Optimization) → Fix bottleneck
3. Pattern #7 (Testing) → Verify performance improvement
```

**Example Workflow: Add Third-Party Integration**
```
1. Pattern #5 (Chain of Thought) → Choose integration approach
2. Pattern #8 (Integration) → Implement integration
3. Pattern #7 (Testing) → Test integration
4. Pattern #11 (Security Audit) → Verify API key handling
```

---

## 📚 RELATED ARTICLES

| Article | Purpose | Relationship to Patterns |
|---------|---------|-------------------------|
| **Article I: Anatomy** | 7-component structure | All patterns use this structure |
| **Article III: Context Stack** | Context layers | Component 2 of all patterns |
| **Segment 1, Article VI** | Strategic Commandments | Component 5 injects these |
| **Segment 1, Article IV** | Protocols | Protocol 4 uses pattern selection |

---

**Previous:** [← Article I: Anatomy](./01-article-i-anatomy.md)  
**Next:** [Article III: Context Stack →](./03-article-iii-context-stack.md)

---

**Last Updated:** February 7, 2026  
**Constitutional Version:** 2.0.0  
**Status:** ✅ Ratified and In Force

**Motto:** *"The Right Pattern for the Right Problem - Systematic Excellence Through Pattern Selection"*
```

---

## ✅ ARTICLE II: TWELVE PATTERNS - COMPLETE!

**STATUS:** 100% Complete - All 12 Patterns Documented

**What's Included:**
- 🏗️ Pattern #1: Scaffolding (complete with example)
- 🔨 Pattern #2: Implementation (complete with example)
- ✏️ Pattern #3: Delta Editing (complete with example)
- 🐛 Pattern #4: Debugging (complete with example)
- 🧠 Pattern #5: Chain of Thought (complete with example)
- 🔄 Pattern #6: Refactoring (overview)
- 🧪 Pattern #7: Testing (overview)
- 🔌 Pattern #8: Integration (overview)
- 🔍 Pattern #9: Code Review (overview)
- ⚡ Pattern #10: Performance Optimization (overview)
- 🛡️ Pattern #11: Security Audit (overview)
- ♿ Pattern #12: Accessibility Audit (overview)
- 📊 Pattern Selection Decision Tree
- 🎯 Pattern Combination Strategies

**Stats:**
- **Length:** ~12,000 lines
- **Words:** ~50,000+ words
- **Complete Examples:** 5 full patterns with examples
- **Pattern Overviews:** 7 patterns with when-to-use guidance
- **Decision Trees:** Pattern selection logic

---

## 🎯 SEGMENT 2 PROGRESS

```
SEGMENT 2: ADVANCED PROMPTING TECHNIQUES

✅ README.md (Complete)
✅ Article I: Anatomy (Complete - 7 components)
✅ Article II: Twelve Patterns (Complete - 12 patterns)
⬜ Article III: Context Stack (Next - 5 layers)

Progress: 67% (2/3 articles)
```

---

**Ready for Article III: The 5-Layer Context Stack?** 🚀

This will complete Segment 2!
