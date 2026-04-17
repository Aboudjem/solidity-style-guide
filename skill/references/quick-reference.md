# Solidity Style — Quick Reference Card

One-page cheatsheet for agents. Full rules in [../SKILL.md](../SKILL.md) and the repo's [README.md](../../README.md).

## Identifier styles

```
Contract / Library / Interface / Struct / Enum / Event / Error   PascalCase
Interface                                                        IPascalCase
Function / Modifier / Variable / Argument                        camelCase
Public constant                                                  SNAKE_UPPER_CASE
Private / internal constant                                      _SNAKE_UPPER_CASE
Non-external function / state var                                _leadingCamelCase
Library first-arg custom-struct param                            self
Reserved-keyword collision                                       trailing_
```

## Pragma

```solidity
// Application contract
pragma solidity 0.8.30;

// Library / mixin
pragma solidity ^0.8.20;
```

## Imports

```solidity
import {Ownable} from "@openzeppelin/contracts/access/Ownable.sol";
import {SafeMath} from "@openzeppelin/contracts/math/SafeMath.sol";

import {Math} from "./Math.sol";
import {MyHelper} from "./helpers/MyHelper.sol";
```

## Layout

```
Pragma
Import
Events
Errors
Interfaces
Libraries
Contracts
  ├── Types
  ├── State vars
  ├── Events
  ├── Errors
  ├── Modifiers
  └── Functions
       ├── constructor
       ├── receive
       ├── fallback
       ├── external (view, pure last)
       ├── public
       ├── internal
       └── private
```

## Errors

```solidity
error InsufficientFunds(uint256 requested, uint256 available);

// ≥ 0.8.26 via-ir / ≥ 0.8.27 legacy
require(amount <= balance, InsufficientFunds(amount, balance));

// Any 0.8.4+
if (amount > balance) revert InsufficientFunds(amount, balance);
```

## ERC-7201 namespaced storage

```solidity
/// @custom:storage-location erc7201:myapp.storage.Vault
struct VaultStorage {
    mapping(address account => uint256) balances;
    uint256 totalDeposited;
}

// keccak256(abi.encode(uint256(keccak256("myapp.storage.Vault")) - 1)) & ~bytes32(uint256(0xff))
bytes32 private constant _VAULT_STORAGE_SLOT =
    0x...; // precompute with `cast keccak` or compile-time tooling

function _getVaultStorage() private pure returns (VaultStorage storage $) {
    bytes32 slot = _VAULT_STORAGE_SLOT;
    assembly { $.slot := slot }
}
```

## Foundry test name matrix

```
test_Description
testFuzz_Description
test_RevertIf_Description
test_RevertWhen_Description
testFork_Description
testForkFuzz_RevertIf_Description
invariant_Property
```

## Gas one-liners

- `calldata` for read-only external params.
- Cache `arr.length` before the loop.
- `immutable` for constructor-set, `constant` for compile-time.
- Explicit `uint256` over smaller ints unless you're deliberately packing.
- Transient storage (EIP-1153) for per-tx flags on Cancun+.
- Checks → Effects → Interactions. Always.
