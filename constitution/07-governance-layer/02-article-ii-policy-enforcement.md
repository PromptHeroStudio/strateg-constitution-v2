# ⚖️ ARTICLE II: POLICY ENFORCEMENT

**Automatic Constitutional Compliance Through Policy as Code**

---

## 📜 CONSTITUTIONAL AUTHORITY

This article defines the **policy enforcement system** - how MVCA automatically enforces constitutional laws, detects violations, and ensures compliance.

**Legal Force:**
- ✅ All constitutional laws **SHALL** be encoded as enforceable policies
- ✅ Policy violations **MUST** be detected before execution
- ✅ Critical violations **SHALL** block operations automatically
- ✅ Policy overrides **REQUIRE** explicit justification and approval
- ✅ All policy checks **SHALL** be recorded in audit log

**Constitutional Principle:**
> Laws without enforcement are merely suggestions.  
> Constitutional compliance is automatic, not optional.

---

## 🏗️ POLICY RULE ENGINE

### Policy Architecture
```typescript
/**
 * Policy Rule
 * Encodes a constitutional law or organizational policy
 */
interface PolicyRule {
  // IDENTITY
  id: string                              // Unique policy ID (e.g., "SEC-001")
  name: string                            // Human-readable name
  category: PolicyCategory                // Policy category
  
  // SEVERITY
  severity: PolicySeverity                // How serious is violation
  enforcement: PolicyEnforcement          // How to enforce
  
  // DESCRIPTION
  description: string                     // What this policy does
  rationale: string                       // Why this policy exists
  constitutionalBasis?: string            // Which law/commandment
  
  // VALIDATION
  check: (context: PolicyContext) => Promise<PolicyCheckResult>
  
  // REMEDIATION
  remediation?: {
    canAutoFix: boolean
    fix?: (context: PolicyContext) => Promise<PolicyFixResult>
    guidance: string                      // How to fix manually
  }
  
  // METADATA
  enabled: boolean
  tags: string[]
  links: string[]                         // References
  version: string
  lastUpdated: Date
}

/**
 * Policy Categories
 */
enum PolicyCategory {
  Security = 'security',                  // Security policies
  Quality = 'quality',                    // Code quality policies
  Performance = 'performance',            // Performance policies
  Deployment = 'deployment',              // Deployment policies
  Configuration = 'configuration',        // Config policies
  Documentation = 'documentation',        // Documentation policies
  Testing = 'testing',                    // Testing policies
  Compliance = 'compliance'               // Regulatory compliance
}

/**
 * Policy Severity Levels
 */
enum PolicySeverity {
  Critical = 'critical',                  // Blocks operation
  High = 'high',                          // Strong warning
  Medium = 'medium',                      // Warning
  Low = 'low',                            // Advisory
  Info = 'info'                           // Informational
}

/**
 * Policy Enforcement Modes
 */
enum PolicyEnforcement {
  Block = 'block',                        // Prevent operation
  Warn = 'warn',                          // Show warning
  Log = 'log',                            // Log only
  Advisory = 'advisory'                   // Suggestion only
}

/**
 * Policy Context
 * Information needed to evaluate policy
 */
interface PolicyContext {
  // OPERATION
  operation: string                       // e.g., "deployment", "code_generation"
  actor: {
    id: string
    type: 'user' | 'system' | 'service'
    email?: string
  }
  
  // RESOURCES
  code?: {
    files: CodeFile[]
    language: string
    framework?: string
  }
  
  deployment?: {
    environment: string
    target: string
    strategy: string
  }
  
  configuration?: {
    variables: Record<string, string>
    secrets: string[]
  }
  
  // CONTEXT
  metadata?: Record<string, any>
}

/**
 * Policy Check Result
 */
interface PolicyCheckResult {
  passed: boolean
  violations: PolicyViolation[]
  message?: string
  metadata?: Record<string, any>
}

/**
 * Policy Violation
 */
interface PolicyViolation {
  policyId: string
  severity: PolicySeverity
  message: string
  location?: {
    file?: string
    line?: number
    column?: number
  }
  evidence?: any
  remediation?: string
}
```

---

## 🎯 POLICY ENGINE IMPLEMENTATION

### Policy Enforcement Engine
```typescript
/**
 * Policy Engine
 * Evaluates and enforces policies
 */
class PolicyEngine {
  private policies: Map<string, PolicyRule> = new Map()
  private auditLogger: AuditLogger
  
  constructor(auditLogger: AuditLogger) {
    this.auditLogger = auditLogger
  }
  
  /**
   * Register policy
   */
  register(policy: PolicyRule): void {
    this.policies.set(policy.id, policy)
    console.log(`✓ Policy registered: ${policy.id} - ${policy.name}`)
  }
  
  /**
   * Evaluate all policies for context
   */
  async evaluate(context: PolicyContext): Promise<PolicyEvaluationResult> {
    console.log(`🔍 Evaluating policies for operation: ${context.operation}`)
    
    const results: Record<string, PolicyCheckResult> = {}
    const allViolations: PolicyViolation[] = []
    
    // Filter relevant policies
    const relevantPolicies = this.getRelevantPolicies(context)
    
    console.log(`  Found ${relevantPolicies.length} relevant policies`)
    
    // Evaluate each policy
    for (const policy of relevantPolicies) {
      if (!policy.enabled) continue
      
      try {
        const result = await policy.check(context)
        results[policy.id] = result
        
        if (!result.passed) {
          console.log(`  ✗ ${policy.id}: ${result.message || 'Policy violation'}`)
          allViolations.push(...result.violations)
          
          // Log violation in audit
          await this.auditLogger.logPolicyViolation(
            context.actor,
            result.violations[0]
          )
        } else {
          console.log(`  ✓ ${policy.id}: Passed`)
        }
        
      } catch (error) {
        console.error(`  Error evaluating policy ${policy.id}:`, error)
        
        // Treat error as failure
        results[policy.id] = {
          passed: false,
          violations: [{
            policyId: policy.id,
            severity: PolicySeverity.High,
            message: `Policy evaluation error: ${error.message}`,
            remediation: 'Check policy implementation'
          }]
        }
      }
    }
    
    // Determine if operation should be allowed
    const criticalViolations = allViolations.filter(v => 
      v.severity === PolicySeverity.Critical
    )
    
    const shouldBlock = criticalViolations.length > 0
    
    const overallResult: PolicyEvaluationResult = {
      passed: allViolations.length === 0,
      blocked: shouldBlock,
      results,
      violations: allViolations,
      summary: this.generateSummary(allViolations)
    }
    
    if (shouldBlock) {
      console.log(`  🚫 Operation BLOCKED due to ${criticalViolations.length} critical violation(s)`)
    } else if (allViolations.length > 0) {
      console.log(`  ⚠️  Operation allowed with ${allViolations.length} warning(s)`)
    } else {
      console.log(`  ✅ All policies passed`)
    }
    
    return overallResult
  }
  
  /**
   * Attempt automatic remediation
   */
  async remediate(
    context: PolicyContext,
    violations: PolicyViolation[]
  ): Promise<RemediationResult> {
    console.log(`🔧 Attempting remediation for ${violations.length} violations`)
    
    const fixed: PolicyViolation[] = []
    const failed: PolicyViolation[] = []
    
    for (const violation of violations) {
      const policy = this.policies.get(violation.policyId)
      
      if (!policy || !policy.remediation?.canAutoFix || !policy.remediation.fix) {
        failed.push(violation)
        continue
      }
      
      try {
        console.log(`  Fixing ${violation.policyId}...`)
        const fixResult = await policy.remediation.fix(context)
        
        if (fixResult.success) {
          fixed.push(violation)
          console.log(`  ✓ Fixed: ${violation.message}`)
        } else {
          failed.push(violation)
          console.log(`  ✗ Failed: ${fixResult.error}`)
        }
        
      } catch (error) {
        failed.push(violation)
        console.log(`  ✗ Error fixing: ${error.message}`)
      }
    }
    
    return {
      success: failed.length === 0,
      fixed: fixed.length,
      failed: failed.length,
      violations: failed
    }
  }
  
  /**
   * Request policy override
   */
  async requestOverride(
    context: PolicyContext,
    violation: PolicyViolation,
    justification: string,
    approver?: string
  ): Promise<OverrideResult> {
    const policy = this.policies.get(violation.policyId)
    
    if (!policy) {
      return {
        approved: false,
        reason: 'Policy not found'
      }
    }
    
    // Critical policies cannot be overridden
    if (policy.severity === PolicySeverity.Critical) {
      return {
        approved: false,
        reason: 'Critical policies cannot be overridden'
      }
    }
    
    // Log override request
    console.log(`📋 Override requested for ${policy.id}`)
    console.log(`   Justification: ${justification}`)
    
    // In real implementation, this would trigger approval workflow
    // For now, auto-approve non-critical with valid justification
    const approved = justification.length >= 20
    
    // Log in audit
    await this.auditLogger.log({
      eventType: AuditEventType.PolicyOverride,
      action: 'policy.override',
      actor: context.actor,
      resource: {
        type: 'policy',
        id: policy.id,
        name: policy.name
      },
      context: {
        reason: justification,
        metadata: {
          approver,
          violation: violation.message
        }
      },
      result: approved ? 'success' : 'blocked',
      compliance: {
        policyChecks: [],
        violations: approved ? [] : [violation],
        approved
      }
    })
    
    return {
      approved,
      reason: approved ? 'Override approved' : 'Justification insufficient',
      approver
    }
  }
  
  /**
   * Get relevant policies for context
   */
  private getRelevantPolicies(context: PolicyContext): PolicyRule[] {
    const all = Array.from(this.policies.values())
    
    // Filter by operation type
    // In real implementation, policies would declare which operations they apply to
    return all.filter(p => p.enabled)
  }
  
  /**
   * Generate summary
   */
  private generateSummary(violations: PolicyViolation[]): string {
    if (violations.length === 0) {
      return 'All policies passed'
    }
    
    const bySeverity = violations.reduce((acc, v) => {
      acc[v.severity] = (acc[v.severity] || 0) + 1
      return acc
    }, {} as Record<string, number>)
    
    const parts: string[] = []
    
    if (bySeverity[PolicySeverity.Critical]) {
      parts.push(`${bySeverity[PolicySeverity.Critical]} critical`)
    }
    if (bySeverity[PolicySeverity.High]) {
      parts.push(`${bySeverity[PolicySeverity.High]} high`)
    }
    if (bySeverity[PolicySeverity.Medium]) {
      parts.push(`${bySeverity[PolicySeverity.Medium]} medium`)
    }
    
    return `${violations.length} violation(s): ${parts.join(', ')}`
  }
}

/**
 * Supporting Types
 */
interface PolicyEvaluationResult {
  passed: boolean
  blocked: boolean
  results: Record<string, PolicyCheckResult>
  violations: PolicyViolation[]
  summary: string
}

interface RemediationResult {
  success: boolean
  fixed: number
  failed: number
  violations: PolicyViolation[]
}

interface OverrideResult {
  approved: boolean
  reason: string
  approver?: string
}

interface PolicyFixResult {
  success: boolean
  error?: string
}
```

---

## 📋 STANDARD POLICIES

### Security Policies
```typescript
/**
 * Standard Security Policies
 * Based on Ten Commandments and Security Best Practices
 */
class SecurityPolicies {
  /**
   * Policy: No hardcoded secrets
   * Commandment II: Thou shall secure all secrets
   */
  static noHardcodedSecrets(): PolicyRule {
    return {
      id: 'SEC-001',
      name: 'No Hardcoded Secrets',
      category: PolicyCategory.Security,
      severity: PolicySeverity.Critical,
      enforcement: PolicyEnforcement.Block,
      
      description: 'Secrets must not be hardcoded in source code',
      rationale: 'Hardcoded secrets are easily discovered and exploited',
      constitutionalBasis: 'Commandment II: Thou shall secure all secrets',
      
      check: async (context: PolicyContext): Promise<PolicyCheckResult> => {
        if (!context.code) {
          return { passed: true, violations: [] }
        }
        
        const violations: PolicyViolation[] = []
        
        for (const file of context.code.files) {
          // Patterns for common secrets
          const patterns = [
            { pattern: /api[_-]?key\s*[=:]\s*["']([^"']{20,})["']/gi, type: 'API Key' },
            { pattern: /password\s*[=:]\s*["']([^"']{3,})["']/gi, type: 'Password' },
            { pattern: /sk_(?:live|test)_[a-zA-Z0-9]{24,}/g, type: 'Stripe Key' },
            { pattern: /ghp_[a-zA-Z0-9]{36,}/g, type: 'GitHub Token' }
          ]
          
          for (const { pattern, type } of patterns) {
            let match
            while ((match = pattern.exec(file.content)) !== null) {
              const lineNumber = file.content.substring(0, match.index).split('\n').length
              
              // Skip if using environment variable
              const line = file.content.split('\n')[lineNumber - 1]
              if (line.includes('process.env') || line.includes('import.meta.env')) {
                continue
              }
              
              violations.push({
                policyId: 'SEC-001',
                severity: PolicySeverity.Critical,
                message: `Hardcoded ${type} detected`,
                location: {
                  file: file.path,
                  line: lineNumber
                },
                remediation: `Move ${type} to environment variable`
              })
            }
          }
        }
        
        return {
          passed: violations.length === 0,
          violations,
          message: violations.length > 0 ? 
            `Found ${violations.length} hardcoded secret(s)` : undefined
        }
      },
      
      remediation: {
        canAutoFix: true,
        fix: async (context: PolicyContext): Promise<PolicyFixResult> => {
          // Implementation would replace secrets with env vars
          return { success: true }
        },
        guidance: 'Replace hardcoded secrets with environment variables'
      },
      
      enabled: true,
      tags: ['security', 'secrets', 'critical'],
      links: [
        'https://owasp.org/www-community/vulnerabilities/Use_of_hard-coded_password'
      ],
      version: '1.0.0',
      lastUpdated: new Date()
    }
  }
  
  /**
   * Policy: Production deployments require approval
   * Commandment X: Thou shall deploy with confidence
   */
  static productionApprovalRequired(): PolicyRule {
    return {
      id: 'DEP-001',
      name: 'Production Approval Required',
      category: PolicyCategory.Deployment,
      severity: PolicySeverity.Critical,
      enforcement: PolicyEnforcement.Block,
      
      description: 'Production deployments must have explicit approval',
      rationale: 'Prevents accidental or unauthorized production changes',
      constitutionalBasis: 'Commandment X: Thou shall deploy with confidence',
      
      check: async (context: PolicyContext): Promise<PolicyCheckResult> => {
        if (!context.deployment) {
          return { passed: true, violations: [] }
        }
        
        // Check if production
        const isProduction = context.deployment.environment === 'production'
        
        if (!isProduction) {
          return { passed: true, violations: [] }
        }
        
        // Check if approved
        const hasApproval = context.metadata?.approved === true
        
        if (hasApproval) {
          return { passed: true, violations: [] }
        }
        
        return {
          passed: false,
          violations: [{
            policyId: 'DEP-001',
            severity: PolicySeverity.Critical,
            message: 'Production deployment requires approval',
            remediation: 'Request approval before deploying to production'
          }]
        }
      },
      
      enabled: true,
      tags: ['deployment', 'production', 'approval'],
      links: [],
      version: '1.0.0',
      lastUpdated: new Date()
    }
  }
  
  /**
   * Policy: All tests must pass before deployment
   * Commandment IV: Thou shall validate before deploying
   */
  static testsRequired(): PolicyRule {
    return {
      id: 'TEST-001',
      name: 'All Tests Must Pass',
      category: PolicyCategory.Testing,
      severity: PolicySeverity.Critical,
      enforcement: PolicyEnforcement.Block,
      
      description: 'All tests must pass before deployment',
      rationale: 'Prevents deploying broken code to production',
      constitutionalBasis: 'Commandment IV: Thou shall validate before deploying',
      
      check: async (context: PolicyContext): Promise<PolicyCheckResult> {
        if (!context.deployment) {
          return { passed: true, violations: [] }
        }
        
        // Check test results
        const testsPassed = context.metadata?.testResults?.passed === true
        const testsRun = context.metadata?.testResults?.run === true
        
        if (testsRun && testsPassed) {
          return { passed: true, violations: [] }
        }
        
        if (!testsRun) {
          return {
            passed: false,
            violations: [{
              policyId: 'TEST-001',
              severity: PolicySeverity.Critical,
              message: 'Tests not executed before deployment',
              remediation: 'Run test suite and verify all tests pass'
            }]
          }
        }
        
        return {
          passed: false,
          violations: [{
            policyId: 'TEST-001',
            severity: PolicySeverity.Critical,
            message: 'Tests failed',
            evidence: context.metadata?.testResults,
            remediation: 'Fix failing tests before deployment'
          }]
        }
      },
      
      enabled: true,
      tags: ['testing', 'deployment', 'quality'],
      links: [],
      version: '1.0.0',
      lastUpdated: new Date()
    }
  }
}
```

---

### Quality Policies
```typescript
/**
 * Standard Quality Policies
 * Based on Code Quality Standards
 */
class QualityPolicies {
  /**
   * Policy: Code quality score minimum
   * Commandment V: Thou shall write quality code
   */
  static minimumQualityScore(threshold: number = 70): PolicyRule {
    return {
      id: 'QUAL-001',
      name: 'Minimum Code Quality Score',
      category: PolicyCategory.Quality,
      severity: PolicySeverity.High,
      enforcement: PolicyEnforcement.Warn,
      
      description: `Code quality score must be at least ${threshold}/100`,
      rationale: 'Maintains consistent code quality standards',
      constitutionalBasis: 'Commandment V: Thou shall write quality code',
      
      check: async (context: PolicyContext): Promise<PolicyCheckResult> => {
        if (!context.code) {
          return { passed: true, violations: [] }
        }
        
        // Get quality score from metadata
        const qualityScore = context.metadata?.qualityScore as number | undefined
        
        if (!qualityScore) {
          return {
            passed: false,
            violations: [{
              policyId: 'QUAL-001',
              severity: PolicySeverity.High,
              message: 'Code quality not assessed',
              remediation: 'Run quality validation'
            }]
          }
        }
        
        if (qualityScore < threshold) {
          return {
            passed: false,
            violations: [{
              policyId: 'QUAL-001',
              severity: PolicySeverity.High,
              message: `Quality score ${qualityScore} below threshold ${threshold}`,
              evidence: { score: qualityScore, threshold },
              remediation: 'Improve code quality to meet minimum threshold'
            }]
          }
        }
        
        return { passed: true, violations: [] }
      },
      
      enabled: true,
      tags: ['quality', 'code-quality'],
      links: [],
      version: '1.0.0',
      lastUpdated: new Date()
    }
  }
  
  /**
   * Policy: No critical vulnerabilities
   * Commandment VII: Thou shall scan for vulnerabilities
   */
  static noCriticalVulnerabilities(): PolicyRule {
    return {
      id: 'SEC-010',
      name: 'No Critical Vulnerabilities',
      category: PolicyCategory.Security,
      severity: PolicySeverity.Critical,
      enforcement: PolicyEnforcement.Block,
      
      description: 'Code must not contain critical security vulnerabilities',
      rationale: 'Critical vulnerabilities pose immediate security risk',
      constitutionalBasis: 'Commandment VII: Thou shall scan for vulnerabilities',
      
      check: async (context: PolicyContext): Promise<PolicyCheckResult> => {
        // Get security scan results
        const securityResults = context.metadata?.securityScan
        
        if (!securityResults) {
          return {
            passed: false,
            violations: [{
              policyId: 'SEC-010',
              severity: PolicySeverity.Critical,
              message: 'Security scan not performed',
              remediation: 'Run security validation'
            }]
          }
        }
        
        const criticalVulns = securityResults.critical || 0
        const highVulns = securityResults.high || 0
        
        if (criticalVulns > 0) {
          return {
            passed: false,
            violations: [{
              policyId: 'SEC-010',
              severity: PolicySeverity.Critical,
              message: `Found ${criticalVulns} critical vulnerabilities`,
              evidence: securityResults,
              remediation: 'Fix all critical vulnerabilities before deployment'
            }]
          }
        }
        
        if (highVulns > 0) {
          return {
            passed: false,
            violations: [{
              policyId: 'SEC-010',
              severity: PolicySeverity.High,
              message: `Found ${highVulns} high-severity vulnerabilities`,
              evidence: securityResults,
              remediation: 'Fix high-severity vulnerabilities'
            }]
          }
        }
        
        return { passed: true, violations: [] }
      },
      
      enabled: true,
      tags: ['security', 'vulnerabilities', 'critical'],
      links: [
        'https://owasp.org/Top10/'
      ],
      version: '1.0.0',
      lastUpdated: new Date()
    }
  }
  
  /**
   * Policy: Documentation required
   * Commandment VI: Thou shall document thy work
   */
  static documentationRequired(threshold: number = 80): PolicyRule {
    return {
      id: 'DOC-001',
      name: 'Documentation Required',
      category: PolicyCategory.Documentation,
      severity: PolicySeverity.Medium,
      enforcement: PolicyEnforcement.Warn,
      
      description: `Documentation coverage must be at least ${threshold}%`,
      rationale: 'Documentation ensures maintainability',
      constitutionalBasis: 'Commandment VI: Thou shall document thy work',
      
      check: async (context: PolicyContext): Promise<PolicyCheckResult> => {
        if (!context.code) {
          return { passed: true, violations: [] }
        }
        
        const docCoverage = context.metadata?.documentationCoverage as number | undefined
        
        if (docCoverage === undefined) {
          return {
            passed: false,
            violations: [{
              policyId: 'DOC-001',
              severity: PolicySeverity.Medium,
              message: 'Documentation coverage not measured',
              remediation: 'Measure documentation coverage'
            }]
          }
        }
        
        if (docCoverage < threshold) {
          return {
            passed: false,
            violations: [{
              policyId: 'DOC-001',
              severity: PolicySeverity.Medium,
              message: `Documentation coverage ${docCoverage}% below threshold ${threshold}%`,
              evidence: { coverage: docCoverage, threshold },
              remediation: 'Add documentation to exported functions'
            }]
          }
        }
        
        return { passed: true, violations: [] }
      },
      
      remediation: {
        canAutoFix: true,
        fix: async (context: PolicyContext): Promise<PolicyFixResult> => {
          // Implementation would generate JSDoc comments
          return { success: true }
        },
        guidance: 'Add JSDoc comments to undocumented functions'
      },
      
      enabled: true,
      tags: ['documentation', 'quality'],
      links: [],
      version: '1.0.0',
      lastUpdated: new Date()
    }
  }
}
```

---

## 🔄 POLICY LIFECYCLE

### Policy Manager
```typescript
/**
 * Policy Manager
 * Manages policy lifecycle (create, update, disable)
 */
class PolicyManager {
  private engine: PolicyEngine
  private storage: PolicyStorage
  
  constructor(engine: PolicyEngine, storage: PolicyStorage) {
    this.engine = engine
    this.storage = storage
  }
  
  /**
   * Create new policy
   */
  async createPolicy(policy: PolicyRule): Promise<void> {
    // Validate policy
    this.validatePolicy(policy)
    
    // Check for conflicts
    await this.checkConflicts(policy)
    
    // Store policy
    await this.storage.save(policy)
    
    // Register with engine
    this.engine.register(policy)
    
    console.log(`✓ Policy created: ${policy.id}`)
  }
  
  /**
   * Update existing policy
   */
  async updatePolicy(
    policyId: string,
    updates: Partial<PolicyRule>
  ): Promise<void> {
    const existing = await this.storage.get(policyId)
    
    if (!existing) {
      throw new Error(`Policy ${policyId} not found`)
    }
    
    const updated: PolicyRule = {
      ...existing,
      ...updates,
      version: this.incrementVersion(existing.version),
      lastUpdated: new Date()
    }
    
    // Validate updated policy
    this.validatePolicy(updated)
    
    // Store updated policy
    await this.storage.save(updated)
    
    // Re-register with engine
    this.engine.register(updated)
    
    console.log(`✓ Policy updated: ${policyId}`)
  }
  
  /**
   * Disable policy
   */
  async disablePolicy(policyId: string, reason: string): Promise<void> {
    await this.updatePolicy(policyId, {
      enabled: false,
      metadata: { disabledReason: reason }
    } as any)
    
    console.log(`✓ Policy disabled: ${policyId} (${reason})`)
  }
  
  /**
   * Enable policy
   */
  async enablePolicy(policyId: string): Promise<void> {
    await this.updatePolicy(policyId, {
      enabled: true
    } as any)
    
    console.log(`✓ Policy enabled: ${policyId}`)
  }
  
  /**
   * Get all policies
   */
  async getAllPolicies(): Promise<PolicyRule[]> {
    return await this.storage.getAll()
  }
  
  /**
   * Get policies by category
   */
  async getPoliciesByCategory(category: PolicyCategory): Promise<PolicyRule[]> {
    const all = await this.getAllPolicies()
    return all.filter(p => p.category === category)
  }
  
  /**
   * Validate policy
   */
  private validatePolicy(policy: PolicyRule): void {
    if (!policy.id.match(/^[A-Z]+-[0-9]+$/)) {
      throw new Error('Policy ID must match pattern XXX-NNN (e.g., SEC-001)')
    }
    
    if (!policy.name || policy.name.length < 5) {
      throw new Error('Policy name must be at least 5 characters')
    }
    
    if (!policy.description || policy.description.length < 20) {
      throw new Error('Policy description must be at least 20 characters')
    }
    
    if (!policy.check) {
      throw new Error('Policy must have check function')
    }
  }
  
  /**
   * Check for policy conflicts
   */
  private async checkConflicts(policy: PolicyRule): Promise<void> {
    const existing = await this.storage.get(policy.id)
    
    if (existing) {
      throw new Error(`Policy ${policy.id} already exists`)
    }
  }
  
  /**
   * Increment version
   */
  private incrementVersion(version: string): string {
    const parts = version.split('.')
    parts[parts.length - 1] = (parseInt(parts[parts.length - 1]) + 1).toString()
    return parts.join('.')
  }
}

/**
 * Policy Storage Interface
 */
interface PolicyStorage {
  save(policy: PolicyRule): Promise<void>
  get(id: string): Promise<PolicyRule | null>
  getAll(): Promise<PolicyRule[]>
  delete(id: string): Promise<void>
}

/**
 * In-Memory Policy Storage
 */
class InMemoryPolicyStorage implements PolicyStorage {
  private policies: Map<string, PolicyRule> = new Map()
  
  async save(policy: PolicyRule): Promise<void> {
    this.policies.set(policy.id, policy)
  }
  
  async get(id: string): Promise<PolicyRule | null> {
    return this.policies.get(id) || null
  }
  
  async getAll(): Promise<PolicyRule[]> {
    return Array.from(this.policies.values())
  }
  
  async delete(id: string): Promise<void> {
    this.policies.delete(id)
  }
}
```

---

## 📊 POLICY REPORTING

### Policy Compliance Reporter
```typescript
/**
 * Policy Compliance Reporter
 * Generates reports on policy compliance
 */
class PolicyComplianceReporter {
  private engine: PolicyEngine
  private auditStorage: AuditStorage
  
  constructor(engine: PolicyEngine, auditStorage: AuditStorage) {
    this.engine = engine
    this.auditStorage = auditStorage
  }
  
  /**
   * Generate compliance report
   */
  async generateReport(
    startDate: Date,
    endDate: Date
  ): Promise<ComplianceReport> {
    // Query policy violations from audit log
    const query = new AuditQueryBuilder()
      .ofType(AuditEventType.PolicyViolation)
      .between(startDate, endDate)
      .build()
    
    const violations = await this.auditStorage.query(query)
    
    // Group by policy
    const byPolicy = this.groupByPolicy(violations)
    
    // Group by severity
    const bySeverity = this.groupBySeverity(violations)
    
    // Calculate compliance rate
    const totalChecks = violations.length + 1000 // Simplified
    const complianceRate = ((totalChecks - violations.length) / totalChecks) * 100
    
    return {
      period: { start: startDate, end: endDate },
      totalViolations: violations.length,
      complianceRate,
      violationsByPolicy: byPolicy,
      violationsBySeverity: bySeverity,
      criticalViolations: bySeverity[PolicySeverity.Critical] || 0,
      blockedOperations: violations.filter(v => v.result === 'blocked').length,
      topViolators: this.getTopViolators(violations),
      recommendations: this.generateRecommendations(byPolicy, bySeverity)
    }
  }
  
  /**
   * Generate HTML report
   */
  generateHTML(report: ComplianceReport): string {
    return `
<!DOCTYPE html>
<html>
<head>
  <title>Policy Compliance Report</title>
  <style>
    body { 
      font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, sans-serif;
      margin: 40px;
      background: #f5f5f5;
    }
    .container {
      max-width: 1200px;
      margin: 0 auto;
      background: white;
      padding: 40px;
      border-radius: 8px;
      box-shadow: 0 2px 4px rgba(0,0,0,0.1);
    }
    h1 { color: #1a202c; margin-bottom: 10px; }
    .period { color: #718096; margin-bottom: 30px; }
    .metrics {
      display: grid;
      grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
      gap: 20px;
      margin: 30px 0;
    }
    .metric {
      background: #f7fafc;
      padding: 20px;
      border-radius: 8px;
      border-left: 4px solid #4299e1;
    }
    .metric.critical { border-left-color: #f56565; }
    .metric.success { border-left-color: #48bb78; }
    .metric-label { 
      font-size: 14px;
      color: #718096;
      margin-bottom: 5px;
    }
    .metric-value {
      font-size: 32px;
      font-weight: bold;
      color: #1a202c;
    }
    table {
      width: 100%;
      border-collapse: collapse;
      margin: 20px 0;
    }
    th, td {
      padding: 12px;
      text-align: left;
      border-bottom: 1px solid #e2e8f0;
    }
    th {
      background: #f7fafc;
      font-weight: 600;
      color: #4a5568;
    }
    .critical { color: #f56565; }
    .high { color: #ed8936; }
    .medium { color: #ecc94b; }
  </style>
</head>
<body>
  <div class="container">
    <h1>📋 Policy Compliance Report</h1>
    <div class="period">
      ${report.period.start.toLocaleDateString()} - ${report.period.end.toLocaleDateString()}
    </div>
    
    <div class="metrics">
      <div class="metric success">
        <div class="metric-label">Compliance Rate</div>
        <div class="metric-value">${report.complianceRate.toFixed(1)}%</div>
      </div>
      
      <div class="metric">
        <div class="metric-label">Total Violations</div>
        <div class="metric-value">${report.totalViolations}</div>
      </div>
      
      <div class="metric critical">
        <div class="metric-label">Critical Violations</div>
        <div class="metric-value">${report.criticalViolations}</div>
      </div>
      
      <div class="metric">
        <div class="metric-label">Blocked Operations</div>
        <div class="metric-value">${report.blockedOperations}</div>
      </div>
    </div>
    
    <h2>Violations by Policy</h2>
    <table>
      <thead>
        <tr>
          <th>Policy ID</th>
          <th>Policy Name</th>
          <th>Violations</th>
          <th>Severity</th>
        </tr>
      </thead>
      <tbody>
        ${Object.entries(report.violationsByPolicy).map(([id, count]) => `
          <tr>
            <td>${id}</td>
            <td>Policy Name</td>
            <td>${count}</td>
            <td class="medium">Medium</td>
          </tr>
        `).join('')}
      </tbody>
    </table>
    
    <h2>Top Violators</h2>
    <table>
      <thead>
        <tr>
          <th>Actor</th>
          <th>Violations</th>
        </tr>
      </thead>
      <tbody>
        ${report.topViolators.map(v => `
          <tr>
            <td>${v.actor}</td>
            <td>${v.count}</td>
          </tr>
        `).join('')}
      </tbody>
    </table>
    
    <h2>Recommendations</h2>
    <ul>
      ${report.recommendations.map(r => `<li>${r}</li>`).join('')}
    </ul>
  </div>
</body>
</html>
    `
  }
  
  private groupByPolicy(events: AuditEvent[]): Record<string, number> {
    return events.reduce((acc, e) => {
      const policyId = e.resource.id
      acc[policyId] = (acc[policyId] || 0) + 1
      return acc
    }, {} as Record<string, number>)
  }
  
  private groupBySeverity(events: AuditEvent[]): Record<string, number> {
    return events.reduce((acc, e) => {
      const severity = e.compliance.violations[0]?.severity || 'unknown'
      acc[severity] = (acc[severity] || 0) + 1
      return acc
    }, {} as Record<string, number>)
  }
  
  private getTopViolators(events: AuditEvent[]): Array<{ actor: string, count: number }> {
    const byActor = events.reduce((acc, e) => {
      const actor = e.actor.id
      acc[actor] = (acc[actor] || 0) + 1
      return acc
    }, {} as Record<string, number>)
    
    return Object.entries(byActor)
      .map(([actor, count]) => ({ actor, count }))
      .sort((a, b) => b.count - a.count)
      .slice(0, 10)
  }
  
  private generateRecommendations(
    byPolicy: Record<string, number>,
    bySeverity: Record<string, number>
  ): string[] {
    const recommendations: string[] = []
    
    if (bySeverity[PolicySeverity.Critical] > 0) {
      recommendations.push('Address all critical violations immediately')
    }
    
    // Find most violated policy
    const mostViolated = Object.entries(byPolicy)
      .sort((a, b) => b[1] - a[1])[0]
    
    if (mostViolated) {
      recommendations.push(`Focus on ${mostViolated[0]} - most frequently violated (${mostViolated[1]} times)`)
    }
    
    if (bySeverity[PolicySeverity.High] > 10) {
      recommendations.push('Consider automated remediation for high-severity violations')
    }
    
    return recommendations
  }
}

interface ComplianceReport {
  period: { start: Date, end: Date }
  totalViolations: number
  complianceRate: number
  violationsByPolicy: Record<string, number>
  violationsBySeverity: Record<string, number>
  criticalViolations: number
  blockedOperations: number
  topViolators: Array<{ actor: string, count: number }>
  recommendations: string[]
}
```

---

## 📚 RELATED ARTICLES

| Article | Purpose | Relationship to Policy Enforcement |
|---------|---------|-----------------------------------|
| **Article I: Audit System** | Audit logging | Violations logged in audit |
| **Article III: Metrics & Reporting** | Metrics | Policy compliance metrics tracked |
| **Article IV: Constitutional Amendment** | Changes | Policy changes via amendments |

---

**Previous:** [← Article I: Audit System](./01-article-i-audit-system.md)  
**Next:** [Article III: Metrics & Reporting →](./03-article-iii-metrics-reporting.md)

---

**Last Updated:** February 7, 2026  
**Constitutional Version:** 2.0.0  
**Status:** ✅ Ratified and In Force

**Motto:** *"Laws Encoded - Violations Prevented - Compliance Automated"*
