# Photoshop Harness

> CLI-Anything Photoshop —— 通过 Windows COM 自动化操控 Adobe Photoshop 的 AI Agent 工具集。

## 概述

Photoshop Harness 是 CLI-Anything 生态的 Photoshop 适配器，让 AI Agent 可以通过**命令行 + COM 自动化接口**直接操控 Photoshop，实现图像编辑、设计稿生成、批量处理等任务的自动化。

## 目录结构

```
photoshop-harness/
├── agent-harness/                         # Python Agent 代码
│   ├── setup.py                           #   pip 安装配置
│   ├── PHOTOSHOP.md                       #   软件操作标准 (SOP)
│   └── cli_anything/
│       └── photoshop/
│           ├── __init__.py
│           ├── photoshop_cli.py           #   CLI 入口
│           ├── core/                      #   核心模块
│           │   ├── session.py             #     COM 会话管理（单例模式）
│           │   ├── project.py             #     项目管理（新建/打开/保存）
│           │   ├── document.py            #     文档操作（尺寸/分辨率/色彩）
│           │   ├── layer.py               #     图层操作（增删改/混合模式）
│           │   ├── selection.py           #     选区操作（全选/羽化/反选）
│           │   ├── text.py                #     文字图层（字体/大小/颜色）
│           │   └── export.py              #     导出（PNG/JPEG/WebP/PSD）
│           ├── utils/
│           │   └── com_bridge.py          #   Windows COM 桥接层
│           └── tests/
│               ├── test_core.py           #   核心模块单元测试
│               ├── test_full_e2e.py       #   端到端测试
│               └── TEST.md               #   测试说明
├── skills/
│   └── cli-anything-photoshop/
│       └── SKILL.md                      # Claude Code Skill 定义
└── e2e_demo.json                         # 端到端演示工作流
```

## 架构设计

```
CLI 命令 → Click CLI 层 → Core 模块 → COM Bridge → Photoshop.Application COM → Photoshop 引擎
```

### 关键设计决策

| 设计原则 | 说明 |
|---------|------|
| **COM 单例模式** | 整个会话共享一个 `Photoshop.Application` 实例，避免重复 Dispatch |
| **状态追踪** | Session 层追踪活跃文档、图层栈、选区状态；支持 undo/redo 快照 |
| **原子操作** | 每个 CLI 命令映射一个或多个 COM API 调用，保持原子性 |
| **JSON 输出** | 所有命令支持 `--json` 输出，方便 AI Agent 解析 |

## 前置条件

- **Windows 10/11**
- **Adobe Photoshop 2023+**（需注册 COM 接口）
- **Python 3.10+** + `pywin32`

## 安装

```bash
cd photoshop-harness/agent-harness
pip install -e .
```

## 命令分组

| 命令组 | 描述 | 对应 COM 对象 |
|--------|------|--------------|
| `project` | 新建/打开/保存 PSD 项目 | `Application.Documents` |
| `document` | 文档属性（尺寸、分辨率、色彩模式） | `Document` |
| `layer` | 图层操作（增删改、可见性、透明度、混合模式） | `ArtLayer`, `LayerSet` |
| `selection` | 选区操作（全选、取消、反转、羽化、扩展） | `Selection` |
| `image` | 图像调整（裁切、旋转、翻转、画布大小） | `Document` methods |
| `text` | 文字图层（添加/修改、字体、大小、颜色） | `ArtLayer.TextItem` |
| `export` | 导出（PNG、JPEG、WebP 等格式） | `Document.Export`, `SaveAs` |
| `filter` | 滤镜操作 | `Document` filter methods |

## 快速开始

```bash
# 创建新项目
cli-anything-photoshop project new output.psd -w 1920 -h 1080

# 添加文字图层
cli-anything-photoshop text add --content "Hello World" --font "Arial" --size 72 --color "#FFFFFF"

# 导出 PNG
cli-anything-photoshop export png --output result.png

# 获取当前状态（JSON 格式，供 Agent 解析）
cli-anything-photoshop --json document info
```

## 状态模型

Agent 可随时查询当前 Photoshop 状态：

```json
{
  "project_path": "path/to/file.psd",
  "active_document": "doc_name",
  "document_info": {
    "width": 1920, "height": 1080,
    "resolution": 72.0,
    "mode": "RGB",
    "bits_per_channel": 8
  },
  "layers": [
    {"name": "Background", "type": "pixel", "visible": true, "opacity": 100},
    {"name": "Title", "type": "text", "visible": true, "opacity": 100}
  ],
  "selection": {"active": false, "bounds": null}
}
```

## Claude Code 集成

安装 Skill 后，在 Claude Code 中可直接用自然语言操控 Photoshop：

```
> 帮我创建一个 1920x1080 的海报，白色背景，加标题"你好世界"
> 把当前图层的不透明度调到 50%
> 导出为 PNG，放在桌面上
```

Skill 定义见 `skills/cli-anything-photoshop/SKILL.md`。

## 测试

```bash
cd agent-harness
pytest cli_anything/photoshop/tests/ -v
```

## 许可

MIT License
