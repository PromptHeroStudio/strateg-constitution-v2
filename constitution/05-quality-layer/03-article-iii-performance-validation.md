# ⚡ ARTICLE III: PERFORMANCE VALIDATION

**Ensuring Optimal Speed, Efficiency, and Scalability**

---

## 📜 CONSTITUTIONAL AUTHORITY

This article defines **performance validation rules** that ensure all MVCA-generated code meets performance benchmarks for speed, efficiency, and scalability.

**Legal Force:**
- ✅ Code **SHOULD** meet performance targets (not blocking, but warnings)
- ✅ Critical performance issues **SHALL** be flagged
- ✅ Performance metrics **SHALL** be measured and reported
- ✅ Optimization suggestions **SHALL** be provided
- ✅ Performance degradation **SHALL** trigger alerts

**Constitutional Principle:**
> Performance matters - slow code is bad code.  
> Users deserve fast, responsive applications.

---

## 🎯 PERFORMANCE METRICS

### Key Performance Indicators (KPIs)
```
PERFORMANCE METRICS HIERARCHY
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

TIER 1: RESPONSE TIME (Critical)
├─ API Response Time: <1 second (target), <3 seconds (acceptable)
├─ Page Load Time: <2 seconds (target), <5 seconds (acceptable)
├─ Time to Interactive (TTI): <3 seconds (target)
└─ First Contentful Paint (FCP): <1.8 seconds (target)

TIER 2: THROUGHPUT (Important)
├─ Requests per Second: >100 (target)
├─ Concurrent Users: >1000 (target)
└─ Database Queries per Second: >500 (target)

TIER 3: RESOURCE USAGE (Important)
├─ Memory Usage: <100MB per request (target)
├─ CPU Usage: <50% average (target)
├─ Bundle Size: <250KB (target), <500KB (acceptable)
└─ Database Connections: <20 active (target)

TIER 4: DATABASE PERFORMANCE (Critical)
├─ Query Execution Time: <100ms (target), <500ms (acceptable)
├─ N+1 Queries: 0 (zero tolerance)
├─ Missing Indexes: 0 (zero tolerance)
└─ Full Table Scans: 0 on large tables (zero tolerance)

TIER 5: ALGORITHMIC EFFICIENCY (Important)
├─ Time Complexity: O(n log n) or better for large datasets
├─ Space Complexity: O(n) or better
└─ Loop Efficiency: No nested loops on unbounded data

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

## 🗄️ DATABASE PERFORMANCE

### Rule PERF-001: Detect N+1 Query Problems
```typescript
const detectN1QueriesRule: ValidationRule = {
  id: 'PERF-001',
  name: 'Detect N+1 Query Problems',
  category: RuleCategory.Performance,
  severity: RuleSeverity.High,
  
  description: 'Detect N+1 query patterns that cause excessive database queries',
  rationale: 'N+1 queries can cause severe performance degradation, turning 1 query into hundreds',
  
  examples: {
    bad: [
      `const users = await prisma.user.findMany()
       for (const user of users) {
         user.posts = await prisma.post.findMany({ where: { userId: user.id } })
       }`,
      `users.map(async user => {
         return { ...user, posts: await Post.find({ userId: user.id }) }
       })`
    ],
    good: [
      `const users = await prisma.user.findMany({
         include: { posts: true }
       })`,
      `const users = await User.find().populate('posts')`
    ]
  },
  
  check: (context: CodeContext): ValidationResult => {
    const violations: Violation[] = []
    
    for (const file of context.files) {
      // Pattern 1: Query inside for loop
      const forLoopQueryPattern = /for\s*\([^)]+\)\s*\{[^}]*(?:await\s+)?(?:prisma\.|mongoose\.|sequelize\.|db\.)/gs
      let match
      
      while ((match = forLoopQueryPattern.exec(file.content)) !== null) {
        const lineNumber = file.content.substring(0, match.index).split('\n').length
        
        violations.push({
          ruleId: 'PERF-001',
          severity: RuleSeverity.High,
          message: 'Potential N+1 query: Database query inside for loop',
          location: {
            file: file.path,
            line: lineNumber,
            snippet: this.getCodeSnippet(file.content, match.index, 3)
          },
          fix: {
            description: 'Use eager loading (include/populate) or batch queries instead of querying in loops',
            autoFixable: false
          }
        })
      }
      
      // Pattern 2: Query inside map/forEach
      const mapQueryPattern = /\.(?:map|forEach)\s*\([^)]*(?:await\s+)?(?:prisma\.|mongoose\.|sequelize\.|db\.)/gs
      
      while ((match = mapQueryPattern.exec(file.content)) !== null) {
        const lineNumber = file.content.substring(0, match.index).split('\n').length
        
        violations.push({
          ruleId: 'PERF-001',
          severity: RuleSeverity.High,
          message: 'Potential N+1 query: Database query inside map/forEach',
          location: {
            file: file.path,
            line: lineNumber,
            snippet: this.getCodeSnippet(file.content, match.index, 3)
          },
          fix: {
            description: 'Use eager loading or Promise.all with batched queries',
            autoFixable: false
          }
        })
      }
    }
    
    return {
      passed: violations.length === 0,
      violations,
      score: violations.length === 0 ? 100 : Math.max(0, 100 - violations.length * 30)
    }
  },
  
  autoFixable: false,
  enabled: true,
  tags: ['performance', 'database', 'n+1', 'queries', 'high'],
  links: [
    'https://stackoverflow.com/questions/97197/what-is-the-n1-selects-problem'
  ]
}
```

---

### Rule PERF-002: Missing Database Indexes
```typescript
const missingIndexesRule: ValidationRule = {
  id: 'PERF-002',
  name: 'Detect Missing Database Indexes',
  category: RuleCategory.Performance,
  severity: RuleSeverity.High,
  
  description: 'Detect queries on fields that should have database indexes',
  rationale: 'Missing indexes cause slow queries and full table scans on large datasets',
  
  examples: {
    bad: [
      `// Prisma schema
       model User {
         id    Int    @id @default(autoincrement())
         email String // Missing @unique - queries will be slow
       }`,
      `await prisma.user.findMany({ where: { status: 'active' } }) // No index on status`
    ],
    good: [
      `model User {
         id     Int    @id @default(autoincrement())
         email  String @unique
         status String
         @@index([status])
       }`
    ]
  },
  
  check: (context: CodeContext): ValidationResult => {
    const violations: Violation[] = []
    
    // Find Prisma schema
    const schemaFile = context.files.find(f => f.path.includes('schema.prisma'))
    if (!schemaFile) return { passed: true, score: 100 }
    
    // Parse models
    const models = this.parseModels(schemaFile.content)
    
    // Find queries in code
    for (const file of context.files) {
      const queries = this.extractQueries(file.content)
      
      for (const query of queries) {
        const model = models.find(m => m.name.toLowerCase() === query.model.toLowerCase())
        if (!model) continue
        
        // Check if queried fields have indexes
        for (const field of query.whereFields) {
          const hasIndex = model.indexes.some(idx => idx.includes(field)) ||
                          model.uniqueFields.includes(field) ||
                          field === 'id'
          
          if (!hasIndex) {
            violations.push({
              ruleId: 'PERF-002',
              severity: RuleSeverity.High,
              message: `Missing index on ${query.model}.${field} (frequently queried)`,
              location: {
                file: schemaFile.path,
                snippet: `model ${query.model}`
              },
              fix: {
                description: `Add index: @@index([${field}])`,
                autoFixable: true,
                fixCode: `@@index([${field}])`
              }
            })
          }
        }
      }
    }
    
    return {
      passed: violations.length === 0,
      violations,
      score: violations.length === 0 ? 100 : Math.max(0, 100 - violations.length * 20)
    }
  },
  
  autoFixable: true,
  enabled: true,
  tags: ['performance', 'database', 'indexes', 'optimization', 'high'],
  links: [
    'https://www.prisma.io/docs/concepts/components/prisma-schema/indexes'
  ],
  
  parseModels(schema: string): Array<{
    name: string,
    fields: string[],
    indexes: string[][],
    uniqueFields: string[]
  }> {
    const models: any[] = []
    const modelRegex = /model\s+(\w+)\s*\{([^}]+)\}/gs
    let match
    
    while ((match = modelRegex.exec(schema)) !== null) {
      const modelName = match[1]
      const modelBody = match[2]
      
      // Extract fields
      const fieldRegex = /(\w+)\s+\w+/g
      const fields: string[] = []
      let fieldMatch
      
      while ((fieldMatch = fieldRegex.exec(modelBody)) !== null) {
        fields.push(fieldMatch[1])
      }
      
      // Extract indexes
      const indexRegex = /@@index\(\[([^\]]+)\]\)/g
      const indexes: string[][] = []
      let indexMatch
      
      while ((indexMatch = indexRegex.exec(modelBody)) !== null) {
        const indexFields = indexMatch[1].split(',').map(f => f.trim())
        indexes.push(indexFields)
      }
      
      // Extract unique fields
      const uniqueRegex = /(\w+)\s+\w+\s+@unique/g
      const uniqueFields: string[] = []
      let uniqueMatch
      
      while ((uniqueMatch = uniqueRegex.exec(modelBody)) !== null) {
        uniqueFields.push(uniqueMatch[1])
      }
      
      models.push({ name: modelName, fields, indexes, uniqueFields })
    }
    
    return models
  },
  
  extractQueries(code: string): Array<{ model: string, whereFields: string[] }> {
    const queries: any[] = []
    
    // Pattern: prisma.model.findMany({ where: { field: value } })
    const queryRegex = /prisma\.(\w+)\.find\w+\(\{[^}]*where:\s*\{([^}]+)\}/gs
    let match
    
    while ((match = queryRegex.exec(code)) !== null) {
      const model = match[1]
      const whereClause = match[2]
      
      // Extract fields from where clause
      const fieldRegex = /(\w+):/g
      const fields: string[] = []
      let fieldMatch
      
      while ((fieldMatch = fieldRegex.exec(whereClause)) !== null) {
        fields.push(fieldMatch[1])
      }
      
      queries.push({ model, whereFields: fields })
    }
    
    return queries
  }
}
```

---

### Rule PERF-003: Slow Query Detection
```typescript
const slowQueryRule: ValidationRule = {
  id: 'PERF-003',
  name: 'Detect Potentially Slow Queries',
  category: RuleCategory.Performance,
  severity: RuleSeverity.Medium,
  
  description: 'Detect queries that may be slow (SELECT *, no LIMIT, etc.)',
  rationale: 'Slow queries impact response time and database performance',
  
  examples: {
    bad: [
      `await prisma.user.findMany()  // No limit - could return millions`,
      `SELECT * FROM posts  // Selecting all columns`,
      `await Post.find({ status: 'active' }).sort({ createdAt: -1 })  // No limit`
    ],
    good: [
      `await prisma.user.findMany({ take: 100 })`,
      `SELECT id, title, excerpt FROM posts LIMIT 50`,
      `await Post.find({ status: 'active' }).sort({ createdAt: -1 }).limit(20)`
    ]
  },
  
  check: (context: CodeContext): ValidationResult => {
    const violations: Violation[] = []
    
    for (const file of context.files) {
      // Pattern 1: findMany without take/limit
      const findManyPattern = /(?:prisma\.\w+\.findMany|find)\s*\(\s*\{(?![^}]*(?:take|limit))[^}]*\}/gi
      let match
      
      while ((match = findManyPattern.exec(file.content)) !== null) {
        const lineNumber = file.content.substring(0, match.index).split('\n').length
        
        violations.push({
          ruleId: 'PERF-003',
          severity: RuleSeverity.Medium,
          message: 'Query without LIMIT/take - may return excessive data',
          location: {
            file: file.path,
            line: lineNumber,
            snippet: match[0]
          },
          fix: {
            description: 'Add take/limit clause (e.g., take: 100)',
            autoFixable: true
          }
        })
      }
      
      // Pattern 2: SELECT *
      const selectStarPattern = /SELECT\s+\*\s+FROM/gi
      
      while ((match = selectStarPattern.exec(file.content)) !== null) {
        const lineNumber = file.content.substring(0, match.index).split('\n').length
        
        violations.push({
          ruleId: 'PERF-003',
          severity: RuleSeverity.Medium,
          message: 'SELECT * query - select only needed columns',
          location: {
            file: file.path,
            line: lineNumber,
            snippet: match[0]
          },
          fix: {
            description: 'Specify columns explicitly instead of SELECT *',
            autoFixable: false
          }
        })
      }
    }
    
    return {
      passed: violations.length === 0,
      violations,
      score: violations.length === 0 ? 100 : Math.max(0, 100 - violations.length * 10)
    }
  },
  
  autoFixable: true,
  enabled: true,
  tags: ['performance', 'database', 'queries', 'optimization', 'medium'],
  links: [
    'https://www.prisma.io/docs/concepts/components/prisma-client/pagination'
  ]
}
```

---

## 💾 MEMORY PERFORMANCE

### Rule PERF-010: Memory Leak Detection
```typescript
const memoryLeakRule: ValidationRule = {
  id: 'PERF-010',
  name: 'Detect Potential Memory Leaks',
  category: RuleCategory.Performance,
  severity: RuleSeverity.High,
  
  description: 'Detect patterns that can cause memory leaks',
  rationale: 'Memory leaks cause applications to consume increasing memory until crash',
  
  examples: {
    bad: [
      `const cache = {}
       app.get('/api/data', (req, res) => {
         cache[req.query.id] = fetchData(req.query.id)  // Cache grows unbounded
       })`,
      `setInterval(() => {
         const data = largeDataFetch()
         // data never cleaned up
       }, 1000)`,
      `global.users = []
       function addUser(user) {
         global.users.push(user)  // Array grows forever
       }`
    ],
    good: [
      `const cache = new Map()
       const MAX_CACHE_SIZE = 1000
       app.get('/api/data', (req, res) => {
         if (cache.size >= MAX_CACHE_SIZE) {
           const firstKey = cache.keys().next().value
           cache.delete(firstKey)
         }
         cache.set(req.query.id, fetchData(req.query.id))
       })`,
      `const interval = setInterval(() => {
         const data = largeDataFetch()
         processData(data)
       }, 1000)
       
       // Cleanup
       process.on('SIGTERM', () => clearInterval(interval))`
    ]
  },
  
  check: (context: CodeContext): ValidationResult => {
    const violations: Violation[] = []
    
    for (const file of context.files) {
      // Pattern 1: Unbounded cache
      const unboundedCachePattern = /(?:const|let|var)\s+(\w+)\s*=\s*(?:\{\}|\[\]|new\s+(?:Map|Set)\(\))[^;]*;[\s\S]{0,500}?\1\[.*?\]\s*=/g
      let match
      
      while ((match = unboundedCachePattern.exec(file.content)) !== null) {
        const cacheName = match[1]
        const snippet = match[0]
        
        // Check if there's a size limit
        const hasSizeLimit = snippet.match(/(?:MAX_SIZE|limit|size.*>=|length.*>=)/i)
        
        if (!hasSizeLimit) {
          const lineNumber = file.content.substring(0, match.index).split('\n').length
          
          violations.push({
            ruleId: 'PERF-010',
            severity: RuleSeverity.High,
            message: `Potential memory leak: Unbounded cache/collection "${cacheName}"`,
            location: {
              file: file.path,
              line: lineNumber,
              snippet: match[0]
            },
            fix: {
              description: 'Implement cache size limit with LRU eviction',
              autoFixable: false
            }
          })
        }
      }
      
      // Pattern 2: setInterval without cleanup
      const intervalPattern = /(?:setInterval|setImmediate)\s*\(/g
      
      while ((match = intervalPattern.exec(file.content)) !== null) {
        const lineNumber = file.content.substring(0, match.index).split('\n').length
        const context = file.content.substring(match.index, match.index + 500)
        
        // Check if there's a clearInterval
        const hasClearInterval = file.content.includes('clearInterval') || 
                                 file.content.includes('clearImmediate')
        
        if (!hasClearInterval) {
          violations.push({
            ruleId: 'PERF-010',
            severity: RuleSeverity.Medium,
            message: 'setInterval without cleanup - potential memory leak',
            location: {
              file: file.path,
              line: lineNumber,
              snippet: match[0]
            },
            fix: {
              description: 'Store interval reference and clear it on cleanup',
              autoFixable: false
            }
          })
        }
      }
      
      // Pattern 3: Global array accumulation
      const globalArrayPattern = /global\.\w+\.push\(/g
      
      while ((match = globalArrayPattern.exec(file.content)) !== null) {
        const lineNumber = file.content.substring(0, match.index).split('\n').length
        
        violations.push({
          ruleId: 'PERF-010',
          severity: RuleSeverity.High,
          message: 'Pushing to global array - potential memory leak',
          location: {
            file: file.path,
            line: lineNumber,
            snippet: match[0]
          },
          fix: {
            description: 'Use bounded collection with cleanup strategy',
            autoFixable: false
          }
        })
      }
    }
    
    return {
      passed: violations.length === 0,
      violations,
      score: violations.length === 0 ? 100 : Math.max(0, 100 - violations.length * 25)
    }
  },
  
  autoFixable: false,
  enabled: true,
  tags: ['performance', 'memory', 'leaks', 'resources', 'high'],
  links: [
    'https://developer.mozilla.org/en-US/docs/Web/JavaScript/Memory_Management'
  ]
}
```

---

## 📦 FRONTEND PERFORMANCE

### Rule PERF-020: Bundle Size Analysis
```typescript
const bundleSizeRule: ValidationRule = {
  id: 'PERF-020',
  name: 'Analyze Frontend Bundle Size',
  category: RuleCategory.Performance,
  severity: RuleSeverity.Medium,
  
  description: 'Detect large bundle sizes that impact page load time',
  rationale: 'Large bundles increase initial page load time, especially on slow connections',
  
  examples: {
    bad: [
      `import _ from 'lodash'  // Imports entire library (71KB)`,
      `import moment from 'moment'  // Large library (67KB)`,
      `import * as allIcons from '@heroicons/react'  // All icons (~500KB)`
    ],
    good: [
      `import { debounce } from 'lodash'  // Tree-shakeable`,
      `import { format } from 'date-fns'  // Smaller alternative (12KB)`,
      `import { CheckIcon } from '@heroicons/react/24/outline'  // Single icon`
    ]
  },
  
  check: (context: CodeContext): ValidationResult => {
    const violations: Violation[] = []
    
    // Only check frontend projects
    if (context.projectType !== 'web') {
      return { passed: true, score: 100 }
    }
    
    for (const file of context.files) {
      // Pattern 1: Full library imports
      const heavyLibraries = [
        { name: 'lodash', size: 71, alternative: 'lodash-es (tree-shakeable)' },
        { name: 'moment', size: 67, alternative: 'date-fns (smaller)' },
        { name: 'axios', size: 13, alternative: 'native fetch API' },
        { name: 'jquery', size: 87, alternative: 'native DOM APIs' }
      ]
      
      for (const lib of heavyLibraries) {
        const fullImportPattern = new RegExp(`import\\s+(?:\\w+|\\*)\\s+from\\s+['"]${lib.name}['"]`, 'g')
        let match
        
        while ((match = fullImportPattern.exec(file.content)) !== null) {
          const lineNumber = file.content.substring(0, match.index).split('\n').length
          
          violations.push({
            ruleId: 'PERF-020',
            severity: RuleSeverity.Medium,
            message: `Large dependency: ${lib.name} (~${lib.size}KB) - consider ${lib.alternative}`,
            location: {
              file: file.path,
              line: lineNumber,
              snippet: match[0]
            },
            fix: {
              description: `Use ${lib.alternative} or import specific functions`,
              autoFixable: false
            }
          })
        }
      }
      
      // Pattern 2: Import all pattern (import * as)
      const importAllPattern = /import\s+\*\s+as\s+\w+\s+from\s+['"]([^'"]+)['"]/g
      let match
      
      while ((match = importAllPattern.exec(file.content)) !== null) {
        const packageName = match[1]
        
        // Skip if it's a local import
        if (packageName.startsWith('.') || packageName.startsWith('@/')) continue
        
        const lineNumber = file.content.substring(0, match.index).split('\n').length
        
        violations.push({
          ruleId: 'PERF-020',
          severity: RuleSeverity.Low,
          message: `Import all (*) from "${packageName}" - may increase bundle size`,
          location: {
            file: file.path,
            line: lineNumber,
            snippet: match[0]
          },
          fix: {
            description: 'Import only needed exports for better tree-shaking',
            autoFixable: false
          }
        })
      }
    }
    
    return {
      passed: violations.length === 0,
      violations,
      score: violations.length === 0 ? 100 : Math.max(0, 100 - violations.length * 5)
    }
  },
  
  autoFixable: false,
  enabled: true,
  tags: ['performance', 'frontend', 'bundle-size', 'optimization', 'medium'],
  links: [
    'https://web.dev/reduce-javascript-payloads-with-tree-shaking/'
  ]
}
```

---

### Rule PERF-021: Image Optimization
```typescript
const imageOptimizationRule: ValidationRule = {
  id: 'PERF-021',
  name: 'Ensure Image Optimization',
  category: RuleCategory.Performance,
  severity: RuleSeverity.Medium,
  
  description: 'Detect unoptimized images that should use Next.js Image component',
  rationale: 'Unoptimized images waste bandwidth and slow page loads',
  
  examples: {
    bad: [
      `<img src="/hero.jpg" alt="Hero" />  // No optimization`,
      `<img src={userAvatar} />  // No lazy loading, no size optimization`
    ],
    good: [
      `<Image src="/hero.jpg" alt="Hero" width={1200} height={600} />`,
      `<Image src={userAvatar} alt="Avatar" width={48} height={48} loading="lazy" />`
    ]
  },
  
  check: (context: CodeContext): ValidationResult => {
    const violations: Violation[] = []
    
    // Only check Next.js projects
    const isNextJs = context.framework === 'next' || 
                     context.dependencies['next'] !== undefined
    
    if (!isNextJs) return { passed: true, score: 100 }
    
    for (const file of context.files) {
      // Skip if not React/JSX file
      if (!file.path.match(/\.(tsx|jsx)$/)) continue
      
      // Pattern: <img> tags instead of Next.js Image
      const imgTagPattern = /<img\s+[^>]*src=/gi
      let match
      
      while ((match = imgTagPattern.exec(file.content)) !== null) {
        const lineNumber = file.content.substring(0, match.index).split('\n').length
        
        violations.push({
          ruleId: 'PERF-021',
          severity: RuleSeverity.Medium,
          message: 'Use Next.js Image component instead of <img> for automatic optimization',
          location: {
            file: file.path,
            line: lineNumber,
            snippet: match[0]
          },
          fix: {
            description: 'Replace <img> with <Image> from next/image',
            autoFixable: true
          }
        })
      }
    }
    
    return {
      passed: violations.length === 0,
      violations,
      score: violations.length === 0 ? 100 : Math.max(0, 100 - violations.length * 10)
    }
  },
  
  autoFixable: true,
  enabled: true,
  tags: ['performance', 'frontend', 'images', 'nextjs', 'medium'],
  links: [
    'https://nextjs.org/docs/basic-features/image-optimization'
  ]
}
```

---

## 🔄 ALGORITHMIC EFFICIENCY

### Rule PERF-030: Inefficient Algorithms
```typescript
const inefficientAlgorithmsRule: ValidationRule = {
  id: 'PERF-030',
  name: 'Detect Inefficient Algorithms',
  category: RuleCategory.Performance,
  severity: RuleSeverity.Medium,
  
  description: 'Detect algorithms with poor time complexity',
  rationale: 'Inefficient algorithms cause performance problems as data grows',
  
  examples: {
    bad: [
      `// O(n²) - Nested loops on same array
       for (const item of items) {
         for (const other of items) {
           if (item.id === other.relatedId) { /* ... */ }
         }
       }`,
      `// O(n²) - Array.includes in loop
       const result = []
       for (const item of items) {
         if (!result.includes(item)) result.push(item)
       }`
    ],
    good: [
      `// O(n) - Use Map for lookups
       const itemMap = new Map(items.map(item => [item.id, item]))
       for (const item of items) {
         const related = itemMap.get(item.relatedId)
       }`,
      `// O(n) - Use Set for uniqueness
       const result = [...new Set(items)]`
    ]
  },
  
  check: (context: CodeContext): ValidationResult => {
    const violations: Violation[] = []
    
    for (const file of context.files) {
      // Pattern 1: Nested loops on arrays
      const nestedLoopPattern = /for\s*\([^)]+of\s+(\w+)[^)]*\)[^{]*\{[^}]*for\s*\([^)]+of\s+\1/gs
      let match
      
      while ((match = nestedLoopPattern.exec(file.content)) !== null) {
        const lineNumber = file.content.substring(0, match.index).split('\n').length
        
        violations.push({
          ruleId: 'PERF-030',
          severity: RuleSeverity.Medium,
          message: 'Nested loops on same array - O(n²) complexity',
          location: {
            file: file.path,
            line: lineNumber,
            snippet: this.getCodeSnippet(file.content, match.index, 5)
          },
          fix: {
            description: 'Use Map/Set for O(n) lookups instead of nested loops',
            autoFixable: false
          }
        })
      }
      
      // Pattern 2: Array.includes/indexOf in loop
      const includesInLoopPattern = /for\s*\([^)]+\)[^{]*\{[^}]*\.(?:includes|indexOf)\(/gs
      
      while ((match = includesInLoopPattern.exec(file.content)) !== null) {
        const lineNumber = file.content.substring(0, match.index).split('\n').length
        
        violations.push({
          ruleId: 'PERF-030',
          severity: RuleSeverity.Low,
          message: 'Array.includes() in loop - consider using Set for O(1) lookups',
          location: {
            file: file.path,
            line: lineNumber,
            snippet: this.getCodeSnippet(file.content, match.index, 3)
          },
          fix: {
            description: 'Convert array to Set before loop for faster lookups',
            autoFixable: true
          }
        })
      }
      
      // Pattern 3: Array.find in loop
      const findInLoopPattern = /for\s*\([^)]+\)[^{]*\{[^}]*\.find\(/gs
      
      while ((match = findInLoopPattern.exec(file.content)) !== null) {
        const lineNumber = file.content.substring(0, match.index).split('\n').length
        
        violations.push({
          ruleId: 'PERF-030',
          severity: RuleSeverity.Medium,
          message: 'Array.find() in loop - consider using Map for O(1) lookups',
          location: {
            file: file.path,
            line: lineNumber,
            snippet: this.getCodeSnippet(file.content, match.index, 3)
          },
          fix: {
            description: 'Convert array to Map before loop',
            autoFixable: true
          }
        })
      }
    }
    
    return {
      passed: violations.length === 0,
      violations,
      score: violations.length === 0 ? 100 : Math.max(0, 100 - violations.length * 15)
    }
  },
  
  autoFixable: true,
  enabled: true,
  tags: ['performance', 'algorithms', 'complexity', 'optimization', 'medium'],
  links: [
    'https://en.wikipedia.org/wiki/Time_complexity'
  ]
}
```

---

## 📊 PERFORMANCE SCORING

### Performance Score Calculator
```typescript
/**
 * Performance Score Calculator
 * Calculates overall performance score based on metrics
 */
class PerformanceScorer {
  /**
   * Calculate performance score
   */
  calculateScore(metrics: PerformanceMetrics): PerformanceScore {
    const scores = {
      database: this.scoreDatabasePerformance(metrics.database),
      memory: this.scoreMemoryPerformance(metrics.memory),
      frontend: this.scoreFrontendPerformance(metrics.frontend),
      algorithms: this.scoreAlgorithmicEfficiency(metrics.algorithms)
    }
    
    // Weighted average
    const weights = {
      database: 0.35,    // 35% - Most critical for backend
      memory: 0.25,      // 25% - Important for stability
      frontend: 0.25,    // 25% - Important for UX
      algorithms: 0.15   // 15% - Important for scalability
    }
    
    const overallScore = 
      scores.database * weights.database +
      scores.memory * weights.memory +
      scores.frontend * weights.frontend +
      scores.algorithms * weights.algorithms
    
    return {
      overall: Math.round(overallScore),
      breakdown: scores,
      grade: this.getGrade(overallScore),
      recommendations: this.generateRecommendations(scores, metrics)
    }
  }
  
  /**
   * Score database performance
   */
  private scoreDatabasePerformance(db: DatabaseMetrics): number {
    let score = 100
    
    // N+1 queries (critical)
    score -= db.n1QueryCount * 30
    
    // Missing indexes (high impact)
    score -= db.missingIndexes * 20
    
    // Slow queries (medium impact)
    score -= db.slowQueries * 10
    
    // Unbounded queries (low impact)
    score -= db.unboundedQueries * 5
    
    return Math.max(0, score)
  }
  
  /**
   * Score memory performance
   */
  private scoreMemoryPerformance(mem: MemoryMetrics): number {
    let score = 100
    
    // Memory leaks (critical)
    score -= mem.potentialLeaks * 25
    
    // Unbounded collections (high impact)
    score -= mem.unboundedCollections * 15
    
    return Math.max(0, score)
  }
  
  /**
   * Score frontend performance
   */
  private scoreFrontendPerformance(fe: FrontendMetrics): number {
    let score = 100
    
    // Large bundle size
    if (fe.bundleSize > 500) score -= 30
    else if (fe.bundleSize > 250) score -= 15
    
    // Unoptimized images
    score -= fe.unoptimizedImages * 10
    
    // Heavy dependencies
    score -= fe.heavyDependencies * 5
    
    return Math.max(0, score)
  }
  
  /**
   * Score algorithmic efficiency
   */
  private scoreAlgorithmicEfficiency(algo: AlgorithmMetrics): number {
    let score = 100
    
    // O(n²) algorithms
    score -= algo.quadraticComplexity * 20
    
    // Inefficient lookups
    score -= algo.inefficientLookups * 10
    
    return Math.max(0, score)
  }
  
  /**
   * Get performance grade
   */
  private getGrade(score: number): string {
    if (score >= 90) return 'A'
    if (score >= 80) return 'B'
    if (score >= 70) return 'C'
    if (score >= 60) return 'D'
    return 'F'
  }
  
  /**
   * Generate optimization recommendations
   */
  private generateRecommendations(
    scores: Record<string, number>,
    metrics: PerformanceMetrics
  ): string[] {
    const recommendations: string[] = []
    
    if (scores.database < 80) {
      recommendations.push('🗄️ Database: Add missing indexes and fix N+1 queries')
    }
    
    if (scores.memory < 80) {
      recommendations.push('💾 Memory: Implement cache size limits and cleanup intervals')
    }
    
    if (scores.frontend < 80) {
      recommendations.push('📦 Frontend: Reduce bundle size and optimize images')
    }
    
    if (scores.algorithms < 80) {
      recommendations.push('🔄 Algorithms: Replace nested loops with Map/Set lookups')
    }
    
    return recommendations
  }
}

/**
 * Supporting Types
 */
interface PerformanceMetrics {
  database: DatabaseMetrics
  memory: MemoryMetrics
  frontend: FrontendMetrics
  algorithms: AlgorithmMetrics
}

interface DatabaseMetrics {
  n1QueryCount: number
  missingIndexes: number
  slowQueries: number
  unboundedQueries: number
}

interface MemoryMetrics {
  potentialLeaks: number
  unboundedCollections: number
}

interface FrontendMetrics {
  bundleSize: number  // KB
  unoptimizedImages: number
  heavyDependencies: number
}

interface AlgorithmMetrics {
  quadraticComplexity: number
  inefficientLookups: number
}

interface PerformanceScore {
  overall: number
  breakdown: Record<string, number>
  grade: string
  recommendations: string[]
}
```

---

## 📚 RELATED ARTICLES

| Article | Purpose | Relationship to Performance Validation |
|---------|---------|---------------------------------------|
| **Article I: Validation Framework** | Framework foundation | Performance rules implement framework |
| **Article II: Security Validation** | Security rules | Complements performance checks |
| **Article IV: Code Quality Validation** | Quality rules | Works alongside performance |

---

**Previous:** [← Article II: Security Validation](./02-article-ii-security-validation.md)  
**Next:** [Article IV: Code Quality Validation →](./04-article-iv-code-quality-validation.md)

---

**Last Updated:** February 7, 2026  
**Constitutional Version:** 2.0.0  
**Status:** ✅ Ratified and In Force

**Motto:** *"Performance Matters - Fast Code is Good Code"*
