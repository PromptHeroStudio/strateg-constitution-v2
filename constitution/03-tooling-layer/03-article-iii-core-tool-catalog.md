# 🧰 ARTICLE III: CORE TOOL CATALOG

**Built-In Tools Available in Every MVCA Installation**

---

## 📜 CONSTITUTIONAL AUTHORITY

This article documents all **core tools** that are built into MVCA and available by default without any configuration. These tools provide essential functionality for file system operations, code analysis, validation, and search.

**Legal Force:**
- ✅ Core tools **MUST** be available in all MVCA installations
- ✅ Core tools **SHALL NOT** require external configuration (work out-of-box)
- ✅ Core tools **SHALL** follow the IToolInterface specification (Article I)
- ✅ Core tools **SHALL** be maintained by the MVCA core team
- ✅ Breaking changes to core tools **SHALL** follow deprecation policy (Segment 7)

**Constitutional Principle:**
> Core tools are the foundation - they must be reliable, always available, and require zero setup.  
> If it requires configuration, it's an integration tool, not a core tool.

---

## 🎯 CORE TOOL CATEGORIES

### Overview
```
CORE TOOLS (Built-In, No Configuration)
├─ FILE SYSTEM TOOLS (8 tools)
│  ├─ read_file
│  ├─ write_file
│  ├─ create_directory
│  ├─ list_directory
│  ├─ delete_file
│  ├─ move_file
│  ├─ copy_file
│  └─ file_exists
│
├─ CODE ANALYSIS TOOLS (6 tools)
│  ├─ parse_typescript
│  ├─ parse_javascript
│  ├─ analyze_dependencies
│  ├─ detect_imports
│  ├─ find_function
│  └─ extract_types
│
├─ VALIDATION TOOLS (5 tools)
│  ├─ validate_json
│  ├─ validate_yaml
│  ├─ validate_schema
│  ├─ lint_code
│  └─ type_check
│
└─ SEARCH TOOLS (4 tools)
   ├─ search_files
   ├─ grep_content
   ├─ find_references
   └─ find_definition

TOTAL: 23 Core Tools
```

---

## 📁 CATEGORY 1: FILE SYSTEM TOOLS

### Tool 1.1: read_file

**Purpose:** Read contents of a file from disk

**Security Tier:** ReadOnly (Tier 1)

**Specification:**
```typescript
export const readFileTool: IToolInterface = {
  // IDENTITY
  name: 'read_file',
  version: '1.0.0',
  category: ToolCategory.FileSystem,
  securityTier: SecurityTier.ReadOnly,
  
  // METADATA
  description: 'Read contents of a file from the file system',
  documentation: 'https://docs.mvca.ai/tools/core/read_file',
  
  // CAPABILITY
  capabilities: [{
    action: 'read',
    description: 'Read file contents as text or base64',
    requiresConfirmation: false,
    estimatedTime: 100
  }],
  
  parameters: z.object({
    path: z.string().describe('Absolute or relative file path'),
    encoding: z.enum(['utf8', 'base64', 'binary']).default('utf8')
      .describe('Encoding format for reading file'),
    maxSize: z.number().optional().describe('Maximum file size in bytes (safety limit)')
  }),
  
  returnType: z.object({
    content: z.string().describe('File contents'),
    size: z.number().describe('File size in bytes'),
    mimeType: z.string().optional().describe('Detected MIME type'),
    encoding: z.string().describe('Encoding used')
  }),
  
  // EXECUTION
  async execute(params: unknown): Promise<ToolResult> {
    const { path, encoding, maxSize } = this.parameters.parse(params)
    
    try {
      // Check file exists
      const exists = await fs.access(path).then(() => true).catch(() => false)
      if (!exists) {
        return {
          success: false,
          error: {
            code: ToolErrorCode.RESOURCE_NOT_FOUND,
            message: `File not found: ${path}`,
            recoverable: false,
            suggestion: 'Check the file path and try again.'
          }
        }
      }
      
      // Get file stats
      const stats = await fs.stat(path)
      
      // Check if file (not directory)
      if (!stats.isFile()) {
        return {
          success: false,
          error: {
            code: ToolErrorCode.VALIDATION_ERROR,
            message: `Path is not a file: ${path}`,
            recoverable: false,
            suggestion: 'Use list_directory for directories.'
          }
        }
      }
      
      // Check size limit
      if (maxSize && stats.size > maxSize) {
        return {
          success: false,
          error: {
            code: ToolErrorCode.VALIDATION_ERROR,
            message: `File too large: ${stats.size} bytes (max: ${maxSize})`,
            recoverable: false,
            suggestion: 'Increase maxSize parameter or read file in chunks.'
          }
        }
      }
      
      // Read file
      const content = await fs.readFile(path, encoding)
      
      // Detect MIME type
      const mimeType = this.detectMimeType(path, content.toString().slice(0, 100))
      
      return {
        success: true,
        data: {
          content: encoding === 'base64' ? content.toString('base64') : content.toString(),
          size: stats.size,
          mimeType,
          encoding
        },
        metadata: {
          duration: Date.now() - startTime,
          resourcesUsed: [path]
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
      return { valid: false, errors: ['Validation failed'] }
    }
  },
  
  handleError(error: Error): ToolError {
    if (error.message.includes('ENOENT')) {
      return {
        code: ToolErrorCode.RESOURCE_NOT_FOUND,
        message: 'File not found',
        recoverable: false,
        suggestion: 'Check the file path and try again.'
      }
    }
    
    if (error.message.includes('EACCES')) {
      return {
        code: ToolErrorCode.PERMISSION_DENIED,
        message: 'Permission denied',
        recoverable: false,
        suggestion: 'Check file permissions.'
      }
    }
    
    return {
      code: ToolErrorCode.TOOL_EXECUTION_FAILED,
      message: error.message,
      recoverable: false,
      originalError: error
    }
  },
  
  // HELPER METHODS
  detectMimeType(path: string, contentSample: string): string {
    const ext = path.split('.').pop()?.toLowerCase()
    
    const mimeTypes: Record<string, string> = {
      'txt': 'text/plain',
      'md': 'text/markdown',
      'json': 'application/json',
      'js': 'application/javascript',
      'ts': 'application/typescript',
      'jsx': 'application/javascript',
      'tsx': 'application/typescript',
      'html': 'text/html',
      'css': 'text/css',
      'xml': 'application/xml',
      'yaml': 'application/yaml',
      'yml': 'application/yaml',
      'png': 'image/png',
      'jpg': 'image/jpeg',
      'jpeg': 'image/jpeg',
      'gif': 'image/gif',
      'svg': 'image/svg+xml',
      'pdf': 'application/pdf',
      'zip': 'application/zip'
    }
    
    return mimeTypes[ext || ''] || 'application/octet-stream'
  }
}
```

**Usage Examples:**
```typescript
// Read text file
const result = await readFileTool.execute({
  path: './README.md',
  encoding: 'utf8'
})
// Returns: { content: "# Project Name\n...", size: 1024, mimeType: "text/markdown" }

// Read binary file as base64
const image = await readFileTool.execute({
  path: './logo.png',
  encoding: 'base64'
})
// Returns: { content: "iVBORw0KGgo...", size: 15234, mimeType: "image/png" }

// Read with size limit
const limited = await readFileTool.execute({
  path: './large-file.log',
  maxSize: 1048576  // 1MB max
})
// Returns error if file > 1MB
```

---

### Tool 1.2: write_file

**Purpose:** Write content to a file on disk

**Security Tier:** Write (Tier 2)

**Specification:**
```typescript
export const writeFileTool: IToolInterface = {
  name: 'write_file',
  version: '1.0.0',
  category: ToolCategory.FileSystem,
  securityTier: SecurityTier.Write,
  
  description: 'Write content to a file (creates file if not exists)',
  documentation: 'https://docs.mvca.ai/tools/core/write_file',
  
  capabilities: [{
    action: 'write',
    description: 'Write text or binary content to file',
    requiresConfirmation: true,  // User must confirm writes
    estimatedTime: 200
  }],
  
  parameters: z.object({
    path: z.string().describe('File path to write to'),
    content: z.string().describe('Content to write (text or base64)'),
    encoding: z.enum(['utf8', 'base64']).default('utf8'),
    createDirs: z.boolean().default(true)
      .describe('Create parent directories if they don\'t exist'),
    overwrite: z.boolean().default(false)
      .describe('Overwrite file if it exists'),
    backup: z.boolean().default(false)
      .describe('Create backup of existing file before overwriting')
  }),
  
  returnType: z.object({
    success: z.boolean(),
    bytesWritten: z.number(),
    path: z.string(),
    backupPath: z.string().optional()
  }),
  
  async execute(params: unknown): Promise<ToolResult> {
    const { path, content, encoding, createDirs, overwrite, backup } = 
      this.parameters.parse(params)
    
    try {
      // Check if file exists
      const exists = await fs.access(path).then(() => true).catch(() => false)
      
      if (exists && !overwrite) {
        return {
          success: false,
          error: {
            code: ToolErrorCode.RESOURCE_CONFLICT,
            message: `File already exists: ${path}`,
            recoverable: true,
            suggestion: 'Set overwrite: true to replace the file.'
          }
        }
      }
      
      // Create backup if requested
      let backupPath: string | undefined
      if (exists && backup) {
        backupPath = `${path}.backup-${Date.now()}`
        await fs.copyFile(path, backupPath)
      }
      
      // Create parent directories if needed
      if (createDirs) {
        const dir = require('path').dirname(path)
        await fs.mkdir(dir, { recursive: true })
      }
      
      // Write file
      const buffer = encoding === 'base64' 
        ? Buffer.from(content, 'base64')
        : Buffer.from(content, 'utf8')
      
      await fs.writeFile(path, buffer)
      
      return {
        success: true,
        data: {
          success: true,
          bytesWritten: buffer.length,
          path,
          backupPath
        },
        metadata: {
          duration: Date.now() - startTime,
          resourcesUsed: [path, backupPath].filter(Boolean) as string[]
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
      const parsed = this.parameters.parse(params)
      
      // Validate path (no dangerous paths)
      if (parsed.path.includes('..')) {
        return {
          valid: false,
          errors: ['Path traversal not allowed (contains ..)']
        }
      }
      
      return { valid: true, errors: [] }
    } catch (error) {
      if (error instanceof z.ZodError) {
        return {
          valid: false,
          errors: error.errors.map(e => e.message)
        }
      }
      return { valid: false, errors: ['Validation failed'] }
    }
  },
  
  handleError(error: Error): ToolError {
    if (error.message.includes('EACCES')) {
      return {
        code: ToolErrorCode.PERMISSION_DENIED,
        message: 'Permission denied - cannot write to file',
        recoverable: false,
        suggestion: 'Check file and directory permissions.'
      }
    }
    
    if (error.message.includes('ENOSPC')) {
      return {
        code: ToolErrorCode.RESOURCE_CONFLICT,
        message: 'No space left on device',
        recoverable: false,
        suggestion: 'Free up disk space and try again.'
      }
    }
    
    return {
      code: ToolErrorCode.TOOL_EXECUTION_FAILED,
      message: error.message,
      recoverable: false,
      originalError: error
    }
  }
}
```

**Usage Examples:**
```typescript
// Write text file
await writeFileTool.execute({
  path: './output.txt',
  content: 'Hello World',
  encoding: 'utf8'
})

// Write with backup (safe overwrite)
await writeFileTool.execute({
  path: './config.json',
  content: JSON.stringify(config, null, 2),
  overwrite: true,
  backup: true
})

// Write binary file
await writeFileTool.execute({
  path: './image.png',
  content: base64ImageData,
  encoding: 'base64'
})
```

---

### Tool 1.3: list_directory

**Purpose:** List files and directories in a directory

**Security Tier:** ReadOnly (Tier 1)

**Specification:**
```typescript
export const listDirectoryTool: IToolInterface = {
  name: 'list_directory',
  version: '1.0.0',
  category: ToolCategory.FileSystem,
  securityTier: SecurityTier.ReadOnly,
  
  description: 'List files and subdirectories in a directory',
  documentation: 'https://docs.mvca.ai/tools/core/list_directory',
  
  capabilities: [{
    action: 'list',
    description: 'List directory contents with metadata',
    requiresConfirmation: false,
    estimatedTime: 100
  }],
  
  parameters: z.object({
    path: z.string().describe('Directory path to list'),
    recursive: z.boolean().default(false)
      .describe('Recursively list subdirectories'),
    maxDepth: z.number().default(3)
      .describe('Maximum recursion depth (if recursive)'),
    includeHidden: z.boolean().default(false)
      .describe('Include hidden files (starting with .)'),
    filter: z.object({
      extensions: z.array(z.string()).optional()
        .describe('Filter by file extensions (e.g., [".ts", ".js"])'),
      pattern: z.string().optional()
        .describe('Filter by glob pattern (e.g., "*.test.ts")')
    }).optional()
  }),
  
  returnType: z.object({
    entries: z.array(z.object({
      name: z.string(),
      path: z.string(),
      type: z.enum(['file', 'directory', 'symlink']),
      size: z.number().optional(),
      modified: z.string(),
      extension: z.string().optional()
    })),
    totalFiles: z.number(),
    totalDirectories: z.number(),
    totalSize: z.number()
  }),
  
  async execute(params: unknown): Promise<ToolResult> {
    const { path, recursive, maxDepth, includeHidden, filter } = 
      this.parameters.parse(params)
    
    try {
      const entries = await this.listDir(path, recursive, maxDepth, includeHidden, filter, 0)
      
      const files = entries.filter(e => e.type === 'file')
      const directories = entries.filter(e => e.type === 'directory')
      const totalSize = files.reduce((sum, f) => sum + (f.size || 0), 0)
      
      return {
        success: true,
        data: {
          entries,
          totalFiles: files.length,
          totalDirectories: directories.length,
          totalSize
        },
        metadata: {
          duration: Date.now() - startTime
        }
      }
      
    } catch (error) {
      return {
        success: false,
        error: this.handleError(error as Error)
      }
    }
  },
  
  async listDir(
    dirPath: string,
    recursive: boolean,
    maxDepth: number,
    includeHidden: boolean,
    filter: any,
    currentDepth: number
  ): Promise<any[]> {
    const entries: any[] = []
    
    const items = await fs.readdir(dirPath, { withFileTypes: true })
    
    for (const item of items) {
      // Skip hidden files if requested
      if (!includeHidden && item.name.startsWith('.')) {
        continue
      }
      
      const fullPath = require('path').join(dirPath, item.name)
      const stats = await fs.stat(fullPath)
      
      const entry = {
        name: item.name,
        path: fullPath,
        type: item.isDirectory() ? 'directory' : 
              item.isSymbolicLink() ? 'symlink' : 'file',
        size: item.isFile() ? stats.size : undefined,
        modified: stats.mtime.toISOString(),
        extension: item.isFile() ? require('path').extname(item.name) : undefined
      }
      
      // Apply filters
      if (filter) {
        if (filter.extensions && entry.type === 'file') {
          if (!filter.extensions.includes(entry.extension)) {
            continue
          }
        }
        
        if (filter.pattern) {
          const minimatch = require('minimatch')
          if (!minimatch(entry.name, filter.pattern)) {
            continue
          }
        }
      }
      
      entries.push(entry)
      
      // Recurse into subdirectories
      if (recursive && item.isDirectory() && currentDepth < maxDepth) {
        const subEntries = await this.listDir(
          fullPath,
          recursive,
          maxDepth,
          includeHidden,
          filter,
          currentDepth + 1
        )
        entries.push(...subEntries)
      }
    }
    
    return entries
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
      return { valid: false, errors: ['Validation failed'] }
    }
  },
  
  handleError(error: Error): ToolError {
    if (error.message.includes('ENOENT')) {
      return {
        code: ToolErrorCode.RESOURCE_NOT_FOUND,
        message: 'Directory not found',
        recoverable: false
      }
    }
    
    if (error.message.includes('ENOTDIR')) {
      return {
        code: ToolErrorCode.VALIDATION_ERROR,
        message: 'Path is not a directory',
        recoverable: false,
        suggestion: 'Use read_file for files.'
      }
    }
    
    return {
      code: ToolErrorCode.TOOL_EXECUTION_FAILED,
      message: error.message,
      recoverable: false,
      originalError: error
    }
  }
}
```

**Usage Examples:**
```typescript
// List current directory
await listDirectoryTool.execute({
  path: '.'
})
// Returns: { entries: [{ name: 'src', type: 'directory', ... }], totalFiles: 15, ... }

// Recursive list with filter
await listDirectoryTool.execute({
  path: './src',
  recursive: true,
  maxDepth: 3,
  filter: {
    extensions: ['.ts', '.tsx']
  }
})
// Returns only TypeScript files

// List with pattern
await listDirectoryTool.execute({
  path: './tests',
  recursive: true,
  filter: {
    pattern: '*.test.ts'
  }
})
// Returns only test files
```

---

### Tool 1.4-1.8: Additional File System Tools (Summary)
```typescript
// Tool 1.4: create_directory
{
  name: 'create_directory',
  purpose: 'Create a new directory (with parent directories if needed)',
  securityTier: SecurityTier.Write,
  parameters: { path: string, recursive: boolean }
}

// Tool 1.5: delete_file
{
  name: 'delete_file',
  purpose: 'Delete a file from the file system',
  securityTier: SecurityTier.Write,
  requiresConfirmation: true,  // Destructive operation
  parameters: { path: string, backup: boolean }
}

// Tool 1.6: move_file
{
  name: 'move_file',
  purpose: 'Move or rename a file',
  securityTier: SecurityTier.Write,
  parameters: { source: string, destination: string, overwrite: boolean }
}

// Tool 1.7: copy_file
{
  name: 'copy_file',
  purpose: 'Copy a file to a new location',
  securityTier: SecurityTier.Write,
  parameters: { source: string, destination: string, overwrite: boolean }
}

// Tool 1.8: file_exists
{
  name: 'file_exists',
  purpose: 'Check if a file or directory exists',
  securityTier: SecurityTier.ReadOnly,
  parameters: { path: string },
  returnType: { exists: boolean, type?: 'file' | 'directory' }
}
```

---

## 🔍 CATEGORY 2: CODE ANALYSIS TOOLS

### Tool 2.1: parse_typescript

**Purpose:** Parse TypeScript/JavaScript code into Abstract Syntax Tree (AST)

**Security Tier:** ReadOnly (Tier 1)

**Specification:**
```typescript
export const parseTypescriptTool: IToolInterface = {
  name: 'parse_typescript',
  version: '1.0.0',
  category: ToolCategory.CodeAnalysis,
  securityTier: SecurityTier.ReadOnly,
  
  description: 'Parse TypeScript or JavaScript code into AST (Abstract Syntax Tree)',
  documentation: 'https://docs.mvca.ai/tools/core/parse_typescript',
  
  capabilities: [{
    action: 'parse',
    description: 'Parse code and extract structure, imports, exports, functions, types',
    requiresConfirmation: false,
    estimatedTime: 300
  }],
  
  parameters: z.object({
    code: z.string().describe('TypeScript or JavaScript code to parse'),
    filePath: z.string().optional()
      .describe('File path (for better error messages)'),
    jsx: z.boolean().default(false)
      .describe('Parse JSX/TSX syntax'),
    extractInfo: z.object({
      imports: z.boolean().default(true),
      exports: z.boolean().default(true),
      functions: z.boolean().default(true),
      classes: z.boolean().default(true),
      interfaces: z.boolean().default(true),
      types: z.boolean().default(true),
      variables: z.boolean().default(false)
    }).optional()
  }),
  
  returnType: z.object({
    ast: z.any().describe('Abstract Syntax Tree'),
    errors: z.array(z.object({
      line: z.number(),
      column: z.number(),
      message: z.string()
    })),
    imports: z.array(z.object({
      source: z.string(),
      specifiers: z.array(z.string()),
      isDefault: z.boolean(),
      isTypeOnly: z.boolean()
    })).optional(),
    exports: z.array(z.object({
      name: z.string(),
      type: z.enum(['function', 'class', 'variable', 'type', 'interface'])
    })).optional(),
    functions: z.array(z.object({
      name: z.string(),
      parameters: z.array(z.string()),
      returnType: z.string().optional(),
      isAsync: z.boolean(),
      line: z.number()
    })).optional()
  }),
  
  async execute(params: unknown): Promise<ToolResult> {
    const { code, filePath, jsx, extractInfo } = this.parameters.parse(params)
    
    try {
      const ts = await import('typescript')
      
      // Parse code
      const sourceFile = ts.createSourceFile(
        filePath || 'temp.ts',
        code,
        ts.ScriptTarget.Latest,
        true,
        jsx ? ts.ScriptKind.TSX : ts.ScriptKind.TS
      )
      
      // Extract syntax errors
      const errors: any[] = []
      const diagnostics = [
        ...ts.getPreEmitDiagnostics(sourceFile as any),
        ...sourceFile.parseDiagnostics
      ]
      
      for (const diagnostic of diagnostics) {
        if (diagnostic.file && diagnostic.start !== undefined) {
          const { line, character } = diagnostic.file.getLineAndCharacterOfPosition(diagnostic.start)
          errors.push({
            line: line + 1,
            column: character + 1,
            message: ts.flattenDiagnosticMessageText(diagnostic.messageText, '\n')
          })
        }
      }
      
      // Extract requested information
      const result: any = {
        ast: this.simplifyAST(sourceFile),
        errors
      }
      
      if (extractInfo?.imports) {
        result.imports = this.extractImports(sourceFile, ts)
      }
      
      if (extractInfo?.exports) {
        result.exports = this.extractExports(sourceFile, ts)
      }
      
      if (extractInfo?.functions) {
        result.functions = this.extractFunctions(sourceFile, ts)
      }
      
      return {
        success: true,
        data: result,
        metadata: {
          duration: Date.now() - startTime
        }
      }
      
    } catch (error) {
      return {
        success: false,
        error: this.handleError(error as Error)
      }
    }
  },
  
  // Extract imports from AST
  extractImports(sourceFile: any, ts: any): any[] {
    const imports: any[] = []
    
    ts.forEachChild(sourceFile, (node: any) => {
      if (ts.isImportDeclaration(node)) {
        const moduleSpecifier = node.moduleSpecifier.text
        const specifiers: string[] = []
        let isDefault = false
        let isTypeOnly = node.importClause?.isTypeOnly || false
        
        if (node.importClause) {
          // Default import
          if (node.importClause.name) {
            specifiers.push(node.importClause.name.text)
            isDefault = true
          }
          
          // Named imports
          if (node.importClause.namedBindings) {
            if (ts.isNamedImports(node.importClause.namedBindings)) {
              for (const element of node.importClause.namedBindings.elements) {
                specifiers.push(element.name.text)
              }
            }
            // Namespace import (import * as X)
            else if (ts.isNamespaceImport(node.importClause.namedBindings)) {
              specifiers.push(`* as ${node.importClause.namedBindings.name.text}`)
            }
          }
        }
        
        imports.push({
          source: moduleSpecifier,
          specifiers,
          isDefault,
          isTypeOnly
        })
      }
    })
    
    return imports
  },
  
  // Extract exports from AST
  extractExports(sourceFile: any, ts: any): any[] {
    const exports: any[] = []
    
    ts.forEachChild(sourceFile, (node: any) => {
      // Export declarations
      if (ts.isExportDeclaration(node)) {
        // export { x, y }
        if (node.exportClause && ts.isNamedExports(node.exportClause)) {
          for (const element of node.exportClause.elements) {
            exports.push({
              name: element.name.text,
              type: 'variable'
            })
          }
        }
      }
      
      // Exported functions
      if (ts.isFunctionDeclaration(node) && 
          node.modifiers?.some((m: any) => m.kind === ts.SyntaxKind.ExportKeyword)) {
        exports.push({
          name: node.name?.text || 'anonymous',
          type: 'function'
        })
      }
      
      // Exported classes
      if (ts.isClassDeclaration(node) && 
          node.modifiers?.some((m: any) => m.kind === ts.SyntaxKind.ExportKeyword)) {
        exports.push({
          name: node.name?.text || 'anonymous',
          type: 'class'
        })
      }
      
      // Exported interfaces
      if (ts.isInterfaceDeclaration(node) && 
          node.modifiers?.some((m: any) => m.kind === ts.SyntaxKind.ExportKeyword)) {
        exports.push({
          name: node.name.text,
          type: 'interface'
        })
      }
      
      // Exported types
      if (ts.isTypeAliasDeclaration(node) && 
          node.modifiers?.some((m: any) => m.kind === ts.SyntaxKind.ExportKeyword)) {
        exports.push({
          name: node.name.text,
          type: 'type'
        })
      }
    })
    
    return exports
  },
  
  // Extract functions from AST
  extractFunctions(sourceFile: any, ts: any): any[] {
    const functions: any[] = []
    
    const visit = (node: any) => {
      if (ts.isFunctionDeclaration(node)) {
        const params = node.parameters.map((p: any) => p.name.text)
        const returnType = node.type ? node.type.getText(sourceFile) : undefined
        const isAsync = node.modifiers?.some((m: any) => 
          m.kind === ts.SyntaxKind.AsyncKeyword
        ) || false
        
        functions.push({
          name: node.name?.text || 'anonymous',
          parameters: params,
          returnType,
          isAsync,
          line: sourceFile.getLineAndCharacterOfPosition(node.pos).line + 1
        })
      }
      
      ts.forEachChild(node, visit)
    }
    
    visit(sourceFile)
    return functions
  },
  
  // Simplify AST for JSON serialization
  simplifyAST(node: any): any {
    const { kind, pos, end, ...rest } = node
    const simplified: any = { kind: node.kind }
    
    for (const key in rest) {
      if (key === 'parent' || key === 'flowNode') continue
      
      const value = rest[key]
      if (Array.isArray(value)) {
        simplified[key] = value.map(v => this.simplifyAST(v))
      } else if (typeof value === 'object' && value !== null) {
        simplified[key] = this.simplifyAST(value)
      } else {
        simplified[key] = value
      }
    }
    
    return simplified
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
      return { valid: false, errors: ['Validation failed'] }
    }
  },
  
  handleError(error: Error): ToolError {
    return {
      code: ToolErrorCode.TOOL_EXECUTION_FAILED,
      message: `Parse error: ${error.message}`,
      recoverable: false,
      originalError: error
    }
  }
}
```

**Usage Examples:**
```typescript
// Parse TypeScript file
const result = await parseTypescriptTool.execute({
  code: `
    import { useState } from 'react'
    
    export function Counter() {
      const [count, setCount] = useState(0)
      return <button onClick={() => setCount(count + 1)}>{count}</button>
    }
  `,
  jsx: true,
  extractInfo: {
    imports: true,
    exports: true,
    functions: true
  }
})

// Returns:
// {
//   imports: [{ source: 'react', specifiers: ['useState'], isDefault: false }],
//   exports: [{ name: 'Counter', type: 'function' }],
//   functions: [{ name: 'Counter', parameters: [], isAsync: false }],
//   errors: []
// }
```

---

### Tool 2.2-2.6: Additional Code Analysis Tools (Summary)
```typescript
// Tool 2.2: analyze_dependencies
{
  name: 'analyze_dependencies',
  purpose: 'Analyze npm dependencies for vulnerabilities, outdated packages',
  parameters: { packageJsonPath: string },
  returnType: {
    vulnerabilities: Array<{ package, severity, fixVersion }>,
    outdated: Array<{ package, current, latest }>
  }
}

// Tool 2.3: detect_imports
{
  name: 'detect_imports',
  purpose: 'Extract all imports from a code file',
  parameters: { code: string, language: 'typescript' | 'javascript' | 'python' },
  returnType: { imports: Array<{ source, specifiers }> }
}

// Tool 2.4: find_function
{
  name: 'find_function',
  purpose: 'Find function definition in codebase',
  parameters: { functionName: string, directory: string },
  returnType: { file: string, line: number, definition: string }
}

// Tool 2.5: extract_types
{
  name: 'extract_types',
  purpose: 'Extract TypeScript type definitions from code',
  parameters: { code: string },
  returnType: { interfaces: Array, types: Array, enums: Array }
}

// Tool 2.6: parse_javascript
{
  name: 'parse_javascript',
  purpose: 'Parse plain JavaScript (no TypeScript)',
  parameters: { code: string },
  returnType: { ast: any, errors: Array }
}
```

---

## ✅ CATEGORY 3: VALIDATION TOOLS

### Tool 3.1: validate_json

**Purpose:** Validate JSON data against JSON Schema

**Security Tier:** ReadOnly (Tier 1)

**Specification:**
```typescript
export const validateJsonTool: IToolInterface = {
  name: 'validate_json',
  version: '1.0.0',
  category: ToolCategory.Validation,
  securityTier: SecurityTier.ReadOnly,
  
  description: 'Validate JSON data against a JSON Schema',
  documentation: 'https://docs.mvca.ai/tools/core/validate_json',
  
  capabilities: [{
    action: 'validate',
    description: 'Validate JSON structure and data types',
    requiresConfirmation: false,
    estimatedTime: 100
  }],
  
  parameters: z.object({
    data: z.string().describe('JSON string to validate'),
    schema: z.any().optional().describe('JSON Schema to validate against'),
    strict: z.boolean().default(true)
      .describe('Strict mode (no additional properties)')
  }),
  
  returnType: z.object({
    valid: z.boolean(),
    errors: z.array(z.object({
      path: z.string(),
      message: z.string(),
      value: z.any().optional()
    })),
    warnings: z.array(z.object({
      message: z.string()
    })).optional()
  }),
  
  async execute(params: unknown): Promise<ToolResult> {
    const { data, schema, strict } = this.parameters.parse(params)
    
    try {
      // Parse JSON
      let parsed: any
      try {
        parsed = JSON.parse(data)
      } catch (parseError) {
        return {
          success: true,
          data: {
            valid: false,
            errors: [{
              path: '$',
              message: `Invalid JSON: ${parseError.message}`
            }]
          }
        }
      }
      
      // If no schema provided, just check if valid JSON
      if (!schema) {
        return {
          success: true,
          data: {
            valid: true,
            errors: []
          }
        }
      }
      
      // Validate against schema
      const Ajv = require('ajv')
      const ajv = new Ajv({ allErrors: true, strict: strict })
      const validate = ajv.compile(schema)
      const valid = validate(parsed)
      
      const errors = valid ? [] : (validate.errors || []).map((err: any) => ({
        path: err.instancePath || '$',
        message: err.message || 'Validation failed',
        value: err.data
      }))
      
      return {
        success: true,
        data: {
          valid,
          errors
        },
        metadata: {
          duration: Date.now() - startTime
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
      return { valid: false, errors: ['Validation failed'] }
    }
  },
  
  handleError(error: Error): ToolError {
    return {
      code: ToolErrorCode.TOOL_EXECUTION_FAILED,
      message: error.message,
      recoverable: false,
      originalError: error
    }
  }
}
```

**Usage Examples:**
```typescript
// Validate simple JSON
await validateJsonTool.execute({
  data: '{"name": "John", "age": 30}'
})
// Returns: { valid: true, errors: [] }

// Validate against schema
await validateJsonTool.execute({
  data: '{"name": "John", "age": "thirty"}',
  schema: {
    type: 'object',
    properties: {
      name: { type: 'string' },
      age: { type: 'number' }
    },
    required: ['name', 'age']
  }
})
// Returns: { valid: false, errors: [{ path: '/age', message: 'must be number' }] }
```

---

### Tool 3.2-3.5: Additional Validation Tools (Summary)
```typescript
// Tool 3.2: validate_yaml
{
  name: 'validate_yaml',
  purpose: 'Validate YAML syntax and structure',
  parameters: { data: string, schema?: any },
  returnType: { valid: boolean, errors: Array }
}

// Tool 3.3: validate_schema
{
  name: 'validate_schema',
  purpose: 'Validate Zod/JSON/OpenAPI schema definition',
  parameters: { schema: any, type: 'zod' | 'json' | 'openapi' },
  returnType: { valid: boolean, errors: Array }
}

// Tool 3.4: lint_code
{
  name: 'lint_code',
  purpose: 'Run ESLint on JavaScript/TypeScript code',
  parameters: { code: string, config?: any },
  returnType: { errors: Array, warnings: Array, fixable: number }
}

// Tool 3.5: type_check
{
  name: 'type_check',
  purpose: 'Run TypeScript compiler type checking',
  parameters: { code: string, compilerOptions?: any },
  returnType: { errors: Array, warnings: Array }
}
```

---

## 🔎 CATEGORY 4: SEARCH TOOLS

### Tool 4.1: search_files

**Purpose:** Search for files by name or pattern

**Security Tier:** ReadOnly (Tier 1)

**Specification:**
```typescript
export const searchFilesTool: IToolInterface = {
  name: 'search_files',
  version: '1.0.0',
  category: ToolCategory.Search,
  securityTier: SecurityTier.ReadOnly,
  
  description: 'Search for files by name, extension, or glob pattern',
  documentation: 'https://docs.mvca.ai/tools/core/search_files',
  
  capabilities: [{
    action: 'search',
    description: 'Find files matching criteria',
    requiresConfirmation: false,
    estimatedTime: 500
  }],
  
  parameters: z.object({
    directory: z.string().describe('Directory to search in'),
    pattern: z.string().optional()
      .describe('Glob pattern (e.g., "**/*.ts", "test-*.js")'),
    name: z.string().optional()
      .describe('Exact or partial file name'),
    extension: z.string().optional()
      .describe('File extension (e.g., ".ts", ".js")'),
    maxResults: z.number().default(100)
      .describe('Maximum number of results'),
    maxDepth: z.number().default(10)
      .describe('Maximum directory depth')
  }),
  
  returnType: z.object({
    files: z.array(z.object({
      path: z.string(),
      name: z.string(),
      size: z.number(),
      modified: z.string()
    })),
    totalFound: z.number(),
    truncated: z.boolean()
  }),
  
  async execute(params: unknown): Promise<ToolResult> {
    const { directory, pattern, name, extension, maxResults, maxDepth } = 
      this.parameters.parse(params)
    
    try {
      const glob = require('glob')
      const path = require('path')
      
      let searchPattern: string
      
      if (pattern) {
        searchPattern = pattern
      } else if (name && extension) {
        searchPattern = `**/*${name}*${extension}`
      } else if (name) {
        searchPattern = `**/*${name}*`
      } else if (extension) {
        searchPattern = `**/*${extension}`
      } else {
        searchPattern = '**/*'
      }
      
      const matches = await glob.glob(searchPattern, {
        cwd: directory,
        nodir: true,
        maxDepth: maxDepth,
        absolute: true
      })
      
      const files: any[] = []
      
      for (let i = 0; i < Math.min(matches.length, maxResults); i++) {
        const filePath = matches[i]
        const stats = await fs.stat(filePath)
        
        files.push({
          path: filePath,
          name: path.basename(filePath),
          size: stats.size,
          modified: stats.mtime.toISOString()
        })
      }
      
      return {
        success: true,
        data: {
          files,
          totalFound: matches.length,
          truncated: matches.length > maxResults
        },
        metadata: {
          duration: Date.now() - startTime
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
      return { valid: false, errors: ['Validation failed'] }
    }
  },
  
  handleError(error: Error): ToolError {
    return {
      code: ToolErrorCode.TOOL_EXECUTION_FAILED,
      message: error.message,
      recoverable: false,
      originalError: error
    }
  }
}
```

**Usage Examples:**
```typescript
// Search by extension
await searchFilesTool.execute({
  directory: './src',
  extension: '.ts'
})

// Search by pattern
await searchFilesTool.execute({
  directory: './tests',
  pattern: '**/*.test.ts'
})

// Search by name
await searchFilesTool.execute({
  directory: './src',
  name: 'user',
  extension: '.tsx'
})
// Finds: UserProfile.tsx, UserList.tsx, EditUser.tsx, etc.
```

---

### Tool 4.2-4.4: Additional Search Tools (Summary)
```typescript
// Tool 4.2: grep_content
{
  name: 'grep_content',
  purpose: 'Search for text content within files (like grep)',
  parameters: { 
    directory: string, 
    query: string, 
    filePattern?: string,
    caseSensitive?: boolean,
    regex?: boolean
  },
  returnType: {
    matches: Array<{ file: string, line: number, content: string, match: string }>
  }
}

// Tool 4.3: find_references
{
  name: 'find_references',
  purpose: 'Find all references to a function/variable in codebase',
  parameters: { identifier: string, directory: string },
  returnType: {
    references: Array<{ file: string, line: number, context: string }>
  }
}

// Tool 4.4: find_definition
{
  name: 'find_definition',
  purpose: 'Find where a function/class/type is defined',
  parameters: { identifier: string, directory: string },
  returnType: {
    definition: { file: string, line: number, code: string }
  }
}
```

---

## 📊 CORE TOOLS SUMMARY TABLE

| Tool Name | Category | Security Tier | Confirmation Required | Description |
|-----------|----------|---------------|----------------------|-------------|
| read_file | FileSystem | ReadOnly | No | Read file contents |
| write_file | FileSystem | Write | Yes | Write to file |
| create_directory | FileSystem | Write | No | Create directory |
| list_directory | FileSystem | ReadOnly | No | List directory contents |
| delete_file | FileSystem | Write | Yes | Delete file |
| move_file | FileSystem | Write | Yes | Move/rename file |
| copy_file | FileSystem | Write | No | Copy file |
| file_exists | FileSystem | ReadOnly | No | Check file existence |
| parse_typescript | CodeAnalysis | ReadOnly | No | Parse TS/JS to AST |
| parse_javascript | CodeAnalysis | ReadOnly | No | Parse JS to AST |
| analyze_dependencies | CodeAnalysis | ReadOnly | No | Check npm packages |
| detect_imports | CodeAnalysis | ReadOnly | No | Extract imports |
| find_function | CodeAnalysis | ReadOnly | No | Find function definition |
| extract_types | CodeAnalysis | ReadOnly | No | Extract type definitions |
| validate_json | Validation | ReadOnly | No | Validate JSON |
| validate_yaml | Validation | ReadOnly | No | Validate YAML |
| validate_schema | Validation | ReadOnly | No | Validate schema |
| lint_code | Validation | ReadOnly | No | Run ESLint |
| type_check | Validation | ReadOnly | No | TypeScript type check |
| search_files | Search | ReadOnly | No | Find files by pattern |
| grep_content | Search | ReadOnly | No | Search text in files |
| find_references | Search | ReadOnly | No | Find code references |
| find_definition | Search | ReadOnly | No | Find code definition |

**Total: 23 Core Tools**

---

## 📚 RELATED ARTICLES

| Article | Purpose | Relationship to Core Tools |
|---------|---------|---------------------------|
| **Article I: Tool Architecture** | Tool interface spec | All core tools implement IToolInterface |
| **Article II: MCP Integration** | MCP protocol | Core tools can be wrapped as MCP servers |
| **Article IV: Integration Tool Catalog** | Optional tools | Builds on core tools foundation |
| **Segment 4: Execution Layer** | Orchestration | Uses core tools in workflows |

---

**Previous:** [← Article II: MCP Integration](./02-article-ii-mcp-integration.md)  
**Next:** [Article IV: Integration Tool Catalog →](./04-article-iv-integration-tool-catalog.md)

---

**Last Updated:** February 7, 2026  
**Constitutional Version:** 2.0.0  
**Status:** ✅ Ratified and In Force

**Motto:** *"Core Tools: The Foundation of Every MVCA Installation - Always Available, Zero Configuration"*
