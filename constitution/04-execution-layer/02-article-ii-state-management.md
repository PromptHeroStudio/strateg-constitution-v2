# 💾 ARTICLE II: STATE MANAGEMENT

**Maintaining Context and Progress Across Conversation Turns**

---

## 📜 CONSTITUTIONAL AUTHORITY

This article defines how MVCA maintains execution state, manages conversation memory, and ensures continuity across sessions.

**Legal Force:**
- ✅ Execution state **MUST** be persisted (no data loss on interruption)
- ✅ Conversation context **SHALL** be retained across turns (up to 90%)
- ✅ State recovery **SHALL** be automatic after failures
- ✅ Users **SHALL** be able to resume execution from checkpoints
- ✅ State **SHALL** be accessible for debugging and auditing

**Constitutional Principle:**
> State is sacred - MVCA never forgets, never loses progress.  
> Interruptions are inconveniences, not catastrophes.

---

## 🧠 CONVERSATION MEMORY

### What is Conversation Memory?

**Definition:** The system's ability to remember previous turns in the conversation and use that information to inform current decisions.

**Memory Types:**
```
┌─────────────────────────────────────────────────────────┐
│ CONVERSATION MEMORY HIERARCHY                            │
├─────────────────────────────────────────────────────────┤
│                                                          │
│  TIER 1: WORKING MEMORY (Current Turn)                 │
│  ├─ Current user message                                │
│  ├─ Immediately preceding turn                          │
│  └─ Active execution context                            │
│  Retention: 100%                                         │
│                                                          │
│  TIER 2: SHORT-TERM MEMORY (Last 5-10 turns)           │
│  ├─ Recent conversation history                         │
│  ├─ Active execution plan                               │
│  └─ Pending tasks                                       │
│  Retention: 90-100%                                      │
│                                                          │
│  TIER 3: MEDIUM-TERM MEMORY (Last 20-50 turns)         │
│  ├─ Summarized conversation history                     │
│  ├─ Completed execution plans                           │
│  └─ Key decisions and outcomes                          │
│  Retention: 70-90% (compressed)                         │
│                                                          │
│  TIER 4: LONG-TERM MEMORY (Entire session)             │
│  ├─ Session-level context (project, tech stack)        │
│  ├─ User preferences and patterns                       │
│  └─ Historical execution statistics                     │
│  Retention: 50-70% (highly compressed)                  │
│                                                          │
└─────────────────────────────────────────────────────────┘
```

---

### Conversation Memory Implementation
```typescript
/**
 * Conversation Memory Manager
 * Maintains context across conversation turns
 */
class ConversationMemoryManager {
  private conversationId: string
  private turns: ConversationTurn[] = []
  private compressionThreshold = 10  // Compress after 10 turns
  
  /**
   * Add new conversation turn
   */
  async addTurn(turn: ConversationTurn): Promise<void> {
    this.turns.push(turn)
    
    // Check if compression needed
    if (this.turns.length >= this.compressionThreshold) {
      await this.compressOldTurns()
    }
    
    // Persist to storage
    await this.persist()
  }
  
  /**
   * Get conversation context for current turn
   */
  getContext(maxTokens: number = 100000): ConversationContext {
    const context: ConversationContext = {
      conversationId: this.conversationId,
      currentTurn: this.turns.length,
      workingMemory: this.getWorkingMemory(),
      shortTermMemory: this.getShortTermMemory(),
      mediumTermMemory: this.getMediumTermMemory(),
      longTermMemory: this.getLongTermMemory(),
      totalTokens: 0
    }
    
    // Calculate token usage
    context.totalTokens = this.estimateTokens(context)
    
    // If over limit, compress further
    if (context.totalTokens > maxTokens) {
      context = this.compressContext(context, maxTokens)
    }
    
    return context
  }
  
  /**
   * Get working memory (Tier 1)
   */
  private getWorkingMemory(): WorkingMemory {
    const recentTurns = this.turns.slice(-2)  // Last 2 turns
    
    return {
      currentMessage: recentTurns[recentTurns.length - 1]?.userMessage,
      previousResponse: recentTurns[0]?.assistantResponse,
      activeExecutionPlan: this.getActiveExecutionPlan(),
      currentTask: this.getCurrentTask()
    }
  }
  
  /**
   * Get short-term memory (Tier 2)
   */
  private getShortTermMemory(): ShortTermMemory {
    const recentTurns = this.turns.slice(-10)  // Last 10 turns
    
    return {
      conversationHistory: recentTurns.map(t => ({
        turn: t.turnNumber,
        userMessage: t.userMessage,
        assistantResponse: this.summarize(t.assistantResponse, 200),  // 200 chars
        timestamp: t.timestamp
      })),
      executionPlans: this.getRecentExecutionPlans(5),
      pendingTasks: this.getPendingTasks(),
      completedTasks: this.getRecentCompletedTasks(10)
    }
  }
  
  /**
   * Get medium-term memory (Tier 3)
   */
  private getMediumTermMemory(): MediumTermMemory {
    const olderTurns = this.turns.slice(-50, -10)  // Turns 10-50 ago
    
    return {
      conversationSummary: this.summarizeConversation(olderTurns),
      completedExecutionPlans: this.getCompletedExecutionPlans(10),
      keyDecisions: this.extractKeyDecisions(olderTurns),
      learningPoints: this.extractLearningPoints(olderTurns)
    }
  }
  
  /**
   * Get long-term memory (Tier 4)
   */
  private getLongTermMemory(): LongTermMemory {
    return {
      projectContext: this.getProjectContext(),
      userPreferences: this.getUserPreferences(),
      executionStatistics: this.getExecutionStatistics(),
      errorPatterns: this.getCommonErrorPatterns(),
      successPatterns: this.getSuccessPatterns()
    }
  }
  
  /**
   * Compress old conversation turns
   */
  private async compressOldTurns(): Promise<void> {
    // Keep last 10 turns uncompressed
    const toCompress = this.turns.slice(0, -10)
    const uncompressed = this.turns.slice(-10)
    
    // Create compressed summary
    const summary = this.summarizeConversation(toCompress)
    
    // Store summary in long-term memory
    await this.storeLongTermSummary(summary)
    
    // Keep only uncompressed turns in memory
    this.turns = uncompressed
  }
  
  /**
   * Summarize conversation turns
   */
  private summarizeConversation(turns: ConversationTurn[]): string {
    // Extract key information
    const keyPoints: string[] = []
    
    for (const turn of turns) {
      // Extract user intents
      if (turn.userIntent) {
        keyPoints.push(`User requested: ${turn.userIntent}`)
      }
      
      // Extract outcomes
      if (turn.outcome) {
        keyPoints.push(`Outcome: ${turn.outcome}`)
      }
      
      // Extract errors/issues
      if (turn.errors && turn.errors.length > 0) {
        keyPoints.push(`Issues: ${turn.errors.map(e => e.message).join(', ')}`)
      }
    }
    
    return keyPoints.join('\n')
  }
  
  /**
   * Compress context to fit token limit
   */
  private compressContext(
    context: ConversationContext,
    maxTokens: number
  ): ConversationContext {
    let currentTokens = context.totalTokens
    
    // Compression strategy (in order of priority):
    // 1. Remove oldest medium-term memory
    // 2. Further compress short-term memory
    // 3. Summarize long-term memory
    
    // Step 1: Compress medium-term
    if (currentTokens > maxTokens) {
      context.mediumTermMemory = this.compressMediumTerm(
        context.mediumTermMemory,
        0.5  // 50% compression
      )
      currentTokens = this.estimateTokens(context)
    }
    
    // Step 2: Compress short-term
    if (currentTokens > maxTokens) {
      context.shortTermMemory = this.compressShortTerm(
        context.shortTermMemory,
        0.7  // 70% of original
      )
      currentTokens = this.estimateTokens(context)
    }
    
    // Step 3: Compress long-term
    if (currentTokens > maxTokens) {
      context.longTermMemory = this.compressLongTerm(
        context.longTermMemory,
        0.5  // 50% compression
      )
    }
    
    context.totalTokens = this.estimateTokens(context)
    return context
  }
  
  /**
   * Estimate token count (rough approximation)
   */
  private estimateTokens(context: ConversationContext): number {
    const text = JSON.stringify(context)
    return Math.ceil(text.length / 4)  // Rough estimate: 4 chars = 1 token
  }
  
  /**
   * Summarize text to target length
   */
  private summarize(text: string, maxLength: number): string {
    if (text.length <= maxLength) return text
    
    // Simple truncation with ellipsis
    return text.slice(0, maxLength - 3) + '...'
  }
  
  /**
   * Persist conversation to storage
   */
  private async persist(): Promise<void> {
    await storage.set(`conversation:${this.conversationId}`, {
      turns: this.turns,
      metadata: {
        lastUpdate: new Date(),
        turnCount: this.turns.length
      }
    })
  }
  
  /**
   * Load conversation from storage
   */
  static async load(conversationId: string): Promise<ConversationMemoryManager> {
    const data = await storage.get(`conversation:${conversationId}`)
    
    const manager = new ConversationMemoryManager()
    manager.conversationId = conversationId
    
    if (data) {
      manager.turns = data.turns
    }
    
    return manager
  }
}
```

---

### Conversation Turn Structure
```typescript
/**
 * Conversation Turn
 * Represents a single exchange in the conversation
 */
interface ConversationTurn {
  // IDENTITY
  turnNumber: number
  conversationId: string
  timestamp: Date
  
  // CONTENT
  userMessage: string
  assistantResponse: string
  
  // METADATA
  userIntent?: string                // Classified intent (build, debug, deploy, etc.)
  outcome?: string                   // Result summary
  
  // EXECUTION
  executionPlanId?: string          // If execution plan created
  tasksExecuted?: string[]          // Task IDs executed this turn
  
  // ERRORS
  errors?: Error[]                  // Any errors encountered
  
  // CONTEXT
  contextUsed: {
    projectContext?: ProjectContext
    previousTasks?: string[]
    relevantFiles?: string[]
  }
  
  // TOKENS
  inputTokens: number
  outputTokens: number
  totalTokens: number
}

/**
 * Conversation Context
 * Complete context for current turn
 */
interface ConversationContext {
  conversationId: string
  currentTurn: number
  
  // MEMORY TIERS
  workingMemory: WorkingMemory
  shortTermMemory: ShortTermMemory
  mediumTermMemory: MediumTermMemory
  longTermMemory: LongTermMemory
  
  // METRICS
  totalTokens: number
}

/**
 * Working Memory (Tier 1)
 */
interface WorkingMemory {
  currentMessage: string
  previousResponse?: string
  activeExecutionPlan?: ExecutionPlan
  currentTask?: Task
}

/**
 * Short-Term Memory (Tier 2)
 */
interface ShortTermMemory {
  conversationHistory: Array<{
    turn: number
    userMessage: string
    assistantResponse: string
    timestamp: Date
  }>
  executionPlans: ExecutionPlan[]
  pendingTasks: Task[]
  completedTasks: Array<{
    taskId: string
    name: string
    result: TaskResult
    completedAt: Date
  }>
}

/**
 * Medium-Term Memory (Tier 3)
 */
interface MediumTermMemory {
  conversationSummary: string
  completedExecutionPlans: Array<{
    planId: string
    request: string
    outcome: string
    tasksCompleted: number
  }>
  keyDecisions: Array<{
    decision: string
    rationale: string
    turn: number
  }>
  learningPoints: Array<{
    lesson: string
    context: string
  }>
}

/**
 * Long-Term Memory (Tier 4)
 */
interface LongTermMemory {
  projectContext: ProjectContext
  userPreferences: UserPreferences
  executionStatistics: {
    totalExecutions: number
    successRate: number
    averageExecutionTime: number
    commonPatterns: string[]
  }
  errorPatterns: Array<{
    error: string
    frequency: number
    commonContext: string
  }>
  successPatterns: Array<{
    pattern: string
    frequency: number
    avgSuccessRate: number
  }>
}

/**
 * Project Context
 */
interface ProjectContext {
  name: string
  type: 'web' | 'mobile' | 'api' | 'cli' | 'desktop'
  framework: string
  language: string
  database?: string
  techStack: string[]
  folderStructure?: string
  conventions?: {
    naming: string
    imports: string
    fileOrganization: string
  }
}
```

---

## 💾 EXECUTION STATE TRACKING

### Execution State Structure
```typescript
/**
 * Execution State
 * Complete state of an execution plan
 */
interface ExecutionState {
  // IDENTITY
  planId: string
  conversationId: string
  createdAt: Date
  updatedAt: Date
  
  // PLAN
  plan: ExecutionPlan
  
  // CURRENT STATUS
  status: 'pending' | 'approved' | 'executing' | 'paused' | 'completed' | 'failed' | 'cancelled'
  currentPhase?: number
  currentTask?: string
  
  // PROGRESS
  tasksCompleted: number
  tasksTotal: number
  percentComplete: number
  
  // RESULTS
  taskResults: Map<string, TaskResult>
  
  // CHECKPOINTS
  lastCheckpoint?: {
    checkpointId: string
    timestamp: Date
    approved: boolean
  }
  
  // TIMING
  startedAt?: Date
  completedAt?: Date
  totalExecutionTime?: number
  
  // ERRORS
  errors: Array<{
    taskId: string
    error: Error
    timestamp: Date
    recovered: boolean
  }>
  
  // RECOVERY
  recoveryAttempts: number
  canResume: boolean
  resumeFromTask?: string
}
```

---

### State Persistence Manager
```typescript
/**
 * State Persistence Manager
 * Saves and loads execution state
 */
class StatePersistenceManager {
  private storage: Storage
  
  constructor(storage: Storage) {
    this.storage = storage
  }
  
  /**
   * Save execution state
   */
  async saveState(state: ExecutionState): Promise<void> {
    const key = `execution:${state.planId}`
    
    // Update timestamp
    state.updatedAt = new Date()
    
    // Serialize state
    const serialized = this.serializeState(state)
    
    // Save to storage
    await this.storage.set(key, serialized)
    
    // Also save to conversation context
    await this.linkToConversation(state)
    
    // Create checkpoint snapshot
    if (state.lastCheckpoint) {
      await this.createCheckpointSnapshot(state)
    }
  }
  
  /**
   * Load execution state
   */
  async loadState(planId: string): Promise<ExecutionState | null> {
    const key = `execution:${planId}`
    
    const data = await this.storage.get(key)
    if (!data) return null
    
    return this.deserializeState(data)
  }
  
  /**
   * Resume execution from saved state
   */
  async resumeExecution(planId: string): Promise<ExecutionState | null> {
    const state = await this.loadState(planId)
    if (!state) return null
    
    // Validate can resume
    if (!state.canResume) {
      throw new Error('Execution cannot be resumed from current state')
    }
    
    // Find resume point
    if (state.resumeFromTask) {
      const task = state.plan.tasks.find(t => t.id === state.resumeFromTask)
      if (task) {
        task.status = 'pending'  // Reset task to pending
      }
    }
    
    // Update status
    state.status = 'executing'
    state.updatedAt = new Date()
    
    await this.saveState(state)
    
    return state
  }
  
  /**
   * Create checkpoint snapshot
   */
  private async createCheckpointSnapshot(state: ExecutionState): Promise<void> {
    const snapshotKey = `checkpoint:${state.planId}:${state.lastCheckpoint!.checkpointId}`
    
    // Save full state snapshot at checkpoint
    await this.storage.set(snapshotKey, this.serializeState(state))
  }
  
  /**
   * Rollback to checkpoint
   */
  async rollbackToCheckpoint(
    planId: string,
    checkpointId: string
  ): Promise<ExecutionState> {
    const snapshotKey = `checkpoint:${planId}:${checkpointId}`
    
    const snapshot = await this.storage.get(snapshotKey)
    if (!snapshot) {
      throw new Error(`Checkpoint ${checkpointId} not found`)
    }
    
    const state = this.deserializeState(snapshot)
    
    // Reset status
    state.status = 'paused'
    state.updatedAt = new Date()
    
    // Mark all tasks after checkpoint as pending
    const checkpointTask = state.lastCheckpoint!.checkpointId
    let foundCheckpoint = false
    
    for (const task of state.plan.tasks) {
      if (foundCheckpoint) {
        task.status = 'pending'
        task.startedAt = undefined
        task.completedAt = undefined
        task.error = undefined
      }
      
      if (task.id === checkpointTask) {
        foundCheckpoint = true
      }
    }
    
    await this.saveState(state)
    
    return state
  }
  
  /**
   * Link state to conversation
   */
  private async linkToConversation(state: ExecutionState): Promise<void> {
    const conversationKey = `conversation:${state.conversationId}:executions`
    
    let executions = await this.storage.get(conversationKey) || []
    
    // Add or update execution reference
    const existing = executions.findIndex((e: any) => e.planId === state.planId)
    
    const reference = {
      planId: state.planId,
      status: state.status,
      progress: state.percentComplete,
      updatedAt: state.updatedAt
    }
    
    if (existing >= 0) {
      executions[existing] = reference
    } else {
      executions.push(reference)
    }
    
    await this.storage.set(conversationKey, executions)
  }
  
  /**
   * Serialize state for storage
   */
  private serializeState(state: ExecutionState): any {
    return {
      ...state,
      taskResults: Array.from(state.taskResults.entries())
    }
  }
  
  /**
   * Deserialize state from storage
   */
  private deserializeState(data: any): ExecutionState {
    return {
      ...data,
      taskResults: new Map(data.taskResults),
      createdAt: new Date(data.createdAt),
      updatedAt: new Date(data.updatedAt),
      startedAt: data.startedAt ? new Date(data.startedAt) : undefined,
      completedAt: data.completedAt ? new Date(data.completedAt) : undefined
    }
  }
  
  /**
   * Clean up old execution states
   */
  async cleanup(olderThan: Date): Promise<number> {
    const allKeys = await this.storage.keys('execution:*')
    let cleaned = 0
    
    for (const key of allKeys) {
      const state = await this.storage.get(key)
      
      if (state && new Date(state.updatedAt) < olderThan) {
        // Only delete if completed, failed, or cancelled
        if (['completed', 'failed', 'cancelled'].includes(state.status)) {
          await this.storage.delete(key)
          cleaned++
        }
      }
    }
    
    return cleaned
  }
}
```

---

## 🔄 CONTEXT WINDOW MANAGEMENT

### Managing Context Window Limits
```typescript
/**
 * Context Window Manager
 * Ensures context fits within model limits
 */
class ContextWindowManager {
  private maxTokens: number
  private reservedTokens: number  // Reserved for response
  
  constructor(maxTokens: number = 200000, reservedTokens: number = 4096) {
    this.maxTokens = maxTokens
    this.reservedTokens = reservedTokens
  }
  
  /**
   * Get available tokens for context
   */
  getAvailableTokens(): number {
    return this.maxTokens - this.reservedTokens
  }
  
  /**
   * Fit context within window
   */
  fitContext(context: ConversationContext): ConversationContext {
    const available = this.getAvailableTokens()
    
    if (context.totalTokens <= available) {
      return context  // Fits already
    }
    
    // Apply compression strategy
    return this.compressToFit(context, available)
  }
  
  /**
   * Compress context to fit available tokens
   */
  private compressToFit(
    context: ConversationContext,
    targetTokens: number
  ): ConversationContext {
    let compressed = { ...context }
    let currentTokens = context.totalTokens
    
    // Compression stages (increasingly aggressive)
    const stages = [
      // Stage 1: Drop medium-term memory (keep 50%)
      () => {
        compressed.mediumTermMemory = this.compressMediumTerm(
          compressed.mediumTermMemory,
          0.5
        )
      },
      
      // Stage 2: Compress short-term memory (keep 70%)
      () => {
        compressed.shortTermMemory = this.compressShortTerm(
          compressed.shortTermMemory,
          0.7
        )
      },
      
      // Stage 3: Further compress medium-term (keep 30%)
      () => {
        compressed.mediumTermMemory = this.compressMediumTerm(
          compressed.mediumTermMemory,
          0.3
        )
      },
      
      // Stage 4: More aggressive short-term compression (keep 50%)
      () => {
        compressed.shortTermMemory = this.compressShortTerm(
          compressed.shortTermMemory,
          0.5
        )
      },
      
      // Stage 5: Drop medium-term entirely
      () => {
        compressed.mediumTermMemory = {
          conversationSummary: 'Context window exceeded - medium-term memory dropped',
          completedExecutionPlans: [],
          keyDecisions: [],
          learningPoints: []
        }
      },
      
      // Stage 6: Compress long-term (keep 50%)
      () => {
        compressed.longTermMemory = this.compressLongTerm(
          compressed.longTermMemory,
          0.5
        )
      }
    ]
    
    // Apply stages until fits
    for (const stage of stages) {
      stage()
      currentTokens = this.estimateTokens(compressed)
      
      if (currentTokens <= targetTokens) {
        break
      }
    }
    
    compressed.totalTokens = currentTokens
    
    // Log compression metrics
    console.log(`Context compressed: ${context.totalTokens} → ${currentTokens} tokens (${Math.round((currentTokens / context.totalTokens) * 100)}%)`)
    
    return compressed
  }
  
  /**
   * Compress short-term memory
   */
  private compressShortTerm(
    memory: ShortTermMemory,
    retainRatio: number
  ): ShortTermMemory {
    const keep = Math.ceil(memory.conversationHistory.length * retainRatio)
    
    return {
      conversationHistory: memory.conversationHistory.slice(-keep),
      executionPlans: memory.executionPlans.slice(-Math.ceil(memory.executionPlans.length * retainRatio)),
      pendingTasks: memory.pendingTasks,  // Keep all pending tasks
      completedTasks: memory.completedTasks.slice(-Math.ceil(memory.completedTasks.length * retainRatio))
    }
  }
  
  /**
   * Compress medium-term memory
   */
  private compressMediumTerm(
    memory: MediumTermMemory,
    retainRatio: number
  ): MediumTermMemory {
    return {
      conversationSummary: this.truncate(memory.conversationSummary, retainRatio),
      completedExecutionPlans: memory.completedExecutionPlans.slice(
        -Math.ceil(memory.completedExecutionPlans.length * retainRatio)
      ),
      keyDecisions: memory.keyDecisions.slice(
        -Math.ceil(memory.keyDecisions.length * retainRatio)
      ),
      learningPoints: memory.learningPoints.slice(
        -Math.ceil(memory.learningPoints.length * retainRatio)
      )
    }
  }
  
  /**
   * Compress long-term memory
   */
  private compressLongTerm(
    memory: LongTermMemory,
    retainRatio: number
  ): LongTermMemory {
    return {
      projectContext: memory.projectContext,  // Always keep project context
      userPreferences: memory.userPreferences,  // Always keep preferences
      executionStatistics: memory.executionStatistics,  // Keep stats
      errorPatterns: memory.errorPatterns.slice(
        -Math.ceil(memory.errorPatterns.length * retainRatio)
      ),
      successPatterns: memory.successPatterns.slice(
        -Math.ceil(memory.successPatterns.length * retainRatio)
      )
    }
  }
  
  /**
   * Truncate string to ratio of original length
   */
  private truncate(text: string, ratio: number): string {
    const targetLength = Math.floor(text.length * ratio)
    if (text.length <= targetLength) return text
    return text.slice(0, targetLength - 3) + '...'
  }
  
  /**
   * Estimate token count
   */
  private estimateTokens(context: ConversationContext): number {
    const text = JSON.stringify(context)
    return Math.ceil(text.length / 4)
  }
}
```

---

## 🔧 STATE RECOVERY

### Recovering from Failures
```typescript
/**
 * State Recovery Manager
 * Handles recovery from execution failures
 */
class StateRecoveryManager {
  private stateManager: StatePersistenceManager
  
  constructor(stateManager: StatePersistenceManager) {
    this.stateManager = stateManager
  }
  
  /**
   * Recover from execution failure
   */
  async recover(planId: string): Promise<RecoveryResult> {
    // Load current state
    const state = await this.stateManager.loadState(planId)
    if (!state) {
      return {
        success: false,
        reason: 'Execution state not found',
        recovered: false
      }
    }
    
    // Analyze failure
    const analysis = this.analyzeFailure(state)
    
    // Determine recovery strategy
    const strategy = this.selectRecoveryStrategy(analysis)
    
    // Execute recovery
    const result = await this.executeRecovery(state, strategy)
    
    return result
  }
  
  /**
   * Analyze failure to understand what went wrong
   */
  private analyzeFailure(state: ExecutionState): FailureAnalysis {
    const lastError = state.errors[state.errors.length - 1]
    
    return {
      failedTask: lastError?.taskId,
      errorType: this.classifyError(lastError?.error),
      recoverable: this.isRecoverable(lastError?.error),
      completedTasks: state.tasksCompleted,
      totalTasks: state.tasksTotal,
      percentComplete: state.percentComplete,
      lastCheckpoint: state.lastCheckpoint,
      canRollback: !!state.lastCheckpoint
    }
  }
  
  /**
   * Classify error type
   */
  private classifyError(error?: Error): ErrorType {
    if (!error) return 'unknown'
    
    const message = error.message.toLowerCase()
    
    if (message.includes('timeout')) return 'timeout'
    if (message.includes('network')) return 'network'
    if (message.includes('authentication')) return 'authentication'
    if (message.includes('rate limit')) return 'rate_limit'
    if (message.includes('permission')) return 'permission'
    if (message.includes('not found')) return 'not_found'
    
    return 'execution_error'
  }
  
  /**
   * Check if error is recoverable
   */
  private isRecoverable(error?: Error): boolean {
    if (!error) return false
    
    const recoverableErrors = [
      'timeout',
      'network',
      'rate_limit'
    ]
    
    const errorType = this.classifyError(error)
    return recoverableErrors.includes(errorType)
  }
  
  /**
   * Select recovery strategy
   */
  private selectRecoveryStrategy(analysis: FailureAnalysis): RecoveryStrategy {
    // Strategy 1: Retry failed task
    if (analysis.recoverable && analysis.completedTasks / analysis.totalTasks > 0.5) {
      return {
        type: 'retry_task',
        description: 'Retry failed task with exponential backoff',
        taskId: analysis.failedTask!,
        maxRetries: 3,
        backoffMs: 2000
      }
    }
    
    // Strategy 2: Rollback to checkpoint
    if (analysis.canRollback && analysis.percentComplete > 30) {
      return {
        type: 'rollback_checkpoint',
        description: 'Rollback to last checkpoint and retry from there',
        checkpointId: analysis.lastCheckpoint!.checkpointId
      }
    }
    
    // Strategy 3: Skip failed task and continue
    if (analysis.completedTasks / analysis.totalTasks > 0.7) {
      return {
        type: 'skip_task',
        description: 'Skip failed task and continue with remaining tasks',
        taskId: analysis.failedTask!
      }
    }
    
    // Strategy 4: Full restart (last resort)
    return {
      type: 'restart',
      description: 'Restart execution from beginning',
      preserveResults: true  // Keep results of completed tasks
    }
  }
  
  /**
   * Execute recovery strategy
   */
  private async executeRecovery(
    state: ExecutionState,
    strategy: RecoveryStrategy
  ): Promise<RecoveryResult> {
    switch (strategy.type) {
      case 'retry_task':
        return await this.retryTask(state, strategy)
      
      case 'rollback_checkpoint':
        return await this.rollbackCheckpoint(state, strategy)
      
      case 'skip_task':
        return await this.skipTask(state, strategy)
      
      case 'restart':
        return await this.restart(state, strategy)
      
      default:
        return {
          success: false,
          reason: 'Unknown recovery strategy',
          recovered: false
        }
    }
  }
  
  /**
   * Retry failed task
   */
  private async retryTask(
    state: ExecutionState,
    strategy: RecoveryStrategy & { type: 'retry_task' }
  ): Promise<RecoveryResult> {
    const task = state.plan.tasks.find(t => t.id === strategy.taskId)
    if (!task) {
      return {
        success: false,
        reason: 'Task not found',
        recovered: false
      }
    }
    
    // Reset task status
    task.status = 'pending'
    task.error = undefined
    
    // Update recovery attempts
    state.recoveryAttempts++
    state.resumeFromTask = task.id
    state.canResume = true
    
    await this.stateManager.saveState(state)
    
    return {
      success: true,
      reason: `Will retry task: ${task.name}`,
      recovered: true,
      resumeFromTask: task.id
    }
  }
  
  /**
   * Rollback to checkpoint
   */
  private async rollbackCheckpoint(
    state: ExecutionState,
    strategy: RecoveryStrategy & { type: 'rollback_checkpoint' }
  ): Promise<RecoveryResult> {
    try {
      const restoredState = await this.stateManager.rollbackToCheckpoint(
        state.planId,
        strategy.checkpointId
      )
      
      return {
        success: true,
        reason: `Rolled back to checkpoint: ${strategy.checkpointId}`,
        recovered: true,
        resumeFromTask: restoredState.resumeFromTask
      }
    } catch (error) {
      return {
        success: false,
        reason: `Rollback failed: ${error.message}`,
        recovered: false
      }
    }
  }
  
  /**
   * Skip failed task
   */
  private async skipTask(
    state: ExecutionState,
    strategy: RecoveryStrategy & { type: 'skip_task' }
  ): Promise<RecoveryResult> {
    const task = state.plan.tasks.find(t => t.id === strategy.taskId)
    if (!task) {
      return {
        success: false,
        reason: 'Task not found',
        recovered: false
      }
    }
    
    // Mark task as skipped
    task.status = 'skipped'
    
    // Find next task
    const nextTask = state.plan.tasks.find(t => 
      t.status === 'pending' &&
      t.dependencies.every(depId => {
        const dep = state.plan.tasks.find(d => d.id === depId)
        return dep?.status === 'completed' || dep?.status === 'skipped'
      })
    )
    
    if (!nextTask) {
      return {
        success: false,
        reason: 'No remaining tasks to execute',
        recovered: false
      }
    }
    
    state.resumeFromTask = nextTask.id
    state.canResume = true
    
    await this.stateManager.saveState(state)
    
    return {
      success: true,
      reason: `Skipped task: ${task.name}, will continue from: ${nextTask.name}`,
      recovered: true,
      resumeFromTask: nextTask.id
    }
  }
  
  /**
   * Restart execution
   */
  private async restart(
    state: ExecutionState,
    strategy: RecoveryStrategy & { type: 'restart' }
  ): Promise<RecoveryResult> {
    // Reset all tasks
    for (const task of state.plan.tasks) {
      if (strategy.preserveResults && task.status === 'completed') {
        // Keep completed tasks
        continue
      }
      
      task.status = 'pending'
      task.startedAt = undefined
      task.completedAt = undefined
      task.error = undefined
    }
    
    // Reset state
    state.status = 'pending'
    state.currentPhase = undefined
    state.currentTask = undefined
    state.errors = []
    state.canResume = true
    state.resumeFromTask = state.plan.tasks[0].id
    
    await this.stateManager.saveState(state)
    
    return {
      success: true,
      reason: 'Execution will restart from beginning',
      recovered: true,
      resumeFromTask: state.plan.tasks[0].id
    }
  }
}

/**
 * Recovery Strategy Types
 */
type ErrorType = 
  | 'timeout' 
  | 'network' 
  | 'authentication' 
  | 'rate_limit' 
  | 'permission' 
  | 'not_found' 
  | 'execution_error' 
  | 'unknown'

interface FailureAnalysis {
  failedTask?: string
  errorType: ErrorType
  recoverable: boolean
  completedTasks: number
  totalTasks: number
  percentComplete: number
  lastCheckpoint?: any
  canRollback: boolean
}

interface RecoveryStrategy {
  type: 'retry_task' | 'rollback_checkpoint' | 'skip_task' | 'restart'
  description: string
  [key: string]: any
}

interface RecoveryResult {
  success: boolean
  reason: string
  recovered: boolean
  resumeFromTask?: string
}
```

---

## 📊 STATE METRICS

### Tracking State Health
```typescript
/**
 * State Metrics Tracker
 */
class StateMetricsTracker {
  private metrics: StateMetrics = {
    totalStates: 0,
    activeStates: 0,
    completedStates: 0,
    failedStates: 0,
    averageStateSize: 0,
    averageRecoveryTime: 0,
    successfulRecoveries: 0,
    failedRecoveries: 0,
    contextCompressionRate: 0
  }
  
  /**
   * Record state save
   */
  recordStateSave(state: ExecutionState): void {
    this.metrics.totalStates++
    
    if (state.status === 'executing' || state.status === 'paused') {
      this.metrics.activeStates++
    } else if (state.status === 'completed') {
      this.metrics.completedStates++
    } else if (state.status === 'failed') {
      this.metrics.failedStates++
    }
    
    // Update average state size
    const stateSize = this.estimateSize(state)
    this.metrics.averageStateSize = 
      (this.metrics.averageStateSize * (this.metrics.totalStates - 1) + stateSize) /
      this.metrics.totalStates
  }
  
  /**
   * Record recovery attempt
   */
  recordRecovery(result: RecoveryResult, duration: number): void {
    if (result.recovered) {
      this.metrics.successfulRecoveries++
      
      // Update average recovery time
      const total = this.metrics.successfulRecoveries + this.metrics.failedRecoveries
      this.metrics.averageRecoveryTime =
        (this.metrics.averageRecoveryTime * (total - 1) + duration) / total
    } else {
      this.metrics.failedRecoveries++
    }
  }
  
  /**
   * Get metrics
   */
  getMetrics(): StateMetrics {
    return { ...this.metrics }
  }
  
  /**
   * Get recovery success rate
   */
  getRecoverySuccessRate(): number {
    const total = this.metrics.successfulRecoveries + this.metrics.failedRecoveries
    if (total === 0) return 0
    return this.metrics.successfulRecoveries / total
  }
  
  private estimateSize(state: ExecutionState): number {
    return JSON.stringify(state).length
  }
}

interface StateMetrics {
  totalStates: number
  activeStates: number
  completedStates: number
  failedStates: number
  averageStateSize: number
  averageRecoveryTime: number
  successfulRecoveries: number
  failedRecoveries: number
  contextCompressionRate: number
}
```

---

## 📚 RELATED ARTICLES

| Article | Purpose | Relationship to State Management |
|---------|---------|----------------------------------|
| **Article I: Task Orchestration** | Task decomposition | State manager saves orchestration progress |
| **Article III: Decision Engine** | Intelligent decisions | Uses conversation memory for context |
| **Segment 3, Article II** | MCP Integration | State persisted for MCP server calls |

---

**Previous:** [← Article I: Task Orchestration](./01-article-i-task-orchestration.md)  
**Next:** [Article III: Decision Engine →](./03-article-iii-decision-engine.md)

---

**Last Updated:** February 7, 2026  
**Constitutional Version:** 2.0.0  
**Status:** ✅ Ratified and In Force

**Motto:** *"State is Sacred - MVCA Never Forgets, Never Loses Progress"*
