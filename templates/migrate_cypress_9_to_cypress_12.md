# Migrate Cypress 9 to Cypress 12

Upgrade Cypress testing framework from version 9 to version 12 with configuration updates and breaking change fixes.

## Summary of changes
- Cypress 9 uses legacy configuration format and has deprecated APIs that need updating
- Major breaking changes include configuration file migration, plugin system overhaul, and command syntax updates
- Migration involves updating package dependencies, configuration files, test syntax, and CI/CD pipeline adjustments
- New features in Cypress 12 include component testing improvements and better TypeScript support

## Execution Steps

### Step 1: Pre-Migration Analysis and Planning
Assess current Cypress setup and identify breaking changes that need to be addressed.

#### 1.1: Audit Current Cypress Configuration
Document the current Cypress 9 setup to understand migration scope and potential issues.
- Create **cypress-migration-audit.md** in the project root documenting current **cypress.json** configuration
- List all custom plugins in **cypress/plugins/index.js** and their purposes
- Inventory all custom commands in **cypress/support/commands.js**
- Document any custom TypeScript configurations in **cypress/tsconfig.json**
- Note any CI/CD pipeline Cypress configurations and scripts

#### 1.2: Identify Breaking Changes Impact
Analyze Cypress 12 breaking changes against current codebase to create migration checklist.
- Review all test files for deprecated `cy.server()` and `cy.route()` usage (replaced with `cy.intercept()`)
- Check for `Cypress.Cookies.preserveOnce()` usage (removed in v12)
- Identify any usage of removed configuration options like `blacklistHosts`
- Document custom plugin dependencies that may need updates
- List any third-party Cypress plugins that require version compatibility checks

### Step 2: Dependency Updates
Update Cypress and related dependencies to version 12 compatible versions.

#### 2.1: Update Core Cypress Dependency
Upgrade Cypress package and verify compatibility with existing Node.js version.
- Update **package.json** to change `"cypress"` dependency from current version to `"^12.17.4"`
- Run `npm install` or `yarn install` to install new version
- Verify Node.js version compatibility (Cypress 12 requires Node.js 14.0.0+)
- Update any Cypress-related dev dependencies like `@cypress/webpack-preprocessor` if present

#### 2.2: Update Related Testing Dependencies
Ensure all Cypress ecosystem packages are compatible with version 12.
- Update `@cypress/code-coverage` to version `^3.10.0` if used
- Update `cypress-axe` to version `^1.4.0` if used for accessibility testing
- Update any other Cypress plugins listed in dependencies to their latest compatible versions
- Run `npm audit` to check for security vulnerabilities in updated packages

### Step 3: Configuration Migration
Migrate from cypress.json to cypress.config.js format and update plugin system.

#### 3.1: Create New Configuration File
Convert legacy cypress.json configuration to the new cypress.config.js format.
- Create **cypress.config.js** in project root using the new configuration structure
- Migrate all settings from **cypress.json** to the `e2e` object in cypress.config.js
- Convert `baseUrl`, `viewportWidth`, `viewportHeight` and other basic settings
- Migrate environment variables from `env` object to new format
- Add `setupNodeEvents` function to replace the old plugins system

#### 3.2: Migrate Plugin System
Convert cypress/plugins/index.js to the new setupNodeEvents format.
- Move plugin logic from **cypress/plugins/index.js** to `setupNodeEvents` function in **cypress.config.js**
- Update task registrations to use the new `on('task', {...})` syntax
- Convert any file preprocessing configurations to the new format
- Update environment variable handling if customized in plugins
- Remove **cypress/plugins/index.js** file after migration is complete

#### 3.3: Update Support File Configuration
Ensure support files are properly configured in the new system.
- Update `supportFile` configuration in cypress.config.js to point to **cypress/support/e2e.js**
- Rename **cypress/support/index.js** to **cypress/support/e2e.js** if it exists
- Verify custom commands in **cypress/support/commands.js** are still imported correctly
- Update any TypeScript support file references if using TypeScript

### Step 4: Test Code Updates
Fix breaking changes in test files and update deprecated APIs.

#### 4.1: Replace Deprecated Network Stubbing
Update all instances of cy.server() and cy.route() to use cy.intercept().
- Search for all `cy.server()` calls in test files and remove them (no longer needed)
- Replace `cy.route()` calls with equivalent `cy.intercept()` syntax
- Update fixture usage in network stubs to work with new intercept format
- Test network stubbing functionality to ensure API calls are properly mocked

#### 4.2: Update Removed Cookie Methods
Replace deprecated cookie preservation methods with new session management.
- Find all instances of `Cypress.Cookies.preserveOnce()` and replace with `cy.session()`
- Update cookie handling logic to use the new session API
- Test login flows and cookie-dependent functionality to ensure proper behavior
- Update any custom cookie manipulation commands

#### 4.3: Fix Configuration Option Changes
Update any references to removed or renamed configuration options.
- Replace `blacklistHosts` with `blockHosts` in configuration if used
- Update `chromeWebSecurity` references to `security` if needed
- Fix any other deprecated configuration option references found in audit
- Update test files that reference `Cypress.config()` with changed option names

### Step 5: TypeScript Support Updates
Update TypeScript configurations and type definitions for Cypress 12.

#### 5.1: Update TypeScript Configuration
Ensure TypeScript setup is compatible with Cypress 12 type definitions.
- Update **cypress/tsconfig.json** to include new Cypress 12 type definitions
- Add `"types": ["cypress"]` to compilerOptions if not already present
- Update `include` paths to match new file structure if changed
- Verify custom command type definitions are still working

#### 5.2: Update Custom Command Types
Fix TypeScript definitions for custom commands to work with Cypress 12.
- Update **cypress/support/commands.ts** or type definition files
- Fix any type errors related to custom command declarations
- Update `Cypress.Commands.add()` type definitions if needed
- Test TypeScript compilation to ensure no type errors remain

### Step 6: CI/CD Pipeline Updates
Update continuous integration configurations to work with Cypress 12.

#### 6.1: Update CI Configuration Files
Modify CI/CD pipeline files to use Cypress 12 and handle any new requirements.
- Update Docker images in **Dockerfile** or CI configs to use Node.js 14+ if needed
- Modify **GitHub Actions**, **Jenkins**, or other CI configurations to install correct Cypress version
- Update any Cypress Dashboard recording configurations if used
- Adjust memory and timeout settings if needed for Cypress 12 performance characteristics

#### 6.2: Update NPM Scripts
Ensure package.json scripts work correctly with new Cypress version.
- Test `npm run cypress:open` script works with new configuration
- Verify `npm run cypress:run` executes tests properly in headless mode
- Update any custom Cypress scripts to use new CLI options if needed
- Test parallel execution scripts if used in CI environment

### Step 7: Testing and Validation
Comprehensive testing of the migrated Cypress setup to ensure full functionality.

#### 7.1: Local Testing Validation
Run complete test suite locally to identify any remaining issues.
- Execute `npx cypress open` to test interactive mode with new configuration
- Run `npx cypress run` to test headless mode execution
- Verify all existing tests pass without modification beyond breaking changes
- Test custom commands and plugins work correctly
- Validate network stubbing and fixture loading functionality

#### 7.2: Cross-Browser Testing Verification
Ensure Cypress 12 browser support works correctly across all target browsers.
- Test Chrome browser execution with `--browser chrome`
- Test Firefox browser execution with `--browser firefox`
- Test Edge browser execution with `--browser edge` if supported
- Verify electron browser still works as default
- Test any browser-specific configurations or workarounds

## Manual testing plan
- Run `npx cypress open` and manually execute a sample of critical test cases in interactive mode
- Execute `npm run cypress:run` to verify headless mode works correctly with full test suite
- Test network stubbing functionality by running tests that mock API calls and verify responses
- Validate custom commands work by running tests that use project-specific Cypress commands
- Test CI/CD pipeline by triggering a build and ensuring Cypress tests execute successfully
- Verify test reporting and screenshots/videos are generated correctly in new version
- Check Cypress Dashboard integration if used by running tests with recording enabled
- Test component testing functionality if used by running component tests in isolation