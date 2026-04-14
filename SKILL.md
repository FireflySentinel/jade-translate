---
name: jade-translate
description: |
  Localize UI strings to natural zh-CN, or audit existing Chinese translations for quality.
  Use when asked to "translate to Chinese", "localize", "zh-CN", "中文翻译",
  "audit Chinese translations", "check zh-CN quality", "检查中文", "审核翻译",
  or when working on i18n/l10n files targeting Chinese.
allowed-tools:
  - Bash
  - Read
  - Edit
  - Write
  - Grep
  - Glob
  - AskUserQuestion
---

# zh-CN UI Localization

Localization, not translation. This skill targets **mainland Chinese (zh-CN)** specifically.
Follow the rules below as a hard checklist on every string.

Foundation: these rules build on [Ant Design's copywriting spec](https://ant.design/docs/spec/copywriting-cn/),
the most complete public mainland Chinese UI writing guide. Where jade-translate goes
beyond Ant Design, it's noted.

## Mode Selection

When invoked, ask the user:

> What would you like to do?
> A) **Translate** — localize new strings from English to zh-CN
> B) **Audit** — check existing zh-CN translations for quality issues

Then follow the corresponding workflow below.

## The 10 Rules

### R1. Use 你, never 您. Strip unnecessary particles.

Use 你 for second person (per Ant Design: 您 is too distant). Don't mix 你 and 我
in the same sentence. Strip unnecessary 的/了/被 when the sentence reads fine
without them.

- 你的文件已保存 → OK (你 is acceptable)
- 您的文件已保存 → 你的文件已保存 (strip 您)
- 您的更改已被成功保存了 → 已保存更改 (strip 您/被/了, remove redundancy)

### R2. Buttons = 2-4 characters max.

保存, 取消, 开始检测. If it's longer, rewrite it shorter.

### R3. Verbs over nominalizations.

搜索 not 执行搜索. 开启 not 启用功能. Chinese UI copy should be direct.

### R4. Use established SaaS vocabulary.

Pull from 飞书/语雀/钉钉. Don't invent terms. If a concept already has a standard
Chinese term in the ecosystem, use it. Reference [Ant Design's full word list](https://ant.design/docs/spec/copywriting-cn/)
as the canonical source for standard terminology.

### R5. Toast/empty state = spoken register.

没有结果 not 未找到任何结果. These are casual moments — write like a person, not a textbook.

### R6. Full-width punctuation for sentences, half-width for code/labels.

。，！？ in prose. `.` `,` in code contexts and terse labels.

### R7. Match register to product type.

- 飞书 — B2B SaaS tone
- Linear 社区译法 — dev tools tone
- 少数派 — article/editorial tone

Match the register to the product type.

### R8. Re-read every string.

If it sounds like a textbook or Google Translate, rewrite it. This is the most important rule.

### R9. Flag uncertain strings.

When you're not sure, give 2 options + a recommendation instead of guessing:
```
"original_key": "Option A" | "Option B" ← recommended: A, because ...
```

### R10. Preserve interpolation syntax exactly.

Never translate or modify interpolation variables. Keep `{name}`, `{count}`, `{{variable}}`,
`%{key}`, `%s`, `%d` exactly as-is in the output.

- Chinese does not inflect for plural. Collapse ICU plural branches:
  `{count, plural, one {# item} other {# items}}` → `{count} 个项目`
- Never translate variable names inside braces.
- Keep HTML tags (`<b>`, `<a href>`, `<br/>`) in their original positions.

Examples:
```
EN:  "Welcome, {name}! You have {count} new notifications."
BAD: "欢迎，{名称}！你有{数量}条新通知。"
GOOD: "欢迎，{name}！你有 {count} 条新通知。"

EN:  "{count, plural, one {# item} other {# items}} in cart"
GOOD: "购物车中有 {count} 件商品"
```

## Rule Priority

When rules conflict, follow this order (highest priority first):

1. **R10** (interpolation) — never break variables
2. **R4** (established vocab) — established usage always wins
3. **R2** (button length) — keep buttons short
4. **R5** (register) — match the context
5. **R1** (pronouns/particles) — strip what's unnecessary
6. **R3** (verbs) — prefer directness

Example: if the established SaaS term is 5 characters (R4 wins over R2's 2-4 char limit),
use the established term.

## Translate Workflow

Use this workflow when localizing new strings from English to zh-CN.

1. **Identify scope.** Find the files that need localization (i18n JSON, .ts, .tsx, .vue, .po, .yaml).
   Use Glob/Grep to locate them.

2. **Read the source strings.** Read the English source file(s) to understand full context.
   Don't translate strings in isolation.

3. **Localize in batches.** Group by feature/screen. Apply all 10 rules to every string.
   For large files (100+ strings), process 50-100 strings per batch to stay within context limits.

4. **Self-review pass.** Re-read every localized string (R8). Fix anything that sounds stiff,
   formal, or machine-translated.

5. **Flag uncertain strings.** Collect all flagged strings at the end with options and
   recommendations (R9). Ask the user to pick.

6. **Write the output.** Use the Edit tool for targeted string replacements. Never rewrite
   entire locale files — this preserves key ordering, indentation, and formatting.

## Lint Workflow

Use this workflow when auditing existing zh-CN translations for quality issues.

1. **Identify scope.** Find existing locale files with zh-CN translations.
   Use Glob/Grep to locate them (common patterns: `locales/zh-CN.json`, `i18n/zh.json`,
   `messages_zh_CN.properties`, `zh-CN.ts`).

2. **Read and scan.** Read the locale file(s). For large files (100+ strings),
   process in batches of 50-100 strings. Apply all 10 rules to every string.

3. **Report issues.** Output a structured lint report:

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

4. **Score.** Calculate: start at 100, subtract 10 per error, 5 per warning, 2 per suggestion.
   Floor at 0.

5. **Fix by severity.** After presenting the report:
   - **Errors** (clear-cut violations: 您→你, textbook register, 8+ char buttons):
     fix automatically using the Edit tool.
   - **Warnings** (judgment calls: could be shorter, slightly formal):
     show the proposed fix and ask for confirmation.
   - **Suggestions** (minor style preferences):
     list but don't fix unless the user asks.

   When fixing, use the Edit tool for targeted string replacement only. Never rewrite the
   entire file. This preserves key ordering, indentation, and formatting.

## Common Pattern Fixes

| English pattern | Bad 中文 | Good 中文 | Rule |
|----------------|---------|----------|------|
| "No results found" | 未找到任何结果 | 没有结果 | R5 |
| "Are you sure you want to delete?" | 您确定要删除吗？ | 确定删除？ | R1 |
| "Successfully saved" | 已成功保存 | 已保存 | R1 |
| "Please enter your email" | 请输入您的电子邮件地址 | 输入邮箱 | R1, R3 |
| "Click here to learn more" | 点击此处了解更多信息 | 了解更多 | R1 |
| "Your changes have been saved" | 您的更改已被保存 | 已保存更改 | R1 |
| "Enable notifications" | 启用通知功能 | 开启通知 | R3 |
| "Perform search" | 执行搜索操作 | 搜索 | R3 |
| "Loading, please wait..." | 正在加载中，请稍候... | 加载中… | R1, R5 |
| "Error occurred" | 发生了一个错误 | 出错了 | R5 |
| "Operation failed" | 操作失败 | 无法完成，请重试 | R5 |
| "Do you want to continue?" | 你是否想要继续进行？ | 继续？ | R1, R3 |
| "Data is being processed" | 数据正在被处理中 | 处理中… | R1 |
| "No permission to access" | 你没有权限访问此资源 | 无访问权限 | R1, R3 |
| "Item deleted successfully" | 项目已被成功删除 | 已删除 | R1 |

## Standard Word Fixes

Per [Ant Design copywriting spec](https://ant.design/docs/spec/copywriting-cn/):

| Use | Avoid | Note |
|-----|-------|------|
| 其他 | 其它 | 其他 covers all contexts |
| 抱歉 | 对不起 | Only for system-caused issues |
| 登录 | 登陆 | 登录 = log in to account |
| 此 | 该 | 此 is more precise for "this" |
| 你 | 您 | 您 is too distant for UI |

Use Arabic numerals (1, 2, 3) instead of Chinese characters (一, 二, 三) for counts.

Error messages: use 无法完成 instead of 失败. Always tell the user what to do next.

Avoid commanding tone: prefer positive framing over 不能/不要/请勿.

Put the most important information first in sentence order.
