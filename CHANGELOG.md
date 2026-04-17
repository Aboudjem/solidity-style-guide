# Changelog

All notable changes to this Solidity Style Guide are documented here.
Format: [Keep a Changelog](https://keepachangelog.com/en/1.1.0/). Versioning: [SemVer](https://semver.org/).

## [2.0.0] - 2026-04-17

### Added

- **Editor and agent context files**: `AGENTS.md`, `CLAUDE.md`, `llms.txt`, `.cursorrules` so Claude Code, Cursor, Codex, and Aider can load this repo's rules automatically.
- **Claude Code skill** under `skill/` — `/solidity-style-guide:review` applies the full guide to any Solidity repo.
- **SEO / GEO polish** on `README.md`: structured FAQ, semantic headings, meta description, richer anchor text, quick-reference table.
- **Animated hero banner** at `assets/banner.svg`.
- **OSS hygiene**: `CONTRIBUTING.md`, `SECURITY.md`, `CODE_OF_CONDUCT.md`, PR + issue templates, `FUNDING.yml`.
- **Tooling configs** matching the guide: `.solhint.json`, `.prettierrc`, `.prettierignore`, `.editorconfig`, `.markdownlint.jsonc`, `.gitignore`, `package.json`.
- **CI**: GitHub Actions workflow running Prettier + markdownlint + markdown-link-check on every PR.

### Updated

- Rule references refreshed for **Solidity 0.8.34** (Pectra default EVM = Prague, transient storage stable, `require(cond, CustomError())` GA since 0.8.27 legacy / 0.8.26 via-ir).
- **ERC-7201** namespaced storage section expanded with the keccak formula and a full worked example.
- **Foundry** testing section updated to the current naming matrix: `test_Description`, `testFuzz_Description`, `test_RevertWhen_Description`, `testFork_Description`, `invariant_Property`.

## [1.0.0] - 2024

- Initial public release of the style guide.

[2.0.0]: https://github.com/Aboudjem/solidity-style-guide/releases/tag/v2.0.0
[1.0.0]: https://github.com/Aboudjem/solidity-style-guide/releases/tag/v1.0.0
