# Jarvis: Agentic AI Development Bible

A comprehensive knowledge base for AI-assisted web application development. This repository contains structured guidelines, best practices, and instructions for various frameworks, tools, and development processes.

## 📚 Table of Contents

### 🎯 Core Frameworks
- [Svelte Guide](./frameworks/svelte/README.md)
- [Angular Guide](./frameworks/angular/README.md)
- [React Guide](./frameworks/react/README.md)
- [Astro Guide](./frameworks/astro/README.md)
- [WordPress Guide](./frameworks/wordpress/README.md)

### 🤖 AI & Automation
- [Agent Development](./ai-agents/README.md)
- [MCP Tools Integration](./mcp-tools/README.md)
- [AI Prompt Engineering](./ai-agents/prompt-engineering.md)

### 🎨 UI Libraries & Styling
- [TailwindCSS Guide](./libraries/tailwindcss/README.md)
- [Bits-UI Guide](./libraries/bits-ui/README.md)
- [Component Libraries](./libraries/component-libraries/README.md)

### 💾 Data & Storage
- [Dexie Guide](./libraries/dexie/README.md)
- [Database Patterns](./data-storage/README.md)

### 🏗️ Development Standards
- [File Management](./standards/file-management.md)
- [Naming Conventions](./standards/naming-conventions.md)
- [Code Style Guide](./standards/code-style.md)
- [Git Workflow](./standards/git-workflow.md)

### 🏛️ Architecture & Patterns
- [Architecture Patterns](./architecture/README.md)
- [Component Architecture](./architecture/component-patterns.md)
- [State Management](./architecture/state-management.md)

### 🧪 Testing & Quality
- [Testing Strategies](./testing/README.md)
- [Testing Frameworks](./testing/frameworks.md)
- [Quality Assurance](./testing/quality-assurance.md)

### 📖 Documentation
- [Documentation Standards](./documentation/README.md)
- [API Documentation](./documentation/api-docs.md)
- [Component Documentation](./documentation/component-docs.md)

### 🛠️ Tools & Workflows
- [VS Code Setup](./tools/vscode-setup.md)
- [Cursor Integration](./tools/cursor-integration.md)
- [GitHub Workflows](./tools/github-workflows.md)
- [Build & Deployment](./tools/build-deployment.md)

## 🚀 Quick Start

### For Jarvis Development
1. **Choose Your Framework**: Navigate to the appropriate framework guide
2. **Set Up AI Tools**: Review the AI agents and MCP tools sections
3. **Follow Standards**: Implement the development standards for your project
4. **Use Templates**: Leverage the provided templates and examples

### For Projects Using Jarvis Instructions
1. **Copy Configuration**: Copy `jarvis-config.json` to your project root
2. **Customize References**: Edit the `extends` section to reference the Jarvis instructions you need
3. **Add Project Rules**: Add your project-specific rules in the `custom` section
4. **Configure Cursor**: Update your project's `.cursor.json` to reference Jarvis instructions

## 📋 Usage Guidelines

- Each section contains specific instructions for AI agents
- Templates are provided for common patterns
- Best practices are highlighted with ⭐
- Common pitfalls are marked with ⚠️
- Performance tips are marked with ⚡

## 🔄 Contributing

This knowledge base is designed to evolve. When you discover new patterns or best practices:

1. Document them in the appropriate section
2. Update related sections if needed
3. Test with AI agents to ensure clarity
4. Share improvements with the community

## 🔗 Integrating Jarvis with Your Projects

### Using Jarvis Instructions in Other Projects

Jarvis is designed to be a reusable knowledge base that can be referenced by any development project. Here's how to integrate it:

#### 1. Copy the Configuration Template
```bash
cp /path/to/jarvis/jarvis-config.json ./jarvis-config.json
```

#### 2. Customize Your Project's Configuration
Edit `jarvis-config.json` to reference only the Jarvis instructions you need:

```json
{
  "extends": {
    "framework": "svelte",
    "standards": ["code-style", "naming", "file-management"],
    "tools": ["cursor"],
    "libraries": ["tailwindcss", "bits-ui"]
  },
  "custom": {
    "project-specific-rules": [
      "Focus on meal planning and recipe management",
      "Use TypeScript for type safety throughout"
    ]
  }
}
```

#### 3. Update Your Project's .cursor.json
Reference the Jarvis instructions in your project's `.cursor.json`:

```json
{
  "rules": [
    "You are working on a SvelteKit web application for meal planning.",
    "Follow the Jarvis instructions referenced in jarvis-config.json",
    "Apply project-specific rules from the custom section"
  ],
  "extends": "./jarvis-config.json"
}
```

#### 4. Override Specific Instructions (Optional)
If you need to override specific Jarvis instructions for your project:

```json
{
  "custom": {
    "overrides": {
      "standards.code-style": "Use 4 spaces instead of 2 for indentation in this project"
    }
  }
}
```

### Available Instruction Categories

- **frameworks**: svelte, react, angular, astro, wordpress
- **standards**: code-style, naming, file-management, git-workflow
- **tools**: cursor, vscode
- **libraries**: tailwindcss, bits-ui, dexie, component-libraries
- **ai-agents**: prompt-engineering, svelte-agents
- **architecture**: patterns
- **testing**: strategies
- **documentation**: standards
- **data-storage**: patterns
- **mcp-tools**: integration

## 🎯 AI Agent Instructions

When working with this knowledge base:

1. **Always reference the relevant section** before making recommendations
2. **Follow the established patterns** unless there's a compelling reason to deviate
3. **Use the provided templates** as starting points
4. **Apply the naming conventions** consistently
5. **Consider the architecture patterns** for larger decisions

---

*This knowledge base is designed to work seamlessly with AI agents, providing clear, actionable guidance for web application development.*
