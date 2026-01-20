# Contributing to the GitKraken Desktop UI Kit test dfghdfg

We're thrilled you're interested in contributing to the GitKraken Desktop UI Kit! This guide outlines everything you need to know to get started, from generating design tokens to developing components in Storybook, testing your code, ensuring it's properly linted, and building the entire library for deployment.

## Generating the Design Tokens Set

The UI Kit relies on TypeScript Design Tokens generated from our Figma file using the [Design Tokens plugin](https://github.com/lukasoppermann/design-tokens). These tokens are exported in JSON format by the plugin and used to compile TypeScript files that contain all the style information we use in our component primitives.

Developers are now expected to manually export the design tokens from the Figma plugin as a `.json` file.

**Steps to export the the design tokens from the Figma**

1. Ensure you have the right permissions and you have the plugin installed in Figma.
2. Got to the `Main menu` -> `Plugins` -> `Export Design Token file`.
3. Include types in export (e.g., `Colors`, `Figma Variables`).
4. Click the `Export` button.

Once exported, this file should be copied into the `/assets` folder in the root of the repository and named `design-tokens.tokens.json`.

After placing the file in the correct location, run the `pnpm build:theme-tokens` and `pnpm build:component-tokens` command to generate the corresponding TypeScript tokens. These generated files must then be committed and versioned as part of the repository.

This process is typically owned and executed by the product design team, but developers can re-generate the tokens as needed — as long as the JSON file follows the W3C Design Tokens specification.

## Developing Components and Pages with Storybook

> [!IMPORTANT]
> The GitKraken Desktop team maintains a [TypeScript and React Styleguide](https://gitkraken.atlassian.net/wiki/x/AYBVO) that all contributions should follow. Please make sure you review and understand the principles described there before submitting your contributions.

First of all run `pnpm storybook` for bootstrapping Storybook, our component development environment. This command launches an interactive web application that allows you to develop, test, and document UI components in isolation. It provides a live, hot-reloading environment, making it easy to iterate on component design and functionality.

### Where to Build Your Components

To maintain a clean and organized codebase, new components should be created within the `/src/components` directory. Each component should reside in its own dedicated folder, named identically to the component itself.

Inside each component folder, you should include the following files:

- `Component.tsx` (or `.jsx`): The main component file, containing the React component's implementation.
- `Component.types.ts`: Exported types of the component (e.g., for use in unit tests or storybooks).
- `Component.test.tsx`: The unit test file, using Vitest and React Testing Library to ensure the component's functionality.
- `Component.stories.tsx`: The Storybook file, providing interactive examples and documentation for the component.
- `Component.module.css` (or `Component.css`): The component-specific CSS file, using CSS modules for scoped styling.
- `index.ts`: A barrel file that re-exports all public members of the component, allowing for clean and consistent import statements.

#### Example:

For a `<Button />` component, the directory structure would look like this:

```
src/
  components/
    Button/
      Button.tsx
      Button.types.ts
      Button.test.tsx
      Button.stories.tsx
      Button.module.css
      index.ts
```

### Creating Integration Pages and Documentation

The `/src/pages` directory serves as a hub for both documentation and integration testing. It allows us to create comprehensive guides and demonstrate how different components work together in real-world scenarios.

The directory structure must follow these conventions:

- **MDX Files (Root of `/src/pages`)**
  - Place general documentation, development guidelines, and informational content directly within the root of the `/src/pages` directory as `.mdx` files.
  - These files can contain rich text, code examples, and embedded components, providing a flexible way to document the UI Kit.
- **Integration Pages (Subfolders):**
  - Create separate folders within `/src/pages` for integration-specific pages.
  - Each folder should contain `.stories.tsx` files that showcase how multiple components interact.
  - These pages are ideal for creating integration tests within Storybook, ensuring that components work seamlessly together.

MDX files provide a platform for creating detailed documentation, usage guides, and best practices, while Storybook stories within integration page folders enable the creation of visual and functional integration tests, validating the interplay between components. These _integration pages_ can serve as examples of how to build complex UI patterns using the UI Kit components.

## Icon Component System

### Adding New Icons

To add a new SVG icon and generate its corresponding React component, follow these steps:

#### Step 1: Place the SVG file

- Add your .svg file to the configured `SVG_FOLDER` directory (Defined in `build-icons.config.ts`, typically `src/components/icons/svg`):

```
gk-ui-kit/
└── src/
    └── components/
        └── icons/
            └── svg/
                └── my-new-icon.svg
```

- The filename should use kebab-case, e.g., `check-circle.svg`. This will be converted to a PascalCase component name: `CheckCircle`.

#### Step 2: Run the generator script

This script will:

- Convert the SVG to a React component using @svgr/core
- Generate:
  - `MyIcon.tsx` (component)
  - `MyIcon.stories.ts` (Storybook stories)
  - `MyIcon.test.tsx` (unit tests)

Run the script:

```
pnpm build:icons
```

You should see log output confirming that files were generated.

**NOTE:** These generated component files are not versioned. This means that the build will automatically generate them.

### Using Icons in Other UI Kit Components

Each icon is exported as a default React component from its file. You can import and use them like this:

```tsx
import CheckCircle from '../components/icons/CheckCircle';

export default function MyComponent() {
  return <CheckCircle />;
}
```

The generated icons API is the same as the one featured by the icons exposed publicly. Please refer to the [README](README.md) file for further guidelines.

## Committing your Changes

We enforce the [Conventional Commits specification](https://www.conventionalcommits.org/en/v1.0.0/) for all Git commit messages in this repository. This standard helps us automate **semantic versioning** for releases and generate accurate, automated **changelogs**. To ensure compliance, a pre-commit hook powered by Husky has been enabled. This hook will automatically audit your last commit message to validate that it adheres to the Conventional Commits schema. If your commit message doesn't meet the required format, the commit will be rejected, and you'll receive feedback on what needs to be corrected.

### How to Write Conventional Commit Messages

A Conventional Commit message generally follows this structure: `<type>[optional scope]: <description>`. This concise format ensures consistency and clarity.

The `type` is crucial and indicates the nature of your change, such as `feat` (new feature), `fix` (bug fix), `docs` (documentation), `style` (formatting), `refactor` (code restructuring), `perf` (performance improvement), `test` (testing changes), `build` (build system changes), `ci` (CI configuration), `chore` (other non-code changes), or `revert` (reverting a previous commit). An optional scope can be added in parentheses to provide more context, for example, `feat(button)` or `fix(storybook)`. The `description` is a mandatory, concise summary of the change, written in the imperative mood (e.g., "add," "fix") and ideally under 72 characters.

Optionally, you can include a more detailed body for complex changes, explaining the "why" and "how." Footers are used for referencing issues (e.g., `Closes #123`) or indicating `BREAKING CHANGE:` when a major change requires a new major version. By following these guidelines, your commits will be clear, easily searchable, and contribute to a more maintainable and well-documented UI kit.

## Testing Your Code

The `pnpm test` command executes all unit and integration tests defined within the project. We utilize [Vitest](https://vitest.dev/) and [React Testing Library](https://testing-library.com/docs/react-testing-library/intro/) to ensure the reliability and correctness of our components and custom hooks. Storybook stories can also contain tests that validate the proper integration of our components in higher-order compositions.

> [!NOTE]
> The `pnpm test` command executes a single test run. For an enhanced development experience, you can run tests in watch mode using `pnpm test:watch`. This command utilizes Vite to run all tests initially and subsequently re-runs only those test suites that have been modified since the last execution.

> [!WARNING]
> Please note that an additional `test:ci` command is available for handling headless Chromium updates for Playwright prior to conducting regular testing operations within the CI/CD pipeline. This command is reserved for our continuous integration and continuous delivery environment and should not be used for development purposes.

### Auditing Code Coverage

Similar to `pnpm test`, the `pnpm test:coverage` command triggers a single run of all tests and generates a comprehensive code coverage report. This report, presented as a static website, provides insights into the percentage of code covered by tests, helping to identify areas that require additional testing.

## Running the Linter

The `pnpm lint` command runs [ESLint](https://eslint.org/), a static code analysis tool that helps maintain code quality and consistency. ESLint enforces coding standards, identifies potential errors, and suggests improvements. We use it in combination with [typescript-eslint](https://typescript-eslint.io/) for refined code auditing of our TypeScript codebase. This ensures that our codebase remains clean, readable, and maintainable.

## Releasing new versions of the UI Kit

Releases of the GitKraken UI Kit (`@axosoft/gk-ui-kit`) are fully automated and driven by our Continuous Integration (CI) pipeline, powered by the `ci.yml` GitHub Action. This ensures a consistent, reliable, and hands-off release process.

### Automated Release Triggers

New versions of the UI Kit are released automatically whenever commits are merged into designated "release channels." Currently, these include the `main` branch for production releases and the `next` branch for pre-releases (e.g., 1.0.0-beta.1).

### CI Pipeline Steps

When code is merged into a release channel, our GitHub Actions CI pipeline kicks into gear. It first runs **code quality checks** (linting and Prettier formatting) and executes all **automated tests**. If everything passes, the UI Kit library is then built. For release channels, a semantic release automatically determines the next version based on Conventional Commits, and the built artifact is then **published to the npm registry** under `@axosoft/gk-ui-kit`.

### Important Considerations

Our release process is fully automated and cannot be triggered manually, removing human error. The **semantic versioning and changelog generation are directly derived from your Conventional Commits**, emphasizing the importance of following our commit message guidelines. This ensures that every time new, version-triggering code merges into a release channel, a thoroughly tested UI Kit package is automatically available on npm.

## Building the Library Manually

The `pnpm build` command compiles the entire UI Kit library into a production-ready bundle. It generates the JavaScript files, TypeScript declaration files (`main.d.ts`), and the compiled CSS styles, all packaged for distribution. This process ensures that the library is optimized for performance and ready for integration into other React applications.

### Building the UI Kit Gallery Website Manually

The `pnpm build:storybook` script generates a static HTML version of the Storybook application. This is essential for deploying the component documentation to a web server, making it accessible to team members, designers, and other stakeholders.

---

&copy; GitKraken 2025
