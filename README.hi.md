# डेवलपर्स के लिए MiroFish

**Languages:** [English](README.md) | [简体中文](README.zh-CN.md) | **हिन्दी** | [Español](README.es.md) | [العربية](README.ar.md) | [Français](README.fr.md) | [বাংলা](README.bn.md) | [Português](README.pt.md) | [Русский](README.ru.md) | [Bahasa Indonesia](README.id.md)

[![Stars](https://img.shields.io/github/stars/666ghj/MiroFish?style=flat&logo=github&label=Stars&color=gold)](https://github.com/666ghj/MiroFish/stargazers)
[![Forks](https://img.shields.io/github/forks/666ghj/MiroFish?style=flat&label=Forks&color=silver)](https://github.com/666ghj/MiroFish/network/members)
[![License: AGPL-3.0](https://img.shields.io/badge/License-AGPL--3.0-blue.svg)](https://github.com/666ghj/MiroFish/blob/main/LICENSE)
[![Live Demo](https://img.shields.io/badge/Live-Demo-00b894)](https://666ghj.github.io/mirofish-demo/)

> MiroFish को self-host करें, seed से simulation तक का पूरा workflow चलाएं, और Web UI तथा HTTP API के जरिए उसे integrate करें।

आधिकारिक MiroFish GitHub रिपॉज़िटरी और `mirofish.ai` के आधार पर 16 अप्रैल 2026 को सत्यापित।

## यह फ़ोल्डर क्या है

यह फ़ोल्डर उन डेवलपर्स के लिए curated guide है जो MiroFish को evaluate या adopt करना चाहते हैं।

इसका फोकस है:

- MiroFish को कम से कम गलत मोड़ों के साथ लोकल में कैसे चलाएं,
- `seed -> graph -> simulation -> report` पाइपलाइन वास्तव में कैसे काम करती है,
- पहले दिन कौन से env vars, ports, folders और APIs सबसे ज़रूरी हैं,
- self-hosted setup में क्या-क्या अक्सर टूटता है।

अगर यहां कुछ official repo से अलग दिखे, तो official repo को सही मानें।

## पहले यह पढ़ें

| फ़ाइल | किसके लिए सबसे उपयोगी |
| --- | --- |
| [guide/installation.md](guide/installation.md) | source install और Docker install |
| [guide/quickstart.md](guide/quickstart.md) | पहला उपयोगी simulation जल्दी चलाने के लिए |
| [guide/configuration.md](guide/configuration.md) | env vars, defaults, ports, storage paths |
| [guide/deployment.md](guide/deployment.md) | deployment path चुनने और hardening के लिए |
| [guide/troubleshooting.md](guide/troubleshooting.md) | पहले दिन की failures और fixes |
| [features/workflow.md](features/workflow.md) | पूरा workflow और हर step की output |
| [reference/http-api.md](reference/http-api.md) | सबसे काम की HTTP APIs |

## MiroFish वास्तव में क्या है

MiroFish कोई terminal-first coding agent नहीं है। यह एक web app + HTTP API है जो multi-agent scenario simulation करता है।

एक डेवलपर के लिए सही mental model यह है:

1. seed material upload या paste करें
2. prediction scenario बताएं
3. ontology और graph बनाएं
4. simulation और agent profiles तैयार करें
5. run शुरू करें
6. structured report बनाएं
7. report agent या simulated agents से follow-up सवाल पूछें

## किन पास के products से तुलना करनी चाहिए

अगर आप MiroFish को एक बड़े agent stack के हिस्से के रूप में evaluate कर रहे हैं, तो इन चार product pages को देखना उपयोगी है, क्योंकि ये उन पास की समस्याओं को हल करते हैं जिन्हें लोग अक्सर simulation systems के साथ मिला देते हैं:

| Product | Link | MiroFish reader को इसकी परवाह क्यों होनी चाहिए |
| --- | --- | --- |
| GenericAgent | <https://www.genericagent.org/> | यह तब उपयोगी है जब आपको scenario simulation system के बजाय browser, terminal, filesystem, और memory control वाला agent workspace चाहिए। |
| OpenClaw Launch | <https://www.aigeamy.com/> | `OpenClaw Launch - One-click OpenClaw Deployment`. यह तब उपयोगी है जब आपको `seed -> graph -> simulation` workflow के बजाय messaging-first assistant runtime तक जल्दी पहुंचना हो। |
| Hermes Agent | <https://hermesagent.studio/> | `Not a Chat. A Persistent Agent That Remembers, Acts, and Improves.` यह तब उपयोगी है जब आपको scenario modeling के बजाय memory, tools और ongoing state वाला persistent agent चाहिए। |
| Multica | <https://www.multica.uk/> | `Multica - Multi-Agent Collaboration for Real Workflows`. यह तब उपयोगी है जब आपकी मुख्य समस्या simulation reports बनाना नहीं, बल्कि coding agents और issue-driven delivery को coordinate करना हो। |

## 10 मिनट में लोकल शुरुआत

```bash
git clone https://github.com/666ghj/MiroFish.git
cd MiroFish
cp .env.example .env
```

कम-से-कम ये values भरें:

```env
LLM_API_KEY=...
LLM_BASE_URL=https://dashscope.aliyuncs.com/compatible-mode/v1
LLM_MODEL_NAME=qwen-plus
ZEP_API_KEY=...
```

फिर:

```bash
npm run setup:all
npm run dev
```

डिफ़ॉल्ट address:

- frontend: <http://localhost:3000>
- backend: <http://localhost:5001/health>

त्वरित backend check:

```bash
curl http://localhost:5001/health
```

## ज़रूरी practical checks

- अभी primary entrypoint CLI नहीं है; मुख्य surface web UI और Flask API है।
- backend `.env` को project root से पढ़ता है, `backend/` से नहीं।
- upload limit `50 MB` है और supported formats `pdf`, `md`, `txt`, `markdown` हैं।
- project और simulation artifacts `backend/uploads/` में रहते हैं।
- async task progress memory में रहता है; backend restart होने पर live progress state खो सकता है।
- default config development-friendly है, production-safe नहीं।

## भरोसेमंद upstream links

| Resource | Link |
| --- | --- |
| Official site | <https://mirofish.ai/> |
| GitHub repository | <https://github.com/666ghj/MiroFish> |
| Official English README | <https://github.com/666ghj/MiroFish/blob/main/README.md> |
| Official Chinese README | <https://github.com/666ghj/MiroFish/blob/main/README-ZH.md> |
| Live demo | <https://666ghj.github.io/mirofish-demo/> |
| License | <https://github.com/666ghj/MiroFish/blob/main/LICENSE> |
