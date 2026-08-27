# codefix

## Description

Code quality skill that provides auto-linting, formatting, and code fixes through a unified CLI interface. Wraps existing tools like ESLint, Prettier, TypeScript compiler, etc.

## Category

Development Tools / Code Quality

## Commands

### fix
Fix code issues in file(s) or directory.

Options:
- `--type <lint|format|types|all>` - Type of fix (default: all)
- `--write` - Write fixes to files
- `--diff` - Show diff of changes
- `--ignore <patterns>` - Patterns to ignore

### analyze / check
Analyze code without making changes.

Options:
- `--format <text|json|github>` - Output format
- `--errors-only` - Show only errors
- `--ignore <patterns>` - Patterns to ignore

## Workflow

When invoked, follow this process:

1. **Detect language/framework** from the target path (TypeScript, JavaScript, CSS, etc.)
2. **Run the appropriate tools** based on what's configured in the project:
   - TypeScript: `npx tsc --noEmit` for type errors
   - ESLint: `npx eslint <path> --ext .ts,.tsx,.js,.jsx`
   - Prettier: `npx prettier --check <path>` or `--write` to fix
   - CSS/SCSS: `npx stylelint <path>`
3. **Collect all issues** across tools
4. **Apply fixes** where `--write` is specified (safe auto-fixes only)
5. **Report results** in the requested format

## Project-Specific Config (alsherief-tech-canvas)

This project uses:
- **TypeScript** (strict mode via `tsconfig.json`)
- **ESLint** with React/TypeScript rules
- **Prettier** for formatting
- **Tailwind CSS** class ordering (if `prettier-plugin-tailwindcss` installed)

### Quick commands for this project:
```bash
# Check all TypeScript errors in frontend
cd frontend && npx tsc --noEmit

# Check ESLint issues
cd frontend && npx eslint src/ --ext .ts,.tsx

# Fix formatting
cd frontend && npx prettier --write src/

# Check backend TypeScript
cd backend && npx tsc --noEmit
```

## Integration

1. **CI Pipeline**: Run analysis in GitHub Actions
2. **Pre-commit Hook**: Auto-fix files before commit
3. **Manual Fix**: Fix specific files or directories on demand
4. **Code Review**: Analyze and report issues in text/JSON format

## Supported Languages

- TypeScript (.ts, .tsx)
- JavaScript (.js, .jsx)
- JSON (.json)
- YAML (.yaml, .yml)
- Markdown (.md)
- CSS/SCSS (.css, .scss)
