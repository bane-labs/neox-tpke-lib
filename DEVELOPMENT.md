# Development Guide

This guide provides comprehensive instructions for building, testing, and publishing the `neox-tpke` package.

## Project Overview

- **Package Name**: `neox-tpke`
- **Description**: Library for Threshold Public Key Encryption (TPKE) utilities in the NeoX ecosystem
- **Repository**: https://github.com/bane-labs/neox-tpke-lib.git

## Prerequisites

Before you begin, ensure you have the following installed:

- **Node.js**: Version 16 or higher
- **npm**: Version 7 or higher (comes with Node.js)
- **Git**: For version control

## Development Setup

### 1. Clone the Repository

```bash
git clone https://github.com/bane-labs/neox-tpke-lib.git
cd neox-tpke-lib
```

### 2. Install Dependencies

```bash
npm install
```

This will install all dependencies including:

**Runtime Dependencies:**

- `@noble/curves`: Elliptic curve cryptography
- `aes-js`: AES encryption implementation
- `bigint-crypto-utils`: BigInt cryptographic utilities
- `hash.js`: Hash functions
- `viem`: Ethereum interaction library

**Development Dependencies:**

- `prettier`: Code formatting
- `vite`: Build tool and development server
- `vite-plugin-dts`: TypeScript declaration file generation
- `vitest`: Testing framework
- `@types/aes-js`: TypeScript types for aes-js

## Development Workflow

### Available Scripts

- `npm run build` - Build the package for production
- `npm run dev` - Build the package in watch mode for development
- `npm run test` - Run the test suite
- `npm run format` - Format code using Prettier
- `npm run changeset` - Create a new changeset
- `npm run changeset:version` - Consume changesets and update version
- `npm run changeset:publish` - Publish packages to npm

### Development Mode

For active development with automatic rebuilding:

```bash
npm run dev
```

This will build the package and watch for changes, automatically rebuilding when source files are modified.

## Building the Package

### Build Process

The build process uses Vite and is configured to:

- Generate both UMD and ES Module formats
- Create TypeScript declaration files
- Generate source maps
- Keep code unminified for better debugging

### Build Command

```bash
npm run build
```

### Build Output

The build creates the following files in the `dist/` directory:

- `neox-tpke.umd.js` - UMD format for browser/Node.js
- `neox-tpke.mjs` - ES Module format
- `index.d.ts` - TypeScript type definitions
- Source map files for debugging

### Build Configuration

The build is configured in `vite.config.mts`:

- **Entry Point**: `src/index.ts`
- **Library Name**: `neox-tpke`
- **Source Maps**: Enabled
- **Minification**: Disabled for better debugging

## Testing

### Running Tests

Execute the complete test suite:

```bash
npm run test
```

### Test Configuration

Tests are configured using Vitest with:

- Global test functions enabled
- TypeScript support
- Fast execution and hot reloading during development

## Code Quality

### Formatting

Format all code according to project standards:

```bash
npm run format
```

The project uses Prettier with configuration in `prettier.config.js`.

### Pre-commit Checklist

Before committing changes:

1. Run tests: `npm run test`
2. Format code: `npm run format`
3. Build successfully: `npm run build`
4. Ensure no TypeScript errors
5. **Create changeset**: If your changes should be included in the next release:
   ```bash
   npm run changeset
   ```

## Release Management with Changesets

This project uses [Changesets](https://github.com/changesets/changesets) for version management and publishing. Changesets provides a structured workflow for tracking changes, generating changelogs, and publishing releases.

### Prerequisites for Publishing

1. **NPM Account**: Ensure you have an npm account with publish permissions
2. **Authentication**: Log in to npm:
   ```bash
   npm login
   ```
3. **Build Status**: Ensure the package builds successfully
4. **Tests Passing**: All tests must pass

### Development Workflow with Changesets

#### 1. Making Changes

When you make changes that should be included in the next release:

1. **Make your code changes** in feature branches as usual
2. **Create a changeset** to document the change:
   ```bash
   npm run changeset
   ```
3. **Follow the prompts**:
   - Select the type of change (patch/minor/major)
   - Provide a description of the change
   - This creates a markdown file in `.changeset/` folder

#### 2. Understanding Change Types

- **Patch** (1.0.0 → 1.0.1): Bug fixes, documentation updates
- **Minor** (1.0.0 → 1.1.0): New features, backward-compatible changes
- **Major** (1.0.0 → 2.0.0): Breaking changes

#### 3. Example Changeset Creation

```bash
$ npm run changeset
🦋  Which packages would you like to include?
● neox-tpke

🦋  Which type of change is this for neox-tpke?
○ patch   🩹  a bug fix
● minor   ✨  a new feature
○ major   💥  a breaking change

🦋  Please enter a summary for this change
Add support for new encryption algorithm

🦋  === Summary ===
- neox-tpke: minor
```

### Release Process

#### 1. Prepare for Release

When you're ready to publish accumulated changes:

```bash
# Update package versions and generate CHANGELOG
npm run changeset:version
```

This command:

- Consumes all changeset files
- Updates `package.json` version according to changes
- Updates `CHANGELOG.md` with release notes
- Deletes consumed changeset files

#### 2. Review Changes

- Review the updated `package.json` version
- Review the generated `CHANGELOG.md` entries
- Commit the version changes:
  ```bash
  git add .
  git commit -m "Version release"
  git push origin main
  ```

#### 3. Publish to NPM

```bash
# Build the package
npm run build

# Publish to NPM registry
npm run changeset:publish
```

This command:

- Publishes the package to npm
- Creates git tags for the new version
- Pushes tags to the repository

### Alternative: Manual Release Steps

If you prefer manual control over the publishing process:

1. **Prepare version**:

   ```bash
   npm run changeset:version
   git add . && git commit -m "Version release"
   ```

2. **Build and test**:

   ```bash
   npm run build
   npm run test
   ```

3. **Dry run** (recommended):

   ```bash
   npm publish --dry-run
   ```

4. **Publish**:

   ```bash
   npm publish
   ```

5. **Create and push git tag**:
   ```bash
   git tag v$(node -p "require('./package.json').version")
   git push origin main --tags
   ```

### Package Contents

Only the `dist/` directory is included in the published package, as specified in the `files` field of `package.json`. This includes:

- Built JavaScript files (UMD and ES modules)
- TypeScript declaration files
- Source maps

## Troubleshooting

### Common Issues

1. **Build Failures**:
   - Check TypeScript errors: `npx tsc --noEmit`
   - Ensure all dependencies are installed: `npm install`

2. **Test Failures**:
   - Run tests in verbose mode: `npm run test -- --reporter=verbose`
   - Check for missing test dependencies

3. **Publish Issues**:
   - Verify npm authentication: `npm whoami`
   - Check package name availability: `npm view neox-tpke`
   - Ensure proper permissions for the package

### Version Management

This project uses **Changesets** for automated [Semantic Versioning](https://semver.org/) (SemVer):

- **Patch** (x.x.X): Bug fixes, documentation updates, internal improvements
- **Minor** (x.X.x): New features, enhancements (backward compatible)
- **Major** (X.x.x): Breaking changes, API changes

#### How Changesets Determines Versions

- Each changeset specifies the type of change (patch/minor/major)
- When releasing, changesets aggregates all pending changes
- The highest change type determines the final version bump
- Example: If you have 5 patch changes and 1 minor change → minor version bump

#### Changeset Files

- Stored in `.changeset/` directory
- Markdown files with metadata and description
- Automatically consumed during version release
- Used to generate `CHANGELOG.md` entries

## Development Environment

### Recommended VS Code Extensions

- TypeScript and JavaScript Language Features
- Prettier - Code formatter
- Vitest - Test runner integration

### Git Workflow

1. Create feature branches from `main`
2. Make changes and test locally
3. Submit pull requests for review
4. Merge to `main` after approval
5. Tag releases and publish from `main`

## Support

For questions or issues:

- Check existing GitHub issues
- Create new issues for bugs or feature requests
