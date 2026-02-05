# 🎯 TECHNIQUE LAYER (Segment 2)

**Status:** 🔄 45% COMPLETE (Articles I-III done, IV-VII in progress)  
**Purpose:** Master the art of commanding AI effectively  
**Target Audience:** STRATEG practitioners who completed Foundation Layer  
**Learning Outcome:** Transform from "asking AI" to "commanding AI strategically"

---

## 📖 OVERVIEW

The **Technique Layer** teaches you how to communicate with AI tools to achieve production-quality results. This is where you learn the **language of AI orchestration**.

In Segment 1 (Foundation Layer), you learned:
- **WHY** vibe coding works (the philosophy)
- **WHAT** principles govern it (immutable laws, commandments)
- **WHEN** to validate (checkpoints)

In Segment 2 (Technique Layer), you will learn:
- **HOW** to write constitutional prompts (7 components)
- **WHICH** patterns to use for different scenarios (12 prompt patterns)
- **HOW** to engineer context (5-layer framework)
- **HOW** to refine AI output iteratively (systematic refinement)
- **HOW** to debug AI-generated code (diagnostic prompting)
- **HOW** to orchestrate multiple AI tools (advanced workflows)

**Critical Prerequisite:** You MUST complete Segment 1 before starting this segment. If you haven't, [go back to Foundation Layer →](../01-foundation-layer/README.md)

---

## 🎯 LEARNING OBJECTIVES

By completing this segment, you will be able to:

1. **Write** constitutional prompts with all 7 components
2. **Apply** the 12 prompt patterns to different scenarios
3. **Engineer** 5-layer context for any project
4. **Refine** AI output systematically from "mediocre" to "production-ready"
5. **Debug** AI-generated code using diagnostic prompts
6. **Orchestrate** multiple AI tools in coordinated workflows
7. **Validate** your own prompts before sending them to AI
8. **Achieve** 90%+ first-prompt success rate (vs. 20% for beginners)

---

## 📚 SEGMENT CONTENTS

### [01. Article I: Anatomy of Constitutional Prompt](./01-article-i-anatomy-of-prompt.md)
**Status:** ✅ COMPLETE  
**Reading Time:** 30 minutes

The **7 Constitutional Components** that every production-ready prompt must include:

**COMPONENT 1: PERSONA ASSIGNMENT**
- Assign AI a specific role/expertise
- Example: "You are a senior Next.js architect with 10 years of experience"

**COMPONENT 2: CONTEXT INJECTION**
- Provide comprehensive project context (5 layers)
- Tech stack, project goals, existing architecture

**COMPONENT 3: TASK DEFINITION**
- Clearly define the specific task
- Break down complex tasks into subtasks

**COMPONENT 4: REQUIREMENTS SPECIFICATION**
- Explicit functional requirements
- Non-functional requirements (performance, security)

**COMPONENT 5: SECURITY MANDATES**
- Enforce the 10 Strategic Commandments
- Make security violations impossible

**COMPONENT 6: META-INSTRUCTIONS**
- Tell AI HOW to think (step-by-step, validate assumptions)
- Specify reasoning process

**COMPONENT 7: OUTPUT FORMAT & VALIDATION**
- Define expected output format (code files, explanations)
- Validation criteria for AI's output

**Includes:**
- ✅ Constitutional Prompt Template (reusable for all projects)
- ✅ Before/After examples (bad prompt → constitutional prompt)
- ✅ Component-by-component breakdown with examples

**Key Takeaway:** A constitutional prompt is not just "a better prompt" - it's a comprehensive specification that leaves no room for AI misinterpretation.

---

### [02. Article II: The 12 Constitutional Prompt Patterns](./02-article-ii-prompt-patterns.md)
**Status:** ✅ COMPLETE  
**Reading Time:** 45 minutes

The **12 reusable patterns** for common development scenarios:

**PATTERN #1: SCAFFOLDING PATTERN**
- When: Starting a new feature from scratch
- Output: Complete file structure, boilerplate code
- Example: "Create authentication system with NextAuth.js"

**PATTERN #2: DELTA PATTERN**
- When: Modifying existing code
- Output: Minimal changes to achieve goal (no rewrite)
- Example: "Add rate limiting to existing API route"

**PATTERN #3: VALIDATOR PATTERN**
- When: Reviewing code for issues
- Output: Bug report, security vulnerabilities, improvements
- Example: "Audit this login endpoint for security issues"

**PATTERN #4: DEBUGGING PATTERN**
- When: Fixing a bug
- Output: Root cause analysis + fix
- Example: "Fix: User session expires immediately after login"

**PATTERN #5: INTEGRATION PATTERN**
- When: Connecting two features/systems
- Output: Integration code + tests
- Example: "Integrate Stripe payments with user dashboard"

**PATTERN #6: REFACTORING PATTERN**
- When: Improving code quality without changing behavior
- Output: Refactored code + explanation
- Example: "Refactor this component to use custom hooks"

**PATTERN #7: TESTING PATTERN**
- When: Writing tests for existing code
- Output: Test suites (unit, integration, e2e)
- Example: "Write Playwright tests for checkout flow"

**PATTERN #8: ARCHITECTURE PATTERN**
- When: Designing system architecture
- Output: Architecture diagram + file structure + tech decisions
- Example: "Design architecture for multi-tenant SaaS app"

**PATTERN #9: MIGRATION PATTERN**
- When: Upgrading libraries or frameworks
- Output: Migration plan + updated code
- Example: "Migrate from Pages Router to App Router (Next.js 14)"

**PATTERN #10: PERFORMANCE OPTIMIZATION PATTERN**
- When: Improving performance
- Output: Optimized code + benchmarks
- Example: "Optimize this database query (currently 2s response time)"

**PATTERN #11: SECURITY AUDIT PATTERN**
- When: Validating security compliance
- Output: Security checklist + violations + fixes
- Example: "Audit against OWASP Top 10 and 10 Commandments"

**PATTERN #12: DOCUMENTATION PATTERN**
- When: Generating documentation
- Output: README, JSDoc comments, API docs
- Example: "Generate API documentation for all endpoints"

**Includes:**
- ✅ Full prompt template for each pattern
- ✅ Real-world examples for each pattern
- ✅ When to use which pattern (decision tree)
- ✅ Pattern combination strategies (using multiple patterns together)

**Key Takeaway:** Don't reinvent prompts for every task. Use proven patterns.

---

### [03. Article III: Context Engineering Mastery](./03-article-iii-context-engineering.md)
**Status:** ✅ COMPLETE  
**Reading Time:** 40 minutes

The **5-Layer Context Framework** that determines AI output quality:

**LAYER 1: PROJECT CONTEXT (Business Foundation)**
- What: Project purpose, target users, success criteria
- Why: AI needs to understand business goals to make design decisions
- Example: "B2B SaaS for project management (competitor to Asana)"

**LAYER 2: TECHNICAL CONTEXT (Technology Stack)**
- What: Languages, frameworks, libraries, architecture
- Why: AI generates code compatible with your stack
- Example: "Next.js 14 App Router, Prisma, PostgreSQL, TailwindCSS"

**LAYER 3: FEATURE CONTEXT (Specific Task)**
- What: The exact feature being built
- Why: AI focuses on this specific task, not the entire app
- Example: "User authentication with email/password + Google OAuth"

**LAYER 4: CONSTRAINTS CONTEXT (Boundaries)**
- What: Limitations, restrictions, requirements
- Why: AI respects constraints (budget, time, performance)
- Example: "Must work on mobile, <500ms response time, GDPR compliant"

**LAYER 5: META-CONTEXT (How AI Should Think)**
- What: Instructions on HOW to approach the problem
- Why: AI uses the right reasoning process
- Example: "Think step-by-step, validate assumptions, consider edge cases"

**Sub-categories of Meta-Context:**
1. **Reasoning Process** - "Think like a senior architect"
2. **Approach Selection** - "Prefer simplicity over cleverness"
3. **Error Anticipation** - "Identify potential failure modes"
4. **Knowledge Synthesis** - "Combine best practices from multiple sources"
5. **Continuous Learning** - "Explain WHY you chose this approach"
6. **Domain-Specific Reasoning** - "Apply e-commerce best practices"

**Includes:**
- ✅ Context Integration Framework (how to combine all 5 layers)
- ✅ Context Templates (reusable context blocks)
- ✅ Context Validation Checklist (ensure nothing is missing)
- ✅ Examples: Poor context vs. Rich context (side-by-side comparison)

**Key Takeaway:** Context is EVERYTHING. 80% of prompt quality comes from context, 20% from task definition.

---

### [04. Article IV: Iterative Refinement Mastery](./04-article-iv-iterative-refinement.md)
**Status:** 🔄 20% COMPLETE (In Progress)  
**Reading Time:** 35 minutes (estimated)

The systematic process of refining AI output from "mediocre" to "production-ready":

**SECTION 1: THE VALIDATION FRAMEWORK**
- **Tier 1:** Immediate Visual Inspection (30 seconds)
- **Tier 2:** Requirement Checklist (2-5 minutes)
- **Tier 3:** Functional Testing (5-15 minutes)

**SECTION 2: THE REFINEMENT PROMPT TAXONOMY**
- **Category 1:** Bug Fix Prompts ("Fix: XYZ is broken")
- **Category 2:** Enhancement Prompts ("Add: Feature XYZ")
- **Category 3:** Refactoring Prompts ("Improve: Code quality")
- **Category 4:** Clarification Prompts ("Explain: Why did you choose X?")

**SECTION 3: ADVANCED REFINEMENT TECHNIQUES**
- Progressive Enhancement Strategy
- Regression Prevention
- Quality Ratcheting (never go backwards)

**SECTION 4: MULTI-ITERATION STRATEGIES**
- When to iterate vs. start fresh
- Managing iteration fatigue
- Knowing when "good enough" is good enough

**SECTION 5: CODE REVIEW PATTERNS**
- Self-review checklist
- Peer review simulation (asking AI to review its own work)
- Automated review tools

**SECTION 6: REGRESSION PREVENTION**
- Version control best practices
- Checkpoint creation (save before major changes)
- Rollback procedures

**SECTION 7: REFINEMENT METRICS & SUCCESS CRITERIA**
- How to measure improvement
- When refinement is complete
- Quality scoring system

**Includes:**
- ✅ 3-Tier Validation Framework (step-by-step)
- ✅ Refinement prompt templates for each category
- ✅ Decision tree: Iterate vs. Start Fresh
- ✅ Real examples: 5+ iterations to production quality

**Key Takeaway:** First AI output is rarely production-ready. Systematic refinement is a skill.

---

### [05. Article V: Debugging Mastery](./05-article-v-debugging-mastery.md)
**Status:** 📝 PLANNED (Not yet written)  
**Reading Time:** 30 minutes (estimated)

How to diagnose and fix bugs in AI-generated code:

**SECTION 1: THE DEBUGGING MINDSET**
- Bug is not failure, it's information
- Systematic vs. random debugging
- Debugging as learning opportunity

**SECTION 2: ERROR MESSAGE ANATOMY**
- Understanding stack traces (beginner-friendly explanation)
- Common error patterns (and what they mean)
- Reading logs effectively

**SECTION 3: DIAGNOSTIC PROMPT ENGINEERING**
- The Perfect Bug Report Prompt
- Importance of reproduction steps
- Isolating the problem

**SECTION 4: ROOT CAUSE ANALYSIS PATTERNS**
- The 5 Whys Technique
- Binary search debugging (narrow down the problem)
- Rubber duck debugging (with AI)

**SECTION 5: FIX VALIDATION**
- How to verify fix actually works
- Testing for regressions (did fix break something else?)
- Documenting the fix

**SECTION 6: COMMON BUG CATEGORIES & SOLUTIONS**
- **Syntax Errors** (beginners) - Missing semicolons, typos
- **Logic Errors** (intermediate) - Wrong algorithm, edge cases
- **Integration Errors** (advanced) - API mismatches, data format issues
- **Performance Issues** - Slow queries, memory leaks
- **Security Vulnerabilities** - Injection attacks, auth bypasses
- **Deployment Errors** - Environment variables, build failures

**Includes:**
- ✅ Debugging prompt templates
- ✅ Common error messages + solutions
- ✅ Bug report template (for AI)
- ✅ Case studies: Real bugs + debugging process

**Key Takeaway:** Debugging is a systematic process, not trial-and-error.

---

### [06. Article VI: Advanced AI Orchestration](./06-article-vi-ai-orchestration.md)
**Status:** 📝 PLANNED (Not yet written)  
**Reading Time:** 40 minutes (estimated)

How to use multiple AI tools in coordinated workflows:

**SECTION 1: MULTI-AI WORKFLOWS**
- When to use multiple AIs (vs. single AI)
- Specialization strategy (Claude for X, GPT for Y, Cursor for Z)
- Handoff patterns between AIs

**SECTION 2: AI TOOL SELECTION MATRIX**
- **Claude** (Strengths: Long context, complex reasoning | Best for: Architecture, security)
- **ChatGPT** (Strengths: Speed, creativity | Best for: Brainstorming, documentation)
- **Cursor** (Strengths: Code editing, IDE integration | Best for: Implementation, refactoring)
- **GitHub Copilot** (Strengths: Inline suggestions | Best for: Autocomplete, snippets)
- **Bolt.new** (Strengths: Rapid prototyping | Best for: Quick demos, MVPs)
- When to use which tool (decision matrix)

**SECTION 3: CONTEXT PRESERVATION ACROSS TOOLS**
- Exporting context from Tool A to Tool B
- Maintaining consistency (don't let tools contradict each other)
- Avoiding context loss

**SECTION 4: DELEGATION PATTERNS**
- **Planning** → Use MVCA (strategic decomposition)
- **Implementation** → Use Cursor/Claude (code generation)
- **Review** → Use Claude (security audit, code review)
- **Documentation** → Use ChatGPT (README, API docs)
- **Optimization** → Use Cursor (refactoring, performance)

**SECTION 5: QUALITY CONTROL ACROSS TOOLS**
- Cross-validation (AI A reviews AI B's work)
- Consistency checking (ensure all AIs follow same architecture)
- Final human validation

**Includes:**
- ✅ AI Tool Comparison Matrix
- ✅ Multi-AI workflow examples
- ✅ Context handoff templates
- ✅ Quality control checklist

**Key Takeaway:** Different AI tools have different strengths. Orchestrate them like a conductor.

---

### [07. Article VII: Certification & Mastery Assessment](./07-article-vii-certification.md)
**Status:** 📝 PLANNED (Not yet written)  
**Reading Time:** 20 minutes + practical exercises

Final validation before proceeding to Segment 3:

**SECTION 1: SEGMENT 2 KNOWLEDGE CHECK**
- Can you write a constitutional prompt (all 7 components)?
- Can you identify and apply the 12 patterns?
- Do you understand the 5-layer context framework?
- Can you refine AI output systematically?

**SECTION 2: PRACTICAL ASSESSMENT**
- **Challenge 1:** Generate prompt for a complex feature (e.g., multi-step checkout)
- **Challenge 2:** Debug AI-generated code with a subtle bug
- **Challenge 3:** Refine mediocre output to production quality (3+ iterations)
- **Challenge 4:** Orchestrate multi-AI workflow for a real project

**SECTION 3: GRADUATION CRITERIA**
- **Minimum Competency:** 80% on knowledge check + all challenges completed
- **Advanced Mastery:** 90%+ first-prompt success rate in real projects
- **Expert-Level:** Can teach others how to write constitutional prompts

**SECTION 4: NEXT STEPS (Transition to Segment 3)**
- What you've mastered (prompt engineering)
- What you'll learn next (tool selection, execution patterns)
- Preview of Segment 3

**Key Takeaway:** Knowledge without practice is useless. Validate your skills before moving on.

---

## 🎓 CERTIFICATION CHECKLIST

Before proceeding to Segment 3 (Tool Selection & Mastery), verify you can answer these questions:

### Knowledge Check

````markdown
□ Can you name all 7 components of a constitutional prompt?
  1. Persona Assignment
  2. Context Injection
  3. Task Definition
  4. Requirements Specification
  5. Security Mandates
  6. Meta-Instructions
  7. Output Format & Validation

□ Can you describe the 5 layers of context?
  1. Project Context
  2. Technical Context
  3. Feature Context
  4. Constraints Context
  5. Meta-Context

□ Can you name at least 8 of the 12 prompt patterns?
  (Scaffolding, Delta, Validator, Debugging, Integration, Refactoring,
   Testing, Architecture, Migration, Performance, Security Audit, Documentation)

□ Can you explain the 3-tier validation framework?
  - Tier 1: Visual Inspection (30 sec)
  - Tier 2: Requirement Checklist (2-5 min)
  - Tier 3: Functional Testing (5-15 min)

□ Can you identify when to use which AI tool?
  (Claude for reasoning, GPT for speed, Cursor for implementation)

□ Do you understand systematic refinement?
  (Iterate with purpose, not randomly)
````

### Practical Application

Can you apply this knowledge to a real scenario?

**Scenario:** You need to build a user authentication system with email/password and Google OAuth.

````markdown
□ Can you write a constitutional prompt for this feature?
  (Include all 7 components)

□ Can you identify which prompt pattern to use?
  (Hint: Scaffolding Pattern - new feature from scratch)

□ Can you engineer 5-layer context for this feature?
  (Project, Technical, Feature, Constraints, Meta)

□ Can you validate AI's output using 3-tier framework?
  (Visual → Requirements → Functional)

□ Can you refine the output if it's not perfect?
  (Use appropriate refinement prompts)
````

---

## ✅ COMPLETION STATUS

**Current Progress:** 🔄 45% COMPLETE

**What's Done:**
- ✅ Article I: Anatomy of Constitutional Prompt
- ✅ Article II: The 12 Prompt Patterns
- ✅ Article III: Context Engineering Mastery

**What's In Progress:**
- 🔄 Article IV: Iterative Refinement Mastery (20% complete)

**What's Planned:**
- ❌ Article V: Debugging Mastery
- ❌ Article VI: Advanced AI Orchestration
- ❌ Article VII: Certification & Mastery Assessment

**When Segment 2 is 100% complete, you will:**
- ✅ Command AI tools with expert-level prompts
- ✅ Achieve 90%+ first-prompt success rate
- ✅ Refine any AI output to production quality
- ✅ Debug AI-generated code systematically
- ✅ Orchestrate multiple AI tools in workflows
- ✅ Be ready for Segment 3 (Tool Selection & Mastery)

---

## 🚀 NEXT STEPS

### Option 1: Start Article I (Recommended if new to Segment 2)

Begin with the fundamentals:

**[→ Article I: Anatomy of Constitutional Prompt](./01-article-i-anatomy-of-prompt.md)**

Learn the 7 components that make prompts production-ready.

### Option 2: Review Foundation Layer (If struggling)

If you're finding Segment 2 difficult, you may need to review:

**[← Foundation Layer (Segment 1)](../01-foundation-layer/README.md)**

Ensure you understand:
- The Three Immutable Laws
- The STRATEG Pyramid
- The 10 Strategic Commandments

### Option 3: Jump to Specific Article (If experienced)

Already familiar with basic prompting? Jump to:

- [Article II: Prompt Patterns](./02-article-ii-prompt-patterns.md) - Learn reusable patterns
- [Article III: Context Engineering](./03-article-iii-context-engineering.md) - Master context
- [Article IV: Iterative Refinement](./04-article-iv-iterative-refinement.md) - Refine systematically

---

## 📖 QUICK REFERENCE

### The 7 Components of Constitutional Prompt

1. **Persona** - Assign AI a role/expertise
2. **Context** - Provide 5-layer context
3. **Task** - Define the specific task
4. **Requirements** - Explicit requirements
5. **Security** - Enforce 10 Commandments
6. **Meta-Instructions** - Tell AI HOW to think
7. **Output Format** - Define expected output

### The 12 Prompt Patterns

1. **Scaffolding** - New feature from scratch
2. **Delta** - Modify existing code
3. **Validator** - Review code for issues
4. **Debugging** - Fix bugs
5. **Integration** - Connect systems
6. **Refactoring** - Improve quality
7. **Testing** - Write tests
8. **Architecture** - Design systems
9. **Migration** - Upgrade libraries
10. **Performance** - Optimize speed
11. **Security Audit** - Validate security
12. **Documentation** - Generate docs

### The 5 Context Layers

1. **Project** - Business goals, users
2. **Technical** - Tech stack, architecture
3. **Feature** - Specific task
4. **Constraints** - Limitations, requirements
5. **Meta** - How AI should think

### The 3-Tier Validation Framework

1. **Visual** (30 sec) - Does it look right?
2. **Requirements** (2-5 min) - Does it meet requirements?
3. **Functional** (5-15 min) - Does it work?

---

## 🔗 NAVIGATION

**Current Location:** Technique Layer (Segment 2)

### Segment 2 Articles:
- [Article I: Anatomy of Prompt](./01-article-i-anatomy-of-prompt.md)
- [Article II: Prompt Patterns](./02-article-ii-prompt-patterns.md)
- [Article III: Context Engineering](./03-article-iii-context-engineering.md)
- [Article IV: Iterative Refinement](./04-article-iv-iterative-refinement.md)
- [Article V: Debugging Mastery](./05-article-v-debugging-mastery.md)
- [Article VI: AI Orchestration](./06-article-vi-ai-orchestration.md)
- [Article VII: Certification](./07-article-vii-certification.md)

### Other Sections:
- [↑ Constitution Index](../README.md)
- [← Foundation Layer (Segment 1)](../01-foundation-layer/README.md)
- [→ Tooling Layer (Segment 3)](../03-tooling-layer/README.md)
- [← Main README](../../README.md)

---

## 💬 FEEDBACK & CONTRIBUTIONS

Found an error? Have a suggestion? Want to contribute?

- **Report Issues:** [GitHub Issues](https://github.com/PromptHeroStudio/strateg-constitution-v2/issues)
- **Contribute:** See [CONTRIBUTING.md](../../CONTRIBUTING.md)
- **Discuss:** [GitHub Discussions](https://github.com/PromptHeroStudio/strateg-constitution-v2/discussions)

---

## 📊 READING STATISTICS

**Total Reading Time (when complete):** ~4 hours (240 minutes)  
**Documents:** 7 articles  
**Word Count:** ~60,000 words (estimated)  
**Code Examples:** 200+ (estimated)  
**Practical Exercises:** 30+ (estimated)

**Current Reading Time (45% complete):** ~2 hours (115 minutes)

**Recommended Pace:**
- **Week 1:** Article I + Article II (75 minutes)
- **Week 2:** Article III + Article IV (75 minutes)
- **Week 3:** Article V + Article VI (70 minutes)
- **Week 4:** Article VII + Practical Exercises (60 minutes)

---

## 🎯 PHILOSOPHY STATEMENT

> **"Segment 1 taught you to THINK like a strategist. Segment 2 teaches you to COMMUNICATE like one."**

**The Difference:**

**Beginner Prompt (20% success rate):**
```
"Create a login page with email and password"
```

**Constitutional Prompt (90% success rate):**
```
PERSONA: You are a senior Next.js 14 architect specializing in secure authentication.

CONTEXT:
- Project: B2B SaaS for project management (Next.js 14 App Router)
- Tech Stack: Next.js 14, Prisma, PostgreSQL, NextAuth.js, TailwindCSS
- Feature: User authentication system (email/password + Google OAuth)
- Constraints: WCAG AA accessible, <500ms load time, GDPR compliant

TASK: Create authentication system with:
1. Login page (/login)
2. Registration page (/register)
3. Server-side validation (Zod)
4. Session management (database sessions)
5. Protected routes (middleware)

REQUIREMENTS:
- Functional: Email/password + Google OAuth, "Remember me", password reset
- Security: bcrypt (cost 12), rate limiting (5 attempts/15 min), CSRF protection
- UX: Loading states, error messages, redirect after login

SECURITY MANDATES (10 Commandments):
- Commandment I: Server-side validation (Zod schemas)
- Commandment III: bcrypt hashing (cost 12)
- Commandment IV: Database sessions (HTTP-only cookies)
- Commandment V: Rate limiting (Upstash Redis)
- Commandment IX: WCAG AA accessible (semantic HTML, ARIA labels)

META-INSTRUCTIONS:
- Think step-by-step (validation → auth logic → UI)
- Validate assumptions (ask if unclear)
- Consider edge cases (password reset, email verification)
- Explain WHY you chose each approach

OUTPUT FORMAT:
1. File structure (list all files to create)
2. Implementation (complete code for each file)
3. Environment variables (required .env variables)
4. Testing checklist (how to validate it works)
```

**Result:** The constitutional prompt produces production-ready code in ONE iteration. The beginner prompt requires 5-10 iterations and still has security issues.

**Your mission in Segment 2:** Master constitutional prompting so AI becomes your most productive team member.

---

**Ready?** [Start with Article I: Anatomy of Constitutional Prompt →](./01-article-i-anatomy-of-prompt.md)

**Need Foundation?** [Review Segment 1 First ←](../01-foundation-layer/README.md)

**Lost?** [Return to Constitution Index ↑](../README.md)
