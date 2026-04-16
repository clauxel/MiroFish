# FAQ

## Is MiroFish basically a chatbot?

No. The important distinction is that MiroFish is a multi-agent simulation workflow, not a single-response chat product.

## Do I need to use the UI, or can I call the backend directly?

You can call the backend directly through its HTTP API. For many developers, the UI is still the fastest way to learn the workflow before writing an integration.

## What file types can I upload?

The backend allows `pdf`, `md`, `txt`, and `markdown` uploads, up to `50 MB`.

## Do I need Zep?

Yes for the standard upstream setup. The backend validates `ZEP_API_KEY` at startup.

## Which models work?

Any provider that exposes an OpenAI-compatible API shape. The upstream README recommends Alibaba Bailian `qwen-plus` as a starting point.

## Where is my local data stored?

Under `backend/uploads/`, especially `projects/` and `simulations/`.

## What survives a restart?

Project and simulation artifacts on disk usually survive. In-memory task progress does not.

## Is there built-in authentication?

Not in the upstream backend routes. Add your own auth layer if you expose MiroFish beyond a trusted network.

## Is there a CLI?

Not as the primary product surface. Developers mainly use the web UI and HTTP API.

## What is the safest first run?

One focused seed, a clear requirement, one platform, and a low round count.
