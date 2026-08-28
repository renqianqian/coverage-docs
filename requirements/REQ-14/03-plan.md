# REQ-14 实现方案

## 目标

在 `project-demo` 仓库内,从空仓库(仅有 `.gitignore` / `README.md`)初始化一个 Vite + React 18 工程,并交付一个**只读 mock 项目列表卡片网格**。

## 技术决策

| 项 | 选择 | 理由 |
|----|------|------|
| 框架 | React 18 | 需求指定 React |
| 构建工具 | Vite 5 | 官方推荐 React 启动器,启动快 |
| 语言 | JavaScript(无 TS) | 需求未指定 TS,且是 demo,降低门槛 |
| 样式 | 原生 CSS | 需求极简,不引入 UI 库;`auto-fill + minmax` 一段 grid 即可 |
| 数据 | 本地 mock 常量 | 需求明确"全部 mock" |

## 目录结构

```
project-demo/
├── .gitignore
├── README.md            # 含启动 / 构建 / 目录说明
├── index.html           # Vite 入口
├── package.json         # react / react-dom / vite / @vitejs/plugin-react
├── vite.config.js       # vite + @vitejs/plugin-react
└── src/
    ├── main.jsx         # ReactDOM.createRoot(StrictMode)
    ├── App.jsx          # 渲染卡片网格
    ├── index.css        # 全局 + .grid / .card 样式
    └── data/
        └── projects.js  # 6 条 mock 数据,字段 id + name
```

## 渲染逻辑(`src/App.jsx`)

```
<div className="page">
  <header>
    <h1>项目管理</h1>
    <p>共 N 个项目(只读 mock)</p>
  </header>
  <section className="grid">
    {projects.map(p => (
      <article className="card" key={p.id}>
        <div className="card__id">{p.id}</div>
        <div className="card__name">{p.name}</div>
      </article>
    ))}
  </section>
</div>
```

## 样式要点

- `.grid`:`display: grid; grid-template-columns: repeat(auto-fill, minmax(220px, 1fr)); gap: 16px;`
- `.card`:白底 + 1px 边 + 8px 圆角,`hover` 时轻微上浮阴影
- `.card__id` 用等宽字体 + 灰色,模拟"次要元信息"的视觉权重
- 容器最大宽度 `1080px`,居中

## Mock 数据(`src/data/projects.js`)

| id | name |
|----|------|
| P-1001 | 官网重构 |
| P-1002 | 内部 CRM 升级 |
| P-1003 | 移动端 App V2 |
| P-1004 | 数据中台一期 |
| P-1005 | 营销自动化平台 |
| P-1006 | AI 客服接入 |

## 分支与提交

- 分支:`req/REQ-14/main`(基于 `project-demo@main` = `0e2b910b`)
- 单次提交:`feat(REQ-14): init project list (read-only mock)`
- SHA:`0def64ecbd75285eb43eaed82e3e128144c336c4`
- 未推送到远端(由用户决定是否 push / 提 MR)

## 本地验证

```bash
git clone https://github.com/renqianqian/project-demo
git checkout req/REQ-14/main
npm install
npm run dev
# 浏览器访问 http://localhost:5173,看到 6 张卡片
```

## 不在本次范围内

- 任何后端 / API 集成
- 路由(单页足够)
- 测试(Vite 默认未配置 jest/vitest,本次未加;后续若要单元测试,引入 vitest + @testing-library/react)
- TypeScript
- 任何 UI 组件库(Antd / MUI / shadcn)