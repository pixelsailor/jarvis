# Cursor Integration Guide

## 🎯 AI Agent Instructions for Cursor

When working with Cursor in web development projects, follow these guidelines to ensure optimal AI-assisted development experience.

## 🔧 Cursor Configuration

### AI Settings
```json
{
  "cursor.ai.enabled": true,
  "cursor.ai.model": "gpt-4",
  "cursor.ai.temperature": 0.7,
  "cursor.ai.maxTokens": 2000,
  "cursor.ai.contextWindow": 8000
}
```

### Code Generation Settings
```json
{
  "cursor.codeGeneration.enabled": true,
  "cursor.codeGeneration.includeComments": true,
  "cursor.codeGeneration.includeTests": true,
  "cursor.codeGeneration.followPatterns": true
}
```

### Context Settings
```json
{
  "cursor.context.includeImports": true,
  "cursor.context.includeTypes": true,
  "cursor.context.includeTests": true,
  "cursor.context.maxFiles": 10
}
```

## 🔌 MCP Server Configuration

### Understanding MCP Servers
Model Context Protocol (MCP) servers extend Cursor's capabilities by providing specialized tools and context for specific frameworks, languages, or development tasks.

### Important: Global Configuration Required
**MCP servers are NOT automatically recognized within project directories.** They must be explicitly configured in Cursor's global settings.

### Setup Process

#### 1. Locate Cursor's Configuration File
The MCP server configuration is stored in Cursor's global configuration file:
- **macOS**: `~/Library/Application Support/Cursor/User/cursor_desktop_config.json`
- **Windows**: `%APPDATA%\Cursor\User\cursor_desktop_config.json`
- **Linux**: `~/.config/Cursor/User/cursor_desktop_config.json`

#### 2. Add MCP Server Configuration
Add your MCP server to the configuration file:

```json
{
  "mcpServers": {
    "svelte-mcp": {
      "command": "npx",
      "args": ["-y", "@sveltejs/mcp"],
      "env": {}
    },
    "filesystem-mcp": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-filesystem", "/path/to/allowed/directory"],
      "env": {}
    }
  }
}
```

#### 3. Restart Cursor
After updating the configuration, restart Cursor completely for the changes to take effect.

### Available MCP Servers

#### Svelte MCP Server
Provides comprehensive Svelte 5 and SvelteKit documentation and tools:
```json
{
  "svelte-mcp": {
    "command": "npx",
    "args": ["-y", "@sveltejs/mcp"],
    "env": {}
  }
}
```

**Available Tools:**
- `list-sections` - Discover available documentation sections
- `get-documentation` - Retrieve specific documentation content
- `svelte-autofixer` - Analyze and fix Svelte code issues
- `playground-link` - Generate Svelte Playground links

#### Filesystem MCP Server
Provides file system access for AI agents:
```json
{
  "filesystem-mcp": {
    "command": "npx",
    "args": ["-y", "@modelcontextprotocol/server-filesystem", "/path/to/project"],
    "env": {}
  }
}
```

### Best Practices for MCP Server Usage

1. **Use Project-Specific Directories**: Configure filesystem MCP servers with specific project directories for security
2. **Test After Configuration**: Verify MCP servers are working by checking available tools in Cursor
3. **Keep Servers Updated**: Regularly update MCP server packages to get latest features
4. **Document Your Setup**: Keep track of which MCP servers you've configured for different projects

### Troubleshooting

- **Server Not Appearing**: Ensure the configuration file is in the correct location and restart Cursor
- **Permission Errors**: Check that the MCP server has appropriate permissions for the directories it needs to access
- **Command Not Found**: Verify that the MCP server package is available via npm/npx

## 🎯 AI Agent Best Practices

When assisting with Cursor integration:

1. **Configure AI settings** for optimal performance
2. **Set up code generation** preferences
3. **Configure context** for better AI understanding
4. **Set up proper prompts** for different tasks
5. **Configure debugging** with AI assistance
6. **Set up proper file associations** for AI context
7. **Configure Git integration** with AI
8. **Set up proper workspace settings** for AI
9. **Configure debugging** with AI assistance
10. **Set up proper extensions** for AI workflow
11. **Configure MCP servers** for enhanced AI capabilities
