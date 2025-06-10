# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

Continue is an open-source AI code assistant providing chat, autocomplete, edit, and agent functionality for VS Code and JetBrains IDEs. The architecture consists of:

- **`/core`** - Shared TypeScript library containing business logic for LLM interactions, context providers, indexing, and autocomplete
- **`/gui`** - React-based UI (Vite + Redux + Tailwind) used as webview in both IDE extensions  
- **`/extensions/vscode`** - VS Code extension (TypeScript)
- **`/extensions/intellij`** - JetBrains extension (Kotlin/Java + Gradle)
- **`/binary`** - Standalone core binary used by JetBrains extension
- **`/docs`** - Docusaurus documentation site

## Development Setup

**Node.js Requirements:** v20.19.0 (see `.nvmrc`)

**Initial Setup:**
```bash
# Install all dependencies across monorepo
./scripts/install-dependencies.sh    # Unix
.\scripts\install-dependencies.ps1   # Windows

# Or via VS Code task: Cmd+Shift+P -> "Tasks: Run Task" -> "install-all-dependencies"
```

## Common Development Commands

**TypeScript Watching (from root):**
```bash
npm run tsc:watch        # Watch all projects
npm run tsc:watch:core   # Watch core only
npm run tsc:watch:vscode # Watch VS Code extension only
npm run tsc:watch:gui    # Watch GUI only
```

**Core Development (`/core`):**
```bash
npm test                 # Run Jest tests
npm run test:coverage    # Tests with coverage
npm run tsc:check        # TypeScript type checking
npm run lint             # ESLint
npm run build:npm        # Build for npm publishing
```

**VS Code Extension (`/extensions/vscode`):**
```bash
npm run esbuild-watch    # Watch and bundle extension
npm run package          # Package as .vsix file
npm run test             # Run tests
npm run e2e:all          # E2E tests
```

**JetBrains Extension (`/extensions/intellij`):**
```bash
./gradlew runIde         # Run in debug mode
./gradlew buildPlugin    # Build plugin ZIP
./gradlew test           # Run tests
```

**GUI Development (`/gui`):**
```bash
npm run dev              # Vite dev server
npm run build            # Production build
npm run test             # Vitest tests
```

**Documentation (`/docs`):**
```bash
npm run start            # Start Docusaurus dev server
```

## Development Workflows

**VS Code Development:**
1. Run task: "install-all-dependencies"
2. Start debugging: Run and Debug → "Launch extension"
3. Changes auto-reload in host VS Code window

**JetBrains Development:**
1. `./scripts/install-dependencies.sh`
2. Open IntelliJ, run "Run Continue" configuration

**Architecture Patterns:**
- Core provides platform-agnostic business logic
- Extensions implement IDE-specific interfaces (`IDE`, `IContextProvider`)
- GUI communicates via JSON-RPC style messaging
- Extensible plugin system for context providers, slash commands, LLM providers

**Key Files:**
- Configuration: `config.yaml` (new), `config.json` (legacy), `.continuerc.json` (workspace)
- Main entry points: `core/index.ts`, `extensions/vscode/src/extension.ts`
- Protocol definitions: `core/protocol/`

**Testing:**
- Unit: Jest (Core), Vitest (GUI)
- E2E: Custom frameworks for both IDE extensions
- Run tests before committing changes

**Build System:**
- TypeScript compilation with `tsc`
- ESBuild bundling (VS Code)
- Gradle (JetBrains)
- Vite (GUI)
- PKG (binary packaging)