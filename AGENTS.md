# AGENTS.md

> A short context file for coding agents (Claude Code, Cursor, Codex, Aider, etc.) working in this repository.
> For humans: see [README.md](./README.md). For deep rule reference: see [docs/rules.md](./docs/rules.md) if present, else the README.

## What this repo is

A **Solidity style guide**: conventions, formatting, best practices, testing, gas, and security rules for modern Solidity (≥ 0.8.20, targeting **0.8.34**). It is documentation, not deployable code.

## Ground rules for agents editing this repo

1. **No emojis** in code examples. Emojis are fine in READMEs when they aid scanning, but avoid adding them where none exist.
2. **Do not shorten the guide.** Each rule is load-bearing; prefer adding a new rule section over collapsing two.
3. **Every rule must have a ✅ Yes / ❌ No Solidity example.** Single-example rules are incomplete.
4. **Max line length 120** (Solidity and Markdown prose). Indent Solidity with 4 spaces.
5. **Pragma in examples:** use `^0.8.20` for libraries and `0.8.34` (pinned) for application contracts, unless the rule is specifically about version pragmas.
6. **Prefer named imports.** `import {X} from "./X.sol";`. Never use bare `import "./X.sol";`.
7. **Prefer custom errors** over `require` strings. Use `require(cond, CustomError())` (Solidity ≥ 0.8.26) or `revert CustomError()`.
8. **When in doubt, cite a source** in the rule (Solidity docs, EIP, Foundry book, RareSkills, Coinbase guide).

## When asked to apply the style guide to a codebase

Treat the following as hard rules (block the PR on violation):

- SPDX license identifier present on every `.sol` file
- Single contract / interface / library per file
- Layout order: pragmas → imports → events → errors → interfaces → libraries → contracts
- Inside a contract: type declarations → state vars → events → errors → modifiers → functions
- Function order: constructor → receive → fallback → external → public → internal → private (view/pure last in each group)
- Interfaces prefixed `I` (e.g., `IToken`)
- Constants `SNAKE_UPPER_CASE`, private constants `_SNAKE_UPPER_CASE`
- Internal / private identifiers prefixed with `_`
- No `uint` / `int` / `byte` — always explicit `uint256`, `int256`, `bytes1`
- Use `calldata` for read-only array/struct parameters on external functions
- Cache `.length` outside the loop header
- For upgradeable contracts, use **ERC-7201** namespaced storage

Treat the following as warnings (suggest, don't block):

- Prefer named returns
- Prefer named arguments for calls with ≥ 3 args
- Named parameters in nested mappings

## Commands agents can run

This repo is docs-only, so there's no compile step. Quality gates:

```bash
# Verify Markdown
npx markdownlint-cli2 "**/*.md" "#node_modules"

# Verify formatting
npx prettier --check .

# Check links in top-level docs
npx markdown-link-check README.md AGENTS.md CONTRIBUTING.md SECURITY.md

# Everything
npm run check
```

## File map

```
/README.md                Main guide (SEO-optimised, animated banner)
/AGENTS.md                This file
/CLAUDE.md                Claude Code specific instructions
/llms.txt                 LLM discovery index
/.cursorrules             Cursor IDE rules mirror
/SECURITY.md              Disclosure policy
/CONTRIBUTING.md          PR workflow
/CODE_OF_CONDUCT.md       Community standards
/.solhint.json            Linter config matching this guide
/.prettierrc              Formatter config matching this guide
/.editorconfig            Editor-level defaults
/assets/banner.svg        Animated hero banner
/skill/                   Claude Code skill package (solidity-style-guide)
/.github/                 Issue / PR templates and CI
```

## Style for *edits to this guide*

- Add a new rule? Match the existing pattern: `#### Rule Name` → short rationale → `**✅ Yes:**` / `**❌ No:**` Solidity blocks → `> [!TIP|NOTE|IMPORTANT|WARNING|CAUTION]` callout.
- Update the table of contents if you add an `H3`/`H4`.
- Keep the tone practical. Cite the relevant EIP, Solidity release note, or audit finding where possible.

## Non-goals

- This repo does not ship example contracts to deploy.
- No Solidity toolchain (Foundry / Hardhat) is installed here — examples compile conceptually, not in CI.
- No breaking of the core doc into a docs site generator. Keep `README.md` as the canonical source.
