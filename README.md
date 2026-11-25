# TaskHub — Vue 3 任务管理系统

期中测试案例「Vue3 任务管理系统」的可运行实现，按案例文档逐步搭建：任务看板、分类筛选、数据持久化、进度统计、登录与路由守卫。

## 技术栈

- Vue 3（`<script setup>` 组合式 API）
- Vite 8（Rolldown 构建）
- Pinia（状态管理 + LocalStorage 持久化）
- Vue Router（登录守卫）
- Tailwind CSS v4（`@tailwindcss/vite` 插件）

## 本地运行

```bash
npm install
npm run dev        # http://localhost:5173/
```

默认账号逻辑为模拟登录：未登录访问 `/` 会被守卫重定向到 `/login`，点击「一键进入系统」即可进入工作台。

## 目录结构

```
src/
├── assets/          # 静态资源
├── components/      # TaskHeader / TaskInput / TaskFilter / TaskItem
├── composables/     # useLocalStorage / useNotification
├── router/          # 路由配置与登录守卫
├── stores/          # Pinia 状态仓库 taskStore
├── views/           # Dashboard / Login
├── App.vue          # 全局容器与页面过渡
└── main.js          # 项目入口
```

## 构建与部署

```bash
npm run build      # 产物输出到 dist/
npm run preview    # 本地预览生产构建
```

### Gitee Pages 部署

站点的访问地址形如 `https://<用户名>.gitee.io/<仓库名>/`，属于**子路径**部署，因此构建时必须指定 base：

```bash
# PowerShell
$env:VITE_BASE='/task-hub/'; npm run build
# Git Bash / macOS / Linux
VITE_BASE=/task-hub/ npm run build
```

`vite.config.js` 通过 `base: process.env.VITE_BASE || '/'` 读取该变量，`router` 使用 `createWebHistory(import.meta.env.BASE_URL)`，保证子路径下资源与路由都正确。

构建完成后把 `dist/` 的内容推到 `gh-pages` 分支，在仓库「服务 → Gitee Pages」中选择该分支发布即可。

### 已知限制

- GitHub Actions 工作流在案例文档中仅作示例，本仓库实际采用本地构建 + Gitee Pages 发布。
- 站点为 HTML5 History 路由，直接刷新子路由（如 `/task-hub/login`）时部分静态托管会返回 404；构建脚本已额外输出 `404.html`（内容同 `index.html`）以兼容这种情况。
