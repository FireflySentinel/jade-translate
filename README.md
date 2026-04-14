# jade-translate

A Claude Code skill that localizes UI strings to natural mainland Chinese (zh-CN), or audits existing translations for quality.

Not translation. Localization. The difference: translation gives you 未找到任何结果. Localization gives you 没有结果.

## Table of Contents

- [What it does](#what-it-does)
- [Install](#install)
- [Usage](#usage)
- [The 10 Rules](#the-10-rules)
- [Lint mode](#lint-mode)
- [Uninstall](#uninstall)
- [Contributing](#contributing)
- [License](#license)

## What it does

jade-translate catches bad Chinese UI copy. It encodes 10 rules for what natural mainland zh-CN sounds like, built on [Ant Design's copywriting spec](https://ant.design/docs/spec/copywriting-cn/) and the conventions of 飞书, 钉钉, and Linear's community translations.

Two modes:

- **Translate** — localize new strings from English to zh-CN
- **Audit** — check existing zh-CN translations for quality issues

### Before / After

| Before | After | Rule |
|--------|-------|------|
| 未找到任何结果 | 没有结果 | R5: spoken register |
| 您确定要删除吗？ | 确定删除？ | R1: strip 您 |
| 请输入您的电子邮件地址 | 输入邮箱 | R1, R3 |
| 启用通知功能 | 开启通知 | R3: verbs over nouns |
| 发生了一个错误 | 出错了 | R5: spoken register |
| 欢迎，{名称}！ | 欢迎，{name}！ | R10: preserve interpolation |

## Install

### One-liner

```bash
mkdir -p ~/.claude/skills/jade-translate && curl -fsSL -o ~/.claude/skills/jade-translate/SKILL.md https://raw.githubusercontent.com/FireflySentinel/jade-translate/main/SKILL.md
```

### Manual

1. Clone the repo:
   ```bash
   git clone https://github.com/FireflySentinel/jade-translate.git
   ```
2. Copy the skill file:
   ```bash
   mkdir -p ~/.claude/skills/jade-translate
   cp jade-translate/SKILL.md ~/.claude/skills/jade-translate/SKILL.md
   ```

### Verify

Open Claude Code and ask it to "translate to Chinese" or "audit Chinese translations." If jade-translate is installed, it will activate automatically.

## Usage

### Translate mode

Ask Claude Code to translate strings to Chinese. jade-translate will:

1. Ask you to confirm translate mode
2. Find your locale files
3. Localize strings following all 10 rules
4. Flag uncertain strings with 2 options + a recommendation

```
> Translate my error messages to Chinese
> 把这些字符串翻译成中文
> Localize src/locales/en.json to zh-CN
```

### Lint mode

Ask Claude Code to audit existing Chinese translations. jade-translate will:

1. Ask you to confirm audit mode
2. Scan your locale files against all 10 rules
3. Output a structured report with score, issues, and fixes
4. Auto-fix errors, prompt for warnings

```
> Audit my Chinese translations
> Check zh-CN quality in locales/zh-CN.json
> 检查中文翻译质量
```

Example output:

```
## Lint Report: locales/zh-CN.json

Score: 72/100

| # | Key | Original | Issue | Fix | Rule | Sev |
|---|-----|----------|-------|-----|------|-----|
| 1 | toast.none | 未找到任何结果 | Textbook register | 没有结果 | R5 | error |
| 2 | btn.delete | 您确定要删除吗？ | Uses 您 | 确定删除？ | R1 | error |
| 3 | btn.save | 保存更改 | Can be shorter | 保存 | R2 | warning |

3 issues: 2 errors, 1 warning
```

## The 10 Rules

| Rule | What | Example |
|------|------|---------|
| R1 | Use 你, never 您. Strip unnecessary particles. | 您的更改已被保存 → 已保存更改 |
| R2 | Buttons = 2-4 characters max | 保存更改 → 保存 |
| R3 | Verbs over nominalizations | 执行搜索 → 搜索 |
| R4 | Use established SaaS vocabulary | Pull from 飞书/钉钉, don't invent terms |
| R5 | Toast/empty state = spoken register | 未找到任何结果 → 没有结果 |
| R6 | Full-width punctuation for prose, half-width for code | 。，！？ vs `.` `,` |
| R7 | Match register to product type | 飞书=B2B, Linear=dev tools, 少数派=editorial |
| R8 | Re-read every string | If it sounds like Google Translate, rewrite |
| R9 | Flag uncertain strings | Give 2 options + recommendation |
| R10 | Preserve interpolation syntax | Keep {name}, {count}, {{var}} exactly as-is |

**Priority order** (when rules conflict): R10 > R4 > R2 > R5 > R1 > R3

Foundation: [Ant Design copywriting spec](https://ant.design/docs/spec/copywriting-cn/)

## Uninstall

```bash
rm -rf ~/.claude/skills/jade-translate
```

## Contributing

See [CONTRIBUTING.md](CONTRIBUTING.md).

## License

MIT. See [LICENSE](LICENSE).
