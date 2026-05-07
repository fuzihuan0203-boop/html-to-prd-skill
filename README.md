# html-to-prd

把现有 HTML/CSS/JavaScript 页面、静态原型或 Axure 导出的 HTML，反向分析为一份图文并茂的中文 HTML 产品需求文档。

这个 skill 面向产品经理、AI 产品个体、前端协作者和需要从旧页面/原型快速沉淀 PRD 的团队。它强调“基于代码和页面分析”，不会凭空编造隐藏业务逻辑；无法从代码确认的内容会标记为 `待确认`。

## 作者

Aike，AI 超级个体 & AI财富自由社群创建人

个人微信：`fuzihuan`

欢迎大家来交流。

## 这个 Skill 能做什么

- 从 HTML/CSS/JS 反向生成中文产品需求文档。
- 输出 HTML PRD，而不是 Markdown 文档。
- 自动截图并嵌入 PRD，生成图文并茂的可访问页面。
- 要求每个组件都使用 `页面-模块-组件名称` 的完整路径描述。
- 精确描述每个按钮的交互逻辑。
- 精确描述每个组件的显示逻辑。
- 区分 `代码已实现`、`根据页面推测需要` 和 `待确认`。
- 提取字段、表格、按钮、筛选、弹窗、导航、上传、分页、排序、接口和数据依赖。

## 输出文档特点

生成的 PRD 是一个可直接打开访问的 HTML 文件，包含：

- 左侧可折叠目录。
- 页面结构 / 信息架构。
- 已实现功能清单。
- 用户流程。
- 页面级需求说明。
- 组件截图与需求描述。
- 组件显示逻辑。
- 字段说明。
- 操作说明。
- 交互规则。
- 校验规则。
- API / 数据接口清单。
- 非功能需求。
- 埋点与指标建议。
- 验收标准。
- 风险与待确认问题。

## 核心规则

### 1. 组件必须用完整路径描述

所有组件都必须遵循：

```text
页面-模块-组件名称
```

示例：

```text
用户管理页-查询筛选区-搜索按钮
用户管理页-用户列表区-编辑按钮
订单详情页-基础信息区-订单状态标签
```

不能只写“搜索按钮”“编辑按钮”“状态标签”。

### 2. 截图必须自动完成

PRD 中的截图必须在生成过程中自动截取、保存并嵌入 HTML。

不允许：

- 后期补充截图。
- 使用截图占位。
- 使用坏图链接。

如果页面无法运行、组件无法触达、依赖缺失或截图失败，应停止并报告阻塞原因，而不是交付一个“缺图版”PRD。

### 3. 截图必须和组件描述一致

每张截图必须和紧邻的组件路径、图片说明、需求描述一致。

必须一致的内容包括：

- 截图内容。
- 截图文件名。
- HTML `alt` 文案。
- 截图 caption。
- `页面-模块-组件名称`。
- 下方需求描述中的组件和 UI 状态。

如果截图和组件描述对不上，必须重新定位组件、重新渲染对应状态、重新截图，并替换 HTML 中的图片引用。

### 4. 按钮交互逻辑必须精确

每个按钮或可点击动作都要描述：

- 触发条件。
- 启用 / 禁用逻辑。
- 用户点击后的系统响应。
- UI 状态变化。
- 数据变化。
- 校验依赖。
- Loading 状态。
- 成功反馈。
- 失败反馈。
- 跳转或 API。

### 5. 组件显示逻辑必须精确

每个重要组件都要描述：

- 默认状态。
- 显示 / 隐藏条件。
- 启用 / 禁用条件。
- 数据来源。
- 状态变化。
- 空态。
- 错误态。
- 加载态。
- 权限或条件控制。

## 目录结构

```text
html-to-prd-skill/
├── html-to-prd/
│   ├── SKILL.md
│   └── agents/
│       └── openai.yaml
├── README.md
├── LICENSE
└── .gitignore
```

## 在 Codex 中使用

OpenAI 官方说明中提到，Skills 支持 Codex，并遵循 Agent Skills 开放标准，可在不同产品之间下载安装使用。

参考：

- [OpenAI Help: Skills in ChatGPT](https://help.openai.com/articles/20001066-skills-in-chatgpt)
- [OpenAI Help: Using Codex with your ChatGPT plan](https://help.openai.com/en/articles/11369540-codex-in-chatgpt)

### 安装到 Codex 本地 Skills

1. 克隆仓库：

```bash
git clone https://github.com/fuzihuan0203-boop/html-to-prd-skill.git
```

2. 复制 skill 到 Codex skills 目录。

macOS / Linux:

```bash
mkdir -p ~/.codex/skills
cp -R html-to-prd-skill/html-to-prd ~/.codex/skills/
```

Windows PowerShell:

```powershell
New-Item -ItemType Directory -Force $env:USERPROFILE\.codex\skills
Copy-Item -Recurse -Force .\html-to-prd-skill\html-to-prd $env:USERPROFILE\.codex\skills\
```

3. 在 Codex 中提出类似请求：

```text
请使用 html-to-prd，根据这个前端项目生成一份中文 HTML PRD。要求自动截图、图文并茂，并精确描述每个按钮交互逻辑和组件显示逻辑。
```

### 推荐输入

- 单个 `index.html`
- HTML + CSS + JavaScript
- Axure 导出的 HTML
- 静态页面原型
- 前端项目目录
- 管理后台页面
- 表单页 / 列表页 / 看板页 / 详情页

## 在 Claude Code 中使用

Anthropic 官方文档说明，Claude Code 支持 Agent Skills。个人 Skills 通常放在 `~/.claude/skills/skill-name/SKILL.md`，项目 Skills 可放在 `.claude/skills/skill-name/SKILL.md`。

参考：

- [Claude Docs: Agent Skills](https://docs.claude.com/en/docs/claude-code/skills)
- [Claude Code overview](https://docs.anthropic.com/en/docs/claude-code/overview)

### 安装为个人 Skill

1. 克隆仓库：

```bash
git clone https://github.com/fuzihuan0203-boop/html-to-prd-skill.git
```

2. 复制到 Claude Code skills 目录：

macOS / Linux:

```bash
mkdir -p ~/.claude/skills
cp -R html-to-prd-skill/html-to-prd ~/.claude/skills/
```

Windows PowerShell:

```powershell
New-Item -ItemType Directory -Force $env:USERPROFILE\.claude\skills
Copy-Item -Recurse -Force .\html-to-prd-skill\html-to-prd $env:USERPROFILE\.claude\skills\
```

3. 进入你的前端项目：

```bash
cd your-frontend-project
claude
```

4. 提出请求：

```text
使用 html-to-prd，把当前 HTML 原型反向生成中文 HTML PRD。必须自动截图并嵌入文档，重点写清楚每个按钮的交互逻辑和每个组件的显示逻辑。
```

### 安装为项目 Skill

如果希望团队在某个项目中共享这个 skill，可以复制到项目内：

```bash
mkdir -p .claude/skills
cp -R html-to-prd-skill/html-to-prd .claude/skills/
```

然后提交 `.claude/skills/html-to-prd/SKILL.md` 到项目仓库。

## 示例 Prompt

```text
请根据当前目录下的 index.html、CSS 和 JS，生成一份中文 HTML PRD。

要求：
1. 自动运行或打开页面并截图。
2. 截图保存到 screenshots/ 目录并嵌入 PRD。
3. PRD 使用 HTML 页面输出，左侧带可折叠目录。
4. 所有组件使用“页面-模块-组件名称”描述。
5. 重点说明每个按钮的交互逻辑。
6. 重点说明每个组件的显示逻辑。
7. 不要编造代码中没有证据的业务规则，不确定的内容标记为“待确认”。
```

## 适用场景

- 从旧系统页面补 PRD。
- 从外包交付的 HTML 页面反推需求。
- 从 Axure HTML 原型整理正式产品文档。
- 从前端代码梳理字段、按钮、交互、接口。
- 产品经理快速接手存量系统。
- AI 辅助生成图文并茂的产品说明。

## 注意事项

- 这个 skill 不会替代真实业务访谈，它只基于代码和可见页面证据生成文档。
- 如果页面依赖后端接口但无法访问，接口细节会标记为 `待确认`。
- 如果截图无法自动完成，skill 要求停止并报告阻塞原因。
- 自动截图通常需要代理具备浏览器自动化能力，并能在本地运行或打开目标页面。

## License

MIT License
