# Security Policy

## Scope

This repository is a style guide. It does **not** ship deployable contracts. "Security issues" here mean:

1. A rule that would lead developers to write **insecure** code.
2. A code example that compiles to a **vulnerable** pattern.
3. A link that redirects to a malicious resource.
4. Supply-chain risk in `package.json` devDependencies.

## Reporting

**Do not open a public issue for security-relevant corrections.** Instead:

- Email: `boudjemaa.adam@gmail.com`
- Or: open a private security advisory via GitHub's _Security → Advisories → Report a vulnerability_ on this repo.

Please include:

- Section / rule name
- Why the current guidance is unsafe (cite EIP / CVE / audit when relevant)
- Suggested replacement text
- Your contact + whether you want public credit

## Response targets

| Severity                                 | Triage | Fix         |
| ---------------------------------------- | ------ | ----------- |
| Critical (guidance leads to fund loss)   | 24h    | 72h         |
| High (guidance enables known vuln class) | 72h    | 1 week      |
| Medium / Low                             | 1 week | Best effort |

## Disclosure

We coordinate disclosure with reporters. Public acknowledgement is in `CHANGELOG.md` and the release notes unless you ask to stay anonymous.

## Out of scope

- Style disagreements without a security angle (open a regular PR).
- Typos (open a PR).
- Tool versions behind "latest" (open a PR).
