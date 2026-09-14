# 用户状态存储与 Factory/Deck 数据流调研

调研日期：2026-09-12
调研分支：feat/pluggable-worldview

## 1、课程完成、学习进度、成长值到底存在哪里？

**结论：目前完全没有持久化——它们只是浏览器内存里的临时变量，刷新页面就丢失。**

- 播放器状态定义在 `public/index.html:657`：
  ```js
  const State={insightScore:0,currentSlide:1,choices:{},clues:[0,0,0,0,0,0],...}
  ```
  `insightScore`（"成长值"）只在内存里累加（`public/index.html:805`），显示到页面上，`reset()` 直接清零（`public/index.html:663`）。
- 全仓库搜索 `localStorage`/`sessionStorage`/`IndexedDB`/`fetch`/`XMLHttpRequest` 在所有播放器 HTML 中**零命中**——没有任何写本地存储或上报后端的代码。
- 搜索数据库/用户模型关键词（`sqlite`/`database`/`class User`/`user_id`/`session_token`）**均无命中**——仓库里没有"用户"这个实体，也没有服务端数据库。
- `wrangler.toml` 显示整个项目是 Cloudflare Workers **纯静态资产**部署，README 也写明"无任何后端依赖"。

即：本仓库范围内不存在 App 客户端或用户数据库，第 1 问所说的持久化机制**在本仓库找不到**，不应臆测它存在于别处的系统里。

## 2、Factory/Deck 能通过什么接口读写用户状态？

**结论：没有这样的接口，Factory/Deck 目前完全不涉及用户状态读写。**

- **Factory**（`factory/studio_server.py`，4064行 Flask 应用）是课程"生产流水线"，40+ 个路由（`factory/studio_server.py:813-3995`）全部是内容生产相关（大纲、图像、TTS、页面装配、发布），**没有一个端点涉及用户/进度/成长值**（没有 `/api/users`、`/api/progress` 之类接口）。
- **Deck** 是课程装配后的静态数据模型：`assemble()`（`factory/studio_server.py:3858-3970`）把标题、角色、音频、页面图等打包成 dict，注入 `factory/deck_template.html:258` 的占位符 `const DECK=/*__DECK_DATA__*/null;`，生成静态 `public/<course>/index.html`（如 `public/class15/index.html:129`）。
- Deck 播放器内**没有** fetch/localStorage/postMessage 等任何 I/O 代码。播放器里看到的"进度"（如章节位置 N/总数）纯粹是基于静态 DECK JSON 现场计算的 UI 展示，不写回、不上报任何地方。

## 3、`factory/courses/` 由谁负责备份和分类？

- `.gitignore:4` 精确忽略路径为 `factory/courses/`。该目录由 `factory/config.py:143-146` 定义为 `COURSES_DIR`，本环境已通过 `.env.local:2` 重定向到仓库外的 `~/xrunda-studio-data/courses`（README.md:44-45 说明原因：生成物约 9.5G，建议不放仓库内）。
- **备份**：搜索 `backup`/`sync`/`archive`/`cron` 等关键词，唯一相关代码是 `factory/studio_server.py:302` 的 `archive_image_version()`——但这只是单张图片素材的历史版本管理（用于重新生成/回退某一页图片），**不是对整个目录的备份**。没有发现任何定时任务、云同步脚本。
- **分类**：搜索 `classify`/`categorize`/`tag`/`sort`，**无相关命中**。
- **CI**：仓库没有 `.github/workflows`，无任何 CI 配置。
- **文档**：仓库没有项目级 CLAUDE.md，没有 docs/ 目录，唯一提及此目录的是 README.md:44-45，只说明用途和体积，**没有规定备份责任人或分类规则**。

**现状：`factory/courses/` 完全没有自动化备份或分类机制，纯粹依赖开发者本地磁盘和人工管理**，一旦本地丢失且未手动另存，课程生成物没有任何兜底。这是当前代码库反映出的真实状况——如果这些生成物（图片、音频、页面 JSON 等）成本较高，建议尽快补一个备份方案（比如定时同步到云存储）。
