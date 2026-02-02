# ARTICLE IV: The 5 Constitutional Protocols

## ⚙️ Constitutional Status: EXECUTION ENGINE
These 5 protocols transform constitutional principles into actionable steps. They are the repeatable processes that guarantee consistent results.

---

## 📋 PROTOCOL #1: IDEA → MVP DECOMPOSITION

### **Purpose**
Break grand visions into buildable, testable increments that AI can execute.

### **Step-by-Step Process**
1. **Capture Raw Idea** (5 minutes)
   - Write your idea in one sentence
   - Example: "App that solves transport problems in Bosnia"

2. **Extract Core Value** (10 minutes)
   - Ask: "What ONE thing makes this valuable?"
   - Example: "Connecting truck drivers with available loads"

3. **Define MVP Boundary** (15 minutes)
   - Absolute minimum to test core value
   - Example: "Web form where drivers post available trucks"

4. **Decompose into AI-Tasks** (20 minutes)
   - Break into 2-hour max tasks
   - Example: 
     - Task 1: Basic HTML form
     - Task 2: Form validation
     - Task 3: Database to store submissions
     - Task 4: Display submissions on another page

### **Validation Criteria**
- [ ] Each task ≤ 2 hours
- [ ] Each task independently testable
- [ ] MVP tests core value hypothesis
- [ ] Clear "done" definition for each task

---

## 🎯 PROTOCOL #2: PROMPT GENERATION

### **Purpose**
Transform AI-tasks into constitutional prompts that produce world-class code.

### **The 7-Component Constitutional Prompt Template**
COMPONENT 1: Persona Assignment
"You are a senior full-stack developer specializing in [TECH STACK]"

COMPONENT 2: Context Injection
"We're building [PROJECT] to solve [PROBLEM]. Current status: [STATUS]"

COMPONENT 3: Task Definition
"Build [SPECIFIC TASK] that does [FUNCTION]"

COMPONENT 4: Requirements Specification
"Requirements: 1. [REQ1], 2. [REQ2], 3. [REQ3]"

COMPONENT 5: Security Mandates
"Security requirements: [SECURITY1], [SECURITY2]"

COMPONENT 6: Meta-Instructions
"Think step-by-step. Validate your own work. Ask clarifying questions."

COMPONENT 7: Output Format & Validation
"Output: [FORMAT]. Include: [VALIDATION CRITERIA]"

### **Example: Transport App Prompt**
You are a senior full-stack Next.js/TypeScript developer.

We're building a transport matching platform for Bosnia to connect truck drivers with available loads. Current status: We have basic Next.js setup.

Build a submission form where truck drivers can post available trucks.

Requirements:

Form fields: Driver name, truck type, capacity, available dates, contact phone

Phone number validation for Bosnian format (+387)

Responsive design that works on mobile

Success message after submission

Security requirements:

Input sanitization on all fields

No SQL injection vulnerabilities

Client-side validation

Think step-by-step. Validate your own work.

Output: Complete Next.js component with TypeScript. Include:

Full component code

Validation logic

Responsive CSS

Test instructions

---

## ✅ PROTOCOL #3: VALIDATION

### **Purpose**
Systematically verify AI output meets constitutional standards before proceeding.

### **3-Tier Validation Framework**

**TIER 1: Immediate Visual Inspection** (30 seconds)
- [ ] Code structure looks organized
- [ ] No obvious syntax errors
- [ ] Files are properly named
- [ ] Comments present where helpful

**TIER 2: Requirement Checklist** (2-5 minutes)
- [ ] All specified requirements implemented
- [ ] Edge cases considered
- [ ] Error handling present
- [ ] Responsive design working

**TIER 3: Functional Testing** (5-15 minutes)
- [ ] Run the code locally
- [ ] Test happy path (normal use)
- [ ] Test error paths (invalid inputs)
- [ ] Test on different screen sizes
- [ ] Verify security requirements

### **Validation Failure Protocol**
If validation fails:
1. Document exact failure point
2. Create refinement prompt (Protocol #4)
3. Do NOT proceed until validation passes

---

## 🔧 PROTOCOL #4: ERROR RECOVERY

### **Purpose**
Transform failures into learning and improvement opportunities.

### **Error Classification & Response**

| Error Type | Immediate Action | Constitutional Response |
|------------|-----------------|-------------------------|
| **Syntax Error** | Copy error message | "Fix these syntax errors: [PASTE ERROR]" |
| **Logic Error** | Describe unexpected behavior | "Debug this: Expected [X], got [Y]" |
| **Missing Feature** | List missing requirements | "Add [FEATURE] to existing code" |
| **Performance Issue** | Note slowness | "Optimize this for better performance" |
| **Security Gap** | Identify vulnerability | "Address security issue: [ISSUE]" |

### **The 5-Why Root Cause Analysis**
1. Why did error occur? (AI misunderstanding)
2. Why misunderstanding? (Prompt unclear)
3. Why prompt unclear? (Missing context)
4. Why missing context? (Didn't follow Protocol #2)
5. Why not follow protocol? (Rushed, skipped steps)

### **Recovery Workflow**
Error detected
↓
Classify error type
↓
Apply constitutional response
↓
5-Why analysis (prevent recurrence)
↓
Document in error log
↓
Continue with Protocol #3 validation

---

## 🔗 PROTOCOL #5: INTEGRATION

### **Purpose**
Combine validated components into cohesive system.

### **Integration Checklist**
**PRE-INTEGRATION** (Before connecting pieces)
- [ ] Each component individually validated
- [ ] API contracts defined
- [ ] Data flow mapped
- [ ] Integration test plan created

**DURING INTEGRATION** (Connecting pieces)
- [ ] Start with data layer (database → API)
- [ ] Add business logic layer (API → services)
- [ ] Connect presentation layer (UI → API)
- [ ] Test after each connection

**POST-INTEGRATION** (After connection)
- [ ] End-to-end testing
- [ ] Performance validation
- [ ] Security re-validation
- [ ] Documentation updated

### **Integration Order Protocol**
1. **Database schema** (if not exists)
2. **API endpoints** (backend)
3. **Data models/services** (business logic)
4. **UI components** (frontend)
5. **Connect UI to API** (integration)
6. **Authentication/authorization** (security)
7. **Testing suite** (validation)

### **Example: Transport App Integration**
Week 1: Database schema for trucks/drivers
Week 2: API endpoints for truck listings
Week 3: Basic UI to display listings
Week 4: Connect UI to API
Week 5: Add search/filter functionality
Week 6: Authentication system
Week 7: Testing and polish

---

## 🔄 PROTOCOL ORCHESTRATION

### **Daily Development Flow**
Morning: Protocol #1 (Plan day's increments)
Development: Protocol #2 → #3 loop (Build & validate)
Afternoon: Protocol #5 (Integrate day's work)
Evening: Protocol #4 review (Error analysis)

text

### **Weekly Protocol Review**
Every Friday:
1. Review Protocol adherence
2. Identify which protocols need strengthening
3. Update protocols based on learnings
4. Plan next week's protocol focus

---

## 🎯 IMMEDIATE APPLICATION

### **Exercise: Protocol Gap Analysis**
For your current project:

1. **Protocol #1**: Is my idea properly decomposed?
2. **Protocol #2**: Are my prompts constitutional quality?
3. **Protocol #3**: Am I validating thoroughly enough?
4. **Protocol #4**: Do I have error recovery process?
5. **Protocol #5**: Is my integration plan clear?

> 🎯 **Constitutional Directive**: Pick ONE protocol to strengthen today. Document specific improvement in `TODO.md`.

---

## 🔗 Constitutional Links
- **Previous**: [Article III: Non-Coder Advantage](../04-article-iii-non-coder-advantage.md)
- **Next**: [Article V: Checkpoints System](../06-article-v-checkpoints-system.md)
