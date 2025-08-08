# Vuh Stencil Components Development Guidelines

The root directory of the component library is `core/`.

Write code in TypeScript using Stencil.js for building web components. Use SCSS for styling with the Sass plugin.

## Core Technologies

- **Stencil.js**: Main framework for building web components
- **TypeScript**: Primary language with JSX support
- **SCSS/Sass**: CSS preprocessing with global variables and mixins
- **Jest**: Testing framework for unit and spec tests
- **Puppeteer**: E2E testing
- **Storybook**: Component documentation and development environment

## Component Architecture

### Component Structure
- Use `@Component` decorator with `tag`, `styleUrl`, and `shadow: true`
- Export class with PascalCase naming (e.g., `Button`, `Input`)
- Tag names use kebab-case with `k-` prefix (e.g., `k-button`, `k-input`)

### Props and State Management
- Use `@Prop()` for component properties with TypeScript types
- Use `@State()` for internal component state
- Use `@Event()` and `EventEmitter` for custom events
- Use `@Watch()` decorators for prop change handlers
- Use `@Listen()` for event listeners
- Set `mutable: true` on props that need to be modified internally
- Set `reflect: true` on props that should reflect to attributes

### Component Patterns
```typescript
import { Component, h, Prop, State, Event, EventEmitter, Host } from '@stencil/core';
import clsx from 'clsx';

@Component({
  tag: 'k-component-name',
  styleUrl: 'k-component-name.scss',
  shadow: true
})
export class ComponentName {
  @Prop() variant?: 'primary' | 'secondary' = 'primary';
  @Prop({ reflect: true }) disabled?: boolean = false;
  @State() isActive: boolean = false;
  @Event() valueChange: EventEmitter;

  render() {
    return (
      <Host>
        <div class={clsx('KComponentName', this.variant, {
          'disabled': this.disabled,
          'active': this.isActive
        })}>
          <slot></slot>
        </div>
      </Host>
    );
  }
}
```

## Styling Guidelines

### SCSS Structure
- Use component-specific SCSS files co-located with components
- Follow the global SCSS structure: `variables.scss` → `global.scss` → component styles
- Use the `color()` function for accessing CSS custom properties: `color(primary-color)`
- Leverage global utility classes with `k-` prefix for common patterns

### CSS Class Naming
- Use PascalCase for main component classes (e.g., `KButton`, `KInput`)
- Use modifier classes for variants (e.g., `.primary`, `.secondary`, `.large`)
- Use `--` prefix for state classes (e.g., `--is-shown`, `--is-active`)
- Utilize `clsx` for conditional class application

### Variables and Colors
- Define SCSS variables in `src/styles/variables.scss`
- Export CSS custom properties in `src/styles/global.scss` with `--vuh-` prefix
- Use the color system: primary (with variants 100-800), gray (50-500), error, warning, success
- Access colors via the `color()` function: `background: color(primary-color)`

## File Organization

### Component Files
Each component should have:
- `k-component-name.tsx` - Main component file
- `k-component-name.scss` - Component styles
- `k-component-name.spec.ts` - Unit tests
- `k-component-name.e2e.ts` - E2E tests
- `readme.md` - Auto-generated documentation

### Directory Structure
```
core/
├── src/
│   ├── components/
│   │   └── k-component-name/
│   │       ├── k-component-name.tsx
│   │       ├── k-component-name.scss
│   │       ├── k-component-name.spec.ts
│   │       ├── k-component-name.e2e.ts
│   │       └── readme.md
│   ├── styles/
│   │   ├── global.scss
│   │   ├── variables.scss
│   │   ├── modules/
│   │   └── partials/
│   └── utils/
└── stencil.config.ts
```

## Testing Standards

### Unit Testing
- Use `newSpecPage` from `@stencil/core/testing`
- Test component rendering with different prop combinations
- Test event emission and user interactions
- Follow the pattern: describe component → test scenarios → expect HTML output

### E2E Testing
- Use `newE2EPage` from `@stencil/core/testing`
- Test user interactions and component behavior in browser environment
- Test accessibility and keyboard navigation

## Build and Development

### Scripts
- `npm start` - Development server with watch mode
- `npm run build` - Production build
- `npm run test` - Run all tests
- `npm run css.sass` - Compile SCSS to CSS
- `npm run storybook` - Start Storybook development server

### Configuration
- TypeScript configured for ES2017 target with DOM libraries
- JSX factory set to Stencil's `h` function
- Sass plugin with global path injection for variables and global styles
- React output target for generating React wrapper components

## Framework Integration

### React Integration
- Auto-generated React components via `@stencil/react-output-target`
- React wrappers available in `component-library-react/src/components.ts`
- Use `onInput` event in React for form components

### Vue Integration
- Direct web component usage in Vue applications
- Use `@input` event for form components in Vue

## Code Style and Standards

### TypeScript
- Use strict TypeScript with explicit types for all props and methods
- Enable `noUnusedLocals` and `noUnusedParameters`
- Use union types for prop variants (e.g., `'primary' | 'secondary'`)
- Provide default values for optional props

### Formatting
- Use consistent indentation and formatting
- Import Stencil core utilities first, then third-party libraries
- Group related imports together

### Accessibility
- Use semantic HTML elements where appropriate
- Implement proper ARIA attributes for complex components
- Ensure keyboard navigation support
- Test with screen readers when applicable

## Documentation

### Component Documentation
- Auto-generated README files with prop tables and examples
- Include usage examples for React and Vue
- Document component dependencies and relationships
- Use Storybook for interactive component documentation

### Naming Conventions
- Components: PascalCase class names (e.g., `Button`, `SearchBar`)
- Tags: kebab-case with `k-` prefix (e.g., `k-button`, `k-search-bar`)
- Props: camelCase (e.g., `validationState`, `helperText`)
- CSS classes: PascalCase for components, lowercase for modifiers
- Files: kebab-case matching the component tag name

When creating new components, follow the established patterns and ensure consistency with the existing component library architecture.
