---
name: solidity-style-guide
description: Apply the Aboudjem/solidity-style-guide rules to Solidity code — review, rewrite, or audit for naming, layout, formatting, NatSpec, custom errors, ERC-7201 storage, gas patterns, and Foundry test structure. Trigger when the user asks to "review Solidity style", "apply the style guide", "format .sol file", or mentions Solhint / Prettier / Foundry conventions in a Solidity repo.
version: 2.0.0
license: MIT
homepage: https://github.com/Aboudjem/solidity-style-guide
---

# Solidity Style Guide — Claude Code Skill

Use this skill whenever you are reviewing or writing Solidity, Foundry tests, or related configuration. It codifies the rules in [Aboudjem/solidity-style-guide](https://github.com/Aboudjem/solidity-style-guide) targeting **Solidity 0.8.34**.

## When to invoke

Trigger automatically on:

- `.sol` files in the workspace
- `foundry.toml`, `remappings.txt`, `.solhint.json` present
- User says: _review my contract_, _clean up this Solidity_, _apply the style guide_, _is this gas-optimal?_, _make this audit-ready_

## Output contract

1. **Produce a checklist** of violations grouped by category (layout → naming → formatting → NatSpec → best practices → tests → gas → security).
2. **Quote the offending snippet** with file path and line number.
3. **Propose a minimal diff** that makes it compliant. Preserve behavior; style changes only unless asked.
4. **Cite the rule** inline, e.g., `[§Best Practices > Using Custom Errors Over Require]`.
5. **Stop before applying** unless the user asked for an auto-fix.

## Hard rules (block the review on violation)

### Files & layout

- SPDX license identifier on every `.sol` file
- One contract / interface / library per file; filename matches the core type
- Top-level order: pragmas → imports → events → errors → interfaces → libraries → contracts
- Inside a contract: type declarations → state vars → events → errors → modifiers → functions
- Function order: `constructor` → `receive` → `fallback` → `external` → `public` → `internal` → `private`
- Within each visibility group, put `view` then `pure` last

### Imports

- **Named imports only**: `import {X} from "./X.sol";`
- Sort by path length within groups; blank line between external and internal
- No wildcard imports

### Pragma

- Applications: pin to a single version — `pragma solidity 0.8.34;`
- Libraries / mixins: open range — `pragma solidity ^0.8.20;`
- Never `^0.8.0 ^0.9.0` or other multi-constraint forms

### Types

- Always explicit: `uint256`, `int256`, `bytes1` — never `uint`, `int`, `byte`
- `bool` booleans, not `uint` flags
- Prefer `bytes` over `string` when non-human-readable

### Naming

| Element                                   | Style                       | Notes                   |
| ----------------------------------------- | --------------------------- | ----------------------- |
| Contract / Library / Interface            | `PascalCase`                | Interfaces prefixed `I` |
| Struct / Event / Enum / custom Error      | `PascalCase`                | Events in past tense    |
| Function / modifier / variable / argument | `camelCase`                 |                         |
| Public constant                           | `SNAKE_UPPER_CASE`          |                         |
| Private / internal constant               | `_SNAKE_UPPER_CASE`         | Leading `_`             |
| Non-external fn / state var               | `_leadingUnderscore`        |                         |
| Avoid                                     | `l`, `O`, `I` single-letter | Visually ambiguous      |

### Formatting

- 4-space indent, spaces (no tabs)
- Max line length **120** chars
- Strings in double quotes
- One space around binary operators; no space inside parens / brackets
- `mapping(K => V)` — no space after `mapping`
- `uint[]` — no space before `[]`
- Braces on same line as declaration; `else` on the same line as the closing brace
- Long signature: each arg on its own line, closing `)` and `{` on their own lines

### Best practices

- Prefer custom errors. Use `require(cond, CustomError(arg1, arg2))` when on Solidity ≥ 0.8.26 (via-ir) or ≥ 0.8.27 (legacy). Otherwise `if (!cond) revert CustomError(...);`.
- If a `require` string message is unavoidable, keep it under 32 bytes.
- `calldata` for read-only array / struct params on `external` functions.
- Cache `.length` before the loop header.
- Prefer **named returns** for functions with ≥ 2 return values.
- Prefer **named arguments** in calls, events, errors with ≥ 3 parameters.
- **Named parameters in mappings**: `mapping(address user => mapping(address asset => uint256 amount))`.
- Interact with external contracts through typed interfaces; if using `call`, use `abi.encodeWithSelector`.
- For upgradeable contracts, use **ERC-7201 namespaced storage** with `@custom:storage-location erc7201:<namespace>`.
- Events in past tense: `OwnerUpdated`, `FundsDeposited`.
- Avoid inline assembly unless there is no high-level alternative; when used, document every opcode.
- Prefer composition over inheritance.

### NatSpec

- `@title`, `@author`, `@notice`, `@dev`, `@param`, `@return` on every public / external function
- Internal / library functions used by other devs: also NatSpec

### Foundry testing conventions

- File naming: `MyContract.sol` ↔ `MyContract.t.sol`; split large: `MyContract.owner.t.sol`, `MyContract.deposits.t.sol`
- Test names: `test_Description`, `testFuzz_Description`, `test_RevertWhen_Condition`, `test_RevertIf_Condition`, `testFork_Description`, `testForkFuzz_RevertIf_Condition`
- Invariants: `invariant_Property` (e.g., `invariant_TotalSupplyEqualsSumOfBalances`)
- No assertions inside `setUp`; use a dedicated `test_SetUpState`
- Verbose assertion messages: `assertEq(x, y, "Invariant violated: …")`

### Gas patterns

- Mark values set once as `immutable`; mark compile-time values as `constant`
- Prefer `uint256` over smaller ints unless packing is intentional
- Hoist array length out of loops; use `unchecked { ++i; }` where safe (Solidity ≥ 0.8.22 already optimizes simple `++i` increments, but `unchecked` still helps for custom counters)
- Use transient storage (`tstore` / `tload`, EIP-1153) for per-transaction flags like reentrancy locks when the EVM target supports it (Cancun+). Solidity 0.8.34 default EVM is Prague.

### Security

- Checks → Effects → Interactions
- Reentrancy guard (mutex) around any external call that precedes state writes
- `call{value: x}("")` for Ether transfers; always check the returned `bool`
- No `tx.origin` for authorization
- Pull over push payment patterns where feasible

## Invocation recipes

### `/solidity-style-guide:review`

Scan the repo, produce a categorised violations report, **do not edit**.

### `/solidity-style-guide:review --fix`

Apply the minimal formatting and naming diffs (no logic changes) and print a summary. Behavior-changing rewrites remain advisory.

### `/solidity-style-guide:apply-config`

Copy the `.solhint.json`, `.prettierrc`, `.editorconfig`, and `.prettierignore` from [this repo](https://github.com/Aboudjem/solidity-style-guide) into the current project (do not overwrite if present without asking).

## References

- [README](../README.md) — full rule catalog with ✅ / ❌ examples
- [Solidity Style Guide (official)](https://docs.soliditylang.org/en/latest/style-guide.html)
- [Foundry Best Practices](https://book.getfoundry.sh/tutorials/best-practices)
- [ERC-7201 Namespaced Storage Layout](https://eips.ethereum.org/EIPS/eip-7201)
- [EIP-1153 Transient Storage Opcodes](https://eips.ethereum.org/EIPS/eip-1153)
- [Coinbase Solidity Style Guide](https://github.com/coinbase/solidity-style-guide)
- [RareSkills Solidity Style Guide](https://www.rareskills.io/post/solidity-style-guide)
