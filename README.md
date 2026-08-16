# 从零到全栈：模块 4.4～6.2

这是跟随[「零到全栈」课程](https://xn--ygr25xpohxwz.com/zero-to-fullstack/)完成的学习项目。仓库记录了同一个网站从数据驱动的 React 应用迁移到 Next.js，再加入 Python、FastAPI、前后端联调和中文文本分析的过程。

当前已完成到 **6.2：让网页真的会分析文字**。

## 已完成课程与提交

| 课程 | 提交 | 阶段成果 |
| --- | --- | --- |
| [4.4：让数据驱动界面](https://xn--ygr25xpohxwz.com/zero-to-fullstack/lessons/module-4-4/) | `321b824` | 数据与界面分离、状态驱动 UI、用 URL 管理路由 |
| [4.5：Next.js——React 之上的生产级框架](https://xn--ygr25xpohxwz.com/zero-to-fullstack/lessons/module-4-5/) | `15b6633` | 从 Vite 迁移到 Next.js，以 App Router 管理页面 |
| [4.6：把前端项目发布到公网](https://xn--ygr25xpohxwz.com/zero-to-fullstack/lessons/module-4-6/) | `8c02ba9` | 开启静态导出，生成可部署的 `out/` |
| [5.1：究竟什么是 API？](https://xn--ygr25xpohxwz.com/zero-to-fullstack/lessons/module-5-1/) | `7cfa2e7` | 用 `requests` 调用公网 API |
| [5.2：Python 的安装和环境设置](https://xn--ygr25xpohxwz.com/zero-to-fullstack/lessons/module-5-2/) | `7cfa2e7`（共享） | 建立后端目录、虚拟环境约定和依赖清单 |
| [5.3：看懂 HTTP，手搓 API](https://xn--ygr25xpohxwz.com/zero-to-fullstack/lessons/module-5-3/) | `7cfa2e7`（共享） | 用 Python 标准库实现 `/api/profile` |
| [5.4：从手搓到框架，FastAPI 登场](https://xn--ygr25xpohxwz.com/zero-to-fullstack/lessons/module-5-4/) | `905cfdb` | 用 FastAPI 重构 API，增加 POST 分析接口 |
| [5.5：前后端联调与 CORS](https://xn--ygr25xpohxwz.com/zero-to-fullstack/lessons/module-5-5/) | `b359c79` | Fetch 联调、CORS 和环境变量 |
| [6.1：第三方库和 PyPI](https://xn--ygr25xpohxwz.com/zero-to-fullstack/lessons/module-6-1/) | `44311cd` | 安装并记录 SnowNLP、pypinyin |
| [6.2：让网页真的会分析文字](https://xn--ygr25xpohxwz.com/zero-to-fullstack/lessons/module-6-2/) | `44311cd`（共享） | 返回真实拼音和情感分析结果 |

5.1～5.3 在同一次提交中完成，6.1～6.2 也在同一次提交中完成，因此表格中标记为“共享”。

## 4.4：让数据驱动界面

对应提交：`321b824`（`react add drive by data`）

- 新增 `src/data/site.js`，把展示数据从 JSX 中抽离。
- 输入框使用 React state 保存文本，字符数随输入自动更新。
- 新增 `src/router/useRoute.js`，根据 URL 在主页和文字实验室之间切换，并响应浏览器前进、后退。

这一阶段仍使用 Vite，入口是 `index.html` 和 `src/main.jsx`。

## 4.5：Next.js——React 之上的生产级框架

对应提交：`15b6633`（`use next.js replace vite`）

- 新增 `app/layout.jsx`，作为全站共享布局和 CSS 入口。
- 新增 `app/page.jsx`，对应主页 `/`。
- 新增 `app/text-lab/page.jsx`，对应 `/text-lab`。
- 使用 Next.js 的 `Link` 和 `usePathname` 重写导航。
- 将需要 state、effect 或浏览器 API 的组件声明为 Client Components。
- 删除 Vite 入口、配置和手写路由，由 App Router 接管。

```text
4.4 手写路由                    4.5 Next.js App Router
src/router/useRoute.js     ->   app/page.jsx
                                app/text-lab/page.jsx
```

## 4.6：把前端项目发布到公网

对应提交：`8c02ba9`（`static next`）

课程介绍了常驻 Next.js 服务和静态导出两种部署方式。本项目选择静态导出，在 `next.config.mjs` 中配置：

```js
const nextConfig = {
  output: "export",
};
```

运行 `npm run build` 后，静态文件生成到 `out/`，可以交给 Nginx 等静态服务器。

## 5.1：究竟什么是 API？

对应提交：`7cfa2e7`（`create a api`）

`backend/api_demo.py` 使用 `requests.get()` 调用公网 IP API，并读取 JSON 响应。项目由此建立前后端边界：浏览器界面负责展示，后端通过 `/api/...` 提供数据。

## 5.2：Python 的安装和环境设置

对应提交：`7cfa2e7`，与 5.1、5.3 共享。

- 创建 `backend/`，将 Python 后端与前端分开。
- 使用 `.venv` 隔离依赖，并将虚拟环境加入 `.gitignore`。
- 使用 pip 安装第三方库。
- 用 `backend/requirements.txt` 记录依赖版本。

## 5.3：看懂 HTTP，手搓 API

对应提交：`7cfa2e7`，与 5.1、5.2 共享。

使用 `BaseHTTPRequestHandler` 和 `HTTPServer` 手动实现服务：

- 监听 `8000` 端口。
- 处理 `GET /api/profile`。
- 设置 HTTP 状态码和 JSON 响应头。
- 将 Python 字典序列化为 UTF-8 JSON。
- 对其他路径返回 `404`。

这份实现后来保留为 `backend/handmade.py`，用于和 FastAPI 对照。

## 5.4：从手搓到框架，FastAPI 登场

对应提交：`905cfdb`（`reconstruct api with fastapi`）

- 用 `@app.get("/api/profile")` 声明 GET 接口。
- 新增 `POST /api/analyze`，约定文字分析的请求与响应。
- 使用 Pydantic `BaseModel` 校验 `{ "text": "..." }` 请求体。
- 通过 Uvicorn 运行应用，并获得 FastAPI 自动生成的 `/docs`。

此时分析接口仍返回占位数据，真实分析逻辑在 6.2 加入。

## 5.5：前后端联调与 CORS

对应提交：`b359c79`（`completed CROS connect firsly`）

- `HomeView.jsx` 请求 `GET /api/profile`。
- `InputCard.jsx` 将输入文字 POST 到 `/api/analyze`。
- `TextLabView.jsx` 提升结果 state，并交给 `ResultCard.jsx` 展示。
- FastAPI 加入 `CORSMiddleware`，允许 `http://localhost:3000` 的请求。
- 前端使用 `NEXT_PUBLIC_API_BASE_URL` 读取后端地址。

## 6.1：第三方库和 PyPI

对应提交：`44311cd`（`use snownlp & pypinyin to analysis the words`）

- `pypinyin`：把中文转换为带声调的拼音。
- `SnowNLP`：计算中文文本的情感倾向分数。
- 两个依赖都记录在 `backend/requirements.txt` 中。

## 6.2：让网页真的会分析文字

对应提交：`44311cd`，与 6.1 共享。

这一节用真实分析替换 5.4 的占位结果，前端和 API 契约保持不变：

```text
浏览器输入中文
    -> POST /api/analyze
    -> SnowNLP 计算情感分数
    -> pypinyin 生成带声调拼音
    -> 返回 text、score、label、pinyin
    -> React 更新结果卡片
```

分数大于等于 `0.6` 为「偏积极」，小于等于 `0.4` 为「偏消极」，其余为「中性」。这是当前已经完成的课程终点。

## 当前项目结构

```text
app/                         Next.js App Router
  layout.jsx                 全站布局与样式入口
  page.jsx                   主页 /
  text-lab/page.jsx          文字实验室 /text-lab
components/                  React 页面与交互组件
css/                         全站样式
data/site.js                 前端默认展示数据
backend/
  main.py                    FastAPI 与中文分析逻辑
  handmade.py                5.3 手搓 HTTP API 留档
  api_demo.py                5.1 调用公网 API 的练习
  requirements.txt           Python 依赖清单
next.config.mjs              Next.js 静态导出配置
```

## 运行当前版本

### 1. 启动后端

```powershell
cd backend
python -m venv .venv
.\.venv\Scripts\Activate.ps1
python -m pip install -r requirements.txt
python -m uvicorn main:app --reload --port 8000
```

API 文档：<http://localhost:8000/docs>

### 2. 启动前端

在项目根目录创建 `.env.local`：

```dotenv
NEXT_PUBLIC_API_BASE_URL=http://localhost:8000
```

然后运行：

```powershell
npm install
npm run dev
```

访问 <http://localhost:3000>。

## 查看课程阶段

```powershell
git show 321b824   # 4.4
git show 15b6633   # 4.5
git show 8c02ba9   # 4.6
git show 7cfa2e7   # 5.1～5.3
git show 905cfdb   # 5.4
git show b359c79   # 5.5
git show 44311cd   # 6.1～6.2
```

下一节是 **6.3：数据库前传——数据都存在哪儿？**。当前仓库尚未引入数据库，分析结果不会持久化。
