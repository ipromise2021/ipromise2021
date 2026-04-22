# Claude Code 系统学习指南

> 一份面向工程师与产品经理的 AI 编程完整知识体系

---

## 目录

1. [认知篇：AI编程工具的演进](#1-认知篇ai编程工具的演进)
2. [安装篇：10分钟快速上手](#2-安装篇10分钟快速上手)
3. [入门篇：第一个项目](#3-入门篇第一个项目)
4. [核心工作流](#4-核心工作流)
5. [记忆系统：CLAUDE.md](#5-记忆系统claudemd)
6. [进阶对话技巧](#6-进阶对话技巧)
7. [扩展体系：Skills、Hooks与MCP](#7-扩展体系skillshooks与mcp)
8. [多Agent协作](#8-多agent协作)
9. [实战：从零构建完整产品](#9-实战从零构建完整产品)
10. [心智模型与最佳实践](#10-心智模型与最佳实践)

---

## 1. 认知篇：AI编程工具的演进

### 1.1 三年三次变革

| 年代 | 工具 | 本质 | 角色定位 |
|------|------|------|----------|
| 2022 | GitHub Copilot | 补全工具 | 你的**输入法** |
| 2023-2024 | Cursor | IDE内嵌Agent | 你的**结对伙伴** |
| 2025+ | Claude Code | 终端独立Agent | 你的**独立工程师团队** |

### 1.2 Claude Code vs Cursor

| 维度 | IDE Agent（Cursor等） | 终端Agent（Claude Code） |
|------|----------------------|--------------------------|
| 运行环境 | 编辑器内嵌，依赖IDE框架 | 终端原生，直接操作操作系统 |
| 自主程度 | 通常需要确认 | 可完全无人值守运行 |
| 系统集成 | 通过插件桥接 | 直接操作git、shell、MCP |
| 记忆系统 | 隐式的项目索引 | 显式的CLAUDE.md记忆文件 |
| 并行能力 | 主要单实例工作 | 原生支持多实例并行 |

### 1.3 核心认知转变

```
传统编程：你想方案，自己写代码，自己调试
Claude Code：描述需求，AI出方案和代码，你管要什么和好不好
```

**你的价值不在于写代码，而在于定义要做什么、判断做得对不对。**

### 1.4 适用人群

- **工程师**：提高10倍效率，让AI接管样板代码、调试、测试、CI/CD
- **产品经理**：自己做MVP，一个周末做出能跑的原型
- **创业者**：实现一人公司，网站/App/后端API全部一个人搞定

---

## 2. 安装篇：10分钟快速上手

### 2.1 三种安装方式

| 安装方式 | 命令 | 适用平台 | 推荐度 |
|----------|------|----------|--------|
| Native Install | `curl -fsSL https://claude.ai/install.sh \| bash` | macOS/Linux | ⭐ 推荐 |
| Homebrew | `brew install /-cask claude-code` | macOS | 适合brew用户 |
| WinGet | `winget install Anthropic.ClaudeCode` | Windows | Windows首选 |

### 2.2 Windows额外步骤

1. 安装Git for Windows（[git-scm.com](https://git-scm.com)）
2. 使用PowerShell或Git Bash运行（不建议直接用CMD）

### 2.3 五种使用环境

| 环境 | 特点 | 适合人群 |
|------|------|----------|
| 终端CLI | 最完整功能，直接在终端输入`claude` | 日常开发主力 |
| VS Code扩展 | 侧边栏运行，直接看文件变更 | 习惯VS Code的开发者 |
| Desktop App | 独立桌面应用 | 不熟悉终端的用户 |
| Web版 | 浏览器访问 claude.ai/code | 临时使用、体验试用 |
| JetBrains插件 | 在IntelliJ IDEA等中使用 | JetBrains用户 |

> **建议**：先在终端把完整能力摸熟，终端才是完全体。

### 2.4 账号与付费

| 方案 | 月费 | 用量 | 适合人群 |
|------|------|------|----------|
| Pro | $20/月 | 基础用量 | 个人开发者、学习者 |
| Max 5x | $100/月 | 5倍Pro用量 | 重度用户、全职AI编程 |
| Max 20x | $200/月 | 20倍Pro用量 | 团队用户、商业项目 |

> **先从Pro开始**，每天用超过2小时再升Max。

### 2.5 首次验证清单

```
✅ CLI可用：claude /-version 显示版本号
✅ 账号已登录：claude启动后不再要求登录
✅ 能创建文件：让Claude创建一个测试文件
✅ 能读取项目：在已有项目目录启动，问项目用途
✅ 能运行命令：让它运行 ls 或 git status
```

### 2.6 常见问题

**连不上/登录不了**（国内网络）：
```bash
export HTTPS_PROXY=http://127.0.0.1:你的端口
export HTTP_PROXY=http://127.0.0.1:你的端口
```
建议把这两行加到 `~/.zshrc` 或 `~/.bashrc`。

**Permission denied**：
```bash
mkdir -p ~/.local/bin
curl -fsSL https://claude.ai/install.sh | bash
```

**升级**：
```bash
# Native Install
curl -fsSL https://claude.ai/install.sh | bash
# Homebrew
brew upgrade /-cask claude-code
# WinGet
winget upgrade Anthropic.ClaudeCode
```

---

## 3. 入门篇：第一个项目

### 3.1 五步流程

```
描述需求 → 审查方案 → 确认执行 → 验证结果 → 迭代改进
```

### 3.2 关键技巧：先方案后代码

```
需求描述的最后加一句：「先别急着写代码，给我一个实现方案」
```

### 3.3 完整示例：AI新闻聚合CLI

**Step 1：描述需求**
```
帮我做一个AI新闻聚合CLI工具。需求如下：
1. 从以下RSS源抓取最近24小时的文章：
   - TechCrunch AI (https://techcrunch.com/category/artificial-intelligence/feed/)
   - The Verge AI (https://www.theverge.com/rss/ai-artificial-intelligence/index.xml)
   - Hacker News前30条 (https://hnrss.org/newest?q=AI&count=30)
2. 对每篇文章提取标题、链接、发布时间、来源
3. 按时间倒序排列，输出一份Markdown格式的日报到 output/ 目录
4. 用TypeScript写，用tsx直接运行

先别急着写代码，给我一个实现方案。
```

**Step 2：审查方案**
Claude会给出项目结构、技术方案、实现流程。你来确认方向、补充细节。

**Step 3：确认执行**
按 `y` 允许权限，看着Claude工作。

**Step 4：验证结果**
```bash
npx tsx src/index.ts
```

**Step 5：迭代改进**
```
# 第一个改进：加AI摘要
# 第二个改进：加定时运行
# 第三个改进：加去重逻辑
```

### 3.4 心态转变

| 传统编程 | Claude Code编程 |
|----------|----------------|
| 自己想方案，自己写代码 | 描述需求，Claude出方案和代码 |
| 一行一行调试 | 把错误信息给Claude，它自己调 |
| 查文档、StackOverflow | 直接问Claude |
| 代码review靠人工 | 让Claude解释它写的代码 |
| 重构要先理解全部代码 | 告诉Claude「把这块重构成XXX模式」 |

---

## 4. 核心工作流

### 4.1 Plan模式：先想清楚再动手

**进入方式**：在输入框中按两次 `Shift+Tab`

**Plan模式行为**：
- 读取文件理解代码，但不会修改任何文件
- 给出详细的实现方案
- 可以反复讨论、修改方案

**完整工作流（Boris推荐）**：
```
1. Plan模式下描述需求，来回讨论
2. 方案大致满意后，按 Ctrl+G 在编辑器中写详细执行指令
3. 切换回执行模式，开启Auto-accept
```

**判断标准**：如果这个任务需要跟同事解释才能让他做，就值得用Plan模式。

### 4.2 Auto模式：减少审批疲劳

**问题**：93%的权限请求被用户直接批准，审批疲劳让安全机制形同虚设。

**Auto模式**：用AI分类器替你判断操作风险。

**两层防御**：
- **输入层**：Prompt Injection探测器，扫描所有读取内容
- **输出层**：Transcript分类器，两阶段评估操作风险

**启用方式**：
```bash
claude /-permission-mode auto
# 或 Shift+Tab 循环切换
```

**会被拦截的案例**：
- 范围升级：「清理旧分支」把远程分支也删了
- 凭证探索：遇到认证错误后自行搜索其他API token
- 绕过安全检查：预检失败后用 /-skip-verify 重试
- 数据外泄：自行创建公开GitHub Gist

### 4.3 权限管理：/permissions

**预授权安全命令**：
```bash
# 允许运行所有npm脚本
Bash(npm run *)
# 允许编辑docs目录
Edit(/docs/**)
# 允许git操作
Bash(git add *)
Bash(git commit *)
Bash(git push)
```

**三层选择**：

| 方式 | 省心程度 | 安全程度 | 适合人群 |
|------|----------|----------|----------|
| Auto模式 | 高 | 中（AI分类器保护） | 大多数日常开发者 |
| /permissions白名单 | 中 | 高（精确控制） | 团队使用、需要精细控制 |
| 逐个确认（默认） | 低 | 最高 | 高风险操作、初学阶段 |

### 4.4 Git操作

**一句话commit**：
```
提交当前的改动，写一个有意义的commit message
```

**创建PR**：
```
创建一个PR，标题和描述写清楚这个功能的作用
```

**Git Worktrees（并行工作利器）**：
```bash
claude /-worktree
# 创建新worktree，在隔离目录中工作
# 好处：不影响当前分支、可同时开多个、互不干扰
```

### 4.5 Computer Use：AI长了眼睛和手

让Claude直接看到屏幕截图，操控鼠标和键盘。

**使用场景**：
- 测试Web应用UI（像用户一样点击页面、填表单）
- 操作没有API的桌面软件/老旧管理后台
- 自动化重复的GUI操作
- 调试Chrome扩展

**当前限制**：
- 慢：每步需要截图→分析→决定→执行
- 精细操作不靠谱（拖拽精确位置等）
- 不适合需要快速反应的场景

### 4.6 Voice Mode：开口说话就能编程

**进入方式**：输入 `/voice`，按住空格说话

**最佳场景**：
- 手不方便时（走路、做饭）
- 脑暴时（想法哗哗冒出来）
- 描述空间和视觉概念时

### 4.7 会话管理

| 操作 | 命令/快捷键 | 使用时机 |
|------|-------------|----------|
| 清空当前会话 | /clear | 切换到完全不相关的任务 |
| 压缩上下文 | /compact | 会话太长、Claude变慢 |
| 停止当前操作 | Esc | Claude在做不想要的事 |
| Rewind（回滚） | Esc × 2 或 /rewind | 改坏了代码 |
| 恢复上次会话 | claude /-continue | 终端不小心关了 |
| 恢复指定会话 | claude /-resume | 回到某个历史会话 |
| 侧链提问 | /btw | 问不相关问题，不污染上下文 |

### 4.8 六个常见坑

1. **一个会话什么都塞**：上下文被塞满，Claude对每个任务理解都很浅
2. **反复纠正，越改越偏**：纠正两次不行就 /clear 重来
3. **看着像对的就接受了**：每一轮改动都实际运行一次
4. **过度微操**：关注结果，让Claude把完整任务做完
5. **需求模糊**：给具体的、可验证的需求
6. **不写CLAUDE.md**：项目根目录必须有这个文件

---

## 5. 记忆系统：CLAUDE.md

### 5.1 为什么它最重要

Claude Code每次启动新会话，第一件事就是读CLAUDE.md。没有它，Claude就像空降到陌生代码库的新同事。

> Shrivu（Abnormal AI）：「在有效使用Claude Code时，代码库中最重要的文件就是根目录的CLAUDE.md。这个文件是agent的'宪法'。」

### 5.2 从护栏开始，别写手册

**核心原则**：不要试图预先写一本完整手册。每次Claude犯错，就加一条规则。

```
Claude犯错 → 记录到CLAUDE.md → 下次不再犯 → 错误率持续降低
```

Boris团队的CLAUDE.md只有约2500 tokens（~100行）。

### 5.3 写什么、不写什么

| 该写 | 不该写 |
|------|--------|
| Claude猜不到的Bash命令 | Claude读代码就能知道的事 |
| 与默认不同的代码风格偏好 | 标准语言规范 |
| 测试命令和偏好框架 | 详细API文档（给链接） |
| 项目架构决策和背景 | 频繁变化的信息 |
| 开发环境的坑 | 文件逐一描述 |
| 常见陷阱和修复方式 | 「写整洁代码」这种废话 |

### 5.4 层级结构

```
~/.claude/CLAUDE.md          ← 全局级：所有项目共用的偏好
./CLAUDE.md                  ← 项目级：检入git，与团队共享
./src/CLAUDE.md             ← 子目录级：monorepo中特定模块的规则
```

### 5.5 示例：一个好的CLAUDE.md

```markdown
# MyApp

## 架构
- Next.js 15 + TypeScript + Tailwind CSS
- 数据库：PostgreSQL + Drizzle ORM
- 认证：Better Auth
- 状态管理：Zustand（不要用Redux）

## 开发命令
- 启动开发服务器：pnpm dev
- 跑测试：pnpm test（Jest + React Testing Library）
- 类型检查：pnpm typecheck
- Lint：pnpm lint

## 代码风格
- 组件用函数式，不用class
- 样式用Tailwind，不要写CSS文件
- 数据获取用server component，不用useEffect
- 错误处理用error.tsx边界

## 常见陷阱
- Drizzle迁移后必须跑 pnpm db:generate
- 环境变量改了之后要重启dev server
- better-auth的session检查在middleware中

## 不要做
- 不要安装新依赖除非我明确同意
- 不要修改 drizzle.config.ts
- 不要在client component中直接调数据库
```

### 5.6 Auto Memory

除了手写的CLAUDE.md，Claude Code还有自动记忆系统。

纠正Claude的行为（如「以后commit message都用英文」），Claude会自动记住，存储在 `~/.claude/projects/<项目>/memory/` 目录下。

| 手写CLAUDE.md | Auto Memory |
|---------------|-------------|
| 团队共享的规则 | 个人偏好 |
| 检入git | 存在本地 |
| 你主动维护 | Claude自动维护 |

---

## 6. 进阶对话技巧

### 6.1 三条核心原则

**1. 具体化**
```
不推荐：帮我加个搜索功能
推荐：在 src/components/Header.tsx 的导航栏中添加搜索框，
      用 Fuse.js 做模糊搜索，搜索范围是 posts 数组，
      参考现有的 FilterDropdown 组件的样式
```

**2. 指向已有模式**
```
「看src/components/UserWidget.tsx的实现方式，照着做一个CalendarWidget」
```

**3. 描述症状，不要猜原因**
```
不推荐：token刷新逻辑有问题
推荐：用户在session超时后登录失败，请检查src/auth/下的token刷新流程
```

### 6.2 Context Engineering

**核心原则**：不是给所有信息，而是给对的信息。

**上下文太多，模型表现反而变差。**

主动管理上下文的方式：
- `@` 引用文件
- 粘贴截图（UI问题比文字描述准确10倍）
- Pipe数据：`cat error.log | claude`
- 给URL：Claude可以读取网页内容

### 6.3 让Claude采访你

对于大功能，先让Claude采访你：
```
我想做一个支付功能，在动手之前，先采访我，问清楚所有你需要知道的事情。
```
采访结束后开新会话执行，避免长对话污染新任务。

### 6.4 Effort级别

| 级别 | 适合场景 | 特点 |
|------|----------|------|
| Low | 简单格式化、重命名 | 快，但容易犯低级错误 |
| Medium | 日常开发任务 | 比默认更轻量 |
| High | 复杂功能开发、调试 | 默认级别 |
| Max | 极端复杂架构决策 | 无限推理token，最慢最深 |

> **别为了省几秒钟把effort调低**，修低级错误花的时间远不止几秒。

### 6.5 多轮对话策略

- **紧密反馈循环**：发现方向偏了，立刻纠正，越早成本越低
- **两次纠正不行，换条路**：/clear 清掉上下文，重新开始
- **换任务就清上下文**：/clear 避免前一个任务的对话历史干扰新任务
- **用subagent做调研**：调研结果返回主会话，思考过程不污染主上下文

---

## 7. 扩展体系：Skills、Hooks与MCP

### 7.1 三种机制对比

| 机制 | 本质 | 确定性 | 适用场景 |
|------|------|--------|----------|
| Skills | Markdown指令包 | 高但非100%（advisory） | 领域知识、可复用工作流 |
| Hooks | Shell脚本钩子 | 100%确定执行 | 格式化、lint、安全检查 |
| MCP | 外部工具连接器 | 100% | 数据库、API、第三方服务 |

**关系**：Skills教Claude怎么做事，Hooks在关键节点自动执行检查，MCP把外面的世界接进来。

### 7.2 Skills

**原理**：在 `.claude/skills/` 目录下创建文件夹和 `SKILL.md` 文件。

```
.claude/skills/
├── react-component/
│   └── SKILL.md
├── fix-issue/
│   └── SKILL.md
└── deploy-preview/
    └── SKILL.md
```

**两种类型**：
- **知识型**：告诉Claude事情应该怎么做
- **工作流型**：告诉Claude按什么步骤执行

**判断标准**：一件事每天做超过一次，就应该把它变成skill。

**防止自动触发**（工作流型skill）：
```markdown
//-
disable-model-invocation: true
//-
```

**安装别人的skill**：
```bash
mkdir -p ~/.claude/skills/boris && \
curl -L -o ~/.claude/skills/boris/SKILL.md \
https://howborisusesclaudecode.com/api/install
```

### 7.3 Hooks

**核心区别**：CLAUDE.md是建议，Hooks是强制执行。

**生命周期钩子**：

| 钩子 | 触发时机 | 典型用途 |
|------|----------|----------|
| PreToolUse | Claude调用工具之前 | 拦截危险操作 |
| PostToolUse | Claude调用工具之后 | 自动格式化、自动测试 |
| PermissionRequest | 需要用户授权时 | 自动批准低风险操作 |
| Stop | Claude完成回合时 | 推动继续执行 |
| PostCompact | 上下文压缩后 | 注入关键指令防遗忘 |
| PermissionDenied | 分类器拒绝操作后 | 记录被拒操作、触发替代方案 |

**示例：自动格式化**
```json
// .claude/settings.json
{
  "hooks": {
    "PostToolUse": [
      {
        "matcher": "Edit|Write",
        "command": "npx eslint /-fix $CLAUDE_FILE_PATH"
      }
    ]
  }
}
```

### 7.4 MCP（Model Context Protocol）

**原理**：Claude Code的USB接口，插上不同的MCP服务器获得对应能力。

**添加MCP服务器**：
```bash
claude mcp add slack /- npx -y @modelcontextprotocol/server-slack
claude mcp list
```

**实用MCP推荐**：

| MCP | 能力 | 适用场景 |
|-----|------|----------|
| Slack MCP | 搜索/发送消息 | 自动同步进度、回复问题 |
| 数据库MCP | 直接查询数据库 | 不用手动复制SQL结果 |
| Figma MCP | 读取设计稿 | 把设计直接转成代码 |
| Sentry MCP | 获取错误日志 | Claude自动定位线上bug |
| GitHub MCP | 操作仓库/Issue/PR | 自动化项目管理 |

**配置文件**（项目根目录 `.mcp.json`）：
```json
{
  "mcpServers": {
    "slack": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-slack"],
      "env": { "SLACK_TOKEN": "${SLACK_TOKEN}" }
    }
  }
}
```

### 7.5 Plugins

Skills、Hooks、MCP的组合打包。一键安装，全部配置好。

输入 `/plugin` 浏览插件市场。

### 7.6 Slash Commands

存放在 `.claude/commands/` 目录中，可以包含内联的Bash脚本来预计算信息。

**Skills vs Commands**：
- Skills：更像「知识和能力」，Claude根据上下文自动应用
- Commands：更像「宏」，包含预计算步骤，强调执行流程

---

## 8. 多Agent协作

### 8.1 为什么需要并行

Claude Code的工作模式是「你给任务 → Claude花几分钟执行 → 你review结果 → 给下一个任务」。只开一个session，大部分时间在等。开5个session，等待时间几乎降到零。

### 8.2 Git Worktrees：并行的基础设施

```bash
# 启动一个在独立worktree中运行的Claude session
claude /-worktree

# 在Tmux会话中启动（可以后台运行）
claude /-worktree /-tmux
```

**Tmux集成**：
```bash
# ~/.zshrc 中添加
alias za="tmux select-window -t claude:0"
alias zb="tmux select-window -t claude:1"
alias zc="tmux select-window -t claude:2"
```

### 8.3 Subagents

在 `.claude/agents/` 目录下放 `.md` 文件定义：

```
.claude/agents/
├── security-reviewer.md
├── code-simplifier.md
├── verify-app.md
└── code-architect.md
```

**核心价值**：独立上下文，不消耗主session的上下文空间。

### 8.4 Agent Teams

多个session之间能互相通信、协调分工。

**Writer/Reviewer模式**：
```
1. Writer Agent 写代码
2. Reviewer Agent 审代码
3. Writer根据反馈修改
4. 形成迭代循环
```

**测试驱动模式**：一个agent写测试，另一个写实现（AI版TDD）。

**四阶段协调**：
- Research：多个worker并行调查代码库
- Synthesis：coordinator综合发现生成规格说明
- Implementation：worker按规格做精准修改
- Verification：验证结果

### 8.5 Fan-out批处理

**非交互模式**：
```bash
claude -p "把这个文件从JavaScript迁移到TypeScript"
```

**批量处理**：
```bash
for file in $(cat files-to-migrate.txt); do
  claude -p "Migrate $file from JS to TS" \
  /-allowedTools "Edit,Bash(git commit *)" &
done
```

**/batch命令**：
```
1. 交互式规划：告诉Claude想做什么，Claude分析项目列出所有文件
2. 确认执行：review计划后Claude启动数十个agent并行执行
3. 汇总结果：所有agent完成后汇总成功/失败情况
```

### 8.6 异步工作

| 功能 | 用途 |
|------|------|
| Remote Control | 手机上远程创建和管理本地session |
| claude.ai/code | 浏览器中运行，不需要安装 |
| /schedule | 云端定时任务，关机也照跑 |
| /loop | 本地长时间运行，最多3天无人值守 |

---

## 9. 实战：从零构建完整产品

### 9.1 项目示例：AI周报助手

**目标**：连接GitHub，获取本周所有commit，用AI总结成可读的周报页面。

### 9.2 Phase 0：需求分析（先别写代码）

```
让Claude采访你，问清楚目标用户、核心功能、技术约束。
采访结束后让Claude整理成SPEC.md。
```

**为什么用新session执行**：需求分析消耗大量上下文，确认完SPEC.md后开新session开始开发。

### 9.3 Phase 1：项目初始化

```
Create a Next.js project called weekly-report with Tailwind CSS.
Set up the basic folder structure following SPEC.md requirements.
Include TypeScript, and configure ESLint.
```

**配置CLAUDE.md**（见§5.5示例格式）。

### 9.4 Phase 2：正式开发

1. **Plan模式聊一轮架构**
2. **确认方案后退出Plan模式**
3. **让Claude逐步实现**

---

## 10. 心智模型与最佳实践

### 10.1 核心理念

```
Copilot 补全 → Cursor 对话 → Claude Code 终端Agent
```

### 10.2 信任但验证

让Claude去做，但每一步检查结果：
- 方案阶段仔细看，确保方向对
- 编码阶段扫一眼文件结构
- 运行阶段看输出符不符合预期
- 改进阶段多测试边界情况

### 10.3 迭代飞轮

```
第1周：空文件 → Claude犯很多错
第2周：护栏初现 → 错误率开始下降
第1个月：飞轮启动 → 输出质量明显提升
之后：持续迭代 → 偶尔加新规则，偶尔删过时
```

### 10.4 并行工作心智模型

把并行工作想象成管理一个远程团队：
- 不需要盯着每个人写每一行代码
- 但需要清楚每个人在做什么、进度如何
- 你的工作从写代码变成了做项目管理

### 10.5 从业者数据

- **Boris Cherny**（Claude Code创建者）：47天里46天在用，最长单次session跑了1天18小时50分钟
- **团队提效**：使用Claude Code的团队平均提效2-5倍
- **增长速度**：GA后仅6个月达到10亿美元年化收入

### 10.6 常见问题速查

| 问题 | 解决方案 |
|------|----------|
| 代码看不懂 | 「解释一下这个文件的实现逻辑」 |
| 写错了 | 描述现象，不定位代码行 |
| 该管多少 | 信任但验证 |
| 跑偏了 | Esc停止，纠正两次不行就 /clear 重来 |
| 会话太长 | /clear 清空，不同任务不同会话 |

---

## 附录

### A. 快捷命令速查

```
/clear       - 清空当前会话
/compact     - 压缩上下文
/btw         - 侧链提问
/voice       - 语音模式
/permissions - 权限管理
/plan        - Plan模式
/rewind      - 回滚对话/代码
/schedule    - 定时任务
/loop        - 长时间运行
```

### B. Claude Code背后的模型

| 模型 | 定位 | 适用场景 |
|------|------|----------|
| Opus 4.6 | 推理能力最强 | 复杂任务和架构决策 |
| Sonnet 4.6 | 性价比最优 | 日常编码主力 |
| Haiku 4.5 | 响应最快 | 简单查询和补全 |

### C. 资源链接

- 官方文档：docs.anthropic.com/claude-code
- 安装脚本：claude.ai/install.sh
- 下载：claude.ai/download
- Web版：claude.ai/code

---

*本文档基于花生《Claude Code学习》整理重构，2026年4月*
