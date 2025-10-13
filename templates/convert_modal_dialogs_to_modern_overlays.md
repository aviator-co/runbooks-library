# Convert Modal Dialogs to Modern Overlays
Modernize the application's modal dialog system by replacing legacy modal implementations with a new overlay component system that provides better accessibility, performance, and user experience.

## Summary of changes
- Legacy modal dialogs are scattered throughout the codebase using inconsistent implementations and outdated patterns
- Create a new modern overlay system with improved accessibility features, better positioning logic, and consistent styling
- Systematically replace all existing modal dialogs with the new overlay components while maintaining backward compatibility
- Implement proper focus management, keyboard navigation, and ARIA compliance in the new overlay system

## Execution Steps

### Step 1: Analysis and Foundation
Establish the groundwork for the modal-to-overlay conversion by analyzing existing implementations and creating the new overlay infrastructure.

#### 1.1: Audit Existing Modal Implementations
Conduct a comprehensive audit of all modal dialog implementations currently in use across the application.
- Scan the codebase for modal-related components, classes, and usage patterns using grep/search tools
- Document all existing modal types, their props/configurations, and usage locations in **modal-audit.md**
- Identify common patterns, inconsistencies, and technical debt in current modal implementations
- Create a mapping document showing which modals will be converted to which overlay types

#### 1.2: Design New Overlay Component Architecture
Design the architecture for the modern overlay system that will replace existing modals.
- Create **overlay-system-design.md** documenting the new component hierarchy and API design
- Define overlay types (dialog, drawer, popover, tooltip) and their specific use cases
- Specify accessibility requirements including focus management, keyboard navigation, and ARIA attributes
- Design the overlay positioning system with support for different anchor points and collision detection

#### 1.3: Create Base Overlay Infrastructure
Implement the foundational overlay system components and utilities.
- Create **src/components/overlay/OverlayProvider.tsx** with context for managing overlay state and z-index stacking
- Implement **src/components/overlay/OverlayPortal.tsx** for rendering overlays outside normal DOM flow
- Create **src/components/overlay/useOverlayPosition.ts** hook for dynamic positioning logic
- Add **src/components/overlay/overlayUtils.ts** with focus management and keyboard event handling utilities

### Step 2: Core Overlay Components
Build the primary overlay components that will replace different types of modal dialogs.

#### 2.1: Implement Base Overlay Component
Create the foundational overlay component with essential features for all overlay types.
- Implement **src/components/overlay/BaseOverlay.tsx** with backdrop handling, animation support, and event management
- Add proper focus trap functionality using focus management utilities
- Implement keyboard event handling for Escape key and tab navigation
- Create comprehensive test suite in **src/components/overlay/__tests__/BaseOverlay.test.tsx**

#### 2.2: Create Dialog Overlay Component
Build the dialog overlay component to replace standard modal dialogs.
- Implement **src/components/overlay/DialogOverlay.tsx** extending BaseOverlay with dialog-specific features
- Add support for title, content, and action button areas with proper ARIA labeling
- Implement size variants (small, medium, large, fullscreen) and positioning options
- Create test suite in **src/components/overlay/__tests__/DialogOverlay.test.tsx** covering all dialog scenarios

#### 2.3: Create Drawer Overlay Component
Build the drawer overlay component for side-panel style interactions.
- Implement **src/components/overlay/DrawerOverlay.tsx** with slide-in animations from different directions
- Add support for resizable drawers and different width/height configurations
- Implement proper backdrop behavior and close-on-outside-click functionality
- Create test suite in **src/components/overlay/__tests__/DrawerOverlay.test.tsx**

### Step 3: Migration Utilities and Compatibility
Create tools and compatibility layers to facilitate smooth migration from old modals to new overlays.

#### 3.1: Create Migration Helper Components
Build wrapper components that provide backward compatibility during the transition period.
- Create **src/components/overlay/LegacyModalWrapper.tsx** that wraps new overlays with old modal API
- Implement **src/components/overlay/ModalToOverlayAdapter.tsx** for automatic prop translation
- Add deprecation warnings and migration guidance in wrapper components
- Create migration utility functions in **src/components/overlay/migrationUtils.ts**

#### 3.2: Update Global Styles and Themes
Modify application styles to support the new overlay system while maintaining visual consistency.
- Update **src/styles/overlay.scss** with modern overlay styling including animations and responsive behavior
- Modify **src/styles/globals.scss** to include overlay-specific CSS custom properties and z-index management
- Update theme configuration files to include overlay-specific color tokens and spacing values
- Ensure existing modal styles don't conflict with new overlay styles

#### 3.3: Create Developer Migration Guide
Document the migration process and provide examples for developers converting existing modals.
- Create **docs/modal-to-overlay-migration.md** with step-by-step conversion instructions
- Include before/after code examples for common modal usage patterns
- Document breaking changes and how to handle them during migration
- Add troubleshooting section for common migration issues

### Step 4: Systematic Modal Replacement
Replace existing modal implementations with new overlay components in phases based on complexity and usage frequency.

#### 4.1: Convert Simple Confirmation Dialogs
Replace basic confirmation and alert dialogs with the new dialog overlay component.
- Identify and convert simple yes/no confirmation dialogs in **src/components/common/** directory
- Update **src/components/ConfirmationDialog.tsx** to use DialogOverlay instead of legacy modal
- Modify **src/components/AlertDialog.tsx** to use new overlay system
- Update all imports and usage of these components throughout the application

#### 4.2: Convert Complex Form Modals
Replace modal dialogs containing forms with appropriate overlay components.
- Convert user profile editing modal in **src/components/user/ProfileModal.tsx** to use DialogOverlay
- Update settings configuration modal in **src/components/settings/SettingsModal.tsx**
- Convert data entry forms in **src/components/forms/** directory from modals to drawer overlays where appropriate
- Ensure form validation and submission logic works correctly with new overlay components

#### 4.3: Convert Navigation and Menu Modals
Replace modal-based navigation and menu components with drawer overlays.
- Convert mobile navigation menu in **src/components/navigation/MobileMenu.tsx** to use DrawerOverlay
- Update sidebar modal in **src/components/layout/Sidebar.tsx** to use drawer overlay
- Convert filter panels in **src/components/filters/** directory to use appropriate overlay types
- Ensure responsive behavior works correctly across different screen sizes

### Step 5: Testing and Cleanup
Comprehensive testing of the new overlay system and removal of legacy modal code.

#### 5.1: Update Integration Tests
Modify existing integration tests to work with the new overlay system.
- Update end-to-end tests in **tests/e2e/** directory to interact with new overlay components
- Modify integration tests in **tests/integration/** to use new overlay selectors and interactions
- Update test utilities in **tests/utils/overlayHelpers.ts** to support new overlay testing patterns
- Ensure all existing test scenarios pass with new overlay implementations

#### 5.2: Performance and Accessibility Testing
Conduct thorough testing of performance and accessibility improvements.
- Run accessibility audit using automated tools and document results in **accessibility-test-results.md**
- Perform manual keyboard navigation testing across all overlay types
- Conduct performance testing comparing old modal vs new overlay rendering times
- Test overlay behavior with screen readers and other assistive technologies

#### 5.3: Remove Legacy Modal Code
Clean up legacy modal implementations and unused code after successful migration.
- Remove old modal components from **src/components/modals/** directory
- Delete unused modal-related CSS from **src/styles/modals.scss**
- Remove legacy modal utilities and helper functions
- Update package.json to remove any modal-specific dependencies that are no longer needed

## Manual testing plan
- Test each overlay type (dialog, drawer) across different browsers and devices to ensure consistent behavior
- Verify keyboard navigation works correctly with Tab, Shift+Tab, and Escape keys in all overlay scenarios
- Test overlay positioning and collision detection by opening overlays near screen edges and in scrollable containers
- Validate screen reader compatibility by testing with NVDA, JAWS, and VoiceOver across different overlay types
- Test overlay stacking behavior by opening multiple overlays simultaneously and verifying proper z-index management
- Verify responsive behavior by testing overlays on mobile, tablet, and desktop screen sizes
- Test overlay animations and transitions for smooth user experience across different performance conditions
- Validate form submission and data handling within overlays to ensure no functionality regression