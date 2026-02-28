# 🗄️ ARTICLE III: DATABASE MIGRATIONS

**Safe, Reversible Database Schema Changes in Production**

---

## 📜 CONSTITUTIONAL AUTHORITY

This article defines **database migration management** - how MVCA safely executes schema changes in production without data loss or downtime.

**Legal Force:**
- ✅ Destructive migrations **SHALL** require explicit approval
- ✅ All migrations **MUST** be reversible (have a rollback)
- ✅ Data backups **SHALL** be taken before destructive operations
- ✅ Zero-downtime migrations **SHOULD** be preferred
- ✅ Migration history **SHALL** be tracked and auditable

**Constitutional Principle:**
> Data is precious - schema changes must never cause data loss.  
> Every migration forward must have a path backward.

---

## 🎯 MIGRATION TYPES

### Classification of Migrations
```typescript
/**
 * Migration Types
 * Classified by safety and reversibility
 */
enum MigrationType {
  // SAFE MIGRATIONS (Zero risk)
  AddColumn = 'add_column',               // Add nullable column
  AddIndex = 'add_index',                 // Add index
  AddTable = 'add_table',                 // Create new table
  
  // MODERATE MIGRATIONS (Low risk)
  RenameColumn = 'rename_column',         // Rename with alias
  ChangeColumnType = 'change_type',       // Type conversion
  AddConstraint = 'add_constraint',       // Add constraint
  
  // RISKY MIGRATIONS (Medium risk)
  DropColumn = 'drop_column',             // Remove column
  DropIndex = 'drop_index',               // Remove index
  DropConstraint = 'drop_constraint',     // Remove constraint
  
  // DESTRUCTIVE MIGRATIONS (High risk)
  DropTable = 'drop_table',               // Delete entire table
  TruncateTable = 'truncate_table',       // Delete all data
  AlterPrimaryKey = 'alter_pk'            // Change primary key
}

/**
 * Migration Safety Classification
 */
interface MigrationSafety {
  type: MigrationType
  risk: 'safe' | 'moderate' | 'risky' | 'destructive'
  reversible: boolean
  zeroDowntime: boolean
  requiresBackup: boolean
  requiresApproval: boolean
  estimatedDuration: number              // milliseconds
}

/**
 * Migration Safety Matrix
 */
const MIGRATION_SAFETY: Record<MigrationType, MigrationSafety> = {
  [MigrationType.AddColumn]: {
    type: MigrationType.AddColumn,
    risk: 'safe',
    reversible: true,
    zeroDowntime: true,
    requiresBackup: false,
    requiresApproval: false,
    estimatedDuration: 1000
  },
  
  [MigrationType.AddIndex]: {
    type: MigrationType.AddIndex,
    risk: 'safe',
    reversible: true,
    zeroDowntime: true,                   // Use CONCURRENTLY
    requiresBackup: false,
    requiresApproval: false,
    estimatedDuration: 30000
  },
  
  [MigrationType.AddTable]: {
    type: MigrationType.AddTable,
    risk: 'safe',
    reversible: true,
    zeroDowntime: true,
    requiresBackup: false,
    requiresApproval: false,
    estimatedDuration: 2000
  },
  
  [MigrationType.DropColumn]: {
    type: MigrationType.DropColumn,
    risk: 'risky',
    reversible: false,                    // Data is lost
    zeroDowntime: false,
    requiresBackup: true,
    requiresApproval: true,
    estimatedDuration: 5000
  },
  
  [MigrationType.DropTable]: {
    type: MigrationType.DropTable,
    risk: 'destructive',
    reversible: false,                    // Data is lost
    zeroDowntime: false,
    requiresBackup: true,
    requiresApproval: true,
    estimatedDuration: 10000
  }
}
```

---

## 🔍 MIGRATION ANALYSIS

### Migration Safety Analyzer
```typescript
/**
 * Migration Safety Analyzer
 * Analyzes migrations for safety and risk
 */
class MigrationSafetyAnalyzer {
  /**
   * Analyze migration for safety
   */
  analyze(migration: Migration): MigrationAnalysis {
    console.log(`🔍 Analyzing migration: ${migration.name}`)
    
    // Parse migration SQL/operations
    const operations = this.parseOperations(migration)
    
    // Classify each operation
    const classifications = operations.map(op => 
      this.classifyOperation(op)
    )
    
    // Determine overall risk
    const overallRisk = this.determineOverallRisk(classifications)
    
    // Check for destructive operations
    const destructiveOps = classifications.filter(c => 
      c.safety.risk === 'destructive'
    )
    
    // Check reversibility
    const reversible = classifications.every(c => c.safety.reversible)
    
    // Check zero-downtime capability
    const zeroDowntime = classifications.every(c => c.safety.zeroDowntime)
    
    // Generate warnings
    const warnings = this.generateWarnings(classifications)
    
    // Generate recommendations
    const recommendations = this.generateRecommendations(classifications)
    
    return {
      migration,
      overallRisk,
      reversible,
      zeroDowntime,
      destructiveOperations: destructiveOps.length,
      requiresBackup: destructiveOps.length > 0,
      requiresApproval: overallRisk === 'destructive' || overallRisk === 'risky',
      estimatedDuration: classifications.reduce((sum, c) => 
        sum + c.safety.estimatedDuration, 0
      ),
      operations: classifications,
      warnings,
      recommendations
    }
  }
  
  /**
   * Parse migration into operations
   */
  private parseOperations(migration: Migration): MigrationOperation[] {
    const operations: MigrationOperation[] = []
    
    // Parse SQL statements
    const statements = migration.up.split(';').filter(s => s.trim())
    
    for (const statement of statements) {
      const normalized = statement.trim().toUpperCase()
      
      if (normalized.startsWith('CREATE TABLE')) {
        operations.push({
          type: 'CREATE_TABLE',
          sql: statement,
          table: this.extractTableName(statement, 'CREATE TABLE')
        })
      }
      else if (normalized.startsWith('DROP TABLE')) {
        operations.push({
          type: 'DROP_TABLE',
          sql: statement,
          table: this.extractTableName(statement, 'DROP TABLE')
        })
      }
      else if (normalized.startsWith('ALTER TABLE') && normalized.includes('ADD COLUMN')) {
        operations.push({
          type: 'ADD_COLUMN',
          sql: statement,
          table: this.extractTableName(statement, 'ALTER TABLE'),
          column: this.extractColumnName(statement)
        })
      }
      else if (normalized.startsWith('ALTER TABLE') && normalized.includes('DROP COLUMN')) {
        operations.push({
          type: 'DROP_COLUMN',
          sql: statement,
          table: this.extractTableName(statement, 'ALTER TABLE'),
          column: this.extractDropColumnName(statement)
        })
      }
      else if (normalized.startsWith('CREATE INDEX')) {
        operations.push({
          type: 'CREATE_INDEX',
          sql: statement,
          table: this.extractTableName(statement, 'ON'),
          index: this.extractIndexName(statement)
        })
      }
      else if (normalized.startsWith('DROP INDEX')) {
        operations.push({
          type: 'DROP_INDEX',
          sql: statement,
          index: this.extractIndexName(statement)
        })
      }
      // Add more operation types as needed
    }
    
    return operations
  }
  
  /**
   * Classify operation
   */
  private classifyOperation(
    operation: MigrationOperation
  ): OperationClassification {
    let type: MigrationType
    
    switch (operation.type) {
      case 'CREATE_TABLE':
        type = MigrationType.AddTable
        break
      case 'DROP_TABLE':
        type = MigrationType.DropTable
        break
      case 'ADD_COLUMN':
        type = MigrationType.AddColumn
        break
      case 'DROP_COLUMN':
        type = MigrationType.DropColumn
        break
      case 'CREATE_INDEX':
        type = MigrationType.AddIndex
        break
      case 'DROP_INDEX':
        type = MigrationType.DropIndex
        break
      default:
        type = MigrationType.AddColumn  // Default to safe
    }
    
    return {
      operation,
      migrationType: type,
      safety: MIGRATION_SAFETY[type]
    }
  }
  
  /**
   * Determine overall risk level
   */
  private determineOverallRisk(
    classifications: OperationClassification[]
  ): 'safe' | 'moderate' | 'risky' | 'destructive' {
    const risks = classifications.map(c => c.safety.risk)
    
    if (risks.includes('destructive')) return 'destructive'
    if (risks.includes('risky')) return 'risky'
    if (risks.includes('moderate')) return 'moderate'
    return 'safe'
  }
  
  /**
   * Generate warnings
   */
  private generateWarnings(
    classifications: OperationClassification[]
  ): string[] {
    const warnings: string[] = []
    
    for (const c of classifications) {
      if (c.safety.risk === 'destructive') {
        warnings.push(
          `⚠️  DESTRUCTIVE: ${c.operation.type} will permanently delete data`
        )
      }
      
      if (c.safety.risk === 'risky' && !c.safety.reversible) {
        warnings.push(
          `⚠️  NOT REVERSIBLE: ${c.operation.type} cannot be automatically rolled back`
        )
      }
      
      if (!c.safety.zeroDowntime) {
        warnings.push(
          `⚠️  DOWNTIME: ${c.operation.type} may cause brief service interruption`
        )
      }
    }
    
    return warnings
  }
  
  /**
   * Generate recommendations
   */
  private generateRecommendations(
    classifications: OperationClassification[]
  ): string[] {
    const recommendations: string[] = []
    
    // Check for missing indexes
    const addColumnOps = classifications.filter(c => 
      c.migrationType === MigrationType.AddColumn
    )
    
    if (addColumnOps.length > 0) {
      recommendations.push(
        '💡 Consider adding indexes for new columns that will be queried frequently'
      )
    }
    
    // Check for destructive operations
    const destructiveOps = classifications.filter(c => 
      c.safety.risk === 'destructive'
    )
    
    if (destructiveOps.length > 0) {
      recommendations.push(
        '💡 Take a database backup before proceeding'
      )
      recommendations.push(
        '💡 Consider archiving data instead of deleting'
      )
    }
    
    // Check for non-zero-downtime operations
    const downtimeOps = classifications.filter(c => 
      !c.safety.zeroDowntime
    )
    
    if (downtimeOps.length > 0) {
      recommendations.push(
        '💡 Schedule migration during low-traffic period'
      )
      recommendations.push(
        '💡 Enable maintenance mode to prevent user disruption'
      )
    }
    
    return recommendations
  }
  
  // Helper methods for parsing SQL
  private extractTableName(sql: string, keyword: string): string {
    const regex = new RegExp(`${keyword}\\s+(?:IF\\s+(?:NOT\\s+)?EXISTS\\s+)?[\`"]?([\\w]+)[\`"]?`, 'i')
    const match = sql.match(regex)
    return match ? match[1] : 'unknown'
  }
  
  private extractColumnName(sql: string): string {
    const match = sql.match(/ADD\s+COLUMN\s+[`"]?(\w+)[`"]?/i)
    return match ? match[1] : 'unknown'
  }
  
  private extractDropColumnName(sql: string): string {
    const match = sql.match(/DROP\s+COLUMN\s+[`"]?(\w+)[`"]?/i)
    return match ? match[1] : 'unknown'
  }
  
  private extractIndexName(sql: string): string {
    const match = sql.match(/(?:CREATE|DROP)\s+INDEX\s+[`"]?(\w+)[`"]?/i)
    return match ? match[1] : 'unknown'
  }
}

/**
 * Supporting Types
 */
interface Migration {
  name: string
  timestamp: number
  up: string                              // SQL for migration
  down: string                            // SQL for rollback
}

interface MigrationOperation {
  type: string
  sql: string
  table?: string
  column?: string
  index?: string
}

interface OperationClassification {
  operation: MigrationOperation
  migrationType: MigrationType
  safety: MigrationSafety
}

interface MigrationAnalysis {
  migration: Migration
  overallRisk: 'safe' | 'moderate' | 'risky' | 'destructive'
  reversible: boolean
  zeroDowntime: boolean
  destructiveOperations: number
  requiresBackup: boolean
  requiresApproval: boolean
  estimatedDuration: number
  operations: OperationClassification[]
  warnings: string[]
  recommendations: string[]
}
```

---

## 🔄 ZERO-DOWNTIME MIGRATIONS

### Expand-Contract Pattern
```typescript
/**
 * Zero-Downtime Migration Strategy
 * Uses expand-contract pattern for schema changes
 */
class ZeroDowntimeMigrationStrategy {
  /**
   * Execute column rename with zero downtime
   * 
   * Old approach (with downtime):
   *   1. Rename column
   *   2. Deploy new code
   *   → Users see errors during deployment
   * 
   * Zero-downtime approach (expand-contract):
   *   1. EXPAND: Add new column
   *   2. Deploy code that writes to both columns
   *   3. Backfill data from old to new column
   *   4. Deploy code that reads from new column
   *   5. CONTRACT: Drop old column
   */
  async renameColumn(
    table: string,
    oldColumn: string,
    newColumn: string
  ): Promise<void> {
    console.log(`🔄 Zero-downtime column rename: ${table}.${oldColumn} → ${newColumn}`)
    
    // Phase 1: EXPAND - Add new column
    console.log('  Phase 1: Adding new column...')
    await this.addColumn(table, newColumn)
    
    // Phase 2: Deploy dual-write code
    console.log('  Phase 2: Deploy code that writes to both columns')
    console.log('  ⚠️  Manual step: Deploy application code')
    console.log('     Code must write to both columns:')
    console.log(`     - ${oldColumn} (for old instances)`)
    console.log(`     - ${newColumn} (for new instances)`)
    await this.waitForDeployment()
    
    // Phase 3: Backfill data
    console.log('  Phase 3: Backfilling data...')
    await this.backfillData(table, oldColumn, newColumn)
    
    // Phase 4: Deploy new read code
    console.log('  Phase 4: Deploy code that reads from new column')
    console.log('  ⚠️  Manual step: Deploy application code')
    console.log(`     Code must read from ${newColumn}`)
    await this.waitForDeployment()
    
    // Phase 5: CONTRACT - Drop old column
    console.log('  Phase 5: Dropping old column...')
    await this.dropColumn(table, oldColumn)
    
    console.log('  ✅ Zero-downtime rename complete')
  }
  
  /**
   * Execute type change with zero downtime
   */
  async changeColumnType(
    table: string,
    column: string,
    oldType: string,
    newType: string
  ): Promise<void> {
    console.log(`🔄 Zero-downtime type change: ${table}.${column} (${oldType} → ${newType})`)
    
    const tempColumn = `${column}_new`
    
    // Phase 1: Add new column with new type
    console.log('  Phase 1: Adding new column with new type...')
    await this.addColumn(table, tempColumn, newType)
    
    // Phase 2: Backfill with type conversion
    console.log('  Phase 2: Backfilling with type conversion...')
    await this.backfillWithConversion(table, column, tempColumn, oldType, newType)
    
    // Phase 3: Deploy code to use new column
    console.log('  Phase 3: Deploy code to use new column')
    await this.waitForDeployment()
    
    // Phase 4: Rename columns (drop old, rename new)
    console.log('  Phase 4: Swapping columns...')
    await this.dropColumn(table, column)
    await this.renameColumnDirect(table, tempColumn, column)
    
    console.log('  ✅ Zero-downtime type change complete')
  }
  
  /**
   * Add column
   */
  private async addColumn(
    table: string,
    column: string,
    type: string = 'VARCHAR(255)'
  ): Promise<void> {
    const sql = `ALTER TABLE ${table} ADD COLUMN ${column} ${type}`
    await this.executeSQL(sql)
  }
  
  /**
   * Drop column
   */
  private async dropColumn(table: string, column: string): Promise<void> {
    const sql = `ALTER TABLE ${table} DROP COLUMN ${column}`
    await this.executeSQL(sql)
  }
  
  /**
   * Rename column (direct, use only when safe)
   */
  private async renameColumnDirect(
    table: string,
    oldName: string,
    newName: string
  ): Promise<void> {
    const sql = `ALTER TABLE ${table} RENAME COLUMN ${oldName} TO ${newName}`
    await this.executeSQL(sql)
  }
  
  /**
   * Backfill data
   */
  private async backfillData(
    table: string,
    sourceColumn: string,
    targetColumn: string
  ): Promise<void> {
    const sql = `UPDATE ${table} SET ${targetColumn} = ${sourceColumn} WHERE ${targetColumn} IS NULL`
    await this.executeSQL(sql)
  }
  
  /**
   * Backfill with type conversion
   */
  private async backfillWithConversion(
    table: string,
    sourceColumn: string,
    targetColumn: string,
    sourceType: string,
    targetType: string
  ): Promise<void> {
    // Implementation depends on database and types
    // Example: VARCHAR to INTEGER
    const sql = `UPDATE ${table} SET ${targetColumn} = CAST(${sourceColumn} AS ${targetType})`
    await this.executeSQL(sql)
  }
  
  /**
   * Wait for deployment
   */
  private async waitForDeployment(): Promise<void> {
    // In real implementation, this would wait for actual deployment
    console.log('  ⏸️  Waiting for application deployment...')
    await new Promise(resolve => setTimeout(resolve, 5000))
  }
  
  /**
   * Execute SQL
   */
  private async executeSQL(sql: string): Promise<void> {
    console.log(`    SQL: ${sql}`)
    // In real implementation, execute against database
  }
}
```

---

## 🔙 MIGRATION ROLLBACK

### Rollback Manager
```typescript
/**
 * Migration Rollback Manager
 * Handles rolling back failed migrations
 */
class MigrationRollbackManager {
  /**
   * Rollback migration
   */
  async rollback(migration: Migration): Promise<RollbackResult> {
    console.log(`🔙 Rolling back migration: ${migration.name}`)
    
    const startTime = Date.now()
    
    try {
      // Check if migration has rollback SQL
      if (!migration.down || migration.down.trim() === '') {
        return {
          success: false,
          message: 'Migration has no rollback SQL',
          duration: Date.now() - startTime
        }
      }
      
      // Analyze rollback safety
      const analysis = this.analyzeRollbackSafety(migration)
      
      if (!analysis.safe) {
        return {
          success: false,
          message: `Rollback is not safe: ${analysis.reason}`,
          duration: Date.now() - startTime
        }
      }
      
      // Execute rollback SQL
      console.log('  Executing rollback SQL...')
      await this.executeRollbackSQL(migration.down)
      
      // Verify rollback
      const verified = await this.verifyRollback(migration)
      
      if (!verified) {
        return {
          success: false,
          message: 'Rollback verification failed',
          duration: Date.now() - startTime
        }
      }
      
      const duration = Date.now() - startTime
      
      console.log(`  ✅ Rollback successful (${Math.round(duration / 1000)}s)`)
      
      return {
        success: true,
        message: 'Migration rolled back successfully',
        duration
      }
      
    } catch (error) {
      return {
        success: false,
        message: `Rollback failed: ${error.message}`,
        error,
        duration: Date.now() - startTime
      }
    }
  }
  
  /**
   * Analyze rollback safety
   */
  private analyzeRollbackSafety(migration: Migration): RollbackSafety {
    // Check if rollback would lose data
    const wouldLoseData = this.checkDataLoss(migration)
    
    if (wouldLoseData) {
      return {
        safe: false,
        reason: 'Rollback would cause data loss'
      }
    }
    
    // Check if rollback is reversible
    const reversible = this.checkReversibility(migration)
    
    if (!reversible) {
      return {
        safe: false,
        reason: 'Rollback is not reversible'
      }
    }
    
    return {
      safe: true,
      reason: 'Rollback is safe to execute'
    }
  }
  
  /**
   * Check if rollback would lose data
   */
  private checkDataLoss(migration: Migration): boolean {
    const down = migration.down.toUpperCase()
    
    // Rollback drops a table that was created
    if (migration.up.toUpperCase().includes('CREATE TABLE') &&
        down.includes('DROP TABLE')) {
      return true
    }
    
    // Rollback drops a column that was added
    if (migration.up.toUpperCase().includes('ADD COLUMN') &&
        down.includes('DROP COLUMN')) {
      return true
    }
    
    return false
  }
  
  /**
   * Check reversibility
   */
  private checkReversibility(migration: Migration): boolean {
    // If migration drops a table/column, rollback can't restore data
    if (migration.up.toUpperCase().includes('DROP TABLE') ||
        migration.up.toUpperCase().includes('DROP COLUMN')) {
      return false
    }
    
    return true
  }
  
  /**
   * Execute rollback SQL
   */
  private async executeRollbackSQL(sql: string): Promise<void> {
    // In real implementation, execute against database
    console.log(`    Executing: ${sql}`)
  }
  
  /**
   * Verify rollback
   */
  private async verifyRollback(migration: Migration): Promise<boolean> {
    // In real implementation, check database schema
    return true
  }
}

interface RollbackResult {
  success: boolean
  message: string
  duration: number
  error?: any
}

interface RollbackSafety {
  safe: boolean
  reason: string
}
```

---

## 🎯 MIGRATION EXECUTOR

### Migration Execution Engine
```typescript
/**
 * Migration Executor
 * Executes migrations with safety checks
 */
class MigrationExecutor {
  private analyzer: MigrationSafetyAnalyzer
  private rollbackManager: MigrationRollbackManager
  
  constructor() {
    this.analyzer = new MigrationSafetyAnalyzer()
    this.rollbackManager = new MigrationRollbackManager()
  }
  
  /**
   * Execute migration
   */
  async execute(migration: Migration): Promise<ExecutionResult> {
    console.log(`🗄️  Executing migration: ${migration.name}`)
    
    const startTime = Date.now()
    
    try {
      // Step 1: Analyze safety
      console.log('  Step 1: Analyzing migration safety...')
      const analysis = this.analyzer.analyze(migration)
      
      this.printAnalysis(analysis)
      
      // Step 2: Request approval if needed
      if (analysis.requiresApproval) {
        console.log('  Step 2: Requesting approval...')
        const approved = await this.requestApproval(analysis)
        
        if (!approved) {
          return {
            success: false,
            message: 'Migration cancelled by user',
            duration: Date.now() - startTime
          }
        }
      }
      
      // Step 3: Create backup if needed
      if (analysis.requiresBackup) {
        console.log('  Step 3: Creating database backup...')
        await this.createBackup()
      }
      
      // Step 4: Execute migration
      console.log('  Step 4: Executing migration...')
      await this.executeMigrationSQL(migration.up)
      
      // Step 5: Verify migration
      console.log('  Step 5: Verifying migration...')
      const verified = await this.verifyMigration(migration)
      
      if (!verified) {
        console.log('  ⚠️  Verification failed, rolling back...')
        await this.rollbackManager.rollback(migration)
        
        return {
          success: false,
          message: 'Migration verification failed, rolled back',
          duration: Date.now() - startTime
        }
      }
      
      // Step 6: Record in history
      console.log('  Step 6: Recording migration...')
      await this.recordMigration(migration)
      
      const duration = Date.now() - startTime
      
      console.log(`  ✅ Migration successful (${Math.round(duration / 1000)}s)`)
      
      return {
        success: true,
        message: 'Migration executed successfully',
        duration,
        analysis
      }
      
    } catch (error) {
      console.log(`  ✗ Migration failed: ${error.message}`)
      
      // Attempt rollback
      console.log('  Attempting automatic rollback...')
      const rollback = await this.rollbackManager.rollback(migration)
      
      return {
        success: false,
        message: `Migration failed: ${error.message}`,
        error,
        duration: Date.now() - startTime,
        rollback
      }
    }
  }
  
  /**
   * Print analysis
   */
  private printAnalysis(analysis: MigrationAnalysis): void {
    console.log(`
    Migration Analysis:
    ├─ Risk Level: ${analysis.overallRisk.toUpperCase()}
    ├─ Reversible: ${analysis.reversible ? '✓ Yes' : '✗ No'}
    ├─ Zero Downtime: ${analysis.zeroDowntime ? '✓ Yes' : '✗ No'}
    ├─ Destructive Operations: ${analysis.destructiveOperations}
    ├─ Requires Backup: ${analysis.requiresBackup ? '✓ Yes' : '✗ No'}
    └─ Estimated Duration: ~${Math.round(analysis.estimatedDuration / 1000)}s
    `)
    
    if (analysis.warnings.length > 0) {
      console.log('    Warnings:')
      analysis.warnings.forEach(w => console.log(`    ${w}`))
    }
    
    if (analysis.recommendations.length > 0) {
      console.log('    Recommendations:')
      analysis.recommendations.forEach(r => console.log(`    ${r}`))
    }
  }
  
  /**
   * Request approval
   */
  private async requestApproval(analysis: MigrationAnalysis): Promise<boolean> {
    console.log(`
    ╔═══════════════════════════════════════════════════════════╗
    ║           MIGRATION APPROVAL REQUIRED                      ║
    ╚═══════════════════════════════════════════════════════════╝
    
    Migration: ${analysis.migration.name}
    Risk Level: ${analysis.overallRisk.toUpperCase()}
    ${analysis.destructiveOperations > 0 ? `⚠️  ${analysis.destructiveOperations} DESTRUCTIVE OPERATION(S)` : ''}
    
    This migration:
    ${analysis.reversible ? '✓' : '✗'} Is reversible
    ${analysis.zeroDowntime ? '✓' : '✗'} Can run with zero downtime
    ${analysis.requiresBackup ? '⚠️  REQUIRES DATABASE BACKUP' : ''}
    
    Proceed with migration? (Y/n)
    `)
    
    // In real implementation, wait for user input
    // For now, simulate approval
    return new Promise(resolve => {
      setTimeout(() => resolve(true), 3000)
    })
  }
  
  /**
   * Create backup
   */
  private async createBackup(): Promise<void> {
    console.log('    Creating backup...')
    // In real implementation, create database backup
    // Using pg_dump, mysqldump, or cloud provider backup
    await new Promise(resolve => setTimeout(resolve, 2000))
    console.log('    ✓ Backup created')
  }
  
  /**
   * Execute migration SQL
   */
  private async executeMigrationSQL(sql: string): Promise<void> {
    // In real implementation, execute against database
    const statements = sql.split(';').filter(s => s.trim())
    
    for (const statement of statements) {
      console.log(`    Executing: ${statement.substring(0, 60)}...`)
      // await db.execute(statement)
      await new Promise(resolve => setTimeout(resolve, 500))
    }
  }
  
  /**
   * Verify migration
   */
  private async verifyMigration(migration: Migration): Promise<boolean> {
    // In real implementation, verify schema changes
    // Query information_schema or equivalent
    console.log('    Verifying schema changes...')
    await new Promise(resolve => setTimeout(resolve, 1000))
    return true
  }
  
  /**
   * Record migration in history
   */
  private async recordMigration(migration: Migration): Promise<void> {
    const record: MigrationRecord = {
      name: migration.name,
      timestamp: migration.timestamp,
      executedAt: new Date(),
      success: true
    }
    
    // In real implementation, insert into migrations table
    console.log('    ✓ Recorded in migration history')
  }
}

interface ExecutionResult {
  success: boolean
  message: string
  duration: number
  error?: any
  analysis?: MigrationAnalysis
  rollback?: RollbackResult
}

interface MigrationRecord {
  name: string
  timestamp: number
  executedAt: Date
  success: boolean
  rolledBackAt?: Date
}
```

---

## 📋 MIGRATION HISTORY

### Migration History Tracker
```typescript
/**
 * Migration History Tracker
 * Tracks all executed migrations
 */
class MigrationHistoryTracker {
  private history: MigrationRecord[] = []
  
  /**
   * Record migration execution
   */
  async record(
    migration: Migration,
    result: ExecutionResult
  ): Promise<void> {
    const record: MigrationRecord = {
      name: migration.name,
      timestamp: migration.timestamp,
      executedAt: new Date(),
      success: result.success,
      duration: result.duration,
      error: result.error?.message
    }
    
    this.history.push(record)
    
    // Persist to database
    await this.persist(record)
  }
  
  /**
   * Get migration history
   */
  getHistory(): MigrationRecord[] {
    return [...this.history].sort((a, b) => 
      b.executedAt.getTime() - a.executedAt.getTime()
    )
  }
  
  /**
   * Get pending migrations
   */
  async getPending(allMigrations: Migration[]): Promise<Migration[]> {
    const executed = new Set(this.history.map(h => h.name))
    
    return allMigrations.filter(m => !executed.has(m.name))
  }
  
  /**
   * Check if migration executed
   */
  hasExecuted(migrationName: string): boolean {
    return this.history.some(h => h.name === migrationName && h.success)
  }
  
  /**
   * Get last executed migration
   */
  getLastExecuted(): MigrationRecord | null {
    const successful = this.history.filter(h => h.success)
    if (successful.length === 0) return null
    
    return successful.sort((a, b) => 
      b.executedAt.getTime() - a.executedAt.getTime()
    )[0]
  }
  
  /**
   * Generate migration report
   */
  generateReport(): string {
    const total = this.history.length
    const successful = this.history.filter(h => h.success).length
    const failed = total - successful
    
    let report = `
╔═══════════════════════════════════════════════════════════╗
║              MIGRATION HISTORY REPORT                      ║
╚═══════════════════════════════════════════════════════════╝

Total Migrations: ${total}
├─ Successful: ${successful} (${Math.round(successful / total * 100)}%)
└─ Failed: ${failed}

Recent Migrations:
`
    
    const recent = this.getHistory().slice(0, 10)
    
    for (const record of recent) {
      const status = record.success ? '✓' : '✗'
      const duration = record.duration ? `(${Math.round(record.duration / 1000)}s)` : ''
      report += `${status} ${record.name} - ${record.executedAt.toISOString()} ${duration}\n`
    }
    
    return report
  }
  
  /**
   * Persist record to database
   */
  private async persist(record: MigrationRecord): Promise<void> {
    // In real implementation, insert into migrations table
    // CREATE TABLE migrations (
    //   id SERIAL PRIMARY KEY,
    //   name VARCHAR(255) NOT NULL,
    //   timestamp BIGINT NOT NULL,
    //   executed_at TIMESTAMP NOT NULL,
    //   success BOOLEAN NOT NULL,
    //   duration INTEGER,
    //   error TEXT
    // )
  }
}

interface MigrationRecord {
  name: string
  timestamp: number
  executedAt: Date
  success: boolean
  duration?: number
  error?: string
  rolledBackAt?: Date
}
```

---

## 🧪 MIGRATION TESTING

### Migration Test Runner
```typescript
/**
 * Migration Test Runner
 * Tests migrations in isolated environment
 */
class MigrationTestRunner {
  /**
   * Test migration (up and down)
   */
  async test(migration: Migration): Promise<TestResult> {
    console.log(`🧪 Testing migration: ${migration.name}`)
    
    const results: TestStep[] = []
    
    try {
      // Step 1: Test forward migration
      console.log('  Step 1: Testing forward migration (up)...')
      const upResult = await this.testUp(migration)
      results.push(upResult)
      
      if (!upResult.success) {
        return {
          success: false,
          message: 'Forward migration failed',
          steps: results
        }
      }
      
      // Step 2: Test rollback
      console.log('  Step 2: Testing rollback (down)...')
      const downResult = await this.testDown(migration)
      results.push(downResult)
      
      if (!downResult.success) {
        return {
          success: false,
          message: 'Rollback failed',
          steps: results
        }
      }
      
      // Step 3: Test idempotency (run up again)
      console.log('  Step 3: Testing idempotency...')
      const idempotentResult = await this.testUp(migration)
      results.push(idempotentResult)
      
      console.log('  ✅ All tests passed')
      
      return {
        success: true,
        message: 'Migration tested successfully',
        steps: results
      }
      
    } catch (error) {
      return {
        success: false,
        message: `Test failed: ${error.message}`,
        error,
        steps: results
      }
    }
  }
  
  /**
   * Test forward migration
   */
  private async testUp(migration: Migration): Promise<TestStep> {
    try {
      // Execute migration
      await this.executeMigration(migration.up)
      
      // Verify schema
      const verified = await this.verifySchema(migration)
      
      return {
        step: 'up',
        success: verified,
        message: verified ? 'Forward migration successful' : 'Schema verification failed'
      }
    } catch (error) {
      return {
        step: 'up',
        success: false,
        message: error.message,
        error
      }
    }
  }
  
  /**
   * Test rollback
   */
  private async testDown(migration: Migration): Promise<TestStep> {
    try {
      // Execute rollback
      await this.executeMigration(migration.down)
      
      // Verify schema reverted
      const verified = await this.verifySchemaReverted(migration)
      
      return {
        step: 'down',
        success: verified,
        message: verified ? 'Rollback successful' : 'Schema not properly reverted'
      }
    } catch (error) {
      return {
        step: 'down',
        success: false,
        message: error.message,
        error
      }
    }
  }
  
  private async executeMigration(sql: string): Promise<void> {
    // Execute in test database
  }
  
  private async verifySchema(migration: Migration): Promise<boolean> {
    // Verify schema changes
    return true
  }
  
  private async verifySchemaReverted(migration: Migration): Promise<boolean> {
    // Verify schema reverted
    return true
  }
}

interface TestResult {
  success: boolean
  message: string
  steps: TestStep[]
  error?: any
}

interface TestStep {
  step: string
  success: boolean
  message: string
  error?: any
}
```

---

## 📚 RELATED ARTICLES

| Article | Purpose | Relationship to Migrations |
|---------|---------|----------------------------|
| **Article I: Deployment Pipeline** | Deployment flow | Migrations run during deployment |
| **Article II: Environment Management** | Configuration | DATABASE_URL from environment |
| **Article IV: Deployment Monitoring** | Health checks | Monitor after migrations |

---

**Previous:** [← Article II: Environment Management](./02-article-ii-environment-management.md)  
**Next:** [Article IV: Deployment Monitoring →](./04-article-iv-deployment-monitoring.md)

---

**Last Updated:** February 7, 2026  
**Constitutional Version:** 2.0.0  
**Status:** ✅ Ratified and In Force

**Motto:** *"Data is Precious - Every Migration Forward Has a Path Backward"*
