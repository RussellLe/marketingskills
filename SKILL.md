---
name: marketingskills (入口 / entry map)
description: >
  本仓库的「入口文档」——面向 AI Agent 的导航地图。Marketing Skills 是一套跨 Agent 的营销技能库：
  50 个 Agent Skill（CRO、文案、冷邮、SEO / AI-SEO、投放、广告创意、定价、留存、RevOps、增长工程等）
  + 64 个零依赖 Node CLI（直连 GA4 / Stripe / Resend / Ahrefs 等真实 API）+ 95 份工具接入指南。
  当任务是「把某个产品卖出去」的营销执行——写什么、怎么转化、怎么衡量——时，先读本文件。
note: >
  这不是被自动加载的 Skill 清单——真正被注册的单元是 `skills/<name>/SKILL.md` 共 50 份
  （`.claude-plugin/plugin.json` 里 `"skills": "./skills"` 只扫该目录）。本文件是仓库级索引与路由，
  不参与注册：`./validate-skills.sh` 实跑输出 `Passed: 50`，不含本文件。
---

# Marketing Skills · 入口导航（Entry Map）

> **给 AI Agent 的一句话**：这里的 50 个 skill 是**指令书**（照着做的操作手册），
> `tools/clis/` 里的 64 个 CLI 是**手脚**（真去调 API 取数、发信、拉报表）。
> 最关键的一条：几乎所有 skill 都以 `.agents/product-marketing.md` 为共同上下文——
> **没有它，你写出来的东西是通用模板；有它，才是这家产品的营销物料。**

## 0. 你该怎么用这份文档

1. 先看 **§1 这是什么**、**§2 先建上下文**——第 2 节是本仓最容易被跳过、也最致命的一步。
2. 拿用户需求去 **§5 路由表**，定位到具体 `skills/<name>/SKILL.md`。
3. 要真实数据时看 **§4 工具层**（CLI 调用契约已实测）。
4. 动手前读 **§6 硬约束**——尤其「合作方披露」与「版本号必须同 PR 提」两条。

**判断是否启用**：任务是**围绕一个产品做营销执行或营销决策**（写页面/邮件/广告、提转化、
定价、找渠道、做增长实验、衡量归因）——用本 Skill。
**纯技术 SEO 审计**（收录、Core Web Vitals、schema 校验、AI 搜索引用）走同目录的 `claude-seo`，
它有真会抓页面的脚本；本仓的 `seo-audit` / `ai-seo` 是方法论指令书，不带审计执行器。

---

## 1. 这是什么

一个遵循 [Agent Skills 规范](https://agentskills.io/specification.md) 的**跨 Agent** 技能库
（Claude Code / Codex / Cursor / Windsurf 通用），同时也是 Claude Code plugin marketplace。
`plugin.json`：`name: marketing-skills`，`version: 2.11.1`，MIT，上游作者 Corey Haines。

| 组成 | 位置 | 实测数量 | 作用 |
|---|---|---|---|
| Agent Skills | `skills/<name>/SKILL.md` | **50** | 每个是一个营销工种的操作手册；可带 `references/` `scripts/` `assets/` |
| CLI 工具 | `tools/clis/*.js` | **64** | 零依赖 Node（18+），直连各家 API；**注意 `AGENTS.md` 里写的「51 tools」已过时** |
| 接入指南 | `tools/integrations/*.md` | **95** | 每个工具的鉴权、端点、常用操作 |
| 工具索引 | `tools/REGISTRY.md` | 95 行工具表 | 按能力找工具：有无 API / MCP / CLI / SDK |
| 合作方治理 | `tools/PARTNERS.md` + `partners.json` | — | 赞助披露规则；索引里 ◆ 标记由 `scripts/sync-partners.mjs` 生成 |
| Composio 接入层 | `tools/composio/` | 2 份 | 给没有原生 MCP 的 OAuth 工具（HubSpot、Salesforce、Meta Ads…）兜底 |
| 版本台账 | `VERSIONS.md` | 50 行版本表 | 每个 skill 的独立版本号（表后另有 Recent Changes 变更日志），用于更新检查 |

---

## 2. 先建上下文（跳过这步等于白干）

`product-marketing` 是**地基 skill**：实测 `skills/` 下有 **103 个文件**引用产品上下文文档。
其余 49 个 skill 在动笔前都要先读它。

- **文档落点**：`.agents/product-marketing.md`（旧位置 `.claude/product-marketing.md` 仍兼容）。
- **不存在时**：跑 `skills/product-marketing/SKILL.md`——它会问你产品、受众、定位、竞品、
  异议、品牌语调、证据点，或从 codebase 自动起草，然后建档。
- **已存在时**：读它，改动只动受影响章节，并**升 Document 版本 + 在 Changelog 顶部追加一行**
  说明改了什么、为什么。

> 用户直接说「帮我写个落地页」而上下文文档不存在时，**先建档再写**，不要凭空生成通用文案。

---

## 3. 能力清单（50 个 skill，按类）

> 何时读本节：需求已明确，要定位到具体 skill 目录。分类与命名均来自仓库实测枚举。

| 类别 | Skills |
|---|---|
| **地基** | `product-marketing`（所有 skill 的共同上下文）、`marketing-plan`（AARRR 全盘计划）、`marketing-council`（多专家视角会诊） |
| **转化优化** | `cro`、`signup`、`onboarding`、`popups`、`paywalls`、`free-tools`、`lead-magnets` |
| **内容与文案** | `copywriting`、`copy-editing`、`cold-email`、`emails`、`sms`、`social`、`video`、`image`、`content-strategy` |
| **SEO 与被发现** | `seo-audit`、`ai-seo`、`programmatic-seo`、`site-architecture`、`schema`、`competitors`、`aso`、`directory-submissions` |
| **投放与分发** | `ads`、`ad-creative`、`events`、`public-relations`、`influencer-marketing`、`community-marketing`、`co-marketing` |
| **衡量与实验** | `analytics`、`ab-testing`、`attribution` |
| **留存与货币化** | `churn-prevention`、`pricing`、`offers`、`referrals` |
| **销售与 RevOps** | `revops`、`sales-enablement`、`prospecting`、`competitor-profiling`、`customer-research` |
| **策略与增长** | `marketing-ideas`、`marketing-psychology`、`marketing-loops`、`launch` |

每个 skill 的完整触发词与边界在自己的 `description` 字段里；跨 skill 依赖在各自的
**Related Skills** 一节。

---

## 4. 工具层：CLI 调用契约（已实测）

> 何时读本节：需求要**真实数字或真实动作**（拉 GA4 数据、发一封 Resend、查 Ahrefs 反链）。

统一形态（实测 `node v24.12.0`，脚本零依赖，Node 18+ 即可）：

```bash
node tools/clis/<tool>.js                       # 无参 = 打印 usage
node tools/clis/<tool>.js <cmd> [--flags]       # 执行
node tools/clis/<tool>.js <cmd> --dry-run       # 只打印将要发出的请求，不发
node --check tools/clis/<tool>.js               # 语法检查
```

**三种实测返回形态**，据此判断卡在哪一步：

| 你看到 | 含义 | 下一步 |
|---|---|---|
| `{"error":"AHREFS_API_KEY environment variable required"}` | 凭据未设 | 读 `tools/integrations/<tool>.md` 拿变量名，让用户提供 |
| `{"error":"Unknown command","usage":{...}}` | 凭据 OK、子命令写错 | 照 `usage` 里的签名改 |
| `{"_dry_run":true,"method":...,"headers":{"Authorization":"***"}...}` | 预演成功（密钥已脱敏） | 去掉 `--dry-run` 真跑 |

**先 `--dry-run` 再真跑**——这些 CLI 会真的发信、真的写数据。
没有原生 MCP 的 OAuth 工具走 Composio：`tools/integrations/composio.md`。

---

## 5. 需求 → 去哪（路由表）

| 用户想要… | 怎么做 / 读哪 |
|---|---|
| 任何营销任务，且没有产品上下文文档 | **先** `skills/product-marketing/SKILL.md` 建 `.agents/product-marketing.md` |
| 「这页转化不行」 | `skills/cro/`；注册流走 `skills/signup/`，付费墙走 `skills/paywalls/` |
| 「写首页 / 落地页文案」 | `skills/copywriting/`；已有稿要润色走 `skills/copy-editing/` |
| 「写一套冷邮序列」 | `skills/cold-email/`；名单从 `skills/prospecting/` 来 |
| 「做生命周期邮件 / 自动化流」 | `skills/emails/`；接入指南 `tools/integrations/{customer-io,mailchimp,resend}.md` |
| 「让 ChatGPT 提到我们」 | `skills/ai-seo/`（AEO/GEO/LLMO 方法论）；要审计执行器则转 `claude-seo` 的 `/seo geo` |
| 「批量出广告素材」 | `skills/ad-creative/`；投放策略 `skills/ads/` |
| 「定价怎么定 / 怎么打包」 | `skills/pricing/`；卖什么本身走 `skills/offers/` |
| 「用户在流失」 | `skills/churn-prevention/`（取消流、挽留 offer、扣款失败恢复） |
| 「做个 A/B 实验」 | `skills/ab-testing/`；埋点先看 `skills/analytics/` |
| 「哪条渠道真带来收入」 | `skills/attribution/`；数据用 `node tools/clis/ga4.js` |
| 「一份完整的营销计划」 | `skills/marketing-plan/`（AARRR 结构） |
| 「没思路，给点招」 | `skills/marketing-ideas/`（140 条 SaaS 打法）、`skills/marketing-psychology/` |
| 「找个能干这活的工具」 | `tools/REGISTRY.md` 按能力查 → `tools/integrations/<tool>.md` 看接法 |
| 「这些 skill 是不是过期了」 | 比对本地与 `VERSIONS.md`（上游 raw 地址见 `CLAUDE.md`「Checking for Updates」） |
| 「我改了 skill，怎么验」 | `./validate-skills.sh`（实测输出 `Passed: 50`，exit 0） |

---

## 6. 硬约束与规范

**内容规则**

- 每个 `SKILL.md` **500 行以内**，细节下沉到 `references/`；`name` 必须与目录名完全一致
  （小写字母/数字/连字符，不得首尾连字符、不得连续 `--`）；`description` 1–1024 字符且含触发词。
- **不要在 `SKILL.md` 里写 Claude Code 专属语法**（如 `` !`command` `` 动态注入）——
  本仓是跨 Agent 的，其他 Agent 会把它当字面量读成乱指令。要用就放自己项目的 `.claude/skills/` 覆盖层。

**合作方披露（最容易被违反的一条）**

- `tools/` 索引里的 ◆ 是**付费赞助并已披露**的合作方，含义是「已披露、已核适配」，
  **不是「品类最优」**。合作方身份**绝不改变任何 skill 的推荐结论**——
  非合作方是对的答案时，就给非合作方。判定基准见 `CONTRIBUTING.md` 的 integrity rubric
  （给选项而非唯一答案、须披露、通过 swap test）。
- ◆ 表格由 `partners.json` 生成，**别手改 `REGISTRY.md` 的合作方块**，跑
  `node scripts/sync-partners.mjs`。

**版本（漏了会让所有已安装用户看不到你的更新）**

- 改了某个 skill ⇒ 必须升它 `SKILL.md` frontmatter 里的 `metadata.version`，并同步 `VERSIONS.md`
  ——更新检查就是拿 `VERSIONS.md` 比对本地。
- 仓库版本号（`plugin.json` / `marketplace.json` / `VERSIONS.md` 标题）三处同一个数：
  **x** = 仓库级改动，**y** = 新增 skill，**z** = 改既有 skill。
  给既有 skill 加内容再多也是 **z，不是 y**。版本号要**和改动在同一个 PR** 里提。

**Git**：分支 `feature/<skill>` / `fix/<skill>-<desc>` / `docs/<desc>`；提交走 Conventional Commits。

**真相来源与同步（本仓是 fork）**

- 本目录是 `RussellLe/marketingskills`，fork 自 **`coreyhaines31/marketingskills`**。
  上游 `CLAUDE.md` 里指向 `coreyhaines31` raw 地址的更新检查**查的是上游**，不是本 fork。
- 本仓被 `russell-harness` 以 submodule 挂在 `skills/seo/marketingskills`：**改了内容要
  commit + push 本仓，再回宿主仓 `git add skills/seo/marketingskills` 回写 gitlink**，两步一组。
- 跟进上游：`git remote add upstream https://github.com/coreyhaines31/marketingskills` 后 fetch/merge。

---

## 7. 权威文件索引

| 文件 | 内容 | 何时读 |
|---|---|---|
| `skills/product-marketing/SKILL.md` | 产品上下文文档的建档与维护流程 | **任何营销任务开工前** |
| `skills/<name>/SKILL.md` | 单个工种的完整操作手册 + Related Skills | 已定位到 skill 后 |
| `README.md` §How Skills Work Together | skill 之间的组合关系图与交叉引用 | 需求横跨多个工种时 |
| `tools/REGISTRY.md` | 95 个工具的能力矩阵（API/MCP/CLI/SDK） | 要选工具时 |
| `tools/integrations/<tool>.md` | 单个工具的鉴权、端点、常用操作 | 要真调某个 API 时 |
| `tools/PARTNERS.md` | 合作方治理：赞助买到什么、绝不买到什么 | 涉及推荐具体工具时 |
| `CLAUDE.md` / `AGENTS.md` | 上游维护者指南（规范、版本、写作风格、更新检查） | 要改本仓内容时；**其「51 tools」数字已过时，以实测 64 为准** |
| `VERSIONS.md` | 每个 skill 的版本与更新日期 | 判断是否过期时 |
| `CONTRIBUTING.md` | 贡献规范 + 提及工具的 integrity rubric | 要新增/修改涉及工具的内容时 |

---

## 8. 边界（不适用）

- **技术 SEO 的实际审计与取数**（抓页面、CWV、schema 校验、GSC/GA4 报表、外链数据）：
  走同目录 `claude-seo`——本仓这几个 skill 是方法论，不带执行器。
- **需要成建制多角色协同的大型营销战役编排**（多学科并行、带审计闸门）：
  看同目录 `aaron-marketing-skills`。
- **与产品营销无关的通用写作 / 工程任务**：不用本 Skill。
- **没有产品上下文又不肯建档时**：可以先给通用框架，但必须**明说这是模板不是针对该产品的方案**，
  不要假装了解用户的受众与定位。
