# Installation

This guide is for developers who want the shortest reliable path to a working MiroFish environment.

## Choose Your Path

| Path | Best for | Tradeoff |
| --- | --- | --- |
| Source install | Local development, debugging, reading code, changing frontend/backend separately | More host dependencies |
| Docker Compose | Fast evaluation on one machine | Less transparent when you need to debug internals |

## Prerequisites

| Tool | Version | Why you need it | Verify |
| --- | --- | --- | --- |
| Node.js | `18+` | Root scripts and Vite frontend | `node -v` |
| npm | bundled with Node | Install JS dependencies | `npm -v` |
| Python | `>=3.11` and `<=3.12` | Flask backend and simulation services | `python --version` |
| uv | latest | Backend dependency management | `uv --version` |

## Source Install

### 1. Clone the upstream repository

```bash
git clone https://github.com/666ghj/MiroFish.git
cd MiroFish
```

### 2. Create the environment file

```bash
cp .env.example .env
```

PowerShell equivalent:

```powershell
Copy-Item .env.example .env
```

### 3. Fill the required keys

Minimum required:

```env
LLM_API_KEY=...
LLM_BASE_URL=https://dashscope.aliyuncs.com/compatible-mode/v1
LLM_MODEL_NAME=qwen-plus
ZEP_API_KEY=...
```

Notes:

- MiroFish expects an **OpenAI-compatible** LLM endpoint.
- The official upstream README recommends Alibaba Bailian `qwen-plus` for a balanced default.
- `ZEP_API_KEY` is required for graph memory features; the backend will refuse to start without it.

### 4. Install dependencies

```bash
npm run setup:all
```

What this does:

- installs root dependencies,
- installs frontend dependencies under `frontend/`,
- runs `uv sync` in `backend/`.

If you prefer step-by-step install:

```bash
npm run setup
npm run setup:backend
```

### 5. Start the stack

```bash
npm run dev
```

Default endpoints:

- frontend: `http://localhost:3000`
- backend: `http://localhost:5001`

### 6. Verify the backend first

```bash
curl http://localhost:5001/health
```

If health works but the UI does not, the problem is usually on the frontend side or a port/proxy mismatch.

## Docker Compose Install

### 1. Clone the upstream repository

```bash
git clone https://github.com/666ghj/MiroFish.git
cd MiroFish
```

### 2. Create `.env`

```bash
cp .env.example .env
```

### 3. Start the container

```bash
docker compose up -d
```

The upstream `docker-compose.yml`:

- uses `ghcr.io/666ghj/mirofish:latest`,
- reads `.env` from the project root,
- exposes `3000` and `5001`,
- mounts `./backend/uploads:/app/backend/uploads`.

### 4. Verify the container

```bash
docker ps
docker logs -f mirofish
```

## Windows Notes

- PowerShell is fine for source install.
- `backend/run.py` includes explicit UTF-8 handling for Windows console output.
- If you still see encoding issues, prefer Windows Terminal and a modern Python build.

## First-Day Success Checklist

- `.env` exists in the repository root.
- `LLM_API_KEY` and `ZEP_API_KEY` are filled.
- `npm run setup:all` completes.
- `http://localhost:5001/health` responds.
- `http://localhost:3000` loads.
- You can reach the ontology upload step in the UI.
