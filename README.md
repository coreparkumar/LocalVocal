# LocalStack — Your Own Coding AI on 6 GB of VRAM

An interactive, single-page setup guide for running a **free, fully local AI coding assistant** in VS Code — no subscription, no API bills, and no source code leaving your machine. It walks you through Ollama, Qwen2.5-Coder, and the Continue extension on a modest NVIDIA laptop GPU, and ships with a built-in Gemini chat panel for the questions the steps don't cover.

![Local](https://img.shields.io/badge/runs-100%25%20local-54d6c4)
![Cost](https://img.shields.io/badge/cost-%240-e0883f)
![GPU](https://img.shields.io/badge/VRAM-6%20GB%20RTX%204050-blue)
![Deps](https://img.shields.io/badge/dependencies-none-555)

---

## Project goal

Make a private, on-device coding assistant achievable on hardware most developers already own. Cloud coding assistants are fast and capable, but they cost money every month and route your code through someone else's servers. This project's goal is to remove both of those barriers with a guide you can actually follow end to end on a 6 GB GPU — and to keep the only optional cloud touchpoint (a Gemini Q&A helper) under your own key and control.

In short: **get a usable AI coding assistant running locally, for free, in about fifteen minutes.**

---

## What you get out of it

- **Zero ongoing cost.** The whole local stack — model runtime, models, autocomplete, and codebase search — is free and open source.
- **Privacy by default.** Inference runs on your GPU. Your code never leaves the machine. The only network call is the optional Gemini chat, which you opt into with your own key.
- **Lower token usage.** A local "second brain" indexes your project and retrieves only the relevant chunks, so when you *do* reach for a cloud model you send a few hundred tokens of context instead of whole files.
- **Sized for real hardware.** Every recommendation is tuned for 6 GB of VRAM — which models fit, what context length is safe, and when to fall back — instead of assuming a datacenter GPU.
- **No-fuss reference.** Copy-paste commands, a ready-made Continue config, and a VRAM budget visualizer, all on one page you can keep open while you work.
- **An escape hatch for hard questions.** The embedded Gemini panel is primed with your exact hardware context, so off-script questions ("why is my model loading on CPU?") get grounded answers.

---

## Who it's for

- Developers who want Copilot-style help **without a subscription**.
- Anyone working with **sensitive or proprietary code** that shouldn't be sent to third-party servers.
- People on **modest or older GPUs** (6–8 GB VRAM) who assume local AI is out of reach.
- Tinkerers who want to **own their stack** end to end.

---

## What the guide sets up

| Layer | Tool | Role |
|---|---|---|
| Runtime | [Ollama](https://ollama.com) | Serves models locally on `localhost:11434` |
| Chat / edit model | `qwen2.5-coder:7b` | The everyday reasoning workhorse |
| Autocomplete model | `qwen2.5-coder:1.5b` | Fast inline completions |
| Embeddings | `nomic-embed-text` | Powers local codebase search |
| Editor integration | [Continue](https://continue.dev) | Autocomplete, chat, edits, `@codebase` RAG |
| Agentic editing *(optional)* | [Cline](https://cline.bot) | Multi-file "do this task" loops |

---

## Project structure

This is a static, dependency-free site — the entire app is one HTML file.

```
localstack/
├── index.html        # the whole app: guide, VRAM meter, config + info modals, Gemini chat
└── README.md         # this file
```

Everything (markup, styles, and logic) lives in `index.html` so it can be opened directly from disk or dropped onto any static host with no build step. Optional additions if you fork it:

```
localstack/
├── index.html
├── README.md
├── LICENSE           # add your license of choice
└── assets/           # optional: split out CSS/JS or add screenshots here
```

---

## Getting started

### Option A — run it locally (simplest)

Download `index.html` and **double-click it**. It runs fully from disk, and the Gemini chat still works. No server, no hosting, nothing to configure.

### Option B — deploy to GitHub Pages

1. Put `index.html` in the root of a repository.
2. Go to **Settings → Pages**.
3. Under **Build and deployment**, choose **Deploy from a branch**, select your branch, and set the folder to `/ (root)`.
4. Save — your page goes live at `https://<username>.github.io/<repo>/`.

> Because the page accepts a personal API key in the browser, keep the repository **private** if you'd rather it not be publicly discoverable. (GitHub Pages serves private repos on paid plans; otherwise the double-click method avoids the question entirely.)

---

## Configuration

The local guide needs **no configuration** — it's just instructions and commands.

The **Gemini chat panel** is the only part that needs a key:

1. Click **Gemini key** in the top-right.
2. Paste your API key and, optionally, change the model (defaults to `gemini-2.5-flash`).
3. Save.

The key is stored only in your browser's local storage on that device and is sent directly to Google's API — never to any intermediary server. Clear it any time from the same panel.

> **Working with company code?** Use an API key provisioned through your organization rather than a personal account, and keep proprietary data out of any cloud calls.

---

## Target system

The guide is calibrated for this configuration:

| Component | Spec | Notes |
|---|---|---|
| GPU | NVIDIA RTX 4050 | 6 GB VRAM — drives all inference |
| RAM | 16 GB | OS + any CPU-offloaded layers |
| Storage | 500 GB SSD | Models total ~3–6 GB |
| OS | Windows | Ollama + CUDA runtime |

**What it runs well:** 3B models entirely on-GPU (instant), and 7B at Q4 with a 4k–8k context (≈25–40 tokens/sec). 14B and larger spill to CPU and slow down — intentionally out of scope.

---

## Troubleshooting

- **Model loads on CPU instead of GPU** — update your NVIDIA driver (it bundles the CUDA runtime) and restart the Ollama service. Confirm with `ollama ps` (the `PROCESSOR` column should read `GPU`).
- **Chat is slow** — lower the context length to 4k in the Continue config, or switch to the 3B model so it stays fully resident.
- **Gemini chat returns "model not found"** — model names change often; open the key panel and switch to a current model.
- **Speed drops on battery** — stay plugged in and set Windows to *Best Performance*; laptop GPUs throttle aggressively on battery.

---

## License

No license is included by default. Add one (e.g. MIT) if you intend to share or accept contributions.

---

*Everything in this project runs offline except the optional Gemini chat, which uses your own key.*
