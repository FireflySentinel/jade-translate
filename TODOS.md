# TODOS

## v2: Context-Aware Register Matching
**What:** Auto-detect product type (B2B SaaS vs consumer vs dev tool) and adjust register accordingly.
**Why:** Currently Rule 7 lists 飞书/Linear/少数派 as manual register references. v2 would detect product type from the codebase (package.json, UI patterns, dependencies) and automatically select the right register.
**Pros:** Removes a judgment call from the user. Better output without user needing to know what "register" means.
**Cons:** Requires defining product type detection heuristics. Adds complexity to the skill.
**Depends on:** v1 shipped and validated with real users.

## v2+: Standalone Linter CLI
**What:** Build a CLI tool (textlint-based or custom) that runs outside Claude Code with --fix flag, integrable into CI/CD.
**Why:** Unlocks distribution beyond the Claude Code ecosystem. CI integration means automated quality gates in build pipelines.
**Pros:** Standalone tool. CI/pre-commit hooks. GitHub stars potential. Not locked to one AI platform.
**Cons:** 1-2 weeks of work. Some rules (register detection) hard to automate without LLM. Need to decide: LLM-powered CLI (requires API key) or pure heuristic (limited accuracy).
**Depends on:** v1 shipped, showcase proving rules work on real projects.
