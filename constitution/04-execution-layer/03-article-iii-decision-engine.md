# 🧠 ARTICLE III: DECISION ENGINE

**Intelligent Decision-Making for Tool Selection, Approach Selection, and Error Recovery**

---

## 📜 CONSTITUTIONAL AUTHORITY

This article defines how MVCA makes intelligent decisions about which tools to use, which approaches to take, and how to recover from errors.

**Legal Force:**
- ✅ Decision rationale **MUST** be transparent (users see why decisions were made)
- ✅ Confidence scores **SHALL** be provided for major decisions
- ✅ Alternative approaches **SHALL** be considered when confidence is low
- ✅ Users **SHALL** be able to override decisions
- ✅ Decision patterns **SHALL** be learned and improved over time

**Constitutional Principle:**
> Decisions are not arbitrary - they are informed, scored, and explainable.  
> When MVCA chooses, it knows why, and users know too.

---

## 🎯 DECISION TYPES

### Overview of Decisions
```
DECISION HIERARCHY
├─ STRATEGIC DECISIONS (High-level)
│  ├─ Which pattern to use? (Scaffolding vs Implementation)
│  ├─ Which approach to take? (Multiple valid solutions)
│  └─ Should execution continue or pause?
│
├─ TACTICAL DECISIONS (Mid-level)
│  ├─ Which tool to use for this task?
│  ├─ Should tasks run in parallel or sequential?
│  └─ Which error recovery strategy to apply?
│
└─ OPERATIONAL DECISIONS (Low-level)
   ├─ Which parameter values to use?
   ├─ How long to retry?
   └─ When to ask for user input?
```

---

## 🔧 TOOL SELECTION

### Tool Selection Algorithm
```typescript
/**
 * Tool Selection Engine
 * Chooses the best tool for each task
 */
class ToolSelectionEngine {
  private toolRegistry: ToolRegistry
  private historyAnalyzer: ToolHistoryAnalyzer
  
  constructor(
    toolRegistry: ToolRegistry,
    historyAnalyzer: ToolHistoryAnalyzer
  ) {
    this.toolRegistry = toolRegistry
    this.historyAnalyzer = historyAnalyzer
  }
  
  /**
   * Select best tool for a task
   */
  async selectTool(task: Task, context: Context): Promise<ToolSelection> {
    // Step 1: Get candidate tools
    const candidates = this.getCandidateTools(task)
    
    if (candidates.length === 0) {
      return {
        tool: null,
        confidence: 0,
        reason: 'No tools available for this task',
        alternatives: []
      }
    }
    
    // Step 2: Score each candidate
    const scored = await this.scoreTools(candidates, task, context)
    
    // Step 3: Sort by score
    scored.sort((a, b) => b.score - a.score)
    
    // Step 4: Check if top tool is clearly best
    const topTool = scored[0]
    const secondBest = scored[1]
    
    const confidence = this.calculateConfidence(topTool, secondBest, scored)
    
    // Step 5: Return selection with alternatives
    return {
      tool: topTool.tool,
      confidence,
      reason: topTool.reason,
      alternatives: scored.slice(1, 4).map(s => ({
        tool: s.tool,
        score: s.score,
        reason: s.reason
      }))
    }
  }
  
  /**
   * Get candidate tools for task
   */
  private getCandidateTools(task: Task): IToolInterface[] {
    // Match by task type
    const byType = this.toolRegistry.getToolsByCategory(
      this.mapTaskTypeToCategory(task.type)
    )
    
    // Match by task action
    const byAction = this.toolRegistry.searchByCapability(task.action)
    
    // Combine and deduplicate
    const candidates = [...new Set([...byType, ...byAction])]
    
    return candidates
  }
  
  /**
   * Score tools based on multiple factors
   */
  private async scoreTools(
    tools: IToolInterface[],
    task: Task,
    context: Context
  ): Promise<ScoredTool[]> {
    const scored: ScoredTool[] = []
    
    for (const tool of tools) {
      const score = await this.scoreTool(tool, task, context)
      scored.push(score)
    }
    
    return scored
  }
  
  /**
   * Score a single tool
   */
  private async scoreTool(
    tool: IToolInterface,
    task: Task,
    context: Context
  ): Promise<ScoredTool> {
    let totalScore = 0
    let maxScore = 0
    const factors: ScoreFactor[] = []
    
    // Factor 1: Capability Match (40 points)
    const capabilityScore = this.scoreCapabilityMatch(tool, task)
    totalScore += capabilityScore.score
    maxScore += 40
    factors.push(capabilityScore)
    
    // Factor 2: Historical Success (25 points)
    const historyScore = await this.scoreHistoricalSuccess(tool, task)
    totalScore += historyScore.score
    maxScore += 25
    factors.push(historyScore)
    
    // Factor 3: Availability (20 points)
    const availabilityScore = this.scoreAvailability(tool, context)
    totalScore += availabilityScore.score
    maxScore += 20
    factors.push(availabilityScore)
    
    // Factor 4: Cost Efficiency (10 points)
    const costScore = this.scoreCostEfficiency(tool, task)
    totalScore += costScore.score
    maxScore += 10
    factors.push(costScore)
    
    // Factor 5: Performance (5 points)
    const performanceScore = this.scorePerformance(tool, task)
    totalScore += performanceScore.score
    maxScore += 5
    factors.push(performanceScore)
    
    // Normalize to 0-100
    const normalizedScore = (totalScore / maxScore) * 100
    
    return {
      tool,
      score: normalizedScore,
      factors,
      reason: this.buildReason(factors)
    }
  }
  
  /**
   * Score capability match
   */
  private scoreCapabilityMatch(
    tool: IToolInterface,
    task: Task
  ): ScoreFactor {
    let score = 0
    let reasons: string[] = []
    
    // Check if tool has required capability
    const hasCapability = tool.capabilities.some(cap => 
      cap.action === task.action ||
      cap.action.includes(task.action) ||
      task.action.includes(cap.action)
    )
    
    if (hasCapability) {
      score += 30
      reasons.push('Has required capability')
    } else {
      reasons.push('Missing exact capability match')
    }
    
    // Check parameter compatibility
    const paramsMatch = this.checkParameterCompatibility(tool, task)
    if (paramsMatch) {
      score += 10
      reasons.push('Parameters compatible')
    }
    
    return {
      name: 'Capability Match',
      score,
      maxScore: 40,
      weight: 0.4,
      reason: reasons.join('; ')
    }
  }
  
  /**
   * Score historical success
   */
  private async scoreHistoricalSuccess(
    tool: IToolInterface,
    task: Task
  ): Promise<ScoreFactor> {
    const history = await this.historyAnalyzer.getToolHistory(tool.name)
    
    if (!history || history.totalUses === 0) {
      return {
        name: 'Historical Success',
        score: 15,  // Neutral score for new tools
        maxScore: 25,
        weight: 0.25,
        reason: 'No historical data (neutral score)'
      }
    }
    
    // Calculate success rate
    const successRate = history.successfulUses / history.totalUses
    
    // Score based on success rate
    let score = successRate * 20  // 0-20 points
    
    // Bonus for similar tasks
    const similarTaskSuccess = await this.historyAnalyzer.getSuccessRateForSimilarTasks(
      tool.name,
      task
    )
    
    if (similarTaskSuccess) {
      score += similarTaskSuccess * 5  // 0-5 points bonus
    }
    
    const reasons: string[] = []
    reasons.push(`${Math.round(successRate * 100)}% success rate`)
    
    if (history.totalUses < 5) {
      reasons.push('Limited historical data')
    }
    
    if (similarTaskSuccess) {
      reasons.push(`${Math.round(similarTaskSuccess * 100)}% success on similar tasks`)
    }
    
    return {
      name: 'Historical Success',
      score,
      maxScore: 25,
      weight: 0.25,
      reason: reasons.join('; ')
    }
  }
  
  /**
   * Score availability
   */
  private scoreAvailability(
    tool: IToolInterface,
    context: Context
  ): ScoreFactor {
    let score = 0
    let reasons: string[] = []
    
    // Check if tool is enabled
    if (context.enabledTools.includes(tool.name)) {
      score += 10
      reasons.push('Tool enabled')
    } else {
      reasons.push('Tool not enabled')
      return {
        name: 'Availability',
        score: 0,
        maxScore: 20,
        weight: 0.2,
        reason: reasons.join('; ')
      }
    }
    
    // Check authentication
    if (tool.requiredAuth) {
      const hasAuth = context.credentials[tool.requiredAuth.configKey]
      if (hasAuth) {
        score += 10
        reasons.push('Authentication configured')
      } else {
        reasons.push('Missing authentication')
      }
    } else {
      score += 10
      reasons.push('No authentication required')
    }
    
    return {
      name: 'Availability',
      score,
      maxScore: 20,
      weight: 0.2,
      reason: reasons.join('; ')
    }
  }
  
  /**
   * Score cost efficiency
   */
  private scoreCostEfficiency(
    tool: IToolInterface,
    task: Task
  ): ScoreFactor {
    let score = 10  // Start with max score
    let reasons: string[] = []
    
    if (!tool.costInfo || tool.costInfo.model === 'free') {
      reasons.push('Free tool')
      return {
        name: 'Cost Efficiency',
        score: 10,
        maxScore: 10,
        weight: 0.1,
        reason: reasons.join('; ')
      }
    }
    
    // Penalize based on estimated cost
    const estimatedCost = tool.costInfo.estimatedCost || 0
    
    if (estimatedCost === 0) {
      reasons.push('No cost')
    } else if (estimatedCost < 0.01) {
      score = 9
      reasons.push('Very low cost (<$0.01)')
    } else if (estimatedCost < 0.10) {
      score = 7
      reasons.push('Low cost (<$0.10)')
    } else if (estimatedCost < 1.00) {
      score = 5
      reasons.push('Moderate cost (<$1.00)')
    } else {
      score = 2
      reasons.push(`High cost ($${estimatedCost.toFixed(2)})`)
    }
    
    return {
      name: 'Cost Efficiency',
      score,
      maxScore: 10,
      weight: 0.1,
      reason: reasons.join('; ')
    }
  }
  
  /**
   * Score performance
   */
  private scorePerformance(
    tool: IToolInterface,
    task: Task
  ): ScoreFactor {
    let score = 5  // Default
    let reasons: string[] = []
    
    // Check estimated time
    const capability = tool.capabilities[0]
    if (capability?.estimatedTime) {
      if (capability.estimatedTime < 1000) {
        score = 5
        reasons.push('Very fast (<1s)')
      } else if (capability.estimatedTime < 5000) {
        score = 4
        reasons.push('Fast (<5s)')
      } else if (capability.estimatedTime < 30000) {
        score = 3
        reasons.push('Moderate (<30s)')
      } else {
        score = 2
        reasons.push('Slow (>30s)')
      }
    } else {
      reasons.push('No performance data')
    }
    
    return {
      name: 'Performance',
      score,
      maxScore: 5,
      weight: 0.05,
      reason: reasons.join('; ')
    }
  }
  
  /**
   * Calculate confidence in selection
   */
  private calculateConfidence(
    topTool: ScoredTool,
    secondBest: ScoredTool | undefined,
    allTools: ScoredTool[]
  ): number {
    // Base confidence on top score
    let confidence = topTool.score / 100
    
    // Increase confidence if clear winner
    if (secondBest) {
      const margin = topTool.score - secondBest.score
      if (margin > 20) {
        confidence = Math.min(confidence + 0.15, 1.0)
      } else if (margin > 10) {
        confidence = Math.min(confidence + 0.1, 1.0)
      } else if (margin < 5) {
        confidence = Math.max(confidence - 0.1, 0.5)
      }
    }
    
    // Decrease confidence if many similar-scoring options
    const similarScoring = allTools.filter(t => 
      Math.abs(t.score - topTool.score) < 10
    ).length
    
    if (similarScoring > 3) {
      confidence = Math.max(confidence - 0.1, 0.5)
    }
    
    return Math.round(confidence * 100) / 100
  }
  
  /**
   * Build reason from factors
   */
  private buildReason(factors: ScoreFactor[]): string {
    const topFactors = factors
      .filter(f => f.score > f.maxScore * 0.5)
      .sort((a, b) => b.score - a.score)
      .slice(0, 3)
    
    return topFactors.map(f => f.reason).join('; ')
  }
  
  /**
   * Check parameter compatibility
   */
  private checkParameterCompatibility(
    tool: IToolInterface,
    task: Task
  ): boolean {
    if (!task.parameters) return true
    
    // Use Zod to validate task parameters against tool schema
    try {
      tool.parameters.parse(task.parameters)
      return true
    } catch {
      return false
    }
  }
  
  /**
   * Map task type to tool category
   */
  private mapTaskTypeToCategory(taskType: string): ToolCategory {
    const mapping: Record<string, ToolCategory> = {
      'code_generation': ToolCategory.CodeAnalysis,
      'file_system': ToolCategory.FileSystem,
      'database': ToolCategory.Database,
      'execution': ToolCategory.Development,
      'analysis': ToolCategory.CodeAnalysis,
      'tool_call': ToolCategory.Service
    }
    
    return mapping[taskType] || ToolCategory.Development
  }
}

/**
 * Supporting Types
 */
interface ToolSelection {
  tool: IToolInterface | null
  confidence: number
  reason: string
  alternatives: Array<{
    tool: IToolInterface
    score: number
    reason: string
  }>
}

interface ScoredTool {
  tool: IToolInterface
  score: number
  factors: ScoreFactor[]
  reason: string
}

interface ScoreFactor {
  name: string
  score: number
  maxScore: number
  weight: number
  reason: string
}
```

---

### Tool Selection Example
```typescript
// Example: Selecting tool for database query task

const task: Task = {
  id: 'task-1',
  name: 'Query users table',
  type: 'database',
  action: 'query',
  parameters: {
    query: 'SELECT * FROM users WHERE active = true'
  }
}

const selection = await toolSelectionEngine.selectTool(task, context)

// Result:
{
  tool: supabaseQueryTool,
  confidence: 0.87,
  reason: 'Has required capability; 92% success rate; Tool enabled',
  alternatives: [
    {
      tool: neonQueryTool,
      score: 72,
      reason: 'Has required capability; Missing authentication'
    },
    {
      tool: planetscaleQueryTool,
      score: 65,
      reason: 'Has required capability; Tool not enabled'
    }
  ]
}

// Decision explanation:
// "Selected: supabase_query (confidence: 87%)
//  Reason: Supabase is enabled and authenticated with 92% historical success rate.
//  Alternative: neon_query available but not authenticated."
```

---

## 🎨 APPROACH SELECTION

### Comparing Multiple Approaches
```typescript
/**
 * Approach Selection Engine
 * When multiple valid approaches exist, choose the best one
 */
class ApproachSelectionEngine {
  /**
   * Evaluate and compare multiple approaches
   */
  async evaluateApproaches(
    problem: Problem,
    approaches: Approach[],
    context: Context
  ): Promise<ApproachEvaluation> {
    // Score each approach
    const evaluated = await Promise.all(
      approaches.map(approach => this.evaluateApproach(approach, problem, context))
    )
    
    // Sort by score
    evaluated.sort((a, b) => b.totalScore - a.totalScore)
    
    // Check confidence
    const topApproach = evaluated[0]
    const confidence = this.calculateApproachConfidence(evaluated)
    
    return {
      recommended: topApproach,
      confidence,
      alternatives: evaluated.slice(1),
      shouldAskUser: confidence < 0.7  // Low confidence = ask user
    }
  }
  
  /**
   * Evaluate a single approach
   */
  private async evaluateApproach(
    approach: Approach,
    problem: Problem,
    context: Context
  ): Promise<EvaluatedApproach> {
    const scores: Record<string, number> = {}
    
    // Criterion 1: Correctness (Does it solve the problem?)
    scores.correctness = this.scoreCorrectness(approach, problem)
    
    // Criterion 2: Complexity (How complex is implementation?)
    scores.complexity = this.scoreComplexity(approach)
    
    // Criterion 3: Maintainability (How easy to maintain?)
    scores.maintainability = this.scoreMaintainability(approach)
    
    // Criterion 4: Performance (How fast/efficient?)
    scores.performance = this.scorePerformance(approach)
    
    // Criterion 5: Cost (Financial cost)
    scores.cost = this.scoreCost(approach)
    
    // Criterion 6: Risk (How risky is this approach?)
    scores.risk = this.scoreRisk(approach, context)
    
    // Criterion 7: Time to Implement (How long to build?)
    scores.timeToImplement = this.scoreTimeToImplement(approach)
    
    // Criterion 8: Scalability (How well does it scale?)
    scores.scalability = this.scoreScalability(approach)
    
    // Calculate weighted total
    const weights = {
      correctness: 0.25,
      complexity: 0.15,
      maintainability: 0.15,
      performance: 0.10,
      cost: 0.10,
      risk: 0.10,
      timeToImplement: 0.10,
      scalability: 0.05
    }
    
    let totalScore = 0
    for (const [criterion, score] of Object.entries(scores)) {
      totalScore += score * weights[criterion]
    }
    
    return {
      approach,
      totalScore,
      scores,
      prosAndCons: this.generateProsAndCons(approach, scores),
      recommendation: this.generateRecommendation(totalScore)
    }
  }
  
  /**
   * Score correctness
   */
  private scoreCorrectness(approach: Approach, problem: Problem): number {
    // Check if approach addresses all requirements
    const requirementsCovered = approach.solves.length / problem.requirements.length
    
    // Check for edge cases
    const edgeCasesCovered = approach.handlesEdgeCases ? 1.0 : 0.7
    
    return (requirementsCovered * 0.7 + edgeCasesCovered * 0.3) * 100
  }
  
  /**
   * Score complexity
   */
  private scoreComplexity(approach: Approach): number {
    // Lower complexity = higher score
    let score = 100
    
    // Penalize for complexity indicators
    if (approach.filesRequired > 5) score -= 10
    if (approach.dependencies.length > 3) score -= 10
    if (approach.linesOfCode > 500) score -= 15
    if (approach.requiresExternalService) score -= 10
    
    return Math.max(score, 0)
  }
  
  /**
   * Score maintainability
   */
  private scoreMaintainability(approach: Approach): number {
    let score = 50  // Base score
    
    // Increase for good practices
    if (approach.wellDocumented) score += 20
    if (approach.followsConventions) score += 15
    if (approach.testable) score += 15
    
    return Math.min(score, 100)
  }
  
  /**
   * Score performance
   */
  private scorePerformance(approach: Approach): number {
    if (!approach.estimatedPerformance) return 50  // Neutral
    
    const perf = approach.estimatedPerformance
    
    // Response time
    let score = 0
    if (perf.responseTime < 100) score += 40
    else if (perf.responseTime < 500) score += 30
    else if (perf.responseTime < 1000) score += 20
    else score += 10
    
    // Memory usage
    if (perf.memoryUsage === 'low') score += 30
    else if (perf.memoryUsage === 'medium') score += 20
    else score += 10
    
    // Database queries
    if (perf.databaseQueries <= 3) score += 30
    else if (perf.databaseQueries <= 10) score += 20
    else score += 10
    
    return Math.min(score, 100)
  }
  
  /**
   * Score cost
   */
  private scoreCost(approach: Approach): number {
    if (approach.estimatedCost === 0) return 100
    
    // Logarithmic penalty for cost
    const penalty = Math.log10(approach.estimatedCost + 1) * 20
    return Math.max(100 - penalty, 0)
  }
  
  /**
   * Score risk
   */
  private scoreRisk(approach: Approach, context: Context): number {
    let score = 100
    
    // Risk factors
    if (approach.requiresDataMigration) score -= 20
    if (approach.breakingChange) score -= 25
    if (approach.affectsProduction && context.environment === 'production') score -= 30
    if (approach.requiresExpertise) score -= 10
    
    return Math.max(score, 0)
  }
  
  /**
   * Score time to implement
   */
  private scoreTimeToImplement(approach: Approach): number {
    // Lower time = higher score
    const hours = approach.estimatedImplementationTime / 3600000  // Convert ms to hours
    
    if (hours < 1) return 100
    if (hours < 4) return 80
    if (hours < 8) return 60
    if (hours < 24) return 40
    return 20
  }
  
  /**
   * Score scalability
   */
  private scoreScalability(approach: Approach): number {
    if (!approach.scalability) return 50  // Neutral
    
    const scales = {
      'excellent': 100,
      'good': 75,
      'moderate': 50,
      'poor': 25
    }
    
    return scales[approach.scalability] || 50
  }
  
  /**
   * Generate pros and cons
   */
  private generateProsAndCons(
    approach: Approach,
    scores: Record<string, number>
  ): { pros: string[], cons: string[] } {
    const pros: string[] = []
    const cons: string[] = []
    
    // Analyze each score
    if (scores.correctness > 90) pros.push('Fully addresses all requirements')
    if (scores.complexity > 80) pros.push('Simple to implement')
    else if (scores.complexity < 50) cons.push('Complex implementation')
    
    if (scores.maintainability > 80) pros.push('Easy to maintain')
    else if (scores.maintainability < 50) cons.push('Difficult to maintain')
    
    if (scores.performance > 80) pros.push('Excellent performance')
    else if (scores.performance < 50) cons.push('Performance concerns')
    
    if (scores.cost > 90) pros.push('No or very low cost')
    else if (scores.cost < 50) cons.push('Significant cost')
    
    if (scores.risk > 80) pros.push('Low risk')
    else if (scores.risk < 50) cons.push('High risk')
    
    if (scores.timeToImplement > 80) pros.push('Quick to implement')
    else if (scores.timeToImplement < 50) cons.push('Time-consuming')
    
    if (scores.scalability > 80) pros.push('Scales well')
    else if (scores.scalability < 50) cons.push('Scalability concerns')
    
    return { pros, cons }
  }
  
  /**
   * Generate recommendation text
   */
  private generateRecommendation(totalScore: number): string {
    if (totalScore > 80) return 'Highly recommended'
    if (totalScore > 70) return 'Recommended'
    if (totalScore > 60) return 'Acceptable'
    if (totalScore > 50) return 'Consider alternatives'
    return 'Not recommended'
  }
  
  /**
   * Calculate confidence in recommendation
   */
  private calculateApproachConfidence(evaluated: EvaluatedApproach[]): number {
    if (evaluated.length === 1) return 0.9  // Only one option
    
    const topScore = evaluated[0].totalScore
    const secondScore = evaluated[1].totalScore
    
    // Large margin = high confidence
    const margin = topScore - secondScore
    
    if (margin > 20) return 0.95
    if (margin > 15) return 0.85
    if (margin > 10) return 0.75
    if (margin > 5) return 0.65
    return 0.55  // Low confidence = should ask user
  }
}

/**
 * Supporting Types
 */
interface Problem {
  description: string
  requirements: string[]
  constraints: string[]
  context: Context
}

interface Approach {
  name: string
  description: string
  solves: string[]  // Which requirements it solves
  handlesEdgeCases: boolean
  filesRequired: number
  dependencies: string[]
  linesOfCode: number
  requiresExternalService: boolean
  wellDocumented: boolean
  followsConventions: boolean
  testable: boolean
  estimatedPerformance?: {
    responseTime: number  // ms
    memoryUsage: 'low' | 'medium' | 'high'
    databaseQueries: number
  }
  estimatedCost: number  // USD
  requiresDataMigration: boolean
  breakingChange: boolean
  affectsProduction: boolean
  requiresExpertise: boolean
  estimatedImplementationTime: number  // ms
  scalability?: 'excellent' | 'good' | 'moderate' | 'poor'
}

interface EvaluatedApproach {
  approach: Approach
  totalScore: number
  scores: Record<string, number>
  prosAndCons: {
    pros: string[]
    cons: string[]
  }
  recommendation: string
}

interface ApproachEvaluation {
  recommended: EvaluatedApproach
  confidence: number
  alternatives: EvaluatedApproach[]
  shouldAskUser: boolean
}
```

---

### Approach Comparison Example
```typescript
// Example: Multi-tenant architecture decision (from Segment 2, Article II)

const problem: Problem = {
  description: 'Design multi-tenant architecture for SaaS app',
  requirements: [
    'Isolate customer data',
    'Support 10+ tenants initially',
    'Cost-effective (<$100/month)',
    'Scalable to 1000+ tenants',
    'GDPR compliant'
  ],
  constraints: [
    'Budget: $100/month',
    'Timeline: 4 weeks',
    'Team: 1 developer'
  ],
  context: currentContext
}

const approaches: Approach[] = [
  {
    name: 'Row-Level Isolation (Shared DB, Shared Schema)',
    description: 'Single database, tenantId column in every table',
    // ... full specification
  },
  {
    name: 'Schema-Level Isolation (Shared DB, Separate Schemas)',
    description: 'Single database, separate schema per tenant',
    // ... full specification
  },
  {
    name: 'Database-Level Isolation (Separate Databases)',
    description: 'Completely separate database per tenant',
    // ... full specification
  }
]

const evaluation = await approachSelectionEngine.evaluateApproaches(
  problem,
  approaches,
  context
)

// Result:
{
  recommended: {
    approach: approaches[1],  // Schema-Level Isolation
    totalScore: 78,
    scores: {
      correctness: 95,
      complexity: 60,
      maintainability: 70,
      performance: 80,
      cost: 85,
      risk: 70,
      timeToImplement: 65,
      scalability: 85
    },
    prosAndCons: {
      pros: [
        'Fully addresses all requirements',
        'Cost-effective',
        'Good performance',
        'Scales well'
      ],
      cons: [
        'Moderate complexity',
        'Time-consuming (4 weeks)'
      ]
    },
    recommendation: 'Highly recommended'
  },
  confidence: 0.85,
  alternatives: [
    { approach: approaches[0], totalScore: 65, ... },
    { approach: approaches[2], totalScore: 58, ... }
  ],
  shouldAskUser: false  // High confidence = don't need to ask
}
```

---

## 🔍 ERROR ANALYSIS

### Analyzing Errors
```typescript
/**
 * Error Analysis Engine
 * Understand what went wrong and why
 */
class ErrorAnalysisEngine {
  /**
   * Analyze an error
   */
  async analyzeError(
    error: Error,
    context: ErrorContext
  ): Promise<ErrorAnalysis> {
    // Classify error
    const classification = this.classifyError(error)
    
    // Identify root cause
    const rootCause = await this.identifyRootCause(error, context)
    
    // Assess severity
    const severity = this.assessSeverity(error, context)
    
    // Determine if recoverable
    const recoverable = this.isRecoverable(classification, severity)
    
    // Generate explanation
    const explanation = this.generateExplanation(
      error,
      classification,
      rootCause
    )
    
    return {
      classification,
      rootCause,
      severity,
      recoverable,
      explanation,
      suggestedActions: this.suggestActions(classification, rootCause)
    }
  }
  
  /**
   * Classify error
   */
  private classifyError(error: Error): ErrorClassification {
    const message = error.message.toLowerCase()
    const stack = error.stack?.toLowerCase() || ''
    
    // Authentication errors
    if (message.includes('authentication') || 
        message.includes('unauthorized') ||
        message.includes('invalid token')) {
      return {
        category: 'authentication',
        subcategory: this.detectAuthSubcategory(message),
        confidence: 0.95
      }
    }
    
    // Network errors
    if (message.includes('network') ||
        message.includes('econnrefused') ||
        message.includes('enotfound') ||
        message.includes('etimedout')) {
      return {
        category: 'network',
        subcategory: this.detectNetworkSubcategory(message),
        confidence: 0.90
      }
    }
    
    // Rate limiting
    if (message.includes('rate limit') ||
        message.includes('too many requests') ||
        message.includes('429')) {
      return {
        category: 'rate_limit',
        subcategory: 'exceeded',
        confidence: 0.95
      }
    }
    
    // Permission errors
    if (message.includes('permission') ||
        message.includes('forbidden') ||
        message.includes('access denied')) {
      return {
        category: 'permission',
        subcategory: this.detectPermissionSubcategory(message),
        confidence: 0.90
      }
    }
    
    // Resource errors
    if (message.includes('not found') ||
        message.includes('does not exist') ||
        message.includes('404')) {
      return {
        category: 'resource',
        subcategory: 'not_found',
        confidence: 0.85
      }
    }
    
    // Validation errors
    if (message.includes('validation') ||
        message.includes('invalid') ||
        message.includes('required')) {
      return {
        category: 'validation',
        subcategory: this.detectValidationSubcategory(message),
        confidence: 0.80
      }
    }
    
    // Timeout errors
    if (message.includes('timeout') ||
        message.includes('timed out')) {
      return {
        category: 'timeout',
        subcategory: this.detectTimeoutSubcategory(message),
        confidence: 0.90
      }
    }
    
    // Syntax errors (code)
    if (message.includes('syntax') ||
        message.includes('unexpected token') ||
        stack.includes('syntaxerror')) {
      return {
        category: 'syntax',
        subcategory: 'code',
        confidence: 0.95
      }
    }
    
    // Type errors
    if (message.includes('type') ||
        stack.includes('typeerror')) {
      return {
        category: 'type',
        subcategory: this.detectTypeSubcategory(message),
        confidence: 0.85
      }
    }
    
    // Unknown
    return {
      category: 'unknown',
      subcategory: 'unclassified',
      confidence: 0.50
    }
  }
  
  /**
   * Identify root cause
   */
  private async identifyRootCause(
    error: Error,
    context: ErrorContext
  ): Promise<RootCause> {
    const classification = this.classifyError(error)
    
    switch (classification.category) {
      case 'authentication':
        return this.analyzeAuthError(error, context)
      
      case 'network':
        return this.analyzeNetworkError(error, context)
      
      case 'rate_limit':
        return {
          cause: 'Rate limit exceeded',
          details: 'Too many requests sent to API in short time',
          likelyCulprit: 'API rate limiting policy'
        }
      
      case 'permission':
        return this.analyzePermissionError(error, context)
      
      case 'validation':
        return this.analyzeValidationError(error, context)
      
      case 'timeout':
        return this.analyzeTimeoutError(error, context)
      
      default:
        return {
          cause: 'Unknown',
          details: error.message,
          likelyCulprit: 'Unidentified'
        }
    }
  }
  
  /**
   * Analyze authentication error
   */
  private analyzeAuthError(
    error: Error,
    context: ErrorContext
  ): RootCause {
    const message = error.message.toLowerCase()
    
    if (message.includes('expired')) {
      return {
        cause: 'Authentication token expired',
        details: 'The API token or session has expired',
        likelyCulprit: 'Token not refreshed',
        fix: 'Refresh authentication token or re-authenticate'
      }
    }
    
    if (message.includes('invalid')) {
      return {
        cause: 'Invalid authentication credentials',
        details: 'The provided API key or token is not valid',
        likelyCulprit: 'Incorrect configuration',
        fix: 'Verify API key in environment variables'
      }
    }
    
    if (message.includes('missing')) {
      return {
        cause: 'Missing authentication',
        details: 'No authentication credentials provided',
        likelyCulprit: 'Environment variable not set',
        fix: `Set ${context.requiredAuthKey} environment variable`
      }
    }
    
    return {
      cause: 'Authentication failure',
      details: error.message,
      likelyCulprit: 'Authentication configuration',
      fix: 'Check authentication setup'
    }
  }
  
  /**
   * Assess severity
   */
  private assessSeverity(
    error: Error,
    context: ErrorContext
  ): ErrorSeverity {
    const classification = this.classifyError(error)
    
    // Critical severity
    if (context.affectsProduction &&
        (classification.category === 'data_loss' ||
         classification.category === 'security')) {
      return {
        level: 'critical',
        score: 10,
        reason: 'Production data or security affected'
      }
    }
    
    // High severity
    if (context.blocksExecution ||
        classification.category === 'authentication' ||
        classification.category === 'permission') {
      return {
        level: 'high',
        score: 8,
        reason: 'Blocks further execution'
      }
    }
    
    // Medium severity
    if (classification.category === 'network' ||
        classification.category === 'timeout' ||
        classification.category === 'rate_limit') {
      return {
        level: 'medium',
        score: 5,
        reason: 'Temporary issue, likely recoverable'
      }
    }
    
    // Low severity
    if (classification.category === 'validation') {
      return {
        level: 'low',
        score: 3,
        reason: 'Input issue, easy to fix'
      }
    }
    
    // Info level
    return {
      level: 'info',
      score: 1,
      reason: 'Minor issue'
    }
  }
  
  /**
   * Check if error is recoverable
   */
  private isRecoverable(
    classification: ErrorClassification,
    severity: ErrorSeverity
  ): boolean {
    // Critical errors are not recoverable
    if (severity.level === 'critical') return false
    
    // Recoverable categories
    const recoverableCategories = [
      'network',
      'timeout',
      'rate_limit',
      'authentication'  // Can refresh token
    ]
    
    return recoverableCategories.includes(classification.category)
  }
  
  /**
   * Generate human-readable explanation
   */
  private generateExplanation(
    error: Error,
    classification: ErrorClassification,
    rootCause: RootCause
  ): string {
    return `
${rootCause.cause}

What happened:
${rootCause.details}

Likely cause:
${rootCause.likelyCulprit}

${rootCause.fix ? `How to fix:\n${rootCause.fix}` : ''}
`.trim()
  }
  
  /**
   * Suggest actions to take
   */
  private suggestActions(
    classification: ErrorClassification,
    rootCause: RootCause
  ): string[] {
    const actions: string[] = []
    
    switch (classification.category) {
      case 'authentication':
        actions.push('Verify API credentials are correct')
        actions.push('Check if token has expired')
        actions.push('Ensure environment variables are set')
        break
      
      case 'network':
        actions.push('Check internet connection')
        actions.push('Verify API endpoint URL is correct')
        actions.push('Try again in a moment')
        break
      
      case 'rate_limit':
        actions.push('Wait before retrying')
        actions.push('Implement rate limiting on client side')
        actions.push('Consider upgrading API plan')
        break
      
      case 'permission':
        actions.push('Check account permissions')
        actions.push('Verify API key has required scopes')
        break
      
      case 'validation':
        actions.push('Check input parameters')
        actions.push('Verify required fields are provided')
        break
      
      case 'timeout':
        actions.push('Increase timeout duration')
        actions.push('Check if operation is too slow')
        actions.push('Try again')
        break
    }
    
    if (rootCause.fix) {
      actions.unshift(rootCause.fix)
    }
    
    return actions
  }
  
  // Additional helper methods...
  private detectAuthSubcategory(message: string): string {
    if (message.includes('expired')) return 'expired'
    if (message.includes('invalid')) return 'invalid'
    if (message.includes('missing')) return 'missing'
    return 'general'
  }
  
  private detectNetworkSubcategory(message: string): string {
    if (message.includes('refused')) return 'connection_refused'
    if (message.includes('not found')) return 'dns_error'
    if (message.includes('timeout')) return 'timeout'
    return 'general'
  }
  
  private detectPermissionSubcategory(message: string): string {
    if (message.includes('read')) return 'read_denied'
    if (message.includes('write')) return 'write_denied'
    if (message.includes('execute')) return 'execute_denied'
    return 'general'
  }
  
  private detectValidationSubcategory(message: string): string {
    if (message.includes('required')) return 'missing_required'
    if (message.includes('format')) return 'invalid_format'
    if (message.includes('type')) return 'wrong_type'
    return 'general'
  }
  
  private detectTimeoutSubcategory(message: string): string {
    if (message.includes('connect')) return 'connection_timeout'
    if (message.includes('read')) return 'read_timeout'
    if (message.includes('write')) return 'write_timeout'
    return 'general'
  }
  
  private detectTypeSubcategory(message: string): string {
    if (message.includes('undefined')) return 'undefined'
    if (message.includes('null')) return 'null'
    if (message.includes('function')) return 'not_a_function'
    return 'type_mismatch'
  }
  
  private analyzeNetworkError(error: Error, context: ErrorContext): RootCause {
    // Similar pattern to analyzeAuthError
    return {
      cause: 'Network connection failed',
      details: error.message,
      likelyCulprit: 'Network connectivity',
      fix: 'Check internet connection and try again'
    }
  }
  
  private analyzePermissionError(error: Error, context: ErrorContext): RootCause {
    return {
      cause: 'Permission denied',
      details: error.message,
      likelyCulprit: 'Insufficient permissions',
      fix: 'Check file/API permissions'
    }
  }
  
  private analyzeValidationError(error: Error, context: ErrorContext): RootCause {
    return {
      cause: 'Validation failed',
      details: error.message,
      likelyCulprit: 'Invalid input parameters',
      fix: 'Check input values match expected format'
    }
  }
  
  private analyzeTimeoutError(error: Error, context: ErrorContext): RootCause {
    return {
      cause: 'Operation timed out',
      details: error.message,
      likelyCulprit: 'Operation took too long',
      fix: 'Increase timeout or optimize operation'
    }
  }
}

/**
 * Supporting Types
 */
interface ErrorContext {
  taskId: string
  toolUsed?: string
  operation: string
  affectsProduction: boolean
  blocksExecution: boolean
  requiredAuthKey?: string
}

interface ErrorClassification {
  category: string
  subcategory: string
  confidence: number
}

interface RootCause {
  cause: string
  details: string
  likelyCulprit: string
  fix?: string
}

interface ErrorSeverity {
  level: 'critical' | 'high' | 'medium' | 'low' | 'info'
  score: number
  reason: string
}

interface ErrorAnalysis {
  classification: ErrorClassification
  rootCause: RootCause
  severity: ErrorSeverity
  recoverable: boolean
  explanation: string
  suggestedActions: string[]
}
```

---

## 🔄 RECOVERY STRATEGY SELECTION

### Choosing Recovery Approach
```typescript
/**
 * Recovery Strategy Selector
 * Determines best recovery approach for errors
 */
class RecoveryStrategySelector {
  private errorAnalyzer: ErrorAnalysisEngine
  
  constructor(errorAnalyzer: ErrorAnalysisEngine) {
    this.errorAnalyzer = errorAnalyzer
  }
  
  /**
   * Select recovery strategy
   */
  async selectStrategy(
    error: Error,
    context: ErrorContext,
    attemptNumber: number = 1
  ): Promise<RecoveryStrategy> {
    // Analyze error
    const analysis = await this.errorAnalyzer.analyzeError(error, context)
    
    // If not recoverable, fail fast
    if (!analysis.recoverable) {
      return {
        type: 'fail',
        reason: 'Error is not recoverable',
        actions: analysis.suggestedActions
      }
    }
    
    // Select strategy based on error type and attempt number
    return this.selectByCategory(
      analysis,
      attemptNumber,
      context
    )
  }
  
  /**
   * Select strategy by error category
   */
  private selectByCategory(
    analysis: ErrorAnalysis,
    attemptNumber: number,
    context: ErrorContext
  ): RecoveryStrategy {
    switch (analysis.classification.category) {
      case 'network':
      case 'timeout':
        return this.networkRecoveryStrategy(attemptNumber)
      
      case 'rate_limit':
        return this.rateLimitRecoveryStrategy(analysis)
      
      case 'authentication':
        return this.authRecoveryStrategy(attemptNumber)
      
      case 'permission':
        return {
          type: 'ask_user',
          reason: 'Permission issue requires user intervention',
          question: 'Grant required permissions and retry?',
          actions: analysis.suggestedActions
        }
      
      case 'validation':
        return {
          type: 'ask_user',
          reason: 'Input validation failed',
          question: 'Fix input parameters and retry?',
          actions: analysis.suggestedActions
        }
      
      case 'resource':
        return {
          type: 'alternative',
          reason: 'Resource not found, trying alternative',
          alternative: 'Use fallback resource or skip'
        }
      
      default:
        return {
          type: 'fail',
          reason: 'Unknown error type',
          actions: analysis.suggestedActions
        }
    }
  }
  
  /**
   * Network recovery strategy
   */
  private networkRecoveryStrategy(attemptNumber: number): RecoveryStrategy {
    const maxAttempts = 3
    
    if (attemptNumber >= maxAttempts) {
      return {
        type: 'fail',
        reason: `Network error persists after ${maxAttempts} attempts`,
        actions: ['Check network connection', 'Contact support if issue persists']
      }
    }
    
    // Exponential backoff
    const backoffMs = Math.pow(2, attemptNumber) * 1000
    
    return {
      type: 'retry',
      reason: `Network error (attempt ${attemptNumber}/${maxAttempts})`,
      delayMs: backoffMs,
      maxAttempts
    }
  }
  
  /**
   * Rate limit recovery strategy
   */
  private rateLimitRecoveryStrategy(analysis: ErrorAnalysis): RecoveryStrategy {
    // Extract wait time from error message if available
    const waitTimeMatch = analysis.rootCause.details.match(/retry after (\d+) seconds/i)
    const waitSeconds = waitTimeMatch ? parseInt(waitTimeMatch[1]) : 60
    
    return {
      type: 'retry',
      reason: 'Rate limit exceeded, waiting before retry',
      delayMs: waitSeconds * 1000,
      maxAttempts: 1  // Only retry once after waiting
    }
  }
  
  /**
   * Authentication recovery strategy
   */
  private authRecoveryStrategy(attemptNumber: number): RecoveryStrategy {
    if (attemptNumber > 1) {
      return {
        type: 'ask_user',
        reason: 'Authentication failed multiple times',
        question: 'Update credentials and retry?',
        actions: ['Check API key', 'Verify credentials', 'Re-authenticate']
      }
    }
    
    return {
      type: 'retry',
      reason: 'Authentication failed, attempting token refresh',
      delayMs: 2000,
      maxAttempts: 2
    }
  }
}

/**
 * Recovery Strategy Types
 */
interface RecoveryStrategy {
  type: 'retry' | 'alternative' | 'skip' | 'ask_user' | 'fail'
  reason: string
  delayMs?: number
  maxAttempts?: number
  alternative?: string
  question?: string
  actions?: string[]
}
```

---

## 📊 CONFIDENCE SCORING

### Measuring Decision Confidence
```typescript
/**
 * Confidence Scorer
 * Calculates confidence in decisions
 */
class ConfidenceScorer {
  /**
   * Calculate confidence score (0-1)
   */
  calculateConfidence(decision: Decision): number {
    let confidence = 0.5  // Start neutral
    
    // Factor 1: Data Quality (30%)
    confidence += this.scoreDataQuality(decision) * 0.3
    
    // Factor 2: Historical Performance (25%)
    confidence += this.scoreHistoricalPerformance(decision) * 0.25
    
    // Factor 3: Consensus (20%)
    confidence += this.scoreConsensus(decision) * 0.20
    
    // Factor 4: Complexity (15%)
    confidence += this.scoreComplexity(decision) * 0.15
    
    // Factor 5: Validation (10%)
    confidence += this.scoreValidation(decision) * 0.10
    
    return Math.max(0, Math.min(1, confidence))
  }
  
  /**
   * Score data quality
   */
  private scoreDataQuality(decision: Decision): number {
    let score = 0
    
    // Complete data
    if (decision.hasCompleteData) score += 0.4
    
    // Recent data
    if (decision.dataRecency === 'current') score += 0.3
    else if (decision.dataRecency === 'recent') score += 0.2
    
    // Verified data
    if (decision.dataVerified) score += 0.3
    
    return score
  }
  
  /**
   * Score historical performance
   */
  private scoreHistoricalPerformance(decision: Decision): number {
    if (!decision.historicalData) return 0.5  // Neutral
    
    const successRate = decision.historicalData.successRate
    const sampleSize = decision.historicalData.sampleSize
    
    // Adjust confidence based on sample size
    let confidence = successRate
    
    if (sampleSize < 5) {
      confidence *= 0.7  // Low confidence with small sample
    } else if (sampleSize < 20) {
      confidence *= 0.9
    }
    
    return confidence
  }
  
  /**
   * Score consensus (multiple indicators agree)
   */
  private scoreConsensus(decision: Decision): number {
    if (!decision.alternativeScores) return 0.5
    
    const topScore = decision.score
    const alternatives = decision.alternativeScores
    
    // Calculate score difference
    const avgAlternative = alternatives.reduce((sum, s) => sum + s, 0) / alternatives.length
    const difference = topScore - avgAlternative
    
    // High difference = high consensus
    if (difference > 20) return 1.0
    if (difference > 10) return 0.8
    if (difference > 5) return 0.6
    return 0.3  // Low consensus
  }
  
  /**
   * Score complexity (simpler = higher confidence)
   */
  private scoreComplexity(decision: Decision): number {
    const complexity = decision.complexity || 'medium'
    
    switch (complexity) {
      case 'simple': return 1.0
      case 'medium': return 0.7
      case 'complex': return 0.4
      case 'very_complex': return 0.2
      default: return 0.5
    }
  }
  
  /**
   * Score validation
   */
  private scoreValidation(decision: Decision): number {
    let score = 0
    
    if (decision.validated) score += 0.5
    if (decision.testedInProduction) score += 0.3
    if (decision.peerReviewed) score += 0.2
    
    return Math.min(score, 1.0)
  }
  
  /**
   * Get confidence level label
   */
  getConfidenceLevel(confidence: number): ConfidenceLevel {
    if (confidence >= 0.9) return 'very_high'
    if (confidence >= 0.75) return 'high'
    if (confidence >= 0.6) return 'medium'
    if (confidence >= 0.4) return 'low'
    return 'very_low'
  }
  
  /**
   * Should ask user? (low confidence = ask)
   */
  shouldAskUser(confidence: number, decision: Decision): boolean {
    // Always ask for critical decisions with low confidence
    if (decision.impact === 'critical' && confidence < 0.7) {
      return true
    }
    
    // Ask for high-impact decisions with medium confidence
    if (decision.impact === 'high' && confidence < 0.6) {
      return true
    }
    
    // Ask for any decision with very low confidence
    if (confidence < 0.5) {
      return true
    }
    
    return false
  }
}

type ConfidenceLevel = 'very_high' | 'high' | 'medium' | 'low' | 'very_low'

interface Decision {
  type: string
  score: number
  alternativeScores?: number[]
  hasCompleteData: boolean
  dataRecency: 'current' | 'recent' | 'old'
  dataVerified: boolean
  historicalData?: {
    successRate: number
    sampleSize: number
  }
  complexity?: 'simple' | 'medium' | 'complex' | 'very_complex'
  validated: boolean
  testedInProduction: boolean
  peerReviewed: boolean
  impact: 'low' | 'medium' | 'high' | 'critical'
}
```

---

## 📚 RELATED ARTICLES

| Article | Purpose | Relationship to Decision Engine |
|---------|---------|--------------------------------|
| **Article I: Task Orchestration** | Task decomposition | Decision engine selects tools for tasks |
| **Article II: State Management** | State persistence | Decision history stored in state |
| **Segment 2, Article II** | Twelve Patterns | Decision engine selects pattern |
| **Segment 3, Article I** | Tool Architecture | Decision engine scores tools |

---

**Previous:** [← Article II: State Management](./02-article-ii-state-management.md)  
**Next Segment:** [Segment 5: Quality Layer →](../05-quality-layer/README.md)

---

**Last Updated:** February 7, 2026  
**Constitutional Version:** 2.0.0  
**Status:** ✅ Ratified and In Force

**Motto:** *"Intelligence Through Systematic Decision-Making - Every Choice Has a Reason"*
```

---

## ✅ ARTICLE III: DECISION ENGINE - 100% COMPLETE!

## 🎉 SEGMENT 4: EXECUTION LAYER - 100% COMPLETE!

**What's Included in Article III:**
- Complete tool selection algorithm (5 scoring factors)
- Approach evaluation and comparison (8 criteria)
- Error analysis engine (10+ error categories)
- Recovery strategy selection (6 strategies)
- Confidence scoring system (5 factors)
- Real-world decision examples

**Segment 4 Complete Stats:**
- **Total Articles:** 3/3 complete (README + 3 articles)
- **Total Lines:** ~6,500 lines
- **Total Words:** ~52,000 words
- **Decision Systems:** Tool selection, Approach selection, Error analysis, Recovery strategies, Confidence scoring

---

## 📊 OVERALL PROJECT STATUS UPDATE
```
✅ SEGMENT 1: FOUNDATION LAYER - 100% (7/7 articles)
✅ SEGMENT 2: ADVANCED PROMPTING - 100% (3/3 articles)
✅ SEGMENT 3: TOOLING LAYER - 100% (4/4 articles)
✅ SEGMENT 4: EXECUTION LAYER - 100% (3/3 articles)
⬜ SEGMENT 5: QUALITY LAYER - 0% (not started)
⬜ SEGMENT 6: DEPLOYMENT LAYER - 0% (not started)
⬜ SEGMENT 7: GOVERNANCE LAYER - 0% (not started)

COMPLETION: 57% (4/7 segments complete)
