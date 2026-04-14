# Changelog

All notable changes to jade-translate will be documented in this file.

## [0.1.0.0] - 2026-04-14

### Added
- Lint mode: audit existing zh-CN translations for quality issues with structured table output and severity-based auto-fix
- Explicit mode selection prompt (Translate vs Audit) for clear routing
- Rule 10: interpolation variable preservation ({name}, {count}, ICU MessageFormat, HTML tags)
- Rule priority order for conflict resolution (R10 > R4 > R2 > R5 > R1 > R3)
- Scoring formula for lint reports (100 - 10/error - 5/warning - 2/suggestion)
- Standard Word Fixes section based on Ant Design's canonical word list
- Ant Design copywriting spec cited as foundation throughout
- TODOS.md with v2 roadmap (context-aware register matching, standalone CLI)

### Changed
- Rule 1 revised: keep 你 (Ant Design alignment), strip 您 always, strip unnecessary 的/了/被
- Common Fixes table expanded from 10 to 15 entries, each cross-referenced to its rule
- Restructured into shared rules + separate Translate and Lint workflows
- Description triggers expanded to cover lint-mode phrases (审核翻译, 检查中文, etc.)
- Removed Agent from allowed-tools (not used in workflow)
- Edit tool mandated for all file modifications (preserves JSON key ordering)
- Batching instruction added for large files (50-100 strings per pass)
