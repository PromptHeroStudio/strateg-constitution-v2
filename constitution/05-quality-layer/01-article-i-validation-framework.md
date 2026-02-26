# ✅ ARTICLE I: VALIDATION FRAMEWORK

**The Foundation of Quality Assurance in MVCA**

---

## 📜 CONSTITUTIONAL AUTHORITY

This article defines the **validation framework** - the system of rules, checkers, and pipelines that ensure all MVCA outputs meet quality standards.

**Legal Force:**
- ✅ All code **MUST** pass validation before delivery
- ✅ Validation rules **SHALL** be applied consistently
- ✅ Validation failures **SHALL** trigger auto-fix attempts
- ✅ Validation reports **SHALL** be provided to users
- ✅ Users **SHALL** be able to override validation (with warnings)

**Constitutional Principle:**
> Validation is systematic, automated, and transparent.  
> Every output is checked. Every failure is explained.

---

## 🎯 VALIDATION RULE SYSTEM

### What is a Validation Rule?

**Definition:** A validation rule is a testable condition that code must satisfy to be considered quality-compliant.

**Rule Structure:**
```typescript
/**
 * Validation Rule Interface
 * All validation rules implement this interface
 */
interface ValidationRule {
  // IDENTITY
  id: string                         // Unique rule ID (e.g., "SEC-001")
  name: string                       // Human-readable name
  category: RuleCategory             // Security, Performance, Quality, etc.
  severity: RuleSeverity             // Critical, High, Medium, Low
  
  // DESCRIPTION
  description: string                // What this rule checks
  rationale: string                  // Why this rule exists
  examples: {
    good: string[]                   // Examples that pass
    bad: string[]                    // Examples that fail
  }
  
  // VALIDATION
  check: (code: CodeContext) => ValidationResult
  
  // FIX
  autoFixable: boolean               // Can this be fixed automatically?
  fix?: (code: CodeContext) => FixResult
  
  // METADATA
  enabled: boolean                   // Is this rule enabled?
  tags: string[]                     // For categorization (owasp, performance, etc.)
  links: string[]                    // Reference links (docs, OWASP, etc.)
}

/**
 * Rule Categories
 */
enum RuleCategory {
  Security = 'security',
  Performance = 'performance',
  CodeQuality = 'code_quality',
  Functionality = 'functionality',
  Accessibility = 'accessibility',
  BestPractices = 'best_practices'
}

/**
 * Rule Severity
 */
enum RuleSeverity {
  Critical = 'critical',    // Must fix (blocks delivery)
  High = 'high',           // Should fix (warning)
  Medium = 'medium',       // Should fix (warning)
  Low = 'low',            // Nice to fix (info)
  Info = 'info'           // Informational only
}

/**
 * Validation Result
 */
interface ValidationResult {
  passed: boolean
  message?: string
  violations?: Violation[]
  score?: number           // 0-100
}

/**
 * Violation
 */
interface Violation {
  ruleId: string
  severity: RuleSeverity
  message: string
  location: {
    file: string
    line?: number
    column?: number
    snippet?: string       // Code snippet showing violation
  }
  fix?: {
    description: string
    autoFixable: boolean
    fixCode?: string       // Suggested fix code
  }
}

/**
 * Code Context
 */
interface CodeContext {
  files: CodeFile[]
  projectType: 'web' | 'api' | 'mobile' | 'cli'
  framework?: string
  language: string
  dependencies: Record<string, string>
  environment: 'development' | 'production'
}

interface CodeFile {
  path: string
  content: string
  ast?: any              // Abstract Syntax Tree (if parsed)
  language: string
}
```

---

### Example Validation Rules

#### Rule 1: No SQL Injection (Security)
```typescript
const noSqlInjectionRule: ValidationRule = {
  // IDENTITY
  id: 'SEC-001',
  name: 'Prevent SQL Injection',
  category: RuleCategory.Security,
  severity: RuleSeverity.Critical,
  
  // DESCRIPTION
  description: 'Detects potential SQL injection vulnerabilities from string concatenation in queries',
  rationale: 'SQL injection is OWASP A03 - one of the most dangerous vulnerabilities. Never concatenate user input into SQL queries.',
  
  examples: {
    bad: [
      `const query = "SELECT * FROM users WHERE id = " + userId`,
      `db.query('DELETE FROM posts WHERE id = ' + req.params.id)`,
      `const sql = \`UPDATE users SET name = '\${userName}' WHERE id = \${id}\``
    ],
    good: [
      `const query = "SELECT * FROM users WHERE id = $1"`,
      `db.query('DELETE FROM posts WHERE id = $1', [req.params.id])`,
      `prisma.user.update({ where: { id }, data: { name } })`
    ]
  },
  
  // VALIDATION
  check: (context: CodeContext): ValidationResult => {
    const violations: Violation[] = []
    
    for (const file of context.files) {
      // Skip non-code files
      if (!file.language.match(/typescript|javascript|tsx|jsx/)) continue
      
      // Pattern 1: String concatenation with query keywords
      const sqlConcatPattern = /(['"`])(?:SELECT|INSERT|UPDATE|DELETE|DROP).*?\1\s*\+/gi
      let match
      
      while ((match = sqlConcatPattern.exec(file.content)) !== null) {
        const lineNumber = file.content.substring(0, match.index).split('\n').length
        const snippet = this.getCodeSnippet(file.content, match.index)
        
        violations.push({
          ruleId: 'SEC-001',
          severity: RuleSeverity.Critical,
          message: 'Potential SQL injection: Query uses string concatenation instead of parameterized queries',
          location: {
            file: file.path,
            line: lineNumber,
            snippet
          },
          fix: {
            description: 'Use parameterized queries with placeholders ($1, $2, etc.) instead of string concatenation',
            autoFixable: false  // Requires context to auto-fix
          }
        })
      }
      
      // Pattern 2: Template literals with SQL keywords
      const templateSqlPattern = /`(?:SELECT|INSERT|UPDATE|DELETE|DROP).*?\$\{/gi
      
      while ((match = templateSqlPattern.exec(file.content)) !== null) {
        const lineNumber = file.content.substring(0, match.index).split('\n').length
        const snippet = this.getCodeSnippet(file.content, match.index)
        
        violations.push({
          ruleId: 'SEC-001',
          severity: RuleSeverity.Critical,
          message: 'Potential SQL injection: Query uses template literal with interpolation',
          location: {
            file: file.path,
            line: lineNumber,
            snippet
          },
          fix: {
            description: 'Use parameterized queries instead of template literals',
            autoFixable: false
          }
        })
      }
    }
    
    return {
      passed: violations.length === 0,
      violations,
      score: violations.length === 0 ? 100 : 0  // Binary: pass or fail
    }
  },
  
  // FIX
  autoFixable: false,  // SQL injection fixes require understanding context
  
  // METADATA
  enabled: true,
  tags: ['security', 'owasp-a03', 'injection', 'sql', 'critical'],
  links: [
    'https://owasp.org/www-community/attacks/SQL_Injection',
    'https://cheatsheetseries.owasp.org/cheatsheets/SQL_Injection_Prevention_Cheat_Sheet.html'
  ],
  
  // HELPER
  getCodeSnippet(content: string, index: number, contextLines: number = 2): string {
    const lines = content.split('\n')
    const lineIndex = content.substring(0, index).split('\n').length - 1
    
    const start = Math.max(0, lineIndex - contextLines)
    const end = Math.min(lines.length, lineIndex + contextLines + 1)
    
    return lines.slice(start, end).join('\n')
  }
}
```

---

#### Rule 2: No Plaintext Passwords (Security)
```typescript
const noPlaintextPasswordsRule: ValidationRule = {
  id: 'SEC-002',
  name: 'No Plaintext Password Storage',
  category: RuleCategory.Security,
  severity: RuleSeverity.Critical,
  
  description: 'Detects password fields being stored without hashing',
  rationale: 'Passwords must ALWAYS be hashed before storage (bcrypt, argon2). Plaintext passwords are OWASP A02 vulnerability.',
  
  examples: {
    bad: [
      `await prisma.user.create({ data: { password: req.body.password } })`,
      `user.password = newPassword; await user.save()`,
      `INSERT INTO users (email, password) VALUES ($1, $2)` // Without hashing
    ],
    good: [
      `const hash = await bcrypt.hash(password, 12); await prisma.user.create({ data: { password: hash } })`,
      `user.password = await argon2.hash(newPassword); await user.save()`
    ]
  },
  
  check: (context: CodeContext): ValidationResult => {
    const violations: Violation[] = []
    
    for (const file of context.files) {
      if (!file.language.match(/typescript|javascript/)) continue
      
      // Pattern: password field assignment without hashing
      const patterns = [
        /\.password\s*=\s*(?!(?:await\s+)?(?:bcrypt|argon2|crypto\.pbkdf2|scrypt))/gi,
        /password:\s*(?:req\.body\.password|password|newPassword)(?!\s*}\s*,\s*hash)/gi
      ]
      
      for (const pattern of patterns) {
        let match
        while ((match = pattern.exec(file.content)) !== null) {
          // Check if this is inside a hash call
          const surroundingCode = file.content.substring(
            Math.max(0, match.index - 100),
            Math.min(file.content.length, match.index + 100)
          )
          
          if (!surroundingCode.match(/bcrypt|argon2|hash|crypto/)) {
            const lineNumber = file.content.substring(0, match.index).split('\n').length
            
            violations.push({
              ruleId: 'SEC-002',
              severity: RuleSeverity.Critical,
              message: 'Password being stored without hashing',
              location: {
                file: file.path,
                line: lineNumber,
                snippet: this.getCodeSnippet(file.content, match.index)
              },
              fix: {
                description: 'Hash password with bcrypt (cost 12) before storing',
                autoFixable: true,
                fixCode: `const hash = await bcrypt.hash(password, 12)`
              }
            })
          }
        }
      }
    }
    
    return {
      passed: violations.length === 0,
      violations,
      score: violations.length === 0 ? 100 : 0
    }
  },
  
  autoFixable: true,
  fix: (context: CodeContext): FixResult => {
    // Implementation of auto-fix
    return {
      success: true,
      filesChanged: [],
      message: 'Added bcrypt hashing for password storage'
    }
  },
  
  enabled: true,
  tags: ['security', 'owasp-a02', 'authentication', 'passwords', 'critical'],
  links: [
    'https://cheatsheetseries.owasp.org/cheatsheets/Password_Storage_Cheat_Sheet.html'
  ]
}
```

---

#### Rule 3: Function Complexity (Code Quality)
```typescript
const functionComplexityRule: ValidationRule = {
  id: 'CQ-001',
  name: 'Limit Function Complexity',
  category: RuleCategory.CodeQuality,
  severity: RuleSeverity.Medium,
  
  description: 'Functions should not exceed cyclomatic complexity of 15',
  rationale: 'High complexity makes code hard to understand, test, and maintain',
  
  examples: {
    bad: [
      `function processOrder(order) {
        if (order.status === 'pending') {
          if (order.total > 100) {
            if (order.customer.vip) {
              // ... 20 more nested conditions
            }
          }
        }
      }`
    ],
    good: [
      `function processOrder(order) {
        if (!canProcessOrder(order)) return
        applyDiscounts(order)
        calculateShipping(order)
        sendConfirmation(order)
      }`
    ]
  },
  
  check: (context: CodeContext): ValidationResult => {
    const violations: Violation[] = []
    
    for (const file of context.files) {
      if (!file.ast) continue  // Need AST for complexity analysis
      
      const functions = this.extractFunctions(file.ast)
      
      for (const func of functions) {
        const complexity = this.calculateComplexity(func)
        
        if (complexity > 15) {
          violations.push({
            ruleId: 'CQ-001',
            severity: RuleSeverity.Medium,
            message: `Function "${func.name}" has complexity ${complexity} (max: 15)`,
            location: {
              file: file.path,
              line: func.line,
              snippet: func.snippet
            },
            fix: {
              description: 'Break function into smaller functions',
              autoFixable: true
            }
          })
        }
      }
    }
    
    const averageComplexity = this.calculateAverageComplexity(context)
    const score = Math.max(0, 100 - (averageComplexity - 5) * 10)
    
    return {
      passed: violations.length === 0,
      violations,
      score
    }
  },
  
  autoFixable: true,
  fix: (context: CodeContext): FixResult => {
    // Implementation of auto-fix (extract methods)
    return {
      success: true,
      filesChanged: [],
      message: 'Split complex functions into smaller functions'
    }
  },
  
  enabled: true,
  tags: ['code-quality', 'complexity', 'maintainability'],
  links: [
    'https://en.wikipedia.org/wiki/Cyclomatic_complexity'
  ]
}
```

---

## 🔄 VALIDATION PIPELINE

### Pipeline Architecture
```typescript
/**
 * Validation Pipeline
 * Orchestrates validation checks in stages
 */
class ValidationPipeline {
  private rules: Map<string, ValidationRule> = new Map()
  private stages: ValidationStage[] = []
  
  /**
   * Create standard validation pipeline
   */
  static createStandard(): ValidationPipeline {
    const pipeline = new ValidationPipeline()
    
    // Stage 1: Pre-validation (Must Pass)
    pipeline.addStage({
      name: 'Pre-validation',
      description: 'Syntax and type checking',
      stopOnFailure: true,  // Stop if these fail
      rules: [
        'SYNTAX-001',  // Valid syntax
        'TYPE-001',    // Type check passes
        'LINT-001'     // ESLint passes
      ]
    })
    
    // Stage 2: Security Validation (Must Pass)
    pipeline.addStage({
      name: 'Security Validation',
      description: 'OWASP and security checks',
      stopOnFailure: true,  // Stop if critical security issues
      rules: [
        'SEC-001',     // SQL injection
        'SEC-002',     // Plaintext passwords
        'SEC-003',     // XSS vulnerabilities
        'SEC-004',     // CSRF protection
        'SEC-005',     // Authentication
        'SEC-006',     // Authorization
        'SEC-007',     // Sensitive data exposure
        'SEC-008',     // Security misconfig
        'SEC-009',     // Dependency vulnerabilities
        'SEC-010'      // Logging sensitive data
      ]
    })
    
    // Stage 3: Performance Validation (Should Pass)
    pipeline.addStage({
      name: 'Performance Validation',
      description: 'Performance benchmarks',
      stopOnFailure: false,  // Continue even if fails
      rules: [
        'PERF-001',    // Response time
        'PERF-002',    // Memory usage
        'PERF-003',    // N+1 queries
        'PERF-004',    // Slow queries
        'PERF-005',    // Bundle size
        'PERF-006'     // Algorithm efficiency
      ]
    })
    
    // Stage 4: Code Quality Validation (Should Pass)
    pipeline.addStage({
      name: 'Code Quality Validation',
      description: 'Code quality metrics',
      stopOnFailure: false,
      rules: [
        'CQ-001',      // Function complexity
        'CQ-002',      // Code duplication
        'CQ-003',      // Function length
        'CQ-004',      // Nesting depth
        'CQ-005',      // Documentation
        'CQ-006'       // Naming conventions
      ]
    })
    
    // Stage 5: Functionality Validation (Should Pass)
    pipeline.addStage({
      name: 'Functionality Validation',
      description: 'Functional correctness',
      stopOnFailure: false,
      rules: [
        'FUNC-001',    // Error handling
        'FUNC-002',    // Input validation
        'FUNC-003',    // Edge cases
        'FUNC-004'     // Output validation
      ]
    })
    
    // Stage 6: Accessibility Validation (If Applicable)
    pipeline.addStage({
      name: 'Accessibility Validation',
      description: 'WCAG 2.1 AA compliance',
      stopOnFailure: false,
      conditional: (context) => context.projectType === 'web',
      rules: [
        'A11Y-001',    // ARIA labels
        'A11Y-002',    // Keyboard navigation
        'A11Y-003',    // Color contrast
        'A11Y-004',    // Screen reader
        'A11Y-005'     // Focus management
      ]
    })
    
    return pipeline
  }
  
  /**
   * Add validation stage
   */
  addStage(stage: ValidationStage): void {
    this.stages.push(stage)
  }
  
  /**
   * Register validation rule
   */
  registerRule(rule: ValidationRule): void {
    this.rules.set(rule.id, rule)
  }
  
  /**
   * Run validation pipeline
   */
  async validate(context: CodeContext): Promise<PipelineResult> {
    const stageResults: StageResult[] = []
    let overallPassed = true
    
    for (const stage of this.stages) {
      // Check if stage should run
      if (stage.conditional && !stage.conditional(context)) {
        stageResults.push({
          stage: stage.name,
          status: 'skipped',
          message: 'Stage not applicable'
        })
        continue
      }
      
      // Run stage
      const stageResult = await this.runStage(stage, context)
      stageResults.push(stageResult)
      
      // Check if should stop
      if (stage.stopOnFailure && stageResult.status === 'failed') {
        overallPassed = false
        break
      }
      
      if (stageResult.status === 'failed') {
        overallPassed = false
      }
    }
    
    return {
      passed: overallPassed,
      stages: stageResults,
      summary: this.generateSummary(stageResults)
    }
  }
  
  /**
   * Run a single stage
   */
  private async runStage(
    stage: ValidationStage,
    context: CodeContext
  ): Promise<StageResult> {
    const violations: Violation[] = []
    let totalScore = 0
    let maxScore = 0
    
    for (const ruleId of stage.rules) {
      const rule = this.rules.get(ruleId)
      if (!rule || !rule.enabled) continue
      
      const result = rule.check(context)
      
      if (!result.passed && result.violations) {
        violations.push(...result.violations)
      }
      
      if (result.score !== undefined) {
        totalScore += result.score
        maxScore += 100
      }
    }
    
    // Calculate stage score
    const score = maxScore > 0 ? (totalScore / maxScore) * 100 : 100
    
    // Determine status
    let status: 'passed' | 'failed' | 'warning' = 'passed'
    
    const criticalViolations = violations.filter(v => v.severity === RuleSeverity.Critical)
    const highViolations = violations.filter(v => v.severity === RuleSeverity.High)
    
    if (criticalViolations.length > 0) {
      status = 'failed'
    } else if (highViolations.length > 0 || score < 70) {
      status = 'warning'
    }
    
    return {
      stage: stage.name,
      status,
      score,
      violations,
      message: this.formatStageMessage(stage, status, violations)
    }
  }
  
  /**
   * Format stage message
   */
  private formatStageMessage(
    stage: ValidationStage,
    status: string,
    violations: Violation[]
  ): string {
    if (status === 'passed') {
      return `✓ ${stage.name}: All checks passed`
    }
    
    const critical = violations.filter(v => v.severity === RuleSeverity.Critical).length
    const high = violations.filter(v => v.severity === RuleSeverity.High).length
    const medium = violations.filter(v => v.severity === RuleSeverity.Medium).length
    
    const parts: string[] = []
    if (critical > 0) parts.push(`${critical} critical`)
    if (high > 0) parts.push(`${high} high`)
    if (medium > 0) parts.push(`${medium} medium`)
    
    return `✗ ${stage.name}: ${parts.join(', ')} issues found`
  }
  
  /**
   * Generate validation summary
   */
  private generateSummary(stageResults: StageResult[]): ValidationSummary {
    const allViolations = stageResults.flatMap(r => r.violations || [])
    
    const critical = allViolations.filter(v => v.severity === RuleSeverity.Critical).length
    const high = allViolations.filter(v => v.severity === RuleSeverity.High).length
    const medium = allViolations.filter(v => v.severity === RuleSeverity.Medium).length
    const low = allViolations.filter(v => v.severity === RuleSeverity.Low).length
    
    const averageScore = stageResults
      .filter(r => r.score !== undefined)
      .reduce((sum, r) => sum + (r.score || 0), 0) / stageResults.length
    
    return {
      totalViolations: allViolations.length,
      critical,
      high,
      medium,
      low,
      overallScore: Math.round(averageScore),
      recommendation: this.getRecommendation(critical, high, medium, averageScore)
    }
  }
  
  /**
   * Get recommendation based on violations
   */
  private getRecommendation(
    critical: number,
    high: number,
    medium: number,
    score: number
  ): string {
    if (critical > 0) {
      return '🚫 BLOCKED: Critical issues must be fixed before delivery'
    }
    
    if (high > 0) {
      return '⚠️  WARNING: High-priority issues should be addressed'
    }
    
    if (score >= 90) {
      return '✅ EXCELLENT: Code meets all quality standards'
    }
    
    if (score >= 80) {
      return '✓ GOOD: Code is production-ready with minor improvements suggested'
    }
    
    if (score >= 70) {
      return '⚠️  ACCEPTABLE: Code passes but has room for improvement'
    }
    
    return '⚠️  REVIEW NEEDED: Code has quality concerns'
  }
}

/**
 * Supporting Types
 */
interface ValidationStage {
  name: string
  description: string
  stopOnFailure: boolean
  conditional?: (context: CodeContext) => boolean
  rules: string[]  // Rule IDs
}

interface StageResult {
  stage: string
  status: 'passed' | 'failed' | 'warning' | 'skipped'
  score?: number
  violations?: Violation[]
  message: string
}

interface PipelineResult {
  passed: boolean
  stages: StageResult[]
  summary: ValidationSummary
}

interface ValidationSummary {
  totalViolations: number
  critical: number
  high: number
  medium: number
  low: number
  overallScore: number
  recommendation: string
}

interface FixResult {
  success: boolean
  filesChanged: string[]
  message: string
  errors?: string[]
}
```

---

## 🔧 AUTO-FIX ENGINE

### Automated Issue Resolution
```typescript
/**
 * Auto-Fix Engine
 * Attempts to automatically fix validation issues
 */
class AutoFixEngine {
  private rules: Map<string, ValidationRule>
  
  constructor(rules: Map<string, ValidationRule>) {
    this.rules = rules
  }
  
  /**
   * Attempt to fix all auto-fixable violations
   */
  async attemptFixes(
    context: CodeContext,
    violations: Violation[]
  ): Promise<FixResult> {
    const fixableViolations = violations.filter(v => {
      const rule = this.rules.get(v.ruleId)
      return rule?.autoFixable === true
    })
    
    if (fixableViolations.length === 0) {
      return {
        success: false,
        filesChanged: [],
        message: 'No auto-fixable violations found'
      }
    }
    
    const filesChanged = new Set<string>()
    const fixResults: Array<{ violation: Violation, success: boolean }> = []
    
    for (const violation of fixableViolations) {
      const rule = this.rules.get(violation.ruleId)
      if (!rule || !rule.fix) continue
      
      try {
        const result = rule.fix(context)
        
        if (result.success) {
          fixResults.push({ violation, success: true })
          result.filesChanged.forEach(f => filesChanged.add(f))
        } else {
          fixResults.push({ violation, success: false })
        }
      } catch (error) {
        fixResults.push({ violation, success: false })
      }
    }
    
    const successCount = fixResults.filter(r => r.success).length
    
    return {
      success: successCount > 0,
      filesChanged: Array.from(filesChanged),
      message: `Fixed ${successCount}/${fixableViolations.length} issues`,
      errors: fixResults
        .filter(r => !r.success)
        .map(r => `Failed to fix: ${r.violation.message}`)
    }
  }
}
```

---

## 📊 VALIDATION REPORT GENERATOR

### Creating Quality Reports
```typescript
/**
 * Validation Report Generator
 * Creates formatted quality reports for users
 */
class ValidationReportGenerator {
  /**
   * Generate comprehensive validation report
   */
  generateReport(result: PipelineResult, context: CodeContext): string {
    let report = ''
    
    // Header
    report += this.formatHeader(context)
    report += '\n\n'
    
    // Overall Score
    report += this.formatOverallScore(result.summary)
    report += '\n\n'
    
    // Stage Results
    report += this.formatStages(result.stages)
    report += '\n\n'
    
    // Violations Details (if any)
    if (result.summary.totalViolations > 0) {
      report += this.formatViolations(result.stages)
      report += '\n\n'
    }
    
    // Recommendation
    report += this.formatRecommendation(result.summary)
    
    return report
  }
  
  /**
   * Format header
   */
  private formatHeader(context: CodeContext): string {
    const fileCount = context.files.length
    const totalLines = context.files.reduce((sum, f) => 
      sum + f.content.split('\n').length, 0
    )
    
    return `
╔═══════════════════════════════════════════════════════════╗
║              QUALITY VALIDATION REPORT                     ║
╚═══════════════════════════════════════════════════════════╝

Project Type: ${context.projectType}
Framework: ${context.framework || 'N/A'}
Language: ${context.language}
Files Validated: ${fileCount}
Lines of Code: ${totalLines.toLocaleString()}
`.trim()
  }
  
  /**
   * Format overall score
   */
  private formatOverallScore(summary: ValidationSummary): string {
    const stars = this.getStars(summary.overallScore)
    const status = this.getStatus(summary)
    
    return `
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

OVERALL QUALITY SCORE: ${summary.overallScore}/100 ${stars}

Status: ${status}

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
`.trim()
  }
  
  /**
   * Format stages
   */
  private formatStages(stages: StageResult[]): string {
    let output = 'DETAILED SCORES:\n\n'
    
    for (const stage of stages) {
      if (stage.status === 'skipped') {
        output += `⊘ ${stage.stage}: SKIPPED\n\n`
        continue
      }
      
      const icon = stage.status === 'passed' ? '✓' : 
                   stage.status === 'warning' ? '⚠' : '✗'
      const statusText = stage.status.toUpperCase()
      const scoreText = stage.score !== undefined ? 
        ` ${Math.round(stage.score)}/100` : ''
      
      output += `${icon} ${stage.stage}${scoreText} ${statusText}\n`
      
      if (stage.violations && stage.violations.length > 0) {
        const critical = stage.violations.filter(v => v.severity === RuleSeverity.Critical).length
        const high = stage.violations.filter(v => v.severity === RuleSeverity.High).length
        const medium = stage.violations.filter(v => v.severity === RuleSeverity.Medium).length
        
        if (critical > 0) output += `  🚨 ${critical} critical\n`
        if (high > 0) output += `  ⚠️  ${high} high\n`
        if (medium > 0) output += `  ⚡ ${medium} medium\n`
      }
      
      output += '\n'
    }
    
    return output
  }
  
  /**
   * Format violations
   */
  private formatViolations(stages: StageResult[]): string {
    let output = 'VIOLATIONS:\n\n'
    
    const allViolations = stages.flatMap(s => s.violations || [])
    const criticalViolations = allViolations.filter(v => v.severity === RuleSeverity.Critical)
    const highViolations = allViolations.filter(v => v.severity === RuleSeverity.High)
    
    // Show critical first
    if (criticalViolations.length > 0) {
      output += '🚨 CRITICAL ISSUES:\n'
      criticalViolations.forEach((v, i) => {
        output += `\n${i + 1}. ${v.message}\n`
        output += `   File: ${v.location.file}:${v.location.line || '?'}\n`
        if (v.fix) {
          output += `   Fix: ${v.fix.description}\n`
        }
      })
      output += '\n'
    }
    
    // Show high
    if (highViolations.length > 0) {
      output += '⚠️  HIGH PRIORITY ISSUES:\n'
      highViolations.forEach((v, i) => {
        output += `\n${i + 1}. ${v.message}\n`
        output += `   File: ${v.location.file}:${v.location.line || '?'}\n`
        if (v.fix) {
          output += `   Fix: ${v.fix.description}\n`
        }
      })
    }
    
    return output
  }
  
  /**
   * Format recommendation
   */
  private formatRecommendation(summary: ValidationSummary): string {
    return `
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

RECOMMENDATION:

${summary.recommendation}

${this.getActionItems(summary)}

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
`.trim()
  }
  
  /**
   * Get star rating
   */
  private getStars(score: number): string {
    if (score >= 90) return '⭐⭐⭐⭐⭐'
    if (score >= 80) return '⭐⭐⭐⭐'
    if (score >= 70) return '⭐⭐⭐'
    if (score >= 60) return '⭐⭐'
    return '⭐'
  }
  
  /**
   * Get status text
   */
  private getStatus(summary: ValidationSummary): string {
    if (summary.critical > 0) {
      return '🚫 BLOCKED (Critical issues must be fixed)'
    }
    
    if (summary.overallScore >= 90) {
      return '✅ PASSED (Production-Ready)'
    }
    
    if (summary.overallScore >= 80) {
      return '✅ PASSED (Minor Improvements Suggested)'
    }
    
    if (summary.overallScore >= 70) {
      return '⚠️  WARNING (Acceptable with Improvements)'
    }
    
    return '⚠️  WARNING (Quality Concerns)'
  }
  
  /**
   * Get action items
   */
  private getActionItems(summary: ValidationSummary): string {
    const items: string[] = []
    
    if (summary.critical > 0) {
      items.push(`1. Fix ${summary.critical} critical security issue(s) immediately`)
    }
    
    if (summary.high > 0) {
      items.push(`${items.length + 1}. Address ${summary.high} high-priority issue(s)`)
    }
    
    if (summary.medium > 0) {
      items.push(`${items.length + 1}. Consider fixing ${summary.medium} medium-priority issue(s)`)
    }
    
    if (items.length === 0) {
      return 'No action required - code meets all quality standards! 🎉'
    }
    
    return 'Action Items:\n' + items.join('\n')
  }
}
```

---

## 📚 RELATED ARTICLES

| Article | Purpose | Relationship to Validation Framework |
|---------|---------|-------------------------------------|
| **Article II: Security Validation** | Security rules | Implements security validation rules |
| **Article III: Performance Validation** | Performance rules | Implements performance validation rules |
| **Article IV: Code Quality Validation** | Quality rules | Implements quality validation rules |
| **Segment 4, Article I** | Task Orchestration | Validation is a task in execution |

---

**Previous:** [← Segment 5 README](./README.md)  
**Next:** [Article II: Security Validation →](./02-article-ii-security-validation.md)

---

**Last Updated:** February 7, 2026  
**Constitutional Version:** 2.0.0  
**Status:** ✅ Ratified and In Force

**Motto:** *"Systematic Validation - Every Rule Has a Purpose, Every Check Has a Reason"*
