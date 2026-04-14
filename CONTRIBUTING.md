# Contributing to jade-translate

Thanks for your interest in improving zh-CN UI localization.

## How to contribute

### Report bad rules

If a rule produces unnatural Chinese in a real project, open an issue with:

- The original English string
- What jade-translate produced
- What a native speaker would write
- Which rule caused the issue

Real-world examples are the most valuable contribution.

### Propose new rules

Open an issue describing:

- The pattern you've noticed (with before/after examples)
- Why existing rules don't catch it
- Which products (飞书, 钉钉, Linear, etc.) demonstrate the correct pattern

### Expand the Common Fixes table

PRs that add new before/after examples are welcome. Each entry needs:

- The English source pattern
- The bad zh-CN translation (what Google Translate or ChatGPT produces)
- The good zh-CN localization (what a native speaker would write)
- Which rule(s) it demonstrates

### Fix bugs

If the skill misbehaves (wrong mode routing, broken interpolation handling, etc.), open an issue or PR.

## Development

The entire skill is one file: `SKILL.md`. Edit it directly.

### Testing changes

1. Copy your modified `SKILL.md` to `~/.claude/skills/jade-translate/SKILL.md`
2. Open Claude Code
3. Test both translate and lint modes against a real locale file

### Commit messages

Follow conventional commits: `feat:`, `fix:`, `docs:`, `chore:`.

## Code of conduct

Be kind. Be specific. Ship examples, not opinions.
