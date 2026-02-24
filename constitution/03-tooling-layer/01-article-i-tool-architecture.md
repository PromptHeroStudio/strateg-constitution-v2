# 🏗️ ARTICLE I: TOOL ARCHITECTURE

**The Foundation of MVCA's Integration Framework**

---

## 📜 CONSTITUTIONAL AUTHORITY

This article defines the **tool interface specification** that all MVCA tools must follow. This standard ensures consistency, security, and reliability across all integrations.

**Legal Force:**
- ✅ All tools **MUST** implement the IToolInterface specification
- ✅ Tools **SHALL** follow the security tier model
- ✅ Tools **SHALL** handle errors gracefully (no crashes)
- ✅ Tools **SHALL** validate all inputs (no injection attacks)
- ✅ Tools **SHALL** document their behavior (purpose, parameters, errors)

**Constitutional Principle:**
> A standardized interface ensures predictability, security, and maintainability.  
> Every tool speaks the same language.

---

## 🎯 THE TOOL INTERFACE SPECIFICATION

### IToolInterface: The Universal Contract
```typescript
/**
 * Universal Tool Interface
 * All MVCA tools must implement this interface
 */
interface IToolInterface {
  // IDENTITY
  name: string                    // Unique tool identifier (e.g., "vercel_deploy")
  version: string                 // Semantic version (e.g., "1.2.3")
  category: ToolCategory          // Tool category (FileSystem, API, Database, etc.)
  securityTier: SecurityTier      // Risk level (ReadOnly, Write, Execute, ExternalAPI)
  
  // METADATA
  description: string             // Human-readable description
  documentation: string           // URL to detailed docs
  requiredAuth?: AuthConfig       // Authentication requirements (API key, OAuth, etc.)
  costInfo?: CostInfo            // Cost information (if applicable)
  
  // CAPABILITY
  capabilities: ToolCapability[]  // What this tool can do
  parameters: ParameterSchema     // Input parameters (Zod schema)
  returnType: ReturnSchema        // Output schema (Zod schema)
  
  // EXECUTION
  execute(params: unknown): Promise<ToolResult>  // Main execution method
  validate(params: unknown): ValidationResult     // Input validation
  
  // ERROR HANDLING
  handleError(error: Error): ToolError           // Error transformation
  retry?: RetryConfig                             // Retry configuration
  
  // LIFECYCLE
  initialize?(): Promise<void>                    // Setup (if needed)
  cleanup?(): Promise<void>                       // Teardown (if needed)
  healthCheck?(): Promise<HealthStatus>          // Status check
}
```

---

### Supporting Types
```typescript
/**
 * Tool Categories
 */
enum ToolCategory {
  FileSystem = 'filesystem',      // File/directory operations
  CodeAnalysis = 'code_analysis', // Parsing, linting, type-checking
  Validation = 'validation',      // Schema validation, format checking
  Search = 'search',              // Code search, grep, find
  API = 'api',                    // REST/GraphQL API calls
  Database = 'database',          // Database queries, migrations
  Service = 'service',            // Third-party services (Stripe, SendGrid)
  Deployment = 'deployment',      // Hosting platforms (Vercel, Netlify)
  Development = 'development',    // Dev tools (Git, npm, Docker)
  Custom = 'custom'               // User-defined custom tools
}

/**
 * Security Tiers (Risk Levels)
 */
enum SecurityTier {
  ReadOnly = 'read_only',         // Tier 1: Safest (read files, list directories)
  Write = 'write',                // Tier 2: Medium risk (write files, create dirs)
  Execute = 'execute',            // Tier 3: High risk (run commands, install packages)
  ExternalAPI = 'external_api'    // Tier 4: Highest risk (API calls, costs money)
}

/**
 * Tool Capability
 */
interface ToolCapability {
  action: string                  // What it does (e.g., "deploy", "query", "send_email")
  description: string             // Human-readable description
  requiresConfirmation: boolean   // Needs user approval?
  estimatedCost?: number         // Estimated cost in USD (if applicable)
  estimatedTime?: number         // Estimated time in milliseconds
}

/**
 * Authentication Configuration
 */
interface AuthConfig {
  type: 'api_key' | 'oauth' | 'basic_auth' | 'bearer_token' | 'custom'
  required: boolean               // Is auth mandatory?
  configKey: string              // Where to find credentials (e.g., "VERCEL_TOKEN")
  setupUrl?: string              // URL to get credentials
  testMethod?: () => Promise<boolean>  // Test if auth works
}

/**
 * Cost Information
 */
interface CostInfo {
  model: 'free' | 'usage_based' | 'subscription' | 'one_time'
  estimatedCost?: number         // Per-operation cost (USD)
  billingUrl?: string            // Where user can see charges
  freeTierLimit?: number         // Free tier limit (if applicable)
}

/**
 * Tool Result
 */
interface ToolResult {
  success: boolean
  data?: unknown                 // Result data (if success)
  error?: ToolError             // Error details (if failure)
  metadata?: {
    duration?: number            // Execution time (ms)
    cost?: number               // Actual cost incurred (USD)
    resourcesUsed?: string[]    // Resources accessed
  }
}

/**
 * Tool Error
 */
interface ToolError {
  code: string                   // Machine-readable error code
  message: string                // Human-readable error message
  recoverable: boolean           // Can retry fix this?
  suggestion?: string            // How to fix (if known)
  originalError?: Error         // Original error (for debugging)
}

/**
 * Retry Configuration
 */
interface RetryConfig {
  maxAttempts: number            // Maximum retry attempts
  backoffMs: number              // Initial backoff duration
  backoffMultiplier: number      // Exponential backoff multiplier
  retryableErrors: string[]      // Error codes that trigger retry
}

/**
 * Health Status
 */
interface HealthStatus {
  healthy: boolean
  message?: string
  lastCheck: Date
  nextCheck?: Date
}
```

---

## 📊 TOOL CATEGORIES

### Category 1: File System Tools

**Purpose:** Interact with local file system (read, write, search files)

**Security Tier:** ReadOnly (read) or Write (write/delete)

**Examples:**
```typescript
// Read File Tool
{
  name: "read_file",
  category: ToolCategory.FileSystem,
  securityTier: SecurityTier.ReadOnly,
  capabilities: [
    {
      action: "read",
      description: "Read file contents",
      requiresConfirmation: false
    }
  ],
  parameters: z.object({
    path: z.string().describe("File path to read"),
    encoding: z.enum(['utf8', 'base64']).default('utf8')
  }),
  returnType: z.object({
    content: z.string(),
    size: z.number(),
    mimeType: z.string()
  })
}

// Write File Tool
{
  name: "write_file",
  category: ToolCategory.FileSystem,
  securityTier: SecurityTier.Write,
  capabilities: [
    {
      action: "write",
      description: "Write content to file",
      requiresConfirmation: true  // Requires user approval
    }
  ],
  parameters: z.object({
    path: z.string(),
    content: z.string(),
    createDirs: z.boolean().default(true)
  }),
  returnType: z.object({
    success: z.boolean(),
    bytesWritten: z.number()
  })
}
```

---

### Category 2: Code Analysis Tools

**Purpose:** Parse, analyze, and understand code structure

**Security Tier:** ReadOnly (analysis doesn't modify code)

**Examples:**
```typescript
// TypeScript Parser Tool
{
  name: "parse_typescript",
  category: ToolCategory.CodeAnalysis,
  securityTier: SecurityTier.ReadOnly,
  capabilities: [
    {
      action: "parse",
      description: "Parse TypeScript/JavaScript code into AST",
      requiresConfirmation: false
    }
  ],
  parameters: z.object({
    code: z.string().describe("TypeScript code to parse"),
    filePath: z.string().optional()
  }),
  returnType: z.object({
    ast: z.any(),
    errors: z.array(z.object({
      line: z.number(),
      message: z.string()
    })),
    imports: z.array(z.string()),
    exports: z.array(z.string())
  })
}

// Dependency Analyzer Tool
{
  name: "analyze_dependencies",
  category: ToolCategory.CodeAnalysis,
  securityTier: SecurityTier.ReadOnly,
  capabilities: [
    {
      action: "analyze",
      description: "Analyze npm dependencies for security issues",
      requiresConfirmation: false
    }
  ],
  parameters: z.object({
    packageJsonPath: z.string()
  }),
  returnType: z.object({
    totalDeps: z.number(),
    vulnerabilities: z.array(z.object({
      package: z.string(),
      severity: z.enum(['low', 'moderate', 'high', 'critical']),
      description: z.string(),
      fixVersion: z.string().optional()
    })),
    outdated: z.array(z.object({
      package: z.string(),
      current: z.string(),
      latest: z.string()
    }))
  })
}
```

---

### Category 3: Validation Tools

**Purpose:** Validate data formats (JSON, YAML, schemas)

**Security Tier:** ReadOnly (validation is read-only)

**Examples:**
```typescript
// JSON Validator Tool
{
  name: "validate_json",
  category: ToolCategory.Validation,
  securityTier: SecurityTier.ReadOnly,
  capabilities: [
    {
      action: "validate",
      description: "Validate JSON against JSON Schema",
      requiresConfirmation: false
    }
  ],
  parameters: z.object({
    data: z.string().describe("JSON string to validate"),
    schema: z.any().describe("JSON Schema to validate against")
  }),
  returnType: z.object({
    valid: z.boolean(),
    errors: z.array(z.object({
      path: z.string(),
      message: z.string()
    }))
  })
}

// TypeScript Type Checker Tool
{
  name: "type_check",
  category: ToolCategory.Validation,
  securityTier: SecurityTier.ReadOnly,
  capabilities: [
    {
      action: "check",
      description: "Run TypeScript compiler to check types",
      requiresConfirmation: false
    }
  ],
  parameters: z.object({
    projectPath: z.string(),
    strict: z.boolean().default(true)
  }),
  returnType: z.object({
    success: z.boolean(),
    errors: z.array(z.object({
      file: z.string(),
      line: z.number(),
      column: z.number(),
      message: z.string()
    })),
    warnings: z.array(z.object({
      file: z.string(),
      message: z.string()
    }))
  })
}
```

---

### Category 4: API Tools

**Purpose:** Make HTTP requests to REST/GraphQL APIs

**Security Tier:** ExternalAPI (highest risk, may incur costs)

**Examples:**
```typescript
// REST API Call Tool
{
  name: "rest_api_call",
  category: ToolCategory.API,
  securityTier: SecurityTier.ExternalAPI,
  requiredAuth: {
    type: 'bearer_token',
    required: true,
    configKey: 'API_TOKEN',
    setupUrl: 'https://example.com/settings/tokens'
  },
  capabilities: [
    {
      action: "request",
      description: "Make HTTP request to REST API",
      requiresConfirmation: true,  // User confirms API calls
      estimatedCost: 0.001  // $0.001 per request (example)
    }
  ],
  parameters: z.object({
    method: z.enum(['GET', 'POST', 'PUT', 'DELETE', 'PATCH']),
    url: z.string().url(),
    headers: z.record(z.string()).optional(),
    body: z.any().optional(),
    timeout: z.number().default(30000)
  }),
  returnType: z.object({
    status: z.number(),
    headers: z.record(z.string()),
    body: z.any(),
    duration: z.number()
  })
}
```

---

### Category 5: Database Tools

**Purpose:** Query and manage databases

**Security Tier:** Write (queries) or Execute (migrations)

**Examples:**
```typescript
// Supabase Query Tool
{
  name: "supabase_query",
  category: ToolCategory.Database,
  securityTier: SecurityTier.Write,
  requiredAuth: {
    type: 'api_key',
    required: true,
    configKey: 'SUPABASE_KEY',
    setupUrl: 'https://app.supabase.com/project/_/settings/api'
  },
  capabilities: [
    {
      action: "query",
      description: "Execute SQL query on Supabase",
      requiresConfirmation: true,
      estimatedTime: 1000  // 1 second typical
    }
  ],
  parameters: z.object({
    query: z.string().describe("SQL query to execute"),
    params: z.array(z.any()).optional()
  }),
  returnType: z.object({
    rows: z.array(z.any()),
    rowCount: z.number(),
    duration: z.number()
  }),
  retry: {
    maxAttempts: 3,
    backoffMs: 1000,
    backoffMultiplier: 2,
    retryableErrors: ['TIMEOUT', 'CONNECTION_ERROR']
  }
}
```

---

### Category 6: Service Tools

**Purpose:** Integrate with third-party services (Stripe, SendGrid, etc.)

**Security Tier:** ExternalAPI (costs money, sends data externally)

**Examples:**
```typescript
// Stripe Create Payment Tool
{
  name: "stripe_create_payment",
  category: ToolCategory.Service,
  securityTier: SecurityTier.ExternalAPI,
  requiredAuth: {
    type: 'api_key',
    required: true,
    configKey: 'STRIPE_SECRET_KEY',
    setupUrl: 'https://dashboard.stripe.com/apikeys'
  },
  costInfo: {
    model: 'usage_based',
    estimatedCost: 0.30,  // $0.30 per transaction (Stripe fee)
    billingUrl: 'https://dashboard.stripe.com/settings/billing'
  },
  capabilities: [
    {
      action: "create_payment",
      description: "Create Stripe payment intent",
      requiresConfirmation: true,  // MUST confirm (costs money!)
      estimatedCost: 0.30
    }
  ],
  parameters: z.object({
    amount: z.number().positive(),
    currency: z.string().length(3),
    customerId: z.string().optional(),
    description: z.string().optional()
  }),
  returnType: z.object({
    paymentIntentId: z.string(),
    clientSecret: z.string(),
    status: z.enum(['succeeded', 'pending', 'failed'])
  })
}

// SendGrid Send Email Tool
{
  name: "sendgrid_send_email",
  category: ToolCategory.Service,
  securityTier: SecurityTier.ExternalAPI,
  requiredAuth: {
    type: 'api_key',
    required: true,
    configKey: 'SENDGRID_API_KEY',
    setupUrl: 'https://app.sendgrid.com/settings/api_keys'
  },
  costInfo: {
    model: 'free',  // Free tier: 100 emails/day
    freeTierLimit: 100,
    billingUrl: 'https://app.sendgrid.com/settings/billing'
  },
  capabilities: [
    {
      action: "send_email",
      description: "Send email via SendGrid",
      requiresConfirmation: true,
      estimatedTime: 2000
    }
  ],
  parameters: z.object({
    to: z.string().email(),
    from: z.string().email(),
    subject: z.string(),
    html: z.string().optional(),
    text: z.string().optional()
  }).refine(data => data.html || data.text, {
    message: "Either html or text must be provided"
  }),
  returnType: z.object({
    messageId: z.string(),
    status: z.enum(['sent', 'queued', 'failed'])
  })
}
```

---

### Category 7: Deployment Tools

**Purpose:** Deploy applications to hosting platforms

**Security Tier:** ExternalAPI (modifies production systems)

**Examples:**
```typescript
// Vercel Deploy Tool
{
  name: "vercel_deploy",
  category: ToolCategory.Deployment,
  securityTier: SecurityTier.ExternalAPI,
  requiredAuth: {
    type: 'bearer_token',
    required: true,
    configKey: 'VERCEL_TOKEN',
    setupUrl: 'https://vercel.com/account/tokens'
  },
  costInfo: {
    model: 'subscription',  // Depends on Vercel plan
    billingUrl: 'https://vercel.com/account/billing'
  },
  capabilities: [
    {
      action: "deploy",
      description: "Deploy project to Vercel",
      requiresConfirmation: true,
      estimatedTime: 45000  // ~45 seconds
    }
  ],
  parameters: z.object({
    projectPath: z.string(),
    production: z.boolean().default(false),
    envVars: z.record(z.string()).optional()
  }),
  returnType: z.object({
    deploymentId: z.string(),
    url: z.string().url(),
    status: z.enum(['ready', 'building', 'error']),
    buildLogs: z.string()
  })
}
```

---

### Category 8: Development Tools

**Purpose:** Development workflow tools (Git, npm, Docker)

**Security Tier:** Execute (runs commands, installs packages)

**Examples:**
```typescript
// npm Install Tool
{
  name: "npm_install",
  category: ToolCategory.Development,
  securityTier: SecurityTier.Execute,
  capabilities: [
    {
      action: "install",
      description: "Install npm package",
      requiresConfirmation: true,  // Installs code, needs approval
      estimatedTime: 10000
    }
  ],
  parameters: z.object({
    package: z.string(),
    version: z.string().optional(),
    dev: z.boolean().default(false),
    projectPath: z.string()
  }),
  returnType: z.object({
    success: z.boolean(),
    installedVersion: z.string(),
    dependencies: z.array(z.string())
  })
}

// Git Commit Tool
{
  name: "git_commit",
  category: ToolCategory.Development,
  securityTier: SecurityTier.Write,
  capabilities: [
    {
      action: "commit",
      description: "Create Git commit",
      requiresConfirmation: true
    }
  ],
  parameters: z.object({
    message: z.string(),
    files: z.array(z.string()).optional(),  // If empty, commits all staged
    projectPath: z.string()
  }),
  returnType: z.object({
    commitHash: z.string(),
    filesChanged: z.number(),
    insertions: z.number(),
    deletions: z.number()
  })
}
```

---

## 🛡️ SECURITY MODEL

### 4-Tier Security System
```
TIER 1: READ-ONLY TOOLS
├─ Risk: Information disclosure (minimal)
├─ Examples: read_file, list_directory, parse_typescript
├─ Permission: Enabled by default
├─ User Confirmation: Not required
└─ Mitigation: No access to .env, private keys, secrets

TIER 2: WRITE TOOLS
├─ Risk: Data modification, potential data loss
├─ Examples: write_file, create_directory, git_commit
├─ Permission: Enabled by default
├─ User Confirmation: Required for destructive operations (delete)
└─ Mitigation: Backup before write, undo capability

TIER 3: EXECUTE TOOLS
├─ Risk: Arbitrary code execution, system compromise
├─ Examples: npm_install, execute_command, docker_run
├─ Permission: Requires explicit user approval
├─ User Confirmation: Required for EVERY execution
└─ Mitigation: Command whitelisting, sandboxing, no sudo/admin

TIER 4: EXTERNAL API TOOLS
├─ Risk: Financial cost, data leakage, service abuse
├─ Examples: vercel_deploy, stripe_charge, sendgrid_send
├─ Permission: Requires API key + explicit approval per action
├─ User Confirmation: Required for EVERY API call
└─ Mitigation: Rate limiting, cost caps, audit logs
```

---

### Security Implementation Example
```typescript
/**
 * Tool Security Validator
 * Enforces security rules before tool execution
 */
class ToolSecurityValidator {
  /**
   * Check if tool can execute based on security tier
   */
  async canExecute(
    tool: IToolInterface,
    params: unknown,
    userContext: UserContext
  ): Promise<SecurityCheckResult> {
    
    // TIER 1: READ-ONLY (always allowed)
    if (tool.securityTier === SecurityTier.ReadOnly) {
      // Still check for sensitive file access
      if (this.isSensitiveFile(params)) {
        return {
          allowed: false,
          reason: 'Cannot access sensitive files (.env, private keys)'
        }
      }
      return { allowed: true }
    }
    
    // TIER 2: WRITE (check user confirmation for destructive ops)
    if (tool.securityTier === SecurityTier.Write) {
      const isDestructive = this.isDestructiveOperation(tool, params)
      
      if (isDestructive && !userContext.confirmedDestructive) {
        return {
          allowed: false,
          reason: 'Destructive operation requires user confirmation',
          needsConfirmation: true
        }
      }
      return { allowed: true }
    }
    
    // TIER 3: EXECUTE (always requires confirmation)
    if (tool.securityTier === SecurityTier.Execute) {
      if (!userContext.confirmedExecution) {
        return {
          allowed: false,
          reason: 'Code execution requires explicit user approval',
          needsConfirmation: true
        }
      }
      
      // Check command whitelist
      if (!this.isWhitelistedCommand(params)) {
        return {
          allowed: false,
          reason: 'Command not in whitelist (security risk)'
        }
      }
      return { allowed: true }
    }
    
    // TIER 4: EXTERNAL API (requires auth + confirmation + cost check)
    if (tool.securityTier === SecurityTier.ExternalAPI) {
      // Check authentication
      if (tool.requiredAuth && !this.hasValidAuth(tool, userContext)) {
        return {
          allowed: false,
          reason: `Missing authentication: ${tool.requiredAuth.configKey}`,
          setupUrl: tool.requiredAuth.setupUrl
        }
      }
      
      // Check user confirmation
      if (!userContext.confirmedAPICall) {
        return {
          allowed: false,
          reason: 'External API call requires user confirmation',
          needsConfirmation: true,
          estimatedCost: tool.costInfo?.estimatedCost
        }
      }
      
      // Check cost limits (if applicable)
      if (tool.costInfo && tool.costInfo.estimatedCost) {
        const totalCostToday = await this.getTotalCostToday(userContext)
        const dailyLimit = userContext.settings.dailyCostLimit || 10.00
        
        if (totalCostToday + tool.costInfo.estimatedCost > dailyLimit) {
          return {
            allowed: false,
            reason: `Daily cost limit exceeded ($${dailyLimit})`,
            currentCost: totalCostToday,
            limit: dailyLimit
          }
        }
      }
      
      return { allowed: true }
    }
    
    // Unknown tier (should never happen)
    return {
      allowed: false,
      reason: 'Unknown security tier'
    }
  }
  
  /**
   * Check if file path is sensitive
   */
  private isSensitiveFile(params: any): boolean {
    const path = params?.path || params?.filePath || ''
    const sensitivePatterns = [
      /\.env/,
      /\.env\./,
      /private.*key/i,
      /secret/i,
      /password/i,
      /\.ssh\//,
      /\.aws\//
    ]
    
    return sensitivePatterns.some(pattern => pattern.test(path))
  }
  
  /**
   * Check if operation is destructive
   */
  private isDestructiveOperation(tool: IToolInterface, params: any): boolean {
    const destructiveActions = ['delete', 'remove', 'drop', 'truncate']
    return destructiveActions.some(action => 
      tool.name.includes(action) || 
      tool.capabilities.some(cap => cap.action.includes(action))
    )
  }
  
  /**
   * Check if command is whitelisted
   */
  private isWhitelistedCommand(params: any): boolean {
    const command = params?.command || ''
    const whitelist = [
      /^npm install/,
      /^npm run/,
      /^git commit/,
      /^git push/,
      /^npx prisma/,
      /^docker build/
    ]
    
    // Block dangerous commands
    const blacklist = [
      /sudo/,
      /rm -rf/,
      /dd if=/,
      /:(){ :|:& };:/,  // Fork bomb
      /curl.*\| sh/,     // Pipe to shell
      /wget.*\| bash/    // Pipe to bash
    ]
    
    if (blacklist.some(pattern => pattern.test(command))) {
      return false
    }
    
    return whitelist.some(pattern => pattern.test(command))
  }
  
  /**
   * Check if valid authentication exists
   */
  private hasValidAuth(tool: IToolInterface, userContext: UserContext): boolean {
    if (!tool.requiredAuth) return true
    
    const authKey = tool.requiredAuth.configKey
    const authValue = userContext.credentials[authKey]
    
    return !!authValue && authValue.trim().length > 0
  }
  
  /**
   * Get total cost incurred today
   */
  private async getTotalCostToday(userContext: UserContext): Promise<number> {
    // Query audit log for today's API calls
    const today = new Date().toISOString().split('T')[0]
    const logs = await this.auditLog.query({
      userId: userContext.userId,
      date: today,
      tier: SecurityTier.ExternalAPI
    })
    
    return logs.reduce((sum, log) => sum + (log.cost || 0), 0)
  }
}
```

---

## ⚙️ TOOL EXECUTION LIFECYCLE

### Complete Execution Flow
```typescript
/**
 * Tool Executor
 * Handles complete tool execution lifecycle
 */
class ToolExecutor {
  async execute(
    tool: IToolInterface,
    params: unknown,
    userContext: UserContext
  ): Promise<ToolResult> {
    
    const executionId = generateExecutionId()
    
    try {
      // PHASE 1: VALIDATION
      const validationResult = tool.validate(params)
      if (!validationResult.valid) {
        return {
          success: false,
          error: {
            code: 'VALIDATION_ERROR',
            message: 'Invalid parameters',
            recoverable: true,
            suggestion: validationResult.errors.join(', ')
          }
        }
      }
      
      // PHASE 2: SECURITY CHECK
      const securityCheck = await this.securityValidator.canExecute(
        tool,
        params,
        userContext
      )
      
      if (!securityCheck.allowed) {
        // Need user confirmation?
        if (securityCheck.needsConfirmation) {
          const confirmed = await this.promptUserConfirmation(
            tool,
            params,
            securityCheck
          )
          
          if (!confirmed) {
            return {
              success: false,
              error: {
                code: 'USER_CANCELLED',
                message: 'User cancelled operation',
                recoverable: false
              }
            }
          }
          
          // Retry security check with confirmation
          userContext.confirmedAPICall = true
          return this.execute(tool, params, userContext)
        }
        
        // Security violation
        return {
          success: false,
          error: {
            code: 'SECURITY_VIOLATION',
            message: securityCheck.reason,
            recoverable: false,
            suggestion: securityCheck.setupUrl
          }
        }
      }
      
      // PHASE 3: INITIALIZATION
      if (tool.initialize) {
        await tool.initialize()
      }
      
      // PHASE 4: EXECUTION (with timing and logging)
      const startTime = Date.now()
      
      await this.auditLog.logStart(executionId, {
        tool: tool.name,
        user: userContext.userId,
        params: this.sanitizeParams(params),  // Remove sensitive data
        timestamp: new Date()
      })
      
      let result: ToolResult
      
      try {
        result = await tool.execute(params)
      } catch (error) {
        // Tool execution failed
        const toolError = tool.handleError(error as Error)
        
        // Should we retry?
        if (toolError.recoverable && tool.retry) {
          result = await this.retryExecution(tool, params, toolError, tool.retry)
        } else {
          result = {
            success: false,
            error: toolError
          }
        }
      }
      
      const duration = Date.now() - startTime
      
      // PHASE 5: RESULT PROCESSING
      if (result.success) {
        result.metadata = {
          ...result.metadata,
          duration
        }
      }
      
      // PHASE 6: AUDIT LOGGING
      await this.auditLog.logComplete(executionId, {
        success: result.success,
        duration,
        cost: result.metadata?.cost,
        error: result.error?.code
      })
      
      // PHASE 7: CLEANUP
      if (tool.cleanup) {
        await tool.cleanup()
      }
      
      return result
      
    } catch (error) {
      // Unexpected error (should not happen with proper error handling)
      await this.auditLog.logError(executionId, error)
      
      return {
        success: false,
        error: {
          code: 'UNEXPECTED_ERROR',
          message: 'An unexpected error occurred',
          recoverable: false,
          originalError: error as Error
        }
      }
    }
  }
  
  /**
   * Retry execution with exponential backoff
   */
  private async retryExecution(
    tool: IToolInterface,
    params: unknown,
    lastError: ToolError,
    retryConfig: RetryConfig
  ): Promise<ToolResult> {
    
    for (let attempt = 1; attempt <= retryConfig.maxAttempts; attempt++) {
      // Wait before retry (exponential backoff)
      const backoff = retryConfig.backoffMs * Math.pow(retryConfig.backoffMultiplier, attempt - 1)
      await this.sleep(backoff)
      
      try {
        const result = await tool.execute(params)
        
        // Success!
        return {
          ...result,
          metadata: {
            ...result.metadata,
            retriedAttempts: attempt
          }
        }
      } catch (error) {
        const toolError = tool.handleError(error as Error)
        
        // Check if error is retryable
        if (!retryConfig.retryableErrors.includes(toolError.code)) {
          // Not retryable, give up
          return {
            success: false,
            error: toolError
          }
        }
        
        // Last attempt failed
        if (attempt === retryConfig.maxAttempts) {
          return {
            success: false,
            error: {
              ...toolError,
              message: `Failed after ${attempt} attempts: ${toolError.message}`
            }
          }
        }
        
        // Continue to next attempt
      }
    }
    
    // Should never reach here
    return {
      success: false,
      error: lastError
    }
  }
  
  /**
   * Prompt user for confirmation
   */
  private async promptUserConfirmation(
    tool: IToolInterface,
    params: unknown,
    securityCheck: SecurityCheckResult
  ): Promise<boolean> {
    const message = this.buildConfirmationMessage(tool, params, securityCheck)
    return await this.ui.confirm(message)
  }
  
  /**
   * Build user-friendly confirmation message
   */
  private buildConfirmationMessage(
    tool: IToolInterface,
    params: unknown,
    securityCheck: SecurityCheckResult
  ): string {
    let message = `${tool.name}: ${tool.description}\n\n`
    
    if (securityCheck.estimatedCost) {
      message += `💰 Estimated cost: $${securityCheck.estimatedCost.toFixed(2)}\n`
    }
    
    if (tool.capabilities[0]?.estimatedTime) {
      const seconds = tool.capabilities[0].estimatedTime / 1000
      message += `⏱️ Estimated time: ${seconds}s\n`
    }
    
    message += `\n📋 Parameters:\n${JSON.stringify(params, null, 2)}\n`
    message += `\nProceed? (Y/n)`
    
    return message
  }
  
  /**
   * Sanitize parameters (remove sensitive data before logging)
   */
  private sanitizeParams(params: any): any {
    const sensitiveKeys = ['password', 'apiKey', 'token', 'secret', 'key']
    
    const sanitized = { ...params }
    
    for (const key of Object.keys(sanitized)) {
      if (sensitiveKeys.some(sensitive => key.toLowerCase().includes(sensitive))) {
        sanitized[key] = '[REDACTED]'
      }
    }
    
    return sanitized
  }
  
  private sleep(ms: number): Promise<void> {
    return new Promise(resolve => setTimeout(resolve, ms))
  }
}
```

---
