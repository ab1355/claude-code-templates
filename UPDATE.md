# Using Claude Code Templates with Other IDEs

This guide explains how to adapt and use components from the Claude Code Templates repository with other AI-powered IDEs, specifically **Zed** and **VS Code (GitHub Copilot)**.

## Table of Contents

- [Overview](#overview)
- [Component Compatibility](#component-compatibility)
- [Zed IDE Integration](#zed-ide-integration)
- [VS Code / GitHub Copilot Integration](#vs-code--github-copilot-integration)
- [Manual Adaptation Guidelines](#manual-adaptation-guidelines)
- [Examples](#examples)
- [Limitations and Considerations](#limitations-and-considerations)

## Overview

While this repository is primarily designed for **Anthropic's Claude Code**, many components can be adapted for use with other AI-powered development environments. The components fall into several categories:

- **Agents** (600+): AI specialists with specific domain expertise
- **Commands** (200+): Custom slash commands for common workflows
- **MCPs** (55+): External service integrations via Model Context Protocol
- **Settings** (60+): IDE configuration files
- **Hooks** (39+): Automation triggers for development workflows
- **Templates** (14+): Complete project configurations

## Component Compatibility

### ✅ Highly Compatible Components

These components can be easily adapted:

- **Agents**: Convert to custom agents/rules in Zed or VS Code
- **Commands**: Adapt as custom instructions or agent behaviors
- **Settings**: May require manual IDE-specific configuration

### ⚠️ Partially Compatible Components

These require significant adaptation:

- **MCPs**: Zed supports MCP protocol; VS Code requires extensions
- **Hooks**: Need IDE-specific automation (tasks, extensions)
- **Templates**: May need restructuring for different IDE workflows

### ❌ Not Compatible

- Claude Code-specific CLI features
- Download tracking and analytics
- Some automation hooks tied to Claude Code APIs

## Zed IDE Integration

Zed IDE has built-in support for AI agents and custom prompts, making it an excellent alternative for Claude Code users.

### Configuration Location

Zed uses:
- **Project-level rules**: `.rules` file in project root
- **Global settings**: `~/.config/zed/settings.json` (Linux/Mac) or `%APPDATA%\Zed\settings.json` (Windows)
- **Agent configuration**: Via Agent Panel UI or settings

### Converting Agents

Claude Code agents can be converted to Zed rules or custom agent configurations.

#### Option 1: Rules File

Create a `.rules` file in your project root:

```markdown
# .rules

## Frontend Development Expert
You are a frontend development specialist focused on React, TypeScript, and modern web technologies.

Key responsibilities:
- Write clean, maintainable React components
- Implement TypeScript best practices
- Optimize performance and bundle size
- Follow accessibility standards (WCAG 2.1 AA)
- Use modern React patterns (hooks, suspense, concurrent features)

## Security Auditor
You are a security expert focused on identifying vulnerabilities.

Key responsibilities:
- Review code for OWASP Top 10 vulnerabilities
- Check for authentication and authorization issues
- Identify data exposure risks
- Recommend security best practices
```

Usage: In Zed's chat, type `@rule Frontend Development Expert` to apply the rule.

#### Option 2: Agent Panel Configuration

1. Open Zed's Agent Panel (`Cmd/Ctrl+Shift+A`)
2. Click "New Agent" or configure existing agents
3. Define agent profile with custom instructions
4. Set permissions and available tools
5. Select preferred AI model

### Converting Commands

Commands become part of your rules or agent instructions:

```markdown
## Test Generation Command
When asked to generate tests:
1. Analyze the target file or function
2. Identify test cases (happy path, edge cases, errors)
3. Use the project's testing framework (Jest, Vitest, Playwright)
4. Follow AAA pattern (Arrange, Act, Assert)
5. Include mock data and setup as needed
6. Ensure tests are isolated and repeatable
```

### Converting Settings

Adapt Claude Code settings to Zed's configuration format:

**Claude Code setting** (`.claude/settings.json`):
```json
{
  "timeout": 300000,
  "maxTokens": 8192
}
```

**Zed equivalent** (`settings.json`):
```json
{
  "assistant": {
    "default_model": "claude-opus-4.5",
    "version": "2",
    "enabled": true,
    "timeout": 300000
  }
}
```

### MCP Support in Zed

Zed natively supports the Model Context Protocol (MCP). To use MCP servers:

1. Configure MCP in Zed settings:
```json
{
  "mcp_servers": {
    "github": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-github"],
      "env": {
        "GITHUB_PERSONAL_ACCESS_TOKEN": "<your-token>"
      }
    }
  }
}
```

2. MCP servers from this repository can often be used directly
3. Restart Zed to load the MCP server

### Example: Complete Zed Setup

**Project structure:**
```
your-project/
├── .rules                    # Custom prompts
├── .zed/                     # Zed-specific config (optional)
└── [your code]
```

**`.rules` file:**
```markdown
# Project Rules

## Architecture
- Use clean architecture principles
- Separate concerns: UI, business logic, data access
- Keep components under 200 lines

## Code Style
- Use TypeScript strict mode
- No `any` types
- Prefer functional components
- Use descriptive variable names

## Testing
- Write tests for all business logic
- Aim for 80% code coverage
- Use Vitest for unit tests
- Use Playwright for E2E tests
```

## VS Code / GitHub Copilot Integration

VS Code with GitHub Copilot supports custom agents and instructions through Markdown files.

### Configuration Locations

- **Workspace instructions**: `.github/copilot-instructions.md` (project root)
- **Custom agents**: `.github/agents/*.agent.md` (project-level)
- **Global agents**: 
  - **Windows**: `%APPDATA%\Code\User\prompts\`
  - **Linux/Mac**: `~/.config/Code/User/prompts/`
- **Path-specific instructions**: `.github/instructions/*.instructions.md`

### Converting Agents

#### Project-Level Agent

Create `.github/agents/frontend-developer.agent.md`:

```markdown
---
description: "Frontend development specialist focused on React and TypeScript"
tools: ['vscode', 'read', 'edit', 'search', 'bash']
model: 'Claude Opus 4.5 (anthropic)'
---

# Frontend Development Expert

You are a senior frontend developer specializing in:
- React 18+ with hooks and concurrent features
- TypeScript strict mode
- Modern CSS (CSS Modules, Tailwind, styled-components)
- Performance optimization
- Accessibility (WCAG 2.1 AA)
- Testing (Jest, React Testing Library, Playwright)

## Approach

1. **Component Design**: 
   - Keep components focused and single-purpose
   - Use composition over inheritance
   - Implement proper prop typing with TypeScript

2. **Performance**:
   - Optimize re-renders with React.memo, useMemo, useCallback
   - Implement code splitting and lazy loading
   - Monitor bundle size

3. **Testing**:
   - Write unit tests for logic
   - Component tests for user interactions
   - E2E tests for critical user flows

## Code Style

- Use functional components with hooks
- Avoid inline functions in JSX props
- Use absolute imports with path aliases
- Follow the project's ESLint configuration

Don't make changes outside the frontend codebase unless explicitly requested.
```

Usage: In Copilot Chat, type `@frontend-developer Create a reusable button component`

#### Global Instructions

Create `.github/copilot-instructions.md`:

```markdown
# Project Coding Standards

## General Guidelines
- Write self-documenting code with clear names
- Keep functions under 50 lines
- Use TypeScript for all new code
- Never use `any` type without justification

## Security
- Never hardcode secrets or API keys
- Use environment variables for configuration
- Validate and sanitize all user input
- Implement proper error handling

## Testing
- Write tests before fixing bugs
- Maintain 70%+ code coverage
- Use descriptive test names
- Follow AAA pattern (Arrange, Act, Assert)

## Git
- Write clear, descriptive commit messages
- Keep commits focused and atomic
- Reference issue numbers in commits
```

#### Path-Specific Instructions

Create `.github/instructions/tests.instructions.md`:

```markdown
---
applyTo: "**/*.test.ts"
---

# Testing Standards

- Use Vitest for unit testing
- Use `describe` for grouping related tests
- Use `test` or `it` for individual test cases
- Name tests descriptively: "should [expected behavior] when [condition]"
- Mock external dependencies
- Clean up after tests (cleanup functions, beforeEach/afterEach)

## Example Structure

```typescript
describe('UserService', () => {
  test('should create user when valid data provided', async () => {
    // Arrange
    const userData = { name: 'John', email: 'john@example.com' };
    
    // Act
    const result = await userService.create(userData);
    
    // Assert
    expect(result).toBeDefined();
    expect(result.email).toBe(userData.email);
  });
});
```
```

### Converting Commands

Commands become custom instructions or can be invoked as Copilot behaviors:

```markdown
# .github/copilot-instructions.md

## Custom Commands

### Generate Tests
When I ask you to "generate tests" or use the phrase "create tests for":
1. Analyze the target code
2. Identify test scenarios (happy path, edge cases, error conditions)
3. Create comprehensive test file with appropriate imports
4. Use the project's testing framework
5. Include setup/teardown as needed
6. Add helpful comments explaining complex test cases

### Optimize Performance
When I ask to "optimize performance":
1. Profile the code to identify bottlenecks
2. Look for unnecessary re-renders (React)
3. Check for inefficient loops or algorithms
4. Suggest memoization where appropriate
5. Recommend code splitting opportunities
6. Provide before/after performance metrics when possible

### Security Audit
When I ask for a "security audit":
1. Check for OWASP Top 10 vulnerabilities
2. Review authentication and authorization
3. Check for injection vulnerabilities (SQL, XSS, etc.)
4. Verify input validation and sanitization
5. Check for sensitive data exposure
6. Review dependency vulnerabilities
```

### MCP Support in VS Code

VS Code doesn't natively support MCP. Alternatives:

1. **Extensions**: Look for VS Code extensions that provide similar functionality
2. **API Integration**: Use VS Code extension API to create custom integrations
3. **External Tools**: Run MCP servers externally and integrate via REST APIs

### Example: Complete VS Code Setup

**Project structure:**
```
your-project/
├── .github/
│   ├── copilot-instructions.md       # Global project instructions
│   ├── agents/
│   │   ├── security-reviewer.agent.md
│   │   ├── frontend-developer.agent.md
│   │   └── backend-architect.agent.md
│   └── instructions/
│       ├── tests.instructions.md      # Test file specific rules
│       └── api.instructions.md        # API endpoint specific rules
├── .vscode/
│   └── settings.json                  # VS Code workspace settings
└── [your code]
```

**`.github/copilot-instructions.md`:**
```markdown
# Code Quality Standards

## TypeScript
- Enable strict mode
- No `any` types
- Use interfaces for object shapes
- Use type guards for runtime checks

## React
- Functional components only
- Use hooks for state and effects
- Implement error boundaries
- Use React.lazy for code splitting

## Testing
- 80% minimum code coverage
- Test behavior, not implementation
- Mock external dependencies
- Use meaningful test descriptions

## Performance
- Keep bundle size under 200KB (gzipped)
- Implement lazy loading for routes
- Use image optimization
- Minimize JavaScript execution time
```

## Manual Adaptation Guidelines

### Step-by-Step Conversion Process

1. **Identify Component Type**
   - Agent → Custom agent/rule file
   - Command → Custom instruction or workflow
   - Setting → IDE-specific configuration
   - MCP → MCP server config or extension
   - Hook → IDE automation or task

2. **Extract Core Logic**
   - Read the component's description and instructions
   - Identify key behaviors and constraints
   - Note any tool or API dependencies

3. **Adapt Format**
   - **For Zed**: Create `.rules` file or agent configuration
   - **For VS Code**: Create `.agent.md` or instructions file

4. **Test and Iterate**
   - Try the converted component in real scenarios
   - Refine instructions based on results
   - Add examples or clarifications as needed

### Content Adaptation Tips

- **Be Specific**: Provide clear, actionable instructions
- **Include Examples**: Show expected input/output formats
- **Define Boundaries**: Specify what the agent should/shouldn't do
- **List Tools**: Explicitly state which tools the agent can use
- **Set Constraints**: Define any limitations or requirements

## Examples

### Example 1: React Expert Agent

**Original Claude Code Agent** (`cli-tool/components/agents/frontend/react-expert.md`):
```markdown
---
name: react-expert
description: React development specialist
tools: Read, Write, Edit, Bash, Grep
model: sonnet
---

You are a React expert focused on modern patterns and performance optimization.
[... full agent content ...]
```

**Zed Conversion** (`.rules`):
```markdown
## React Expert
You are a React development specialist focused on modern patterns and performance.

Key expertise:
- React 18+ features (concurrent rendering, transitions)
- Performance optimization (memo, useMemo, useCallback)
- Component architecture and composition
- State management (Context, reducers)
- Testing with React Testing Library

When working on React code:
- Use functional components with hooks
- Implement proper prop types with TypeScript
- Optimize re-renders
- Follow React best practices
- Write testable components
```

**VS Code Conversion** (`.github/agents/react-expert.agent.md`):
```markdown
---
description: "React development specialist for modern web applications"
tools: ['vscode', 'read', 'edit', 'search', 'bash']
model: 'Claude Opus 4.5 (anthropic)'
---

# React Development Expert

Specialized in React 18+ development with focus on:

## Core Competencies
- Modern React patterns (hooks, suspense, concurrent features)
- Performance optimization and profiling
- Component architecture and reusability
- TypeScript integration
- Testing strategies

## Development Approach
1. Component design with single responsibility
2. Proper state management
3. Performance-first mindset
4. Accessibility compliance
5. Comprehensive testing

## Code Standards
- Functional components only
- Strict TypeScript typing
- Descriptive naming
- Proper error boundaries
- Optimized re-renders

Don't modify backend code or infrastructure unless explicitly asked.
```

### Example 2: Test Generation Command

**Original Claude Code Command** (`cli-tool/components/commands/testing/generate-tests.md`):
```markdown
# /generate-tests

Create comprehensive test suite for the specified code.
[... full command content ...]
```

**Zed Conversion** (`.rules`):
```markdown
## Test Generation
When asked to generate or create tests:

1. **Analysis Phase**:
   - Identify the code to test (function, component, module)
   - Determine test types needed (unit, integration, E2E)
   - Review existing test patterns in the project

2. **Test Creation**:
   - Use the project's testing framework
   - Follow AAA pattern (Arrange, Act, Assert)
   - Cover happy path and edge cases
   - Include error scenarios

3. **Test Structure**:
   ```javascript
   describe('Feature', () => {
     test('should behave correctly when given valid input', () => {
       // Arrange
       // Act
       // Assert
     });
   });
   ```

4. **Coverage Goals**:
   - All public APIs
   - Edge cases and boundaries
   - Error conditions
   - Integration points
```

**VS Code Conversion** (`.github/copilot-instructions.md`):
```markdown
## Test Generation Workflow

When I request test generation (e.g., "generate tests for X" or "create tests"):

### Step 1: Analyze Target
- Identify what needs testing (function, class, component, API)
- Determine appropriate test types
- Check for existing tests to match style

### Step 2: Create Test Structure
- Use project's testing framework (Jest, Vitest, etc.)
- Organize with `describe` blocks
- Use descriptive test names
- Follow AAA pattern: Arrange, Act, Assert

### Step 3: Test Coverage
- ✅ Happy path (expected behavior)
- ✅ Edge cases (boundaries, empty inputs)
- ✅ Error scenarios (invalid inputs, exceptions)
- ✅ Integration points (external dependencies)

### Step 4: Best Practices
- Mock external dependencies
- Use test utilities and helpers
- Keep tests isolated and independent
- Add setup/teardown as needed
- Include helpful comments

### Example Test Format
```typescript
describe('UserService', () => {
  test('should create user with valid data', async () => {
    // Arrange
    const userData = { name: 'John', email: 'john@example.com' };
    
    // Act
    const result = await userService.create(userData);
    
    // Assert
    expect(result.id).toBeDefined();
    expect(result.email).toBe(userData.email);
  });
  
  test('should throw error when email is invalid', async () => {
    // Arrange
    const userData = { name: 'John', email: 'invalid' };
    
    // Act & Assert
    await expect(userService.create(userData)).rejects.toThrow();
  });
});
```
```

### Example 3: Security Settings

**Original Claude Code Setting** (`cli-tool/components/settings/security/read-only-mode.json`):
```json
{
  "description": "Enable read-only mode to prevent code modifications",
  "env": {
    "CLAUDE_CODE_READ_ONLY": "1"
  }
}
```

**Zed Conversion** (`settings.json`):
```json
{
  "assistant": {
    "agent_permissions": {
      "file_write": false,
      "file_edit": false,
      "file_delete": false,
      "bash_execute": false
    }
  }
}
```

**VS Code Conversion** (`.github/agents/reviewer.agent.md`):
```markdown
---
description: "Read-only code reviewer"
tools: ['vscode', 'read', 'search']
model: 'Claude Opus 4.5 (anthropic)'
---

# Code Reviewer (Read-Only)

You are a code reviewer who only reads and analyzes code. You cannot make edits.

## Capabilities
- ✅ Read and analyze code
- ✅ Search for patterns
- ✅ Provide recommendations
- ✅ Explain code behavior

## Restrictions
- ❌ No file modifications
- ❌ No code edits
- ❌ No file creation or deletion
- ❌ No command execution

When reviewing code:
1. Analyze for correctness and best practices
2. Identify potential bugs or issues
3. Suggest improvements
4. Explain reasoning clearly

Always provide actionable feedback with examples.
```

## Limitations and Considerations

### Zed IDE Limitations

- **Learning Curve**: Different UI and workflows from Claude Code
- **MCP Maturity**: MCP support is newer, some servers may not work
- **Extension Ecosystem**: Smaller than VS Code
- **Agent Handoffs**: Complex multi-agent workflows may need manual orchestration

### VS Code / Copilot Limitations

- **No Native MCP**: Requires extensions or workarounds
- **Copilot-Specific**: Features tied to GitHub Copilot subscription
- **Agent Context**: Limited context sharing between agents
- **Model Selection**: Less flexibility in choosing AI models

### General Considerations

1. **Manual Conversion Required**: No automated tool to convert components
2. **Testing Needed**: Converted components should be tested in real scenarios
3. **Ongoing Maintenance**: Updates to Claude Code components won't auto-sync
4. **Feature Parity**: Some Claude Code features have no equivalent
5. **Performance**: Agent behavior may vary between different AI models

## Getting Started

### Quick Start for Zed

1. Browse [Claude Code Templates](https://aitmpl.com) to find useful agents
2. Copy the agent's core instructions
3. Create/edit `.rules` file in your project root
4. Add the agent instructions in markdown format
5. Use `@rule [agent-name]` in Zed's chat

### Quick Start for VS Code

1. Create `.github/copilot-instructions.md` for general guidelines
2. Create `.github/agents/*.agent.md` for specialized agents
3. Test agents with `@agent-name` in Copilot Chat
4. Refine based on results
5. Share with team via Git

## Contributing

Found a better way to convert components? Have IDE-specific tips? 

- **Contribute examples**: Submit PRs with conversion examples
- **Share workflows**: Document your adaptation process
- **Report issues**: Let us know what doesn't work

## Resources

### Zed IDE
- [Official Zed Documentation](https://zed.dev/docs)
- [Agent Panel Guide](https://zed.dev/docs/ai/agent-panel)
- [Configuration Reference](https://zed.dev/docs/ai/configuration)

### VS Code / GitHub Copilot
- [Custom Agents Documentation](https://code.visualstudio.com/docs/copilot/customization/custom-agents)
- [Custom Instructions Guide](https://code.visualstudio.com/docs/copilot/customization/custom-instructions)
- [Awesome Copilot Customizations](https://github.com/microsoft/awesome-copilot)

### Claude Code Templates
- [Browse Templates](https://aitmpl.com)
- [Documentation](https://docs.aitmpl.com)
- [GitHub Repository](https://github.com/davila7/claude-code-templates)

---

**Note**: This is an unofficial guide for adapting Claude Code Templates to other IDEs. Results may vary based on IDE version, AI model, and component complexity. Always test converted components thoroughly before relying on them in production workflows.
