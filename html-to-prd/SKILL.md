---
name: html-to-prd
description: Reverse-engineer existing HTML/CSS/JavaScript pages, static frontend prototypes, Axure-exported HTML, index.html files, routes, forms, tables, dashboards, or management pages into a structured Chinese HTML Product Requirements Document with automatically captured component screenshots and illustrated explanations. Use when the user provides HTML code or frontend project files and asks to generate a PRD, HTML PRD, product requirements document, requirements explanation, field inventory, action inventory, button interaction logic, component display logic, UI analysis, API/data dependency analysis, screenshot-based PRD, or to infer implemented product functions from existing HTML/UI code.
---

# HTML to PRD

## Role

Act as a senior product manager and frontend product analyst. Read existing HTML, CSS, and JavaScript code, then generate a Chinese HTML PRD that describes what the current page or prototype already implements, with automatically captured component screenshots embedded in the document for concrete requirement descriptions.

Do not invent hidden business logic. If something cannot be determined from code, mark it as `待确认`. Clearly distinguish `代码已实现`, `根据页面推测需要`, and `待确认`.

## Workflow

### 1. Identify Scope

Determine the analysis boundary before writing the PRD:

- Single page or multi-page system.
- Static HTML or JavaScript-driven interactions.
- Presence of forms, tables, filters, modals, navigation, tabs, upload controls, pagination, sorting, or CRUD actions.
- APIs through `fetch`, `axios`, `XMLHttpRequest`, `<form action>`, WebSocket, config objects, route params, query params, or hash routing.
- Role or permission hints such as hidden controls, disabled states, admin labels, permission menus, or conditional rendering.
- Mock data, hardcoded data, fixtures, placeholder content, or generated prototype artifacts.

If files are incomplete, continue with available code and mark missing parts as `待确认`.

### 2. Extract Product Structure

Collect and group product information from DOM structure, visible text, attributes, scripts, styles, and filenames.

For page metadata, extract:

- Page title.
- Main headings.
- Navigation/menu items.
- Breadcrumbs.
- Main content regions.
- Footer and auxiliary regions.

For functional modules, group UI into product modules such as:

- 登录/注册
- 用户管理
- 数据看板
- 查询筛选
- 列表管理
- 表单创建/编辑
- 详情查看
- 上传/导出
- 审批流
- 设置中心

When describing any UI component in the PRD, always use the full naming format `页面-模块-组件名称`. Do not refer to a component by component name alone. Apply this to fields, buttons, tabs, tables, filters, modals, upload controls, navigation items, cards, charts, pagination controls, and any other UI element. If the page or module name is unclear, use `待确认页面` or `待确认模块` rather than omitting the hierarchy.

For fields, analyze `label`, `placeholder`, `name`, `id`, `aria-label`, `title`, `required`, `pattern`, `maxlength`, `min`, `max`, `type`, `data-*`, option text, and visible copy.

Field inventory table:

| 字段名称 | 组件路径 | 控件类型 | 是否必填 | 默认值 | 校验规则 | 选项值 | 业务含义 |
|---|---|---|---|---|---|---|---|

For tables or lists, extract:

| 列名 | 组件路径 | 数据含义 | 格式 | 是否可排序 | 是否可筛选 | 行操作 |
|---|---|---|---|---|---|---|

For actions, include buttons, links, form submit, search/reset, add/edit/delete, detail view, import/export, upload/download, pagination, sorting, tab switching, modal open/close, and menu navigation. Button interaction logic is a priority: describe each button's trigger conditions, enabled/disabled states, click behavior, data changes, UI state changes, loading behavior, success feedback, failure feedback, validation dependencies, navigation, and API calls.

Action inventory table:

| 操作名称 | 组件路径 | 触发元素 | 前置条件 | 启用/禁用逻辑 | 用户动作 | 系统响应 | 状态变化 | 成功反馈 | 失败反馈 | 跳转/接口 |
|---|---|---|---|---|---|---|---|---|---|---|

For component display logic, describe how each important component appears, hides, changes state, or renders data. Cover:

- 默认显示状态.
- 显示/隐藏条件.
- 启用/禁用条件.
- 数据来源 and data binding.
- Loading, empty, error, disabled, selected, expanded, collapsed, hover/focus, and validation states.
- Conditional rendering caused by route, tab, permission, form value, API result, local storage, mock data, or JavaScript state.
- Mark unclear display logic as `待确认`.

Component display logic table:

| 组件路径 | 组件类型 | 默认状态 | 显示/隐藏逻辑 | 启用/禁用逻辑 | 数据来源 | 状态变化 | 空/错/加载状态 | 权限/条件控制 |
|---|---|---|---|---|---|---|---|---|

### 3. Extract APIs and Data Dependencies

Look for real APIs and client-side data sources:

- `fetch(...)`
- `axios.get/post/put/delete`
- `XMLHttpRequest`
- `<form action="...">`
- WebSocket
- Hardcoded API base URLs
- JSON fixtures or mock data
- `localStorage` / `sessionStorage`
- Query parameters and URL hash routing

Use this table:

| 接口用途 | 方法 | 路径 | 触发场景 | 请求参数 | 响应字段 | 是否真实接口 | 备注 |
|---|---|---|---|---|---|---|---|

If no real API is found but the page clearly requires backend support, list it under `建议接口 / 待确认`, not as implemented.

### 4. Automatically Capture Component Screenshots

The PRD must be illustrated with real screenshots captured during the PRD generation process. For each concrete page-level requirement and important component description, automatically capture and embed a component screenshot using the same `页面-模块-组件名称` path.

Required screenshot workflow:

- Open or serve the HTML/prototype/project locally. Use a browser automation workflow when possible.
- Render each relevant page state needed by the PRD.
- Capture screenshots for key pages, modules, and components before writing the final HTML PRD.
- Prefer focused component screenshots over full-page screenshots when describing fields, tables, modals, filters, upload controls, charts, and action areas.
- Capture alternate states when needed, such as modal open, dropdown expanded, validation error, empty state, filtered result, or selected tab.
- Save screenshots next to the generated HTML PRD under a `screenshots/` folder, and reference them with relative paths from the HTML file.
- Use meaningful filenames derived from the component path, such as `用户管理-查询筛选-搜索按钮.png`.
- Verify that every `<img>` referenced by the HTML PRD exists on disk and is viewable through the generated HTML.
- Verify screenshot-to-description alignment before completion. For every figure, the image content must match the adjacent `页面-模块-组件名称`, `alt`, caption, and requirement text below it.
- If a screenshot shows a different component, page area, module, state, or button than the requirement describes, treat it as invalid. Re-locate the component, re-render the required UI state, re-capture the screenshot, replace the image reference, and re-check the alignment.
- Prefer component-level element screenshots. Use full-page screenshots only when the requirement describes the whole page or a broad layout region, and annotate the caption so the described component is unmistakable.

Do not use screenshot placeholders. If a real screenshot cannot be captured because the code is incomplete, dependencies are missing, rendering fails, or a component cannot be reached, stop and report the blocker with the missing screenshot list instead of delivering a completed PRD.

## PRD Output Structure

Output a standalone HTML document in Chinese unless the user requests otherwise. Do not output the PRD as a Markdown document.

```html
<!doctype html>
<html lang="zh-CN">
<head>
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <title>[产品/页面名称] 产品需求文档</title>
  <style>
    body { font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", sans-serif; line-height: 1.65; color: #1f2937; margin: 0; background: #f8fafc; }
    .layout { display: grid; grid-template-columns: 280px minmax(0, 1fr); min-height: 100vh; }
    .layout.toc-collapsed { grid-template-columns: 0 minmax(0, 1fr); }
    .toc { position: sticky; top: 0; height: 100vh; overflow: auto; border-right: 1px solid #d1d5db; background: #fff; padding: 16px; transition: transform .2s ease, padding .2s ease; }
    .layout.toc-collapsed .toc { transform: translateX(-100%); padding-left: 0; padding-right: 0; border-right: 0; }
    .toc-header { display: flex; align-items: center; justify-content: space-between; gap: 8px; margin-bottom: 12px; }
    .toc-title { margin: 0; font-size: 15px; font-weight: 700; color: #111827; }
    .icon-button { width: 34px; height: 34px; display: inline-flex; align-items: center; justify-content: center; border: 1px solid #cbd5e1; background: #fff; color: #111827; border-radius: 6px; padding: 0; cursor: pointer; }
    .icon-button svg { width: 18px; height: 18px; stroke: currentColor; stroke-width: 2; fill: none; stroke-linecap: round; stroke-linejoin: round; }
    .toc ol { list-style: none; padding: 0; margin: 0; }
    .toc li { margin: 4px 0; }
    .toc a { display: block; color: #374151; text-decoration: none; padding: 6px 8px; border-radius: 6px; font-size: 14px; }
    .toc a:hover { background: #f3f4f6; color: #0f766e; }
    .content-wrap { min-width: 0; }
    .floating-toc-toggle { position: fixed; left: 12px; top: 12px; z-index: 20; box-shadow: 0 4px 12px rgba(15, 23, 42, .12); }
    main { max-width: 1180px; margin: 0 auto; padding: 32px 24px 64px; background: #fff; }
    h1, h2, h3, h4 { color: #111827; line-height: 1.3; }
    table { width: 100%; border-collapse: collapse; margin: 16px 0 24px; font-size: 14px; }
    th, td { border: 1px solid #d1d5db; padding: 8px 10px; vertical-align: top; }
    th { background: #f3f4f6; text-align: left; }
    figure { margin: 16px 0 24px; border: 1px solid #d1d5db; background: #f9fafb; padding: 12px; }
    figure img { max-width: 100%; height: auto; display: block; border: 1px solid #e5e7eb; background: #fff; }
    figcaption { margin-top: 8px; color: #4b5563; font-size: 13px; }
    .component-path { font-weight: 600; color: #0f766e; }
    .status { display: inline-block; padding: 2px 8px; border-radius: 999px; background: #e0f2fe; color: #075985; font-size: 12px; }
    @media (max-width: 860px) {
      .layout { display: block; }
      .toc { position: fixed; z-index: 15; width: 280px; box-sizing: border-box; box-shadow: 0 12px 30px rgba(15, 23, 42, .18); }
      .layout.toc-collapsed .toc { transform: translateX(-100%); }
      main { padding-top: 56px; }
    }
  </style>
</head>
<body>
<div class="layout" id="prdLayout">
<aside class="toc" aria-label="PRD目录">
  <div class="toc-header">
    <p class="toc-title">目录</p>
    <button class="icon-button toc-toggle" type="button" data-toggle-toc aria-label="隐藏目录" title="隐藏目录">
      <svg viewBox="0 0 24 24" aria-hidden="true"><path d="M15 6l-6 6 6 6"></path></svg>
    </button>
  </div>
  <nav>
    <ol>
      <li><a href="#doc-note">1. 文档说明</a></li>
      <li><a href="#overview">2. 产品概述</a></li>
      <li><a href="#ia">3. 页面结构 / 信息架构</a></li>
      <li><a href="#implemented-features">4. 已实现功能清单</a></li>
      <li><a href="#flows">5. 用户流程</a></li>
      <li><a href="#page-requirements">6. 页面级需求说明</a></li>
      <li><a href="#data-fields">7. 数据字段与枚举</a></li>
      <li><a href="#apis">8. API / 数据接口清单</a></li>
      <li><a href="#non-functional">9. 非功能需求</a></li>
      <li><a href="#analytics">10. 埋点与指标建议</a></li>
      <li><a href="#acceptance">11. 验收标准</a></li>
      <li><a href="#risks">12. 风险与待确认问题</a></li>
    </ol>
  </nav>
</aside>
<div class="content-wrap">
<button class="icon-button floating-toc-toggle" type="button" data-toggle-toc aria-label="显示目录" title="显示目录">
  <svg viewBox="0 0 24 24" aria-hidden="true"><path d="M4 6h16"></path><path d="M4 12h16"></path><path d="M4 18h16"></path></svg>
</button>
<main>
  <h1>[产品/页面名称] 产品需求文档</h1>

  <section id="doc-note">
    <h2>1. 文档说明</h2>
    <ul>
      <li>文档来源：基于现有 HTML/CSS/JavaScript 代码反向分析</li>
      <li>分析范围：</li>
      <li>生成日期：</li>
      <li>可信度说明：</li>
      <li>待确认事项摘要：</li>
      <li>输出格式：HTML 图文需求文档</li>
    </ul>
  </section>

  <section id="overview">
    <h2>2. 产品概述</h2>
    <p><strong>产品定位：</strong></p>
    <p><strong>目标用户：</strong></p>
    <p><strong>核心使用场景：</strong></p>
    <p><strong>页面/系统主要价值：</strong></p>
  </section>

  <section id="ia">
    <h2>3. 页面结构 / 信息架构</h2>
    <ul>
      <li>页面清单：</li>
      <li>导航结构：</li>
      <li>页面区域说明：</li>
      <li>页面之间的跳转关系：</li>
      <li>组件命名规则：所有组件描述必须使用 <code>页面-模块-组件名称</code></li>
    </ul>
  </section>

  <section id="implemented-features">
    <h2>4. 已实现功能清单</h2>
    <table>
      <thead>
        <tr><th>模块</th><th>功能点</th><th>功能说明</th><th>页面/区域</th><th>状态</th></tr>
      </thead>
      <tbody></tbody>
    </table>
  </section>

  <section id="flows">
    <h2>5. 用户流程</h2>
    <p>按入口、用户操作、系统反馈、结果、异常情况描述主流程。</p>
  </section>

  <section id="page-requirements">
    <h2>6. 页面级需求说明</h2>

    <article class="page-requirement">
      <h3>6.x [页面名称]</h3>
      <h4>页面目标</h4>
      <p></p>

      <h4>页面布局</h4>
      <p></p>

      <h4>组件截图与需求描述</h4>
      <figure>
        <img src="screenshots/[页面-模块-组件名称].png" alt="[页面-模块-组件名称]截图">
        <figcaption><span class="component-path">[页面-模块-组件名称]</span>：[基于自动截图和代码分析说明该组件的用途、展示内容和交互位置]</figcaption>
      </figure>

      <h4>组件显示逻辑</h4>
      <table>
        <thead>
          <tr><th>组件路径</th><th>组件类型</th><th>默认状态</th><th>显示/隐藏逻辑</th><th>启用/禁用逻辑</th><th>数据来源</th><th>状态变化</th><th>空/错/加载状态</th><th>权限/条件控制</th></tr>
        </thead>
        <tbody></tbody>
      </table>

      <h4>字段说明</h4>
      <table>
        <thead>
          <tr><th>字段名称</th><th>组件路径</th><th>控件类型</th><th>是否必填</th><th>默认值</th><th>校验规则</th><th>选项值</th><th>业务含义</th></tr>
        </thead>
        <tbody></tbody>
      </table>

      <h4>操作说明</h4>
      <table>
        <thead>
          <tr><th>操作名称</th><th>组件路径</th><th>触发元素</th><th>前置条件</th><th>启用/禁用逻辑</th><th>用户动作</th><th>系统响应</th><th>状态变化</th><th>成功反馈</th><th>失败反馈</th><th>跳转/接口</th></tr>
        </thead>
        <tbody></tbody>
      </table>

      <h4>交互规则</h4>
      <p>重点描述每个按钮、链接、Tab、弹窗、表单控件、分页、排序、上传/下载控件的触发条件、执行步骤、状态变化、反馈信息和异常路径；代码中无法确认的内容标记为 <code>待确认</code>。</p>

      <h4>校验规则</h4>
      <p></p>

      <h4>异常/空状态</h4>
      <p></p>

      <h4>权限/状态控制</h4>
      <p></p>

      <h4>验收标准</h4>
      <p>Given / When / Then。</p>
    </article>
  </section>

  <section id="data-fields">
    <h2>7. 数据字段与枚举</h2>
    <p>列出发现的字段、选项、状态值、标签和常量。</p>
  </section>

  <section id="apis">
    <h2>8. API / 数据接口清单</h2>
    <p>分别列出真实接口与建议接口。</p>
  </section>

  <section id="non-functional">
    <h2>9. 非功能需求</h2>
    <p>仅包含代码中有证据或可由代码合理体现的兼容性、响应式布局、可访问性、性能、安全、错误处理要求；不可见则标记为 <code>待确认</code>。</p>
  </section>

  <section id="analytics">
    <h2>10. 埋点与指标建议</h2>
    <p><span class="status">建议补充</span></p>
    <table>
      <thead>
        <tr><th>事件名称</th><th>触发动作</th><th>关键属性</th><th>业务目的</th></tr>
      </thead>
      <tbody></tbody>
    </table>
  </section>

  <section id="acceptance">
    <h2>11. 验收标准</h2>
    <p>尽量使用 Given / When / Then。</p>
  </section>

  <section id="risks">
    <h2>12. 风险与待确认问题</h2>
    <p>列出歧义、缺失后端信息、不明确校验、缺失错误状态、不明确权限和不完整交互逻辑。</p>
</section>
</main>
</div>
</div>
<script>
  document.querySelectorAll('[data-toggle-toc]').forEach(function (button) {
    button.addEventListener('click', function () {
      var layout = document.getElementById('prdLayout');
      var collapsed = layout.classList.toggle('toc-collapsed');
      var sidebarToggle = document.querySelector('.toc-toggle');
      var floatingToggle = document.querySelector('.floating-toc-toggle');
      if (sidebarToggle) {
        sidebarToggle.setAttribute('aria-label', collapsed ? '显示目录' : '隐藏目录');
        sidebarToggle.setAttribute('title', collapsed ? '显示目录' : '隐藏目录');
      }
      if (floatingToggle) {
        floatingToggle.setAttribute('aria-label', collapsed ? '显示目录' : '隐藏目录');
        floatingToggle.setAttribute('title', collapsed ? '显示目录' : '隐藏目录');
      }
    });
  });
</script>
</body>
</html>
```

## Rules

- Generate the final PRD as an `.html` file or standalone HTML content. Do not generate a Markdown PRD unless the user explicitly asks for Markdown.
- Put the PRD table of contents on the left side of the page. It must link to the main sections and support collapse/hide behavior with icon-only toggle buttons. Do not use visible text labels such as `隐藏` or `目录` inside the toggle buttons; use icons plus `aria-label` and `title`.
- Make the PRD illustrated with automatically captured screenshots: every concrete requirement description for a component must include an adjacent real screenshot or focused component image.
- Do not use deferred screenshot labels, screenshot placeholders, or broken image references. If required screenshots cannot be captured, stop and report the blocker instead of delivering a completed PRD.
- Pair screenshots with component paths and captions. The caption must include the full `页面-模块-组件名称` path and a concise explanation of what the image proves.
- Verify before completion that each `<img src="...">` in the HTML PRD points to an existing screenshot file that can be viewed when the HTML file is opened.
- Verify before completion that every screenshot matches the component being described immediately next to or below it. The screenshot, filename, `alt`, caption, component path, and requirement description must all refer to the same page-module-component and UI state.
- If any screenshot does not match its component description, do not deliver the PRD. Re-capture the correct component/state, update the HTML image reference, and repeat the alignment check.
- Treat button interaction logic and component display logic as priority content, not optional detail.
- For every button or clickable action, specify trigger conditions, enabled/disabled logic, click result, state changes, validation dependencies, success feedback, failure feedback, navigation, and API calls.
- For every important component, specify default state, display/hide logic, enabled/disabled logic, data source, state changes, empty/error/loading states, and permission or condition controls.
- Avoid vague interaction descriptions such as `点击后执行操作` or `显示组件`. Replace them with concrete behavior found in code, or mark the missing detail as `待确认`.
- Base claims only on code, visible UI, attributes, filenames, routes, config, or hardcoded data.
- Do not claim an API exists unless it is found in code.
- Do not convert suggested backend needs into implemented requirements.
- Describe every component with the full `页面-模块-组件名称` path; never use only the component name. If page or module cannot be determined, use `待确认页面` or `待确认模块` in the path.
- Use product-manager-friendly Chinese.
- When code cannot confirm a detail, use cautious language such as `页面显示`, `代码中可见`, `推测需要`, or `待确认`.
