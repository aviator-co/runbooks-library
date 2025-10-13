# Angular 12 to Angular 15 Migration Runbook
Step-by-step migration plan to upgrade Angular application from version 12 to 15 with minimal breaking changes.

## Summary of changes
- Current codebase is running on Angular 12 with outdated dependencies and potential security vulnerabilities
- Migration will be performed incrementally through Angular 13, 14, and finally 15 to ensure stability
- Breaking changes include Ivy renderer becoming mandatory, Angular Package Format changes, and updated TypeScript requirements
- Node.js version upgrade to 16+ required for Angular 15 compatibility
- Migration includes updating Angular CLI, Core, Material, and related ecosystem packages

## Execution Steps

### Step 1: Pre-Migration Assessment and Preparation
Establish baseline and prepare environment for the migration process.

#### 1.1: Environment and Dependency Audit
Create comprehensive documentation of current state and identify potential migration blockers.
- Document current Angular version, Node.js version, and all major dependencies in **migration-audit.md**
- Run `ng update` command to identify available updates and potential issues
- Check for deprecated APIs usage by running `ng lint` with deprecation rules enabled
- Verify current test suite passes with `ng test` and `ng e2e`
- Document any custom webpack configurations, build scripts, or Angular CLI customizations

#### 1.2: Node.js Version Upgrade
Upgrade Node.js to version 16 or higher as required by Angular 15.
- Update **package.json** engines field to specify Node.js 16+ requirement
- Update CI/CD pipeline configurations to use Node.js 16 in **/.github/workflows/** or equivalent CI files
- Update Docker files if applicable to use Node.js 16 base image
- Test local development environment with new Node.js version
- Update team documentation with new Node.js requirement

#### 1.3: Create Migration Branch and Backup
Establish proper version control strategy for the migration process.
- Create new branch `feature/angular-15-migration` from main branch
- Tag current state as `pre-angular-15-migration` for easy rollback
- Update **README.md** with migration status and any temporary development notes
- Commit initial state with message "Pre-migration checkpoint"

### Step 2: Angular 13 Migration
Migrate from Angular 12 to Angular 13 as the first incremental step.

#### 2.1: Update Angular Core to Version 13
Perform the core Angular framework upgrade to version 13.
- Run `ng update @angular/core@13 @angular/cli@13` to update core packages
- Update **package.json** to reflect new Angular 13 versions
- Resolve any peer dependency warnings by updating related packages
- Run `npm install` to install updated dependencies
- Execute `ng build` to verify successful compilation

#### 2.2: Update Angular Material and CDK
Upgrade Angular Material components to version 13 compatibility.
- Run `ng update @angular/material@13` to update Material Design components
- Update any custom theme files in **src/styles/** to handle breaking changes
- Test Material components functionality with `ng serve` and manual verification
- Update any custom Material component usage that may have changed APIs
- Run existing tests to ensure Material components still function correctly

#### 2.3: Address Angular 13 Breaking Changes
Handle specific breaking changes introduced in Angular 13.
- Update **angular.json** to remove deprecated options like `extractCss`
- Replace deprecated `ViewEngine` references with `Ivy` in any custom configurations
- Update any custom webpack configurations to be compatible with Angular 13
- Modify **tsconfig.json** if needed for new TypeScript version compatibility
- Run full test suite with `ng test` and fix any failing tests

#### 2.4: Verify Angular 13 Stability
Ensure the application is fully functional on Angular 13 before proceeding.
- Run `ng build --prod` to verify production build works correctly
- Execute complete test suite including unit tests and e2e tests
- Perform manual smoke testing of critical application features
- Check browser console for any new warnings or errors
- Commit changes with message "Complete Angular 13 migration"

### Step 3: Angular 14 Migration
Migrate from Angular 13 to Angular 14 with focus on standalone components preparation.

#### 3.1: Update Angular Core to Version 14
Perform the Angular 14 framework upgrade.
- Run `ng update @angular/core@14 @angular/cli@14` to update to Angular 14
- Update **package.json** with new Angular 14 dependency versions
- Install dependencies with `npm install` and resolve any conflicts
- Update **angular.json** configuration for any new Angular 14 options
- Verify build process with `ng build` command

#### 3.2: Update Supporting Packages for Angular 14
Upgrade ecosystem packages to Angular 14 compatible versions.
- Run `ng update @angular/material@14` for Material Design components
- Update other Angular packages like `@angular/forms`, `@angular/router` if separately managed
- Update **package.json** with compatible versions of third-party Angular libraries
- Resolve any peer dependency conflicts in package management
- Test updated packages with development server using `ng serve`

#### 3.3: Implement Angular 14 Features and Fixes
Address Angular 14 specific changes and prepare for Angular 15 features.
- Update **tsconfig.json** for new TypeScript version requirements
- Review and update any custom Angular schematics for Angular 14 compatibility
- Replace any deprecated APIs identified in Angular 14 migration guide
- Update **angular.json** build configurations for new optimization options
- Run comprehensive testing with `ng test` and `ng e2e`

#### 3.4: Optional Standalone Components Preparation
Prepare codebase for Angular 15 standalone components (optional but recommended).
- Identify components that could benefit from standalone architecture
- Create documentation in **standalone-components-plan.md** for future conversion
- Update component imports to be more explicit in preparation for standalone migration
- Test current component architecture remains stable
- Commit Angular 14 migration with message "Complete Angular 14 migration"

### Step 4: Angular 15 Migration
Final migration step from Angular 14 to Angular 15 with modern features.

#### 4.1: Update Angular Core to Version 15
Perform the final Angular framework upgrade to version 15.
- Run `ng update @angular/core@15 @angular/cli@15` for the final version upgrade
- Update **package.json** to reflect Angular 15 versions across all Angular packages
- Resolve any peer dependency conflicts with `npm install`
- Update **angular.json** for new Angular 15 configuration options
- Verify successful compilation with `ng build`

#### 4.2: Update Angular Material to Version 15
Upgrade Material Design components to the latest version.
- Run `ng update @angular/material@15` to update Material components
- Update **src/styles/** theme files for any Material 15 breaking changes
- Test Material components with development server using `ng serve`
- Update any custom Material component implementations for API changes
- Verify Material Design components render correctly in browser

#### 4.3: Implement Angular 15 Specific Updates
Handle breaking changes and new features specific to Angular 15.
- Update **tsconfig.json** for TypeScript 4.8+ compatibility requirements
- Remove any remaining ViewEngine references from **angular.json** and custom configurations
- Update **package.json** scripts if any Angular CLI command syntax has changed
- Implement new Angular 15 features like directive composition API if applicable
- Update any custom Angular builders or schematics for Angular 15 compatibility

#### 4.4: Final Testing and Validation
Comprehensive testing to ensure Angular 15 migration is complete and stable.
- Run full production build with `ng build --configuration production`
- Execute complete test suite including unit tests (`ng test`) and e2e tests (`ng e2e`)
- Perform thorough manual testing of all application features
- Check browser developer tools for any console warnings or errors
- Validate application performance compared to pre-migration baseline

#### 4.5: Update Documentation and Dependencies
Finalize migration with updated documentation and dependency cleanup.
- Update **README.md** with new Angular 15 requirements and setup instructions
- Update **package.json** to remove any temporary migration dependencies
- Create **MIGRATION-NOTES.md** documenting any breaking changes for team reference
- Update CI/CD pipeline configurations for Angular 15 build requirements
- Commit final changes with message "Complete Angular 15 migration"

## Manual testing plan
- Verify application loads correctly in development mode using `ng serve`
- Test all major user workflows including authentication, navigation, and core features
- Validate responsive design works across different screen sizes and browsers
- Check that all forms submit correctly and validation messages display properly
- Test any third-party integrations like payment processors, analytics, or external APIs
- Verify production build deploys successfully to staging environment
- Perform cross-browser testing on Chrome, Firefox, Safari, and Edge
- Test application performance using browser developer tools and compare to pre-migration metrics
- Validate that all existing bookmarks and deep links continue to work correctly
- Check that PWA features (if applicable) like service workers and offline functionality still work