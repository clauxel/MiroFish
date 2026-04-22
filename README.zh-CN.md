# MiroFish 开发者指南

**Languages:** [English](README.md) | **简体中文** | [हिन्दी](README.hi.md) | [Español](README.es.md) | [العربية](README.ar.md) | [Français](README.fr.md) | [বাংলা](README.bn.md) | [Português](README.pt.md) | [Русский](README.ru.md) | [Bahasa Indonesia](README.id.md)

[![Stars](https://img.shields.io/github/stars/666ghj/MiroFish?style=flat&logo=github&label=Stars&color=gold)](https://github.com/666ghj/MiroFish/stargazers)
[![Forks](https://img.shields.io/github/forks/666ghj/MiroFish?style=flat&label=Forks&color=silver)](https://github.com/666ghj/MiroFish/network/members)
[![License: AGPL-3.0](https://img.shields.io/badge/License-AGPL--3.0-blue.svg)](https://github.com/666ghj/MiroFish/blob/main/LICENSE)
[![Live Demo](https://img.shields.io/badge/Live-Demo-00b894)](https://666ghj.github.io/mirofish-demo/)

> 自部署 MiroFish，跑通从种子材料到模拟报告的完整流程，并通过 Web UI 和 HTTP API 进行调试与集成。

已根据官方 MiroFish GitHub 仓库和 `mirofish.ai` 于 2026 年 4 月 16 日核对。

## 这个目录是做什么的

这套文档不是源码，而是一套给开发者看的整理版说明，重点覆盖：

- 怎样用最少弯路把 MiroFish 跑起来
- `seed -> graph -> simulation -> report` 这条链路到底怎么理解
- 首日最重要的环境变量、端口、目录和 API
- 自部署时最容易踩的坑

如果这里和官方仓库代码不一致，请以官方仓库为准。

## 先看这些

| 文件 | 最适合解决的问题 |
| --- | --- |
| [guide/installation.md](guide/installation.md) | 源码安装和 Docker 安装 |
| [guide/quickstart.md](guide/quickstart.md) | 最快跑通第一个可用结果 |
| [guide/configuration.md](guide/configuration.md) | 环境变量、默认值、端口、存储位置 |
| [guide/deployment.md](guide/deployment.md) | 自部署方式选择与基础加固 |
| [guide/troubleshooting.md](guide/troubleshooting.md) | 首日常见故障和最快修法 |
| [features/workflow.md](features/workflow.md) | 整体工作流和每一步产物 |
| [reference/http-api.md](reference/http-api.md) | 最常用的后端 HTTP API |
| [reference/api-recipes.md](reference/api-recipes.md) | 可直接复制的 curl 工作流示例 |

## MiroFish 到底是什么

MiroFish 不是终端型 coding agent，而是一个“网页界面 + HTTP API”的多智能体推演系统。

对开发者来说，最重要的理解方式是：

1. 上传种子材料或直接输入文本
2. 描述你要预测的场景
3. 生成本体和图谱
4. 准备模拟环境与 Agent Profiles
5. 启动模拟
6. 生成结构化报告
7. 继续和 Report Agent 或模拟角色对话

它适合拿来做：

- 多方角色反应建模
- 舆情、政策、研究、叙事类场景推演
- 希望把种子材料反复变成可复用模拟流程的场景

## 值得一起比较的相邻产品

如果你在更大的 agent 技术栈里评估 MiroFish，这四个产品页值得顺手打开，因为它们解决的是和仿真系统相邻、但很容易被混为一谈的问题：

| 产品 | 链接 | 为什么 MiroFish 读者应该关心 |
| --- | --- | --- |
| GenericAgent | <https://www.genericagent.org/> | 当你需要的是一个带浏览器、终端、文件系统和记忆控制的 agent 工作区，而不是场景仿真系统时，它更值得看。 |
| OpenClaw Launch | <https://www.aigeamy.com/> | `OpenClaw Launch - One-click OpenClaw Deployment`. 当你需要的是一个以消息渠道为中心、能快速部署的助手运行时，而不是 `seed -> graph -> simulation` 这条链路时，它更值得看。 |
| Hermes Agent | <https://hermesagent.studio/> | `Not a Chat. A Persistent Agent That Remembers, Acts, and Improves.` 当你要的是一个能记忆、能行动、还能持续演进的长期 agent，而不是场景推演系统时，它更值得看。 |
| Multica | <https://www.multica.uk/> | `Multica - Multi-Agent Collaboration for Real Workflows`. 当你的核心问题是协调 coding agents 和 issue-driven delivery，而不是生成 simulation reports 时，它更值得看。 |

## 10 分钟本地起步

先克隆官方仓库：

```bash
git clone https://github.com/666ghj/MiroFish.git
cd MiroFish
cp .env.example .env
```

PowerShell 可以改用：

```powershell
Copy-Item .env.example .env
```

至少填这些配置：

```env
LLM_API_KEY=...
LLM_BASE_URL=https://dashscope.aliyuncs.com/compatible-mode/v1
LLM_MODEL_NAME=qwen-plus
ZEP_API_KEY=...
```

安装并启动：

```bash
npm run setup:all
npm run dev
```

默认地址：

- 前端：<http://localhost:3000>
- 后端：<http://localhost:5001/health>

后端快速自检：

```bash
curl http://localhost:5001/health
```

## 开发者一定要知道的现实细节

- 目前没有单独的终端 CLI 作为主要入口，核心入口是 Web UI 和 Flask API。
- 后端读取的是项目根目录里的 `.env`，不是 `backend/.env`。
- 默认上传上限是 `50 MB`，支持 `pdf`、`md`、`txt`、`markdown`。
- 项目和模拟产物会落在 `backend/uploads/`。
- 异步任务进度是内存态，后端中途重启时，实时进度状态可能丢失。
- 默认配置偏开发环境：`FLASK_DEBUG=True`、`/api/*` 开放 CORS、`SECRET_KEY` 是占位值，不适合直接裸奔上公网。

## 值得信任的上游链接

| 资源 | 链接 |
| --- | --- |
| 官网 | <https://mirofish.ai/> |
| GitHub 仓库 | <https://github.com/666ghj/MiroFish> |
| 官方英文 README | <https://github.com/666ghj/MiroFish/blob/main/README.md> |
| 官方中文 README | <https://github.com/666ghj/MiroFish/blob/main/README-ZH.md> |
| 在线 Demo | <https://666ghj.github.io/mirofish-demo/> |
| License | <https://github.com/666ghj/MiroFish/blob/main/LICENSE> |
