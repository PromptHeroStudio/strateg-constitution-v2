# 🔄 ARTICLE I: DEPLOYMENT PIPELINE

**Automated, Safe, and Reversible Deployment Workflows**

---

## 📜 CONSTITUTIONAL AUTHORITY

This article defines the **deployment pipeline** - the automated workflow that takes code from development to production with quality gates, health checks, and rollback capabilities.

**Legal Force:**
- ✅ All deployments **MUST** follow the defined pipeline stages
- ✅ Quality gates **SHALL** be enforced before production
- ✅ Production deployments **SHALL** require explicit approval
- ✅ Health checks **SHALL** verify deployment success
- ✅ Automatic rollback **SHALL** trigger on health check failures

**Constitutional Principle:**
> A deployment is not complete when code ships - it's complete when it's proven healthy in production.  
> Every deployment must be safe, monitored, and reversible.

---

## 🎯 PIPELINE ARCHITECTURE

### The Six-Stage Pipeline
```typescript
/**
 * Deployment Pipeline
 * Six stages from code to production
 */
interface DeploymentPipeline {
  stages: [
    ValidationStage,      // Stage 1: Pre-deployment validation
    BuildStage,          // Stage 2: Build and package
    StagingStage,        // Stage 3: Deploy to staging
    ApprovalStage,       // Stage 4: Request production approval
    ProductionStage,     // Stage 5: Deploy to production
    MonitoringStage      // Stage 6: Health monitoring
  ]
  
  config: PipelineConfig
  state: PipelineState
}

/**
 * Pipeline Configuration
 */
interface PipelineConfig {
  // QUALITY GATES
  qualityGates: {
    security: boolean              // Enforce security checks
    performance: boolean           // Enforce performance checks
    tests: boolean                 // Require tests to pass
    codeQuality: boolean          // Check code quality
  }
  
  // DEPLOYMENT STRATEGY
  strategy: 'blue-green' | 'canary' | 'rolling'
  
  // STAGING
  stagingRequired: boolean
  stagingDuration: number          // How long to monitor staging (ms)
  
  // APPROVAL
  autoApproveStaging: boolean      // Auto-approve staging deployments
  autoApproveProduction: boolean   // Auto-approve production (dangerous!)
  
  // MONITORING
  healthCheckInterval: number      // How often to check health (ms)
  healthCheckTimeout: number       // When to give up (ms)
  errorRateThreshold: number       // Max acceptable error rate (%)
  
  // ROLLBACK
  autoRollback: boolean            // Automatic rollback on failure
  rollbackOnErrorRate: number      // Rollback if error rate exceeds this
  rollbackOnResponseTime: number   // Rollback if response time exceeds this
}

/**
 * Pipeline State
 */
interface PipelineState {
  id: string
  status: 'pending' | 'running' | 'success' | 'failed' | 'rolled_back'
  currentStage: number
  startTime: Date
  endTime?: Date
  
  stageResults: StageResult[]
  
  metrics: {
    totalDuration: number
    buildDuration: number
    deployDuration: number
    healthCheckDuration: number
  }
  
  rollback?: {
    triggered: boolean
    reason: string
    timestamp: Date
    success: boolean
  }
}
```

---

## 🏗️ STAGE 1: VALIDATION

### Pre-Deployment Validation
```typescript
/**
 * Validation Stage
 * Runs all quality gates before deployment
 */
class ValidationStage implements PipelineStage {
  name = 'Validation'
  
  async execute(context: DeploymentContext): Promise<StageResult> {
    const results: ValidationResult[] = []
    
    console.log('🔍 Running pre-deployment validation...')
    
    // Gate 1: Security Validation
    if (context.config.qualityGates.security) {
      const securityResult = await this.runSecurityValidation(context)
      results.push(securityResult)
      
      if (!securityResult.passed) {
        return this.failStage('Security validation failed', results)
      }
    }
    
    // Gate 2: Test Validation
    if (context.config.qualityGates.tests) {
      const testResult = await this.runTests(context)
      results.push(testResult)
      
      if (!testResult.passed) {
        return this.failStage('Tests failed', results)
      }
    }
    
    // Gate 3: Performance Validation
    if (context.config.qualityGates.performance) {
      const perfResult = await this.runPerformanceValidation(context)
      results.push(perfResult)
      
      if (!perfResult.passed) {
        return this.failStage('Performance validation failed', results)
      }
    }
    
    // Gate 4: Code Quality Validation
    if (context.config.qualityGates.codeQuality) {
      const qualityResult = await this.runCodeQualityValidation(context)
      results.push(qualityResult)
      
      if (!qualityResult.passed) {
        return this.failStage('Code quality validation failed', results)
      }
    }
    
    // Gate 5: Environment Validation
    const envResult = await this.validateEnvironment(context)
    results.push(envResult)
    
    if (!envResult.passed) {
      return this.failStage('Environment validation failed', results)
    }
    
    console.log('✅ All validation gates passed')
    
    return {
      stage: this.name,
      status: 'success',
      duration: Date.now() - context.stageStartTime,
      data: { results }
    }
  }
  
  /**
   * Run security validation
   */
  private async runSecurityValidation(
    context: DeploymentContext
  ): Promise<ValidationResult> {
    console.log('  🔒 Checking security...')
    
    // Use security validation from Segment 5
    const validator = new SecurityValidator()
    const result = await validator.validate(context.code)
    
    const critical = result.violations.filter(v => v.severity === 'critical')
    const high = result.violations.filter(v => v.severity === 'high')
    
    if (critical.length > 0) {
      return {
        name: 'Security',
        passed: false,
        message: `${critical.length} critical security vulnerabilities found`,
        details: result.violations
      }
    }
    
    if (high.length > 0) {
      console.log(`  ⚠️  Warning: ${high.length} high-severity issues found`)
    }
    
    return {
      name: 'Security',
      passed: true,
      message: 'No critical security issues',
      score: result.score
    }
  }
  
  /**
   * Run tests
   */
  private async runTests(context: DeploymentContext): Promise<ValidationResult> {
    console.log('  🧪 Running tests...')
    
    try {
      // Run npm test or equivalent
      const testCommand = context.projectType === 'node' ? 'npm test' : 'pytest'
      const result = await exec(testCommand, { cwd: context.projectPath })
      
      // Parse test results
      const summary = this.parseTestResults(result.stdout)
      
      if (summary.failed > 0) {
        return {
          name: 'Tests',
          passed: false,
          message: `${summary.failed} test(s) failed`,
          details: summary
        }
      }
      
      return {
        name: 'Tests',
        passed: true,
        message: `All ${summary.passed} tests passed`,
        details: summary
      }
    } catch (error) {
      return {
        name: 'Tests',
        passed: false,
        message: 'Test execution failed',
        error: error.message
      }
    }
  }
  
  /**
   * Validate environment configuration
   */
  private async validateEnvironment(
    context: DeploymentContext
  ): Promise<ValidationResult> {
    console.log('  🌍 Validating environment...')
    
    const issues: string[] = []
    
    // Check required environment variables
    const requiredVars = this.getRequiredEnvVars(context)
    
    for (const varName of requiredVars) {
      if (!context.environment.variables[varName]) {
        issues.push(`Missing required variable: ${varName}`)
      }
    }
    
    // Check database connection
    if (context.database) {
      const dbConnectable = await this.testDatabaseConnection(context.database)
      if (!dbConnectable) {
        issues.push('Cannot connect to database')
      }
    }
    
    // Check external services
    for (const service of context.externalServices || []) {
      const serviceUp = await this.testServiceConnection(service)
      if (!serviceUp) {
        issues.push(`Cannot connect to ${service.name}`)
      }
    }
    
    if (issues.length > 0) {
      return {
        name: 'Environment',
        passed: false,
        message: `${issues.length} environment issue(s)`,
        details: issues
      }
    }
    
    return {
      name: 'Environment',
      passed: true,
      message: 'Environment configuration valid'
    }
  }
  
  private failStage(message: string, results: ValidationResult[]): StageResult {
    return {
      stage: this.name,
      status: 'failed',
      message,
      data: { results }
    }
  }
}
```

---

## 📦 STAGE 2: BUILD

### Build and Package
```typescript
/**
 * Build Stage
 * Builds application and creates deployment artifact
 */
class BuildStage implements PipelineStage {
  name = 'Build'
  
  async execute(context: DeploymentContext): Promise<StageResult> {
    console.log('🔨 Building application...')
    
    const startTime = Date.now()
    
    try {
      // Step 1: Install dependencies
      await this.installDependencies(context)
      
      // Step 2: Run build command
      const buildOutput = await this.runBuild(context)
      
      // Step 3: Optimize assets
      await this.optimizeAssets(context)
      
      // Step 4: Create deployment artifact
      const artifact = await this.createArtifact(context)
      
      const duration = Date.now() - startTime
      
      console.log(`✅ Build complete (${Math.round(duration / 1000)}s)`)
      
      return {
        stage: this.name,
        status: 'success',
        duration,
        data: {
          artifact,
          buildOutput: buildOutput.substring(0, 1000)  // Truncate
        }
      }
    } catch (error) {
      return {
        stage: this.name,
        status: 'failed',
        message: `Build failed: ${error.message}`,
        error
      }
    }
  }
  
  /**
   * Install dependencies
   */
  private async installDependencies(context: DeploymentContext): Promise<void> {
    console.log('  📦 Installing dependencies...')
    
    // Use npm ci for reproducible installs
    const command = context.projectType === 'node' ? 'npm ci' : 'pip install -r requirements.txt'
    
    await exec(command, { cwd: context.projectPath })
  }
  
  /**
   * Run build
   */
  private async runBuild(context: DeploymentContext): Promise<string> {
    console.log('  🏗️  Running build...')
    
    // Detect build command
    const buildCommand = this.detectBuildCommand(context)
    
    const result = await exec(buildCommand, { 
      cwd: context.projectPath,
      env: {
        ...process.env,
        NODE_ENV: 'production'
      }
    })
    
    return result.stdout
  }
  
  /**
   * Optimize assets
   */
  private async optimizeAssets(context: DeploymentContext): Promise<void> {
    console.log('  ⚡ Optimizing assets...')
    
    // For Next.js/React apps, build already optimizes
    // For other apps, might need additional optimization
    
    if (context.framework === 'next') {
      // Next.js already optimizes during build
      return
    }
    
    // Compress images, minify CSS, etc.
    // Implementation depends on project type
  }
  
  /**
   * Create deployment artifact
   */
  private async createArtifact(context: DeploymentContext): Promise<Artifact> {
    console.log('  📦 Creating deployment artifact...')
    
    // For Docker deployments, build Docker image
    if (context.deploymentTarget.type === 'docker') {
      return await this.buildDockerImage(context)
    }
    
    // For serverless, create zip
    if (context.deploymentTarget.type === 'serverless') {
      return await this.createZipArchive(context)
    }
    
    // For static sites, just reference build directory
    return {
      type: 'directory',
      path: context.buildOutputPath,
      size: await this.getDirectorySize(context.buildOutputPath)
    }
  }
  
  private detectBuildCommand(context: DeploymentContext): string {
    // Check package.json for build script
    if (context.projectType === 'node') {
      const pkg = require(path.join(context.projectPath, 'package.json'))
      return pkg.scripts?.build ? 'npm run build' : 'echo "No build step"'
    }
    
    return 'echo "No build step"'
  }
}

interface Artifact {
  type: 'docker' | 'zip' | 'directory'
  path: string
  size: number
  checksum?: string
}
```

---

## 🧪 STAGE 3: STAGING DEPLOYMENT

### Deploy to Staging Environment
```typescript
/**
 * Staging Stage
 * Deploy to staging and run smoke tests
 */
class StagingStage implements PipelineStage {
  name = 'Staging'
  
  async execute(context: DeploymentContext): Promise<StageResult> {
    console.log('🔬 Deploying to staging...')
    
    try {
      // Step 1: Deploy to staging
      const deployment = await this.deployToStaging(context)
      
      // Step 2: Wait for deployment to be ready
      await this.waitForDeployment(deployment, 300000)  // 5 minutes max
      
      // Step 3: Run smoke tests
      const smokeTests = await this.runSmokeTests(deployment)
      
      if (!smokeTests.passed) {
        return {
          stage: this.name,
          status: 'failed',
          message: 'Smoke tests failed in staging',
          data: { smokeTests }
        }
      }
      
      // Step 4: Monitor for configured duration
      const monitorDuration = context.config.stagingDuration || 300000  // Default 5 min
      const health = await this.monitorHealth(deployment, monitorDuration)
      
      if (!health.healthy) {
        return {
          stage: this.name,
          status: 'failed',
          message: 'Health checks failed in staging',
          data: { health }
        }
      }
      
      console.log('✅ Staging deployment successful')
      
      return {
        stage: this.name,
        status: 'success',
        data: {
          deployment,
          smokeTests,
          health
        }
      }
    } catch (error) {
      return {
        stage: this.name,
        status: 'failed',
        message: `Staging deployment failed: ${error.message}`,
        error
      }
    }
  }
  
  /**
   * Deploy to staging environment
   */
  private async deployToStaging(
    context: DeploymentContext
  ): Promise<Deployment> {
    console.log('  🚀 Deploying to staging environment...')
    
    const platform = context.deploymentTarget.platform
    
    switch (platform) {
      case 'vercel':
        return await this.deployToVercel(context, 'staging')
      
      case 'netlify':
        return await this.deployToNetlify(context, 'staging')
      
      case 'railway':
        return await this.deployToRailway(context, 'staging')
      
      default:
        throw new Error(`Unsupported platform: ${platform}`)
    }
  }
  
  /**
   * Run smoke tests
   */
  private async runSmokeTests(deployment: Deployment): Promise<SmokeTestResult> {
    console.log('  🧪 Running smoke tests...')
    
    const tests = [
      {
        name: 'Homepage loads',
        test: async () => {
          const response = await fetch(deployment.url)
          return response.status === 200
        }
      },
      {
        name: 'API health check',
        test: async () => {
          const response = await fetch(`${deployment.url}/api/health`)
          return response.status === 200
        }
      },
      {
        name: 'Database connection',
        test: async () => {
          const response = await fetch(`${deployment.url}/api/health/db`)
          return response.status === 200
        }
      }
    ]
    
    const results = []
    let passed = 0
    let failed = 0
    
    for (const test of tests) {
      try {
        const result = await test.test()
        if (result) {
          console.log(`    ✓ ${test.name}`)
          passed++
        } else {
          console.log(`    ✗ ${test.name}`)
          failed++
        }
        results.push({ name: test.name, passed: result })
      } catch (error) {
        console.log(`    ✗ ${test.name}: ${error.message}`)
        failed++
        results.push({ name: test.name, passed: false, error: error.message })
      }
    }
    
    return {
      passed: failed === 0,
      total: tests.length,
      passed: passed,
      failed: failed,
      results
    }
  }
  
  /**
   * Monitor health for duration
   */
  private async monitorHealth(
    deployment: Deployment,
    duration: number
  ): Promise<HealthResult> {
    console.log(`  💓 Monitoring health for ${Math.round(duration / 1000)}s...`)
    
    const startTime = Date.now()
    const checks = []
    
    while (Date.now() - startTime < duration) {
      const health = await this.checkHealth(deployment)
      checks.push(health)
      
      if (!health.healthy) {
        return {
          healthy: false,
          reason: health.reason,
          checks
        }
      }
      
      // Wait 10 seconds between checks
      await this.sleep(10000)
    }
    
    console.log(`    ✓ Monitored ${checks.length} health checks, all passed`)
    
    return {
      healthy: true,
      checks
    }
  }
  
  /**
   * Check deployment health
   */
  private async checkHealth(deployment: Deployment): Promise<HealthCheck> {
    try {
      const start = Date.now()
      const response = await fetch(deployment.url, { timeout: 5000 })
      const responseTime = Date.now() - start
      
      return {
        timestamp: new Date(),
        healthy: response.status === 200 && responseTime < 2000,
        statusCode: response.status,
        responseTime,
        reason: response.status !== 200 ? `HTTP ${response.status}` :
                responseTime >= 2000 ? 'Slow response' : undefined
      }
    } catch (error) {
      return {
        timestamp: new Date(),
        healthy: false,
        reason: error.message
      }
    }
  }
  
  private async sleep(ms: number): Promise<void> {
    return new Promise(resolve => setTimeout(resolve, ms))
  }
}
```

---

## ✅ STAGE 4: APPROVAL

### Request Production Approval
```typescript
/**
 * Approval Stage
 * Request user approval for production deployment
 */
class ApprovalStage implements PipelineStage {
  name = 'Approval'
  
  async execute(context: DeploymentContext): Promise<StageResult> {
    // Auto-approve if configured
    if (context.config.autoApproveProduction) {
      console.log('⚠️  Auto-approving production deployment')
      return {
        stage: this.name,
        status: 'success',
        data: { autoApproved: true }
      }
    }
    
    console.log('⏸️  Requesting production deployment approval...')
    
    // Present deployment summary
    this.presentSummary(context)
    
    // Wait for user approval
    const approved = await this.requestApproval(context)
    
    if (!approved) {
      return {
        stage: this.name,
        status: 'failed',
        message: 'Production deployment cancelled by user'
      }
    }
    
    console.log('✅ Production deployment approved')
    
    return {
      stage: this.name,
      status: 'success',
      data: { approved: true }
    }
  }
  
  /**
   * Present deployment summary
   */
  private presentSummary(context: DeploymentContext): void {
    const summary = `
╔═══════════════════════════════════════════════════════════╗
║           PRODUCTION DEPLOYMENT APPROVAL                   ║
╚═══════════════════════════════════════════════════════════╝

Environment: ${context.environment.name}
Platform: ${context.deploymentTarget.platform}
Strategy: ${context.config.strategy}
URL: ${context.deploymentTarget.productionUrl}

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

VALIDATION RESULTS:
${this.formatValidation(context.validationResults)}

STAGING RESULTS:
${this.formatStaging(context.stagingResults)}

DEPLOYMENT PLAN:
${this.formatPlan(context)}

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

Estimated downtime: ${this.estimateDowntime(context)}
Estimated duration: ${this.estimateDuration(context)}
Rollback available: Yes (automatic on failure)

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

Proceed with production deployment? (Y/n)
`
    
    console.log(summary)
  }
  
  /**
   * Request user approval
   */
  private async requestApproval(context: DeploymentContext): Promise<boolean> {
    // In real implementation, this would wait for user input
    // For now, simulate approval
    
    // This would be replaced with actual user interaction
    return new Promise((resolve) => {
      // Simulate user approval after 5 seconds
      setTimeout(() => resolve(true), 5000)
    })
  }
  
  private estimateDowntime(context: DeploymentContext): string {
    if (context.config.strategy === 'blue-green') {
      return '0 seconds (zero-downtime deployment)'
    }
    
    if (context.config.strategy === 'canary') {
      return '0 seconds (gradual rollout)'
    }
    
    return '~30 seconds (rolling deployment)'
  }
  
  private estimateDuration(context: DeploymentContext): string {
    const minutes = Math.ceil(context.estimatedDuration / 60000)
    return `~${minutes} minute${minutes > 1 ? 's' : ''}`
  }
}
```

---

## 🚀 STAGE 5: PRODUCTION DEPLOYMENT

### Deploy to Production
```typescript
/**
 * Production Stage
 * Deploy to production using selected strategy
 */
class ProductionStage implements PipelineStage {
  name = 'Production'
  
  async execute(context: DeploymentContext): Promise<StageResult> {
    console.log('🚀 Deploying to production...')
    
    try {
      const strategy = this.selectStrategy(context.config.strategy)
      const result = await strategy.deploy(context)
      
      console.log('✅ Production deployment complete')
      
      return {
        stage: this.name,
        status: 'success',
        data: result
      }
    } catch (error) {
      return {
        stage: this.name,
        status: 'failed',
        message: `Production deployment failed: ${error.message}`,
        error
      }
    }
  }
  
  /**
   * Select deployment strategy
   */
  private selectStrategy(strategy: string): DeploymentStrategy {
    switch (strategy) {
      case 'blue-green':
        return new BlueGreenStrategy()
      
      case 'canary':
        return new CanaryStrategy()
      
      case 'rolling':
        return new RollingStrategy()
      
      default:
        throw new Error(`Unknown strategy: ${strategy}`)
    }
  }
}

/**
 * Blue-Green Deployment Strategy
 */
class BlueGreenStrategy implements DeploymentStrategy {
  async deploy(context: DeploymentContext): Promise<DeploymentResult> {
    console.log('  🔵🟢 Executing blue-green deployment...')
    
    // Step 1: Identify current environment (blue)
    const currentEnv = await this.getCurrentEnvironment(context)
    console.log(`    Current: ${currentEnv} environment`)
    
    // Step 2: Deploy to idle environment (green)
    const targetEnv = currentEnv === 'blue' ? 'green' : 'blue'
    console.log(`    Deploying to: ${targetEnv} environment`)
    
    const deployment = await this.deployToEnvironment(context, targetEnv)
    
    // Step 3: Run health checks on new environment
    console.log('    Running health checks...')
    const healthy = await this.verifyHealth(deployment)
    
    if (!healthy) {
      throw new Error('Health checks failed on new environment')
    }
    
    // Step 4: Switch traffic to new environment
    console.log('    Switching traffic...')
    await this.switchTraffic(context, targetEnv)
    
    // Step 5: Keep old environment running (for rollback)
    console.log(`    Keeping ${currentEnv} environment for rollback`)
    
    return {
      strategy: 'blue-green',
      oldEnvironment: currentEnv,
      newEnvironment: targetEnv,
      deployment,
      trafficSwitched: true,
      rollbackAvailable: true
    }
  }
  
  private async getCurrentEnvironment(
    context: DeploymentContext
  ): Promise<'blue' | 'green'> {
    // Query current active environment
    // Implementation depends on platform
    return 'blue'
  }
  
  private async deployToEnvironment(
    context: DeploymentContext,
    environment: 'blue' | 'green'
  ): Promise<Deployment> {
    // Deploy to specified environment
    // Implementation depends on platform
    return {
      id: generateId(),
      environment,
      url: `https://${environment}.${context.domain}`,
      timestamp: new Date()
    }
  }
  
  private async verifyHealth(deployment: Deployment): Promise<boolean> {
    // Run health checks (similar to staging)
    for (let i = 0; i < 5; i++) {
      const response = await fetch(deployment.url)
      if (response.status !== 200) {
        return false
      }
      await this.sleep(2000)
    }
    return true
  }
  
  private async switchTraffic(
    context: DeploymentContext,
    targetEnv: string
  ): Promise<void> {
    // Update load balancer / routing rules
    // Implementation depends on platform
  }
  
  private async sleep(ms: number): Promise<void> {
    return new Promise(resolve => setTimeout(resolve, ms))
  }
}

/**
 * Canary Deployment Strategy
 */
class CanaryStrategy implements DeploymentStrategy {
  async deploy(context: DeploymentContext): Promise<DeploymentResult> {
    console.log('  🐤 Executing canary deployment...')
    
    const phases = [
      { percentage: 10, duration: 300000 },   // 10% for 5 minutes
      { percentage: 50, duration: 300000 },   // 50% for 5 minutes
      { percentage: 100, duration: 0 }        // 100%
    ]
    
    for (const phase of phases) {
      console.log(`    Routing ${phase.percentage}% traffic to new version...`)
      
      await this.routeTraffic(context, phase.percentage)
      
      if (phase.duration > 0) {
        console.log(`    Monitoring for ${phase.duration / 60000} minutes...`)
        const healthy = await this.monitorPhase(context, phase.duration)
        
        if (!healthy) {
          throw new Error(`Canary phase failed at ${phase.percentage}%`)
        }
      }
    }
    
    console.log('    ✓ Canary deployment complete')
    
    return {
      strategy: 'canary',
      phases,
      rollbackAvailable: false  // Already at 100%
    }
  }
  
  private async routeTraffic(
    context: DeploymentContext,
    percentage: number
  ): Promise<void> {
    // Update routing to send percentage% to new version
    // Implementation depends on platform
  }
  
  private async monitorPhase(
    context: DeploymentContext,
    duration: number
  ): Promise<boolean> {
    // Monitor error rate and performance
    const startTime = Date.now()
    
    while (Date.now() - startTime < duration) {
      const metrics = await this.collectMetrics(context)
      
      if (metrics.errorRate > context.config.rollbackOnErrorRate) {
        return false
      }
      
      if (metrics.avgResponseTime > context.config.rollbackOnResponseTime) {
        return false
      }
      
      await this.sleep(10000)  // Check every 10 seconds
    }
    
    return true
  }
  
  private async collectMetrics(context: DeploymentContext): Promise<Metrics> {
    // Collect error rate and response time
    // Implementation depends on monitoring platform
    return {
      errorRate: 0.5,
      avgResponseTime: 250
    }
  }
  
  private async sleep(ms: number): Promise<void> {
    return new Promise(resolve => setTimeout(resolve, ms))
  }
}
```

---

## 💓 STAGE 6: MONITORING

### Health Monitoring and Rollback
```typescript
/**
 * Monitoring Stage
 * Monitor deployment health and trigger rollback if needed
 */
class MonitoringStage implements PipelineStage {
  name = 'Monitoring'
  
  async execute(context: DeploymentContext): Promise<StageResult> {
    console.log('💓 Monitoring deployment health...')
    
    const monitorDuration = context.config.healthCheckTimeout || 600000  // 10 min
    const startTime = Date.now()
    const checks = []
    
    while (Date.now() - startTime < monitorDuration) {
      const health = await this.checkHealth(context)
      checks.push(health)
      
      console.log(`  ${health.healthy ? '✓' : '✗'} Health check: ${this.formatHealth(health)}`)
      
      if (!health.healthy && context.config.autoRollback) {
        console.log('  ⚠️  Unhealthy deployment detected, triggering rollback...')
        
        const rollback = await this.performRollback(context)
        
        return {
          stage: this.name,
          status: 'failed',
          message: 'Deployment rolled back due to health check failure',
          data: {
            checks,
            rollback
          }
        }
      }
      
      await this.sleep(context.config.healthCheckInterval || 10000)
    }
    
    console.log('✅ Deployment is healthy')
    
    return {
      stage: this.name,
      status: 'success',
      data: { checks }
    }
  }
  
  /**
   * Check deployment health
   */
  private async checkHealth(context: DeploymentContext): Promise<HealthCheck> {
    try {
      // HTTP health check
      const httpHealth = await this.checkHTTP(context)
      
      // Metrics check (error rate, response time)
      const metricsHealth = await this.checkMetrics(context)
      
      return {
        timestamp: new Date(),
        healthy: httpHealth.healthy && metricsHealth.healthy,
        http: httpHealth,
        metrics: metricsHealth
      }
    } catch (error) {
      return {
        timestamp: new Date(),
        healthy: false,
        error: error.message
      }
    }
  }
  
  /**
   * Perform automatic rollback
   */
  private async performRollback(
    context: DeploymentContext
  ): Promise<RollbackResult> {
    console.log('  🔄 Rolling back deployment...')
    
    const startTime = Date.now()
    
    try {
      const strategy = this.selectStrategy(context.config.strategy)
      await strategy.rollback(context)
      
      const duration = Date.now() - startTime
      
      console.log(`  ✓ Rollback complete (${Math.round(duration / 1000)}s)`)
      
      return {
        success: true,
        duration,
        timestamp: new Date()
      }
    } catch (error) {
      console.log(`  ✗ Rollback failed: ${error.message}`)
      
      return {
        success: false,
        error: error.message,
        timestamp: new Date()
      }
    }
  }
  
  private formatHealth(health: HealthCheck): string {
    if (health.http) {
      return `HTTP ${health.http.statusCode} (${health.http.responseTime}ms)`
    }
    return health.error || 'Unknown'
  }
  
  private async sleep(ms: number): Promise<void> {
    return new Promise(resolve => setTimeout(resolve, ms))
  }
}
```

---

## 📊 DEPLOYMENT ORCHESTRATOR

### Complete Pipeline Execution
```typescript
/**
 * Deployment Orchestrator
 * Executes the complete deployment pipeline
 */
class DeploymentOrchestrator {
  private pipeline: DeploymentPipeline
  private context: DeploymentContext
  
  constructor(pipeline: DeploymentPipeline, context: DeploymentContext) {
    this.pipeline = pipeline
    this.context = context
  }
  
  /**
   * Execute deployment pipeline
   */
  async execute(): Promise<PipelineResult> {
    console.log('🚀 Starting deployment pipeline...')
    console.log(`  Target: ${this.context.environment.name}`)
    console.log(`  Strategy: ${this.context.config.strategy}`)
    console.log('')
    
    const startTime = Date.now()
    const stageResults: StageResult[] = []
    
    try {
      for (let i = 0; i < this.pipeline.stages.length; i++) {
        const stage = this.pipeline.stages[i]
        
        console.log(`━━━ Stage ${i + 1}/${this.pipeline.stages.length}: ${stage.name} ━━━`)
        
        this.context.stageStartTime = Date.now()
        const result = await stage.execute(this.context)
        
        stageResults.push(result)
        
        if (result.status === 'failed') {
          console.log(`✗ Stage failed: ${result.message}`)
          
          return this.failPipeline(stageResults, startTime)
        }
        
        console.log('')
      }
      
      const duration = Date.now() - startTime
      
      console.log('━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━')
      console.log(`✅ DEPLOYMENT SUCCESSFUL (${Math.round(duration / 1000)}s)`)
      console.log('━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━')
      
      return {
        success: true,
        duration,
        stages: stageResults
      }
      
    } catch (error) {
      console.log(`✗ Pipeline error: ${error.message}`)
      
      return this.failPipeline(stageResults, startTime, error)
    }
  }
  
  /**
   * Handle pipeline failure
   */
  private failPipeline(
    stages: StageResult[],
    startTime: number,
    error?: Error
  ): PipelineResult {
    const duration = Date.now() - startTime
    
    console.log('━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━')
    console.log(`✗ DEPLOYMENT FAILED (${Math.round(duration / 1000)}s)`)
    console.log('━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━')
    
    return {
      success: false,
      duration,
      stages,
      error: error?.message
    }
  }
}

/**
 * Supporting Types
 */
interface PipelineResult {
  success: boolean
  duration: number
  stages: StageResult[]
  error?: string
}

interface StageResult {
  stage: string
  status: 'success' | 'failed'
  duration?: number
  message?: string
  data?: any
  error?: any
}
```

---

## 📚 RELATED ARTICLES

| Article | Purpose | Relationship to Pipeline |
|---------|---------|--------------------------|
| **Article II: Environment Management** | Env config | Used in validation stage |
| **Article III: Database Migrations** | Schema changes | Executed during deployment |
| **Article IV: Deployment Monitoring** | Health checks | Used in monitoring stage |
| **Segment 5: Quality Layer** | Validation | Quality gates in validation stage |

---

**Previous:** [← Segment 6 README](./README.md)  
**Next:** [Article II: Environment Management →](./02-article-ii-environment-management.md)

---

**Last Updated:** February 7, 2026  
**Constitutional Version:** 2.0.0  
**Status:** ✅ Ratified and In Force

**Motto:** *"Deploy Once, Verify Always - Every Deployment Must Prove Its Health"*
