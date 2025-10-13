# Format code with Prettier configuration
Implement Prettier code formatting across the codebase with proper configuration and integration into development workflow.

## Summary of changes
- Current codebase lacks consistent code formatting standards leading to inconsistent style across files
- Add Prettier configuration with project-specific rules for consistent code formatting
- Integrate Prettier into development workflow with pre-commit hooks and CI/CD pipeline
- Format existing codebase to match new standards while preserving functionality

## Execution Steps

### Step 1: Prettier Setup and Configuration
Configure Prettier with project-specific formatting rules and install necessary dependencies.

#### 1.1: Install Prettier dependencies
Add Prettier and related packages to the project with appropriate version constraints.
- Install prettier as a dev dependency using `npm install --save-dev prettier`
- Install prettier plugins if needed (e.g., `@prettier/plugin-php`, `prettier-plugin-java`) based on project languages
- Update **package.json** to include prettier in devDependencies section
- Verify installation by running `npx prettier --version`

#### 1.2: Create Prettier configuration files
Establish formatting rules and ignore patterns for the project.
- Create **.prettierrc.json** file in project root with formatting rules (tab width, semicolons, quotes, etc.)
- Create **.prettierignore** file to exclude build artifacts, node_modules, and generated files
- Add common ignore patterns like `dist/`, `build/`, `*.min.js`, `coverage/`
- Test configuration by running `npx prettier --check .` on a sample file

#### 1.3: Configure editor integration
Set up development environment for automatic formatting on save.
- Create **.vscode/settings.json** with Prettier as default formatter and format-on-save enabled
- Add **.editorconfig** file if not present to ensure consistent editor behavior
- Document Prettier setup instructions in **README.md** or **CONTRIBUTING.md**
- Include instructions for other popular editors (IntelliJ, Sublime, etc.)

### Step 2: Workflow Integration
Integrate Prettier into development and CI/CD workflows to enforce consistent formatting.

#### 2.1: Add npm scripts for formatting
Create convenient commands for developers to format and check code.
- Add `"format"` script to **package.json** that runs `prettier --write .`
- Add `"format:check"` script that runs `prettier --check .`
- Add `"format:staged"` script for formatting only staged files using `prettier --write $(git diff --cached --name-only --diff-filter=ACMR)`
- Test scripts by running `npm run format:check` to verify they work correctly

#### 2.2: Set up pre-commit hooks
Ensure code is formatted before commits using Git hooks.
- Install husky for Git hooks management: `npm install --save-dev husky`
- Install lint-staged for running commands on staged files: `npm install --save-dev lint-staged`
- Initialize husky with `npx husky install` and add to **package.json** prepare script
- Create **.husky/pre-commit** hook that runs `npx lint-staged`
- Configure lint-staged in **package.json** to run prettier on staged files

#### 2.3: Update CI/CD pipeline
Add formatting checks to continuous integration to prevent unformatted code from being merged.
- Update CI configuration file (**.github/workflows/**, **.gitlab-ci.yml**, etc.) to include formatting check step
- Add step that runs `npm run format:check` after dependency installation
- Configure step to fail the build if formatting issues are found
- Add formatting check as a required status check for pull requests

### Step 3: Codebase Formatting
Apply Prettier formatting to existing codebase while maintaining functionality.

#### 3.1: Format existing codebase
Apply consistent formatting to all existing code files.
- Run `npm run format` to format all files in the codebase
- Review changes using `git diff` to ensure only formatting changes were made
- Verify no functional changes by running existing test suite
- Commit formatting changes with descriptive message like "Apply Prettier formatting to entire codebase"

#### 3.2: Update documentation and guidelines
Ensure team members understand the new formatting standards and workflow.
- Update **CONTRIBUTING.md** with Prettier setup and usage instructions
- Add section about formatting requirements for pull requests
- Document how to resolve formatting conflicts and common issues
- Include troubleshooting guide for common Prettier configuration problems

## Manual testing plan
- Verify Prettier installation by running `npx prettier --version` and confirming version output
- Test formatting by creating a poorly formatted test file and running `npm run format`, then verify it gets properly formatted
- Test pre-commit hook by staging an unformatted file and attempting to commit, verify it gets formatted automatically
- Verify CI/CD integration by creating a pull request with unformatted code and confirming the build fails
- Test editor integration by saving a file in VS Code and confirming it formats automatically
- Run full test suite after formatting changes to ensure no functionality was broken
- Verify ignore patterns work by checking that files in **.prettierignore** are not formatted