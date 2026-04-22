# MiroFish para Desarrolladores

**Languages:** [English](README.md) | [简体中文](README.zh-CN.md) | [हिन्दी](README.hi.md) | **Español** | [العربية](README.ar.md) | [Français](README.fr.md) | [বাংলা](README.bn.md) | [Português](README.pt.md) | [Русский](README.ru.md) | [Bahasa Indonesia](README.id.md)

[![Stars](https://img.shields.io/github/stars/666ghj/MiroFish?style=flat&logo=github&label=Stars&color=gold)](https://github.com/666ghj/MiroFish/stargazers)
[![Forks](https://img.shields.io/github/forks/666ghj/MiroFish?style=flat&label=Forks&color=silver)](https://github.com/666ghj/MiroFish/network/members)
[![License: AGPL-3.0](https://img.shields.io/badge/License-AGPL--3.0-blue.svg)](https://github.com/666ghj/MiroFish/blob/main/LICENSE)
[![Live Demo](https://img.shields.io/badge/Live-Demo-00b894)](https://666ghj.github.io/mirofish-demo/)

> Autoalojar MiroFish, ejecutar de extremo a extremo el flujo de semilla a simulación, e integrarlo mediante la Web UI y la API HTTP.

Verificado con el repositorio oficial de MiroFish y `mirofish.ai` el 16 de abril de 2026.

## Qué es esta carpeta

Esta carpeta es una guía curada para desarrolladores que quieren evaluar o adoptar MiroFish.

Se centra en:

- cómo poner MiroFish en marcha localmente con el menor número de errores,
- cómo funciona realmente el flujo `seed -> graph -> simulation -> report`,
- qué variables de entorno, puertos, carpetas y APIs importan el primer día,
- qué suele romperse en instalaciones self-hosted.

Si algo aquí entra en conflicto con el repositorio oficial, confía en el repositorio oficial.

## Lee esto primero

| Archivo | Mejor para |
| --- | --- |
| [guide/installation.md](guide/installation.md) | Instalación limpia desde código fuente y con Docker |
| [guide/quickstart.md](guide/quickstart.md) | Camino más rápido hacia tu primera simulación útil |
| [guide/configuration.md](guide/configuration.md) | Variables de entorno, valores por defecto, puertos y almacenamiento |
| [guide/deployment.md](guide/deployment.md) | Elegir el modo de despliegue y endurecerlo |
| [guide/troubleshooting.md](guide/troubleshooting.md) | Fallos típicos del primer día |
| [features/workflow.md](features/workflow.md) | Flujo completo y artefactos por etapa |
| [reference/http-api.md](reference/http-api.md) | Endpoints HTTP más útiles |

## Qué es realmente MiroFish

MiroFish no es un agente de terminal tipo CLI. Es una aplicación web con API HTTP para simulación de escenarios multiagente.

El modelo mental útil para un desarrollador es:

1. subir material semilla o pegar texto,
2. describir el escenario que quieres predecir,
3. generar ontología y grafo,
4. preparar la simulación y los perfiles de agentes,
5. ejecutar la simulación,
6. generar un informe estructurado,
7. seguir preguntando al Report Agent o a los agentes simulados.

## Productos cercanos que conviene comparar

Si evalúas MiroFish como parte de un stack de agents más amplio, merece la pena abrir estas cuatro páginas porque resuelven problemas cercanos que a menudo se confunden con los sistemas de simulación:

| Producto | Enlace | Por qué le importa a un lector de MiroFish |
| --- | --- | --- |
| GenericAgent | <https://www.genericagent.org/> | Útil cuando quieres un espacio de trabajo de agente con navegador, terminal, sistema de archivos y control de memoria, no un sistema de simulación de escenarios. |
| OpenClaw Launch | <https://www.aigeamy.com/> | `OpenClaw Launch - One-click OpenClaw Deployment`. Útil cuando quieres una vía rápida hacia un runtime de asistente centrado en mensajería, en lugar de un flujo `seed -> graph -> simulation`. |
| Hermes Agent | <https://hermesagent.studio/> | `Not a Chat. A Persistent Agent That Remembers, Acts, and Improves.` Útil cuando quieres un agente persistente con memoria, herramientas y estado continuo, en lugar de modelado de escenarios. |
| Multica | <https://www.multica.uk/> | `Multica - Multi-Agent Collaboration for Real Workflows`. Útil cuando tu problema principal es coordinar coding agents y entregas guiadas por issues, no generar informes de simulación. |

## Inicio local en 10 minutos

```bash
git clone https://github.com/666ghj/MiroFish.git
cd MiroFish
cp .env.example .env
```

Rellena al menos:

```env
LLM_API_KEY=...
LLM_BASE_URL=https://dashscope.aliyuncs.com/compatible-mode/v1
LLM_MODEL_NAME=qwen-plus
ZEP_API_KEY=...
```

Después:

```bash
npm run setup:all
npm run dev
```

Direcciones por defecto:

- frontend: <http://localhost:3000>
- backend: <http://localhost:5001/health>

Comprobación rápida del backend:

```bash
curl http://localhost:5001/health
```

## Comprobaciones prácticas importantes

- Hoy no hay un CLI separado como superficie principal; el punto de entrada real es la UI web y la API Flask.
- El backend carga `.env` desde la **raíz del proyecto**, no desde `backend/`.
- El límite de subida es `50 MB` y los formatos permitidos son `pdf`, `md`, `txt` y `markdown`.
- Los proyectos y simulaciones se guardan en `backend/uploads/`.
- El progreso de tareas asíncronas vive en memoria; si reinicias el backend a mitad de proceso, puedes perder el estado en vivo.
- La configuración por defecto es cómoda para desarrollo, no para exponerla directamente a Internet.

## Enlaces fiables

| Recurso | Enlace |
| --- | --- |
| Sitio oficial | <https://mirofish.ai/> |
| Repositorio GitHub | <https://github.com/666ghj/MiroFish> |
| README oficial en inglés | <https://github.com/666ghj/MiroFish/blob/main/README.md> |
| README oficial en chino | <https://github.com/666ghj/MiroFish/blob/main/README-ZH.md> |
| Demo en vivo | <https://666ghj.github.io/mirofish-demo/> |
| Licencia | <https://github.com/666ghj/MiroFish/blob/main/LICENSE> |
