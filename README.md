# Claude Code 技能与 MCP 清单

本机 Claude Code 配置快照，记录当前启用的技能（来自插件）与 MCP 服务器的来源。导出日期 2026-10-28，Claude Code 2.1.311，macOS。

共安装 13 个插件（全部启用），提供 80 个技能；6 个 MCP 服务器全部为 stdio 传输。

## 技能（按插件）

| 插件 | 版本 | 状态 | 技能 | 来源 |
|---|---|---|---|---|
| superpowers | 7.0.0 | 启用 | 15 个：brainstorming, diagnosing-superpowers, dispatching-parallel-agents, executing-plans, finishing-a-development-branch, receiving-code-review, requesting-code-review, subagent-driven-development, systematic-debugging, test-driven-development, using-git-worktrees, using-superpowers, verification-before-completion, writing-plans, writing-skills | [anthropics/claude-plugins-official](https://github.com/anthropics/claude-plugins-official) |
| caveman | 2.7.0 | 启用 | 17 个：caveman, cavecrew, caveman-commit, caveman-compress, caveman-discover, caveman-evidence-review, caveman-explore, caveman-help, caveman-init, caveman-learn, caveman-manage, caveman-optimize, caveman-setup, caveman-stats, lean-build, safe-refactor, surgical-patch；另含 3 个 agent：cavecrew-builder, cavecrew-investigator, cavecrew-reviewer | [JuliusBrussee/caveman](https://github.com/JuliusBrussee/caveman) |
| mattpocock-skills | 1.3.1 | 启用 | 27 个：ask-matt, code-review, codebase-design, diagnosing-bugs, domain-modeling, grill-me, grill-with-docs, grilling, handoff, implement, implement-spec, improve-codebase-architecture, pr, prototype, research, retro, setup-matt-pocock-skills, tdd, teach, to-questionnaire, to-spec, to-tickets, triage, wait-what, wayfinder, wizard, writing-for-agents | [anthropics/claude-plugins-official](https://github.com/anthropics/claude-plugins-official)（上游 [mattpocock/skills](https://github.com/mattpocock/skills)） |
| dev-skills | 1.0.0 | 启用 | 13 个：ai-regression-testing, api-and-interface-design, context-budget, contract-first, database-migrations, design-taste-frontend, e2e-testing, error-handling, frontend-ui-engineering, observability-and-instrumentation, performance-optimization, regex-vs-llm-structured-text, security-and-hardening | 本地目录 marketplace `dev-skills-local`，无上游仓库 |
| ralph-loop | 1.0.1 | 启用 | 3 个：ralph-loop, cancel-ralph, help | [anthropics/claude-plugins-official](https://github.com/anthropics/claude-plugins-official) |
| cicd-skills | 1.0.0 | 启用 | 2 个：ci-cd-pipeline, gh-fix-ci | 本地目录 marketplace `cicd-skills-local`，无上游仓库 |
| andrej-karpathy-skills | 1.0.0 | 启用 | 1 个：karpathy-guidelines | [multica-ai/andrej-karpathy-skills](https://github.com/multica-ai/andrej-karpathy-skills) |
| anthropic-dev-skills | 1.0.0 | 启用 | 1 个：mcp-builder | 本地目录 marketplace `anthropic-skills-local`，无上游仓库 |
| slide-maker | 5.8.0 | 启用 | 1 个：slide-maker | [addsumtech/slides_maker](https://github.com/addsumtech/slides_maker) |

合计 80 个技能。

### 语言服务器插件（无技能，只提供 LSP）

jdtls-lsp（Java）、pyright-lsp（Python）、typescript-lsp（TypeScript/JavaScript）、clangd-lsp（C/C++），版本均为 1.0.0，来源 [anthropics/claude-plugins-official](https://github.com/anthropics/claude-plugins-official)。

## MCP 服务器

全部为 stdio，配置位于 `~/.claude.json` 的 `mcpServers`；OpenCode 侧在 `~/.config/opencode/opencode.json` 有同样 6 个。

| 名称 | 版本 | 安装方式 | 来源 | 说明 |
|---|---|---|---|---|
| github | 1.9.0 | 二进制 `~/.local/bin/github-mcp-server` | [github/github-mcp-server](https://github.com/github/github-mcp-server) | 通过 `github-mcp-server-launcher.sh` 启动，token 由 `gh auth token` 运行时注入，不落盘 |
| codebase-memory-mcp | 0.12.1 | 二进制 `~/.local/bin/codebase-memory-mcp` | [DeusData/codebase-memory-mcp](https://github.com/DeusData/codebase-memory-mcp) | 代码图谱索引与查询，数据在 `~/.cache/codebase-memory-mcp/` |
| chrome-devtools | 1.12.0 | `npm i -g chrome-devtools-mcp` | [ChromeDevTools/chrome-devtools-mcp](https://github.com/ChromeDevTools/chrome-devtools-mcp) | 浏览器调试，启动参数 `--no-usage-statistics` |
| context7 | 4.1.1 | `npm i -g @upstash/context7-mcp` | [upstash/context7](https://github.com/upstash/context7) | 库文档检索 |
| dbhub | 1.3.1 | `npm i -g @bytebase/dbhub` | [bytebase/dbhub](https://github.com/bytebase/dbhub) | 当前以 `--demo` 模式运行 |
| playwright | 0.0.82 | `npm i -g @playwright/mcp` | [microsoft/playwright-mcp](https://github.com/microsoft/playwright-mcp) | 浏览器自动化，环境变量 `PLAYWRIGHT_DOWNLOAD_HOST` 指向下载镜像 |

## 其他本地内容

- `~/.claude/agents/` 有 3 个手写 agent：`codebase-memory-auditor`、`codebase-memory-scout`、`codebase-memory`。
- `~/.claude/skills/` 为空，所有技能都通过插件加载。
- `~/.agents/skills` 是 75 个指向插件缓存的软链接，属于 OpenCode 侧的镜像，不含独立内容。

## 恢复要点

```bash
# 1. 添加 marketplace（GitHub 源）
claude plugin marketplace add anthropics/claude-plugins-official
claude plugin marketplace add JuliusBrussee/caveman
claude plugin marketplace add multica-ai/andrej-karpathy-skills
claude plugin marketplace add addsumtech/slides_maker

# 2. 安装插件
claude plugin install superpowers@claude-plugins-official
claude plugin install caveman@caveman
# ……其余插件同理，格式为 <插件>@<marketplace>

# 3. npm 全局包（MCP 依赖）
npm i -g @bytebase/dbhub chrome-devtools-mcp @upstash/context7-mcp @playwright/mcp

# 4. 二进制
# github-mcp-server、codebase-memory-mcp 放入 ~/.local/bin/
# github MCP 需要先 gh auth login
```

三个本地目录 marketplace（dev-skills、anthropic-skills、cicd-skills）没有上游仓库，无法用一条命令恢复，需要从原机器复制 `~/.claude/plugins/local/` 下的对应目录。

## 未包含的内容

插件缓存（`~/.claude/plugins/cache`，199 MB）、marketplace 克隆（236 MB）、`~/.cc-switch`（含 provider 密钥）、会话转录、hooks 配置和任何 token 都不在本仓库中。
