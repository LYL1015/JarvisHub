<p align="center">
  <img src="assets/logo.png" alt="JarvisHub Logo" width="150">
</p>

<h1 align="center">JarvisHub</h1>

<p align="center">
  <strong>An Open Harness for Canvas-Native Multimodal Creative Agents</strong><br>
  Turn an editable canvas into shared project state for long-horizon creative work.
</p>

<p align="center">
  <a href="LICENSE"><img src="https://img.shields.io/badge/license-Apache--2.0-green.svg" alt="Apache-2.0 License"></a>
  <img src="https://img.shields.io/badge/Node.js-24.15.0-339933.svg?logo=node.js&logoColor=white" alt="Node.js 24.15.0">
  <img src="https://img.shields.io/badge/pnpm-10.8.1-f69220.svg?logo=pnpm&logoColor=white" alt="pnpm 10.8.1">
  <img src="https://img.shields.io/badge/Python-3.10%2B-3776ab.svg?logo=python&logoColor=white" alt="Python 3.10+">
  <img src="https://img.shields.io/badge/Docker-required-2496ed.svg?logo=docker&logoColor=white" alt="Docker required">
</p>

<p align="center">
  <a href="https://www.jarvishub.site/"><img src="https://img.shields.io/badge/Project-Page-111111.svg?style=for-the-badge&labelColor=000000&logo=googlechrome&logoColor=white" alt="Project Page"></a>
  <a href="https://arxiv.org/pdf/2607.23588"><img src="https://img.shields.io/badge/arXiv-Paper-111111.svg?style=for-the-badge&labelColor=000000&logo=arxiv&logoColor=white" alt="arXiv Paper"></a>
  <a href="https://huggingface.co/papers/2607.23588"><img src="https://img.shields.io/badge/Hugging%20Face-Daily%20Papers%20%233-111111.svg?style=for-the-badge&labelColor=000000&logo=huggingface&logoColor=white" alt="Hugging Face Daily Papers #3"></a>
  <a href="README_ZH.md"><img src="https://img.shields.io/badge/README-中文-111111.svg?style=for-the-badge&labelColor=000000&logo=markdown&logoColor=white" alt="中文 README"></a>
</p>

---

## 📮 Updates

- [2026.7.28] Repository open-sourced and paper uploaded.

## 🎬 Demo Video

[![Watch the JarvisHub demo](docs/assets/readme/jarvishub-demo-first-frame.jpg)](https://youtu.be/rHUQ5YgjGLw)

*Click the image above to watch the demo video.*

---

## Overview

**JarvisHub is a canvas-native creative agent harness for long-horizon multimodal creation.** The canvas is not only a visual interface. It is the workspace shared by people and agents, external memory that the agent can inspect, a protocol-constrained action space, and persistent state for artifacts, dependencies, versions, status, and feedback.

![Comparison of existing creative systems and JarvisHub](docs/assets/readme/jarvishub-motivation.png)

*Prompt tools, chatbot agents, and node workflows each cover part of creative production. JarvisHub keeps the complete project state visible and editable on one canvas.*

---

## What We're Building (And Why It's Different)

Existing creative systems leave important gaps:

- **Prompt-to-output tools** are effective at producing individual assets, but usually hide intermediate decisions, failed trials, and revision history.
- **Chatbot agents** can invoke tools and follow multi-step instructions, but linear conversation is a poor representation for spatial layouts, asset dependencies, version branches, and local edit targets.
- **Node-based workflows** expose execution steps, but commonly rely on manually specified pipelines rather than a project state an agent can continuously inspect, repair, and extend.

JarvisHub makes the canvas the source of truth. Text, references, character sheets, scenes, storyboards, images, videos, audio, webpage previews, presentation slides, candidates, and revision notes become addressable nodes and links. On every turn, the agent observes the current canvas, selects a permitted action, invokes a model or tool, and commits the returned artifacts and evidence to the same workspace.

The goal is not to replace generation models. JarvisHub provides an open and inspectable runtime for building agents that preserve context, orchestrate tools, use feedback, and recover from failure across extended creative workflows.

---

## ✨ What You Can Do

The JarvisHub paper demonstrates three representative long-horizon tasks:

| Task | Process organized by JarvisHub | Representative outputs |
| --- | --- | --- |
| **Narrative media generation** | Develop a story or script through character and location references, shot planning, storyboards, candidates, and cross-shot revision | Character sheets, scene designs, storyboards, shot lists, image sequences, video clips, and animatics |
| **Interactive web development** | Turn design goals and interaction requirements into layouts, frontend code, rendered previews, and iterative visual revisions | Landing pages, dynamic websites, web interfaces, interactive prototypes, code, and previews |
| **Presentation deck generation** | Select and organize content, synthesize diagrams, and maintain narrative and visual consistency across slides | Academic talks, reports, pitch decks, lecture slides, summaries, and explanatory diagrams |

JarvisHub also exposes the reusable state behind the final deliverable:

- Keep prompts, references, drafts, candidates, versions, and feedback on the canvas.
- Express references, generation dependencies, version lineage, and workflow continuation with typed nodes and edges.
- Invoke image, video, audio, code, browser, file, document, presentation, and MCP-backed tools.
- Use **Skills** for reusable procedures and **Memory** for preferences and prior decisions.
- Use **Subagents** to explore independent subtasks before the parent agent integrates useful results.
- Record requests, actions, observations, feedback, repair decisions, and canvas updates for inspection and recovery.

---

## 🧠 How JarvisHub Works

![JarvisHub architecture](docs/assets/readme/jarvishub-architecture.png)

JarvisHub is organized into three core layers:

| Layer | Responsibility | Main implementation |
| --- | --- | --- |
| **Canvas State** | Stores editable artifacts, spatial layout, dependencies, versions, runtime status, user choices, and feedback | `apps/web`, `packages/canvas-layout`, `packages/schemas`, PostgreSQL |
| **Protocol Bridge** | Exposes capabilities and execution grants, validates canvas mutations and tool actions, and commits state transitions | `apps/hono-api` |
| **Agent Runtime** | Observes the canvas, plans actions, invokes Skills / Memory / Tools / Subagents, and returns commit-ready observations | `apps/agents-cli` |

---

## 🚀 Quick Start

### Requirements

- Node.js `v24.15.0`
- pnpm
- Docker and Docker Compose for the default local PostgreSQL database
- Python `3.10+` with `python-pptx` and Pillow

### One-command startup

```bash
./run.sh
```

On the first run, the launcher automatically:

- installs missing workspace dependencies;
- starts and waits for local PostgreSQL;
- validates the vendored PPT Master runtime;
- selects a compatible Python executable;
- builds the Agents CLI;
- starts Web, API, Agents Bridge, and Trace Viewer.

The launcher stays in the foreground and aggregates service logs. Press `Ctrl+C` to stop the processes started by that run.

### Docker Compose startup

```bash
./scripts/dev.sh docker --build
```

Docker mode starts Web, API, Agents Bridge, Trace Viewer, PostgreSQL, and Redis. The first run pulls images and installs dependencies, so it can take several minutes; dependencies are cached in Docker volumes and later starts are faster.

If Docker Hub pulls are slow or unavailable, use a registry mirror for that run:

```bash
DOCKERHUB_REGISTRY=mirror.gcr.io/library ./scripts/dev.sh docker --build
```

Default Docker endpoints:

| URL | Service |
| --- | --- |
| `http://localhost:5173` | Web |
| `http://localhost:8788` | API |
| `http://localhost:8799` | Agents Bridge |
| `http://localhost:5781` | Trace API |
| `http://localhost:5782` | Trace Web |

Override host ports when the defaults are already in use:

```bash
WEB_PORT=5174 API_PORT=18788 AGENTS_PORT=18799 TRACE_API_PORT=15781 TRACE_WEB_PORT=15782 POSTGRES_PORT=15432 ./scripts/dev.sh docker --build
```

Stop the Docker services:

```bash
docker compose -f apps/hono-api/docker-compose.yml down
```

### Common commands

```bash
./run.sh             # Full startup
./run.sh restart     # Rebuild Agents and restart every service
./run.sh stop        # Stop local services
```

<details>
<summary>Maintenance commands</summary>

```bash
./run.sh --install   # Explicitly rerun pnpm install
./run.sh --no-build  # Skip the Agents build
./run.sh --clean     # Clear Agents caches before building
./run.sh db          # Start PostgreSQL only
./run.sh db:logs     # Follow PostgreSQL logs
./run.sh db:stop     # Stop PostgreSQL
```

</details>

### Service endpoints

| URL | Service | Purpose |
| --- | --- | --- |
| `http://localhost:5175` | Web | JarvisHub canvas and primary UI |
| `http://localhost:8788` | API | Application API and Protocol Bridge |
| `http://localhost:8799` | Agents | Agents HTTP Bridge |
| `http://localhost:5781` | Trace API | Trajectory query API |
| `http://localhost:5782` | Trace Web | Trace Viewer UI |

---

## ⚙️ Configuration

`run.sh` is the single entry point for local development. Override defaults directly from the shell:

```bash
WEB_PORT=5176 API_PORT=8789 ./run.sh
NPM_REGISTRY=https://registry.npmjs.org ./run.sh
LOCAL_PROXY_URL=http://127.0.0.1:7890 ./run.sh
DATABASE_URL=postgresql://user:password@host:5432/database ./run.sh
PPT_MASTER_PYTHON=/absolute/path/to/python3 ./run.sh
```

| Environment variable | Default | Purpose |
| --- | --- | --- |
| `WEB_PORT=5175` | 5175 | Web development server |
| `API_PORT=8788` | 8788 | JarvisHub API |
| `AGENTS_PORT=8799` | 8799 | Agents Bridge |
| `TRACE_API_PORT=5781` | 5781 | Trace Viewer API |
| `TRACE_WEB_PORT=5782` | 5782 | Trace Viewer Web |

Model providers, API keys, and default models are configured in the JarvisHub Web UI and stored in the database. The application can start without model configuration, but generation tasks that depend on those models will not run.

### Shared test R2

By default, `./run.sh` connects to a public and disposable **shared test R2** bucket so open-source users can quickly try asset upload and generated-media hosting.

> **Data warning:** every user shares the same test space, and objects may be accessed, overwritten, cleaned up, or deleted by others. **Do not upload private data**, business materials, credentials, or anything that must be retained.

Stable deployments and private data must use a separate R2 bucket:

```bash
R2_ACCESS_KEY_ID=your-access-key \
R2_SECRET_ACCESS_KEY=your-secret-key \
R2_BUCKET_URL=https://account-id.r2.cloudflarestorage.com/your-bucket \
R2_REGION=auto \
R2_PUBLIC_BASE_URL=https://assets.example.com \
./run.sh
```

Production deployments should inject database, storage, and infrastructure credentials through the deployment platform. Never commit private credentials to the repository.

---

## 🗂 Repository Layout

```text
apps/
  web/              # Editable canvas and frontend application
  hono-api/         # API, project state, Protocol Bridge, and storage integration
  agents-cli/       # Agent Runtime, Skills, Memory, Tools, and collaboration
packages/
  canvas-layout/    # Canvas layout capabilities
  schemas/          # Shared data and protocol schemas
tools/
  trace-viewer/     # Agent trajectory viewer
vendor/
  ppt-master/       # Vendored PPT Master runtime
run.sh              # Single local-development entry point
```

---

## 🔬 Research Scope and Limitations

- The current paper reports qualitative demonstrations rather than a completed benchmark or leaderboard.
- JarvisHub focuses on orchestration, project state, and traceability. Final artifact quality still depends on external models and tools.
- The Protocol Bridge makes actions explicit, inspectable, and recoverable, but cannot guarantee that every creative decision is semantically correct.
- Trajectories may support analysis and future training, but require quality filtering, user consent, anonymization, and copyright review.
- The open-source runtime is evolving, and interfaces or data structures may change.

---

## 📖 Citation

Read the [JarvisHub paper (PDF)](https://arxiv.org/pdf/2607.23588).

```bibtex
@article{lin2026jarvishub,
  title={JarvisHub: An Open Harness for Canvas-Native Multimodal Creative Agents},
  author={Lin, Yunlong and Lin, Zixu and Xing, Zhaohu and Li, Biqiang and Li, Chenxin and Wang, Haonan and Wu, Haitao and Liu, Hengyu and Chen, Jianghai and Feng, Kaituo and others},
  journal={arXiv preprint arXiv:2607.23588},
  year={2026}
}
```

---

## 🙏 Acknowledgements

JarvisHub builds on the open-source foundation of [TapCanvas](https://github.com/anymouschina/TapCanvas) and integrates [ppt-master](https://github.com/hugohe3/ppt-master) for presentation-generation workflows. We are grateful to the maintainers and contributors of both projects for making their work openly available.

---

## License

JarvisHub is licensed under the [Apache License 2.0](LICENSE).

---

<p align="center"><strong>If JarvisHub helps your work, consider giving the project a ⭐ Star.</strong></p>
