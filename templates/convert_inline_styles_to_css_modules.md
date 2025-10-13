# Convert Inline Styles to CSS Modules

Convert all inline styles across the codebase to CSS modules for better maintainability, performance, and separation of concerns.

## Summary of changes
- Audit existing inline styles across React components to understand scope and complexity
- Create CSS module files with equivalent styles and proper naming conventions
- Replace inline style objects with CSS module class references
- Update component imports to include CSS modules
- Ensure build system supports CSS modules compilation
- Maintain visual consistency and responsive behavior during conversion

## Execution Steps

### Step 1: Analysis and Setup
Prepare the codebase for CSS modules conversion by analyzing current inline styles and setting up necessary tooling.

#### 1.1: Audit Inline Styles Usage
Create a comprehensive inventory of all inline styles in the codebase to plan the conversion strategy.
- Scan all **React component files** (`.jsx`, `.tsx`) for `style={}` attributes and `styled-components`
- Document components with inline styles in **`docs/inline-styles-audit.md`** including file paths, component names, and complexity level
- Identify dynamic styles that depend on props or state variables
- Categorize styles by complexity: simple static styles, dynamic styles, and conditional styles
- Estimate effort required for each component conversion

#### 1.2: Configure CSS Modules Support
Ensure the build system properly supports CSS modules compilation and import resolution.
- Verify **webpack.config.js** or **vite.config.js** has CSS modules loader configured with `modules: true`
- Test CSS modules compilation by creating a sample **`test.module.css`** file and importing it
- Update **TypeScript configuration** in **`tsconfig.json`** to recognize `.module.css` imports if using TypeScript
- Install any missing dependencies like `css-loader` or CSS modules type definitions
- Create **`.css.d.ts`** type declaration files if needed for TypeScript support

#### 1.3: Establish CSS Modules Conventions
Define consistent naming and organization patterns for CSS modules across the project.
- Create **`docs/css-modules-guidelines.md`** with naming conventions for CSS classes and files
- Define file naming pattern (e.g., `ComponentName.module.css` alongside `ComponentName.jsx`)
- Establish CSS class naming conventions using camelCase or kebab-case consistently
- Document patterns for handling dynamic styles and CSS custom properties
- Create template files showing proper CSS modules structure and usage examples

### Step 2: Core Component Conversion
Convert components with inline styles to CSS modules, starting with simple cases and progressing to complex ones.

#### 2.1: Convert Simple Static Styles
Begin conversion with components that have only static inline styles without dynamic values.
- Identify components from audit with only static inline styles (no props/state dependencies)
- For each component, create corresponding **`.module.css`** file in the same directory
- Extract inline style objects to CSS classes with descriptive names
- Replace `style={}` attributes with `className={styles.className}` references
- Add CSS module import statement: `import styles from './ComponentName.module.css'`
- Test each converted component to ensure visual consistency

#### 2.2: Convert Dynamic Styles with CSS Custom Properties
Handle components with dynamic styles by leveraging CSS custom properties (CSS variables).
- Identify components with inline styles that depend on props or state
- Create **`.module.css`** files with CSS classes using custom properties for dynamic values
- Replace inline styles with `className` and `style` attributes setting CSS custom properties
- Update component logic to pass dynamic values as CSS custom properties: `style={{'--dynamic-color': color}}`
- Ensure fallback values are provided for CSS custom properties
- Test dynamic behavior with different prop values and state changes

#### 2.3: Convert Conditional and Complex Styles
Transform components with conditional styling logic and complex style compositions.
- Handle conditional styles using `classnames` utility library or template literals
- Install `classnames` package: `npm install classnames` and `npm install --save-dev @types/classnames`
- Create CSS classes for different style variants and states
- Replace conditional inline style logic with conditional class application
- Use `classnames()` to combine multiple CSS module classes based on conditions
- Test all style variants and edge cases to ensure proper visual rendering

### Step 3: Testing and Validation
Ensure all converted components maintain their original appearance and behavior through comprehensive testing.

#### 3.1: Visual Regression Testing
Verify that CSS modules conversion doesn't introduce visual changes or regressions.
- Take screenshots of all converted components in their various states before and after conversion
- Use visual regression testing tools like **Chromatic** or **Percy** if available
- Create **`tests/visual-regression-checklist.md`** documenting components to verify manually
- Test responsive behavior across different screen sizes and breakpoints
- Verify hover states, focus states, and other interactive style changes

#### 3.2: Update Component Tests
Modify existing tests to work with CSS modules and add new tests for style-related functionality.
- Update unit tests that rely on inline style assertions to check CSS classes instead
- Modify **Jest** or testing library selectors from style-based to class-based queries
- Add tests for dynamic CSS custom property application in components with dynamic styles
- Update snapshot tests to reflect new CSS module class names
- Ensure all existing component tests pass with CSS modules implementation

## Manual testing plan
- Navigate through the application and visually inspect all converted components
- Test responsive behavior by resizing browser window and checking mobile/tablet views
- Verify hover, focus, and active states work correctly on interactive elements
- Test dynamic styling by changing props/state that should affect component appearance
- Check browser developer tools to ensure CSS modules are generating unique class names
- Validate that no inline styles remain by searching for `style={}` in converted components
- Test in multiple browsers (Chrome, Firefox, Safari) to ensure cross-browser compatibility
- Verify that CSS modules are properly tree-shaken in production builds