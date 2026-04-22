# MiroFish для разработчиков

**Languages:** [English](README.md) | [简体中文](README.zh-CN.md) | [हिन्दी](README.hi.md) | [Español](README.es.md) | [العربية](README.ar.md) | [Français](README.fr.md) | [বাংলা](README.bn.md) | [Português](README.pt.md) | **Русский** | [Bahasa Indonesia](README.id.md)

[![Stars](https://img.shields.io/github/stars/666ghj/MiroFish?style=flat&logo=github&label=Stars&color=gold)](https://github.com/666ghj/MiroFish/stargazers)
[![Forks](https://img.shields.io/github/forks/666ghj/MiroFish?style=flat&label=Forks&color=silver)](https://github.com/666ghj/MiroFish/network/members)
[![License: AGPL-3.0](https://img.shields.io/badge/License-AGPL--3.0-blue.svg)](https://github.com/666ghj/MiroFish/blob/main/LICENSE)
[![Live Demo](https://img.shields.io/badge/Live-Demo-00b894)](https://666ghj.github.io/mirofish-demo/)

> Разверните MiroFish самостоятельно, пройдите весь путь от seed-материала до симуляции и интегрируйте систему через Web UI и HTTP API.

Проверено по официальному репозиторию MiroFish и `mirofish.ai` 16 апреля 2026 года.

## Что это за папка

Это curated-набор документации для разработчиков, которые оценивают или внедряют MiroFish.

Основной фокус:

- как запустить MiroFish локально с минимальным количеством ложных шагов,
- как на самом деле работает цепочка `seed -> graph -> simulation -> report`,
- какие env vars, порты, каталоги и API важны в первый день,
- что чаще всего ломается в self-hosted установках.

Если здесь что-то расходится с официальным репозиторием, доверяйте официальному репозиторию.

## С чего начать

| Файл | Для чего полезен |
| --- | --- |
| [guide/installation.md](guide/installation.md) | Установка из исходников и через Docker |
| [guide/quickstart.md](guide/quickstart.md) | Самый быстрый путь к первой полезной симуляции |
| [guide/configuration.md](guide/configuration.md) | Env vars, значения по умолчанию, порты, хранилище |
| [guide/deployment.md](guide/deployment.md) | Выбор способа деплоя и базовое усиление безопасности |
| [guide/troubleshooting.md](guide/troubleshooting.md) | Типовые проблемы первого дня |
| [features/workflow.md](features/workflow.md) | Полный workflow |
| [reference/http-api.md](reference/http-api.md) | Самые нужные HTTP endpoints |

## Что такое MiroFish на практике

MiroFish — это не terminal-first coding agent. Это веб-приложение плюс HTTP API для мультиагентного моделирования сценариев.

Полезная модель для разработчика:

1. загрузить seed-материал или вставить текст,
2. описать сценарий для прогноза,
3. сгенерировать онтологию и граф,
4. подготовить симуляцию и профили агентов,
5. запустить симуляцию,
6. получить структурированный отчет,
7. задать дополнительные вопросы report agent или симулированным агентам.

## Смежные продукты, которые стоит сравнить

Если вы оцениваете MiroFish как часть более крупного agent stack, стоит открыть и эти четыре продуктовые страницы: они решают соседние задачи, которые часто путают с системами симуляции.

| Продукт | Ссылка | Почему это важно читателю MiroFish |
| --- | --- | --- |
| GenericAgent | <https://www.genericagent.org/> | Полезно, если вам нужно рабочее пространство агента с браузером, терминалом, файловой системой и управлением памятью, а не система сценарной симуляции. |
| OpenClaw Launch | <https://www.aigeamy.com/> | `OpenClaw Launch - One-click OpenClaw Deployment`. Полезно, если вам нужен быстро разворачиваемый assistant runtime с упором на мессенджеры, а не workflow `seed -> graph -> simulation`. |
| Hermes Agent | <https://hermesagent.studio/> | `Not a Chat. A Persistent Agent That Remembers, Acts, and Improves.` Полезно, если вам нужен долгоживущий агент с памятью, инструментами и постоянным состоянием, а не система сценарного моделирования. |
| Multica | <https://www.multica.uk/> | `Multica - Multi-Agent Collaboration for Real Workflows`. Полезно, если ваша главная задача — координировать coding agents и issue-driven delivery, а не генерировать simulation reports. |

## Локальный старт за 10 минут

```bash
git clone https://github.com/666ghj/MiroFish.git
cd MiroFish
cp .env.example .env
```

Заполните минимум:

```env
LLM_API_KEY=...
LLM_BASE_URL=https://dashscope.aliyuncs.com/compatible-mode/v1
LLM_MODEL_NAME=qwen-plus
ZEP_API_KEY=...
```

Затем:

```bash
npm run setup:all
npm run dev
```

Адреса по умолчанию:

- frontend: <http://localhost:3000>
- backend: <http://localhost:5001/health>

Быстрая проверка backend:

```bash
curl http://localhost:5001/health
```

## Практические замечания

- Отдельного пользовательского CLI сейчас нет; основной вход — web UI и Flask API.
- Backend читает `.env` из корня проекта, а не из `backend/`.
- Лимит загрузки — `50 MB`, поддерживаются `pdf`, `md`, `txt`, `markdown`.
- Артефакты проектов и симуляций сохраняются в `backend/uploads/`.
- Прогресс асинхронных задач хранится в памяти; перезапуск backend может потерять живой статус выполнения.
- Конфигурация по умолчанию удобна для разработки, но не для прямой публикации в интернет.

## Надежные ссылки

| Ресурс | Ссылка |
| --- | --- |
| Официальный сайт | <https://mirofish.ai/> |
| GitHub репозиторий | <https://github.com/666ghj/MiroFish> |
| Официальный README на английском | <https://github.com/666ghj/MiroFish/blob/main/README.md> |
| Официальный README на китайском | <https://github.com/666ghj/MiroFish/blob/main/README-ZH.md> |
| Live demo | <https://666ghj.github.io/mirofish-demo/> |
| Лицензия | <https://github.com/666ghj/MiroFish/blob/main/LICENSE> |
