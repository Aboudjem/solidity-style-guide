# Contributing

Thanks for helping improve the Solidity Style Guide. This doc is read by humans and by AI coding agents, so clarity and consistency matter a lot.

## Ways to contribute

- **New rule**: add a `####` section under the right category.
- **Update a rule**: cite the source (Solidity docs, EIP, audit report).
- **Fix a typo, dead link, or broken example**.
- **Propose tooling**: a Solhint rule, a Prettier option, a CI check.

## Rule template

Every rule must follow this shape:

````markdown
#### Rule Name

**Rule:** One-line statement of what to do.

Short rationale (1–3 sentences). Cite the source if non-obvious.

**✅ Yes:**

```solidity
// SPDX-License-Identifier: MIT
pragma solidity 0.8.30;

contract Good {
    // ...
}
```

**❌ No:**

```solidity
// SPDX-License-Identifier: MIT
pragma solidity 0.8.30;

contract Bad {
    // ...
}
```

> [!TIP]
> A one-sentence takeaway. Use `NOTE`, `IMPORTANT`, `WARNING`, `CAUTION` as appropriate.
````

## Workflow

1. Fork & branch: `feat/<short-name>` or `fix/<short-name>`.
2. Edit `README.md`. Update the table of contents if you add headings.
3. If you add a rule, mirror it in `skill/SKILL.md`.
4. Run `npm run check` (Markdown lint + Prettier).
5. Open a PR. Fill in the template. Small PRs review faster.

## What gets merged quickly

- Rules that cite an official source (Solidity docs, EIP, Solidity release notes, Foundry book, Coinbase/RareSkills/OpenZeppelin guides).
- Examples that match existing formatting exactly.
- Changes that don't break the existing TOC anchor links.

## What gets pushback

- New rules without examples.
- Personal-preference rules with no security / gas / readability rationale.
- Emoji-ifying sections that don't need it.
- Mega-PRs mixing unrelated changes.

## Review etiquette

- Be specific. "This conflicts with §Naming Conventions > Contract Names" beats "weird".
- Prefer suggestions with committable diffs.
- When two conventions are both defensible, pick the one already in use.

## Code of conduct

See [CODE_OF_CONDUCT.md](./CODE_OF_CONDUCT.md). Short version: be kind, stay on topic, no harassment.

## Security

Don't open a public issue for a flaw in a rule that could mislead developers into writing insecure code. See [SECURITY.md](./SECURITY.md).

## License

By contributing you agree that your contributions are licensed under the MIT License (see [LICENSE](./LICENSE)).
