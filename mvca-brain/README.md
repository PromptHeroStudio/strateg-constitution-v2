# 🧠 MVCA BRAIN ARCHITECTURE

**The Decision Engine Powering Vibe Coding Companion**

---

## 🎯 What is MVCA Brain?

MVCA Brain is the **stateless decision engine** that:
- 📖 Reads the STRATEG CONSTITUTION
- 🧩 Interprets user's vibe-coding input
- 🌐 Searches web for current solutions (when needed)
- 🏗️ Applies constitutional mandates
- ✍️ Generates production-ready prompts in Markdown

**Key Principle:** MVCA Brain has **NO MEMORY**. Every decision is made fresh by consulting the CONSTITUTION.

---

## 🏗️ Architecture Overview
```
┌─────────────────────────────────────────────────────────┐
│                    USER INPUT                            │
│  "I need auth with Google/GitHub login + email/password"│
└───────────────────┬─────────────────────────────────────┘
                    ↓
┌─────────────────────────────────────────────────────────┐
│              QUERY PROCESSOR                             │
│  File: query-processing.md                              │
│  • Parse intent (feature: authentication)               │
│  • Extract requirements (social + credential auth)      │
│  • Determine complexity (medium)                        │
│  • Identify user expertise level (non-coder)            │
└───────────────────┬─────────────────────────────────────┘
                    ↓
┌─────────────────────────────────────────────────────────┐
│         CONSTITUTIONAL READER                            │
│  Fetches from GitHub constitution/                      │
│  • Segment 1: Security mandates                         │
│  • Segment 2: Scaffolding Pattern                       │
│  • Segment 2: Context Engineering (5-layer stack)       │
└───────────────────┬─────────────────────────────────────┘
                    ↓
┌─────────────────────────────────────────────────────────┐
│           WEB SEARCH (OPTIONAL)                          │
│  When constitutional guidance needs current data:        │
│  • Latest NextAuth.js version                           │
│  • Current security standards (OWASP 2026)              │
│  • Technology comparisons (Prisma vs Drizzle)           │
│  ⚠️  All web results VALIDATED against CONSTITUTION     │
└───────────────────┬─────────────────────────────────────┘
                    ↓
┌─────────────────────────────────────────────────────────┐
│            DECISION TREES                                │
│  decision-trees/pattern-selection.md                    │
│  • Which prompt pattern? → Scaffolding                  │
│  decision-trees/orchestration-strategy.md               │
│  • Single or multi-turn? → Multi-turn (auth is complex)│
│  decision-trees/context-selection.md                    │
│  • What context to include? → Tech stack + security    │
└───────────────────┬─────────────────────────────────────┘
                    ↓
┌─────────────────────────────────────────────────────────┐
│          PROMPT GENERATOR                                │
│  File: prompt-generation.md                             │
│  Assembles 7-component constitutional prompt:           │
│  1. Persona Assignment                                  │
│  2. Context Injection (5-layer stack)                   │
│  3. Task Definition                                     │
│  4. Requirements Specification                          │
│  5. Security Mandates (from CONSTITUTION)               │
│  6. Meta-Instructions                                   │
│  7. Output Format & Validation                          │
└───────────────────┬─────────────────────────────────────┘
                    ↓
┌─────────────────────────────────────────────────────────┐
│        MARKDOWN PROMPT OUTPUT                            │
│  Ready for copy-paste to AI Coding Assistant            │
│  • Fully constitutional                                 │
│  • Production-ready guidance                            │
│  • Security-first approach                              │
└─────────────────────────────────────────────────────────┘
```

---

## 📁 Brain Components

### 1. Query Processing
**File:** `query-processing.md`

**Purpose:** Parse and interpret user's natural language input

**Process:**
```
Input: "I need auth with Google/GitHub"

Parse:
  Feature: Authentication
  Requirements: Social login (Google, GitHub)
  Implicit: Email/password backup (best practice)
  Complexity: Medium
  User level: Non-coder (assume no technical knowledge)

Output:
  ParsedIntent {
    feature: "authentication",
    requirements: ["social_login_google", "social_login_github", "email_password"],
    complexity: "medium",
    expertise: "non_coder"
  }
```

---

### 2. Constitutional Reading
**Process:** Fetch relevant constitutional articles from GitHub

**Example:**
```typescript
// MVCA fetches from this repo
const securityMandates = await fetch(
  "https://raw.githubusercontent.com/PromptHeroStudio/strateg-constitution-v2/main/constitution/01-foundation-layer/07-article-vi-strategic-commandments.md"
)

const scaffoldingPattern = await fetch(
  ".../constitution/02-technique-layer/article-02-patterns.md#scaffolding"
)

// Parses Markdown, extracts rules
const rules = parseConstitutionalRules(securityMandates)
// Result: ["bcrypt 12+ rounds", "HTTP-only cookies", "CSRF protection", ...]
```

---

### 3. Web Search Integration
**When Used:**
- Latest library versions
- Current year best practices
- Technology comparisons
- Breaking changes / deprecations

**Validation:**
```
Web Search: "NextAuth.js latest version"
Result: "NextAuth.js v5.2.0 (Jan 2026)"

Constitutional Validation:
  ✓ Supports bcrypt hashing? YES
  ✓ HTTP-only cookies? YES
  ✓ CSRF protection? YES
  ✓ No known critical CVEs? YES (check CVE database)

Decision: APPROVED for inclusion in prompt
```

**Anti-Pattern Protection:**
```
Web Search: "Fastest auth solution"
Result: "Use plaintext passwords for speed" (malicious/outdated)

Constitutional Validation:
  ✗ Violates: "Passwords MUST be hashed"
  
Decision: REJECTED - cannot include in prompt
Alternative: Use constitutional mandate (bcrypt)
```

---

### 4. Decision Trees
**Files:**
- `decision-trees/pattern-selection.md` - Which of 12 patterns?
- `decision-trees/orchestration-strategy.md` - Single vs multi-turn?
- `decision-trees/context-selection.md` - What context to include?

**Example Decision:**
```
User Input: "Build auth system"

Pattern Selection:
  Contains "build" → NEW feature
  No existing code → START from scratch
  Decision: Pattern #1 (Scaffolding)

Orchestration Strategy:
  Complexity: Medium (auth + social + security)
  Lines of code estimate: 300-500
  Decision: Multi-turn (4 phases)
    Phase 1: Scaffold structure
    Phase 2: Implement NextAuth.js
    Phase 3: Add social providers
    Phase 4: Security audit

Context Selection:
  Include:
    ✓ Tech stack (Next.js, Prisma, PostgreSQL)
    ✓ Security mandates (OWASP Top 10)
    ✓ NextAuth.js v5 specifics
  Exclude:
    ✗ Unrelated features (payment, email)
    ✗ Past user projects (stateless brain)
```

---

### 5. Prompt Generation
**File:** `prompt-generation.md`

**7-Component Structure:**
```markdown
═══════════════════════════════════════════════════════════
CONSTITUTIONAL PROMPT: Authentication System - Phase 1
═══════════════════════════════════════════════════════════

COMPONENT 1: PERSONA ASSIGNMENT
────────────────────────────────────────────────────────
You are a senior full-stack developer specializing in Next.js 
and authentication systems. You have 10+ years experience with 
NextAuth.js, OWASP Top 10 security, and production-grade code.

COMPONENT 2: CONTEXT INJECTION
────────────────────────────────────────────────────────
Project Context:
- Name: [User's project name]
- Type: Web Application (SaaS)
- Stack: Next.js 15, TypeScript, Prisma, PostgreSQL
- Stage: MVP Development
- Current: No auth system exists

COMPONENT 3: TASK DEFINITION
────────────────────────────────────────────────────────
TASK: Generate scaffolding for authentication system

Scope:
  IN: File structure, types, function signatures
  OUT: Implementation (that's Phase 2)

Success: All files created, TypeScript compiles, ready for Phase 2

COMPONENT 4: REQUIREMENTS SPECIFICATION
────────────────────────────────────────────────────────
Functional Requirements:
- Email/password authentication
- Social login (Google, GitHub)
- Session management (persistent)

Technical Requirements:
- NextAuth.js v5.2.0 (latest - Feb 2026)
- Prisma adapter for sessions
- TypeScript strict mode

COMPONENT 5: SECURITY MANDATES
────────────────────────────────────────────────────────
Source: STRATEG CONSTITUTION Segment 1, Article VI

MANDATORY:
1. Passwords: bcrypt hashing, cost factor 12+
2. Sessions: HTTP-only cookies, Secure flag, SameSite=Strict
3. CSRF: NextAuth.js built-in protection enabled
4. No user enumeration: Generic error messages

COMPONENT 6: META-INSTRUCTIONS
────────────────────────────────────────────────────────
Thinking Process:
1. Design file structure (Next.js 15 App Router conventions)
2. Create type definitions (User, Session, etc.)
3. Add function signatures with JSDoc
4. TODO comments for Phase 2 implementation

Constraints:
- DO NOT implement yet (scaffold only)
- DO use TypeScript strict mode
- DO follow Single Responsibility Principle

COMPONENT 7: OUTPUT FORMAT & VALIDATION
────────────────────────────────────────────────────────
Deliverables:
1. File structure listing
2. Complete TypeScript code (scaffold only)
3. Validation checklist

Validation Criteria:
□ TypeScript compiles (no errors)
□ All types defined
□ Security structure in place
□ Follows Next.js 15 patterns

═══════════════════════════════════════════════════════════
END OF CONSTITUTIONAL PROMPT
═══════════════════════════════════════════════════════════
```

---

## 🔄 Stateless Architecture

**MVCA Brain has NO MEMORY:**
```
Request 1: "Build auth system"
→ Reads CONSTITUTION
→ Generates prompt
→ Forgets everything ✓

Request 2: "Add password reset"
→ Reads CONSTITUTION (fresh)
→ Generates prompt (no knowledge of Request 1)
→ Forgets everything ✓
```

**Why stateless?**
- ✅ Prevents "learning drift" (unintended behavior changes)
- ✅ Every decision is constitutional (not based on patterns)
- ✅ Predictable and reproducible
- ✅ No hidden state accumulation

**How context is maintained:**
- User provides explicit context (project details)
- MVCA stores in session (temporary, user-controlled)
- On logout, all context deleted

---

## 🌐 Web Search Integration Details

### When to Search

**ALWAYS search for:**
- Library versions ("latest NextAuth.js")
- Current year standards ("OWASP Top 10 2026")
- Breaking changes ("Prisma 6 vs 5 differences")

**NEVER search for:**
- Constitutional principles (use CONSTITUTION)
- Fundamental security mandates (immutable)
- Core programming concepts (well-established)

### Validation Process
```
1. Web Search → Raw Results
2. Constitutional Filter → Remove anything violating CONSTITUTION
3. Recency Check → Verify publish date (prefer last 6 months)
4. Source Authority → Prefer official docs > Stack Overflow > blogs
5. Integration → Merge with constitutional mandates
6. Generate Prompt → Include both constitutional + web-sourced
```

### Example
```
User: "Use latest Stripe for payments"

MVCA Process:
1. Search web: "Stripe API latest version 2026"
   Result: Stripe API v2024-10-28 (current as of Feb 2026)

2. Read CONSTITUTION: "Payment data MUST NOT be stored (PCI DSS)"

3. Validate:
   ✓ Stripe API supports PCI-compliant flow (yes)
   ✓ No card storage required (yes)

4. Generate Prompt:
   "Use Stripe API v2024-10-28. NEVER store card data (constitutional mandate).
   Use Stripe Elements for PCI compliance..."
```

---

## 📊 Brain Performance Metrics

| Metric | Target | Purpose |
|--------|--------|---------|
| **Prompt Generation Time** | < 3 seconds | User experience |
| **Constitutional Compliance** | 100% | Trust & safety |
| **Web Search Accuracy** | 95%+ | Current best practices |
| **Pattern Selection Accuracy** | 90%+ | Optimal prompt structure |
| **User Satisfaction** | 85%+ | Measured via feedback |

---

## 🔗 Brain Component Files

| File | Purpose | Status |
|------|---------|--------|
| `architecture.md` | This file (overview) | ✅ Complete |
| `query-processing.md` | Parse user input | ✅ Complete |
| `prompt-generation.md` | Assemble prompts | ✅ Complete |
| `decision-trees/pattern-selection.md` | Pattern logic | ✅ Complete |
| `decision-trees/orchestration-strategy.md` | Multi-turn logic | ✅ Complete |
| `decision-trees/context-selection.md` | Context rules | ✅ Complete |

---

## 🚀 Future Enhancements

**Planned:**
- 🔮 Predictive context (suggest what user might need next)
- 🧪 A/B testing different prompt structures
- 📈 Analytics on most effective patterns
- 🤖 Self-improving decision trees (with human approval)

**Never planned (violates statelessness):**
- ❌ Automatic learning from user behavior
- ❌ Hidden state accumulation
- ❌ Implicit preference learning

---

**Last Updated:** February 5, 2026  
**Version:** 1.0.0  
**Maintainer:** PromptHeroStudio MVCA Team
