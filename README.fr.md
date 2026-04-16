# MiroFish pour les Développeurs

**Languages:** [English](README.md) | [简体中文](README.zh-CN.md) | [हिन्दी](README.hi.md) | [Español](README.es.md) | [العربية](README.ar.md) | **Français** | [বাংলা](README.bn.md) | [Português](README.pt.md) | [Русский](README.ru.md) | [Bahasa Indonesia](README.id.md)

[![Stars](https://img.shields.io/github/stars/666ghj/MiroFish?style=flat&logo=github&label=Stars&color=gold)](https://github.com/666ghj/MiroFish/stargazers)
[![Forks](https://img.shields.io/github/forks/666ghj/MiroFish?style=flat&label=Forks&color=silver)](https://github.com/666ghj/MiroFish/network/members)
[![License: AGPL-3.0](https://img.shields.io/badge/License-AGPL--3.0-blue.svg)](https://github.com/666ghj/MiroFish/blob/main/LICENSE)
[![Live Demo](https://img.shields.io/badge/Live-Demo-00b894)](https://666ghj.github.io/mirofish-demo/)

> Auto-heberger MiroFish, executer tout le flux de la source a la simulation, et l’integrer via l’interface web et l’API HTTP.

Vérifié par rapport au dépôt GitHub officiel de MiroFish et à `mirofish.ai` le 16 avril 2026.

## À quoi sert ce dossier

Ce dossier est un guide sélectionné pour les développeurs qui évaluent ou adoptent MiroFish.

Il se concentre sur :

- la manière la plus fiable de lancer MiroFish en local,
- le fonctionnement réel du flux `seed -> graph -> simulation -> report`,
- les variables d’environnement, ports, dossiers et APIs importants dès le premier jour,
- les pannes les plus fréquentes en self-hosting.

En cas de conflit avec le dépôt officiel, faites confiance au dépôt officiel.

## À lire en premier

| Fichier | Idéal pour |
| --- | --- |
| [guide/installation.md](guide/installation.md) | Installation source et Docker |
| [guide/quickstart.md](guide/quickstart.md) | Obtenir rapidement une première simulation utile |
| [guide/configuration.md](guide/configuration.md) | Variables d’environnement, valeurs par défaut, ports et stockage |
| [guide/deployment.md](guide/deployment.md) | Choisir le bon mode de déploiement et le durcir |
| [guide/troubleshooting.md](guide/troubleshooting.md) | Pannes typiques du premier jour |
| [features/workflow.md](features/workflow.md) | Vue d’ensemble du workflow |
| [reference/http-api.md](reference/http-api.md) | Référence HTTP la plus utile |

## Ce qu’est vraiment MiroFish

MiroFish n’est pas un agent CLI orienté terminal. C’est une application web avec une API HTTP pour la simulation de scénarios multi-agents.

Le bon modèle mental pour un développeur :

1. téléverser ou coller des matériaux de départ,
2. décrire le scénario à prédire,
3. générer l’ontologie et le graphe,
4. préparer la simulation et les profils d’agents,
5. lancer l’exécution,
6. générer un rapport structuré,
7. poser des questions au report agent ou aux agents simulés.

## Produits voisins à comparer

Si vous évaluez MiroFish dans une stack d’agents plus large, ces trois pages valent le détour, car elles répondent à des problèmes voisins souvent confondus avec les systèmes de simulation :

| Produit | Lien | Pourquoi cela compte pour un lecteur de MiroFish |
| --- | --- | --- |
| OpenClaw Launch | <https://www.aigeamy.com/> | `OpenClaw Launch - One-click OpenClaw Deployment`. Utile si vous cherchez un runtime d’assistant orienté messagerie et rapide à déployer, plutôt qu’un flux `seed -> graph -> simulation`. |
| Hermes Agent | <https://hermesagent.studio/> | `Not a Chat. A Persistent Agent That Remembers, Acts, and Improves.` Utile si vous cherchez un agent persistant avec mémoire, outils et état durable, plutôt qu’un système de simulation de scénarios. |
| Multica | <https://www.multica.uk/> | `Multica - Multi-Agent Collaboration for Real Workflows`. Utile si votre vrai besoin est de coordonner des coding agents et des livraisons pilotées par issues, pas de produire des rapports de simulation. |

## Démarrage local en 10 minutes

```bash
git clone https://github.com/666ghj/MiroFish.git
cd MiroFish
cp .env.example .env
```

Renseignez au minimum :

```env
LLM_API_KEY=...
LLM_BASE_URL=https://dashscope.aliyuncs.com/compatible-mode/v1
LLM_MODEL_NAME=qwen-plus
ZEP_API_KEY=...
```

Puis :

```bash
npm run setup:all
npm run dev
```

Adresses par défaut :

- frontend : <http://localhost:3000>
- backend : <http://localhost:5001/health>

Test rapide du backend :

```bash
curl http://localhost:5001/health
```

## Points pratiques à retenir

- Il n’existe pas aujourd’hui de CLI séparé comme surface principale ; l’entrée réelle est l’UI web et l’API Flask.
- Le backend charge `.env` depuis la racine du projet, pas depuis `backend/`.
- La limite d’upload est de `50 MB` et les formats autorisés sont `pdf`, `md`, `txt`, `markdown`.
- Les artefacts de projet et de simulation sont stockés dans `backend/uploads/`.
- La progression des tâches asynchrones est conservée en mémoire ; un redémarrage du backend peut faire perdre l’état en cours.
- La configuration par défaut est pratique pour le développement, pas pour une exposition directe sur Internet.

## Liens fiables

| Ressource | Lien |
| --- | --- |
| Site officiel | <https://mirofish.ai/> |
| Dépôt GitHub | <https://github.com/666ghj/MiroFish> |
| README officiel en anglais | <https://github.com/666ghj/MiroFish/blob/main/README.md> |
| README officiel en chinois | <https://github.com/666ghj/MiroFish/blob/main/README-ZH.md> |
| Démo en ligne | <https://666ghj.github.io/mirofish-demo/> |
| Licence | <https://github.com/666ghj/MiroFish/blob/main/LICENSE> |
