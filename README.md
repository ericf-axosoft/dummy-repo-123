# UI Kit Components for GitKraken Desktop test

This repository contains the source code for GitKraken Desktop's UI Kit, a collection of reusable components designed to streamline development and ensure consistent user experiences. Built with modern web technologies, this kit provides a robust foundation for building high-quality interfaces in our flagship product.

We leverage TypeScript for type safety, React 19 for a performant UI, and Vite for rapid development. Comprehensive testing is achieved with Vitest and React Testing Library, including tools for testing custom hooks. Code quality is maintained through ESLint, while Storybook provides interactive component documentation and Playwright ensures end-to-end testing.

## Installation and Usage

The `@axosoft/gk-ui-kit` library is distributed as a private npm package. To use it in your React projects, you'll need to install it via pnpm (preferably), yarn or npm.

### Prerequisites:

- This library requires Node.js version 20+.
- This library requires pnpm version 9+.
- Ensure you have a valid `.npmrc` file configured with the necessary credentials to access our private npm registry.

### Installation:

```bash
pnpm install @axosoft/gk-ui-kit
```

### Usage:

Once installed, you can import and use the components directly in your React code:

```jsx
import { Button } from '@axosoft/gk-ui-kit';

function MyComponent() {
  return <Button>Click Me</Button>;
}
```

Refer to the Storybook documentation for a complete list of available components and their usage guidelines.

#### Importing and Using Icons

Icons are exposed as React components from `@axosoft/gk-ui-kit/icons`.

##### Usage:

```javascript
// Custom SVG icons (Gk prefix)
import { GkBranchIcon, GkPlusIcon } from '@axosoft/gk-ui-kit/icons';

// Font Awesome icons
import { CheckCircleIcon, ArrowDownIcon } from '@axosoft/gk-ui-kit/icons';
```

#### Customizing Color and Size

All icon components support the following props:

| Prop       | Type    | Description                                                           |
| ---------- | ------- | --------------------------------------------------------------------- |
| color      | Color   | Optional. A semantic color key from the `Color` type.                 |
| size       | Size    | Optional. A size key from your `Size` type.                           |
| title      | string  | Optional. Sets a title for accessibility purposes.                    |
| decorative | boolean | Optional. If true, hides icon from screen readers. Default is `true`. |

**Example**

```tsx
<GkCheckCircleIcon
  color="success"
  size="md"
  title="Success icon"
  decorative={false}
/>
```

The example above:

- Sets the icon color to the CSS variable defined by `colors.success`.
- Sets the size using the `md` size variant.
- Adds an accessible `<title>` for screen readers.

### Font Awesome Pro Icons

This library includes Font Awesome Pro icons which require a valid FA Pro license.

#### Prerequisites

1. **Font Awesome Pro License** - Your organization needs an active FA Pro subscription
2. **NPM Token** - Get your token from [Font Awesome account](https://fontawesome.com/account)

#### Setup .npmrc

Create or update `.npmrc` in your project root:

```
@fortawesome:registry=https://npm.fontawesome.com/
//npm.fontawesome.com/:_authToken=YOUR_FA_PRO_TOKEN
```

> **Note**: Never commit your `.npmrc` with the actual token to version control. Use environment variables or CI/CD secrets.

#### Peer Dependencies

Font Awesome icons require the following peer dependencies (use these exact versions for compatibility):

```bash
pnpm add @fortawesome/fontawesome-svg-core@1.2.36 @fortawesome/react-fontawesome@0.1.15
pnpm add @fortawesome/pro-solid-svg-icons@5.15.4 @fortawesome/pro-regular-svg-icons@5.15.4
pnpm add @fortawesome/pro-light-svg-icons@5.15.4 @fortawesome/pro-duotone-svg-icons@5.15.4
pnpm add @fortawesome/free-brands-svg-icons@5.15.4
```

#### Licensing

Font Awesome Pro icons require proper license attribution. Add the following to your software's license or credits:

```
This software uses Font Awesome Pro icons.
Font Awesome Pro License: https://fontawesome.com/license
```

Consult your FA Pro license agreement for specific attribution requirements.

## Contributing to the UI Kit

For all development-specific guidelines and contribution instructions, please refer to the `CONTRIBUTING.md` file. This document provides detailed information on setting up your development environment, coding standards, submitting pull requests, and more.

[Take me there!](./CONTRIBUTING.md)

---

&copy; GitKraken 2025
