# VS Code Setup Guide

## 🎯 AI Agent Instructions for VS Code

When working with VS Code in web development projects, follow these guidelines to ensure optimal development experience and productivity.

## 🔧 Essential Extensions

### Core Extensions
```json
{
  "recommendations": [
    "ms-vscode.vscode-typescript-next",
    "bradlc.vscode-tailwindcss",
    "esbenp.prettier-vscode",
    "dbaeumer.vscode-eslint",
    "ms-vscode.vscode-json",
    "redhat.vscode-yaml",
    "ms-vscode.vscode-markdown"
  ]
}
```

### Framework-Specific Extensions
```json
{
  "recommendations": [
    "ms-vscode.vscode-react-snippets",
    "ms-vscode.vscode-vue",
    "angular.ng-template",
    "svelte.svelte-vscode",
    "astro-build.astro-vscode"
  ]
}
```

### Productivity Extensions
```json
{
  "recommendations": [
    "ms-vscode.vscode-git",
    "ms-vscode.vscode-github",
    "ms-vscode.vscode-docker",
    "ms-vscode.vscode-kubernetes",
    "ms-vscode.vscode-azure"
  ]
}
```

## ⚙️ Workspace Configuration

### settings.json
```json
{
  "editor.formatOnSave": true,
  "editor.codeActionsOnSave": {
    "source.fixAll.eslint": true,
    "source.organizeImports": true
  },
  "editor.tabSize": 2,
  "editor.insertSpaces": true,
  "editor.rulers": [80, 100],
  "files.trimTrailingWhitespace": true,
  "files.insertFinalNewline": true,
  "typescript.preferences.importModuleSpecifier": "relative",
  "typescript.suggest.autoImports": true,
  "emmet.includeLanguages": {
    "javascript": "javascriptreact",
    "typescript": "typescriptreact"
  }
}
```

### tasks.json
```json
{
  "version": "2.0.0",
  "tasks": [
    {
      "label": "npm: dev",
      "type": "shell",
      "command": "npm run dev",
      "group": "build",
      "presentation": {
        "echo": true,
        "reveal": "always",
        "focus": false,
        "panel": "shared"
      }
    },
    {
      "label": "npm: build",
      "type": "shell",
      "command": "npm run build",
      "group": "build"
    },
    {
      "label": "npm: test",
      "type": "shell",
      "command": "npm test",
      "group": "test"
    }
  ]
}
```

### launch.json
```json
{
  "version": "0.2.0",
  "configurations": [
    {
      "name": "Launch Chrome",
      "type": "chrome",
      "request": "launch",
      "url": "http://localhost:3000",
      "webRoot": "${workspaceFolder}/src"
    },
    {
      "name": "Attach to Chrome",
      "type": "chrome",
      "request": "attach",
      "port": 9222,
      "webRoot": "${workspaceFolder}/src"
    }
  ]
}
```

## 🎯 AI Agent Best Practices

When assisting with VS Code setup:

1. **Configure essential extensions** for the technology stack
2. **Set up proper formatting** and linting rules
3. **Configure debugging** for the development environment
4. **Set up tasks** for common development operations
5. **Configure IntelliSense** for better code completion
6. **Set up proper file associations** for different file types
7. **Configure Git integration** for version control
8. **Set up proper workspace settings** for the project
9. **Configure debugging** for the specific framework
10. **Set up proper extensions** for the development workflow
