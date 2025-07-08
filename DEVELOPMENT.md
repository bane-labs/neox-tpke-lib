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

## Publishing to NPM

### Prerequisites for Publishing

1. **NPM Account**: Ensure you have an npm account with publish permissions
2. **Authentication**: Log in to npm:
   ```bash
   npm login
   ```
3. **Build Status**: Ensure the package builds successfully
4. **Tests Passing**: All tests must pass

### Pre-publish Steps

1. **Update Version**: Increment the version number in `package.json`

   ```bash
   # For patch releases (bug fixes)
   npm version patch

   # For minor releases (new features)
   npm version minor

   # For major releases (breaking changes)
   npm version major
   ```

2. **Update Changelog**: Document changes in [`CHANGELOG.md`](CHANGELOG.md)

3. **Final Build**: Create a clean production build

   ```bash
   npm run build
   ```

4. **Final Tests**: Ensure all tests pass
   ```bash
   npm run test
   ```

### Publishing Process

1. **Dry Run** (recommended): Test the publish process without actually publishing

   ```bash
   npm publish --dry-run
   ```

2. **Publish**: Release to npm registry
   ```bash
   npm publish
   ```

### Post-publish Steps

1. **Git Tag**: The `npm version` command automatically creates a git tag
2. **Push Changes**: Push the version bump and tags to repository

   ```bash
   git push origin main
   git push origin --tags
   ```

3. **GitHub Release**: Create a release on GitHub with the changelog

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

- Follow [Semantic Versioning](https://semver.org/) (SemVer)
- Patch (x.x.X): Bug fixes
- Minor (x.X.x): New features (backward compatible)
- Major (X.x.x): Breaking changes

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
