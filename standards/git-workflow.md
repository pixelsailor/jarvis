# Git Workflow Guide

## 🎯 AI Agent Instructions for Git Workflow

When working with Git in web development projects, follow these guidelines to ensure optimal version control, collaboration, and code quality.

## 🌿 Branch Naming Conventions

### Branch Types
```
# Feature branches
feature/user-authentication     # New features
feature/payment-integration     # New features
feature/dashboard-redesign      # New features

# Bug fix branches
bugfix/login-error-handling     # Bug fixes
bugfix/memory-leak-fix          # Bug fixes
bugfix/validation-error         # Bug fixes

# Hotfix branches
hotfix/security-patch           # Critical fixes
hotfix/production-bug           # Critical fixes
hotfix/performance-issue        # Critical fixes

# Release branches
release/v1.2.0                  # Release preparation
release/v2.0.0                  # Release preparation
release/v1.2.1                  # Release preparation

# Development branches
develop                         # Development integration
staging                         # Staging environment
main                            # Production-ready code
```

### Branch Naming Rules
1. **Use lowercase** with hyphens for separation
2. **Be descriptive** - clearly indicate the purpose
3. **Use prefixes** to categorize branch types
4. **Keep names concise** but meaningful
5. **Avoid special characters** except hyphens
6. **Use consistent patterns** across the project

## 📝 Commit Message Standards

### Commit Message Format
```
<type>(<scope>): <description>

[optional body]

[optional footer(s)]
```

### Commit Types
```
feat:     # New feature
fix:      # Bug fix
docs:     # Documentation changes
style:    # Code style changes (formatting, etc.)
refactor: # Code refactoring
test:     # Adding or updating tests
chore:    # Maintenance tasks
perf:     # Performance improvements
ci:       # CI/CD changes
build:    # Build system changes
```

### Commit Examples
```
# Feature commits
feat(auth): add OAuth2 integration
feat(ui): implement dark mode toggle
feat(api): add user profile endpoints

# Bug fix commits
fix(login): resolve authentication timeout issue
fix(validation): fix email validation regex
fix(ui): correct button alignment on mobile

# Documentation commits
docs(readme): update installation instructions
docs(api): add endpoint documentation
docs(components): update component examples

# Style commits
style(components): format code with prettier
style(css): fix indentation in stylesheets
style(js): remove unused imports

# Refactor commits
refactor(services): extract common API logic
refactor(components): simplify user profile component
refactor(utils): optimize date formatting functions

# Test commits
test(components): add unit tests for Button component
test(services): add integration tests for UserService
test(utils): add tests for validation functions

# Chore commits
chore(deps): update dependencies to latest versions
chore(config): update ESLint configuration
chore(scripts): add new build script

# Performance commits
perf(api): optimize database queries
perf(ui): implement virtual scrolling for large lists
perf(components): memoize expensive calculations

# CI/CD commits
ci(github): add automated testing workflow
ci(deploy): update deployment configuration
ci(build): optimize build process

# Build commits
build(webpack): update webpack configuration
build(docker): update Dockerfile for production
build(scripts): add new build optimization
```

### Commit Message Best Practices
1. **Use imperative mood** - "add feature" not "added feature"
2. **Keep first line under 50 characters**
3. **Capitalize the first letter** of the description
4. **No period at the end** of the description
5. **Use body for complex changes** - explain what and why
6. **Reference issues** in the footer when applicable
7. **Be consistent** with the established format

## 🔄 Git Workflow Strategies

### Git Flow Workflow
```
main                    # Production-ready code
├── develop            # Development integration
│   ├── feature/auth   # Feature branches
│   ├── feature/ui     # Feature branches
│   └── feature/api    # Feature branches
├── release/v1.2.0     # Release preparation
└── hotfix/security    # Critical fixes
```

### GitHub Flow Workflow
```
main                    # Production-ready code
├── feature/auth       # Feature branches
├── feature/ui         # Feature branches
└── feature/api        # Feature branches
```

### GitLab Flow Workflow
```
main                    # Production-ready code
├── develop            # Development integration
├── staging            # Staging environment
├── feature/auth       # Feature branches
└── feature/ui         # Feature branches
```

## 🚀 Branch Management

### Creating Branches
```bash
# Create and switch to new feature branch
git checkout -b feature/user-authentication

# Create branch from specific commit
git checkout -b feature/new-feature <commit-hash>

# Create branch from specific tag
git checkout -b feature/new-feature v1.2.0

# Create branch from remote branch
git checkout -b feature/new-feature origin/develop
```

### Branch Operations
```bash
# Switch to existing branch
git checkout feature/user-authentication

# Switch to previous branch
git checkout -

# List all branches
git branch -a

# List local branches
git branch

# List remote branches
git branch -r

# Delete local branch
git branch -d feature/user-authentication

# Delete remote branch
git push origin --delete feature/user-authentication

# Rename current branch
git branch -m new-branch-name

# Rename any branch
git branch -m old-branch-name new-branch-name
```

### Branch Synchronization
```bash
# Fetch latest changes from remote
git fetch origin

# Update local branch with remote changes
git pull origin main

# Rebase current branch on main
git rebase main

# Merge main into current branch
git merge main

# Update remote branch with local changes
git push origin feature/user-authentication

# Force push (use with caution)
git push --force-with-lease origin feature/user-authentication
```

## 🔀 Merge Strategies

### Merge Commits
```bash
# Create merge commit
git merge feature/user-authentication

# Create merge commit with no fast-forward
git merge --no-ff feature/user-authentication

# Create merge commit with custom message
git merge -m "Merge feature/user-authentication into main" feature/user-authentication
```

### Rebase Strategy
```bash
# Rebase current branch on main
git rebase main

# Interactive rebase for last 3 commits
git rebase -i HEAD~3

# Rebase and squash commits
git rebase -i main

# Continue rebase after resolving conflicts
git rebase --continue

# Abort rebase
git rebase --abort
```

### Squash and Merge
```bash
# Squash all commits into one
git rebase -i main

# In the interactive editor, change 'pick' to 'squash' for all commits except the first
# Then edit the commit message to combine all changes
```

## 🔧 Pull Request Workflow

### Pull Request Template
```markdown
## Description
Brief description of the changes made.

## Type of Change
- [ ] Bug fix (non-breaking change which fixes an issue)
- [ ] New feature (non-breaking change which adds functionality)
- [ ] Breaking change (fix or feature that would cause existing functionality to not work as expected)
- [ ] Documentation update
- [ ] Performance improvement
- [ ] Code refactoring

## Testing
- [ ] Unit tests pass
- [ ] Integration tests pass
- [ ] Manual testing completed
- [ ] Cross-browser testing completed

## Checklist
- [ ] Code follows the project's style guidelines
- [ ] Self-review of the code has been performed
- [ ] Code has been commented, particularly in hard-to-understand areas
- [ ] Corresponding changes to the documentation have been made
- [ ] Changes generate no new warnings
- [ ] New and existing unit tests pass locally

## Screenshots (if applicable)
Add screenshots to help explain your changes.

## Related Issues
Closes #123
Fixes #456
```

### Pull Request Best Practices
1. **Keep PRs small** - Focus on one feature or fix
2. **Write descriptive titles** - Clear and concise
3. **Provide detailed descriptions** - Explain what and why
4. **Add screenshots** - For UI changes
5. **Link related issues** - Use GitHub issue linking
6. **Request appropriate reviewers** - Include relevant team members
7. **Ensure tests pass** - All tests must be green
8. **Keep PRs up to date** - Rebase on main regularly

## 🏷️ Tagging and Releases

### Tag Naming
```bash
# Semantic versioning tags
v1.0.0                    # Major release
v1.1.0                    # Minor release
v1.1.1                    # Patch release
v2.0.0-beta.1             # Pre-release
v2.0.0-rc.1               # Release candidate

# Date-based tags
2024-01-15                # Date-based release
2024-Q1                   # Quarterly release
```

### Tag Operations
```bash
# Create lightweight tag
git tag v1.2.0

# Create annotated tag
git tag -a v1.2.0 -m "Release version 1.2.0"

# Push tags to remote
git push origin v1.2.0

# Push all tags
git push origin --tags

# Delete local tag
git tag -d v1.2.0

# Delete remote tag
git push origin --delete v1.2.0

# List all tags
git tag -l

# List tags with pattern
git tag -l "v1.*"
```

### Release Process
```bash
# 1. Create release branch
git checkout -b release/v1.2.0

# 2. Update version numbers
# Update package.json, version files, etc.

# 3. Update changelog
# Add new features, bug fixes, etc.

# 4. Commit changes
git add .
git commit -m "chore: prepare release v1.2.0"

# 5. Create tag
git tag -a v1.2.0 -m "Release version 1.2.0"

# 6. Push branch and tag
git push origin release/v1.2.0
git push origin v1.2.0

# 7. Merge to main
git checkout main
git merge release/v1.2.0
git push origin main

# 8. Merge to develop
git checkout develop
git merge release/v1.2.0
git push origin develop

# 9. Delete release branch
git branch -d release/v1.2.0
git push origin --delete release/v1.2.0
```

## 🔍 Code Review Process

### Review Checklist
- [ ] **Code Quality**
  - [ ] Code follows project style guidelines
  - [ ] Code is readable and well-structured
  - [ ] No code duplication
  - [ ] Proper error handling
  - [ ] Performance considerations

- [ ] **Functionality**
  - [ ] Feature works as expected
  - [ ] Edge cases are handled
  - [ ] No breaking changes
  - [ ] Backward compatibility maintained

- [ ] **Testing**
  - [ ] Unit tests are included
  - [ ] Integration tests are included
  - [ ] Tests are meaningful and comprehensive
  - [ ] All tests pass

- [ ] **Documentation**
  - [ ] Code is properly commented
  - [ ] README is updated if needed
  - [ ] API documentation is updated
  - [ ] Changelog is updated

- [ ] **Security**
  - [ ] No security vulnerabilities
  - [ ] Input validation is implemented
  - [ ] Sensitive data is protected
  - [ ] Authentication/authorization is proper

### Review Best Practices
1. **Be constructive** - Provide helpful feedback
2. **Be specific** - Point out exact issues
3. **Be respectful** - Maintain professional tone
4. **Be thorough** - Check all aspects of the code
5. **Be timely** - Review PRs promptly
6. **Be collaborative** - Work together to improve code
7. **Be consistent** - Apply same standards to all PRs

## 🛠️ Git Hooks

### Pre-commit Hook
```bash
#!/bin/sh
# .git/hooks/pre-commit

# Run linting
npm run lint

# Run type checking
npm run type-check

# Run tests
npm run test

# Check for TODO comments
if git diff --cached --name-only | xargs grep -l "TODO\|FIXME"; then
  echo "Warning: Found TODO or FIXME comments in staged files"
  read -p "Continue with commit? (y/n) " -n 1 -r
  echo
  if [[ ! $REPLY =~ ^[Yy]$ ]]; then
    exit 1
  fi
fi
```

### Commit Message Hook
```bash
#!/bin/sh
# .git/hooks/commit-msg

# Check commit message format
commit_regex='^(feat|fix|docs|style|refactor|test|chore|perf|ci|build)(\(.+\))?: .{1,50}'

if ! grep -qE "$commit_regex" "$1"; then
  echo "Invalid commit message format!"
  echo "Format: <type>(<scope>): <description>"
  echo "Types: feat, fix, docs, style, refactor, test, chore, perf, ci, build"
  exit 1
fi
```

### Pre-push Hook
```bash
#!/bin/sh
# .git/hooks/pre-push

# Run full test suite
npm run test:ci

# Run build
npm run build

# Check for sensitive data
if git diff --name-only | xargs grep -l "password\|secret\|key"; then
  echo "Error: Found potential sensitive data in staged files"
  exit 1
fi
```

## 🔧 Git Configuration

### Global Configuration
```bash
# Set user information
git config --global user.name "Your Name"
git config --global user.email "your.email@example.com"

# Set default branch name
git config --global init.defaultBranch main

# Set default editor
git config --global core.editor "code --wait"

# Set merge tool
git config --global merge.tool vscode

# Set diff tool
git config --global diff.tool vscode

# Set pull strategy
git config --global pull.rebase false

# Set push strategy
git config --global push.default simple

# Set line ending handling
git config --global core.autocrlf input  # Linux/Mac
git config --global core.autocrlf true   # Windows

# Set credential helper
git config --global credential.helper store
```

### Project Configuration
```bash
# Set project-specific user
git config user.name "Project Name"
git config user.email "project@example.com"

# Set project-specific editor
git config core.editor "code --wait"

# Set project-specific merge tool
git config merge.tool vscode

# Set project-specific diff tool
git config diff.tool vscode

# Set project-specific pull strategy
git config pull.rebase false

# Set project-specific push strategy
git config push.default simple
```

## 🎯 Git Workflow Best Practices

### General Guidelines
1. **Commit often** - Make small, focused commits
2. **Write good commit messages** - Clear and descriptive
3. **Keep branches focused** - One feature per branch
4. **Review code thoroughly** - Quality over speed
5. **Use meaningful branch names** - Clear and descriptive
6. **Keep main branch clean** - Only production-ready code
7. **Use pull requests** - For all changes
8. **Test before merging** - Ensure quality

### Collaboration Guidelines
1. **Communicate changes** - Let team know about major changes
2. **Coordinate merges** - Avoid conflicts
3. **Review promptly** - Don't leave PRs hanging
4. **Provide feedback** - Help improve code quality
5. **Follow conventions** - Maintain consistency
6. **Document decisions** - Explain why, not just what
7. **Be respectful** - Maintain professional tone
8. **Learn from feedback** - Improve continuously

## 🎯 AI Agent Best Practices

When assisting with Git workflow:

1. **Follow established patterns** in the project
2. **Use consistent naming conventions** for branches and commits
3. **Write clear commit messages** following the project's format
4. **Use appropriate branch types** for different changes
5. **Follow the established workflow** for the project
6. **Ensure proper code review** process is followed
7. **Use meaningful branch names** that describe the changes
8. **Follow the established merge strategies** for the project
9. **Use proper tagging** for releases and versions
10. **Maintain clean commit history** with focused commits

## 🔗 Additional Resources

- [Git Workflow Guide](https://git-workflow.org/)
- [Conventional Commits](https://conventionalcommits.org/)
- [Git Flow](https://nvie.com/posts/a-successful-git-branching-model/)
- [GitHub Flow](https://guides.github.com/introduction/flow/)
- [GitLab Flow](https://docs.gitlab.com/ee/topics/gitlab_flow.html)
