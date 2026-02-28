# 🌍 ARTICLE II: ENVIRONMENT MANAGEMENT

**Secure Configuration and Environment Consistency Across Deployments**

---

## 📜 CONSTITUTIONAL AUTHORITY

This article defines **environment management** - how MVCA handles configuration, secrets, and environment variables across development, staging, and production environments.

**Legal Force:**
- ✅ Secrets **MUST** be encrypted at rest and in transit
- ✅ Environment variables **SHALL** be validated before deployment
- ✅ Production secrets **SHALL NOT** be accessible in development
- ✅ Environment parity **SHOULD** be maintained (dev/staging/prod similar)
- ✅ Configuration changes **SHALL** be auditable

**Constitutional Principle:**
> Secrets are sacred - they must never be exposed in code, logs, or errors.  
> Every environment is different, but consistency prevents surprises.

---

## 🏗️ ENVIRONMENT TIERS

### The Three-Tier Architecture
```typescript
/**
 * Environment Tiers
 * Three standard environments with different purposes
 */
enum EnvironmentTier {
  Development = 'development',
  Staging = 'staging',
  Production = 'production'
}

/**
 * Environment Configuration
 */
interface Environment {
  // IDENTITY
  tier: EnvironmentTier
  name: string                    // e.g., "production-us-east"
  
  // VARIABLES
  variables: Record<string, string>
  secrets: Record<string, Secret>
  
  // DEPLOYMENT
  deploymentTarget: DeploymentTarget
  url: string
  
  // SETTINGS
  settings: EnvironmentSettings
  
  // METADATA
  createdAt: Date
  updatedAt: Date
  createdBy: string
}

/**
 * Environment Settings
 */
interface EnvironmentSettings {
  // FEATURES
  featureFlags: Record<string, boolean>
  
  // RESOURCES
  resources: {
    cpu: string                   // e.g., "1 vCPU"
    memory: string                // e.g., "512 MB"
    instances: number             // Number of replicas
  }
  
  // MONITORING
  monitoring: {
    enabled: boolean
    logLevel: 'debug' | 'info' | 'warn' | 'error'
    metricsRetention: number      // Days
  }
  
  // SECURITY
  security: {
    requireHTTPS: boolean
    corsOrigins: string[]
    rateLimiting: {
      enabled: boolean
      requestsPerMinute: number
    }
  }
}

/**
 * Deployment Target
 */
interface DeploymentTarget {
  platform: 'vercel' | 'netlify' | 'railway' | 'fly' | 'aws' | 'docker'
  region?: string
  projectId?: string
  config?: Record<string, any>
}
```

---

### Environment Characteristics
```
ENVIRONMENT TIER COMPARISON
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

                    DEVELOPMENT    STAGING       PRODUCTION
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

PURPOSE             Local testing  Pre-prod      Live users
                    & iteration    validation    

DATA                Fake/seeded    Anonymized    Real user
                    test data      production    data
                                   data

DEPLOYMENT          Manual/auto    Auto on       Manual with
                    on save        main branch   approval

MONITORING          Basic          Full          Full +
                                                 alerts

RESOURCES           Minimal        Medium        Full scale
                    (local)        (1-2 servers) (auto-scale)

SECRETS             Local .env     Encrypted     Encrypted +
                                   vault         HSM

ERROR HANDLING      Verbose        Verbose       Generic
                    stack traces   stack traces  messages

RATE LIMITING       Disabled       Enabled       Strict
                                   (lenient)

LOGGING             DEBUG level    INFO level    WARN level

CACHING             Disabled       Enabled       Aggressive

HTTPS               Optional       Required      Required

COSTS               $0             ~$10/month    Variable

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

## 🔐 SECRET MANAGEMENT

### Secret Storage and Encryption
```typescript
/**
 * Secret Manager
 * Handles secure storage and retrieval of secrets
 */
class SecretManager {
  private vault: SecretVault
  private encryption: EncryptionService
  
  constructor(vault: SecretVault, encryption: EncryptionService) {
    this.vault = vault
    this.encryption = encryption
  }
  
  /**
   * Store a secret
   */
  async setSecret(
    environment: EnvironmentTier,
    key: string,
    value: string,
    metadata?: SecretMetadata
  ): Promise<void> {
    // Validate secret
    this.validateSecret(key, value)
    
    // Encrypt value
    const encrypted = await this.encryption.encrypt(value)
    
    // Store in vault
    const secret: Secret = {
      key,
      value: encrypted,
      environment,
      metadata: {
        ...metadata,
        createdAt: new Date(),
        lastRotated: new Date()
      }
    }
    
    await this.vault.store(environment, key, secret)
    
    console.log(`✓ Secret "${key}" stored for ${environment}`)
  }
  
  /**
   * Get a secret
   */
  async getSecret(
    environment: EnvironmentTier,
    key: string
  ): Promise<string> {
    // Retrieve from vault
    const secret = await this.vault.retrieve(environment, key)
    
    if (!secret) {
      throw new Error(`Secret "${key}" not found in ${environment}`)
    }
    
    // Decrypt value
    const decrypted = await this.encryption.decrypt(secret.value)
    
    // Update last accessed timestamp
    await this.vault.updateLastAccessed(environment, key)
    
    return decrypted
  }
  
  /**
   * Delete a secret
   */
  async deleteSecret(
    environment: EnvironmentTier,
    key: string
  ): Promise<void> {
    await this.vault.delete(environment, key)
    console.log(`✓ Secret "${key}" deleted from ${environment}`)
  }
  
  /**
   * Rotate a secret
   */
  async rotateSecret(
    environment: EnvironmentTier,
    key: string,
    newValue: string
  ): Promise<void> {
    // Store old value for rollback
    const oldSecret = await this.vault.retrieve(environment, key)
    
    // Set new value
    await this.setSecret(environment, key, newValue, {
      previousVersion: oldSecret?.value,
      rotatedAt: new Date()
    })
    
    console.log(`✓ Secret "${key}" rotated in ${environment}`)
  }
  
  /**
   * List all secrets (keys only, not values)
   */
  async listSecrets(environment: EnvironmentTier): Promise<SecretSummary[]> {
    const secrets = await this.vault.list(environment)
    
    return secrets.map(s => ({
      key: s.key,
      createdAt: s.metadata?.createdAt,
      lastRotated: s.metadata?.lastRotated,
      lastAccessed: s.metadata?.lastAccessed
    }))
  }
  
  /**
   * Validate secret before storing
   */
  private validateSecret(key: string, value: string): void {
    // Key validation
    if (!key.match(/^[A-Z][A-Z0-9_]*$/)) {
      throw new Error('Secret key must be UPPER_SNAKE_CASE')
    }
    
    // Value validation
    if (value.length < 8) {
      throw new Error('Secret value must be at least 8 characters')
    }
    
    // Check for common mistakes
    if (value.includes(' ')) {
      console.warn('⚠️  Warning: Secret contains spaces')
    }
    
    if (value.toLowerCase().includes('example') || 
        value.toLowerCase().includes('test') ||
        value.toLowerCase().includes('placeholder')) {
      throw new Error('Secret appears to be a placeholder value')
    }
  }
}

/**
 * Secret Interface
 */
interface Secret {
  key: string
  value: string                    // Encrypted
  environment: EnvironmentTier
  metadata?: SecretMetadata
}

interface SecretMetadata {
  description?: string
  createdAt?: Date
  lastRotated?: Date
  lastAccessed?: Date
  expiresAt?: Date
  previousVersion?: string         // For rollback
  rotatedAt?: Date
}

interface SecretSummary {
  key: string
  createdAt?: Date
  lastRotated?: Date
  lastAccessed?: Date
}
```

---

### Secret Vault Implementation
```typescript
/**
 * Secret Vault
 * Stores encrypted secrets with access control
 */
class SecretVault {
  private storage: Map<string, Map<string, Secret>> = new Map()
  
  /**
   * Store secret
   */
  async store(
    environment: EnvironmentTier,
    key: string,
    secret: Secret
  ): Promise<void> {
    if (!this.storage.has(environment)) {
      this.storage.set(environment, new Map())
    }
    
    const envSecrets = this.storage.get(environment)!
    envSecrets.set(key, secret)
    
    // Persist to disk/database
    await this.persist(environment)
  }
  
  /**
   * Retrieve secret
   */
  async retrieve(
    environment: EnvironmentTier,
    key: string
  ): Promise<Secret | null> {
    const envSecrets = this.storage.get(environment)
    if (!envSecrets) return null
    
    return envSecrets.get(key) || null
  }
  
  /**
   * Delete secret
   */
  async delete(
    environment: EnvironmentTier,
    key: string
  ): Promise<void> {
    const envSecrets = this.storage.get(environment)
    if (envSecrets) {
      envSecrets.delete(key)
      await this.persist(environment)
    }
  }
  
  /**
   * List all secrets in environment
   */
  async list(environment: EnvironmentTier): Promise<Secret[]> {
    const envSecrets = this.storage.get(environment)
    if (!envSecrets) return []
    
    return Array.from(envSecrets.values())
  }
  
  /**
   * Update last accessed timestamp
   */
  async updateLastAccessed(
    environment: EnvironmentTier,
    key: string
  ): Promise<void> {
    const secret = await this.retrieve(environment, key)
    if (secret && secret.metadata) {
      secret.metadata.lastAccessed = new Date()
      await this.store(environment, key, secret)
    }
  }
  
  /**
   * Persist to storage
   */
  private async persist(environment: EnvironmentTier): Promise<void> {
    // In real implementation, persist to encrypted file or database
    // For now, just log
    console.log(`Persisting secrets for ${environment}`)
  }
}
```

---

## 🔧 ENVIRONMENT VARIABLES

### Variable Management
```typescript
/**
 * Environment Variable Manager
 * Manages non-secret configuration variables
 */
class EnvironmentVariableManager {
  private variables: Map<EnvironmentTier, Map<string, string>> = new Map()
  
  /**
   * Set variable
   */
  setVariable(
    environment: EnvironmentTier,
    key: string,
    value: string
  ): void {
    if (!this.variables.has(environment)) {
      this.variables.set(environment, new Map())
    }
    
    const envVars = this.variables.get(environment)!
    envVars.set(key, value)
    
    console.log(`✓ Variable "${key}" set for ${environment}`)
  }
  
  /**
   * Get variable
   */
  getVariable(
    environment: EnvironmentTier,
    key: string
  ): string | undefined {
    const envVars = this.variables.get(environment)
    return envVars?.get(key)
  }
  
  /**
   * Get all variables
   */
  getAllVariables(environment: EnvironmentTier): Record<string, string> {
    const envVars = this.variables.get(environment)
    if (!envVars) return {}
    
    return Object.fromEntries(envVars)
  }
  
  /**
   * Validate required variables
   */
  validateRequired(
    environment: EnvironmentTier,
    requiredVars: string[]
  ): ValidationResult {
    const missing: string[] = []
    const envVars = this.variables.get(environment)
    
    for (const varName of requiredVars) {
      if (!envVars?.has(varName)) {
        missing.push(varName)
      }
    }
    
    if (missing.length > 0) {
      return {
        valid: false,
        missing,
        message: `Missing required variables: ${missing.join(', ')}`
      }
    }
    
    return {
      valid: true,
      message: 'All required variables present'
    }
  }
  
  /**
   * Export to .env format
   */
  exportToEnvFile(environment: EnvironmentTier): string {
    const envVars = this.getAllVariables(environment)
    
    return Object.entries(envVars)
      .map(([key, value]) => `${key}=${value}`)
      .join('\n')
  }
  
  /**
   * Import from .env format
   */
  importFromEnvFile(
    environment: EnvironmentTier,
    content: string
  ): void {
    const lines = content.split('\n')
    
    for (const line of lines) {
      const trimmed = line.trim()
      
      // Skip empty lines and comments
      if (!trimmed || trimmed.startsWith('#')) continue
      
      // Parse KEY=VALUE
      const [key, ...valueParts] = trimmed.split('=')
      const value = valueParts.join('=')  // Handle = in value
      
      if (key && value) {
        this.setVariable(environment, key.trim(), value.trim())
      }
    }
  }
}

interface ValidationResult {
  valid: boolean
  missing?: string[]
  message: string
}
```

---

### Required Variables Detection
```typescript
/**
 * Required Variables Detector
 * Analyzes code to detect required environment variables
 */
class RequiredVariablesDetector {
  /**
   * Detect required variables from code
   */
  detectFromCode(code: CodeContext): string[] {
    const required = new Set<string>()
    
    for (const file of code.files) {
      // Pattern 1: process.env.VAR_NAME
      const processEnvPattern = /process\.env\.([A-Z][A-Z0-9_]*)/g
      let match
      
      while ((match = processEnvPattern.exec(file.content)) !== null) {
        required.add(match[1])
      }
      
      // Pattern 2: import.meta.env.VAR_NAME (Vite)
      const viteEnvPattern = /import\.meta\.env\.([A-Z][A-Z0-9_]*)/g
      
      while ((match = viteEnvPattern.exec(file.content)) !== null) {
        required.add(match[1])
      }
      
      // Pattern 3: Zod env schema
      const zodEnvPattern = /z\.object\(\{([^}]+)\}\)/g
      
      while ((match = zodEnvPattern.exec(file.content)) !== null) {
        const envVars = this.extractZodEnvVars(match[1])
        envVars.forEach(v => required.add(v))
      }
    }
    
    return Array.from(required).sort()
  }
  
  /**
   * Detect from framework config
   */
  detectFromFramework(framework: string, config: any): string[] {
    const required: string[] = []
    
    switch (framework) {
      case 'next':
        // Next.js requires certain variables
        required.push('NEXT_PUBLIC_API_URL')
        if (config.i18n) required.push('NEXT_PUBLIC_DEFAULT_LOCALE')
        break
      
      case 'remix':
        // Remix specific variables
        required.push('SESSION_SECRET')
        break
      
      case 'astro':
        // Astro specific variables
        if (config.output === 'server') {
          required.push('ASTRO_PORT')
        }
        break
    }
    
    return required
  }
  
  /**
   * Detect from dependencies
   */
  detectFromDependencies(dependencies: Record<string, string>): string[] {
    const required: string[] = []
    
    // Database clients
    if (dependencies['prisma']) {
      required.push('DATABASE_URL')
    }
    
    if (dependencies['mongoose']) {
      required.push('MONGODB_URI')
    }
    
    // Authentication
    if (dependencies['next-auth']) {
      required.push('NEXTAUTH_SECRET', 'NEXTAUTH_URL')
    }
    
    // Payment processors
    if (dependencies['stripe']) {
      required.push('STRIPE_SECRET_KEY', 'STRIPE_WEBHOOK_SECRET')
    }
    
    // Email services
    if (dependencies['nodemailer']) {
      required.push('SMTP_HOST', 'SMTP_USER', 'SMTP_PASS')
    }
    
    if (dependencies['@sendgrid/mail']) {
      required.push('SENDGRID_API_KEY')
    }
    
    // Storage
    if (dependencies['@aws-sdk/client-s3']) {
      required.push('AWS_ACCESS_KEY_ID', 'AWS_SECRET_ACCESS_KEY', 'AWS_REGION')
    }
    
    return required
  }
  
  private extractZodEnvVars(schemaBody: string): string[] {
    const vars: string[] = []
    const varPattern = /([A-Z][A-Z0-9_]*):/g
    let match
    
    while ((match = varPattern.exec(schemaBody)) !== null) {
      vars.push(match[1])
    }
    
    return vars
  }
}
```

---

## 🎯 ENVIRONMENT CONFIGURATION

### Configuration Builder
```typescript
/**
 * Environment Configuration Builder
 * Creates complete environment configurations
 */
class EnvironmentConfigBuilder {
  private secretManager: SecretManager
  private variableManager: EnvironmentVariableManager
  private detector: RequiredVariablesDetector
  
  constructor(
    secretManager: SecretManager,
    variableManager: EnvironmentVariableManager
  ) {
    this.secretManager = secretManager
    this.variableManager = variableManager
    this.detector = new RequiredVariablesDetector()
  }
  
  /**
   * Build complete environment configuration
   */
  async buildConfiguration(
    tier: EnvironmentTier,
    context: DeploymentContext
  ): Promise<EnvironmentConfiguration> {
    console.log(`📋 Building ${tier} configuration...`)
    
    // Step 1: Detect required variables
    const required = this.detectRequiredVariables(context)
    console.log(`  Detected ${required.length} required variables`)
    
    // Step 2: Get all variables
    const variables = this.variableManager.getAllVariables(tier)
    
    // Step 3: Get all secrets
    const secrets = await this.secretManager.listSecrets(tier)
    
    // Step 4: Validate
    const validation = this.validate(required, variables, secrets)
    
    if (!validation.valid) {
      throw new Error(`Configuration validation failed: ${validation.message}`)
    }
    
    // Step 5: Build configuration object
    const config: EnvironmentConfiguration = {
      tier,
      variables,
      secrets: secrets.map(s => s.key),  // Only include keys, not values
      required,
      validation,
      metadata: {
        generatedAt: new Date(),
        variableCount: Object.keys(variables).length,
        secretCount: secrets.length
      }
    }
    
    console.log('  ✓ Configuration built successfully')
    
    return config
  }
  
  /**
   * Detect required variables from all sources
   */
  private detectRequiredVariables(context: DeploymentContext): string[] {
    const required = new Set<string>()
    
    // From code
    const fromCode = this.detector.detectFromCode(context.code)
    fromCode.forEach(v => required.add(v))
    
    // From framework
    const fromFramework = this.detector.detectFromFramework(
      context.framework,
      context.frameworkConfig
    )
    fromFramework.forEach(v => required.add(v))
    
    // From dependencies
    const fromDeps = this.detector.detectFromDependencies(
      context.dependencies
    )
    fromDeps.forEach(v => required.add(v))
    
    return Array.from(required).sort()
  }
  
  /**
   * Validate configuration
   */
  private validate(
    required: string[],
    variables: Record<string, string>,
    secrets: SecretSummary[]
  ): ValidationResult {
    const missing: string[] = []
    const secretKeys = secrets.map(s => s.key)
    const allKeys = [...Object.keys(variables), ...secretKeys]
    
    for (const varName of required) {
      if (!allKeys.includes(varName)) {
        missing.push(varName)
      }
    }
    
    if (missing.length > 0) {
      return {
        valid: false,
        missing,
        message: `Missing: ${missing.join(', ')}`
      }
    }
    
    return {
      valid: true,
      message: 'All required variables present'
    }
  }
  
  /**
   * Generate .env file template
   */
  generateEnvTemplate(required: string[]): string {
    const lines = [
      '# Environment Variables',
      '# Generated by MVCA',
      '',
      '# Required Variables',
      ''
    ]
    
    for (const varName of required) {
      lines.push(`${varName}=`)
    }
    
    return lines.join('\n')
  }
}

interface EnvironmentConfiguration {
  tier: EnvironmentTier
  variables: Record<string, string>
  secrets: string[]              // Just the keys
  required: string[]
  validation: ValidationResult
  metadata: {
    generatedAt: Date
    variableCount: number
    secretCount: number
  }
}
```

---

## 🔄 ENVIRONMENT PARITY

### Parity Checker
```typescript
/**
 * Environment Parity Checker
 * Ensures consistency across environments
 */
class EnvironmentParityChecker {
  /**
   * Check parity between environments
   */
  checkParity(
    env1: Environment,
    env2: Environment
  ): ParityReport {
    console.log(`🔍 Checking parity: ${env1.tier} vs ${env2.tier}`)
    
    const issues: ParityIssue[] = []
    
    // Check variable parity
    const varIssues = this.checkVariableParity(env1, env2)
    issues.push(...varIssues)
    
    // Check secret parity
    const secretIssues = this.checkSecretParity(env1, env2)
    issues.push(...secretIssues)
    
    // Check settings parity
    const settingsIssues = this.checkSettingsParity(env1, env2)
    issues.push(...settingsIssues)
    
    const score = this.calculateParityScore(issues)
    
    return {
      env1: env1.tier,
      env2: env2.tier,
      score,
      issues,
      recommendation: this.getRecommendation(score)
    }
  }
  
  /**
   * Check variable parity
   */
  private checkVariableParity(
    env1: Environment,
    env2: Environment
  ): ParityIssue[] {
    const issues: ParityIssue[] = []
    
    const keys1 = Object.keys(env1.variables)
    const keys2 = Object.keys(env2.variables)
    
    // Variables in env1 but not env2
    for (const key of keys1) {
      if (!keys2.includes(key)) {
        issues.push({
          type: 'missing_variable',
          severity: 'medium',
          message: `Variable "${key}" exists in ${env1.tier} but not ${env2.tier}`,
          suggestion: `Add "${key}" to ${env2.tier}`
        })
      }
    }
    
    // Variables in env2 but not env1
    for (const key of keys2) {
      if (!keys1.includes(key)) {
        issues.push({
          type: 'extra_variable',
          severity: 'low',
          message: `Variable "${key}" exists in ${env2.tier} but not ${env1.tier}`,
          suggestion: `Consider adding "${key}" to ${env1.tier} or removing from ${env2.tier}`
        })
      }
    }
    
    return issues
  }
  
  /**
   * Check secret parity
   */
  private checkSecretParity(
    env1: Environment,
    env2: Environment
  ): ParityIssue[] {
    const issues: ParityIssue[] = []
    
    const keys1 = Object.keys(env1.secrets)
    const keys2 = Object.keys(env2.secrets)
    
    // Secrets in env1 but not env2
    for (const key of keys1) {
      if (!keys2.includes(key)) {
        issues.push({
          type: 'missing_secret',
          severity: 'high',
          message: `Secret "${key}" exists in ${env1.tier} but not ${env2.tier}`,
          suggestion: `Add "${key}" secret to ${env2.tier}`
        })
      }
    }
    
    return issues
  }
  
  /**
   * Check settings parity
   */
  private checkSettingsParity(
    env1: Environment,
    env2: Environment
  ): ParityIssue[] {
    const issues: ParityIssue[] = []
    
    // Compare deployment targets
    if (env1.deploymentTarget.platform !== env2.deploymentTarget.platform) {
      issues.push({
        type: 'platform_mismatch',
        severity: 'medium',
        message: `Different platforms: ${env1.deploymentTarget.platform} vs ${env2.deploymentTarget.platform}`,
        suggestion: 'Consider using same platform for consistency'
      })
    }
    
    // Compare security settings
    if (env1.settings.security.requireHTTPS !== env2.settings.security.requireHTTPS) {
      issues.push({
        type: 'security_mismatch',
        severity: 'high',
        message: 'HTTPS requirement differs between environments',
        suggestion: 'Enable HTTPS in all environments'
      })
    }
    
    return issues
  }
  
  /**
   * Calculate parity score
   */
  private calculateParityScore(issues: ParityIssue[]): number {
    if (issues.length === 0) return 100
    
    let score = 100
    
    for (const issue of issues) {
      switch (issue.severity) {
        case 'high':
          score -= 20
          break
        case 'medium':
          score -= 10
          break
        case 'low':
          score -= 5
          break
      }
    }
    
    return Math.max(0, score)
  }
  
  /**
   * Get recommendation based on score
   */
  private getRecommendation(score: number): string {
    if (score >= 90) return 'Excellent parity between environments'
    if (score >= 75) return 'Good parity with minor differences'
    if (score >= 60) return 'Moderate parity - address high-severity issues'
    return 'Poor parity - significant configuration drift detected'
  }
}

interface ParityReport {
  env1: EnvironmentTier
  env2: EnvironmentTier
  score: number
  issues: ParityIssue[]
  recommendation: string
}

interface ParityIssue {
  type: string
  severity: 'high' | 'medium' | 'low'
  message: string
  suggestion: string
}
```

---

## 🚩 FEATURE FLAGS

### Feature Flag Management
```typescript
/**
 * Feature Flag Manager
 * Enables/disables features per environment
 */
class FeatureFlagManager {
  private flags: Map<EnvironmentTier, Map<string, FeatureFlag>> = new Map()
  
  /**
   * Create feature flag
   */
  createFlag(
    name: string,
    description: string,
    defaultValue: boolean = false
  ): void {
    const flag: FeatureFlag = {
      name,
      description,
      defaultValue,
      values: {
        [EnvironmentTier.Development]: defaultValue,
        [EnvironmentTier.Staging]: defaultValue,
        [EnvironmentTier.Production]: defaultValue
      },
      createdAt: new Date()
    }
    
    // Store flag in all environments
    for (const tier of Object.values(EnvironmentTier)) {
      if (!this.flags.has(tier)) {
        this.flags.set(tier, new Map())
      }
      
      this.flags.get(tier)!.set(name, flag)
    }
    
    console.log(`✓ Feature flag "${name}" created`)
  }
  
  /**
   * Set flag value for environment
   */
  setFlag(
    environment: EnvironmentTier,
    name: string,
    value: boolean
  ): void {
    const envFlags = this.flags.get(environment)
    if (!envFlags) {
      throw new Error(`No flags configured for ${environment}`)
    }
    
    const flag = envFlags.get(name)
    if (!flag) {
      throw new Error(`Feature flag "${name}" not found`)
    }
    
    flag.values[environment] = value
    flag.lastModified = new Date()
    
    console.log(`✓ Feature flag "${name}" set to ${value} in ${environment}`)
  }
  
  /**
   * Get flag value
   */
  getFlag(environment: EnvironmentTier, name: string): boolean {
    const envFlags = this.flags.get(environment)
    if (!envFlags) return false
    
    const flag = envFlags.get(name)
    return flag?.values[environment] ?? flag?.defaultValue ?? false
  }
  
  /**
   * Get all flags for environment
   */
  getAllFlags(environment: EnvironmentTier): Record<string, boolean> {
    const envFlags = this.flags.get(environment)
    if (!envFlags) return {}
    
    const result: Record<string, boolean> = {}
    
    for (const [name, flag] of envFlags) {
      result[name] = flag.values[environment] ?? flag.defaultValue
    }
    
    return result
  }
  
  /**
   * Enable gradual rollout
   */
  enableGradualRollout(name: string, schedule: RolloutSchedule): void {
    // Enable in development immediately
    this.setFlag(EnvironmentTier.Development, name, true)
    
    // Schedule staging
    setTimeout(() => {
      this.setFlag(EnvironmentTier.Staging, name, true)
      console.log(`✓ Feature "${name}" enabled in staging`)
    }, schedule.stagingDelay)
    
    // Schedule production
    setTimeout(() => {
      this.setFlag(EnvironmentTier.Production, name, true)
      console.log(`✓ Feature "${name}" enabled in production`)
    }, schedule.stagingDelay + schedule.productionDelay)
  }
}

interface FeatureFlag {
  name: string
  description: string
  defaultValue: boolean
  values: Record<EnvironmentTier, boolean>
  createdAt: Date
  lastModified?: Date
}

interface RolloutSchedule {
  stagingDelay: number      // ms
  productionDelay: number   // ms
}
```

---

## 📚 RELATED ARTICLES

| Article | Purpose | Relationship to Environment Management |
|---------|---------|----------------------------------------|
| **Article I: Deployment Pipeline** | Deployment flow | Uses environment config during deployment |
| **Article III: Database Migrations** | Schema changes | Uses DATABASE_URL from environment |
| **Article IV: Deployment Monitoring** | Health checks | Uses monitoring config from environment |

---

**Previous:** [← Article I: Deployment Pipeline](./01-article-i-deployment-pipeline.md)  
**Next:** [Article III: Database Migrations →](./03-article-iii-database-migrations.md)

---

**Last Updated:** February 7, 2026  
**Constitutional Version:** 2.0.0  
**Status:** ✅ Ratified and In Force

**Motto:** *"Secrets are Sacred - Configuration is Consistent - Environments are Isolated"*
