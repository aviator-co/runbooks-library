# Runbooks Library

A comprehensive collection of Runbook templates for AI-powered code migrations, refactoring, and systematic code improvements. These templates provide structured, step-by-step instructions that can be executed by AI coding agents to automate complex development tasks.

## What are Runbooks?

Runbooks are structured markdown documents that define step-by-step procedures for code modifications, migrations, and improvements. Each Runbook contains:

- **Clear objectives** - What the migration/change accomplishes
- **Detailed steps** - Specific, actionable instructions for AI agents
- **Context and rationale** - Why each change is needed
- **Validation criteria** - How to verify successful completion

Runbooks can be executed by AI coding agents like [Claude Code](https://claude.com/claude-code) or used through [Aviator Runbooks](https://aviator.co/runbooks).

## Template Categories

### 🔵 React

Templates for React framework migrations and improvements:
- Component modernization (Class to Hooks)
- State management updates
- Performance optimizations
- React 18+ feature adoption

### 📘 TypeScript

TypeScript migration and type safety improvements:
- JavaScript to TypeScript conversion
- Type definition updates
- Strict mode adoption
- Generic implementations

### 🔄 Migration

General framework and language migration templates:
- Framework upgrades (Angular, Vue, etc.)
- Library migrations
- API version updates
- Database schema changes

### 🔧 Refactor

Code refactoring and systematic improvements:
- Code organization restructuring
- Design pattern implementations
- Code quality improvements
- Technical debt reduction

### ⚙️ Setup

Project setup and configuration templates:
- Development environment setup
- CI/CD pipeline configuration
- Tooling integration
- Project structure standardization

### 🛠️ Tooling

Development tooling and build system improvements:
- Build tool migrations (Webpack to Vite, etc.)
- Linting and formatting setup
- Development workflow optimization
- Package management updates

### 🧪 Test Quality

Testing improvements and quality assurance:
- Test coverage improvements
- Testing framework migrations
- Test structure optimization
- Quality gate implementations

### ⬆️ Upgrade

Framework and dependency upgrade procedures:
- Major version upgrades
- Breaking change handling
- Dependency conflict resolution
- Compatibility updates

### ⚡ Performance

Performance optimization and build improvements:
- Bundle size optimization
- Runtime performance improvements
- Build time reduction
- Resource optimization

### 🔧 Flaky Test

Flaky test resolution and test stability:
- Test reliability improvements
- Race condition fixes
- Environment-specific test issues
- Test isolation improvements

### 📖 Readability

Code readability and documentation improvements:
- Code formatting standardization
- Documentation generation
- Comment and naming improvements
- Code style consistency

### 📦 Misc

Miscellaneous templates for various use cases:
- Custom implementations
- Specialized migrations
- Experimental procedures
- Community contributions

## Usage

### With Claude Code

1. Copy the template you want to use
2. Provide to Claude Code as a Runbook and ask it to customize it for your code base

Example:

```
I want to migrate my React class components to hooks. Please customize this Runbook by analyzing our current codebase, and write that in a markdown file keeping the same format.

[Paste the raw Runbook here]
```

1. Execute this Rnbook with claude code, either all at once or step by step:

```markdown
Please use this new Runbook to execute Step 1 and create a PR.
```

### With Aviator Runbooks

1. **Browse templates** at [aviator.co/runbooks](https://aviator.co/runbooks)
2. **Select a template** that matches your needs
3. **Customize and execute** through the Aviator platform.

[Read the docs for details](https://docs.aviator.co/runbooks/concepts/templates).

### Manual Implementation

Each template can also be used as a manual checklist:
1. **Review the template** steps carefully
2. **Adapt instructions** to your specific codebase
3. **Execute steps manually** or with your preferred tools

## Template Structure

Each Runbook template follows this structure:

```markdown
# Runbook: [A short title of Runbook]

## Summary of changes
- List of findings based on the current code
- High level summary of changes that are covered in the steps below

## Prerequisites
- Links to other resources that could be relevant (e.g. code review guidelines)
- Some considerations and requirements that must be true

## Execution Steps

### Step 1: [Category Name]
Context if needed to explain the category, limit to 2-3 lines.
#### 1.1: [Task Name]
Description of the task with context. Do not use list items for this.
- Concrete substep description 1 as a list. \
Use trailing backslash for multiple lines.
- Concrete substep description 2 as a list.

#### 1.2: [Task Name]
Description of the task with context. Do not use list items for this.
- Concrete substep description A as a list.
- Concrete substep description B as a list.
- Concrete substep description C as a list.

#### 1.3: [NO-CODE-CHANGE] [Documentation Task Name]
Description of a documentation/planning task that doesn't generate code changes.
- Documentation substep A as a list.
- Planning substep B as a list.

### Step 2: [Category Name]
#### 2.1: [Task Name]
Description of the task with context. Do not use list items for this.
- Concrete substep description A as a list.

```

## Contributing

We welcome contributions to expand and improve this template library!

### How to Contribute

1. **Fork this repository**
2. **Create a new template** following our structure guidelines
3. **Test your template** with real projects
4. **Submit a pull request** with:
    - Clear description of the template’s purpose
    - Example use cases
    - Validation that it works as expected

### Template Guidelines

- **Be specific**: Provide concrete, actionable instructions
- **Include context**: Explain why each step is needed
- **Add examples**: Show code samples where helpful
- **Test thoroughly**: Verify templates work on real projects
- **Follow conventions**: Use consistent formatting and structure

### Categories

When creating templates, assign them to appropriate categories:
- Use existing categories when possible
- Suggest new categories for novel use cases
- Templates can belong to multiple categories

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## Support

- **Template Issues**: Open a GitHub issue for template-specific problems
- **Usage Questions**: Check our [documentation]([https://aviator.co/runbooks](https://docs.aviator.co/runbooks)) or community forums
- **Feature Requests**: Submit enhancement ideas via GitHub issues

## Related Projects

- [**Aviator Runbooks**](https://aviator.co/runbooks) - Professional platform for executing Runbooks
- [**Claude Code**](https://claude.com/claude-code) - AI coding assistant that can execute Runbooks

---

**Note**: These templates are designed to be executed by AI coding agents but can also serve as manual checklists for development teams. Always review and test changes in a development environment before applying to production code.
