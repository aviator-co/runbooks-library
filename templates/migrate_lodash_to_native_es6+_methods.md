# Migrate Lodash to native ES6+ methods

Replace Lodash utility functions with native JavaScript ES6+ equivalents to reduce bundle size and improve performance.

## Summary of changes
- Audit current Lodash usage across the codebase to identify migration candidates
- Replace commonly used Lodash methods with native ES6+ equivalents where performance and functionality are comparable
- Update build configuration to remove Lodash dependency and optimize bundle size
- Ensure backward compatibility and maintain existing functionality throughout the migration

## Execution Steps

### Step 1: Analysis and Planning
Comprehensive analysis of current Lodash usage and creation of migration strategy.

#### 1.1: Audit Lodash Usage
Create a comprehensive inventory of all Lodash functions currently used in the codebase.
- Search for all Lodash imports using `grep -r "import.*lodash" src/` and `grep -r "require.*lodash" src/`
- Search for Lodash method calls using `grep -r "_\." src/` and `grep -r "lodash\." src/`
- Create **lodash-audit.md** file in **docs/** directory documenting all found usages with file locations and frequency
- Categorize usages by Lodash method type (array, object, string, utility functions)

#### 1.2: Create Migration Plan
Develop a detailed migration strategy based on the audit findings.
- Research native ES6+ equivalents for each identified Lodash method
- Document performance implications and browser compatibility requirements in **lodash-migration-plan.md**
- Identify methods that cannot be easily replaced and require custom utility functions
- Create priority order for migration based on usage frequency and complexity
- Define rollback strategy for each migration phase

#### 1.3: Setup Migration Utilities
Create helper utilities and configuration for the migration process.
- Create **src/utils/native-helpers.js** with custom utility functions for complex Lodash replacements
- Add ESLint rules to **eslintrc.js** to prevent new Lodash usage during migration
- Update **package.json** scripts to include Lodash usage detection commands
- Create unit tests for custom utility functions in **src/utils/__tests__/native-helpers.test.js**

### Step 2: Array Method Migration
Replace Lodash array manipulation methods with native equivalents.

#### 2.1: Migrate Basic Array Methods
Replace commonly used Lodash array methods with native JavaScript equivalents.
- Replace `_.map()` with native `Array.prototype.map()`
- Replace `_.filter()` with native `Array.prototype.filter()`
- Replace `_.find()` with native `Array.prototype.find()`
- Replace `_.forEach()` with native `Array.prototype.forEach()` or for...of loops
- Update corresponding unit tests to verify functionality remains unchanged

#### 2.2: Migrate Complex Array Methods
Replace advanced Lodash array methods with native or custom implementations.
- Replace `_.uniq()` with `[...new Set(array)]` or custom deduplication logic
- Replace `_.flatten()` with `Array.prototype.flat()` where supported
- Replace `_.chunk()` with custom implementation in **native-helpers.js**
- Replace `_.groupBy()` with `Array.prototype.reduce()` based implementation
- Update all affected unit tests and integration tests

#### 2.3: Migrate Array Utility Methods
Replace remaining Lodash array utility methods.
- Replace `_.isEmpty()` for arrays with `array.length === 0`
- Replace `_.first()` and `_.last()` with array indexing `array[0]` and `array[array.length - 1]`
- Replace `_.compact()` with `array.filter(Boolean)`
- Replace `_.without()` with `array.filter()` based implementation
- Verify all array-related functionality through existing test suites

### Step 3: Object Method Migration
Replace Lodash object manipulation methods with native equivalents.

#### 3.1: Migrate Object Property Methods
Replace Lodash object property manipulation methods.
- Replace `_.get()` with optional chaining `?.` operator where possible, fallback to custom implementation
- Replace `_.set()` with direct assignment or custom implementation for nested paths
- Replace `_.has()` with `Object.prototype.hasOwnProperty()` or `in` operator
- Replace `_.omit()` with object destructuring and rest operator
- Update all object manipulation tests

#### 3.2: Migrate Object Iteration Methods
Replace Lodash object iteration methods with native equivalents.
- Replace `_.forOwn()` with `Object.keys().forEach()` or `for...in` loops
- Replace `_.mapValues()` with `Object.fromEntries()` and `Object.entries().map()`
- Replace `_.pickBy()` with `Object.fromEntries()` and `Object.entries().filter()`
- Replace `_.keys()` with native `Object.keys()`
- Verify object iteration functionality through comprehensive testing

#### 3.3: Migrate Object Utility Methods
Replace remaining Lodash object utility methods.
- Replace `_.clone()` with spread operator `{...obj}` for shallow cloning
- Replace `_.cloneDeep()` with `structuredClone()` or custom deep clone implementation
- Replace `_.merge()` with custom implementation using object spread and recursion
- Replace `_.isEmpty()` for objects with `Object.keys(obj).length === 0`
- Update all object utility tests and edge case handling

### Step 4: String and Utility Method Migration
Replace Lodash string manipulation and utility methods.

#### 4.1: Migrate String Methods
Replace Lodash string manipulation methods with native equivalents.
- Replace `_.camelCase()` with custom implementation in **native-helpers.js**
- Replace `_.kebabCase()` with custom implementation using regex and string methods
- Replace `_.startsWith()` and `_.endsWith()` with native `String.prototype.startsWith()` and `endsWith()`
- Replace `_.trim()` with native `String.prototype.trim()`
- Create comprehensive tests for custom string utility functions

#### 4.2: Migrate Utility Functions
Replace remaining Lodash utility functions.
- Replace `_.isArray()` with native `Array.isArray()`
- Replace `_.isObject()` with `typeof obj === 'object' && obj !== null`
- Replace `_.isString()`, `_.isNumber()`, `_.isBoolean()` with native `typeof` checks
- Replace `_.debounce()` and `_.throttle()` with custom implementations
- Ensure all utility function replacements maintain exact same behavior

#### 4.3: Handle Edge Cases and Performance
Address edge cases and performance considerations for migrated methods.
- Review and optimize custom implementations for performance
- Add comprehensive error handling to match Lodash behavior
- Test edge cases like null/undefined inputs, empty arrays/objects
- Benchmark critical path functions to ensure no performance regression
- Update documentation for any behavior changes

### Step 5: Dependency Cleanup and Optimization
Remove Lodash dependency and optimize the build configuration.

#### 5.1: Remove Lodash Dependency
Clean up Lodash references and dependencies from the project.
- Remove Lodash from **package.json** dependencies
- Remove all Lodash import statements from source files
- Update **webpack.config.js** or build configuration to remove Lodash-specific optimizations
- Run `npm install` to update **package-lock.json**
- Verify no remaining Lodash references using grep searches

#### 5.2: Update Build and Linting Configuration
Update project configuration to prevent future Lodash usage.
- Add ESLint rules to **eslintrc.js** to disallow Lodash imports
- Update **tsconfig.json** if using TypeScript to remove Lodash type definitions
- Update bundle analyzer configuration to verify bundle size reduction
- Add pre-commit hooks to detect accidental Lodash usage
- Update CI/CD pipeline to include Lodash usage checks

#### 5.3: Documentation and Team Communication
Update project documentation and communicate changes to the development team.
- Update **README.md** with information about the migration and new utility functions
- Create **MIGRATION.md** documenting the changes and new patterns to use
- Update code style guide to include preferred native JavaScript patterns
- Document custom utility functions in **src/utils/README.md**
- Schedule team training session on new patterns and utility functions

## Manual testing plan
- Verify all existing functionality works correctly by running the full application
- Test critical user workflows to ensure no regressions were introduced
- Perform performance testing on key application features to verify no slowdowns
- Test edge cases manually, especially with empty/null data inputs
- Verify bundle size reduction using webpack-bundle-analyzer or similar tools
- Test application in different browsers to ensure ES6+ compatibility
- Validate that all custom utility functions behave identically to their Lodash counterparts
- Check that ESLint rules properly prevent new Lodash usage
- Verify that the application builds and deploys successfully without Lodash