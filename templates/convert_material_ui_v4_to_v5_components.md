# Material UI v4 to v5 Migration Runbook
Comprehensive migration plan to upgrade Material UI components from version 4 to version 5 with minimal breaking changes.

## Summary of changes
- Material UI v5 introduces breaking changes in theming, styling system, and component APIs
- Migration requires updating imports, theme structure, styling approaches, and component props
- New emotion-based styling system replaces JSS-based makeStyles
- Component API changes require prop updates and deprecated feature removal
- Codemod tools will assist with automated transformations where possible

## Execution Steps

### Step 1: Pre-Migration Analysis and Setup
Prepare the codebase for migration by analyzing current usage and setting up necessary tooling.

#### 1.1: Audit Current Material UI Usage
Create a comprehensive audit of existing Material UI v4 usage across the codebase.
- Scan all files for Material UI imports using grep or similar tools
- Document all components currently in use in **audit-mui-usage.md**
- Identify custom themes, makeStyles usage, and withStyles implementations
- List all Material UI dependencies in package.json and their versions
- Create backup branch **backup/pre-mui-v5-migration** for rollback purposes

#### 1.2: Install Migration Dependencies
Set up the necessary packages and tools for the migration process.
- Install Material UI v5 packages alongside v4: `@mui/material`, `@mui/system`, `@emotion/react`, `@emotion/styled`
- Install migration helpers: `@mui/codemod`, `@mui/types`
- Update **package.json** to include both v4 and v5 dependencies temporarily
- Install emotion dependencies to replace JSS: `@emotion/cache`, `@emotion/server`
- Run `npm install` to ensure all dependencies are properly installed

#### 1.3: Create Migration Strategy Document
Document the step-by-step migration approach and potential risks.
- Create **migration-strategy.md** with detailed migration phases
- Document breaking changes specific to components used in the project
- List rollback procedures and contingency plans
- Define testing checkpoints after each major migration step
- Include timeline estimates for each migration phase

### Step 2: Theme System Migration
Migrate the theme structure and styling system from v4 to v5 format.

#### 2.1: Update Theme Provider Setup
Replace v4 ThemeProvider with v5 equivalent and update theme structure.
- Replace `@material-ui/core/styles/ThemeProvider` imports with `@mui/material/styles/ThemeProvider`
- Update theme creation from `createMuiTheme` to `createTheme` in theme files
- Modify theme structure to match v5 format (palette, typography, spacing changes)
- Update any custom theme augmentation to use v5 TypeScript interfaces
- Test theme provider setup with a simple component to ensure no runtime errors

#### 2.2: Migrate Custom Styling Solutions
Convert makeStyles and withStyles usage to v5 styling approaches.
- Run Material UI codemod for makeStyles: `npx @mui/codemod v5.0.0/jss-to-styled`
- Manually convert complex makeStyles implementations to `styled` components or `sx` prop
- Replace `withStyles` HOCs with `styled` components from `@mui/material/styles`
- Update theme access from `useTheme` hook where theme structure changed
- Ensure all styling solutions compile and render correctly

#### 2.3: Update Global Styles and CSS Baseline
Migrate global styling setup to use v5 equivalents.
- Replace `@material-ui/core/CssBaseline` with `@mui/material/CssBaseline`
- Update any custom global styles to work with emotion instead of JSS
- Migrate `@material-ui/core/styles/createGlobalStyle` usage if present
- Test global styles application across different pages and components
- Verify CSS reset and normalization still works as expected

### Step 3: Core Component Migration
Systematically migrate Material UI components from v4 to v5 imports and APIs.

#### 3.1: Update Component Imports
Replace all Material UI v4 imports with v5 equivalents across the codebase.
- Run codemod for import updates: `npx @mui/codemod v5.0.0/preset-safe`
- Manually update any imports not caught by codemod (custom paths, re-exports)
- Replace `@material-ui/core` imports with `@mui/material`
- Update `@material-ui/icons` imports to `@mui/icons-material`
- Verify all import statements resolve correctly and components render

#### 3.2: Fix Breaking Component API Changes
Update component props and APIs that have breaking changes in v5.
- Update `TextField` variant prop values (`filled` becomes `outlined` default)
- Replace deprecated `color="textPrimary"` with `color="primary"` in Typography
- Update `Chip` component props (remove deprecated `clickable` combinations)
- Fix `Autocomplete` prop changes and new required props
- Update `DataGrid` component if used (significant API changes)
- Test each updated component for proper functionality

#### 3.3: Handle Removed Components and Props
Address components and props that were removed or significantly changed in v5.
- Replace `Backdrop` component usage with updated v5 API
- Update `Modal` component props and event handling
- Handle `withMobileDialog` removal by implementing custom responsive logic
- Replace removed `ExpansionPanel` with `Accordion` component
- Update any custom component wrappers that depend on removed v4 features

### Step 4: Advanced Features Migration
Migrate advanced Material UI features like lab components and date pickers.

#### 4.1: Migrate Lab Components
Update Material UI Lab components to their v5 equivalents or stable versions.
- Replace `@material-ui/lab` imports with `@mui/lab` or `@mui/material`
- Update `Autocomplete` if it was imported from lab (now in core)
- Migrate `DatePicker` and related components to `@mui/x-date-pickers`
- Update `TreeView` component imports and API usage
- Test all lab components for proper functionality and styling

#### 4.2: Update Date and Time Pickers
Migrate date picker components to the new MUI X package structure.
- Install `@mui/x-date-pickers` package for date/time picker components
- Replace `@material-ui/pickers` imports with `@mui/x-date-pickers`
- Update `LocalizationProvider` setup and adapter configuration
- Fix any breaking changes in date picker component APIs
- Update date picker styling to work with v5 theme system
- Test date picker functionality across different locales and formats

#### 4.3: Handle Custom Component Integration
Update custom components that integrate deeply with Material UI v4 internals.
- Review custom components that extend Material UI components
- Update any usage of internal Material UI utilities or undocumented APIs
- Fix custom components that rely on v4-specific theme structure
- Update component testing that mocks Material UI internals
- Ensure custom components maintain their functionality with v5 components

### Step 5: Testing and Cleanup
Comprehensive testing and removal of v4 dependencies.

#### 5.1: Update Test Suite
Ensure all tests pass with the new Material UI v5 setup.
- Update test utilities that mock Material UI components
- Fix snapshot tests that changed due to component structure updates
- Update integration tests that rely on specific Material UI behavior
- Add new tests for any custom migration logic or workarounds
- Run full test suite and fix any remaining failures

#### 5.2: Performance and Bundle Analysis
Analyze the impact of migration on application performance and bundle size.
- Run bundle analyzer to compare v4 vs v5 bundle sizes
- Check for any performance regressions in component rendering
- Verify tree-shaking is working properly with v5 imports
- Document any significant performance changes in **performance-impact.md**
- Optimize imports and remove any unnecessary dependencies

#### 5.3: Remove v4 Dependencies
Clean up the codebase by removing all Material UI v4 dependencies.
- Remove all `@material-ui/*` packages from **package.json**
- Remove any v4-specific polyfills or compatibility code
- Update any documentation that references v4 components or APIs
- Remove migration-specific code and comments
- Run final dependency audit to ensure no v4 packages remain

### Step 6: Documentation and Deployment
Finalize migration with updated documentation and deployment verification.

#### 6.1: Update Development Documentation
Ensure all development documentation reflects the v5 migration.
- Update component usage examples in **README.md** and development guides
- Document any new patterns or best practices adopted during migration
- Update styling guidelines to reflect emotion-based approach
- Create troubleshooting guide for common v5 issues in **mui-v5-troubleshooting.md**
- Update onboarding documentation for new developers

#### 6.2: Prepare Deployment Strategy
Plan and execute the deployment of the migrated codebase.
- Create deployment checklist with rollback procedures
- Plan staged deployment if possible (feature flags, gradual rollout)
- Prepare monitoring and alerting for post-deployment issues
- Document any environment-specific configuration changes needed
- Schedule deployment during low-traffic periods if possible

## Manual testing plan
- Verify all major user workflows function correctly with v5 components
- Test responsive design and mobile compatibility across different screen sizes
- Validate theme consistency and custom styling throughout the application
- Check accessibility features and keyboard navigation still work properly
- Test form components including validation, submission, and error handling
- Verify date pickers, autocomplete, and other complex components work as expected
- Test dark mode and theme switching functionality if implemented
- Validate print styles and any special CSS media queries still function
- Check browser compatibility across supported browsers and versions
- Perform visual regression testing comparing v4 and v5 rendered components