# Migrate Babel 6 to Babel 7
Upgrade the project's Babel configuration and dependencies from version 6 to version 7 to leverage improved performance, better preset handling, and enhanced plugin ecosystem.

## Summary of changes
- Babel 6 uses outdated preset and plugin naming conventions that need to be updated to scoped packages
- Configuration format has changed from .babelrc to babel.config.js with new syntax and options
- Several breaking changes in preset behavior, particularly for env and react presets
- Plugin and preset resolution has been improved but requires dependency updates
- Build tools integration may need adjustments for webpack, jest, and other tooling

## Execution Steps

### Step 1: Analysis and Planning
Preparation phase to understand current Babel usage and plan the migration strategy.

#### 1.1: Audit Current Babel Configuration
Document the existing Babel 6 setup to understand the scope of changes needed.
- Create **docs/babel-migration-audit.md** documenting current .babelrc configuration
- List all babel-related dependencies in package.json with their current versions
- Identify all build scripts and tools that use Babel (webpack, jest, npm scripts)
- Document any custom babel plugins or presets currently in use
- Note any babel-related configuration in webpack.config.js or other build files

#### 1.2: Create Migration Strategy Document
Plan the migration approach based on the audit findings.
- Create **docs/babel-7-migration-plan.md** with step-by-step upgrade strategy
- Document expected breaking changes based on current preset usage
- Identify which presets and plugins need to be replaced or updated
- Plan rollback strategy in case of issues during migration
- Document testing approach to verify functionality after each major change

### Step 2: Core Dependencies Update
Update Babel core packages and essential dependencies to version 7.

#### 2.1: Update Babel Core Packages
Replace Babel 6 core packages with their Babel 7 equivalents.
- Update **package.json** to replace `babel-core` with `@babel/core@^7.0.0`
- Replace `babel-cli` with `@babel/cli@^7.0.0` if present
- Update `babel-node` to `@babel/node@^7.0.0` if present
- Run `npm install` to install new core packages
- Verify installation by running `npx babel --version` to confirm Babel 7

#### 2.2: Update Babel Runtime Dependencies
Update runtime and helper packages to Babel 7 versions.
- Replace `babel-runtime` with `@babel/runtime@^7.0.0` in package.json
- Update `babel-polyfill` to `@babel/polyfill@^7.0.0` if present
- Replace any `babel-helper-*` packages with their `@babel/helper-*` equivalents
- Run `npm install` and verify no dependency conflicts exist
- Update any import statements in code that reference old babel-runtime paths

### Step 3: Presets Migration
Update all Babel presets to their Babel 7 equivalents with proper configuration.

#### 3.1: Update Environment Preset
Replace babel-preset-env with the new @babel/preset-env and update configuration.
- Replace `babel-preset-env` with `@babel/preset-env@^7.0.0` in package.json
- Update .babelrc to use `@babel/preset-env` instead of `env` or `babel-preset-env`
- Migrate any `useBuiltIns` configuration from `true` to `"usage"` or `"entry"`
- Add `corejs: 3` option if using `useBuiltIns` to specify core-js version
- Run `npm install` and test build to ensure preset works correctly

#### 3.2: Update React Preset
Migrate React preset to Babel 7 version with updated options.
- Replace `babel-preset-react` with `@babel/preset-react@^7.0.0` in package.json
- Update .babelrc to use `@babel/preset-react` instead of `react`
- Review and update any React-specific options like `development` mode
- Add `runtime: "automatic"` option if using React 17+ for new JSX transform
- Test React component compilation to ensure JSX transforms work correctly

#### 3.3: Update TypeScript Preset (if applicable)
Update TypeScript preset if the project uses TypeScript with Babel.
- Replace `babel-preset-typescript` with `@babel/preset-typescript@^7.0.0`
- Update .babelrc to use `@babel/preset-typescript` instead of `typescript`
- Verify `allowNamespaces` and `onlyRemoveTypeImports` options if needed
- Test TypeScript file compilation to ensure type stripping works correctly
- Update any tsconfig.json settings that might conflict with Babel TypeScript handling

### Step 4: Plugins Migration
Update all Babel plugins to their Babel 7 scoped package versions.

#### 4.1: Update Transform Plugins
Replace commonly used transform plugins with their Babel 7 equivalents.
- Replace `babel-plugin-transform-object-rest-spread` with `@babel/plugin-proposal-object-rest-spread@^7.0.0`
- Update `babel-plugin-transform-class-properties` to `@babel/plugin-proposal-class-properties@^7.0.0`
- Replace `babel-plugin-transform-async-to-generator` with `@babel/plugin-transform-async-to-generator@^7.0.0`
- Update any other transform plugins to their `@babel/plugin-*` equivalents
- Run `npm install` and verify all plugins are properly installed

#### 4.2: Update Syntax Plugins
Replace syntax plugins with their Babel 7 versions.
- Update `babel-plugin-syntax-dynamic-import` to `@babel/plugin-syntax-dynamic-import@^7.0.0`
- Replace `babel-plugin-syntax-jsx` with `@babel/plugin-syntax-jsx@^7.0.0` if explicitly used
- Update any other syntax plugins to use the `@babel/plugin-syntax-*` naming
- Remove any syntax plugins that are now included by default in presets
- Test that all syntax features still parse correctly after plugin updates

#### 4.3: Update Third-Party Plugins
Update community and third-party plugins that support Babel 7.
- Research Babel 7 compatibility for plugins like `babel-plugin-styled-components`
- Update plugin versions to their Babel 7 compatible releases
- Replace any plugins that don't support Babel 7 with alternatives
- Update plugin configuration syntax if changed between versions
- Test functionality of each third-party plugin after update

### Step 5: Configuration Migration
Convert Babel configuration files to the new Babel 7 format and structure.

#### 5.1: Create Babel 7 Configuration File
Replace .babelrc with babel.config.js using the new configuration format.
- Create **babel.config.js** in project root with module.exports structure
- Migrate all presets from .babelrc to the new file using full scoped names
- Convert all plugins to use the new `@babel/plugin-*` naming convention
- Add environment-specific configuration using the `env` key if needed
- Remove the old **.babelrc** file after successful migration

#### 5.2: Update Environment-Specific Configuration
Configure different Babel settings for development, production, and test environments.
- Add `env.development` configuration for development-specific plugins
- Configure `env.production` settings for production optimizations
- Set up `env.test` configuration for testing frameworks like Jest
- Ensure `NODE_ENV` environment variable properly switches configurations
- Test each environment configuration by running builds in different modes

#### 5.3: Update Build Tool Integration
Ensure webpack, Jest, and other build tools work with the new Babel configuration.
- Update **webpack.config.js** babel-loader options to work with Babel 7
- Modify **jest.config.js** or package.json jest configuration for Babel 7
- Update any gulp, rollup, or other build tool Babel integrations
- Remove any redundant Babel configuration now that babel.config.js is centralized
- Test all build processes to ensure they work with new configuration

### Step 6: Code Updates and Polyfills
Update application code and polyfill imports to work with Babel 7 changes.

#### 6.1: Update Polyfill Imports
Modify polyfill imports to use the new Babel 7 approach.
- Replace `import 'babel-polyfill'` with `import '@babel/polyfill'` in entry files
- Consider switching to `core-js/stable` and `regenerator-runtime/runtime` for better tree-shaking
- Update any manual polyfill imports to use core-js 3 paths if using `useBuiltIns: 'usage'`
- Remove redundant polyfill imports that are now automatically included
- Test polyfill functionality in target browsers to ensure compatibility

#### 6.2: Update Runtime Imports
Fix any direct imports from babel-runtime to use the new @babel/runtime structure.
- Replace `import 'babel-runtime/regenerator'` with `import '@babel/runtime/regenerator'`
- Update any `babel-runtime/helpers/*` imports to `@babel/runtime/helpers/*`
- Fix any dynamic imports or requires that reference the old babel-runtime paths
- Update any webpack aliases or module resolution that pointed to babel-runtime
- Test that all runtime helper functions work correctly with the new imports

### Step 7: Testing and Validation
Comprehensive testing to ensure the Babel 7 migration is successful and doesn't break functionality.

#### 7.1: Run Build and Test Suites
Execute all existing build processes and test suites to verify compatibility.
- Run `npm run build` or equivalent build command to test production builds
- Execute `npm test` to run the full test suite and ensure all tests pass
- Run development server to test hot reloading and development builds
- Check that all JavaScript and JSX files compile without errors
- Verify that source maps are generated correctly for debugging

#### 7.2: Browser Compatibility Testing
Test the compiled output in target browsers to ensure polyfills and transforms work correctly.
- Test application functionality in minimum supported browser versions
- Verify that async/await, class properties, and other transformed features work
- Check that polyfills are properly included for missing browser features
- Test bundle size to ensure it hasn't increased significantly
- Validate that tree-shaking still works effectively with the new configuration

#### 7.3: Performance Validation
Measure build performance and output quality compared to Babel 6.
- Compare build times between Babel 6 and Babel 7 configurations
- Measure bundle sizes for development and production builds
- Test that code splitting and dynamic imports still work correctly
- Verify that development rebuild times haven't regressed significantly
- Document any performance improvements or regressions in **docs/babel-7-migration-results.md**

## Manual testing plan
- Clone the repository and run `npm install` to verify all dependencies install correctly
- Execute `npm run build` and confirm the build completes without errors
- Run `npm test` to ensure all existing tests pass with the new Babel configuration
- Start the development server and verify hot reloading works correctly
- Test the application in Chrome, Firefox, Safari, and Edge to ensure cross-browser compatibility
- Check that async/await, class properties, JSX, and other transformed syntax work as expected
- Verify that polyfills are correctly applied by testing in older browsers or using browser dev tools to simulate older environments
- Compare bundle sizes before and after migration to ensure no significant increase
- Test source map generation by setting breakpoints in original source files during debugging