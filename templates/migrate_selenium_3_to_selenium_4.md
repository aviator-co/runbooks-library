# Migrate Selenium 3 to Selenium 4
Upgrade existing Selenium WebDriver test automation framework from version 3.x to 4.x to leverage improved performance, enhanced features, and continued support.

## Summary of changes
- Current codebase uses deprecated Selenium 3.x APIs and dependencies that are no longer maintained
- Selenium 4 introduces breaking changes in WebDriver initialization, element location strategies, and browser management
- Migration requires updating dependencies, refactoring deprecated API calls, and adapting to new WebDriver manager patterns
- Enhanced features like relative locators, improved error handling, and better browser DevTools integration will be available post-migration

## Execution Steps

### Step 1: Assessment and Planning
Comprehensive analysis of current Selenium usage and impact assessment for the migration.

#### 1.1: Audit Current Selenium Usage
Create a comprehensive inventory of all Selenium-related code and dependencies in the codebase.
- Scan all **pom.xml**, **build.gradle**, **package.json**, or equivalent dependency files for Selenium-related dependencies
- Identify all classes and methods using Selenium WebDriver APIs across the codebase
- Document current browser driver management approach (ChromeDriver, FirefoxDriver, etc.)
- Create **docs/selenium-migration/current-usage-audit.md** with findings and affected file locations

#### 1.2: Identify Breaking Changes Impact
Analyze specific breaking changes that will affect the current implementation.
- Review usage of deprecated `DesiredCapabilities` class and document replacement needs
- Identify instances of `findElement()` and `findElements()` that may need updates
- Document current WebDriver initialization patterns and browser options configuration
- Catalog any custom WebDriver extensions or utilities that may be affected
- Update **docs/selenium-migration/breaking-changes-analysis.md** with detailed impact assessment

#### 1.3: Create Migration Strategy Document
Develop a detailed migration approach and rollback plan.
- Define testing strategy for validating functionality during migration
- Create compatibility matrix for supported browsers and versions
- Document rollback procedures in case of critical issues
- Establish success criteria and validation checkpoints
- Create **docs/selenium-migration/migration-strategy.md** with complete migration approach

### Step 2: Dependency Updates
Update all Selenium-related dependencies to version 4.x while maintaining backward compatibility where possible.

#### 2.1: Update Core Selenium Dependencies
Upgrade primary Selenium WebDriver dependencies to latest 4.x version.
- Update **selenium-java** dependency from 3.x to latest 4.x version in build configuration
- Update **selenium-server-standalone** if used for grid setup
- Add **selenium-devtools-v4** dependency for enhanced browser debugging capabilities
- Update any Selenium-related testing framework integrations (TestNG, JUnit)
- Run dependency resolution to ensure no conflicts exist

#### 2.2: Update WebDriver Management Dependencies
Modernize browser driver management approach using WebDriverManager or similar tools.
- Add **webdrivermanager** dependency if not already present for automatic driver management
- Update any existing driver management utilities to use Selenium 4 compatible versions
- Remove manual driver executable management code if WebDriverManager is adopted
- Update CI/CD pipeline configurations to handle new driver management approach

#### 2.3: Resolve Dependency Conflicts
Address any version conflicts introduced by the Selenium 4 upgrade.
- Run dependency tree analysis to identify conflicting transitive dependencies
- Update conflicting dependencies to compatible versions
- Add explicit version declarations for commonly conflicting libraries (Guava, Apache Commons)
- Verify all dependencies resolve correctly and build succeeds

### Step 3: WebDriver Initialization Refactoring
Migrate from deprecated DesiredCapabilities to modern Options-based WebDriver initialization.

#### 3.1: Refactor Chrome WebDriver Initialization
Update ChromeDriver initialization to use ChromeOptions instead of DesiredCapabilities.
- Replace `DesiredCapabilities.chrome()` with `new ChromeOptions()` in all WebDriver factory methods
- Migrate capability settings like `--headless`, `--disable-web-security` to ChromeOptions methods
- Update browser preference settings to use `ChromeOptions.setExperimentalOption()`
- Replace `new ChromeDriver(capabilities)` with `new ChromeDriver(chromeOptions)`
- Update corresponding test configuration files and property mappings

#### 3.2: Refactor Firefox WebDriver Initialization
Update FirefoxDriver initialization to use FirefoxOptions instead of DesiredCapabilities.
- Replace `DesiredCapabilities.firefox()` with `new FirefoxOptions()` in WebDriver factory methods
- Migrate Firefox profile settings to use `FirefoxOptions.setProfile()`
- Update binary path configuration to use `FirefoxOptions.setBinary()`
- Replace `new FirefoxDriver(capabilities)` with `new FirefoxDriver(firefoxOptions)`
- Update Firefox-specific test configurations and preferences

#### 3.3: Refactor Edge and Safari WebDriver Initialization
Update remaining browser drivers to use Options-based initialization pattern.
- Replace EdgeDriver DesiredCapabilities with `EdgeOptions` class usage
- Update Safari WebDriver initialization to use `SafariOptions` where applicable
- Migrate any Internet Explorer driver usage to Edge compatibility mode
- Standardize all WebDriver factory methods to use consistent Options pattern
- Update browser selection logic in test configuration utilities

### Step 4: Element Location and Interaction Updates
Update element location strategies and interaction methods to use Selenium 4 enhanced APIs.

#### 4.1: Update Element Location Strategies
Migrate to improved element location methods and add relative locator support.
- Review and optimize existing `By` locator strategies for better performance
- Replace any deprecated locator methods with Selenium 4 equivalents
- Implement relative locators (`RelativeLocator.with()`) for complex element relationships where beneficial
- Update page object model classes to leverage new locator capabilities
- Add utility methods for common relative locator patterns

#### 4.2: Enhance Wait Strategies
Implement improved explicit wait patterns using Selenium 4 enhancements.
- Update `WebDriverWait` usage to leverage improved expected conditions
- Replace deprecated `ExpectedConditions` methods with current alternatives
- Implement custom wait conditions using new lambda-based approaches
- Add timeout and polling interval optimizations for better test performance
- Update wait utility classes to use Selenium 4 best practices

#### 4.3: Update Element Interaction Methods
Modernize element interaction code to use enhanced Selenium 4 capabilities.
- Update action chains and complex user interactions using improved Actions API
- Implement new screenshot capabilities for elements and full page captures
- Add enhanced error handling using improved WebDriver exception hierarchy
- Update file upload mechanisms to use modern approaches
- Optimize element visibility and interactability checks

### Step 5: Browser DevTools Integration
Implement enhanced browser debugging and monitoring capabilities available in Selenium 4.

#### 5.1: Add DevTools Protocol Support
Integrate Chrome DevTools Protocol for advanced browser control and monitoring.
- Add DevTools session management in WebDriver factory classes
- Implement network traffic monitoring and manipulation capabilities
- Add performance metrics collection using DevTools Performance domain
- Create utility classes for common DevTools operations (console logs, network logs)
- Update test base classes to optionally enable DevTools features

#### 5.2: Implement Enhanced Logging
Leverage improved logging capabilities for better test debugging and monitoring.
- Configure enhanced WebDriver logging using new Selenium 4 logging options
- Implement browser console log capture and analysis
- Add network request/response logging for API testing integration
- Create log analysis utilities for automated test result enhancement
- Update CI/CD pipeline to collect and archive enhanced logs

#### 5.3: Add Browser Context Management
Implement improved browser context and session management features.
- Add support for multiple browser contexts within single WebDriver instance
- Implement cookie and local storage management using enhanced APIs
- Add browser window and tab management improvements
- Create utilities for cross-browser context testing scenarios
- Update test isolation strategies to leverage new context capabilities

### Step 6: Grid and Remote WebDriver Updates
Modernize Selenium Grid usage and remote WebDriver configurations for Selenium 4.

#### 6.1: Update Selenium Grid Configuration
Migrate to Selenium Grid 4 architecture and configuration approach.
- Update Grid hub and node configuration files to Selenium 4 format
- Replace deprecated Grid 3 command-line options with Grid 4 equivalents
- Update Docker configurations if using containerized Grid setup
- Implement new Grid 4 observability and monitoring features
- Update CI/CD pipeline Grid deployment scripts

#### 6.2: Refactor Remote WebDriver Usage
Update remote WebDriver initialization and capability management.
- Replace deprecated `RemoteWebDriver` constructor patterns with Options-based approach
- Update remote capability passing to use browser-specific Options classes
- Implement improved error handling for remote WebDriver connection issues
- Add retry logic and connection pooling for better remote execution reliability
- Update test configuration to support both local and remote execution seamlessly

#### 6.3: Enhance Parallel Execution Support
Implement improved parallel test execution capabilities using Selenium 4 features.
- Update thread-safe WebDriver management for parallel test execution
- Implement improved test isolation using enhanced WebDriver session management
- Add dynamic browser allocation and cleanup for parallel execution
- Update test reporting to handle parallel execution results properly
- Optimize resource usage and cleanup in parallel execution scenarios

### Step 7: Testing and Validation
Comprehensive testing of migrated functionality to ensure compatibility and performance.

#### 7.1: Execute Regression Test Suite
Run comprehensive test suite to validate migration success and identify any remaining issues.
- Execute full regression test suite against all supported browsers
- Validate test execution times and performance compared to Selenium 3 baseline
- Verify all existing test scenarios pass without modification
- Test parallel execution stability and resource usage
- Document any performance improvements or regressions observed

#### 7.2: Validate New Feature Integration
Test newly implemented Selenium 4 features to ensure proper integration and functionality.
- Validate relative locator functionality in representative test scenarios
- Test DevTools integration features and logging capabilities
- Verify enhanced wait strategies and error handling improvements
- Test Grid 4 functionality and remote execution stability
- Validate browser context management and session handling

#### 7.3: Performance and Stability Testing
Conduct extended testing to validate long-term stability and performance characteristics.
- Execute long-running test suites to validate memory usage and stability
- Test WebDriver cleanup and resource management under various scenarios
- Validate CI/CD pipeline integration and build stability
- Test error recovery and retry mechanisms under failure conditions
- Document performance benchmarks and comparison with Selenium 3 baseline

## Manual testing plan
- Execute a representative subset of automated tests manually across Chrome, Firefox, and Edge browsers
- Verify WebDriver initialization works correctly with various browser options and configurations
- Test relative locator functionality by manually validating complex element relationships
- Validate DevTools integration by checking console logs and network traffic capture during test execution
- Test Grid 4 functionality by running tests against remote WebDriver instances
- Verify enhanced error messages provide better debugging information compared to Selenium 3
- Test parallel execution by running multiple test classes simultaneously and verifying isolation
- Validate new screenshot capabilities by capturing element and full-page screenshots
- Test browser context management by switching between multiple tabs and windows
- Verify CI/CD pipeline integration by triggering builds and monitoring test execution logs