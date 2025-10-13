# Reduce npm install times
Optimize package installation performance by implementing caching strategies, dependency analysis, and build configuration improvements.

## Summary of changes
- Current npm install times are impacting developer productivity and CI/CD pipeline efficiency
- Implement multi-layered caching strategy including Docker layer caching and npm cache optimization
- Analyze and optimize package.json dependencies to remove unused packages and consolidate duplicates
- Configure build tools and CI systems to maximize cache utilization and parallel processing

## Execution Steps

### Step 1: Dependency Analysis and Audit
Analyze current dependency structure to identify optimization opportunities before implementing caching strategies.

#### 1.1: Create dependency analysis report
Generate comprehensive analysis of current package dependencies and their impact on install times.
- Run `npm ls --depth=0` and `npm ls --all` to document current dependency tree
- Use `npm audit` to identify security vulnerabilities that may require package updates
- Create **dependency-analysis.md** report documenting findings, duplicate dependencies, and unused packages
- Measure baseline npm install times across different environments (local, CI, fresh vs cached)

#### 1.2: Identify optimization opportunities
Analyze package.json and lock files to find immediate optimization targets.
- Review **package.json** devDependencies vs dependencies classification accuracy
- Identify packages that can be moved to devDependencies or removed entirely
- Document duplicate functionality packages (e.g., multiple date libraries, utility libraries)
- Create **optimization-targets.md** with prioritized list of changes and expected impact

### Step 2: Package Dependency Optimization
Clean up and optimize the dependency structure to reduce the number of packages that need to be installed.

#### 2.1: Remove unused dependencies
Eliminate packages that are no longer needed to reduce install overhead.
- Use `npx depcheck` to identify unused dependencies
- Remove identified unused packages from **package.json**
- Update **package-lock.json** by running `npm install`
- Verify build and tests pass with `npm run build` and `npm test`

#### 2.2: Consolidate duplicate functionality
Replace multiple packages that serve similar purposes with single, well-maintained alternatives.
- Replace duplicate utility libraries (lodash variants, date libraries) with single choices
- Update import statements throughout codebase to use consolidated packages
- Update **package.json** to remove old packages and add consolidated ones
- Run full test suite to ensure functionality remains intact

#### 2.3: Optimize dependency classification
Ensure packages are correctly classified as dependencies vs devDependencies for production builds.
- Move build-only tools (webpack, babel, testing frameworks) to devDependencies
- Move runtime-required packages to dependencies
- Update **package.json** with correct classifications
- Test production build with `npm ci --only=production` to verify classification

### Step 3: npm Configuration Optimization
Configure npm settings and package.json to optimize installation performance.

#### 3.1: Configure npm for performance
Optimize npm configuration settings for faster installs.
- Create **.npmrc** file with performance optimizations (registry settings, cache settings)
- Configure `prefer-offline=true` and `audit=false` for faster installs
- Set appropriate `cache-max` and `cache-min` values
- Test install performance improvements with clean cache

#### 3.2: Optimize package.json scripts
Streamline npm scripts to reduce unnecessary operations during install lifecycle.
- Review and optimize **package.json** scripts, especially `preinstall`, `postinstall` hooks
- Remove or optimize heavy postinstall scripts that can be moved to build time
- Configure `engines` field to specify supported Node.js versions
- Update scripts to use `npm ci` instead of `npm install` where appropriate

### Step 4: Caching Strategy Implementation
Implement comprehensive caching strategies for different environments.

#### 4.1: Implement Docker layer caching
Optimize Docker builds to maximize npm cache utilization.
- Update **Dockerfile** to copy **package*.json** files before other source code
- Structure Docker layers to cache npm install step separately from application code
- Add `.dockerignore` file to exclude unnecessary files from Docker context
- Test Docker build times with and without cache to measure improvements

#### 4.2: Configure CI/CD caching
Set up caching in continuous integration systems to reuse node_modules across builds.
- Update CI configuration files (**.github/workflows/**, **.gitlab-ci.yml**, etc.) to cache node_modules
- Configure cache keys based on package-lock.json hash for accurate cache invalidation
- Implement fallback cache strategies for partial cache hits
- Add cache statistics reporting to CI logs for monitoring

#### 4.3: Implement local development caching
Optimize local development environment for faster subsequent installs.
- Create **scripts/setup-dev-cache.sh** script to pre-populate npm cache
- Document npm cache location and management commands in **README.md**
- Configure IDE/editor settings to exclude node_modules from file watching
- Create **scripts/clean-install.sh** for troubleshooting cache issues

### Step 5: Alternative Package Manager Evaluation
Evaluate and potentially implement faster package managers as alternatives to npm.

#### 5.1: Evaluate yarn and pnpm performance
Compare alternative package managers against optimized npm setup.
- Install and benchmark yarn and pnpm with current project
- Create **package-manager-comparison.md** with performance metrics and feature comparison
- Test compatibility with existing scripts and CI/CD pipelines
- Document migration effort and potential risks for each alternative

#### 5.2: Implement chosen package manager
If evaluation shows significant benefits, implement the selected package manager.
- Add appropriate lock files (**yarn.lock** or **pnpm-lock.yaml**)
- Update **package.json** scripts to use new package manager commands
- Update **Dockerfile** and CI/CD configurations for new package manager
- Update documentation and developer setup guides
- Maintain npm compatibility during transition period

### Step 6: Monitoring and Documentation
Establish monitoring and documentation to maintain optimized install performance.

#### 6.1: Implement performance monitoring
Create tools and processes to monitor npm install performance over time.
- Create **scripts/measure-install-time.sh** script for consistent performance measurement
- Add install time reporting to CI/CD pipelines
- Set up alerts for install time regressions above baseline thresholds
- Document performance baselines in **performance-metrics.md**

#### 6.2: Update development documentation
Ensure all optimization strategies are documented for team adoption.
- Update **README.md** with new installation instructions and performance tips
- Create **docs/npm-optimization-guide.md** with detailed optimization strategies
- Document troubleshooting steps for common cache and installation issues
- Add performance considerations to contribution guidelines

## Manual testing plan
- Measure npm install times in clean environment before and after each step to verify improvements
- Test fresh installs on different operating systems (Windows, macOS, Linux) to ensure cross-platform compatibility
- Verify that production builds work correctly with optimized dependencies using `npm ci --only=production`
- Test CI/CD pipeline performance with new caching strategies by triggering builds with and without cache
- Validate that development workflow remains smooth with new package manager or configuration changes
- Confirm that all existing npm scripts continue to work as expected after optimization changes
- Test Docker build performance improvements by building images with cold and warm caches