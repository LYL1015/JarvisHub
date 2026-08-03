<p align="center">
  <img src="assets/logo.png" alt="JarvisHub Logo" width="150">
</p>

<h1 align="center">JarvisHub</h1>

<p align="center">
  <strong>面向画布原生多模态创作 Agent 的开放运行框架</strong><br>
  将可编辑画布变成长程创作中人类与 Agent 共享的项目状态。
</p>

<p align="center">
  <a href="LICENSE"><img src="https://img.shields.io/badge/license-Apache--2.0-green.svg" alt="Apache-2.0 License"></a>
  <img src="https://img.shields.io/badge/Node.js-24.15.0-339933.svg?logo=node.js&logoColor=white" alt="Node.js 24.15.0">
  <img src="https://img.shields.io/badge/pnpm-10.8.1-f69220.svg?logo=pnpm&logoColor=white" alt="pnpm 10.8.1">
  <img src="https://img.shields.io/badge/Python-3.10%2B-3776ab.svg?logo=python&logoColor=white" alt="Python 3.10+">
  <img src="https://img.shields.io/badge/Docker-required-2496ed.svg?logo=docker&logoColor=white" alt="Docker required">
</p>

<p align="center">
  <a href="https://www.jarvishub.site/"><img src="https://img.shields.io/badge/Project-Page-111111.svg?style=for-the-badge&labelColor=000000&logo=googlechrome&logoColor=white" alt="项目主页"></a>
  <a href="https://arxiv.org/pdf/2607.23588"><img src="https://img.shields.io/badge/arXiv-Paper-111111.svg?style=for-the-badge&labelColor=000000&logo=arxiv&logoColor=white" alt="arXiv Paper"></a>
  <a href="https://huggingface.co/papers/2607.23588"><img src="https://img.shields.io/badge/Hugging%20Face-Daily%20Papers%20%233-111111.svg?style=for-the-badge&labelColor=000000&logo=huggingface&logoColor=white" alt="Hugging Face Daily Papers #3"></a>
  <a href="README.md"><img src="https://img.shields.io/badge/README-English-111111.svg?style=for-the-badge&labelColor=000000&logo=markdown&logoColor=white" alt="English README"></a>
</p>

---

## 📮 更新

- [2026.7.28] 项目开源，论文已上传。

## 项目概览

**JarvisHub 是一个面向长程多模态创作的 Canvas-Native Creative Agent Harness。** 画布不仅是用户界面，也是人类与 Agent 共享的项目工作区、Agent 可观察的外部记忆、受协议约束的动作空间，以及保存资产、依赖、版本、状态和反馈的持久项目状态。

![现有创作系统与 JarvisHub 的比较](docs/assets/readme/jarvishub-motivation.png)

*单次生成工具、Chatbot Agent 和节点工作流分别覆盖创作过程的一部分；JarvisHub 将完整项目状态统一保留在可编辑画布中。*

---

## 我们在构建什么（以及为什么不同）

现有创作系统仍存在明显缺口：

- **Prompt-to-Output 工具**擅长生成单个资产，但通常隐藏中间决策、失败尝试和版本历史。
- **Chatbot Agent**可以调用工具并执行多步任务，但线性聊天记录难以表达空间布局、资产依赖、版本分支和局部编辑目标。
- **节点式工作流**让执行步骤可见，但往往依赖人工预先搭建固定管线，而不是让 Agent 持续检查、修复和扩展项目状态。

JarvisHub 将画布作为唯一可信的项目状态。文本、参考图、角色设定、场景、故事板、图片、视频、音频、网页预览、PPT 页面、候选结果和修改意见都可以表示为可寻址的节点与连接。Agent 在每一轮观察当前画布，选择被允许的动作，调用模型或工具，再把返回的资产与证据写回同一个工作区。

JarvisHub 的目标不是替代底层生成模型，而是提供一个开放、可检查的运行框架，用来构建能够在长时间、多步骤创作中保持上下文、编排工具、利用反馈并恢复失败的 Agent。

---

## ✨ 可以做什么

JarvisHub 论文展示了三类代表性的长程任务：

| 任务 | JarvisHub 组织的过程 | 代表性产物 |
| --- | --- | --- |
| **叙事媒体生成** | 从故事或剧本出发，组织角色与场景参考、镜头规划、故事板、候选画面和跨镜头修改 | 角色设定、场景图、故事板、镜头表、图像序列、视频片段和 Animatic |
| **交互式网页开发** | 将设计目标和交互要求转化为布局、前端代码、渲染预览与多轮视觉修订 | Landing Page、动态网站、Web UI、交互原型、代码和预览 |
| **演示文稿生成** | 选择并组织内容，生成解释性图示，并保持跨页叙事和视觉一致性 | 学术报告、项目汇报、Pitch Deck、课程 PPT、视觉摘要和图表 |

JarvisHub 还会保留最终产物背后的可复用状态：

- 在画布中保存 Prompt、参考素材、草稿、候选、版本和反馈。
- 使用有类型的节点与边表达引用、生成依赖、版本谱系和工作流延续关系。
- 调用图片、视频、音频、代码、浏览器、文件、文档、PPT 和 MCP 工具。
- 使用 **Skills** 组织可复用流程，使用 **Memory** 保留偏好和历史决策。
- 使用 **Subagents** 并行探索独立子任务，再由主 Agent 整合有效结果。
- 记录请求、动作、观察、反馈、修复决定和画布更新，支持检查与恢复。

---

## 🧠 JarvisHub 如何工作

![JarvisHub 系统架构](docs/assets/readme/jarvishub-architecture.png)

JarvisHub 由三个核心层组成：

| 层 | 职责 | 仓库中的主要实现 |
| --- | --- | --- |
| **Canvas State** | 保存可编辑资产、空间布局、依赖、版本、运行状态、用户选择和反馈 | `apps/web`、`packages/canvas-layout`、`packages/schemas`、PostgreSQL |
| **Protocol Bridge** | 暴露能力清单与本轮执行授权，校验画布修改与工具动作，并提交状态转换 | `apps/hono-api` |
| **Agent Runtime** | 观察画布、规划动作、调用 Skills / Memory / Tools / Subagents，并返回可提交的观察结果 | `apps/agents-cli` |

---

## 🚀 快速开始

### 环境要求

- Node.js `v24.15.0`
- pnpm
- Docker 与 Docker Compose（默认本地 PostgreSQL 使用）
- Python `3.10+`，并安装 `python-pptx` 和 Pillow

### 一键启动

```bash
./run.sh
```

首次运行会自动：

- 检查并安装缺失的 workspace 依赖；
- 启动并等待本地 PostgreSQL；
- 检查仓库内置的 PPT Master；
- 自动选择满足要求的 Python；
- 构建 Agents CLI；
- 启动 Web、API、Agents Bridge 和 Trace Viewer。

脚本以前台方式运行并汇总服务日志，按 `Ctrl+C` 可以停止本次启动的全部服务。

### Docker Compose 一键启动

```bash
./scripts/dev.sh docker --build
```

Docker 模式会启动 Web、API、Agents Bridge、Trace Viewer、PostgreSQL 和 Redis。首次运行需要拉取镜像并安装依赖，可能需要几分钟；依赖会缓存在 Docker volume 中，后续启动会更快。

如果 Docker Hub 拉取较慢或不可用，可以临时使用镜像代理：

```bash
DOCKERHUB_REGISTRY=mirror.gcr.io/library ./scripts/dev.sh docker --build
```

Docker 模式默认地址：

| 地址 | 服务 |
| --- | --- |
| `http://localhost:5173` | Web |
| `http://localhost:8788` | API |
| `http://localhost:8799` | Agents Bridge |
| `http://localhost:5781` | Trace API |
| `http://localhost:5782` | Trace Web |

需要避开本机已有端口时，可以在命令前覆盖：

```bash
WEB_PORT=5174 API_PORT=18788 AGENTS_PORT=18799 TRACE_API_PORT=15781 TRACE_WEB_PORT=15782 POSTGRES_PORT=15432 ./scripts/dev.sh docker --build
```

停止 Docker 服务：

```bash
docker compose -f apps/hono-api/docker-compose.yml down
```

### 常用命令

```bash
./run.sh             # 完整启动
./run.sh restart     # 重建 Agents，并重启全部服务
./run.sh stop        # 停止本地服务
```

<details>
<summary>维护命令</summary>

```bash
./run.sh --install   # 显式重新执行 pnpm install
./run.sh --no-build  # 跳过 Agents 构建
./run.sh --clean     # 清理 Agents 缓存后构建
./run.sh db          # 单独启动 PostgreSQL
./run.sh db:logs     # 查看 PostgreSQL 日志
./run.sh db:stop     # 停止 PostgreSQL
```

</details>

### 服务地址

| 地址 | 服务 | 用途 |
| --- | --- | --- |
| `http://localhost:5175` | Web | JarvisHub 画布与主要交互入口 |
| `http://localhost:8788` | API | 应用 API 与 Protocol Bridge |
| `http://localhost:8799` | Agents | Agents HTTP Bridge |
| `http://localhost:5781` | Trace API | 执行轨迹查询接口 |
| `http://localhost:5782` | Trace Web | Trace Viewer 页面 |

---

## ⚙️ 配置说明

`run.sh` 是本地开发的统一入口。需要覆盖默认值时，直接在命令前提供变量：

```bash
WEB_PORT=5176 API_PORT=8789 ./run.sh
NPM_REGISTRY=https://registry.npmjs.org ./run.sh
LOCAL_PROXY_URL=http://127.0.0.1:7890 ./run.sh
DATABASE_URL=postgresql://user:password@host:5432/database ./run.sh
PPT_MASTER_PYTHON=/absolute/path/to/python3 ./run.sh
```

| 环境变量 | 默认值 | 作用 |
| --- | --- | --- |
| `WEB_PORT=5175` | 5175 | Web 开发服务 |
| `API_PORT=8788` | 8788 | JarvisHub API |
| `AGENTS_PORT=8799` | 8799 | Agents Bridge |
| `TRACE_API_PORT=5781` | 5781 | Trace Viewer API |
| `TRACE_WEB_PORT=5782` | 5782 | Trace Viewer Web |

模型供应商、API Key 和默认模型通过 JarvisHub Web 界面配置并保存在数据库中。未配置模型时，应用可以正常启动，但依赖模型的生成任务无法执行。

### 共享测试 R2

`./run.sh` 默认连接一个公开、可随时清理的 **共享测试 R2** 存储桶，让开源用户可以快速体验资产上传和生成结果托管。

> **数据警告：**所有用户共用同一测试空间，数据可能被其他用户访问、覆盖、清理或删除。**请勿上传个人隐私**、商业资料、密钥或任何需要长期保存的数据。

稳定部署和私有化数据必须使用自己的 R2：

```bash
R2_ACCESS_KEY_ID=your-access-key \
R2_SECRET_ACCESS_KEY=your-secret-key \
R2_BUCKET_URL=https://account-id.r2.cloudflarestorage.com/your-bucket \
R2_REGION=auto \
R2_PUBLIC_BASE_URL=https://assets.example.com \
./run.sh
```

生产部署应通过部署平台注入数据库、存储和基础设施凭据，不应把私有密钥提交到仓库。

---

## 🗂 项目结构

```text
apps/
  web/              # 可编辑画布与前端应用
  hono-api/         # API、项目状态、Protocol Bridge 与存储集成
  agents-cli/       # Agent Runtime、Skills、Memory、Tools 与协作能力
packages/
  canvas-layout/    # 画布布局能力
  schemas/          # 共享数据与协议 Schema
tools/
  trace-viewer/     # Agent 执行轨迹查看器
vendor/
  ppt-master/       # 仓库内置的 PPT Master runtime
run.sh              # 本地开发统一入口
```

---

## 🔬 研究范围与限制

- 当前论文报告的是代表性定性案例，而不是完整 benchmark 或排行榜。
- JarvisHub 关注 Agent 编排、项目状态与可追踪性；最终资产质量仍取决于外部模型和工具。
- Protocol Bridge 让动作显式、可检查和可恢复，但不能保证每个创作决策在语义上始终正确。
- 执行轨迹可用于分析和未来训练，但使用前需要进行质量过滤、用户授权、匿名化与版权审查。
- 开源运行框架仍在演进，接口与数据结构可能变化。

---

## 📖 引用

```bibtex
@article{lin2026jarvishub,
  title={JarvisHub: An Open Harness for Canvas-Native Multimodal Creative Agents},
  author={Lin, Yunlong and Lin, Zixu and Xing, Zhaohu and Li, Biqiang and Li, Chenxin and Wang, Haonan and Wu, Haitao and Liu, Hengyu and Chen, Jianghai and Feng, Kaituo and others},
  journal={arXiv preprint arXiv:2607.23588},
  year={2026}
}
```

---

## 🙏 致谢

JarvisHub 基于 [TapCanvas](https://github.com/anymouschina/TapCanvas) 的开源能力构建，并集成 [ppt-master](https://github.com/hugohe3/ppt-master) 以支持演示文稿生成工作流。感谢两个项目的维护者和贡献者将这些成果开放给社区。

---

## 许可证

JarvisHub 使用 [Apache License 2.0](LICENSE)。

---

<p align="center"><strong>如果 JarvisHub 对你有帮助，欢迎给项目一个 ⭐ Star。</strong></p>
