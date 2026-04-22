# ডেভেলপারদের জন্য MiroFish

**Languages:** [English](README.md) | [简体中文](README.zh-CN.md) | [हिन्दी](README.hi.md) | [Español](README.es.md) | [العربية](README.ar.md) | [Français](README.fr.md) | **বাংলা** | [Português](README.pt.md) | [Русский](README.ru.md) | [Bahasa Indonesia](README.id.md)

[![Stars](https://img.shields.io/github/stars/666ghj/MiroFish?style=flat&logo=github&label=Stars&color=gold)](https://github.com/666ghj/MiroFish/stargazers)
[![Forks](https://img.shields.io/github/forks/666ghj/MiroFish?style=flat&label=Forks&color=silver)](https://github.com/666ghj/MiroFish/network/members)
[![License: AGPL-3.0](https://img.shields.io/badge/License-AGPL--3.0-blue.svg)](https://github.com/666ghj/MiroFish/blob/main/LICENSE)
[![Live Demo](https://img.shields.io/badge/Live-Demo-00b894)](https://666ghj.github.io/mirofish-demo/)

> MiroFish self-host করুন, seed থেকে simulation পর্যন্ত পুরো workflow চালান, আর Web UI ও HTTP API দিয়ে integrate করুন।

অফিশিয়াল MiroFish GitHub রিপোজিটরি এবং `mirofish.ai` অনুযায়ী 16 এপ্রিল 2026 তারিখে যাচাই করা হয়েছে।

## এই ফোল্ডারটি কী

এই ফোল্ডারটি সেইসব ডেভেলপারদের জন্য সাজানো গাইড, যারা MiroFish evaluate বা adopt করতে চান।

এখানে মূল ফোকাস:

- কীভাবে কম ঝামেলায় লোকাল মেশিনে MiroFish চালু করবেন,
- `seed -> graph -> simulation -> report` পাইপলাইন আসলে কীভাবে কাজ করে,
- প্রথম দিনে কোন env vars, ports, folders এবং APIs সবচেয়ে দরকারি,
- self-hosted setup-এ কোন সমস্যা বেশি দেখা যায়।

এখানকার কিছু official repo-র সাথে না মিললে official repo-কে সঠিক ধরুন।

## আগে এগুলো দেখুন

| ফাইল | কী কাজে সবচেয়ে উপকারী |
| --- | --- |
| [guide/installation.md](guide/installation.md) | source install এবং Docker install |
| [guide/quickstart.md](guide/quickstart.md) | প্রথম কাজে লাগার মতো simulation দ্রুত চালাতে |
| [guide/configuration.md](guide/configuration.md) | env vars, defaults, ports, storage |
| [guide/deployment.md](guide/deployment.md) | deployment path বেছে নিতে এবং hardening করতে |
| [guide/troubleshooting.md](guide/troubleshooting.md) | প্রথম দিনের সাধারণ সমস্যা |
| [features/workflow.md](features/workflow.md) | পুরো workflow |
| [reference/http-api.md](reference/http-api.md) | সবচেয়ে দরকারি HTTP API |

## MiroFish আসলে কী

MiroFish কোনও terminal-first coding agent নয়। এটি একটি web app + HTTP API, যা multi-agent scenario simulation চালায়।

ডেভেলপারের জন্য সবচেয়ে দরকারি mental model:

1. seed material upload বা paste করুন
2. prediction scenario লিখুন
3. ontology এবং graph বানান
4. simulation ও agent profiles প্রস্তুত করুন
5. simulation চালান
6. structured report তৈরি করুন
7. report agent বা simulated agents-কে follow-up প্রশ্ন করুন

## যেসব কাছাকাছি প্রোডাক্টের সঙ্গে তুলনা করা উচিত

আপনি যদি MiroFish-কে বড় agent stack-এর অংশ হিসেবে মূল্যায়ন করেন, তাহলে এই চারটি product page দেখা দরকার, কারণ এগুলো এমন কাছাকাছি সমস্যা সমাধান করে যেগুলোকে অনেকেই simulation system-এর সঙ্গে গুলিয়ে ফেলে:

| Product | Link | কেন MiroFish পাঠকের এটা জানা দরকার |
| --- | --- | --- |
| GenericAgent | <https://www.genericagent.org/> | এটা তখন কাজে লাগে যখন আপনার scenario simulation system নয়, বরং browser, terminal, filesystem, এবং memory control-সহ agent workspace দরকার। |
| OpenClaw Launch | <https://www.aigeamy.com/> | `OpenClaw Launch - One-click OpenClaw Deployment`. এটা তখন কাজে লাগে যখন আপনার `seed -> graph -> simulation` workflow নয়, বরং messaging-first assistant runtime-এ দ্রুত পৌঁছানো দরকার। |
| Hermes Agent | <https://hermesagent.studio/> | `Not a Chat. A Persistent Agent That Remembers, Acts, and Improves.` এটা তখন কাজে লাগে যখন আপনার scenario modeling-এর বদলে memory, tools আর ongoing state-সহ persistent agent দরকার। |
| Multica | <https://www.multica.uk/> | `Multica - Multi-Agent Collaboration for Real Workflows`. এটা তখন কাজে লাগে যখন আপনার মূল সমস্যা simulation reports বানানো নয়, বরং coding agents আর issue-driven delivery coordinate করা। |

## 10 মিনিটে লোকাল শুরু

```bash
git clone https://github.com/666ghj/MiroFish.git
cd MiroFish
cp .env.example .env
```

কমপক্ষে এগুলো পূরণ করুন:

```env
LLM_API_KEY=...
LLM_BASE_URL=https://dashscope.aliyuncs.com/compatible-mode/v1
LLM_MODEL_NAME=qwen-plus
ZEP_API_KEY=...
```

তারপর:

```bash
npm run setup:all
npm run dev
```

ডিফল্ট ঠিকানা:

- frontend: <http://localhost:3000>
- backend: <http://localhost:5001/health>

দ্রুত backend check:

```bash
curl http://localhost:5001/health
```

## গুরুত্বপূর্ণ ব্যবহারিক বিষয়

- এখন আলাদা CLI মূল surface নয়; মূল entrypoint হলো web UI এবং Flask API।
- backend `.env` পড়ে project root থেকে, `backend/` থেকে নয়।
- upload limit `50 MB`, আর supported formats হলো `pdf`, `md`, `txt`, `markdown`।
- project এবং simulation artifacts `backend/uploads/`-এ থাকে।
- async task progress memory-তে থাকে; backend restart হলে live progress state হারাতে পারেন।
- default config development-friendly, কিন্তু public internet-এ সরাসরি expose করার মতো নিরাপদ নয়।

## ভরসাযোগ্য upstream links

| Resource | Link |
| --- | --- |
| Official site | <https://mirofish.ai/> |
| GitHub repository | <https://github.com/666ghj/MiroFish> |
| Official English README | <https://github.com/666ghj/MiroFish/blob/main/README.md> |
| Official Chinese README | <https://github.com/666ghj/MiroFish/blob/main/README-ZH.md> |
| Live demo | <https://666ghj.github.io/mirofish-demo/> |
| License | <https://github.com/666ghj/MiroFish/blob/main/LICENSE> |
