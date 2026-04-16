# Deployment

This page is about choosing the right self-hosted shape and avoiding the most common production mistakes.

## Pick the Right Deployment Style

| Deployment | Best for | Notes |
| --- | --- | --- |
| Source mode | Local development and debugging | Easiest way to inspect frontend and backend separately |
| Docker Compose | Fast internal demo or single-node deployment | Good default when you do not need to modify code immediately |
| Reverse-proxied self-host | Team/internal use | Add auth, TLS, and network controls yourself |

## Recommended Production Shape

With the current upstream repository, the most practical production-minded layout is:

1. run the Flask backend behind a process manager,
2. build the frontend once,
3. serve the built frontend through Nginx,
4. proxy `/api` and `/health` to the backend,
5. persist `backend/uploads`.

That is usually a better long-term shape than exposing the Vite dev server.

## Source Mode

Use source mode when you want:

- Vue frontend hot reload,
- direct Flask logs,
- easier API debugging,
- quick code changes.

Baseline commands:

```bash
git clone https://github.com/666ghj/MiroFish.git
cd MiroFish
cp .env.example .env
npm run setup:all
npm run dev
```

## Docker Compose Mode

Use Docker when you want the quickest containerized evaluation path:

```bash
git clone https://github.com/666ghj/MiroFish.git
cd MiroFish
cp .env.example .env
docker compose up -d
```

Persist this directory:

```text
backend/uploads
```

Without it, you lose stored local artifacts after container recreation.

For most teams, this is the safest first deployment path:

- keep the upstream container for app behavior,
- put Nginx or another reverse proxy in front,
- mount `backend/uploads`,
- add your own access control.

## Source Build + Static Frontend

If you want a source-based deployment without shipping the Vite dev server:

```bash
git clone https://github.com/666ghj/MiroFish.git
cd MiroFish
cp .env.example .env
npm run setup:all
npm run build
cd backend
uv run python run.py
```

What this gives you:

- built frontend assets under `frontend/dist`,
- backend on port `5001`,
- a static frontend that can be served by Nginx.

Important detail:
the frontend router uses HTML5 history mode, so your web server must send unknown application routes back to `index.html`.

## Reverse Proxy Guidance

MiroFish does not ship with a full internet-facing production shell.

If you expose it beyond a private machine:

- terminate TLS at a reverse proxy,
- proxy `/api` to backend port `5001`,
- serve the built frontend directly from the reverse proxy, or only proxy port `3000` for temporary internal testing,
- restrict access with VPN, SSO, or an auth proxy,
- do not leave the raw Flask port openly exposed on the public internet.

### Example Nginx config

```nginx
server {
  listen 80;
  server_name mirofish.example.com;

  root /srv/mirofish/frontend/dist;
  index index.html;

  location /api/ {
    proxy_pass http://127.0.0.1:5001;
    proxy_http_version 1.1;
    proxy_set_header Host $host;
    proxy_set_header X-Real-IP $remote_addr;
    proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    proxy_set_header X-Forwarded-Proto $scheme;
    proxy_read_timeout 600s;
  }

  location = /health {
    proxy_pass http://127.0.0.1:5001/health;
    proxy_http_version 1.1;
    proxy_set_header Host $host;
  }

  location / {
    try_files $uri $uri/ /index.html;
  }
}
```

Why `try_files` matters:
the frontend uses `createWebHistory()`, so routes like `/report/<report_id>` will 404 after refresh unless Nginx falls back to `index.html`.

### Example systemd unit for the backend

```ini
[Unit]
Description=MiroFish backend
After=network.target

[Service]
Type=simple
User=mirofish
Group=mirofish
WorkingDirectory=/srv/mirofish/backend
Environment=PYTHONUNBUFFERED=1
ExecStart=/usr/local/bin/uv run python run.py
Restart=always
RestartSec=5

[Install]
WantedBy=multi-user.target
```

Adjust:

- `User` / `Group`
- `WorkingDirectory`
- the `uv` path

### Backend deploy/update sequence

```bash
cd /srv/mirofish
git pull
npm run setup:all
npm run build
sudo systemctl restart mirofish-backend
```

If you are using Docker instead, prefer rebuilding or repulling the image instead of mixing source and container workflows.

## Security Reality Check

Out of the box, the backend is optimized for development, not hardened production:

- `FLASK_DEBUG` defaults to `True`,
- `SECRET_KEY` defaults to a placeholder,
- CORS allows all origins for `/api/*`,
- there is no built-in authentication layer in the backend routes.

Minimum hardening checklist:

- set `FLASK_DEBUG=False`,
- set a real `SECRET_KEY`,
- place the app behind a reverse proxy,
- restrict network access,
- protect the host and secrets store,
- avoid sharing the raw API publicly without an auth layer.

What not to do:

- do not expose the raw Flask port directly to the public internet,
- do not rely on the Vite dev server as your public frontend,
- do not forget to persist `backend/uploads`,
- do not forget the history-mode fallback for frontend routes.

## Operational Checks

Before calling a deployment healthy:

- `/health` responds through your intended access path,
- the frontend can reach `/api/*`,
- direct deep links like `/simulation/<id>` or `/report/<id>` do not 404 after refresh,
- uploads persist across restarts,
- a graph build completes,
- a report can be generated end to end,
- logs are visible somewhere your team can inspect.

## When To Rebuild vs Restart

Restart is usually enough when:

- you changed `.env`,
- a long-running task got wedged,
- you are testing networking changes.

Rebuild is usually needed when:

- you changed frontend code,
- you changed backend dependencies,
- you changed the container image/tag,
- the image cache is stale.
