# Illustrate Harness

> Claude Code 全栈智能代理框架 —— 规则、代理、命令、钩子的统一编排系统。

## 概述

Illustrate Harness 是 Claude Code 的核心扩展框架，提供了一套完整的 **规则引擎**、**多代理系统**、**自定义命令** 和 **钩子机制**，让 AI 编码助手从"聊天工具"升级为"自主开发平台"。

## 目录结构

```
illustrate-harness/
├── CLAUDE.md                  # 核心指令文件（行为准则 + 环境配置）
├── AGENTS.md                  # 多代理编排指南
├── rules/                     # 规则引擎（30+ 规则）
│   ├── ecc/                   #   ECC 扩展规则（zh/ common/ python/ web/ ...）
│   ├── plan-first-workflow.md #   规划优先工作流
│   ├── orchestrator-protocol.md#   编排器协议
│   ├── cross-artifact-review.md#   交叉产物审查
│   ├── quality-gates.md       #   质量门禁
│   ├── session-logging.md     #   会话日志
│   ├── tikz-*.md              #   TikZ 图表规范
│   └── ...                    #   更多规则
├── agents/                    # 代理定义（70+ 专业代理）
│   ├── architect.md           #   软件架构师
│   ├── code-reviewer.md       #   代码审查
│   ├── security-reviewer.md   #   安全分析
│   ├── tdd-guide.md           #   测试驱动开发
│   ├── planner.md             #   实现规划
│   ├── python-reviewer.md     #   Python 审查
│   ├── rust-reviewer.md       #   Rust 审查
│   ├── ...                    #   更多代理
├── commands/                  # 自定义命令（80+ 命令）
│   ├── code-review.md         #   /code-review
│   ├── plan.md                #   /plan
│   ├── commit.md              #   /commit
│   ├── feature-dev.md         #   /feature-dev
│   └── ...                    #   更多命令
├── hooks/                     # 钩子系统
│   ├── hooks.json             #   钩子配置
│   ├── README.md              #   钩子使用说明
│   └── memory-persistence/    #   记忆持久化钩子
├── ecc/                       # ECC 扩展（多语言/多框架）
│   ├── common/                #   通用规则（编码风格/安全/测试/Git）
│   ├── python/                #   Python 特定规则
│   ├── web/                   #   Web 前端规则
│   ├── zh/                    #   中文翻译规则
│   └── ...                    #   更多语言
└── mcp-configs/               # MCP 服务器配置
    └── mcp-servers.json       #   MCP 服务列表
```

## 核心能力

### 1. 规则引擎 (Rules)
- **30+ 行为规则**，覆盖：编码风格、Git 工作流、安全、测试、性能、代理编排
- 支持 **语言特定规则**（Python / TypeScript / Rust / Go / C++ / Java / Swift …）
- 支持 **中文规则** (`rules/ecc/zh/`)
- **规划优先工作流**：非平凡任务自动进入 Plan Mode
- **质量门禁**：80/90/95 分阈值体系

### 2. 多代理系统 (Agents)
- **70+ 专业代理**，按需调用：
  - 🏗️ **设计类**：architect, planner, code-architect
  - 🔍 **审查类**：code-reviewer, security-reviewer, python-reviewer, rust-reviewer ...
  - ✅ **测试类**：tdd-guide, e2e-runner, pr-test-analyzer
  - 🔧 **构建类**：build-error-resolver, cpp-build-resolver, go-build-resolver ...
  - 📄 **文档类**：doc-updater, docs-lookup, comment-analyzer
  - 🎨 **前端类**：ui-ux-pro-max, frontend-design, slide-auditor
  - 🔬 **科研类**：domain-reviewer, tikz-reviewer, pedagogy-reviewer
- 支持 **并行代理**、**多视角分析**、**对抗验证**

### 3. 自定义命令 (Commands)
- **80+ 命令**：`/code-review`, `/plan`, `/commit`, `/feature-dev`, `/security-scan` ...
- 覆盖完整开发流程：规划 → 实现 → 审查 → 测试 → 提交 → 部署

### 4. 钩子系统 (Hooks)
- **PreToolUse**：工具调用前验证
- **PostToolUse**：自动格式化、代码检查
- **Stop**：会话结束验证（构建检查）
- **记忆持久化**：跨会话知识保留

### 5. ECC 扩展 (ECC)
- **多语言覆盖**：Python, TypeScript, Rust, Go, C++, Java, Kotlin, Swift, F#, Dart, PHP, Ruby …
- **Web 前端**：React, CSS, 动画, 性能, 无障碍
- **Angular / ArkTS / HarmonyOS** 等框架支持

## 快速开始

### 安装

```bash
# 克隆仓库
git clone https://github.com/yb2460/cli-anything-wps.git
cd cli-anything-wps

# 复制 harness 到 Claude Code 配置目录
cp -r illustrate-harness/* ~/.claude/
```

### 配置

编辑 `~/.claude/CLAUDE.md`，填写你的环境信息：

```markdown
| **GitHub 账号** | your-username | Token: `<YOUR_GITHUB_TOKEN>` |
| **Python 路径** | `/path/to/python` | 自定义安装 |
```

### 使用

安装后，Claude Code 自动加载所有规则和代理。常用命令：

```
/code-review     # 代码审查
/plan            # 制定实现计划
/commit          # 规范化提交
/feature-dev     # 完整功能开发流程
/security-scan   # 安全扫描
```

## 进阶

- **自定义规则**：在 `rules/` 下添加 `.md` 文件
- **创建代理**：在 `agents/` 下添加代理定义文件
- **添加命令**：在 `commands/` 下添加命令文件
- **配置钩子**：编辑 `hooks/hooks.json`

详见 `AGENTS.md` 和 `PLUGIN_SCHEMA_NOTES.md`。

## 许可

MIT License
