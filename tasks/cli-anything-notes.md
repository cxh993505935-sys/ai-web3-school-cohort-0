# CLI-Anything 使用笔记

> AI x Web3 School Week 1 | AI 工具实践
> 作者：Chris_TPB | 2025-05-21

---

## 一、项目简介

**CLI-Anything** 是香港大学数据科学实验室 (HKUDS) 的开源项目。

**核心理念：** Making ALL Software Agent-Native — 让所有软件对 AI Agent 原生可用。

**原理：** 为各种桌面/Web 软件生成标准化的 CLI 工具层（Harness），AI Agent 通过命令行就能操控这些软件，无需专门的 API 集成。

**仓库：** https://github.com/HKUDS/CLI-Anything
**官网：** https://clianything.cc/

---

## 二、为什么 CLI 是 Agent 的最佳接口？

1. **结构化 & 可组合** — 文本命令天然匹配 LLM 输出格式，可以串联成复杂工作流
2. **轻量 & 通用** — 跨平台，无依赖，不需要 GUI
3. **自描述** — `--help` 就是文档，Agent 能自动发现功能
4. **Agent 优先设计** — 支持 JSON 输出，无需复杂的文本解析
5. **确定性 & 可靠** — 相同命令产生一致结果

---

## 三、安装过程

### 环境准备
- 系统 Python 3.12 受 Debian 管理（externally-managed），不能直接 pip install
- 使用 `uv` 创建独立 venv 解决

### 安装步骤
```bash
# 1. 用 uv 创建 Python 3.12 venv
uv venv ~/cli-anything-env --python 3.12

# 2. 安装 cli-hub
uv pip install --python ~/cli-anything-env/bin/python cli-anything-hub

# 3. 加入 PATH
echo 'export PATH="$HOME/cli-anything-env/bin:$PATH"' >> ~/.bashrc
source ~/.bashrc

# 4. 验证
cli-hub --help
```

### 踩坑记录
- Python 3.11 有 f-string 语法限制，cli-hub 0.3.0 不兼容（f-string 不能包含反斜杠）
- 系统 Python 3.12 受 PEP 668 保护，需要 venv
- `python3.12 -m venv` 需要 `python3.12-venv` 包，无 sudo 权限时用 `uv venv` 代替

---

## 四、CLI-Hub 使用方法

### 常用命令
```bash
cli-hub list              # 查看所有可用 CLI
cli-hub search <关键词>    # 搜索 CLI
cli-hub info <名称>       # 查看某个 CLI 详情
cli-hub install <名称>    # 安装 CLI
cli-hub uninstall <名称>  # 卸载 CLI
cli-hub update <名称>     # 更新 CLI
cli-hub launch <名称>     # 启动已安装的 CLI
```

### 支持的软件分类
| 分类 | 代表应用 |
|------|---------|
| 3D | Blender, FreeCAD |
| AI | ComfyUI, Ollama, MiniMax, Dify |
| Audio | Audacity, ElevenLabs |
| Automation | n8n |
| Communication | Feishu, WeCom, Zoom, Twitter/X |
| DevOps | PM2, Sentry, iTerm2, Eth2-Quickstart |
| Diagrams | Draw.io, Mermaid |
| Game | Godot, Slay the Spire II |
| Knowledge | Obsidian, Zotero |
| Media | Kdenlive, Shotcut, VideoCaptioner |
| Office | LibreOffice, MuseScore |
| Web | Exa, CLIBrowser |

### 支持的 Agent 平台
- Claude Code
- Pi Coding Agent
- OpenClaw
- OpenCode
- Codex
- GitHub Copilot CLI
- Qodercli

---

## 五、项目架构理解

```
CLI-Anything
├── 每个软件 → 一个 Harness（CLI 包装层）
│   ├── CLI 命令定义（Click 框架）
│   ├── SKILL.md（Agent 可发现的技能描述）
│   ├── 单元测试 + E2E 测试
│   └── JSON + Human 双输出格式
├── CLI-Hub（包管理器）
│   ├── pip 安装
│   ├── 浏览 / 搜索 / 安装 / 更新
│   └── 社区贡献注册表
└── 社区驱动
    ├── 任何人都可以提交新 CLI
    ├── PR 审核后合并
    └── 每个 CLI 独立维护
```

---

## 六、与 AI × Web3 的关联思考

CLI-Anything 的设计理念和 MCP（Model Context Protocol）高度相关：

| 维度 | CLI-Anything | MCP |
|------|-------------|-----|
| 接口标准 | 命令行（文本） | JSON-RPC 协议 |
| 发现机制 | `--help` + SKILL.md | Tool schema |
| 输出格式 | JSON + Human | JSON |
| 目标 | 让 Agent 操控任意软件 | 让 Agent 连接任意工具 |
| 社区 | GitHub PR | MCP Server 注册表 |

**对 Web3 的启发：**
- 链上工具（合约交互、区块浏览器、DeFi 协议）也可以做成 CLI Harness
- Agent 可以通过 CLI 直接操作链上资产，不需要专门的 Web3 SDK 集成
- `eth2-quickstart` CLI 已经是一个 Web3 相关的 CLI 实践

---

## 七、个人收获

1. **CLI 是 Agent 时代的"万能接口"** — 比 API 更轻量，比 GUI 更易自动化
2. **SKILL.md 模式值得学习** — 让 Agent 自动发现工具能力，类似 MCP 的 tool schema
3. **社区驱动的扩展模式** — 任何人都能贡献，降低参与门槛
4. **测试很重要** — 每个 CLI 都有完整的单元测试和 E2E 测试，2269 个测试全部通过
5. **踩坑经验** — Python 版本兼容性、venv 管理、uv 工具链的使用

---

## 八、参考链接

- 仓库：https://github.com/HKUDS/CLI-Anything
- CLI-Hub：https://hkuds.github.io/CLI-Anything/
- 文档：https://clianything.cc/
- 安装：`pip install cli-anything-hub`

---

> 📝 本文记录了 CLI-Anything 项目的安装、使用和理解，作为 Week 1 AI 工具实践的 PoW 材料。
