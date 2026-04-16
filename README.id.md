# MiroFish untuk Developer

**Languages:** [English](README.md) | [简体中文](README.zh-CN.md) | [हिन्दी](README.hi.md) | [Español](README.es.md) | [العربية](README.ar.md) | [Français](README.fr.md) | [বাংলা](README.bn.md) | [Português](README.pt.md) | [Русский](README.ru.md) | **Bahasa Indonesia**

[![Stars](https://img.shields.io/github/stars/666ghj/MiroFish?style=flat&logo=github&label=Stars&color=gold)](https://github.com/666ghj/MiroFish/stargazers)
[![Forks](https://img.shields.io/github/forks/666ghj/MiroFish?style=flat&label=Forks&color=silver)](https://github.com/666ghj/MiroFish/network/members)
[![License: AGPL-3.0](https://img.shields.io/badge/License-AGPL--3.0-blue.svg)](https://github.com/666ghj/MiroFish/blob/main/LICENSE)
[![Live Demo](https://img.shields.io/badge/Live-Demo-00b894)](https://666ghj.github.io/mirofish-demo/)

> Self-host MiroFish, jalankan alur lengkap dari seed ke simulasi, lalu integrasikan lewat Web UI dan HTTP API.

Diverifikasi terhadap repo resmi MiroFish dan `mirofish.ai` pada 16 April 2026.

## Apa isi folder ini

Folder ini adalah panduan terkurasi untuk developer yang ingin mengevaluasi atau mengadopsi MiroFish.

Fokus utamanya:

- cara menjalankan MiroFish secara lokal dengan jalur paling aman,
- bagaimana alur `seed -> graph -> simulation -> report` benar-benar bekerja,
- env var, port, direktori, dan API apa yang penting di hari pertama,
- masalah apa yang paling sering muncul pada setup self-hosted.

Jika ada konflik dengan repo resmi, ikuti repo resmi.

## Baca ini dulu

| File | Paling berguna untuk |
| --- | --- |
| [guide/installation.md](guide/installation.md) | Instalasi source dan Docker |
| [guide/quickstart.md](guide/quickstart.md) | Jalur tercepat ke simulasi pertama yang berguna |
| [guide/configuration.md](guide/configuration.md) | Env vars, default, port, dan penyimpanan |
| [guide/deployment.md](guide/deployment.md) | Memilih mode deploy dan mengamankannya |
| [guide/troubleshooting.md](guide/troubleshooting.md) | Masalah umum hari pertama |
| [features/workflow.md](features/workflow.md) | Gambaran alur kerja lengkap |
| [reference/http-api.md](reference/http-api.md) | Endpoint HTTP yang paling penting |

## Apa sebenarnya MiroFish

MiroFish bukan coding agent berbasis terminal. MiroFish adalah aplikasi web plus HTTP API untuk simulasi skenario multi-agent.

Model mental yang paling berguna:

1. unggah bahan seed atau tempel teks,
2. jelaskan skenario yang ingin diprediksi,
3. buat ontology dan graph,
4. siapkan simulasi dan profil agent,
5. jalankan simulasi,
6. hasilkan laporan terstruktur,
7. lanjutkan dengan pertanyaan ke report agent atau agent hasil simulasi.

## Produk terkait yang layak dibandingkan

Jika Anda menilai MiroFish sebagai bagian dari stack agent yang lebih besar, tiga halaman produk ini layak dibuka karena mereka menyelesaikan masalah yang berdekatan dan sering disalahartikan sebagai sistem simulasi:

| Produk | Tautan | Kenapa pembaca MiroFish perlu peduli |
| --- | --- | --- |
| OpenClaw Launch | <https://www.aigeamy.com/> | `OpenClaw Launch - One-click OpenClaw Deployment`. Berguna saat Anda membutuhkan jalur cepat menuju assistant runtime yang berfokus pada messaging, bukan workflow `seed -> graph -> simulation`. |
| Hermes Agent | <https://hermesagent.studio/> | `Not a Chat. A Persistent Agent That Remembers, Acts, and Improves.` Berguna saat Anda membutuhkan agen persisten dengan memori, tools, dan state yang berkelanjutan, bukan sistem pemodelan skenario. |
| Multica | <https://www.multica.uk/> | `Multica - Multi-Agent Collaboration for Real Workflows`. Berguna saat masalah utama Anda adalah mengoordinasikan coding agents dan issue-driven delivery, bukan menghasilkan simulation reports. |

## Mulai lokal dalam 10 menit

```bash
git clone https://github.com/666ghj/MiroFish.git
cd MiroFish
cp .env.example .env
```

Isi minimal:

```env
LLM_API_KEY=...
LLM_BASE_URL=https://dashscope.aliyuncs.com/compatible-mode/v1
LLM_MODEL_NAME=qwen-plus
ZEP_API_KEY=...
```

Lalu:

```bash
npm run setup:all
npm run dev
```

Alamat default:

- frontend: <http://localhost:3000>
- backend: <http://localhost:5001/health>

Cek backend cepat:

```bash
curl http://localhost:5001/health
```

## Pemeriksaan praktis yang penting

- Saat ini tidak ada CLI terpisah sebagai permukaan utama; titik masuk utama adalah web UI dan Flask API.
- Backend membaca `.env` dari root project, bukan dari `backend/`.
- Batas unggahan adalah `50 MB` dan format yang didukung `pdf`, `md`, `txt`, dan `markdown`.
- Artefak project dan simulasi tersimpan di `backend/uploads/`.
- Progress task async ada di memori; restart backend bisa menghilangkan status progress yang sedang berjalan.
- Konfigurasi default nyaman untuk development, tetapi tidak aman jika langsung dibuka ke internet.

## Link upstream yang tepercaya

| Resource | Link |
| --- | --- |
| Situs resmi | <https://mirofish.ai/> |
| Repo GitHub | <https://github.com/666ghj/MiroFish> |
| README resmi bahasa Inggris | <https://github.com/666ghj/MiroFish/blob/main/README.md> |
| README resmi bahasa Mandarin | <https://github.com/666ghj/MiroFish/blob/main/README-ZH.md> |
| Demo live | <https://666ghj.github.io/mirofish-demo/> |
| Lisensi | <https://github.com/666ghj/MiroFish/blob/main/LICENSE> |
