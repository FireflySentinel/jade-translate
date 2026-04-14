---
name: jade-translate
description: |
  Localize UI strings to natural zh-CN. Not translation — localization.
  Use when asked to "translate to Chinese", "localize", "zh-CN", "中文翻译",
  or when working on i18n/l10n files targeting Chinese.
allowed-tools:
  - Bash
  - Read
  - Edit
  - Write
  - Grep
  - Glob
  - Agent
  - AskUserQuestion
---

# zh-CN UI Localization

Localization, not translation. Never translate UI strings directly from English to Chinese.
Follow the spec below as a hard checklist on every string.

## The 9 Rules

1. **Strip English grammar residue.** Delete 你/您/的/了/被 when the sentence reads fine without them. Chinese doesn't need pronouns or particles the way English needs "you" and "the."

2. **Buttons = 2-4 characters max.** 保存, 取消, 开始检测. If it's longer, rewrite it shorter.

3. **Verbs over nominalizations.** 搜索 not 执行搜索. 开启 not 启用功能. Chinese UI copy should be direct.

4. **Use established SaaS vocabulary.** Pull from 飞书/语雀/钉钉. Don't invent terms. If a concept already has a standard Chinese term in the ecosystem, use it.

5. **Toast/empty state = spoken register.** 没有结果 not 未找到任何结果. These are casual moments — write like a person, not a textbook.

6. **Full-width punctuation for sentences, half-width for inline code and short labels.** 。，！？ in prose. `.` `,` in code contexts and terse labels.

7. **Reference products for register:**
   - 飞书 — B2B SaaS tone
   - Linear 社区译法 — dev tools tone
   - 少数派 — article/editorial tone
   Match the register to the product type.

8. **Re-read every string after translating.** If it sounds like a textbook or Google Translate, rewrite it. This is the most important rule.

9. **Flag uncertain strings.** When you're not sure, give 2 options + a recommendation instead of guessing. Format:
   ```
   "original_key": "Option A" | "Option B" ← recommended: A, because ...
   ```

## Workflow

1. **Identify scope.** Find the files that need localization (i18n JSON, .ts, .tsx, .vue, etc.). Use Glob/Grep to locate them.

2. **Read the source strings.** Read the English source file(s) to understand full context — don't translate strings in isolation.

3. **Localize in batches.** Group by feature/screen. Apply all 9 rules to every string.

4. **Self-review pass.** Re-read every localized string out loud (rule 8). Fix anything that sounds stiff, formal, or machine-translated.

5. **Flag uncertain strings.** Collect all flagged strings at the end with options and recommendations (rule 9). Ask the user to pick.

6. **Write the output.** Write the localized file(s). Preserve the original file structure and key ordering.

## Common Fixes

| English pattern | Bad 中文 | Good 中文 |
|----------------|---------|----------|
| "No results found" | 未找到任何结果 | 没有结果 |
| "Are you sure you want to delete?" | 您确定要删除吗？ | 确定删除？ |
| "Successfully saved" | 已成功保存 | 已保存 |
| "Please enter your email" | 请输入您的电子邮件地址 | 输入邮箱 |
| "Click here to learn more" | 点击此处了解更多信息 | 了解更多 |
| "Your changes have been saved" | 您的更改已被保存 | 已保存更改 |
| "Enable notifications" | 启用通知功能 | 开启通知 |
| "Perform search" | 执行搜索操作 | 搜索 |
| "Loading, please wait..." | 正在加载中，请稍候... | 加载中… |
| "Error occurred" | 发生了一个错误 | 出错了 |
