# CLAUDE.md

Claude Code reads this file first. Everything here also applies to Cursor, Codex, Aider, and similar agents — see [AGENTS.md](./AGENTS.md) for the canonical agent contract.

## TL;DR for Claude Code

You are working in a **Solidity style guide** documentation repo. The deliverable is `README.md` plus supporting docs and configs. There is no contract build step.

### When the user says "apply this guide" in another repo

Load the rules from `README.md` (or invoke the bundled skill in `skill/SKILL.md`) and review or rewrite the target `.sol` files to match. Report violations as a checklist before editing.

### When editing *this* repo

1. Keep `README.md` as the single canonical source.
2. Match existing formatting: H3 sections, `#### Rule Name`, `✅ Yes` / `❌ No` Solidity blocks, `> [!NOTE|TIP|IMPORTANT|WARNING|CAUTION]` callouts.
3. Update the table of contents when adding headings.
4. Run `npm run check` before claiming completion.
5. Use `/oh-my-claudecode:ccg` or the `oh-my-claudecode:code-reviewer` agent for a second opinion on substantive rule changes.

### Hard "never do this" list

- Don't collapse multiple rules into one.
- Don't remove the `✅ Yes` / `❌ No` pattern — the contrast is the pedagogy.
- Don't add rules that conflict with the official [Solidity style guide](https://docs.soliditylang.org/en/latest/style-guide.html) without citing a rationale.
- Don't introduce `require("string")` examples unless demonstrating the "avoid this" side.
- Don't add Hardhat-only examples. Foundry first.
- Don't bump dependencies casually. `package.json` is tooling-only; each bump should be justified in the PR.

### Preferred agents / skills

- **Review style edits**: `oh-my-claudecode:code-reviewer` (opus)
- **Verify examples compile conceptually**: `oh-my-claudecode:verifier`
- **Refactor README layout**: `oh-my-claudecode:designer` + `oh-my-claudecode:writer`
- **SEO / GEO polish**: `/seo-geo-mastery:optimize`
- **Humanize prose**: `/humanizer:humanizer`
- **Final launch audit**: `/repo-polish`

### What "done" looks like

- `npm run check` passes.
- Banner renders in the rendered README on GitHub.
- At least one `✅ Yes` and one `❌ No` Solidity block per `####` rule.
- `skill/SKILL.md` stays in sync with `README.md` rule titles.
- Git working tree clean; commits atomic.
