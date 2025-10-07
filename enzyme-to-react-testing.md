# Migrate from Enzyme to React Testing Library

Complete migration of existing React component tests from Enzyme to React Testing Library for improved testing practices and React 18+ compatibility.

## Summary of changes

- React Testing Library provides better testing practices by focusing on user behavior rather than implementation details
- Enzyme lacks official support for React 18+ and modern React features like hooks and concurrent rendering
- Migration involves replacing Enzyme's shallow/mount rendering with React Testing Library's render method
- Query methods change from Enzyme's find/prop assertions to React Testing Library's semantic queries
- Event simulation changes from Enzyme's simulate to React Testing Library's user-event library
- Setup and teardown logic needs to be updated for React Testing Library's cleanup approach

## Execution Steps

### Step 1: Environment Setup and Dependencies

Prepare the testing environment by installing React Testing Library and configuring the necessary dependencies.

### 1.1: Install React Testing Library Dependencies

Install the core React Testing Library packages and remove Enzyme dependencies gradually.

- Install `@testing-library/react`, `@testing-library/jest-dom`, and `@testing-library/user-event` as dev dependencies
- Keep Enzyme dependencies temporarily to avoid breaking existing tests during migration
- Update package.json scripts if needed to include new testing library setup

### 1.2: Configure Testing Environment

Set up Jest configuration and test setup files for React Testing Library.

- Create or update **jest.config.js** to include React Testing Library setup
- Create or update **src/setupTests.js** to import `@testing-library/jest-dom/extend-expect`
- Configure Jest environment to use `jsdom` if not already configured
- Add React Testing Library cleanup in afterEach hooks in setup file

### 1.3: Create Migration Utilities

Develop helper functions and utilities to assist with the migration process.

- Create **src/test-utils.js** with custom render function that includes common providers (Router, Theme, etc.)
- Create migration helper functions for common Enzyme-to-RTL patterns
- Document common migration patterns in **TESTING_MIGRATION.md** for team reference

### Step 2: Test File Structure Migration

Update the basic structure and imports of test files to use React Testing Library.

### 2.1: Update Test File Imports

Replace Enzyme imports with React Testing Library imports in all test files.

- Replace `import { shallow, mount } from 'enzyme'` with `import { render, screen } from '@testing-library/react'`
- Add `import userEvent from '@testing-library/user-event'` for interaction testing
- Remove Enzyme Adapter imports and configuration
- Update custom test utility imports to use new **test-utils.js**

### 2.2: Convert Basic Rendering Patterns

Transform Enzyme's shallow and mount calls to React Testing Library's render method.

- Replace `shallow(<Component />)` with `render(<Component />)`
- Replace `mount(<Component />)` with `render(<Component />)`
- Update wrapper variable assignments to use destructured returns from render
- Remove Enzyme wrapper cleanup calls and rely on RTL's automatic cleanup

### 2.3: Update Test Describe Blocks and Structure

Ensure test structure follows React Testing Library best practices.

- Review and update test descriptions to focus on user behavior rather than implementation
- Group related tests logically using describe blocks
- Ensure each test is independent and doesn't rely on previous test state

### Step 3: Query Method Migration

Convert Enzyme's element selection methods to React Testing Library's semantic queries.

### 3.1: Convert Basic Element Queries

Replace Enzyme's find methods with React Testing Library's query methods.

- Replace `wrapper.find('button')` with `screen.getByRole('button')` or appropriate semantic query
- Replace `wrapper.find('.className')` with `screen.getByTestId()` after adding test-id attributes
- Replace `wrapper.find('#id')` with `screen.getById()` or semantic alternatives
- Convert `wrapper.findWhere()` to appropriate RTL queries based on the predicate logic

### 3.2: Convert Text and Content Queries

Transform text-based element finding to React Testing Library's text queries.

- Replace `wrapper.find({ text: 'content' })` with `screen.getByText('content')`
- Replace `wrapper.contains('text')` with `screen.getByText()` or `toHaveTextContent` assertion
- Convert prop-based searches to text or role-based queries where appropriate
- Update regex-based text searches to use RTL's regex support

### 3.3: Handle Complex Selectors

Convert complex Enzyme selectors to React Testing Library patterns.

- Replace chained selectors like `wrapper.find('div').find('button')` with single semantic queries
- Convert `wrapper.childAt()` and `wrapper.parent()` to container-based queries
- Replace `wrapper.first()` and `wrapper.last()` with `getAllBy*` queries and array indexing
- Update conditional element finding to use `queryBy*` methods that return null

### Step 4: Assertion Pattern Migration

Convert Enzyme assertions to React Testing Library and Jest DOM matcher patterns.

### 4.1: Convert Existence and Visibility Assertions

Transform Enzyme's existence checks to React Testing Library patterns.

- Replace `expect(wrapper.find('element')).toHaveLength(1)` with `expect(screen.getByRole()).toBeInTheDocument()`
- Replace `wrapper.exists()` with `screen.queryByRole()` and `toBeInTheDocument()` or `toBeNull()`
- Convert `wrapper.isEmptyRender()` to checking for empty container content
- Update visibility checks to use `toBeVisible()` matcher

### 4.2: Convert Property and State Assertions

Replace Enzyme's prop and state assertions with behavior-focused tests.

- Replace `wrapper.prop('propName')` with testing the rendered output that reflects the prop
- Remove direct state assertions `wrapper.state()` in favor of testing rendered behavior
- Convert `wrapper.hasClass()` to `toHaveClass()` matcher using screen queries
- Update attribute checks to use `toHaveAttribute()` with semantic queries

### 4.3: Convert Content and Value Assertions

Transform content-based assertions to React Testing Library patterns.

- Replace `wrapper.text()` with `toHaveTextContent()` matcher
- Replace `wrapper.html()` with container.innerHTML checks where necessary
- Convert form value assertions to use `toHaveValue()` and `toHaveDisplayValue()` matchers
- Update innerHTML comparisons to focus on user-visible content

### Step 5: Event Simulation Migration

Convert Enzyme's event simulation to React Testing Library's user-event library.

### 5.1: Convert Basic Click Events

Replace Enzyme's click simulation with user-event click methods.

- Replace `wrapper.find('button').simulate('click')` with `await userEvent.click(screen.getByRole('button'))`
- Add async/await to test functions that use user-event
- Update click event testing to use semantic button queries
- Convert custom click handlers to use user-event click with proper element targeting

### 5.2: Convert Form Interaction Events

Transform form-related event simulations to user-event methods.

- Replace `wrapper.find('input').simulate('change', { target: { value: 'text' }})` with `await userEvent.type(screen.getByRole('textbox'), 'text')`
- Convert form submission events to use `userEvent.click()` on submit buttons
- Replace select option changes with `userEvent.selectOptions()`
- Update checkbox and radio button interactions to use `userEvent.click()`

### 5.3: Convert Complex Event Interactions

Handle advanced event scenarios with user-event library.

- Replace keyboard event simulations with `userEvent.keyboard()` or specific key methods
- Convert mouse events like hover to `userEvent.hover()` and `userEvent.unhover()`
- Update focus/blur events to use `userEvent.click()` or `.focus()` methods
- Replace custom event dispatching with user-event equivalents or fireEvent as fallback

### Step 6: Async Testing Migration

Convert Enzyme's async testing patterns to React Testing Library's async utilities.

### 6.1: Convert Async State Updates

Replace Enzyme's update() calls with React Testing Library's waitFor utilities.

- Replace `wrapper.update()` after async operations with `await waitFor(() => expect(...).toBeInTheDocument())`
- Remove manual component update calls and rely on RTL's automatic re-rendering
- Convert `setImmediate` and `setTimeout` workarounds to `waitFor` patterns
- Update async prop changes to use `waitFor` for assertion timing

### 6.2: Convert Async Component Loading

Transform async component rendering tests to use React Testing Library patterns.

- Replace manual promise resolution waits with `waitFor` and `findBy*` queries
- Convert lazy-loaded component tests to use `await screen.findByText()` patterns
- Update API call mocking to work with RTL's async testing approach
- Convert setTimeout-based async tests to use `waitFor` with appropriate timeout configs

### 6.3: Handle Error Boundary and Loading States

Update tests for async error and loading states using React Testing Library patterns.

- Convert loading state tests to use `queryBy*` and `findBy*` queries appropriately
- Update error boundary tests to use `waitFor` for error state assertions
- Replace manual error throwing with proper async error simulation
- Convert retry logic tests to use `waitFor` with timeout and interval options

### Step 7: Custom Hook and Context Testing

Migrate tests that involve React hooks and context using React Testing Library's specialized utilities.

### 7.1: Convert Custom Hook Tests

Replace Enzyme-based hook testing with React Testing Library's renderHook utility.

- Install and import `@testing-library/react-hooks` if using React < 18, or use `renderHook` from `@testing-library/react`
- Replace shallow wrapper hook testing with `renderHook(() => useCustomHook())`
- Update hook result assertions to use `result.current` property
- Convert hook re-rendering tests to use `rerender` function from renderHook

### 7.2: Convert Context Provider Tests

Transform context-related tests to use React Testing Library patterns.

- Replace Enzyme context prop drilling tests with rendered component behavior tests
- Update context provider wrapper tests to use custom render with provider wrapping
- Convert context consumer tests to focus on rendered output rather than context values
- Update context value change tests to use user interactions that trigger context updates

### 7.3: Convert HOC and Render Prop Tests

Update Higher-Order Component and render prop tests for React Testing Library.

- Replace HOC shallow rendering with full render testing of wrapped component behavior
- Convert render prop tests to focus on the rendered UI rather than prop function calls
- Update component composition tests to use semantic queries on the final rendered output
- Replace prop function spy assertions with behavior-based testing

### Step 8: Cleanup and Optimization

Remove Enzyme dependencies and optimize the test suite for React Testing Library.

### 8.1: Remove Enzyme Dependencies

Clean up Enzyme-related code and dependencies after migration is complete.

- Remove `enzyme`, `enzyme-adapter-react-*` packages from package.json
- Delete Enzyme configuration files and setup code
- Remove Enzyme import statements and unused test utilities
- Clean up any Enzyme-specific test helpers and utilities

### 8.2: Optimize Test Performance

Improve test performance using React Testing Library best practices.

- Review and optimize queries to use the most appropriate `getBy*`, `queryBy*`, or `findBy*` methods
- Add `screen.debug()` calls temporarily to debug failing tests, then remove
- Configure `@testing-library/jest-dom` matchers for better error messages
- Update test timeouts and async configurations for optimal performance

### 8.3: [NO-CODE-CHANGE] Update Documentation and Guidelines

Create comprehensive documentation for the new testing approach.

- Update **README.md** with React Testing Library testing guidelines and examples
- Create **TESTING_BEST_PRACTICES.md** with team standards for RTL usage
- Document common patterns and anti-patterns in **TESTING_PATTERNS.md**
- Update CI/CD documentation to reflect any changes in test running or reporting

## Manual testing plan

1. **Run Full Test Suite**: Execute `npm test` to ensure all migrated tests pass without errors
2. **Verify Test Coverage**: Run `npm run test:coverage` to confirm coverage metrics are maintained or improved
3. **Test Development Workflow**: Create a new component with RTL tests to verify the new testing setup works for ongoing development
4. **Browser Compatibility**: Run tests in different Node.js versions used in CI/CD pipeline to ensure compatibility
5. **Performance Testing**: Compare test execution time before and after migration to ensure performance is acceptable
6. **Integration Testing**: Run any existing integration or end-to-end tests to ensure they still pass with the new unit test setup
7. **IDE Integration**: Verify that test running and debugging works properly in the team's development IDEs
8. **CI/CD Verification**: Trigger a full CI/CD build to ensure the migration doesn't break automated testing pipeline