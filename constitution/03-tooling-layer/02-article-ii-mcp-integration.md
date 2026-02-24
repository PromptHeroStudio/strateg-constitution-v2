# 📡 ARTICLE II: MCP INTEGRATION

**Model Context Protocol: Extending MVCA Through Standardized Server Communication**

---

## 📜 CONSTITUTIONAL AUTHORITY

This article defines how MVCA integrates with **MCP (Model Context Protocol)** servers to extend its capabilities beyond built-in tools. MCP is Anthropic's standardized protocol for AI-to-server communication.

**Legal Force:**
- ✅ MVCA **SHALL** support MCP protocol for extensibility
- ✅ MCP servers **MUST** follow the MCP specification (v1.0+)
- ✅ MCP resources **SHALL** be sandboxed (security isolation)
- ✅ MCP server failures **SHALL NOT** crash MVCA (graceful degradation)
- ✅ Users **SHALL** control which MCP servers are enabled (sovereignty)

**Constitutional Principle:**
> MCP enables infinite extensibility while maintaining security boundaries.  
> Standardized protocols prevent vendor lock-in and enable ecosystem growth.

---

## 🎯 WHAT IS MCP?

### Model Context Protocol Overview

**MCP (Model Context Protocol)** is Anthropic's open standard for connecting AI models to external data sources and tools.

**Key Concepts:**
```
┌─────────────────────────────────────────────────────────┐
│                      MVCA (Client)                       │
│  ┌──────────────────────────────────────────────────┐  │
│  │           MCP Client Implementation               │  │
│  │  - Discovers MCP servers                         │  │
│  │  - Sends prompts/tools requests                  │  │
│  │  - Receives resources/tool results               │  │
│  └──────────────────────────────────────────────────┘  │
└────────────────────┬────────────────────────────────────┘
                     │ MCP Protocol (JSON-RPC over stdio/HTTP)
                     │
        ┌────────────┼────────────┐
        │            │            │
        ▼            ▼            ▼
┌──────────────┐ ┌──────────────┐ ┌──────────────┐
│ MCP Server 1 │ │ MCP Server 2 │ │ MCP Server 3 │
│  (Database)  │ │ (File System)│ │   (GitHub)   │
└──────────────┘ └──────────────┘ └──────────────┘
        │            │            │
        └────────────┴────────────┘
         Each server provides:
         - Resources (data sources)
         - Tools (actions)
         - Prompts (templates)
```

**MCP vs Traditional APIs:**
```
TRADITIONAL API INTEGRATION:
- Custom code for each service
- Different authentication methods
- Inconsistent error handling
- No standardized discovery
- Tight coupling to service

MCP INTEGRATION:
- Standardized protocol for all services
- Consistent authentication (OAuth, API keys)
- Standardized error format
- Auto-discovery of capabilities
- Loose coupling (swappable servers)
```

---

## 🏗️ MCP ARCHITECTURE

### Core Components
```typescript
/**
 * MCP Server Specification
 * What every MCP server must implement
 */
interface MCPServer {
  // SERVER IDENTITY
  name: string                      // Server name (e.g., "postgres-db")
  version: string                   // Server version (e.g., "1.0.0")
  protocol: {
    version: string                 // MCP protocol version (e.g., "1.0")
    transport: 'stdio' | 'http'     // Communication method
  }
  
  // SERVER CAPABILITIES
  capabilities: {
    resources?: ResourceCapability   // Can provide data sources
    tools?: ToolCapability          // Can execute actions
    prompts?: PromptCapability      // Can provide prompt templates
    logging?: LoggingCapability     // Can emit logs
  }
  
  // SERVER LIFECYCLE
  initialize(): Promise<InitializeResult>
  shutdown(): Promise<void>
}

/**
 * MCP Resource
 * A data source that can be read by the AI
 */
interface MCPResource {
  uri: string                       // Resource identifier (e.g., "file:///path/to/file")
  name: string                      // Human-readable name
  description?: string              // What this resource contains
  mimeType?: string                // Content type (e.g., "text/plain", "application/json")
}

/**
 * MCP Tool
 * An action that can be executed by the AI
 */
interface MCPTool {
  name: string                      // Tool identifier (e.g., "query_database")
  description: string               // What this tool does
  inputSchema: JSONSchema           // Input parameters (JSON Schema)
}

/**
 * MCP Prompt
 * A reusable prompt template
 */
interface MCPPrompt {
  name: string                      // Prompt identifier (e.g., "code_review")
  description: string               // What this prompt does
  arguments?: PromptArgument[]     // Template variables
}
```

---

### MCP Communication Protocol

**MCP uses JSON-RPC 2.0 over stdio or HTTP:**
```typescript
/**
 * MCP Request Format (JSON-RPC 2.0)
 */
interface MCPRequest {
  jsonrpc: '2.0'
  id: string | number              // Request ID (for matching responses)
  method: string                   // Method name (e.g., "resources/list")
  params?: object                  // Method parameters
}

/**
 * MCP Response Format (JSON-RPC 2.0)
 */
interface MCPResponse {
  jsonrpc: '2.0'
  id: string | number              // Matches request ID
  result?: unknown                 // Success result
  error?: {                        // Error (if failed)
    code: number
    message: string
    data?: unknown
  }
}

/**
 * MCP Notification (no response expected)
 */
interface MCPNotification {
  jsonrpc: '2.0'
  method: string
  params?: object
  // No 'id' field (one-way message)
}
```

---

### Standard MCP Methods
```typescript
/**
 * MCP Standard Methods
 * All MCP servers must implement these
 */
enum MCPMethod {
  // SERVER LIFECYCLE
  Initialize = 'initialize',         // Initialize connection
  Shutdown = 'shutdown',            // Clean shutdown
  
  // RESOURCES (Data Sources)
  ResourcesList = 'resources/list',        // List available resources
  ResourcesRead = 'resources/read',        // Read resource content
  ResourcesSubscribe = 'resources/subscribe',  // Watch for changes
  ResourcesUnsubscribe = 'resources/unsubscribe',
  
  // TOOLS (Actions)
  ToolsList = 'tools/list',         // List available tools
  ToolsCall = 'tools/call',         // Execute a tool
  
  // PROMPTS (Templates)
  PromptsList = 'prompts/list',     // List available prompts
  PromptsGet = 'prompts/get',       // Get prompt template
  
  // LOGGING
  LoggingSetLevel = 'logging/setLevel'  // Configure log verbosity
}
```

---

## 🔌 MCP CLIENT IMPLEMENTATION

### MVCA's MCP Client
```typescript
/**
 * MCP Client
 * MVCA's implementation for connecting to MCP servers
 */
class MCPClient {
  private servers: Map<string, MCPServerConnection> = new Map()
  private activeConnections: Map<string, Transport> = new Map()
  
  /**
   * Connect to MCP server
   */
  async connect(config: MCPServerConfig): Promise<void> {
    const transport = await this.createTransport(config)
    
    // Send initialize request
    const initResponse = await this.sendRequest(transport, {
      jsonrpc: '2.0',
      id: this.generateId(),
      method: 'initialize',
      params: {
        protocolVersion: '1.0',
        capabilities: {
          resources: true,
          tools: true,
          prompts: true
        },
        clientInfo: {
          name: 'MVCA',
          version: '2.0.0'
        }
      }
    })
    
    if (initResponse.error) {
      throw new Error(`Failed to initialize: ${initResponse.error.message}`)
    }
    
    // Store connection
    this.activeConnections.set(config.name, transport)
    this.servers.set(config.name, {
      config,
      capabilities: initResponse.result.capabilities,
      serverInfo: initResponse.result.serverInfo
    })
    
    console.log(`✓ Connected to MCP server: ${config.name}`)
  }
  
  /**
   * Disconnect from MCP server
   */
  async disconnect(serverName: string): Promise<void> {
    const transport = this.activeConnections.get(serverName)
    if (!transport) return
    
    // Send shutdown request
    await this.sendRequest(transport, {
      jsonrpc: '2.0',
      id: this.generateId(),
      method: 'shutdown'
    })
    
    // Close connection
    await transport.close()
    
    this.activeConnections.delete(serverName)
    this.servers.delete(serverName)
    
    console.log(`✓ Disconnected from MCP server: ${serverName}`)
  }
  
  /**
   * List available resources from server
   */
  async listResources(serverName: string): Promise<MCPResource[]> {
    const transport = this.activeConnections.get(serverName)
    if (!transport) throw new Error(`Server not connected: ${serverName}`)
    
    const response = await this.sendRequest(transport, {
      jsonrpc: '2.0',
      id: this.generateId(),
      method: 'resources/list'
    })
    
    if (response.error) {
      throw new Error(`Failed to list resources: ${response.error.message}`)
    }
    
    return response.result.resources
  }
  
  /**
   * Read resource content
   */
  async readResource(
    serverName: string,
    resourceUri: string
  ): Promise<{ content: string; mimeType?: string }> {
    const transport = this.activeConnections.get(serverName)
    if (!transport) throw new Error(`Server not connected: ${serverName}`)
    
    const response = await this.sendRequest(transport, {
      jsonrpc: '2.0',
      id: this.generateId(),
      method: 'resources/read',
      params: { uri: resourceUri }
    })
    
    if (response.error) {
      throw new Error(`Failed to read resource: ${response.error.message}`)
    }
    
    return {
      content: response.result.contents[0].text || response.result.contents[0].blob,
      mimeType: response.result.contents[0].mimeType
    }
  }
  
  /**
   * List available tools from server
   */
  async listTools(serverName: string): Promise<MCPTool[]> {
    const transport = this.activeConnections.get(serverName)
    if (!transport) throw new Error(`Server not connected: ${serverName}`)
    
    const response = await this.sendRequest(transport, {
      jsonrpc: '2.0',
      id: this.generateId(),
      method: 'tools/list'
    })
    
    if (response.error) {
      throw new Error(`Failed to list tools: ${response.error.message}`)
    }
    
    return response.result.tools
  }
  
  /**
   * Call tool on server
   */
  async callTool(
    serverName: string,
    toolName: string,
    parameters: Record<string, unknown>
  ): Promise<unknown> {
    const transport = this.activeConnections.get(serverName)
    if (!transport) throw new Error(`Server not connected: ${serverName}`)
    
    const response = await this.sendRequest(transport, {
      jsonrpc: '2.0',
      id: this.generateId(),
      method: 'tools/call',
      params: {
        name: toolName,
        arguments: parameters
      }
    })
    
    if (response.error) {
      throw new Error(`Tool execution failed: ${response.error.message}`)
    }
    
    return response.result
  }
  
  /**
   * Create transport (stdio or HTTP)
   */
  private async createTransport(config: MCPServerConfig): Promise<Transport> {
    if (config.transport === 'stdio') {
      return new StdioTransport(config)
    } else {
      return new HTTPTransport(config)
    }
  }
  
  /**
   * Send request and wait for response
   */
  private async sendRequest(
    transport: Transport,
    request: MCPRequest
  ): Promise<MCPResponse> {
    return await transport.send(request)
  }
  
  private generateId(): string {
    return `${Date.now()}-${Math.random().toString(36).substr(2, 9)}`
  }
}
```

---

### Transport Implementations

#### Stdio Transport (Local Servers)
```typescript
/**
 * Stdio Transport
 * Communicates with local MCP servers via stdin/stdout
 */
class StdioTransport implements Transport {
  private process: ChildProcess
  private pendingRequests: Map<string | number, {
    resolve: (value: MCPResponse) => void
    reject: (error: Error) => void
  }> = new Map()
  
  constructor(private config: MCPServerConfig) {}
  
  async connect(): Promise<void> {
    // Spawn MCP server process
    this.process = spawn(this.config.command, this.config.args || [], {
      stdio: ['pipe', 'pipe', 'pipe'],
      env: {
        ...process.env,
        ...this.config.env
      }
    })
    
    // Handle stdout (responses from server)
    this.process.stdout.on('data', (data) => {
      const lines = data.toString().split('\n').filter(Boolean)
      
      for (const line of lines) {
        try {
          const response: MCPResponse = JSON.parse(line)
          
          // Match response to pending request
          const pending = this.pendingRequests.get(response.id)
          if (pending) {
            pending.resolve(response)
            this.pendingRequests.delete(response.id)
          }
        } catch (error) {
          console.error('Failed to parse MCP response:', error)
        }
      }
    })
    
    // Handle stderr (logs from server)
    this.process.stderr.on('data', (data) => {
      console.error(`[MCP Server ${this.config.name}]:`, data.toString())
    })
    
    // Handle process exit
    this.process.on('exit', (code) => {
      console.log(`MCP server ${this.config.name} exited with code ${code}`)
      
      // Reject all pending requests
      for (const [id, pending] of this.pendingRequests) {
        pending.reject(new Error('Server disconnected'))
      }
      this.pendingRequests.clear()
    })
  }
  
  async send(request: MCPRequest): Promise<MCPResponse> {
    return new Promise((resolve, reject) => {
      // Store pending request
      this.pendingRequests.set(request.id, { resolve, reject })
      
      // Send request to server stdin
      this.process.stdin.write(JSON.stringify(request) + '\n')
      
      // Timeout after 30 seconds
      setTimeout(() => {
        if (this.pendingRequests.has(request.id)) {
          this.pendingRequests.delete(request.id)
          reject(new Error('Request timeout'))
        }
      }, 30000)
    })
  }
  
  async close(): Promise<void> {
    this.process.kill('SIGTERM')
    
    // Wait for process to exit
    await new Promise<void>((resolve) => {
      this.process.on('exit', () => resolve())
      
      // Force kill after 5 seconds
      setTimeout(() => {
        this.process.kill('SIGKILL')
        resolve()
      }, 5000)
    })
  }
}
```

#### HTTP Transport (Remote Servers)
```typescript
/**
 * HTTP Transport
 * Communicates with remote MCP servers via HTTP
 */
class HTTPTransport implements Transport {
  private baseUrl: string
  private headers: Record<string, string>
  
  constructor(private config: MCPServerConfig) {
    this.baseUrl = config.url!
    this.headers = {
      'Content-Type': 'application/json',
      ...config.headers
    }
  }
  
  async connect(): Promise<void> {
    // HTTP transport doesn't need explicit connection
    // Just verify server is reachable
    try {
      await fetch(this.baseUrl, {
        method: 'HEAD',
        headers: this.headers
      })
    } catch (error) {
      throw new Error(`Cannot reach MCP server at ${this.baseUrl}`)
    }
  }
  
  async send(request: MCPRequest): Promise<MCPResponse> {
    try {
      const response = await fetch(this.baseUrl, {
        method: 'POST',
        headers: this.headers,
        body: JSON.stringify(request)
      })
      
      if (!response.ok) {
        throw new Error(`HTTP ${response.status}: ${response.statusText}`)
      }
      
      const data: MCPResponse = await response.json()
      return data
      
    } catch (error) {
      throw new Error(`HTTP request failed: ${error.message}`)
    }
  }
  
  async close(): Promise<void> {
    // HTTP transport doesn't need explicit cleanup
  }
}
```

---

## 🗂️ MCP SERVER TYPES

### 1. Filesystem Server

**Purpose:** Provides access to local files and directories

**Capabilities:**
- Resources: File contents
- Tools: Create/update/delete files

**Example Configuration:**
```json
{
  "name": "filesystem",
  "transport": "stdio",
  "command": "npx",
  "args": ["-y", "@modelcontextprotocol/server-filesystem", "/path/to/workspace"],
  "description": "Access local files and directories"
}
```

**Usage:**
```typescript
// List files
const resources = await mcpClient.listResources('filesystem')
// Returns: [
//   { uri: "file:///path/to/workspace/README.md", name: "README.md" },
//   { uri: "file:///path/to/workspace/package.json", name: "package.json" }
// ]

// Read file
const content = await mcpClient.readResource('filesystem', 'file:///path/to/workspace/README.md')
// Returns: { content: "# Project Name\n...", mimeType: "text/markdown" }

// Create file (via tool)
await mcpClient.callTool('filesystem', 'write_file', {
  path: '/path/to/workspace/new-file.txt',
  content: 'Hello World'
})
```

---

### 2. Database Server

**Purpose:** Query and manage databases (PostgreSQL, MySQL, MongoDB)

**Capabilities:**
- Resources: Database schemas, table contents
- Tools: Execute queries, run migrations

**Example Configuration:**
```json
{
  "name": "postgres-db",
  "transport": "stdio",
  "command": "npx",
  "args": ["-y", "@modelcontextprotocol/server-postgres"],
  "env": {
    "DATABASE_URL": "postgresql://user:pass@localhost:5432/mydb"
  },
  "description": "PostgreSQL database access"
}
```

**Usage:**
```typescript
// List tables
const resources = await mcpClient.listResources('postgres-db')
// Returns: [
//   { uri: "postgres://mydb/users", name: "users table" },
//   { uri: "postgres://mydb/posts", name: "posts table" }
// ]

// Read table schema
const schema = await mcpClient.readResource('postgres-db', 'postgres://mydb/users')
// Returns: { content: "CREATE TABLE users (id SERIAL, email TEXT, ...)", mimeType: "text/sql" }

// Execute query (via tool)
const result = await mcpClient.callTool('postgres-db', 'query', {
  sql: 'SELECT COUNT(*) FROM users WHERE created_at > $1',
  params: ['2024-01-01']
})
// Returns: { rows: [{ count: 47 }] }
```

---

### 3. GitHub Server

**Purpose:** Interact with GitHub repositories (read files, create PRs, manage issues)

**Capabilities:**
- Resources: Repository files, issues, PRs
- Tools: Create issues, create PRs, comment

**Example Configuration:**
```json
{
  "name": "github",
  "transport": "stdio",
  "command": "npx",
  "args": ["-y", "@modelcontextprotocol/server-github"],
  "env": {
    "GITHUB_TOKEN": "ghp_xxx",
    "GITHUB_OWNER": "your-org",
    "GITHUB_REPO": "your-repo"
  },
  "description": "GitHub repository access"
}
```

**Usage:**
```typescript
// List repository files
const resources = await mcpClient.listResources('github')
// Returns: [
//   { uri: "github://your-org/your-repo/README.md", name: "README.md" },
//   { uri: "github://your-org/your-repo/package.json", name: "package.json" }
// ]

// Read file from GitHub
const content = await mcpClient.readResource('github', 'github://your-org/your-repo/README.md')
// Returns: { content: "# Your Repo\n...", mimeType: "text/markdown" }

// Create issue (via tool)
await mcpClient.callTool('github', 'create_issue', {
  title: 'Bug: Login button not working',
  body: 'Steps to reproduce:\n1. ...',
  labels: ['bug', 'priority-high']
})

// Create pull request (via tool)
await mcpClient.callTool('github', 'create_pull_request', {
  title: 'Fix: Login button click handler',
  body: 'This PR fixes the login button issue',
  head: 'feature/fix-login',
  base: 'main'
})
```

---

### 4. Google Drive Server

**Purpose:** Access files stored in Google Drive

**Capabilities:**
- Resources: Drive files, folders
- Tools: Upload, download, share files

**Example Configuration:**
```json
{
  "name": "google-drive",
  "transport": "stdio",
  "command": "npx",
  "args": ["-y", "@modelcontextprotocol/server-google-drive"],
  "env": {
    "GOOGLE_CLIENT_ID": "xxx.apps.googleusercontent.com",
    "GOOGLE_CLIENT_SECRET": "xxx",
    "GOOGLE_REFRESH_TOKEN": "xxx"
  },
  "description": "Google Drive file access"
}
```

**Usage:**
```typescript
// List files in Drive
const resources = await mcpClient.listResources('google-drive')
// Returns: [
//   { uri: "gdrive://1abc/Q3-Report.docx", name: "Q3 Report" },
//   { uri: "gdrive://2def/Budget.xlsx", name: "Budget 2024" }
// ]

// Read file from Drive
const content = await mcpClient.readResource('google-drive', 'gdrive://1abc/Q3-Report.docx')
// Returns: { content: "...", mimeType: "application/vnd.openxmlformats-officedocument.wordprocessingml.document" }

// Upload file (via tool)
await mcpClient.callTool('google-drive', 'upload', {
  name: 'New Document.docx',
  content: base64Content,
  mimeType: 'application/vnd.openxmlformats-officedocument.wordprocessingml.document'
})
```

---

### 5. Slack Server

**Purpose:** Read and send Slack messages, manage channels

**Capabilities:**
- Resources: Channel messages, user info
- Tools: Send messages, create channels

**Example Configuration:**
```json
{
  "name": "slack",
  "transport": "stdio",
  "command": "npx",
  "args": ["-y", "@modelcontextprotocol/server-slack"],
  "env": {
    "SLACK_BOT_TOKEN": "xoxb-xxx",
    "SLACK_WORKSPACE": "your-workspace"
  },
  "description": "Slack workspace access"
}
```

**Usage:**
```typescript
// List channels
const resources = await mcpClient.listResources('slack')
// Returns: [
//   { uri: "slack://your-workspace/general", name: "#general" },
//   { uri: "slack://your-workspace/engineering", name: "#engineering" }
// ]

// Read recent messages
const messages = await mcpClient.readResource('slack', 'slack://your-workspace/engineering')
// Returns: { content: "User1: Hey\nUser2: Hi there\n...", mimeType: "text/plain" }

// Send message (via tool)
await mcpClient.callTool('slack', 'send_message', {
  channel: '#engineering',
  text: 'Deployment complete! 🚀',
  thread_ts: '1234567890.123456'  // Optional: reply to thread
})
```

---

### 6. Custom MCP Server

**Purpose:** Build your own MCP server for any data source or service

**Example: Weather API Server**
```typescript
/**
 * Custom MCP Server: Weather API
 * Provides weather data from OpenWeatherMap
 */
import { Server } from '@modelcontextprotocol/sdk/server/index.js'
import { StdioServerTransport } from '@modelcontextprotocol/sdk/server/stdio.js'

const server = new Server({
  name: 'weather-api',
  version: '1.0.0'
}, {
  capabilities: {
    resources: true,
    tools: true
  }
})

// Register resources (current weather for cities)
server.setRequestHandler('resources/list', async () => {
  return {
    resources: [
      {
        uri: 'weather://current/new-york',
        name: 'New York Current Weather',
        description: 'Current weather conditions in New York City',
        mimeType: 'application/json'
      },
      {
        uri: 'weather://current/london',
        name: 'London Current Weather',
        description: 'Current weather conditions in London',
        mimeType: 'application/json'
      }
    ]
  }
})

// Handle resource reads
server.setRequestHandler('resources/read', async (request) => {
  const uri = request.params.uri as string
  const city = uri.split('/').pop()
  
  // Fetch from OpenWeatherMap API
  const response = await fetch(
    `https://api.openweathermap.org/data/2.5/weather?q=${city}&appid=${process.env.OPENWEATHER_API_KEY}&units=metric`
  )
  const data = await response.json()
  
  return {
    contents: [{
      uri: uri,
      mimeType: 'application/json',
      text: JSON.stringify({
        city: data.name,
        temperature: data.main.temp,
        conditions: data.weather[0].description,
        humidity: data.main.humidity,
        windSpeed: data.wind.speed
      }, null, 2)
    }]
  }
})

// Register tools (forecast lookup)
server.setRequestHandler('tools/list', async () => {
  return {
    tools: [{
      name: 'get_forecast',
      description: 'Get 5-day weather forecast for a city',
      inputSchema: {
        type: 'object',
        properties: {
          city: {
            type: 'string',
            description: 'City name (e.g., "New York", "London")'
          },
          days: {
            type: 'number',
            description: 'Number of days (1-5)',
            minimum: 1,
            maximum: 5,
            default: 5
          }
        },
        required: ['city']
      }
    }]
  }
})

// Handle tool calls
server.setRequestHandler('tools/call', async (request) => {
  if (request.params.name === 'get_forecast') {
    const { city, days = 5 } = request.params.arguments as { city: string; days?: number }
    
    const response = await fetch(
      `https://api.openweathermap.org/data/2.5/forecast?q=${city}&appid=${process.env.OPENWEATHER_API_KEY}&units=metric&cnt=${days * 8}`
    )
    const data = await response.json()
    
    // Process forecast data
    const forecast = data.list.slice(0, days * 8).map((item: any) => ({
      date: new Date(item.dt * 1000).toLocaleDateString(),
      temp: item.main.temp,
      conditions: item.weather[0].description
    }))
    
    return {
      content: [{
        type: 'text',
        text: JSON.stringify(forecast, null, 2)
      }]
    }
  }
  
  throw new Error('Unknown tool')
})

// Start server
const transport = new StdioServerTransport()
await server.connect(transport)
```

**Configuration:**
```json
{
  "name": "weather",
  "transport": "stdio",
  "command": "node",
  "args": ["./weather-server.js"],
  "env": {
    "OPENWEATHER_API_KEY": "your-api-key"
  },
  "description": "Weather data from OpenWeatherMap"
}
```

**Usage:**
```typescript
// Get current weather
const weather = await mcpClient.readResource('weather', 'weather://current/new-york')
// Returns: { content: "{ temperature: 18, conditions: 'partly cloudy', ... }" }

// Get 5-day forecast
const forecast = await mcpClient.callTool('weather', 'get_forecast', {
  city: 'London',
  days: 5
})
// Returns: [{ date: '2024-02-07', temp: 12, conditions: 'rainy' }, ...]
```

---

## 🛡️ MCP SECURITY MODEL

### Security Boundaries
```typescript
/**
 * MCP Security Sandbox
 * Isolates MCP servers from system
 */
class MCPSecuritySandbox {
  /**
   * Validate MCP server configuration before connecting
   */
  validateServerConfig(config: MCPServerConfig): ValidationResult {
    const errors: string[] = []
    
    // Check command whitelist (stdio transport)
    if (config.transport === 'stdio') {
      const allowedCommands = ['node', 'npx', 'python', 'python3']
      const command = config.command
      
      if (!allowedCommands.includes(command)) {
        errors.push(`Command not allowed: ${command}. Allowed: ${allowedCommands.join(', ')}`)
      }
      
      // Check for dangerous flags
      const dangerousFlags = ['--allow-run', '--allow-write', '--allow-net']
      const args = config.args?.join(' ') || ''
      
      for (const flag of dangerousFlags) {
        if (args.includes(flag)) {
          errors.push(`Dangerous flag detected: ${flag}`)
        }
      }
    }
    
    // Check URL whitelist (HTTP transport)
    if (config.transport === 'http') {
      const url = new URL(config.url!)
      
      // Only allow HTTPS in production
      if (process.env.NODE_ENV === 'production' && url.protocol !== 'https:') {
        errors.push('HTTPS required in production')
      }
      
      // Check domain whitelist
      const allowedDomains = process.env.MCP_ALLOWED_DOMAINS?.split(',') || []
      if (allowedDomains.length > 0 && !allowedDomains.includes(url.hostname)) {
        errors.push(`Domain not allowed: ${url.hostname}`)
      }
    }
    
    // Check environment variables (no sensitive data)
    if (config.env) {
      for (const [key, value] of Object.entries(config.env)) {
        if (value.length > 10000) {
          errors.push(`Environment variable too large: ${key}`)
        }
      }
    }
    
    return {
      valid: errors.length === 0,
      errors
    }
  }
  
  /**
   * Sanitize resource URI before access
   */
  sanitizeResourceUri(uri: string): string {
    // Remove dangerous path traversal
    const sanitized = uri.replace(/\.\./g, '')
    
    // Ensure no file:// access outside workspace
    if (uri.startsWith('file://')) {
      const workspaceRoot = process.env.WORKSPACE_ROOT || process.cwd()
      const resourcePath = uri.replace('file://', '')
      
      if (!resourcePath.startsWith(workspaceRoot)) {
        throw new Error('Access denied: Resource outside workspace')
      }
    }
    
    return sanitized
  }
  
  /**
   * Validate tool parameters before execution
   */
  validateToolParameters(
    tool: MCPTool,
    parameters: Record<string, unknown>
  ): ValidationResult {
    // Use JSON Schema validation
    const ajv = new Ajv()
    const validate = ajv.compile(tool.inputSchema)
    
    if (!validate(parameters)) {
      return {
        valid: false,
        errors: validate.errors?.map(e => e.message || 'Invalid') || ['Validation failed']
      }
    }
    
    return { valid: true, errors: [] }
  }
}
```

---

### Resource Access Control
```typescript
/**
 * MCP Resource Access Policy
 * Controls which resources can be accessed
 */
class MCPResourcePolicy {
  private policies: Map<string, ResourcePolicy> = new Map()
  
  /**
   * Set access policy for resource type
   */
  setPolicy(resourceType: string, policy: ResourcePolicy): void {
    this.policies.set(resourceType, policy)
  }
  
  /**
   * Check if resource access is allowed
   */
  canAccess(
    resource: MCPResource,
    userContext: UserContext
  ): boolean {
    const resourceType = this.extractResourceType(resource.uri)
    const policy = this.policies.get(resourceType)
    
    if (!policy) {
      // No policy = deny by default
      return false
    }
    
    // Check user permissions
    if (policy.requiredPermission) {
      if (!userContext.permissions.includes(policy.requiredPermission)) {
        return false
      }
    }
    
    // Check resource-specific rules
    if (policy.allowedPatterns) {
      const matches = policy.allowedPatterns.some(pattern =>
        new RegExp(pattern).test(resource.uri)
      )
      if (!matches) return false
    }
    
    if (policy.deniedPatterns) {
      const matches = policy.deniedPatterns.some(pattern =>
        new RegExp(pattern).test(resource.uri)
      )
      if (matches) return false
    }
    
    return true
  }
  
  private extractResourceType(uri: string): string {
    const protocol = uri.split('://')[0]
    return protocol  // e.g., "file", "postgres", "github"
  }
}

/**
 * Example: Filesystem resource policy
 */
const filesystemPolicy: ResourcePolicy = {
  requiredPermission: 'read:files',
  allowedPatterns: [
    '^file:///workspace/.*',  // Only files in /workspace
    '^file:///home/user/projects/.*'  // Or user's projects
  ],
  deniedPatterns: [
    '.*\\.env$',         // No .env files
    '.*\\.env\\..*',     // No .env.local, .env.production, etc.
    '.*private.*key.*',  // No private keys
    '.*\\.ssh/.*',       // No SSH keys
    '.*password.*'       // No password files
  ]
}
```

---

## 📊 MCP SERVER MANAGEMENT

### Server Configuration File
```json
// ~/.config/mvca/mcp-servers.json
{
  "servers": {
    "filesystem": {
      "enabled": true,
      "transport": "stdio",
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-filesystem", "/home/user/projects"],
      "description": "Local filesystem access",
      "autoStart": true
    },
    "postgres": {
      "enabled": true,
      "transport": "stdio",
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-postgres"],
      "env": {
        "DATABASE_URL": "${POSTGRES_URL}"
      },
      "description": "PostgreSQL database",
      "autoStart": true
    },
    "github": {
      "enabled": false,
      "transport": "stdio",
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-github"],
      "env": {
        "GITHUB_TOKEN": "${GITHUB_TOKEN}",
        "GITHUB_OWNER": "your-org",
        "GITHUB_REPO": "your-repo"
      },
      "description": "GitHub repository access",
      "autoStart": false
    },
    "weather": {
      "enabled": true,
      "transport": "stdio",
      "command": "node",
      "args": ["./mcp-servers/weather-server.js"],
      "env": {
        "OPENWEATHER_API_KEY": "${OPENWEATHER_API_KEY}"
      },
      "description": "Weather data API",
      "autoStart": true
    }
  },
  "security": {
    "allowedCommands": ["node", "npx", "python3"],
    "allowedDomains": ["api.openweathermap.org"],
    "resourcePolicies": {
      "file": {
        "allowedPaths": ["/home/user/projects"],
        "deniedPatterns": ["\\.env", "private.*key"]
      }
    }
  }
}
```

---

### MCP Server Manager CLI
```bash
# List available servers
mvca mcp list

# Output:
# ✓ filesystem (enabled, running)
#   Local filesystem access
#   Resources: 47 files
#
# ✓ postgres (enabled, running)
#   PostgreSQL database
#   Resources: 12 tables
#
# ○ github (disabled)
#   GitHub repository access
#
# ✓ weather (enabled, running)
#   Weather data API
#   Tools: get_forecast

# Enable server
mvca mcp enable github

# Start server
mvca mcp start github

# Stop server
mvca mcp stop filesystem

# View server logs
mvca mcp logs postgres

# Test server connection
mvca mcp test weather
# Output: ✓ Connection successful
#         ✓ Resources accessible (2)
#         ✓ Tools available (1)
```

---

## 📚 RELATED ARTICLES

| Article | Purpose | Relationship to MCP Integration |
|---------|---------|--------------------------------|
| **Article I: Tool Architecture** | Tool interface spec | MCP tools implement IToolInterface |
| **Article III: Core Tool Catalog** | Built-in tools | Some wrap MCP servers |
| **Article IV: Integration Tool Catalog** | Optional tools | Many use MCP servers |
| **Segment 4: Execution Layer** | Orchestration | Uses MCP resources in workflows |

---

**Previous:** [← Article I: Tool Architecture](./01-article-i-tool-architecture.md)  
**Next:** [Article III: Core Tool Catalog →](./03-article-iii-core-tool-catalog.md)

---

**Last Updated:** February 7, 2026  
**Constitutional Version:** 2.0.0  
**Status:** ✅ Ratified and In Force

**Motto:** *"MCP: The Universal Protocol for AI-to-Service Communication"*
