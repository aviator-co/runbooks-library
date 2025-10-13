# Update Angular Material to Latest Version
Upgrade Angular Material from current version to the latest stable release to leverage new features, bug fixes, and security improvements.

## Summary of changes
- Current Angular Material version needs to be identified and documented
- Breaking changes between current and target versions must be analyzed
- Dependencies and peer dependencies require updates alongside Angular Material
- Component APIs, theming system, and import paths may need modifications
- Custom styles and overrides might require adjustments for new Material Design specifications

## Execution Steps

### Step 1: Analysis and Planning
Establish baseline understanding of current state and plan the upgrade path.

#### 1.1: Audit Current Angular Material Usage
Document the current Angular Material version and catalog all components, services, and features being used across the application.
- Create **`docs/angular-material-audit.md`** documenting current version from **`package.json`**
- Scan codebase for all Angular Material imports using `grep -r "@angular/material" src/`
- List all Material components currently in use (MatButton, MatDialog, MatTable, etc.)
- Document any custom theming configuration in **`src/styles.scss`** or theme files
- Identify any Material CDK usage and custom component extensions

#### 1.2: Research Target Version and Breaking Changes
Analyze the upgrade path from current version to latest stable Angular Material release.
- Check Angular Material releases page and identify latest stable version
- Document target version in **`docs/angular-material-audit.md`**
- Review Angular Material changelog for breaking changes between current and target versions
- Identify deprecated APIs, removed components, or changed import paths
- Note any new peer dependency requirements (Angular version, TypeScript version)

#### 1.3: Create Migration Strategy Document
Develop comprehensive migration plan based on breaking changes analysis.
- Create **`docs/angular-material-migration-plan.md`** with step-by-step upgrade strategy
- Document all breaking changes that affect the current codebase
- Plan component-by-component migration approach for complex changes
- Identify high-risk areas requiring thorough testing
- Estimate effort and timeline for each migration phase

### Step 2: Dependency Updates
Update Angular Material and related dependencies while maintaining build compatibility.

#### 2.1: Update Angular Material Core Package
Upgrade the main Angular Material package and verify basic build compatibility.
- Update **`package.json`** to specify latest Angular Material version
- Run `npm install` or `yarn install` to install new version
- Verify application builds successfully with `ng build`
- Fix any immediate TypeScript compilation errors related to imports
- Update **`package-lock.json`** or **`yarn.lock`** in the same commit

#### 2.2: Update Angular CDK and Related Packages
Ensure all Angular Material ecosystem packages are updated to compatible versions.
- Update **`@angular/cdk`** to version matching Angular Material
- Update **`@angular/flex-layout`** if used to compatible version
- Update any other Material-related packages (**`@angular/material-moment-adapter`**, etc.)
- Verify all peer dependencies are satisfied by checking npm warnings
- Run `ng build` to ensure no dependency conflicts

#### 2.3: Update Development and Testing Dependencies
Update development tools and testing utilities to support new Angular Material version.
- Update **`@angular/material`** types in **`package.json`** devDependencies if applicable
- Update any Material-related testing utilities or harnesses
- Update **`karma.conf.js`** or **`jest.config.js`** if Material-specific configurations exist
- Verify test suite runs without dependency-related errors using `ng test`

### Step 3: Code Migration
Apply necessary code changes to work with the new Angular Material version.

#### 3.1: Update Import Statements
Migrate all Angular Material imports to use new import paths and module structure.
- Update component imports in all **`*.ts`** files to use new import paths
- Replace deprecated import paths with current equivalents
- Update module imports in **`app.module.ts`** and feature modules
- Verify all imports resolve correctly by running `ng build --prod`
- Fix any remaining import-related TypeScript errors

#### 3.2: Migrate Component API Changes
Update component usage to match new API signatures and property names.
- Replace deprecated component properties with new equivalents
- Update method calls that have changed signatures
- Modify component templates in **`*.html`** files for API changes
- Update component bindings and event handlers as needed
- Test each modified component individually to ensure functionality

#### 3.3: Update Custom Theming
Migrate custom theme configuration to work with new theming system.
- Update **`src/styles.scss`** or custom theme files for new theming API
- Replace deprecated theme mixins with current equivalents
- Update custom color palette definitions if format changed
- Verify custom CSS overrides still work with new component structure
- Test theme application across all components in development environment

### Step 4: Testing and Validation
Comprehensive testing to ensure all functionality works correctly with updated Angular Material.

#### 4.1: Update Unit Tests
Modify unit tests to work with new Angular Material testing APIs and component behaviors.
- Update test imports for Angular Material testing utilities
- Fix test cases that depend on changed component APIs or DOM structure
- Update component harnesses if using Angular Material test harnesses
- Verify all unit tests pass with `ng test`
- Add new tests for any newly available features being adopted

#### 4.2: Update Integration Tests
Ensure end-to-end and integration tests work with updated Material components.
- Update e2e test selectors if component DOM structure changed
- Modify integration tests for any changed component behaviors
- Update **`cypress/`** or **`e2e/`** test files for new Material component interactions
- Run full e2e test suite to verify application functionality
- Fix any test failures related to timing or component behavior changes

#### 4.3: Performance and Bundle Size Verification
Validate that the upgrade doesn't negatively impact application performance or bundle size.
- Run `ng build --prod --stats-json` and analyze bundle size changes
- Use webpack-bundle-analyzer to identify any unexpected size increases
- Verify tree-shaking still works effectively for unused Material components
- Test application performance in development and production builds
- Document any significant bundle size or performance changes

## Manual testing plan
- Verify all Material components render correctly across different screen sizes and browsers
- Test theming by switching between light/dark themes if applicable
- Navigate through all application routes to ensure Material components work in all contexts
- Test form components (inputs, selects, date pickers) for proper validation and user interaction
- Verify accessibility features still work correctly with screen readers and keyboard navigation
- Test Material dialogs, snackbars, and overlays for proper z-index and positioning
- Validate that custom Material component extensions still function as expected
- Check responsive behavior of Material layout components (grid, flex-layout)
- Test any Material animations and transitions for smooth operation
- Verify print styles still work correctly with updated Material components