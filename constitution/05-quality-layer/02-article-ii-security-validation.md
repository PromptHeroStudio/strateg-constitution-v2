# 🔒 ARTICLE II: SECURITY VALIDATION

**Ensuring OWASP Compliance and Security Best Practices**

---

## 📜 CONSTITUTIONAL AUTHORITY

This article defines **security validation rules** that ensure all MVCA-generated code is secure and compliant with OWASP Top 10 standards.

**Legal Force:**
- ✅ All code **MUST** be OWASP Top 10 compliant
- ✅ Critical security vulnerabilities **SHALL** block delivery
- ✅ Security checks **SHALL** be mandatory (cannot be disabled)
- ✅ Secrets **SHALL NOT** be exposed in code or logs
- ✅ Security violations **SHALL** be reported with remediation guidance

**Constitutional Principle:**
> Security is non-negotiable - no exceptions, no shortcuts.  
> Law #3 (Security First) is enforced here.

---

## 🎯 OWASP TOP 10 COMPLIANCE

### The OWASP Top 10 (2021)
```
OWASP TOP 10 WEB APPLICATION SECURITY RISKS
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

A01:2021 – Broken Access Control
├─ Missing authorization checks
├─ Insecure Direct Object References (IDOR)
├─ Privilege escalation
└─ CORS misconfiguration

A02:2021 – Cryptographic Failures
├─ Sensitive data transmitted in plaintext
├─ Weak cryptographic algorithms
├─ Missing encryption
└─ Hardcoded secrets

A03:2021 – Injection
├─ SQL Injection
├─ NoSQL Injection
├─ Command Injection
├─ XSS (Cross-Site Scripting)
└─ Template Injection

A04:2021 – Insecure Design
├─ Missing security controls in design
├─ Insufficient rate limiting
├─ No input validation
└─ Unsafe defaults

A05:2021 – Security Misconfiguration
├─ Unnecessary features enabled
├─ Default credentials
├─ Verbose error messages
└─ Missing security headers

A06:2021 – Vulnerable and Outdated Components
├─ Outdated dependencies
├─ Known vulnerabilities in packages
└─ Unsupported software versions

A07:2021 – Identification and Authentication Failures
├─ Weak passwords allowed
├─ No rate limiting on auth
├─ Session fixation
└─ Missing MFA

A08:2021 – Software and Data Integrity Failures
├─ Unsigned packages/updates
├─ Insecure CI/CD pipeline
└─ Missing integrity checks

A09:2021 – Security Logging and Monitoring Failures
├─ Insufficient logging
├─ Logs contain sensitive data
└─ No alerting on security events

A10:2021 – Server-Side Request Forgery (SSRF)
├─ Unvalidated URLs
├─ No URL whitelisting
└─ Internal network access

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

## 🛡️ A01: BROKEN ACCESS CONTROL

### Rule SEC-010: Authorization Check Required
```typescript
const authorizationCheckRule: ValidationRule = {
  id: 'SEC-010',
  name: 'Authorization Check Required',
  category: RuleCategory.Security,
  severity: RuleSeverity.Critical,
  
  description: 'All routes that modify data must verify user authorization',
  rationale: 'OWASP A01 - Missing authorization allows unauthorized users to access/modify data',
  
  examples: {
    bad: [
      `router.delete('/api/posts/:id', async (req, res) => {
        await prisma.post.delete({ where: { id: req.params.id } })
      })`,
      `router.put('/api/users/:id', async (req, res) => {
        await prisma.user.update({ where: { id: req.params.id }, data: req.body })
      })`
    ],
    good: [
      `router.delete('/api/posts/:id', async (req, res) => {
        const post = await prisma.post.findUnique({ where: { id: req.params.id } })
        if (post.authorId !== req.user.id) throw new ForbiddenError()
        await prisma.post.delete({ where: { id: req.params.id } })
      })`,
      `router.put('/api/users/:id', requireOwnership, async (req, res) => {
        await prisma.user.update({ where: { id: req.params.id }, data: req.body })
      })`
    ]
  },
  
  check: (context: CodeContext): ValidationResult => {
    const violations: Violation[] = []
    
    for (const file of context.files) {
      // Find route handlers for write operations
      const writeRoutes = this.findWriteRoutes(file)
      
      for (const route of writeRoutes) {
        // Check if authorization check exists
        const hasAuthCheck = this.hasAuthorizationCheck(route)
        
        if (!hasAuthCheck) {
          violations.push({
            ruleId: 'SEC-010',
            severity: RuleSeverity.Critical,
            message: `Missing authorization check in ${route.method} route`,
            location: {
              file: file.path,
              line: route.line,
              snippet: route.code
            },
            fix: {
              description: 'Add authorization middleware or check user ownership before modification',
              autoFixable: false
            }
          })
        }
      }
    }
    
    return {
      passed: violations.length === 0,
      violations,
      score: violations.length === 0 ? 100 : 0
    }
  },
  
  autoFixable: false,
  enabled: true,
  tags: ['security', 'owasp-a01', 'authorization', 'access-control', 'critical'],
  links: [
    'https://owasp.org/Top10/A01_2021-Broken_Access_Control/'
  ]
}
```

---

### Rule SEC-011: No Insecure Direct Object References
```typescript
const noIDORRule: ValidationRule = {
  id: 'SEC-011',
  name: 'Prevent Insecure Direct Object References (IDOR)',
  category: RuleCategory.Security,
  severity: RuleSeverity.Critical,
  
  description: 'Database queries using user-supplied IDs must verify ownership',
  rationale: 'OWASP A01 - IDOR allows users to access objects they should not have access to',
  
  examples: {
    bad: [
      `const post = await prisma.post.findUnique({ 
        where: { id: req.params.id } 
      })`,
      `const order = await Order.findById(req.query.orderId)`
    ],
    good: [
      `const post = await prisma.post.findFirst({ 
        where: { id: req.params.id, authorId: req.user.id } 
      })`,
      `const order = await Order.findOne({ 
        _id: req.query.orderId, 
        userId: req.user.id 
      })`
    ]
  },
  
  check: (context: CodeContext): ValidationResult => {
    const violations: Violation[] = []
    
    for (const file of context.files) {
      // Pattern: Database queries with user-supplied IDs
      const queryPatterns = [
        /findUnique\(\s*\{\s*where:\s*\{\s*id:\s*req\.params/g,
        /findFirst\(\s*\{\s*where:\s*\{\s*id:\s*req\.params/g,
        /findById\(\s*req\.(params|query|body)\./g,
        /findOne\(\s*\{\s*_id:\s*req\./g
      ]
      
      for (const pattern of queryPatterns) {
        let match
        while ((match = pattern.exec(file.content)) !== null) {
          const lineNumber = file.content.substring(0, match.index).split('\n').length
          const snippet = this.getCodeSnippet(file.content, match.index, 5)
          
          // Check if ownership verification exists nearby
          const hasOwnershipCheck = snippet.match(/authorId|userId|ownerId.*req\.user/i)
          
          if (!hasOwnershipCheck) {
            violations.push({
              ruleId: 'SEC-011',
              severity: RuleSeverity.Critical,
              message: 'Potential IDOR: Query uses user-supplied ID without ownership check',
              location: {
                file: file.path,
                line: lineNumber,
                snippet: match[0]
              },
              fix: {
                description: 'Add ownership check (e.g., authorId: req.user.id) to where clause',
                autoFixable: false
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
  
  autoFixable: false,
  enabled: true,
  tags: ['security', 'owasp-a01', 'idor', 'access-control', 'critical'],
  links: [
    'https://cheatsheetseries.owasp.org/cheatsheets/Insecure_Direct_Object_Reference_Prevention_Cheat_Sheet.html'
  ]
}
```

---

## 🔐 A02: CRYPTOGRAPHIC FAILURES

### Rule SEC-020: No Hardcoded Secrets
```typescript
const noHardcodedSecretsRule: ValidationRule = {
  id: 'SEC-020',
  name: 'No Hardcoded Secrets',
  category: RuleCategory.Security,
  severity: RuleSeverity.Critical,
  
  description: 'API keys, passwords, and secrets must not be hardcoded',
  rationale: 'OWASP A02 - Hardcoded secrets in code can be discovered and exploited',
  
  examples: {
    bad: [
      `const apiKey = "sk_live_abc123xyz789"`,
      `const password = "MyPassword123"`,
      `const dbUrl = "postgresql://user:pass123@localhost:5432/db"`,
      `const jwt_secret = "my-secret-key"`
    ],
    good: [
      `const apiKey = process.env.STRIPE_SECRET_KEY`,
      `const password = process.env.DB_PASSWORD`,
      `const dbUrl = process.env.DATABASE_URL`,
      `const jwt_secret = process.env.JWT_SECRET`
    ]
  },
  
  check: (context: CodeContext): ValidationResult => {
    const violations: Violation[] = []
    
    for (const file of context.files) {
      // Skip .env files (they're supposed to have secrets)
      if (file.path.includes('.env')) continue
      
      // Patterns for common secrets
      const secretPatterns = [
        { pattern: /(?:api[_-]?key|apikey)\s*[=:]\s*["']([^"']{20,})["']/gi, type: 'API Key' },
        { pattern: /(?:secret|token)\s*[=:]\s*["']([^"']{20,})["']/gi, type: 'Secret/Token' },
        { pattern: /password\s*[=:]\s*["']([^"']{3,})["']/gi, type: 'Password' },
        { pattern: /sk_(?:live|test)_[a-zA-Z0-9]{24,}/g, type: 'Stripe Key' },
        { pattern: /ghp_[a-zA-Z0-9]{36,}/g, type: 'GitHub Token' },
        { pattern: /xox[baprs]-[a-zA-Z0-9-]{10,}/g, type: 'Slack Token' },
        { pattern: /postgresql:\/\/[^:]+:[^@]+@/g, type: 'Database URL with password' }
      ]
      
      for (const { pattern, type } of secretPatterns) {
        let match
        while ((match = pattern.exec(file.content)) !== null) {
          const lineNumber = file.content.substring(0, match.index).split('\n').length
          const line = file.content.split('\n')[lineNumber - 1]
          
          // Skip if it's using environment variable
          if (line.includes('process.env') || line.includes('import.meta.env')) {
            continue
          }
          
          // Skip if it's a comment
          if (line.trim().startsWith('//') || line.trim().startsWith('*')) {
            continue
          }
          
          violations.push({
            ruleId: 'SEC-020',
            severity: RuleSeverity.Critical,
            message: `Hardcoded ${type} detected`,
            location: {
              file: file.path,
              line: lineNumber,
              snippet: '[REDACTED]'  // Don't show the actual secret
            },
            fix: {
              description: `Move ${type} to environment variable`,
              autoFixable: true,
              fixCode: `process.env.${this.suggestEnvVarName(type)}`
            }
          })
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
    // Implementation: Replace hardcoded secrets with env vars
    return {
      success: true,
      filesChanged: [],
      message: 'Replaced hardcoded secrets with environment variables'
    }
  },
  
  enabled: true,
  tags: ['security', 'owasp-a02', 'secrets', 'credentials', 'critical'],
  links: [
    'https://owasp.org/Top10/A02_2021-Cryptographic_Failures/'
  ],
  
  suggestEnvVarName(type: string): string {
    const mapping: Record<string, string> = {
      'API Key': 'API_KEY',
      'Secret/Token': 'SECRET_TOKEN',
      'Password': 'PASSWORD',
      'Stripe Key': 'STRIPE_SECRET_KEY',
      'GitHub Token': 'GITHUB_TOKEN',
      'Slack Token': 'SLACK_TOKEN',
      'Database URL with password': 'DATABASE_URL'
    }
    return mapping[type] || 'SECRET_VALUE'
  }
}
```

---

### Rule SEC-021: Use Strong Encryption
```typescript
const strongEncryptionRule: ValidationRule = {
  id: 'SEC-021',
  name: 'Use Strong Encryption Algorithms',
  category: RuleCategory.Security,
  severity: RuleSeverity.High,
  
  description: 'Weak or deprecated encryption algorithms must not be used',
  rationale: 'OWASP A02 - Weak encryption can be broken, exposing sensitive data',
  
  examples: {
    bad: [
      `crypto.createCipher('des', key)`,      // DES is weak
      `crypto.createHash('md5')`,             // MD5 is broken
      `crypto.createHash('sha1')`,            // SHA1 is weak
      `bcrypt.hash(password, 4)`              // Cost too low
    ],
    good: [
      `crypto.createCipheriv('aes-256-gcm', key, iv)`,
      `crypto.createHash('sha256')`,
      `crypto.createHash('sha512')`,
      `bcrypt.hash(password, 12)`             // Cost 12+ recommended
    ]
  },
  
  check: (context: CodeContext): ValidationResult => {
    const violations: Violation[] = []
    
    for (const file of context.files) {
      // Weak algorithms
      const weakAlgorithms = [
        { pattern: /createCipher\(['"](?:des|rc4|blowfish)['"]/gi, algo: 'DES/RC4/Blowfish', replacement: 'aes-256-gcm' },
        { pattern: /createHash\(['"](?:md5|md4|sha1)['"]/gi, algo: 'MD5/MD4/SHA1', replacement: 'sha256' },
        { pattern: /bcrypt\.hash\([^,]+,\s*([0-9])\)/g, algo: 'bcrypt (low cost)', replacement: '12' }
      ]
      
      for (const { pattern, algo, replacement } of weakAlgorithms) {
        let match
        while ((match = pattern.exec(file.content)) !== null) {
          // Special handling for bcrypt cost
          if (algo.includes('bcrypt')) {
            const cost = parseInt(match[1])
            if (cost >= 12) continue  // Cost is acceptable
          }
          
          const lineNumber = file.content.substring(0, match.index).split('\n').length
          
          violations.push({
            ruleId: 'SEC-021',
            severity: RuleSeverity.High,
            message: `Weak algorithm detected: ${algo}`,
            location: {
              file: file.path,
              line: lineNumber,
              snippet: match[0]
            },
            fix: {
              description: `Replace with ${replacement}`,
              autoFixable: true
            }
          })
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
  tags: ['security', 'owasp-a02', 'encryption', 'cryptography', 'high'],
  links: [
    'https://cheatsheetseries.owasp.org/cheatsheets/Cryptographic_Storage_Cheat_Sheet.html'
  ]
}
```

---

## 💉 A03: INJECTION

### Rule SEC-030: Prevent XSS
```typescript
const preventXSSRule: ValidationRule = {
  id: 'SEC-030',
  name: 'Prevent Cross-Site Scripting (XSS)',
  category: RuleCategory.Security,
  severity: RuleSeverity.Critical,
  
  description: 'User input must be sanitized before rendering in HTML',
  rationale: 'OWASP A03 - XSS allows attackers to inject malicious scripts into web pages',
  
  examples: {
    bad: [
      `<div dangerouslySetInnerHTML={{ __html: userInput }} />`,
      `element.innerHTML = userComment`,
      `document.write(userInput)`,
      `eval(userInput)`
    ],
    good: [
      `<div>{DOMPurify.sanitize(userInput)}</div>`,
      `element.textContent = userComment`,
      `const sanitized = DOMPurify.sanitize(userInput)`
    ]
  },
  
  check: (context: CodeContext): ValidationResult => {
    const violations: Violation[] = []
    
    for (const file of context.files) {
      // Pattern 1: dangerouslySetInnerHTML without sanitization
      const dangerousHTMLPattern = /dangerouslySetInnerHTML\s*=\s*\{\{?\s*__html:\s*([^}]+)\s*\}\}?/gi
      let match
      
      while ((match = dangerousHTMLPattern.exec(file.content)) !== null) {
        const htmlSource = match[1].trim()
        
        // Check if sanitized
        if (!htmlSource.includes('sanitize') && 
            !htmlSource.includes('DOMPurify') &&
            !htmlSource.includes('xss')) {
          
          const lineNumber = file.content.substring(0, match.index).split('\n').length
          
          violations.push({
            ruleId: 'SEC-030',
            severity: RuleSeverity.Critical,
            message: 'Unsanitized HTML being rendered (XSS risk)',
            location: {
              file: file.path,
              line: lineNumber,
              snippet: match[0]
            },
            fix: {
              description: 'Sanitize HTML with DOMPurify before rendering',
              autoFixable: true,
              fixCode: `dangerouslySetInnerHTML={{ __html: DOMPurify.sanitize(${htmlSource}) }}`
            }
          })
        }
      }
      
      // Pattern 2: innerHTML assignment
      const innerHTMLPattern = /\.innerHTML\s*=\s*([^;]+)/gi
      
      while ((match = innerHTMLPattern.exec(file.content)) !== null) {
        const value = match[1].trim()
        
        if (!value.includes('sanitize') && !value.includes('DOMPurify')) {
          const lineNumber = file.content.substring(0, match.index).split('\n').length
          
          violations.push({
            ruleId: 'SEC-030',
            severity: RuleSeverity.Critical,
            message: 'innerHTML assignment without sanitization (XSS risk)',
            location: {
              file: file.path,
              line: lineNumber,
              snippet: match[0]
            },
            fix: {
              description: 'Use textContent or sanitize with DOMPurify',
              autoFixable: false
            }
          })
        }
      }
      
      // Pattern 3: eval() usage
      const evalPattern = /\beval\s*\(/gi
      
      while ((match = evalPattern.exec(file.content)) !== null) {
        const lineNumber = file.content.substring(0, match.index).split('\n').length
        
        violations.push({
          ruleId: 'SEC-030',
          severity: RuleSeverity.Critical,
          message: 'eval() usage detected (XSS and code injection risk)',
          location: {
            file: file.path,
            line: lineNumber,
            snippet: match[0]
          },
          fix: {
            description: 'Avoid eval() - use safer alternatives like JSON.parse() or new Function()',
            autoFixable: false
          }
        })
      }
    }
    
    return {
      passed: violations.length === 0,
      violations,
      score: violations.length === 0 ? 100 : 0
    }
  },
  
  autoFixable: true,
  enabled: true,
  tags: ['security', 'owasp-a03', 'xss', 'injection', 'critical'],
  links: [
    'https://owasp.org/www-community/attacks/xss/',
    'https://cheatsheetseries.owasp.org/cheatsheets/Cross_Site_Scripting_Prevention_Cheat_Sheet.html'
  ]
}
```

---

### Rule SEC-031: Prevent Command Injection
```typescript
const preventCommandInjectionRule: ValidationRule = {
  id: 'SEC-031',
  name: 'Prevent Command Injection',
  category: RuleCategory.Security,
  severity: RuleSeverity.Critical,
  
  description: 'User input must not be used in system commands without validation',
  rationale: 'OWASP A03 - Command injection allows attackers to execute arbitrary system commands',
  
  examples: {
    bad: [
      `exec(\`git clone \${req.body.repo}\`)`,
      `child_process.exec('ping ' + userInput)`,
      `shell.exec(\`rm -rf \${dirName}\`)`
    ],
    good: [
      `execFile('git', ['clone', req.body.repo])`,
      `spawn('ping', ['-c', '4', validateHostname(userInput)])`,
      `rimraf(path.join(safeDir, sanitize(dirName)))`
    ]
  },
  
  check: (context: CodeContext): ValidationResult => {
    const violations: Violation[] = []
    
    for (const file of context.files) {
      // Pattern: exec/spawn with string interpolation
      const commandPatterns = [
        /(?:exec|spawn|execSync|spawnSync)\s*\(\s*[`'"].*?\$\{/g,
        /(?:exec|spawn|execSync|spawnSync)\s*\(\s*['"][^'"]*\s*\+/g
      ]
      
      for (const pattern of commandPatterns) {
        let match
        while ((match = pattern.exec(file.content)) !== null) {
          const lineNumber = file.content.substring(0, match.index).split('\n').length
          
          violations.push({
            ruleId: 'SEC-031',
            severity: RuleSeverity.Critical,
            message: 'Command injection risk: User input in system command',
            location: {
              file: file.path,
              line: lineNumber,
              snippet: match[0]
            },
            fix: {
              description: 'Use execFile() or spawn() with array arguments instead of string interpolation',
              autoFixable: false
            }
          })
        }
      }
    }
    
    return {
      passed: violations.length === 0,
      violations,
      score: violations.length === 0 ? 100 : 0
    }
  },
  
  autoFixable: false,
  enabled: true,
  tags: ['security', 'owasp-a03', 'command-injection', 'injection', 'critical'],
  links: [
    'https://owasp.org/www-community/attacks/Command_Injection'
  ]
}
```

---

## 🚦 A04: INSECURE DESIGN

### Rule SEC-040: Rate Limiting Required
```typescript
const rateLimitingRequiredRule: ValidationRule = {
  id: 'SEC-040',
  name: 'Rate Limiting Required',
  category: RuleCategory.Security,
  severity: RuleSeverity.High,
  
  description: 'Authentication routes must have rate limiting to prevent brute force attacks',
  rationale: 'OWASP A04 & A07 - Missing rate limiting allows brute force and DoS attacks',
  
  examples: {
    bad: [
      `router.post('/api/auth/login', async (req, res) => {
        const user = await authenticate(req.body)
        res.json({ token: createToken(user) })
      })`
    ],
    good: [
      `router.post('/api/auth/login', rateLimit({ max: 5, windowMs: 900000 }), async (req, res) => {
        const user = await authenticate(req.body)
        res.json({ token: createToken(user) })
      })`
    ]
  },
  
  check: (context: CodeContext): ValidationResult => {
    const violations: Violation[] = []
    
    for (const file of context.files) {
      // Find authentication routes
      const authRoutePattern = /router\.(?:post|put)\s*\(\s*['"]\/api\/auth\/(?:login|register|reset-password|verify)['"]/gi
      let match
      
      while ((match = authRoutePattern.exec(file.content)) !== null) {
        const lineIndex = match.index
        const lineNumber = file.content.substring(0, lineIndex).split('\n').length
        
        // Check if rate limiting middleware exists in this route
        const routeEnd = this.findRouteEnd(file.content, lineIndex)
        const routeCode = file.content.substring(lineIndex, routeEnd)
        
        const hasRateLimit = routeCode.match(/rateLimit|rateLimiter|limit\(/i)
        
        if (!hasRateLimit) {
          violations.push({
            ruleId: 'SEC-040',
            severity: RuleSeverity.High,
            message: 'Authentication route missing rate limiting',
            location: {
              file: file.path,
              line: lineNumber,
              snippet: match[0]
            },
            fix: {
              description: 'Add rate limiting middleware (e.g., express-rate-limit with max 5 requests per 15 minutes)',
              autoFixable: true,
              fixCode: `rateLimit({ max: 5, windowMs: 900000 })`
            }
          })
        }
      }
    }
    
    return {
      passed: violations.length === 0,
      violations,
      score: violations.length === 0 ? 100 : Math.max(0, 100 - violations.length * 25)
    }
  },
  
  autoFixable: true,
  enabled: true,
  tags: ['security', 'owasp-a04', 'owasp-a07', 'rate-limiting', 'brute-force', 'high'],
  links: [
    'https://owasp.org/Top10/A04_2021-Insecure_Design/',
    'https://cheatsheetseries.owasp.org/cheatsheets/Denial_of_Service_Cheat_Sheet.html'
  ]
}
```

---

## 🔧 A05: SECURITY MISCONFIGURATION

### Rule SEC-050: Security Headers Required
```typescript
const securityHeadersRule: ValidationRule = {
  id: 'SEC-050',
  name: 'Security Headers Required',
  category: RuleCategory.Security,
  severity: RuleSeverity.Medium,
  
  description: 'Web applications must set security headers (CSP, HSTS, X-Frame-Options, etc.)',
  rationale: 'OWASP A05 - Missing security headers leave applications vulnerable to various attacks',
  
  examples: {
    bad: [
      `app.listen(3000)  // No security headers configured`
    ],
    good: [
      `app.use(helmet())  // Sets multiple security headers`,
      `res.setHeader('X-Frame-Options', 'DENY')`,
      `res.setHeader('Content-Security-Policy', "default-src 'self'")`
    ]
  },
  
  check: (context: CodeContext): ValidationResult => {
    const violations: Violation[] = []
    
    // Only check web applications
    if (context.projectType !== 'web') {
      return { passed: true, score: 100 }
    }
    
    let hasHelmet = false
    let hasManualHeaders = false
    
    for (const file of context.files) {
      // Check for helmet middleware
      if (file.content.includes('helmet()')) {
        hasHelmet = true
      }
      
      // Check for manual header setting
      const securityHeaders = [
        'X-Frame-Options',
        'Content-Security-Policy',
        'Strict-Transport-Security',
        'X-Content-Type-Options',
        'X-XSS-Protection'
      ]
      
      for (const header of securityHeaders) {
        if (file.content.includes(header)) {
          hasManualHeaders = true
          break
        }
      }
    }
    
    if (!hasHelmet && !hasManualHeaders) {
      violations.push({
        ruleId: 'SEC-050',
        severity: RuleSeverity.Medium,
        message: 'No security headers configured',
        location: {
          file: 'Application configuration',
          snippet: ''
        },
        fix: {
          description: 'Add helmet middleware: npm install helmet && app.use(helmet())',
          autoFixable: true
        }
      })
    }
    
    return {
      passed: violations.length === 0,
      violations,
      score: violations.length === 0 ? 100 : 70
    }
  },
  
  autoFixable: true,
  enabled: true,
  tags: ['security', 'owasp-a05', 'headers', 'helmet', 'medium'],
  links: [
    'https://owasp.org/Top10/A05_2021-Security_Misconfiguration/',
    'https://cheatsheetseries.owasp.org/cheatsheets/HTTP_Headers_Cheat_Sheet.html'
  ]
}
```

---

## 📦 A06: VULNERABLE COMPONENTS

### Rule SEC-060: Dependency Vulnerability Check
```typescript
const dependencyVulnerabilityRule: ValidationRule = {
  id: 'SEC-060',
  name: 'Check for Vulnerable Dependencies',
  category: RuleCategory.Security,
  severity: RuleSeverity.High,
  
  description: 'Dependencies must not have known security vulnerabilities',
  rationale: 'OWASP A06 - Vulnerable dependencies can be exploited to compromise the application',
  
  examples: {
    bad: [
      `"express": "3.0.0"  // Very old version with known vulnerabilities`
    ],
    good: [
      `"express": "^4.18.0"  // Recent version with security patches`
    ]
  },
  
  check: async (context: CodeContext): Promise<ValidationResult> => {
    const violations: Violation[] = []
    
    // Find package.json
    const packageJson = context.files.find(f => f.path.endsWith('package.json'))
    if (!packageJson) {
      return { passed: true, score: 100 }
    }
    
    try {
      const pkg = JSON.parse(packageJson.content)
      const allDeps = {
        ...pkg.dependencies,
        ...pkg.devDependencies
      }
      
      // Check against known vulnerable versions
      const vulnerablePackages = await this.checkVulnerabilities(allDeps)
      
      for (const vuln of vulnerablePackages) {
        violations.push({
          ruleId: 'SEC-060',
          severity: this.mapSeverity(vuln.severity),
          message: `Vulnerable dependency: ${vuln.package}@${vuln.version} - ${vuln.vulnerability}`,
          location: {
            file: 'package.json',
            snippet: `"${vuln.package}": "${vuln.version}"`
          },
          fix: {
            description: `Update to ${vuln.package}@${vuln.fixedVersion}`,
            autoFixable: true,
            fixCode: `"${vuln.package}": "${vuln.fixedVersion}"`
          }
        })
      }
    } catch (error) {
      // Invalid package.json
    }
    
    const criticalCount = violations.filter(v => v.severity === RuleSeverity.Critical).length
    const score = Math.max(0, 100 - (criticalCount * 30) - (violations.length - criticalCount) * 10)
    
    return {
      passed: criticalCount === 0,
      violations,
      score
    }
  },
  
  autoFixable: true,
  enabled: true,
  tags: ['security', 'owasp-a06', 'dependencies', 'vulnerabilities', 'high'],
  links: [
    'https://owasp.org/Top10/A06_2021-Vulnerable_and_Outdated_Components/'
  ],
  
  async checkVulnerabilities(dependencies: Record<string, string>): Promise<Vulnerability[]> {
    // This would integrate with npm audit or Snyk API
    // Simplified for example
    return []
  },
  
  mapSeverity(severity: string): RuleSeverity {
    switch (severity.toLowerCase()) {
      case 'critical': return RuleSeverity.Critical
      case 'high': return RuleSeverity.High
      case 'medium': return RuleSeverity.Medium
      default: return RuleSeverity.Low
    }
  }
}

interface Vulnerability {
  package: string
  version: string
  vulnerability: string
  severity: string
  fixedVersion: string
}
```

---

## 🔑 A07: AUTHENTICATION FAILURES

### Rule SEC-070: Strong Password Policy
```typescript
const strongPasswordPolicyRule: ValidationRule = {
  id: 'SEC-070',
  name: 'Enforce Strong Password Policy',
  category: RuleCategory.Security,
  severity: RuleSeverity.Medium,
  
  description: 'Password requirements must enforce minimum length and complexity',
  rationale: 'OWASP A07 - Weak passwords are easily compromised through brute force or dictionary attacks',
  
  examples: {
    bad: [
      `if (password.length < 6) throw new Error('Password too short')`,
      `const minLength = 8  // Without complexity requirements`
    ],
    good: [
      `if (password.length < 12) throw new Error('Password must be at least 12 characters')`,
      `zod.string().min(12).regex(/[A-Z]/).regex(/[0-9]/).regex(/[!@#$%^&*]/)`,
      `validator.isStrongPassword(password, { minLength: 12, minLowercase: 1, minUppercase: 1, minNumbers: 1, minSymbols: 1 })`
    ]
  },
  
  check: (context: CodeContext): ValidationResult => {
    const violations: Violation[] = []
    
    for (const file of context.files) {
      // Pattern: password length validation
      const lengthPattern = /password\.length\s*[<>=!]+\s*(\d+)/gi
      let match
      
      while ((match = lengthPattern.exec(file.content)) !== null) {
        const minLength = parseInt(match[1])
        
        if (minLength < 12) {
          const lineNumber = file.content.substring(0, match.index).split('\n').length
          
          violations.push({
            ruleId: 'SEC-070',
            severity: RuleSeverity.Medium,
            message: `Weak password policy: Minimum length ${minLength} (recommended: 12+)`,
            location: {
              file: file.path,
              line: lineNumber,
              snippet: match[0]
            },
            fix: {
              description: 'Increase minimum password length to 12 characters',
              autoFixable: true,
              fixCode: match[0].replace(match[1], '12')
            }
          })
        }
      }
    }
    
    return {
      passed: violations.length === 0,
      violations,
      score: violations.length === 0 ? 100 : 80
    }
  },
  
  autoFixable: true,
  enabled: true,
  tags: ['security', 'owasp-a07', 'passwords', 'authentication', 'medium'],
  links: [
    'https://cheatsheetseries.owasp.org/cheatsheets/Authentication_Cheat_Sheet.html'
  ]
}
```

---

## 📊 SECURITY VALIDATION SUMMARY

### All Security Rules
```typescript
/**
 * Complete Security Rule Set
 */
const SECURITY_RULES: ValidationRule[] = [
  // A01: Broken Access Control
  authorizationCheckRule,           // SEC-010
  noIDORRule,                        // SEC-011
  
  // A02: Cryptographic Failures
  noPlaintextPasswordsRule,          // SEC-002 (from Article I)
  noHardcodedSecretsRule,           // SEC-020
  strongEncryptionRule,             // SEC-021
  
  // A03: Injection
  noSqlInjectionRule,               // SEC-001 (from Article I)
  preventXSSRule,                   // SEC-030
  preventCommandInjectionRule,      // SEC-031
  
  // A04: Insecure Design
  rateLimitingRequiredRule,         // SEC-040
  
  // A05: Security Misconfiguration
  securityHeadersRule,              // SEC-050
  
  // A06: Vulnerable Components
  dependencyVulnerabilityRule,      // SEC-060
  
  // A07: Authentication Failures
  strongPasswordPolicyRule,         // SEC-070
  
  // A09: Security Logging Failures
  noSensitiveDataInLogsRule,        // SEC-090 (to be defined)
  
  // A10: SSRF
  preventSSRFRule                   // SEC-100 (to be defined)
]
```

---

## 📚 RELATED ARTICLES

| Article | Purpose | Relationship to Security Validation |
|---------|---------|-------------------------------------|
| **Article I: Validation Framework** | Framework foundation | Security rules implement framework |
| **Article III: Performance Validation** | Performance rules | Complements security checks |
| **Article IV: Code Quality Validation** | Quality rules | Works alongside security |
| **Segment 1, Article VI** | Ten Commandments | Security rules enforce commandments |

---

**Previous:** [← Article I: Validation Framework](./01-article-i-validation-framework.md)  
**Next:** [Article III: Performance Validation →](./03-article-iii-performance-validation.md)

---

**Last Updated:** February 7, 2026  
**Constitutional Version:** 2.0.0  
**Status:** ✅ Ratified and In Force

**Motto:** *"Security First - OWASP Compliance is Non-Negotiable"*
