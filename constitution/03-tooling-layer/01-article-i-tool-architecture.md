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
## 🔍 TOOL DISCOVERY MECHANISM

### How MVCA Finds Available Tools

**Discovery Process:**
```typescript
/**
 * Tool Registry
 * Central registry for all available tools
 */
class ToolRegistry {
  private tools: Map<string, IToolInterface> = new Map()
  private categories: Map<ToolCategory, IToolInterface[]> = new Map()
  
  /**
   * Register a tool
   */
  register(tool: IToolInterface): void {
    // Validate tool implements interface correctly
    this.validateTool(tool)
    
    // Add to registry
    this.tools.set(tool.name, tool)
    
    // Add to category index
    if (!this.categories.has(tool.category)) {
      this.categories.set(tool.category, [])
    }
    this.categories.get(tool.category)!.push(tool)
  }
  
  /**
   * Get tool by name
   */
  getTool(name: string): IToolInterface | undefined {
    return this.tools.get(name)
  }
  
  /**
   * Get all tools in category
   */
  getToolsByCategory(category: ToolCategory): IToolInterface[] {
    return this.categories.get(category) || []
  }
  
  /**
   * Get all available tools for user
   */
  getAvailableTools(userContext: UserContext): IToolInterface[] {
    return Array.from(this.tools.values()).filter(tool => {
      // Check if user has required authentication
      if (tool.requiredAuth?.required) {
        const hasAuth = userContext.credentials[tool.requiredAuth.configKey]
        if (!hasAuth) return false
      }
      
      // Check if user enabled this tool
      if (tool.securityTier === SecurityTier.ExternalAPI) {
        return userContext.enabledTools.includes(tool.name)
      }
      
      return true
    })
  }
  
  /**
   * Search tools by capability
   */
  searchByCapability(query: string): IToolInterface[] {
    const results: IToolInterface[] = []
    
    for (const tool of this.tools.values()) {
      // Search in description
      if (tool.description.toLowerCase().includes(query.toLowerCase())) {
        results.push(tool)
        continue
      }
      
      // Search in capabilities
      for (const capability of tool.capabilities) {
        if (capability.description.toLowerCase().includes(query.toLowerCase())) {
          results.push(tool)
          break
        }
      }
    }
    
    return results
  }
  
  /**
   * Validate tool implementation
   */
  private validateTool(tool: IToolInterface): void {
    // Check required fields
    if (!tool.name || !tool.version || !tool.category) {
      throw new Error(`Invalid tool: missing required fields`)
    }
    
    // Check execute method exists
    if (typeof tool.execute !== 'function') {
      throw new Error(`Tool ${tool.name}: execute method required`)
    }
    
    // Check validate method exists
    if (typeof tool.validate !== 'function') {
      throw new Error(`Tool ${tool.name}: validate method required`)
    }
    
    // Check parameters schema
    if (!tool.parameters || typeof tool.parameters.parse !== 'function') {
      throw new Error(`Tool ${tool.name}: valid Zod schema required for parameters`)
    }
    
    // Check capabilities
    if (!tool.capabilities || tool.capabilities.length === 0) {
      throw new Error(`Tool ${tool.name}: at least one capability required`)
    }
  }
}
```

---

### Tool Auto-Discovery (Plugin System)
```typescript
/**
 * Tool Loader
 * Automatically discovers and loads tools from directories
 */
class ToolLoader {
  private registry: ToolRegistry
  
  constructor(registry: ToolRegistry) {
    this.registry = registry
  }
  
  /**
   * Load all tools from directory
   */
  async loadFromDirectory(directory: string): Promise<void> {
    const files = await fs.readdir(directory)
    
    for (const file of files) {
      if (!file.endsWith('.tool.ts') && !file.endsWith('.tool.js')) {
        continue
      }
      
      try {
        const toolPath = path.join(directory, file)
        const toolModule = await import(toolPath)
        
        // Tool module should export 'tool' or default export
        const tool = toolModule.tool || toolModule.default
        
        if (!tool) {
          console.warn(`Tool file ${file}: no tool exported`)
          continue
        }
        
        // Register tool
        this.registry.register(tool)
        console.log(`✓ Loaded tool: ${tool.name}`)
        
      } catch (error) {
        console.error(`Failed to load tool ${file}:`, error)
      }
    }
  }
  
  /**
   * Load core tools (built-in)
   */
  async loadCoreTools(): Promise<void> {
    await this.loadFromDirectory('./tools/core')
  }
  
  /**
   * Load integration tools (optional)
   */
  async loadIntegrationTools(): Promise<void> {
    await this.loadFromDirectory('./tools/integrations')
  }
  
  /**
   * Load user custom tools
   */
  async loadUserTools(userDirectory: string): Promise<void> {
    await this.loadFromDirectory(userDirectory)
  }
}
```

---

## ⚠️ ERROR HANDLING PATTERNS

### Standardized Error Handling

**Error Categories:**
```typescript
/**
 * Tool Error Codes (Standardized)
 */
enum ToolErrorCode {
  // Validation Errors (4xx)
  VALIDATION_ERROR = 'VALIDATION_ERROR',
  INVALID_PARAMETERS = 'INVALID_PARAMETERS',
  MISSING_REQUIRED_PARAM = 'MISSING_REQUIRED_PARAM',
  
  // Authentication Errors (4xx)
  AUTHENTICATION_REQUIRED = 'AUTHENTICATION_REQUIRED',
  INVALID_CREDENTIALS = 'INVALID_CREDENTIALS',
  AUTHENTICATION_EXPIRED = 'AUTHENTICATION_EXPIRED',
  
  // Authorization Errors (4xx)
  PERMISSION_DENIED = 'PERMISSION_DENIED',
  INSUFFICIENT_PRIVILEGES = 'INSUFFICIENT_PRIVILEGES',
  SECURITY_VIOLATION = 'SECURITY_VIOLATION',
  
  // User Action Errors (4xx)
  USER_CANCELLED = 'USER_CANCELLED',
  USER_REJECTED = 'USER_REJECTED',
  CONFIRMATION_TIMEOUT = 'CONFIRMATION_TIMEOUT',
  
  // Resource Errors (4xx)
  RESOURCE_NOT_FOUND = 'RESOURCE_NOT_FOUND',
  RESOURCE_LOCKED = 'RESOURCE_LOCKED',
  RESOURCE_CONFLICT = 'RESOURCE_CONFLICT',
  
  // Rate Limiting (4xx)
  RATE_LIMIT_EXCEEDED = 'RATE_LIMIT_EXCEEDED',
  QUOTA_EXCEEDED = 'QUOTA_EXCEEDED',
  COST_LIMIT_EXCEEDED = 'COST_LIMIT_EXCEEDED',
  
  // External Service Errors (5xx)
  EXTERNAL_SERVICE_ERROR = 'EXTERNAL_SERVICE_ERROR',
  API_ERROR = 'API_ERROR',
  NETWORK_ERROR = 'NETWORK_ERROR',
  TIMEOUT = 'TIMEOUT',
  
  // Tool Errors (5xx)
  TOOL_INITIALIZATION_FAILED = 'TOOL_INITIALIZATION_FAILED',
  TOOL_EXECUTION_FAILED = 'TOOL_EXECUTION_FAILED',
  UNEXPECTED_ERROR = 'UNEXPECTED_ERROR'
}

/**
 * Error Handler Base Class
 */
abstract class BaseToolErrorHandler {
  /**
   * Transform error into standardized ToolError
   */
  handleError(error: Error): ToolError {
    // Identify error type
    if (error instanceof ValidationError) {
      return this.handleValidationError(error)
    }
    
    if (error instanceof AuthenticationError) {
      return this.handleAuthError(error)
    }
    
    if (error instanceof NetworkError) {
      return this.handleNetworkError(error)
    }
    
    if (error instanceof TimeoutError) {
      return this.handleTimeoutError(error)
    }
    
    // Unknown error
    return this.handleUnknownError(error)
  }
  
  private handleValidationError(error: ValidationError): ToolError {
    return {
      code: ToolErrorCode.VALIDATION_ERROR,
      message: error.message,
      recoverable: true,
      suggestion: 'Please check your input parameters and try again.'
    }
  }
  
  private handleAuthError(error: AuthenticationError): ToolError {
    return {
      code: ToolErrorCode.AUTHENTICATION_REQUIRED,
      message: 'Authentication failed',
      recoverable: true,
      suggestion: 'Please check your API credentials and try again.'
    }
  }
  
  private handleNetworkError(error: NetworkError): ToolError {
    return {
      code: ToolErrorCode.NETWORK_ERROR,
      message: 'Network connection failed',
      recoverable: true,
      suggestion: 'Please check your internet connection and try again.'
    }
  }
  
  private handleTimeoutError(error: TimeoutError): ToolError {
    return {
      code: ToolErrorCode.TIMEOUT,
      message: 'Operation timed out',
      recoverable: true,
      suggestion: 'The operation took too long. Please try again.'
    }
  }
  
  private handleUnknownError(error: Error): ToolError {
    return {
      code: ToolErrorCode.UNEXPECTED_ERROR,
      message: 'An unexpected error occurred',
      recoverable: false,
      suggestion: 'Please report this issue to support.',
      originalError: error
    }
  }
}
```

---

### Error Recovery Strategies
```typescript
/**
 * Error Recovery Handler
 * Attempts to recover from tool errors
 */
class ErrorRecoveryHandler {
  /**
   * Attempt to recover from error
   */
  async tryRecover(
    tool: IToolInterface,
    params: unknown,
    error: ToolError
  ): Promise<ToolResult | null> {
    
    switch (error.code) {
      case ToolErrorCode.AUTHENTICATION_EXPIRED:
        return await this.refreshAuthentication(tool, params)
      
      case ToolErrorCode.RATE_LIMIT_EXCEEDED:
        return await this.waitAndRetry(tool, params, error)
      
      case ToolErrorCode.NETWORK_ERROR:
        return await this.retryWithBackoff(tool, params)
      
      case ToolErrorCode.TIMEOUT:
        return await this.retryWithLongerTimeout(tool, params)
      
      case ToolErrorCode.RESOURCE_LOCKED:
        return await this.waitForResourceUnlock(tool, params)
      
      default:
        return null  // Cannot recover
    }
  }
  
  private async refreshAuthentication(
    tool: IToolInterface,
    params: unknown
  ): Promise<ToolResult | null> {
    // Attempt to refresh OAuth token or re-authenticate
    try {
      await tool.initialize()  // Re-initialize (re-auth)
      return await tool.execute(params)
    } catch {
      return null  // Recovery failed
    }
  }
  
  private async waitAndRetry(
    tool: IToolInterface,
    params: unknown,
    error: ToolError
  ): Promise<ToolResult | null> {
    // Extract wait time from error (if provided)
    const waitMs = this.extractWaitTime(error) || 60000  // Default 1 minute
    
    await this.sleep(waitMs)
    
    try {
      return await tool.execute(params)
    } catch {
      return null
    }
  }
  
  private async retryWithBackoff(
    tool: IToolInterface,
    params: unknown
  ): Promise<ToolResult | null> {
    const attempts = 3
    const backoff = 1000  // 1 second
    
    for (let i = 0; i < attempts; i++) {
      await this.sleep(backoff * Math.pow(2, i))
      
      try {
        return await tool.execute(params)
      } catch {
        if (i === attempts - 1) return null
      }
    }
    
    return null
  }
  
  private extractWaitTime(error: ToolError): number | null {
    // Try to parse "Retry after X seconds" from error message
    const match = error.message.match(/retry after (\d+) seconds/i)
    if (match) {
      return parseInt(match[1]) * 1000
    }
    return null
  }
  
  private sleep(ms: number): Promise<void> {
    return new Promise(resolve => setTimeout(resolve, ms))
  }
}
```

---

## 📊 COMPLETE TOOL IMPLEMENTATION EXAMPLE

### Example: Vercel Deploy Tool (Full Implementation)
```typescript
import { z } from 'zod'

/**
 * Vercel Deploy Tool
 * Deploys Next.js applications to Vercel
 */
export const vercelDeployTool: IToolInterface = {
  // IDENTITY
  name: 'vercel_deploy',
  version: '1.0.0',
  category: ToolCategory.Deployment,
  securityTier: SecurityTier.ExternalAPI,
  
  // METADATA
  description: 'Deploy Next.js application to Vercel hosting platform',
  documentation: 'https://vercel.com/docs/rest-api#endpoints/deployments',
  
  requiredAuth: {
    type: 'bearer_token',
    required: true,
    configKey: 'VERCEL_TOKEN',
    setupUrl: 'https://vercel.com/account/tokens',
    testMethod: async () => {
      try {
        const response = await fetch('https://api.vercel.com/v2/user', {
          headers: {
            Authorization: `Bearer ${process.env.VERCEL_TOKEN}`
          }
        })
        return response.ok
      } catch {
        return false
      }
    }
  },
  
  costInfo: {
    model: 'subscription',
    billingUrl: 'https://vercel.com/account/billing'
  },
  
  // CAPABILITY
  capabilities: [
    {
      action: 'deploy',
      description: 'Deploy application to Vercel (production or preview)',
      requiresConfirmation: true,
      estimatedTime: 45000  // ~45 seconds
    }
  ],
  
  parameters: z.object({
    projectPath: z.string().describe('Path to Next.js project'),
    production: z.boolean().default(false).describe('Deploy to production?'),
    envVars: z.record(z.string()).optional().describe('Environment variables'),
    buildCommand: z.string().optional().describe('Custom build command'),
    framework: z.enum(['nextjs', 'react', 'vue', 'svelte']).default('nextjs')
  }),
  
  returnType: z.object({
    success: z.boolean(),
    deploymentId: z.string(),
    url: z.string().url(),
    status: z.enum(['ready', 'building', 'error', 'queued']),
    buildLogs: z.string().optional(),
    inspectorUrl: z.string().url().optional()
  }),
  
  // EXECUTION
  async execute(params: unknown): Promise<ToolResult> {
    const validated = this.parameters.parse(params)
    
    try {
      // Step 1: Validate project exists
      const projectExists = await this.validateProject(validated.projectPath)
      if (!projectExists) {
        return {
          success: false,
          error: {
            code: ToolErrorCode.RESOURCE_NOT_FOUND,
            message: `Project not found at ${validated.projectPath}`,
            recoverable: false,
            suggestion: 'Check the project path and try again.'
          }
        }
      }
      
      // Step 2: Create deployment
      const deploymentResponse = await fetch('https://api.vercel.com/v13/deployments', {
        method: 'POST',
        headers: {
          'Authorization': `Bearer ${process.env.VERCEL_TOKEN}`,
          'Content-Type': 'application/json'
        },
        body: JSON.stringify({
          name: path.basename(validated.projectPath),
          files: await this.getProjectFiles(validated.projectPath),
          target: validated.production ? 'production' : 'preview',
          env: validated.envVars || {},
          buildCommand: validated.buildCommand,
          framework: validated.framework
        })
      })
      
      if (!deploymentResponse.ok) {
        const error = await deploymentResponse.json()
        throw new Error(`Vercel API error: ${error.error.message}`)
      }
      
      const deployment = await deploymentResponse.json()
      
      // Step 3: Wait for build to complete
      const finalStatus = await this.waitForDeployment(deployment.id)
      
      // Step 4: Get build logs
      const buildLogs = await this.getBuildLogs(deployment.id)
      
      return {
        success: finalStatus.readyState === 'READY',
        data: {
          success: finalStatus.readyState === 'READY',
          deploymentId: deployment.id,
          url: `https://${deployment.url}`,
          status: finalStatus.readyState === 'READY' ? 'ready' : 'error',
          buildLogs: buildLogs,
          inspectorUrl: `https://vercel.com/${deployment.creator.username}/${deployment.name}`
        },
        metadata: {
          duration: Date.now() - startTime,
          resourcesUsed: ['Vercel API']
        }
      }
      
    } catch (error) {
      return {
        success: false,
        error: this.handleError(error as Error)
      }
    }
  },
  
  validate(params: unknown): ValidationResult {
    try {
      this.parameters.parse(params)
      return { valid: true, errors: [] }
    } catch (error) {
      if (error instanceof z.ZodError) {
        return {
          valid: false,
          errors: error.errors.map(e => e.message)
        }
      }
      return {
        valid: false,
        errors: ['Invalid parameters']
      }
    }
  },
  
  // ERROR HANDLING
  handleError(error: Error): ToolError {
    if (error.message.includes('authentication')) {
      return {
        code: ToolErrorCode.AUTHENTICATION_REQUIRED,
        message: 'Vercel authentication failed',
        recoverable: true,
        suggestion: 'Check your VERCEL_TOKEN environment variable.'
      }
    }
    
    if (error.message.includes('rate limit')) {
      return {
        code: ToolErrorCode.RATE_LIMIT_EXCEEDED,
        message: 'Vercel API rate limit exceeded',
        recoverable: true,
        suggestion: 'Wait a few minutes and try again.'
      }
    }
    
    if (error.message.includes('timeout')) {
      return {
        code: ToolErrorCode.TIMEOUT,
        message: 'Deployment timed out',
        recoverable: true,
        suggestion: 'Try again or check Vercel dashboard for deployment status.'
      }
    }
    
    return {
      code: ToolErrorCode.TOOL_EXECUTION_FAILED,
      message: error.message,
      recoverable: false,
      originalError: error
    }
  },
  
  retry: {
    maxAttempts: 2,
    backoffMs: 5000,
    backoffMultiplier: 2,
    retryableErrors: [
      ToolErrorCode.NETWORK_ERROR,
      ToolErrorCode.TIMEOUT,
      ToolErrorCode.RATE_LIMIT_EXCEEDED
    ]
  },
  
  // LIFECYCLE
  async initialize(): Promise<void> {
    // Test Vercel authentication
    const valid = await this.requiredAuth!.testMethod!()
    if (!valid) {
      throw new Error('Vercel authentication failed')
    }
  },
  
  async healthCheck(): Promise<HealthStatus> {
    try {
      const response = await fetch('https://api.vercel.com/v2/user', {
        headers: {
          Authorization: `Bearer ${process.env.VERCEL_TOKEN}`
        }
      })
      
      return {
        healthy: response.ok,
        message: response.ok ? 'Vercel API accessible' : 'Vercel API error',
        lastCheck: new Date()
      }
    } catch {
      return {
        healthy: false,
        message: 'Cannot reach Vercel API',
        lastCheck: new Date()
      }
    }
  },
  
  // HELPER METHODS
  async validateProject(projectPath: string): Promise<boolean> {
    try {
      const packageJson = await fs.readFile(
        path.join(projectPath, 'package.json'),
        'utf-8'
      )
      const pkg = JSON.parse(packageJson)
      return !!pkg.dependencies?.next
    } catch {
      return false
    }
  },
  
  async getProjectFiles(projectPath: string): Promise<any[]> {
    // Implementation to gather all project files for deployment
    // This would recursively read files and create file objects
    // Simplified for example
    return []
  },
  
  async waitForDeployment(deploymentId: string): Promise<any> {
    const maxWait = 300000  // 5 minutes
    const startTime = Date.now()
    
    while (Date.now() - startTime < maxWait) {
      const response = await fetch(
        `https://api.vercel.com/v13/deployments/${deploymentId}`,
        {
          headers: {
            Authorization: `Bearer ${process.env.VERCEL_TOKEN}`
          }
        }
      )
      
      const deployment = await response.json()
      
      if (deployment.readyState === 'READY' || deployment.readyState === 'ERROR') {
        return deployment
      }
      
      await new Promise(resolve => setTimeout(resolve, 5000))  // Wait 5s
    }
    
    throw new Error('Deployment timeout')
  },
  
  async getBuildLogs(deploymentId: string): Promise<string> {
    const response = await fetch(
      `https://api.vercel.com/v2/deployments/${deploymentId}/events`,
      {
        headers: {
          Authorization: `Bearer ${process.env.VERCEL_TOKEN}`
        }
      }
    )
    
    const events = await response.json()
    return events.map((e: any) => e.text).join('\n')
  }
}
```

---

## 🧪 TOOL TESTING GUIDELINES

### Testing Framework for Tools
```typescript
/**
 * Tool Test Suite
 * Standard test patterns for all tools
 */
describe('Tool: vercel_deploy', () => {
  let tool: IToolInterface
  let mockUserContext: UserContext
  
  beforeEach(() => {
    tool = vercelDeployTool
    mockUserContext = {
      userId: 'test-user',
      credentials: {
        VERCEL_TOKEN: 'test-token'
      },
      enabledTools: ['vercel_deploy'],
      settings: {
        dailyCostLimit: 10.00
      }
    }
  })
  
  describe('Interface Compliance', () => {
    test('implements all required fields', () => {
      expect(tool.name).toBeDefined()
      expect(tool.version).toBeDefined()
      expect(tool.category).toBeDefined()
      expect(tool.securityTier).toBeDefined()
      expect(tool.description).toBeDefined()
      expect(tool.capabilities).toBeDefined()
      expect(tool.parameters).toBeDefined()
      expect(tool.returnType).toBeDefined()
    })
    
    test('has execute method', () => {
      expect(typeof tool.execute).toBe('function')
    })
    
    test('has validate method', () => {
      expect(typeof tool.validate).toBe('function')
    })
    
    test('has handleError method', () => {
      expect(typeof tool.handleError).toBe('function')
    })
  })
  
  describe('Parameter Validation', () => {
    test('validates correct parameters', () => {
      const result = tool.validate({
        projectPath: '/path/to/project',
        production: true
      })
      
      expect(result.valid).toBe(true)
    })
    
    test('rejects invalid parameters', () => {
      const result = tool.validate({
        projectPath: 123,  // Should be string
        production: 'yes'  // Should be boolean
      })
      
      expect(result.valid).toBe(false)
      expect(result.errors.length).toBeGreaterThan(0)
    })
    
    test('rejects missing required parameters', () => {
      const result = tool.validate({})
      
      expect(result.valid).toBe(false)
      expect(result.errors).toContain('projectPath is required')
    })
  })
  
  describe('Execution', () => {
    test('succeeds with valid parameters', async () => {
      const result = await tool.execute({
        projectPath: './test-project',
        production: false
      })
      
      expect(result.success).toBe(true)
      expect(result.data).toBeDefined()
      expect(result.data.deploymentId).toBeDefined()
      expect(result.data.url).toMatch(/^https:\/\//)
    })
    
    test('fails gracefully with invalid project', async () => {
      const result = await tool.execute({
        projectPath: './non-existent',
        production: false
      })
      
      expect(result.success).toBe(false)
      expect(result.error).toBeDefined()
      expect(result.error.code).toBe(ToolErrorCode.RESOURCE_NOT_FOUND)
    })
    
    test('handles authentication errors', async () => {
      delete process.env.VERCEL_TOKEN
      
      const result = await tool.execute({
        projectPath: './test-project'
      })
      
      expect(result.success).toBe(false)
      expect(result.error.code).toBe(ToolErrorCode.AUTHENTICATION_REQUIRED)
    })
  })
  
  describe('Error Handling', () => {
    test('transforms errors correctly', () => {
      const error = new Error('authentication failed')
      const toolError = tool.handleError(error)
      
      expect(toolError.code).toBe(ToolErrorCode.AUTHENTICATION_REQUIRED)
      expect(toolError.recoverable).toBe(true)
      expect(toolError.suggestion).toBeDefined()
    })
    
    test('marks non-recoverable errors', () => {
      const error = new Error('project not found')
      const toolError = tool.handleError(error)
      
      expect(toolError.recoverable).toBe(false)
    })
  })
  
  describe('Retry Logic', () => {
    test('has retry configuration', () => {
      expect(tool.retry).toBeDefined()
      expect(tool.retry.maxAttempts).toBeGreaterThan(0)
      expect(tool.retry.retryableErrors).toBeDefined()
    })
    
    test('retries on network error', async () => {
      // Mock network error
      jest.spyOn(global, 'fetch').mockRejectedValueOnce(new Error('network'))
      jest.spyOn(global, 'fetch').mockResolvedValueOnce({ ok: true } as Response)
      
      const result = await tool.execute({
        projectPath: './test-project'
      })
      
      expect(result.success).toBe(true)
      expect(result.metadata.retriedAttempts).toBe(1)
    })
  })
  
  describe('Security', () => {
    test('requires authentication', () => {
      expect(tool.requiredAuth).toBeDefined()
      expect(tool.requiredAuth.required).toBe(true)
    })
    
    test('is external API tier', () => {
      expect(tool.securityTier).toBe(SecurityTier.ExternalAPI)
    })
    
    test('requires user confirmation', () => {
      expect(tool.capabilities[0].requiresConfirmation).toBe(true)
    })
  })
  
  describe('Health Check', () => {
    test('reports healthy status', async () => {
      const status = await tool.healthCheck!()
      
      expect(status.healthy).toBe(true)
      expect(status.lastCheck).toBeInstanceOf(Date)
    })
    
    test('reports unhealthy when API down', async () => {
      jest.spyOn(global, 'fetch').mockRejectedValue(new Error('network'))
      
      const status = await tool.healthCheck!()
      
      expect(status.healthy).toBe(false)
      expect(status.message).toContain('Cannot reach')
    })
  })
})
```

---

## 📋 TOOL DEVELOPMENT CHECKLIST
```markdown
TOOL IMPLEMENTATION CHECKLIST:

INTERFACE COMPLIANCE:
□ Implements IToolInterface completely
□ All required fields present (name, version, category, etc.)
□ execute() method implemented
□ validate() method implemented
□ handleError() method implemented

DOCUMENTATION:
□ Description clear and concise
□ Parameters documented with Zod schema
□ Return type documented with Zod schema
□ Capabilities list complete
□ Documentation URL provided

SECURITY:
□ Security tier assigned correctly
□ Authentication configured (if required)
□ User confirmation required (if external API or execute tier)
□ Input validation implemented
□ Sensitive data not logged

ERROR HANDLING:
□ All errors transformed to ToolError
□ Error codes use standardized enum
□ Recoverable flag set correctly
□ Suggestions provided for common errors
□ Original error preserved (for debugging)

RETRY LOGIC (if applicable):
□ Retry configuration defined
□ Retryable errors identified
□ Backoff strategy implemented
□ Max attempts reasonable

TESTING:
□ Unit tests written (interface compliance)
□ Integration tests written (actual execution)
□ Error case tests written
□ Security tests written
□ Mock tests for external APIs

LIFECYCLE:
□ initialize() implemented (if needed)
□ cleanup() implemented (if needed)
□ healthCheck() implemented

PERFORMANCE:
□ Estimated time documented
□ Timeouts configured
□ No blocking operations
□ Async operations use async/await

GRADING:
All checks pass: ⭐⭐⭐⭐⭐ Production ready
1-2 missing: ⭐⭐⭐⭐ Acceptable (address before merge)
3-5 missing: ⭐⭐⭐ Needs work
>5 missing: ⭐⭐ Not ready
```

---

## 📚 RELATED ARTICLES

| Article | Purpose | Relationship to Tool Architecture |
|---------|---------|-----------------------------------|
| **Article II: MCP Integration** | MCP protocol details | MCP servers use this architecture |
| **Article III: Core Tool Catalog** | Built-in tools | All implement this interface |
| **Article IV: Integration Tool Catalog** | Optional tools | All implement this interface |
| **Segment 4: Execution Layer** | Tool orchestration | Uses tools defined here |

---

**Previous:** [← Segment 3 README](./README.md)  
**Next:** [Article II: MCP Integration →](./02-article-ii-mcp-integration.md)

---

**Last Updated:** February 7, 2026  
**Constitutional Version:** 2.0.0  
**Status:** ✅ Ratified and In Force

**Motto:** *"A Standardized Interface Ensures Predictability, Security, and Excellence"*
