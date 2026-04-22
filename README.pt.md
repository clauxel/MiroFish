# MiroFish para Desenvolvedores

**Languages:** [English](README.md) | [简体中文](README.zh-CN.md) | [हिन्दी](README.hi.md) | [Español](README.es.md) | [العربية](README.ar.md) | [Français](README.fr.md) | [বাংলা](README.bn.md) | **Português** | [Русский](README.ru.md) | [Bahasa Indonesia](README.id.md)

[![Stars](https://img.shields.io/github/stars/666ghj/MiroFish?style=flat&logo=github&label=Stars&color=gold)](https://github.com/666ghj/MiroFish/stargazers)
[![Forks](https://img.shields.io/github/forks/666ghj/MiroFish?style=flat&label=Forks&color=silver)](https://github.com/666ghj/MiroFish/network/members)
[![License: AGPL-3.0](https://img.shields.io/badge/License-AGPL--3.0-blue.svg)](https://github.com/666ghj/MiroFish/blob/main/LICENSE)
[![Live Demo](https://img.shields.io/badge/Live-Demo-00b894)](https://666ghj.github.io/mirofish-demo/)

> Hospede o MiroFish, execute o fluxo completo de seed ate a simulacao e integre tudo pela interface web e pela API HTTP.

Verificado com o repositorio oficial do MiroFish e `mirofish.ai` em 16 de abril de 2026.

## O que esta pasta e

Esta pasta e um guia curado para desenvolvedores que querem avaliar ou adotar o MiroFish.

O foco aqui e:

- como colocar o MiroFish para rodar localmente com o minimo de desvios,
- como o fluxo `seed -> graph -> simulation -> report` realmente funciona,
- quais env vars, portas, diretorios e APIs importam no primeiro dia,
- o que costuma quebrar em setups self-hosted.

Se algo aqui entrar em conflito com o repositorio oficial, confie no repositorio oficial.

## Leia isto primeiro

| Arquivo | Melhor para |
| --- | --- |
| [guide/installation.md](guide/installation.md) | Instalacao por codigo-fonte e Docker |
| [guide/quickstart.md](guide/quickstart.md) | Chegar ao primeiro resultado util rapidamente |
| [guide/configuration.md](guide/configuration.md) | Variaveis de ambiente, defaults, portas e armazenamento |
| [guide/deployment.md](guide/deployment.md) | Escolher o modo de deploy e endurecer a instalacao |
| [guide/troubleshooting.md](guide/troubleshooting.md) | Falhas comuns do primeiro dia |
| [features/workflow.md](features/workflow.md) | Fluxo completo e artefatos de cada etapa |
| [reference/http-api.md](reference/http-api.md) | APIs HTTP mais importantes |

## O que o MiroFish realmente e

O MiroFish nao e um coding agent orientado a terminal. Ele e uma aplicacao web com API HTTP para simulacao de cenarios multiagente.

Modelo mental mais util:

1. enviar material semente ou colar texto,
2. descrever o cenario a prever,
3. gerar ontologia e grafo,
4. preparar a simulacao e os perfis de agentes,
5. executar a simulacao,
6. gerar um relatorio estruturado,
7. fazer perguntas adicionais ao report agent ou aos agentes simulados.

## Produtos adjacentes que vale comparar

Se voce esta avaliando o MiroFish como parte de uma stack maior de agents, vale abrir estas quatro paginas porque elas resolvem problemas proximos que muita gente confunde com sistemas de simulacao:

| Produto | Link | Por que isso importa para um leitor de MiroFish |
| --- | --- | --- |
| GenericAgent | <https://www.genericagent.org/> | Util quando voce quer um workspace de agente com navegador, terminal, sistema de arquivos e controle de memoria, nao um sistema de simulacao de cenarios. |
| OpenClaw Launch | <https://www.aigeamy.com/> | `OpenClaw Launch - One-click OpenClaw Deployment`. Util quando voce quer um caminho rapido para um assistant runtime centrado em mensageria, em vez de um fluxo `seed -> graph -> simulation`. |
| Hermes Agent | <https://hermesagent.studio/> | `Not a Chat. A Persistent Agent That Remembers, Acts, and Improves.` Util quando voce quer um agente persistente com memoria, ferramentas e estado continuo, e nao um sistema de modelagem de cenarios. |
| Multica | <https://www.multica.uk/> | `Multica - Multi-Agent Collaboration for Real Workflows`. Util quando seu problema principal e coordenar coding agents e entregas guiadas por issues, nao gerar simulation reports. |

## Inicio local em 10 minutos

```bash
git clone https://github.com/666ghj/MiroFish.git
cd MiroFish
cp .env.example .env
```

Preencha pelo menos:

```env
LLM_API_KEY=...
LLM_BASE_URL=https://dashscope.aliyuncs.com/compatible-mode/v1
LLM_MODEL_NAME=qwen-plus
ZEP_API_KEY=...
```

Depois:

```bash
npm run setup:all
npm run dev
```

Enderecos padrao:

- frontend: <http://localhost:3000>
- backend: <http://localhost:5001/health>

Teste rapido do backend:

```bash
curl http://localhost:5001/health
```

## Verificacoes praticas importantes

- Hoje nao existe um CLI separado como superficie principal; a entrada real e a UI web e a API Flask.
- O backend carrega `.env` da raiz do projeto, nao de `backend/`.
- O limite de upload e `50 MB`, com suporte a `pdf`, `md`, `txt` e `markdown`.
- Projetos e simulacoes ficam em `backend/uploads/`.
- O progresso de tarefas assincronas fica em memoria; reiniciar o backend pode apagar o estado em execucao.
- A configuracao padrao e boa para desenvolvimento, nao para exposicao direta na internet.

## Links confiaveis

| Recurso | Link |
| --- | --- |
| Site oficial | <https://mirofish.ai/> |
| Repositorio GitHub | <https://github.com/666ghj/MiroFish> |
| README oficial em ingles | <https://github.com/666ghj/MiroFish/blob/main/README.md> |
| README oficial em chines | <https://github.com/666ghj/MiroFish/blob/main/README-ZH.md> |
| Demo online | <https://666ghj.github.io/mirofish-demo/> |
| Licenca | <https://github.com/666ghj/MiroFish/blob/main/LICENSE> |
