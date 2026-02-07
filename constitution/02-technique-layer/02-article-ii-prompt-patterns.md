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
