# 🎯 ARTICLE I: TASK ORCHESTRATION

**Breaking Complex Requests into Executable Steps**

---

## 📜 CONSTITUTIONAL AUTHORITY

This article defines how MVCA decomposes complex user requests into atomic tasks and orchestrates their execution systematically.

**Legal Force:**
- ✅ MVCA **MUST** decompose complex tasks (>3 steps) into execution plans
- ✅ Execution plans **SHALL** be presented to users for approval
- ✅ Task dependencies **SHALL** be identified and respected
- ✅ Progress updates **SHALL** be provided at checkpoints
- ✅ Users **SHALL** be able to pause/modify/cancel execution at any time

**Constitutional Principle:**
> Complexity is tamed through decomposition.  
> Show the plan, execute systematically, report progress.

---

## 🎯 TASK DECOMPOSITION

### What is Task Decomposition?

**Definition:** Breaking a complex request into atomic, independently executable tasks.

**Atomic Task Criteria:**
```
A task is "atomic" if it:
1. Has a single, clear objective
2. Can be completed in one execution step
3. Produces a verifiable output
4. Takes <5 minutes to execute
5. Doesn't depend on user input mid-execution

Examples:
✓ "Create user.ts type definition" (atomic)
✓ "Write getUserById API route" (atomic)
✓ "Run npm install bcrypt" (atomic)

✗ "Build authentication system" (NOT atomic - too complex)
✗ "Make the app secure" (NOT atomic - too vague)
✗ "Fix all bugs" (NOT atomic - unbounded)
```

---

### Decomposition Algorithm
```typescript
/**
 * Task Decomposition Engine
 * Breaks complex requests into atomic tasks
 */
class TaskDecomposer {
  /**
   * Main decomposition method
   */
  async decompose(userRequest: string, context: Context): Promise<Task[]> {
    // Step 1: Understand intent
    const intent = await this.analyzeIntent(userRequest)
    
    // Step 2: Select decomposition strategy
    const strategy = this.selectStrategy(intent, context)
    
    // Step 3: Apply strategy
    const tasks = await strategy.decompose(userRequest, context)
    
    // Step 4: Validate atomicity
    const validatedTasks = this.validateAtomicity(tasks)
    
    // Step 5: Assign priorities
    const prioritizedTasks = this.assignPriorities(validatedTasks)
    
    return prioritizedTasks
  }
  
  /**
   * Analyze user intent
   */
  private async analyzeIntent(request: string): Promise<Intent> {
    // Classify intent using patterns
    if (this.matchesPattern(request, PATTERNS.BUILD_FEATURE)) {
      return {
        type: 'build_feature',
        complexity: this.estimateComplexity(request),
        domain: this.extractDomain(request)
      }
    }
    
    if (this.matchesPattern(request, PATTERNS.DEBUG_ISSUE)) {
      return {
        type: 'debug',
        severity: this.extractSeverity(request),
        component: this.extractComponent(request)
      }
    }
    
    if (this.matchesPattern(request, PATTERNS.REFACTOR_CODE)) {
      return {
        type: 'refactor',
        scope: this.extractScope(request),
        pattern: this.extractPattern(request)
      }
    }
    
    // ... more intent patterns
    
    return { type: 'general', complexity: 'medium' }
  }
  
  /**
   * Select appropriate decomposition strategy
   */
  private selectStrategy(intent: Intent, context: Context): DecompositionStrategy {
    switch (intent.type) {
      case 'build_feature':
        return new FeatureBuildStrategy()
      
      case 'debug':
        return new DebugStrategy()
      
      case 'refactor':
        return new RefactorStrategy()
      
      case 'deploy':
        return new DeploymentStrategy()
      
      case 'test':
        return new TestingStrategy()
      
      default:
        return new GeneralStrategy()
    }
  }
  
  /**
   * Validate that all tasks are atomic
   */
  private validateAtomicity(tasks: Task[]): Task[] {
    const validated: Task[] = []
    
    for (const task of tasks) {
      if (this.isAtomic(task)) {
        validated.push(task)
      } else {
        // Recursively decompose non-atomic tasks
        const subtasks = this.decomposeTask(task)
        validated.push(...subtasks)
      }
    }
    
    return validated
  }
  
  /**
   * Check if task is atomic
   */
  private isAtomic(task: Task): boolean {
    return (
      task.estimatedTime < 300000 &&  // < 5 minutes
      task.steps.length === 1 &&       // Single step
      task.dependencies.length <= 3 &&  // Few dependencies
      !task.requiresUserInput          // No mid-execution input
    )
  }
  
  /**
   * Assign priorities based on dependencies
   */
  private assignPriorities(tasks: Task[]): Task[] {
    // Topological sort based on dependencies
    const sorted = this.topologicalSort(tasks)
    
    // Assign priority (higher number = execute first)
    sorted.forEach((task, index) => {
      task.priority = sorted.length - index
    })
    
    return sorted
  }
  
  /**
   * Topological sort (respects dependencies)
   */
  private topologicalSort(tasks: Task[]): Task[] {
    const sorted: Task[] = []
    const visited = new Set<string>()
    const visiting = new Set<string>()
    
    const visit = (task: Task) => {
      if (visited.has(task.id)) return
      if (visiting.has(task.id)) {
        throw new Error(`Circular dependency detected: ${task.id}`)
      }
      
      visiting.add(task.id)
      
      // Visit dependencies first
      for (const depId of task.dependencies) {
        const dep = tasks.find(t => t.id === depId)
        if (dep) visit(dep)
      }
      
      visiting.delete(task.id)
      visited.add(task.id)
      sorted.push(task)
    }
    
    for (const task of tasks) {
      visit(task)
    }
    
    return sorted
  }
}
```

---

### Decomposition Strategies

#### Strategy 1: Feature Build Strategy

**When:** User requests a new feature

**Pattern:**
```
1. Understand Requirements (gather context)
2. Plan Architecture (decide structure)
3. Scaffold Structure (create files, types)
4. Implement Backend (API, database)
5. Implement Frontend (UI components)
6. Add Tests (unit + integration)
7. Validate & Review
```

**Example:**
```typescript
class FeatureBuildStrategy implements DecompositionStrategy {
  async decompose(request: string, context: Context): Promise<Task[]> {
    const feature = this.extractFeatureName(request)
    
    // Phase 1: Scaffolding
    const scaffoldingTasks = [
      {
        id: 'scaffold-1',
        name: `Create ${feature} folder structure`,
        type: 'file_system',
        action: 'create_directories',
        estimatedTime: 30000,  // 30 seconds
        dependencies: [],
        priority: 100
      },
      {
        id: 'scaffold-2',
        name: `Define ${feature} TypeScript types`,
        type: 'code_generation',
        action: 'create_types',
        estimatedTime: 60000,  // 1 minute
        dependencies: ['scaffold-1'],
        priority: 99
      },
      {
        id: 'scaffold-3',
        name: `Update Prisma schema for ${feature}`,
        type: 'database',
        action: 'update_schema',
        estimatedTime: 120000,  // 2 minutes
        dependencies: ['scaffold-2'],
        priority: 98,
        checkpoint: true  // User reviews schema before proceeding
      }
    ]
    
    // Phase 2: Implementation
    const implementationTasks = [
      {
        id: 'impl-1',
        name: `Implement ${feature} API routes`,
        type: 'code_generation',
        action: 'create_api_routes',
        estimatedTime: 180000,  // 3 minutes
        dependencies: ['scaffold-3'],
        priority: 97
      },
      {
        id: 'impl-2',
        name: `Implement ${feature} UI components`,
        type: 'code_generation',
        action: 'create_components',
        estimatedTime: 240000,  // 4 minutes
        dependencies: ['impl-1'],
        priority: 96
      },
      {
        id: 'impl-3',
        name: `Add ${feature} validation logic`,
        type: 'code_generation',
        action: 'add_validation',
        estimatedTime: 90000,  // 1.5 minutes
        dependencies: ['impl-1'],
        priority: 95
      }
    ]
    
    // Phase 3: Testing
    const testingTasks = [
      {
        id: 'test-1',
        name: `Create ${feature} unit tests`,
        type: 'code_generation',
        action: 'create_tests',
        estimatedTime: 180000,  // 3 minutes
        dependencies: ['impl-2', 'impl-3'],
        priority: 94
      },
      {
        id: 'test-2',
        name: `Run ${feature} tests`,
        type: 'execution',
        action: 'run_tests',
        estimatedTime: 60000,  // 1 minute
        dependencies: ['test-1'],
        priority: 93
      }
    ]
    
    return [
      ...scaffoldingTasks,
      ...implementationTasks,
      ...testingTasks
    ]
  }
}
```

**Example Decomposition:**
```
User Request: "Add password reset feature"

Decomposed Tasks:
┌─────────────────────────────────────────────────────────┐
│ PHASE 1: SCAFFOLDING (Checkpoint)                       │
├─────────────────────────────────────────────────────────┤
│ 1. Create password-reset folder structure               │
│    Dependencies: None                                    │
│    Estimated: 30s                                        │
│                                                          │
│ 2. Define ResetToken TypeScript type                    │
│    Dependencies: [1]                                     │
│    Estimated: 1m                                         │
│                                                          │
│ 3. Update Prisma schema (ResetToken model)             │
│    Dependencies: [2]                                     │
│    Estimated: 2m                                         │
│    ⚠️  CHECKPOINT: User reviews schema                   │
└─────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────┐
│ PHASE 2: IMPLEMENTATION                                  │
├─────────────────────────────────────────────────────────┤
│ 4. Implement /api/auth/forgot-password route           │
│    Dependencies: [3]                                     │
│    Estimated: 3m                                         │
│                                                          │
│ 5. Implement /api/auth/reset-password route            │
│    Dependencies: [3]                                     │
│    Estimated: 3m                                         │
│                                                          │
│ 6. Create password reset page UI                        │
│    Dependencies: [5]                                     │
│    Estimated: 4m                                         │
│                                                          │
│ 7. Add email sending logic (SendGrid)                  │
│    Dependencies: [4]                                     │
│    Estimated: 2m                                         │
│                                                          │
│ 8. Add input validation (Zod schemas)                  │
│    Dependencies: [4, 5]                                  │
│    Estimated: 1.5m                                       │
└─────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────┐
│ PHASE 3: TESTING                                         │
├─────────────────────────────────────────────────────────┤
│ 9. Create unit tests                                    │
│    Dependencies: [4, 5, 6, 7, 8]                        │
│    Estimated: 3m                                         │
│                                                          │
│ 10. Run tests                                           │
│     Dependencies: [9]                                    │
│     Estimated: 1m                                        │
└─────────────────────────────────────────────────────────┘

Total Tasks: 10
Total Estimated Time: ~20 minutes
Checkpoints: 1 (after Phase 1)
```

---

#### Strategy 2: Debug Strategy

**When:** User reports a bug or issue

**Pattern:**
```
1. Reproduce Issue (verify bug exists)
2. Collect Evidence (logs, error messages)
3. Analyze Root Cause (identify problem)
4. Generate Fix (code changes)
5. Verify Fix (test that it works)
6. Prevent Regression (add test)
```

**Example:**
```typescript
class DebugStrategy implements DecompositionStrategy {
  async decompose(request: string, context: Context): Promise<Task[]> {
    const issue = this.extractIssueDescription(request)
    
    return [
      {
        id: 'debug-1',
        name: 'Reproduce issue',
        type: 'analysis',
        action: 'reproduce_bug',
        estimatedTime: 120000,  // 2 minutes
        description: `Attempt to reproduce: ${issue}`,
        dependencies: []
      },
      {
        id: 'debug-2',
        name: 'Collect evidence',
        type: 'analysis',
        action: 'gather_logs',
        estimatedTime: 60000,  // 1 minute
        description: 'Gather error logs, stack traces, relevant code',
        dependencies: ['debug-1']
      },
      {
        id: 'debug-3',
        name: 'Analyze root cause',
        type: 'analysis',
        action: 'identify_cause',
        estimatedTime: 180000,  // 3 minutes
        description: 'Identify why the bug is happening',
        dependencies: ['debug-2'],
        checkpoint: true  // Present findings to user
      },
      {
        id: 'debug-4',
        name: 'Generate fix',
        type: 'code_generation',
        action: 'create_fix',
        estimatedTime: 120000,  // 2 minutes
        description: 'Write code to fix the root cause',
        dependencies: ['debug-3']
      },
      {
        id: 'debug-5',
        name: 'Verify fix',
        type: 'execution',
        action: 'test_fix',
        estimatedTime: 90000,  // 1.5 minutes
        description: 'Test that fix resolves the issue',
        dependencies: ['debug-4']
      },
      {
        id: 'debug-6',
        name: 'Add regression test',
        type: 'code_generation',
        action: 'create_test',
        estimatedTime: 120000,  // 2 minutes
        description: 'Add test to prevent this bug from recurring',
        dependencies: ['debug-5']
      }
    ]
  }
}
```

---

#### Strategy 3: Refactor Strategy

**When:** User wants to improve existing code

**Pattern:**
```
1. Analyze Current Code (understand structure)
2. Identify Improvements (what to refactor)
3. Plan Refactoring (step-by-step approach)
4. Execute Refactoring (make changes)
5. Verify Behavior Unchanged (tests still pass)
6. Optimize Further (if needed)
```

---

#### Strategy 4: Deployment Strategy

**When:** User wants to deploy application

**Pattern:**
```
1. Pre-deployment Checks (tests pass, env vars set)
2. Build Application (compile, bundle)
3. Deploy to Platform (Vercel, Netlify, etc.)
4. Post-deployment Verification (smoke tests)
5. Rollback if Failed (restore previous version)
```

---

## 📊 EXECUTION PLAN CREATION

### Execution Plan Structure
```typescript
/**
 * Execution Plan
 * Complete plan for executing a complex request
 */
interface ExecutionPlan {
  // IDENTITY
  id: string                        // Unique plan ID
  userRequest: string               // Original user request
  createdAt: Date
  
  // TASKS
  tasks: Task[]                     // All tasks to execute
  totalTasks: number
  
  // PHASES (Logical grouping of tasks)
  phases: Phase[]
  
  // DEPENDENCIES
  dependencyGraph: DependencyGraph  // DAG of task dependencies
  
  // ESTIMATES
  estimatedTime: number             // Total time (milliseconds)
  estimatedCost: number             // Total cost (USD)
  
  // CHECKPOINTS
  checkpoints: Checkpoint[]         // User approval points
  
  // STATUS
  status: 'pending' | 'approved' | 'executing' | 'completed' | 'failed' | 'cancelled'
  currentTask?: string              // ID of currently executing task
  completedTasks: string[]          // IDs of completed tasks
  
  // RESULTS
  results: Map<string, TaskResult>  // Results of completed tasks
  
  // METADATA
  complexity: 'simple' | 'medium' | 'complex'
  riskLevel: 'low' | 'medium' | 'high'
}

/**
 * Task Definition
 */
interface Task {
  id: string
  name: string
  description: string
  
  // TYPE & ACTION
  type: 'code_generation' | 'file_system' | 'database' | 'execution' | 'analysis' | 'tool_call'
  action: string                    // Specific action to perform
  
  // EXECUTION
  tool?: string                     // Tool to use (if type = 'tool_call')
  parameters?: Record<string, any>  // Tool parameters
  code?: string                     // Code to execute (if type = 'execution')
  
  // TIMING
  estimatedTime: number             // Estimated duration (ms)
  timeout?: number                  // Max execution time
  
  // DEPENDENCIES
  dependencies: string[]            // IDs of tasks that must complete first
  priority: number                  // Higher = execute first (when dependencies satisfied)
  
  // CHECKPOINT
  checkpoint: boolean               // Pause for user approval after this task
  
  // STATUS
  status: 'pending' | 'ready' | 'executing' | 'completed' | 'failed' | 'skipped'
  startedAt?: Date
  completedAt?: Date
  error?: Error
  
  // RETRY
  retryable: boolean
  retryCount: number
  maxRetries: number
}

/**
 * Phase Definition
 * Logical grouping of related tasks
 */
interface Phase {
  id: string
  name: string
  description: string
  tasks: string[]                   // Task IDs in this phase
  checkpoint: boolean               // Pause after this phase
  estimatedTime: number
}

/**
 * Checkpoint Definition
 * Points where execution pauses for user approval
 */
interface Checkpoint {
  id: string
  name: string
  description: string
  afterTask: string                 // Task ID after which this checkpoint occurs
  questions?: string[]              // Questions to ask user
  approved?: boolean
  userFeedback?: string
}
```

---

### Plan Creation Example
```typescript
/**
 * Execution Plan Builder
 */
class ExecutionPlanBuilder {
  /**
   * Build execution plan from decomposed tasks
   */
  async buildPlan(
    userRequest: string,
    tasks: Task[],
    context: Context
  ): Promise<ExecutionPlan> {
    
    // Create dependency graph
    const graph = this.buildDependencyGraph(tasks)
    
    // Detect circular dependencies
    if (this.hasCircularDependencies(graph)) {
      throw new Error('Circular dependency detected in task plan')
    }
    
    // Group tasks into phases
    const phases = this.groupIntoPhases(tasks)
    
    // Identify checkpoints
    const checkpoints = this.identifyCheckpoints(tasks, phases)
    
    // Calculate estimates
    const estimatedTime = this.calculateTotalTime(tasks, graph)
    const estimatedCost = this.calculateTotalCost(tasks)
    
    // Assess complexity and risk
    const complexity = this.assessComplexity(tasks)
    const riskLevel = this.assessRisk(tasks, context)
    
    return {
      id: generateId(),
      userRequest,
      createdAt: new Date(),
      tasks,
      totalTasks: tasks.length,
      phases,
      dependencyGraph: graph,
      estimatedTime,
      estimatedCost,
      checkpoints,
      status: 'pending',
      completedTasks: [],
      results: new Map(),
      complexity,
      riskLevel
    }
  }
  
  /**
   * Build dependency graph (Directed Acyclic Graph)
   */
  private buildDependencyGraph(tasks: Task[]): DependencyGraph {
    const graph: DependencyGraph = {
      nodes: new Map(),
      edges: []
    }
    
    // Add nodes
    for (const task of tasks) {
      graph.nodes.set(task.id, {
        task,
        inDegree: 0,
        outDegree: 0
      })
    }
    
    // Add edges
    for (const task of tasks) {
      for (const depId of task.dependencies) {
        graph.edges.push({
          from: depId,
          to: task.id
        })
        
        // Update degrees
        graph.nodes.get(depId)!.outDegree++
        graph.nodes.get(task.id)!.inDegree++
      }
    }
    
    return graph
  }
  
  /**
   * Detect circular dependencies using DFS
   */
  private hasCircularDependencies(graph: DependencyGraph): boolean {
    const visited = new Set<string>()
    const recursionStack = new Set<string>()
    
    const hasCycle = (nodeId: string): boolean => {
      visited.add(nodeId)
      recursionStack.add(nodeId)
      
      // Check all neighbors
      const neighbors = graph.edges
        .filter(e => e.from === nodeId)
        .map(e => e.to)
      
      for (const neighbor of neighbors) {
        if (!visited.has(neighbor)) {
          if (hasCycle(neighbor)) return true
        } else if (recursionStack.has(neighbor)) {
          return true  // Cycle detected!
        }
      }
      
      recursionStack.delete(nodeId)
      return false
    }
    
    // Check each node
    for (const nodeId of graph.nodes.keys()) {
      if (!visited.has(nodeId)) {
        if (hasCycle(nodeId)) return true
      }
    }
    
    return false
  }
  
  /**
   * Group tasks into logical phases
   */
  private groupIntoPhases(tasks: Task[]): Phase[] {
    const phases: Phase[] = []
    const assigned = new Set<string>()
    
    // Phase 1: Tasks with no dependencies
    const phase1Tasks = tasks.filter(t => t.dependencies.length === 0)
    if (phase1Tasks.length > 0) {
      phases.push({
        id: 'phase-1',
        name: this.inferPhaseName(phase1Tasks),
        description: 'Initial setup and scaffolding',
        tasks: phase1Tasks.map(t => t.id),
        checkpoint: phase1Tasks.some(t => t.checkpoint),
        estimatedTime: phase1Tasks.reduce((sum, t) => sum + t.estimatedTime, 0)
      })
      phase1Tasks.forEach(t => assigned.add(t.id))
    }
    
    // Subsequent phases: Group by dependency depth
    let phaseNum = 2
    while (assigned.size < tasks.length) {
      const phaseTasks = tasks.filter(t => 
        !assigned.has(t.id) &&
        t.dependencies.every(depId => assigned.has(depId))
      )
      
      if (phaseTasks.length === 0) {
        break  // No more tasks can be assigned (circular dependency)
      }
      
      phases.push({
        id: `phase-${phaseNum}`,
        name: this.inferPhaseName(phaseTasks),
        description: `Phase ${phaseNum}`,
        tasks: phaseTasks.map(t => t.id),
        checkpoint: phaseTasks.some(t => t.checkpoint),
        estimatedTime: phaseTasks.reduce((sum, t) => sum + t.estimatedTime, 0)
      })
      
      phaseTasks.forEach(t => assigned.add(t.id))
      phaseNum++
    }
    
    return phases
  }
  
  /**
   * Infer phase name from tasks
   */
  private inferPhaseName(tasks: Task[]): string {
    // Look for common patterns
    if (tasks.every(t => t.type === 'file_system')) {
      return 'Scaffolding'
    }
    if (tasks.some(t => t.name.includes('test'))) {
      return 'Testing'
    }
    if (tasks.some(t => t.name.includes('deploy'))) {
      return 'Deployment'
    }
    if (tasks.some(t => t.action.includes('create'))) {
      return 'Implementation'
    }
    
    return 'Execution'
  }
  
  /**
   * Identify checkpoint locations
   */
  private identifyCheckpoints(tasks: Task[], phases: Phase[]): Checkpoint[] {
    const checkpoints: Checkpoint[] = []
    
    // Checkpoint after each phase that has checkpoint flag
    for (const phase of phases) {
      if (phase.checkpoint) {
        const lastTask = phase.tasks[phase.tasks.length - 1]
        checkpoints.push({
          id: `checkpoint-${phase.id}`,
          name: `Review ${phase.name}`,
          description: `Review ${phase.name} before proceeding`,
          afterTask: lastTask,
          questions: this.generateCheckpointQuestions(phase)
        })
      }
    }
    
    // Checkpoint before any high-risk task
    const highRiskTasks = tasks.filter(t => this.isHighRisk(t))
    for (const task of highRiskTasks) {
      if (!checkpoints.some(cp => cp.afterTask === task.id)) {
        checkpoints.push({
          id: `checkpoint-${task.id}`,
          name: `Confirm ${task.name}`,
          description: `This operation is high-risk. Confirm before proceeding.`,
          afterTask: this.findPreviousTask(task, tasks)?.id || task.id,
          questions: [`Proceed with "${task.name}"?`]
        })
      }
    }
    
    return checkpoints
  }
  
  /**
   * Calculate total estimated time (considers parallel execution)
   */
  private calculateTotalTime(tasks: Task[], graph: DependencyGraph): number {
    // Use critical path method
    const criticalPath = this.findCriticalPath(graph)
    return criticalPath.reduce((sum, taskId) => {
      const task = tasks.find(t => t.id === taskId)
      return sum + (task?.estimatedTime || 0)
    }, 0)
  }
  
  /**
   * Find critical path (longest path through graph)
   */
  private findCriticalPath(graph: DependencyGraph): string[] {
    // Dynamic programming to find longest path
    const longestPath = new Map<string, number>()
    const parent = new Map<string, string>()
    
    // Topological sort
    const sorted = this.topologicalSort(graph)
    
    // Initialize
    for (const nodeId of sorted) {
      longestPath.set(nodeId, 0)
    }
    
    // Calculate longest paths
    for (const nodeId of sorted) {
      const node = graph.nodes.get(nodeId)!
      const currentLength = longestPath.get(nodeId)!
      
      // Check all outgoing edges
      const outgoing = graph.edges.filter(e => e.from === nodeId)
      for (const edge of outgoing) {
        const newLength = currentLength + node.task.estimatedTime
        if (newLength > longestPath.get(edge.to)!) {
          longestPath.set(edge.to, newLength)
          parent.set(edge.to, nodeId)
        }
      }
    }
    
    // Reconstruct path
    let maxLength = 0
    let endNode = ''
    for (const [nodeId, length] of longestPath) {
      if (length > maxLength) {
        maxLength = length
        endNode = nodeId
      }
    }
    
    const path: string[] = []
    let current: string | undefined = endNode
    while (current) {
      path.unshift(current)
      current = parent.get(current)
    }
    
    return path
  }
  
  /**
   * Assess plan complexity
   */
  private assessComplexity(tasks: Task[]): 'simple' | 'medium' | 'complex' {
    if (tasks.length <= 3) return 'simple'
    if (tasks.length <= 10) return 'medium'
    return 'complex'
  }
  
  /**
   * Assess risk level
   */
  private assessRisk(tasks: Task[], context: Context): 'low' | 'medium' | 'high' {
    // Check for high-risk tasks
    const hasDestructiveOps = tasks.some(t => 
      t.action.includes('delete') || 
      t.action.includes('drop') ||
      t.action.includes('truncate')
    )
    
    const hasExternalAPICalls = tasks.some(t =>
      t.type === 'tool_call' && 
      t.tool?.includes('stripe') ||
      t.tool?.includes('payment')
    )
    
    const hasProductionDeployment = tasks.some(t =>
      t.name.includes('production') ||
      t.name.includes('deploy to prod')
    )
    
    if (hasDestructiveOps || hasExternalAPICalls || hasProductionDeployment) {
      return 'high'
    }
    
    if (tasks.length > 10 || context.environment === 'production') {
      return 'medium'
    }
    
    return 'low'
  }
}
```

---
## 📋 PLAN PRESENTATION

### Presenting Plans to Users
```typescript
/**
 * Plan Presenter
 * Formats execution plans for user approval
 */
class ExecutionPlanPresenter {
  /**
   * Present plan to user in readable format
   */
  present(plan: ExecutionPlan): string {
    let presentation = ''
    
    // Header
    presentation += this.formatHeader(plan)
    
    // Summary
    presentation += this.formatSummary(plan)
    
    // Phases
    presentation += this.formatPhases(plan)
    
    // Warnings (if any)
    presentation += this.formatWarnings(plan)
    
    // Approval prompt
    presentation += this.formatApprovalPrompt(plan)
    
    return presentation
  }
  
  /**
   * Format header
   */
  private formatHeader(plan: ExecutionPlan): string {
    const complexity = this.getComplexityEmoji(plan.complexity)
    const risk = this.getRiskEmoji(plan.riskLevel)
    
    return `
╔═══════════════════════════════════════════════════════════╗
║                   EXECUTION PLAN                           ║
╚═══════════════════════════════════════════════════════════╝

Request: "${plan.userRequest}"

Complexity: ${complexity} ${plan.complexity.toUpperCase()}
Risk Level: ${risk} ${plan.riskLevel.toUpperCase()}
`
  }
  
  /**
   * Format summary
   */
  private formatSummary(plan: ExecutionPlan): string {
    const time = this.formatDuration(plan.estimatedTime)
    const cost = plan.estimatedCost > 0 ? `$${plan.estimatedCost.toFixed(2)}` : 'Free'
    
    return `
┌───────────────────────────────────────────────────────────┐
│ SUMMARY                                                    │
├───────────────────────────────────────────────────────────┤
│ Total Tasks:      ${plan.totalTasks.toString().padEnd(44)} │
│ Phases:           ${plan.phases.length.toString().padEnd(44)} │
│ Checkpoints:      ${plan.checkpoints.length.toString().padEnd(44)} │
│ Estimated Time:   ${time.padEnd(44)} │
│ Estimated Cost:   ${cost.padEnd(44)} │
└───────────────────────────────────────────────────────────┘
`
  }
  
  /**
   * Format phases
   */
  private formatPhases(plan: ExecutionPlan): string {
    let output = '\n'
    
    for (let i = 0; i < plan.phases.length; i++) {
      const phase = plan.phases[i]
      const tasks = phase.tasks.map(id => plan.tasks.find(t => t.id === id)!)
      const time = this.formatDuration(phase.estimatedTime)
      
      output += `
┌───────────────────────────────────────────────────────────┐
│ PHASE ${i + 1}: ${phase.name.toUpperCase().padEnd(48)} │
├───────────────────────────────────────────────────────────┤
│ Estimated Time: ${time.padEnd(44)} │
├───────────────────────────────────────────────────────────┤
`
      
      for (let j = 0; j < tasks.length; j++) {
        const task = tasks[j]
        const taskTime = this.formatDuration(task.estimatedTime)
        const icon = this.getTaskIcon(task.type)
        
        output += `│ ${(j + 1).toString().padStart(2)}. ${icon} ${task.name.padEnd(41)} │\n`
        output += `│     └─ ${taskTime.padEnd(48)} │\n`
        
        if (task.checkpoint) {
          output += `│     ⚠️  CHECKPOINT: Review before proceeding${' '.repeat(14)} │\n`
        }
      }
      
      output += `└───────────────────────────────────────────────────────────┘\n`
    }
    
    return output
  }
  
  /**
   * Format warnings
   */
  private formatWarnings(plan: ExecutionPlan): string {
    const warnings: string[] = []
    
    // Risk warnings
    if (plan.riskLevel === 'high') {
      warnings.push('⚠️  HIGH RISK: This plan includes operations that could affect production data or incur costs.')
    }
    
    // Long execution warnings
    if (plan.estimatedTime > 600000) {  // > 10 minutes
      warnings.push('⏱️  LONG EXECUTION: This plan will take more than 10 minutes to complete.')
    }
    
    // Cost warnings
    if (plan.estimatedCost > 10) {
      warnings.push(`💰 COST WARNING: This plan will incur approximately $${plan.estimatedCost.toFixed(2)} in service fees.`)
    }
    
    // External API warnings
    const externalAPIs = plan.tasks.filter(t => 
      t.type === 'tool_call' && 
      t.tool?.includes('stripe') ||
      t.tool?.includes('sendgrid') ||
      t.tool?.includes('twilio')
    )
    if (externalAPIs.length > 0) {
      warnings.push(`🌐 EXTERNAL APIS: This plan will call ${externalAPIs.length} external service(s).`)
    }
    
    if (warnings.length === 0) return ''
    
    return `
┌───────────────────────────────────────────────────────────┐
│ WARNINGS                                                   │
├───────────────────────────────────────────────────────────┤
${warnings.map(w => `│ ${w.padEnd(58)} │`).join('\n')}
└───────────────────────────────────────────────────────────┘
`
  }
  
  /**
   * Format approval prompt
   */
  private formatApprovalPrompt(plan: ExecutionPlan): string {
    return `
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

Proceed with this execution plan? (Y/n)

Commands:
  Y/yes   - Proceed with execution
  n/no    - Cancel
  m/modify - Modify plan before execution
  d/details - Show detailed task information

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
`
  }
  
  // Helper methods
  private getComplexityEmoji(complexity: string): string {
    switch (complexity) {
      case 'simple': return '🟢'
      case 'medium': return '🟡'
      case 'complex': return '🔴'
      default: return '⚪'
    }
  }
  
  private getRiskEmoji(risk: string): string {
    switch (risk) {
      case 'low': return '✅'
      case 'medium': return '⚠️'
      case 'high': return '🚨'
      default: return '❓'
    }
  }
  
  private getTaskIcon(type: string): string {
    const icons: Record<string, string> = {
      'code_generation': '📝',
      'file_system': '📁',
      'database': '🗄️',
      'execution': '⚙️',
      'analysis': '🔍',
      'tool_call': '🔧'
    }
    return icons[type] || '•'
  }
  
  private formatDuration(ms: number): string {
    if (ms < 60000) {
      return `${Math.ceil(ms / 1000)}s`
    }
    const minutes = Math.floor(ms / 60000)
    const seconds = Math.ceil((ms % 60000) / 1000)
    return seconds > 0 ? `${minutes}m ${seconds}s` : `${minutes}m`
  }
}
```

**Example Output:**
```
╔═══════════════════════════════════════════════════════════╗
║                   EXECUTION PLAN                           ║
╚═══════════════════════════════════════════════════════════╝

Request: "Add password reset feature"

Complexity: 🟡 MEDIUM
Risk Level: ✅ LOW

┌───────────────────────────────────────────────────────────┐
│ SUMMARY                                                    │
├───────────────────────────────────────────────────────────┤
│ Total Tasks:      10                                       │
│ Phases:           3                                        │
│ Checkpoints:      1                                        │
│ Estimated Time:   18m 30s                                  │
│ Estimated Cost:   Free                                     │
└───────────────────────────────────────────────────────────┘

┌───────────────────────────────────────────────────────────┐
│ PHASE 1: SCAFFOLDING                                       │
├───────────────────────────────────────────────────────────┤
│ Estimated Time: 3m 30s                                     │
├───────────────────────────────────────────────────────────┤
│  1. 📁 Create password-reset folder structure              │
│     └─ 30s                                                 │
│  2. 📝 Define ResetToken TypeScript type                   │
│     └─ 1m                                                  │
│  3. 🗄️ Update Prisma schema (ResetToken model)            │
│     └─ 2m                                                  │
│     ⚠️  CHECKPOINT: Review before proceeding               │
└───────────────────────────────────────────────────────────┘

┌───────────────────────────────────────────────────────────┐
│ PHASE 2: IMPLEMENTATION                                    │
├───────────────────────────────────────────────────────────┤
│ Estimated Time: 12m                                        │
├───────────────────────────────────────────────────────────┤
│  4. 📝 Implement /api/auth/forgot-password route           │
│     └─ 3m                                                  │
│  5. 📝 Implement /api/auth/reset-password route            │
│     └─ 3m                                                  │
│  6. 📝 Create password reset page UI                       │
│     └─ 4m                                                  │
│  7. 🔧 Add email sending logic (SendGrid)                  │
│     └─ 2m                                                  │
└───────────────────────────────────────────────────────────┘

┌───────────────────────────────────────────────────────────┐
│ PHASE 3: TESTING                                           │
├───────────────────────────────────────────────────────────┤
│ Estimated Time: 3m                                         │
├───────────────────────────────────────────────────────────┤
│  8. 📝 Create unit tests                                   │
│     └─ 2m                                                  │
│  9. ⚙️ Run tests                                           │
│     └─ 1m                                                  │
└───────────────────────────────────────────────────────────┘

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

Proceed with this execution plan? (Y/n)

Commands:
  Y/yes   - Proceed with execution
  n/no    - Cancel
  m/modify - Modify plan before execution
  d/details - Show detailed task information

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

## ⚙️ EXECUTION ORCHESTRATION

### The Orchestration Engine
```typescript
/**
 * Task Orchestrator
 * Executes tasks according to plan
 */
class TaskOrchestrator {
  private plan: ExecutionPlan
  private executor: TaskExecutor
  private stateManager: StateManager
  
  constructor(
    plan: ExecutionPlan,
    executor: TaskExecutor,
    stateManager: StateManager
  ) {
    this.plan = plan
    this.executor = executor
    this.stateManager = stateManager
  }
  
  /**
   * Execute the plan
   */
  async execute(): Promise<ExecutionResult> {
    try {
      // Update status
      this.plan.status = 'executing'
      await this.stateManager.savePlan(this.plan)
      
      // Execute phases sequentially
      for (const phase of this.plan.phases) {
        await this.executePhase(phase)
        
        // Check for checkpoint
        if (phase.checkpoint) {
          const checkpoint = this.plan.checkpoints.find(
            cp => cp.afterTask === phase.tasks[phase.tasks.length - 1]
          )
          
          if (checkpoint) {
            const approved = await this.handleCheckpoint(checkpoint)
            if (!approved) {
              this.plan.status = 'cancelled'
              return {
                success: false,
                reason: 'User cancelled at checkpoint',
                completedTasks: this.plan.completedTasks.length,
                totalTasks: this.plan.totalTasks
              }
            }
          }
        }
      }
      
      // All phases complete
      this.plan.status = 'completed'
      await this.stateManager.savePlan(this.plan)
      
      return {
        success: true,
        completedTasks: this.plan.completedTasks.length,
        totalTasks: this.plan.totalTasks,
        results: this.plan.results
      }
      
    } catch (error) {
      this.plan.status = 'failed'
      await this.stateManager.savePlan(this.plan)
      
      return {
        success: false,
        reason: error.message,
        completedTasks: this.plan.completedTasks.length,
        totalTasks: this.plan.totalTasks,
        error
      }
    }
  }
  
  /**
   * Execute a single phase
   */
  private async executePhase(phase: Phase): Promise<void> {
    const phaseTasks = phase.tasks.map(id => 
      this.plan.tasks.find(t => t.id === id)!
    )
    
    // Report phase start
    this.reportProgress({
      type: 'phase_start',
      phase: phase.name,
      tasksInPhase: phaseTasks.length
    })
    
    // Identify tasks that can run in parallel
    const parallelBatches = this.identifyParallelBatches(phaseTasks)
    
    // Execute batches
    for (const batch of parallelBatches) {
      if (batch.length === 1) {
        // Single task - execute sequentially
        await this.executeTask(batch[0])
      } else {
        // Multiple tasks - execute in parallel
        await this.executeTasksInParallel(batch)
      }
    }
    
    // Report phase complete
    this.reportProgress({
      type: 'phase_complete',
      phase: phase.name
    })
  }
  
  /**
   * Execute a single task
   */
  private async executeTask(task: Task): Promise<void> {
    // Check dependencies satisfied
    const dependenciesSatisfied = task.dependencies.every(depId =>
      this.plan.completedTasks.includes(depId)
    )
    
    if (!dependenciesSatisfied) {
      throw new Error(`Task ${task.id} dependencies not satisfied`)
    }
    
    // Update task status
    task.status = 'executing'
    task.startedAt = new Date()
    this.plan.currentTask = task.id
    await this.stateManager.savePlan(this.plan)
    
    // Report task start
    this.reportProgress({
      type: 'task_start',
      task: task.name,
      estimatedTime: task.estimatedTime
    })
    
    try {
      // Execute task
      const result = await this.executor.execute(task)
      
      // Task succeeded
      task.status = 'completed'
      task.completedAt = new Date()
      this.plan.completedTasks.push(task.id)
      this.plan.results.set(task.id, result)
      
      // Report success
      this.reportProgress({
        type: 'task_complete',
        task: task.name,
        actualTime: Date.now() - task.startedAt!.getTime()
      })
      
    } catch (error) {
      // Task failed
      task.status = 'failed'
      task.error = error as Error
      
      // Report failure
      this.reportProgress({
        type: 'task_failed',
        task: task.name,
        error: error.message
      })
      
      // Attempt recovery
      if (task.retryable && task.retryCount < task.maxRetries) {
        await this.retryTask(task)
      } else {
        throw error  // Propagate failure
      }
    } finally {
      this.plan.currentTask = undefined
      await this.stateManager.savePlan(this.plan)
    }
  }
  
  /**
   * Execute multiple tasks in parallel
   */
  private async executeTasksInParallel(tasks: Task[]): Promise<void> {
    this.reportProgress({
      type: 'parallel_start',
      tasks: tasks.map(t => t.name)
    })
    
    const promises = tasks.map(task => this.executeTask(task))
    
    try {
      await Promise.all(promises)
      
      this.reportProgress({
        type: 'parallel_complete',
        tasks: tasks.map(t => t.name)
      })
    } catch (error) {
      // At least one task failed
      throw error
    }
  }
  
  /**
   * Retry a failed task
   */
  private async retryTask(task: Task): Promise<void> {
    task.retryCount++
    
    this.reportProgress({
      type: 'task_retry',
      task: task.name,
      attempt: task.retryCount,
      maxRetries: task.maxRetries
    })
    
    // Exponential backoff
    const backoff = Math.pow(2, task.retryCount) * 1000
    await this.sleep(backoff)
    
    // Retry
    await this.executeTask(task)
  }
  
  /**
   * Identify tasks that can run in parallel
   */
  private identifyParallelBatches(tasks: Task[]): Task[][] {
    const batches: Task[][] = []
    const remaining = new Set(tasks.map(t => t.id))
    const completed = new Set(this.plan.completedTasks)
    
    while (remaining.size > 0) {
      // Find tasks whose dependencies are satisfied
      const ready = tasks.filter(t => 
        remaining.has(t.id) &&
        t.dependencies.every(depId => completed.has(depId))
      )
      
      if (ready.length === 0) {
        throw new Error('No tasks ready (circular dependency?)')
      }
      
      batches.push(ready)
      
      // Mark as completed for next batch
      ready.forEach(t => {
        remaining.delete(t.id)
        completed.add(t.id)
      })
    }
    
    return batches
  }
  
  /**
   * Handle checkpoint (pause for user approval)
   */
  private async handleCheckpoint(checkpoint: Checkpoint): Promise<boolean> {
    this.reportProgress({
      type: 'checkpoint',
      name: checkpoint.name,
      description: checkpoint.description,
      questions: checkpoint.questions
    })
    
    // Wait for user response
    const response = await this.waitForUserApproval(checkpoint)
    
    checkpoint.approved = response.approved
    checkpoint.userFeedback = response.feedback
    
    return response.approved
  }
  
  /**
   * Report progress to user
   */
  private reportProgress(progress: ProgressUpdate): void {
    // Emit progress event (user interface listens to this)
    this.emit('progress', progress)
    
    // Log to console
    console.log(this.formatProgressUpdate(progress))
  }
  
  /**
   * Format progress update for display
   */
  private formatProgressUpdate(progress: ProgressUpdate): string {
    const timestamp = new Date().toISOString().split('T')[1].split('.')[0]
    
    switch (progress.type) {
      case 'phase_start':
        return `[${timestamp}] 🚀 Starting Phase: ${progress.phase} (${progress.tasksInPhase} tasks)`
      
      case 'phase_complete':
        return `[${timestamp}] ✅ Completed Phase: ${progress.phase}`
      
      case 'task_start':
        return `[${timestamp}] ⏳ Executing: ${progress.task} (est. ${this.formatDuration(progress.estimatedTime)})`
      
      case 'task_complete':
        return `[${timestamp}] ✓ Completed: ${progress.task} (${this.formatDuration(progress.actualTime)})`
      
      case 'task_failed':
        return `[${timestamp}] ✗ Failed: ${progress.task}\n    Error: ${progress.error}`
      
      case 'task_retry':
        return `[${timestamp}] 🔄 Retrying: ${progress.task} (attempt ${progress.attempt}/${progress.maxRetries})`
      
      case 'checkpoint':
        return `[${timestamp}] ⏸️  Checkpoint: ${progress.name}\n    ${progress.description}`
      
      case 'parallel_start':
        return `[${timestamp}] ⚡ Parallel Execution: ${progress.tasks.length} tasks`
      
      case 'parallel_complete':
        return `[${timestamp}] ✅ Parallel Execution Complete`
      
      default:
        return `[${timestamp}] ${progress.type}`
    }
  }
  
  private sleep(ms: number): Promise<void> {
    return new Promise(resolve => setTimeout(resolve, ms))
  }
  
  private formatDuration(ms: number): string {
    if (ms < 60000) return `${Math.ceil(ms / 1000)}s`
    const minutes = Math.floor(ms / 60000)
    const seconds = Math.ceil((ms % 60000) / 1000)
    return seconds > 0 ? `${minutes}m ${seconds}s` : `${minutes}m`
  }
}
```

---

## 🔀 PARALLEL EXECUTION

### Identifying Parallel Opportunities
```typescript
/**
 * Parallel Execution Analyzer
 * Identifies tasks that can run concurrently
 */
class ParallelExecutionAnalyzer {
  /**
   * Analyze which tasks can run in parallel
   */
  analyzeParallelism(tasks: Task[]): ParallelAnalysis {
    // Build dependency graph
    const graph = this.buildGraph(tasks)
    
    // Find independent task groups
    const independentGroups = this.findIndependentGroups(graph)
    
    // Calculate potential speedup
    const sequentialTime = tasks.reduce((sum, t) => sum + t.estimatedTime, 0)
    const parallelTime = this.calculateParallelTime(tasks, graph)
    const speedup = sequentialTime / parallelTime
    
    return {
      canParallelize: independentGroups.length > 1,
      independentGroups,
      sequentialTime,
      parallelTime,
      speedup,
      recommendation: this.makeRecommendation(speedup)
    }
  }
  
  /**
   * Find groups of tasks with no dependencies between them
   */
  private findIndependentGroups(graph: DependencyGraph): Task[][] {
    const groups: Task[][] = []
    const assigned = new Set<string>()
    
    // Use graph coloring to find independent sets
    for (const [nodeId, node] of graph.nodes) {
      if (assigned.has(nodeId)) continue
      
      const group: Task[] = [node.task]
      assigned.add(nodeId)
      
      // Find other tasks independent of this one
      for (const [otherNodeId, otherNode] of graph.nodes) {
        if (assigned.has(otherNodeId)) continue
        
        // Check if independent (no path between them)
        if (!this.hasPath(graph, nodeId, otherNodeId) &&
            !this.hasPath(graph, otherNodeId, nodeId)) {
          group.push(otherNode.task)
          assigned.add(otherNodeId)
        }
      }
      
      groups.push(group)
    }
    
    return groups
  }
  
  /**
   * Check if path exists from source to target
   */
  private hasPath(
    graph: DependencyGraph,
    source: string,
    target: string
  ): boolean {
    const visited = new Set<string>()
    const queue = [source]
    
    while (queue.length > 0) {
      const current = queue.shift()!
      if (current === target) return true
      if (visited.has(current)) continue
      
      visited.add(current)
      
      // Add neighbors
      const neighbors = graph.edges
        .filter(e => e.from === current)
        .map(e => e.to)
      
      queue.push(...neighbors)
    }
    
    return false
  }
  
  /**
   * Calculate total time with parallel execution
   */
  private calculateParallelTime(
    tasks: Task[],
    graph: DependencyGraph
  ): number {
    // Use level-by-level BFS to calculate parallel time
    const levels: Task[][] = []
    const taskLevels = new Map<string, number>()
    
    // Assign level to each task (0 = no dependencies)
    const assignLevel = (taskId: string): number => {
      if (taskLevels.has(taskId)) {
        return taskLevels.get(taskId)!
      }
      
      const task = tasks.find(t => t.id === taskId)!
      
      if (task.dependencies.length === 0) {
        taskLevels.set(taskId, 0)
        return 0
      }
      
      const depLevels = task.dependencies.map(depId => assignLevel(depId))
      const level = Math.max(...depLevels) + 1
      taskLevels.set(taskId, level)
      return level
    }
    
    // Assign levels to all tasks
    tasks.forEach(t => assignLevel(t.id))
    
    // Group tasks by level
    const maxLevel = Math.max(...taskLevels.values())
    for (let i = 0; i <= maxLevel; i++) {
      const levelTasks = tasks.filter(t => taskLevels.get(t.id) === i)
      levels.push(levelTasks)
    }
    
    // Calculate time (max time per level)
    let totalTime = 0
    for (const level of levels) {
      const levelTime = Math.max(...level.map(t => t.estimatedTime))
      totalTime += levelTime
    }
    
    return totalTime
  }
  
  /**
   * Make recommendation based on speedup
   */
  private makeRecommendation(speedup: number): string {
    if (speedup < 1.2) {
      return 'Sequential execution recommended (minimal parallelism benefit)'
    } else if (speedup < 2) {
      return 'Moderate parallelism possible (20-50% speedup)'
    } else if (speedup < 3) {
      return 'Good parallelism opportunity (2-3x speedup)'
    } else {
      return 'Excellent parallelism opportunity (3x+ speedup)'
    }
  }
}
```

**Example Parallel Execution:**
```
SEQUENTIAL EXECUTION (Total: 12 minutes):
Task 1: Create types (1m)      →
Task 2: Update schema (2m)     →
Task 3: API route 1 (3m)       →
Task 4: API route 2 (3m)       →
Task 5: UI component 1 (2m)    →
Task 6: UI component 2 (1m)    →

PARALLEL EXECUTION (Total: 6 minutes):
Level 0: Task 1 (1m)                    }
Level 1: Task 2 (2m)                    } Sequential
Level 2: Task 3 (3m) | Task 4 (3m)      } ← Parallel (both 3m, run simultaneously)
Level 3: Task 5 (2m) | Task 6 (1m)      } ← Parallel (max 2m)

Speedup: 12m / 6m = 2x faster
```

---

## 🏁 CHECKPOINT SYSTEM

### Checkpoint Implementation
```typescript
/**
 * Checkpoint Manager
 * Handles execution pauses for user approval
 */
class CheckpointManager {
  /**
   * Wait for user approval at checkpoint
   */
  async waitForApproval(checkpoint: Checkpoint): Promise<CheckpointResponse> {
    // Present checkpoint to user
    console.log(`
╔═══════════════════════════════════════════════════════════╗
║                      CHECKPOINT                            ║
╚═══════════════════════════════════════════════════════════╝

${checkpoint.name}

${checkpoint.description}
`)
    
    // Ask questions (if any)
    if (checkpoint.questions && checkpoint.questions.length > 0) {
      console.log('\nQuestions:')
      checkpoint.questions.forEach((q, i) => {
        console.log(`  ${i + 1}. ${q}`)
      })
    }
    
    console.log(`
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

Continue execution? (Y/n/m)

  Y/yes    - Continue
  n/no     - Cancel execution
  m/modify - Modify remaining tasks
  r/review - Review completed tasks

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
`)
    
    // Wait for user input
    const response = await this.getUserInput()
    
    return this.processResponse(response, checkpoint)
  }
  
  /**
   * Process user response
   */
  private processResponse(
    input: string,
    checkpoint: Checkpoint
  ): CheckpointResponse {
    const normalized = input.trim().toLowerCase()
    
    switch (normalized) {
      case 'y':
      case 'yes':
        return {
          action: 'continue',
          approved: true,
          feedback: 'User approved continuation'
        }
      
      case 'n':
      case 'no':
        return {
          action: 'cancel',
          approved: false,
          feedback: 'User cancelled execution'
        }
      
      case 'm':
      case 'modify':
        return {
          action: 'modify',
          approved: false,
          feedback: 'User requested modifications'
        }
      
      case 'r':
      case 'review':
        return {
          action: 'review',
          approved: false,
          feedback: 'User requested review'
        }
      
      default:
        console.log('Invalid input. Please enter Y, n, m, or r.')
        return this.processResponse(
          this.getUserInput(),
          checkpoint
        )
    }
  }
  
  /**
   * Get user input (implementation depends on environment)
   */
  private async getUserInput(): Promise<string> {
    // In CLI: use readline
    // In web: use prompt or modal
    // In API: use webhook or callback
    
    // Placeholder implementation
    return new Promise((resolve) => {
      // This would be replaced with actual input mechanism
      resolve('y')
    })
  }
}
```

---

## 📊 EXECUTION METRICS

### Tracking Execution Performance
```typescript
/**
 * Execution Metrics Tracker
 */
class ExecutionMetricsTracker {
  private metrics: ExecutionMetrics
  
  constructor() {
    this.metrics = {
      totalExecutions: 0,
      successfulExecutions: 0,
      failedExecutions: 0,
      cancelledExecutions: 0,
      averageExecutionTime: 0,
      averageTasksPerExecution: 0,
      checkpointApprovalRate: 0,
      errorRecoveryRate: 0,
      parallelizationEfficiency: 0
    }
  }
  
  /**
   * Record execution completion
   */
  recordExecution(plan: ExecutionPlan, result: ExecutionResult): void {
    this.metrics.totalExecutions++
    
    if (result.success) {
      this.metrics.successfulExecutions++
    } else if (plan.status === 'cancelled') {
      this.metrics.cancelledExecutions++
    } else {
      this.metrics.failedExecutions++
    }
    
    // Update averages
    this.updateAverages(plan, result)
  }
  
  /**
   * Get current metrics
   */
  getMetrics(): ExecutionMetrics {
    return { ...this.metrics }
  }
  
  /**
   * Get success rate
   */
  getSuccessRate(): number {
    if (this.metrics.totalExecutions === 0) return 0
    return this.metrics.successfulExecutions / this.metrics.totalExecutions
  }
  
  private updateAverages(plan: ExecutionPlan, result: ExecutionResult): void {
    // Update average execution time
    const totalTime = plan.tasks
      .filter(t => t.completedAt && t.startedAt)
      .reduce((sum, t) => {
        return sum + (t.completedAt!.getTime() - t.startedAt!.getTime())
      }, 0)
    
    this.metrics.averageExecutionTime = 
      (this.metrics.averageExecutionTime * (this.metrics.totalExecutions - 1) + totalTime) /
      this.metrics.totalExecutions
    
    // Update average tasks per execution
    this.metrics.averageTasksPerExecution =
      (this.metrics.averageTasksPerExecution * (this.metrics.totalExecutions - 1) + plan.totalTasks) /
      this.metrics.totalExecutions
  }
}
```

---

## 📚 RELATED ARTICLES

| Article | Purpose | Relationship to Task Orchestration |
|---------|---------|-----------------------------------|
| **Article II: State Management** | State persistence | Orchestrator uses state manager to save progress |
| **Article III: Decision Engine** | Intelligent decisions | Orchestrator uses decision engine for tool selection |
| **Segment 2, Article II** | Twelve Patterns | Orchestrator selects pattern for decomposition |
| **Segment 3, Article I** | Tool Architecture | Orchestrator calls tools during execution |

---

**Previous:** [← Segment 4 README](./README.md)  
**Next:** [Article II: State Management →](./02-article-ii-state-management.md)

---

**Last Updated:** February 7, 2026  
**Constitutional Version:** 2.0.0  
**Status:** ✅ Ratified and In Force

**Motto:** *"Complexity Tamed Through Systematic Decomposition and Orchestration"*
