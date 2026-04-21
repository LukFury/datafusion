# Agent Guidelines for Apache DataFusion

## IUM — How to Navigate This Codebase

This project is indexed with IUM (Indexed Universal Matrix). The index lives at
`ium-out/index.db`. Always use IUM before reading raw files.

**Rules:**
- NEVER guess file paths or grep blindly. Use IUM first.
- For any symbol lookup: `ium find <name>` before opening any file
- For concept-based questions: `ium search "<concept>"` to get relevant symbol names, then `ium find` for coordinates
- Only read raw source AFTER IUM gives you the exact file:line
- After modifying any `.rs` files: run `ium scan .` to keep the index current

**Workflow:**
```bash
# Unknown concept → find relevant symbols
ium search "memory pressure spilling"

# Known symbol name → get exact location
ium find MemoryPool --suffix d          # where defined
ium find MemoryPool --suffix c          # everywhere called

# Then read only the lines you need
ium source MemoryPool                   # shows 30 lines at definition
```

**Commands:**
- `ium scan .` — reindex changed files
- `ium embed` — rebuild RAG vectors after a scan
- `ium find <name> [--suffix d|c|m|r]` — exact symbol lookup
- `ium search "<concept>"` — semantic search
- `ium inspect <name>` — symbol metadata
- `ium source <name>` — source lines at definition
- `ium stats` — index statistics

**Why this matters:** DataFusion is 764k lines across 1,506 files. IUM reduces
the token cost of navigation by ~99% — straight to the symbol, never read a
whole file blindly.

---

## Developer Documentation

- [Quick Start Setup](docs/source/contributor-guide/development_environment.md#quick-start)
- [Testing Quick Start](docs/source/contributor-guide/testing.md#testing-quick-start)
- [Before Submitting a PR](docs/source/contributor-guide/index.md#before-submitting-a-pr)
- [Contributor Guide](docs/source/contributor-guide/index.md)
- [Architecture Guide](docs/source/contributor-guide/architecture.md)

## Before Committing

Before committing any changes, you MUST follow the instructions in
[Before Submitting a PR](docs/source/contributor-guide/index.md#before-submitting-a-pr)
and ensure the required checks listed there pass. Do not commit code that
fails any of those checks.

At a minimum, you MUST run and fix any errors from these commands before
committing:

```bash
# Format code
cargo fmt --all

# Lint (must pass with no warnings)
cargo clippy --all-targets --all-features -- -D warnings
```

You can also run the full lint suite used by CI:

```bash
./dev/rust_lint.sh
# or auto-fix: ./dev/rust_lint.sh --write --allow-dirty
```

When creating a PR, you MUST follow the [PR template](.github/pull_request_template.md).

## Testing

See the [Testing Quick Start](docs/source/contributor-guide/testing.md#testing-quick-start)
for the recommended pre-PR test commands.
