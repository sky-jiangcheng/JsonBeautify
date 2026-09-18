# User Instruction Memory

This file records user instructions, preferences, and teachings for reference in future interactions.

## Format

### User Instruction Entry
User instruction entries should follow this format:

[User Instruction Summary]
- Date: [YYYY-MM-DD]
- Context: [Mentioned scenario or time]
- Instructions:
  - [Content of user teaching or instruction, described line by line]

### Project Knowledge Entry
Entries discovered by the Agent during task execution should follow this format:

[Project Knowledge Summary]
- Date: [YYYY-MM-DD]
- Context: Discovered by Agent while performing [specific task description]
- Category: [Operations & Deployment|Build Methods|Testing Methods|Troubleshooting & Debugging|Workflow & Collaboration|Environment Configuration]
- Instructions:
  - [Specific knowledge points, described line by line]

## Deduplication Strategy
- Before adding a new entry, check for similar or identical instructions.
- If a duplicate is found, skip the new entry or merge it with the existing one.
- When merging, update the context or date information.
- This helps avoid redundant entries and keeps the memory file tidy.

## Entries

[User Instruction Summary]
- Date: 2026-09-15
- Context: 代码评审修复与 i18n 布局兼容任务
- Instructions:
  - 改代码保持最小改动原则（"一定要谨慎，没必要动的就不动"）：只修确认过的问题，跳过顺手重构、死代码清理等非必要改动，需产品决策的事项先列出选项再动

[Project Knowledge Summary]
- Date: 2026-09-15
- Context: Discovered by Agent while performing i18n 布局溢出修复的验证工作
- Category: Environment Configuration
- Instructions:
  - 本开发环境无 Chrome/chromium，playwright-core 拉不起浏览器，tests/layout.spec.mjs 无法在会话内运行，需要用户在本地环境跑
  - 会话内的替代验证方法：node --check 校验 JS、CSS 花括号配平计数、对 dist 产物写内联 node 正则断言、跑 node scripts/build.js 和 node scripts/pre-commit-check.js（husky 预提交也会跑后者）

[Project Knowledge Summary]
- Date: 2026-09-15
- Context: Discovered by Agent while user纠正"产品支持5语言下拉切换"，追查历史代码
- Category: Operations & Deployment
- Instructions:
  - 版本线以远程 tag 为准（v1.5.65 → v1.5.71），v1.5.69 = 5 语言版（zh/en/es/de/ja，系统语言检测）；2026-09-15 起 main 已与发布线对齐，此后 main 为权威开发线，tag 随版本发布打
  - 后续开发/修复基于 main 最新提交，发布时 bump-version.js 推进版本并打对应 tag（release.yml 的 version-gate 校验 tag 与版本文件一致）
  - 语言切换 UI 是 JS 动态生成的 #lang-menu（app.js ensureLangMenu），挂在 body 层、position:fixed 定位，在 index.html 里搜 <select> 是搜不到的；顶栏 .toolbar 的 overflow-x:auto 滚动方案与它兼容（不会被裁剪）
  - release.yml 的 iOS 图标熔断是内容校验（appiconset 与 src-tauri/icons/ios 产物 cmp 一致 + 1024x1024 尺寸），基于体积的旧判据会误杀 v1.5.67 起的扁平小图标（~78KB）；注意 generate-icons.js 未做 flatten，产物保留 alpha 通道（与 workflow 注释"无 alpha"不符，Apple 若因此拒审需在脚本加 .flatten()）
  - 基于 v1.5.65 做的第一轮修复保留在 git stash（"v1.5.65-based fixes"）和 /tmp/opencode/backup-v1.5.65-fixes/，正式修复已移植到 main（v1.5.70 起）
