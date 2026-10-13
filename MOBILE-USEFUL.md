# Mobile-useful notes

**Purpose:** After every search, anything found that could actually be used on this
phone (Android 16, Alpine aarch64, no gcc, no Docker, terminal-only) goes here —
one line per item, plain language, with a link. Note what works and what does not.

---

## Anything × Awesome search (2026-10-13)

- pinkbubblebubble/awesome-agentic-anything (github) — list of AI agent systems; good for picking agent ideas that can run as small Node/CLI tools here — https://github.com/pinkbubblebubble/awesome-agentic-anything
- ArtemShatokhin/awesome-anythingllm-alternatives (github) — free chat-AI apps; AnythingLLM is Node-based so it can run on this phone (11.7 GB RAM), others may too — https://github.com/ArtemShatokhin/awesome-anythingllm-alternatives
- VainF/Awesome-Anything (github) — big list of general AI methods; browser-browse only, not runnable here — https://github.com/VainF/Awesome-Anything
- Segment Anything (SAM) repos (10) — photo-cutting AI needs a big GPU; NOT usable on this phone — https://github.com/Hedlen/awesome-segment-anything
- Renumics/awesome-anything2vec — sensor-to-numbers tools, industrial Python; not useful here — https://github.com/Renumics/awesome-anything2vec
- szrlee/awesome-learning-to-do-anything — 2018 paper list, stale; skip — https://github.com/szrlee/awesome-learning-to-do-anything- msitarzewski/agency-agents — 200+ AI agent personas as plain .md files; install script supports OpenCode (OPENCODE_AGENTS_DIR); text-only so runs fine here as reference/agent ideas — https://github.com/msitarzewski/agency-agents
- msitarzewski/agency-agents-app — desktop app (Win/Mac/Linux) for browsing/installing those personas; NOT for this phone — https://github.com/msitarzewski/agency-agents-app

## AI models by capability (2026-10-13)

- models-researched-ranked.md — researched ranking of all 43 models your OmniRoute gateway knows (21 real families), grouped by capability: coding, reasoning, agents, vision, cyber, science, value — file: /sdcard/Documents/opencode-catalog/research/models-researched-ranked.md
- Cheapest good daily pick here: mimo-v2.6-pro (open, $0.44/$0.87) — reachable via OmniRoute gateway on this phone
- Best all-rounders here: gpt-6-astra, claude-opus-5.5, claude-fable-5.1 — all cloud, used through OmniRoute, no local run
- None of these run on the phone itself; they are API models (no GPU here) — used via the OmniRoute gateway

## OmniRoute origin (2026-10-14)

- OmniRoute (v3.8.51, installed here) — AI gateway: one endpoint in front of ~358 providers with auto-fallback. Origin: a TypeScript fork of 9Router, which is a JS port of the Go project CLIProxyAPI. Runs fine on this phone as Node/Next.js on port 20128; skip the heavy ONNX LLMLingua-2 engine, prefer RTK/Caveman/Lite. As of 2026-10-14 it answers through GitHub Copilot (OAuth, no key pasting: omniroute oauth start --provider copilot --no-browser) with tool calling, and its OpenCode plugin exposes ~500 live models. Full notes: research/omniroute-origin.md
- diegosouzapw/OmniRoute — upstream repo of the gateway installed on this phone — https://github.com/diegosouzapw/OmniRoute
- decolua/9router — the JavaScript project OmniRoute forked from; also runs on Node here — https://github.com/decolua/9router
- router-for-me/CLIProxyAPI — the Go original that started the whole idea; Go means NOT installable here (no compiler), reference only — https://github.com/router-for-me/CLIProxyAPI

## Uncensored models (2026-10-14)

- No uncensored chat model is built into the gateway. The only NSFW entries are 164 AI Horde image checkpoints (not chat). Details: research/uncensored-models.md
- UncloseAI / Hermes (provider alias unc, free, no key) — the one uncensored-ish chat route that works here. Live id turboderp/Qwen3.8-27B-exl3, called as unc/turboderp/Qwen3.8-27B-exl3 on the gateway. Verified answering. The id shipped in the catalog (Lorbus/Qwen3.6-27B-int4-AutoRound) is stale and 404s; the provider is passthrough so the new id still works. It is a reasoning model (set chat_template_kwargs.enable_thinking=false) and rate-limits (429) when busy. Endpoint: https://hermes.ai.unturf.com/v1/models
- Need a key for real uncensored models: venice (Venice.ai), uc-direct (uncensored.com), nous-research (Hermes), openrouter (abliterated community models). Add with omniroute providers add <id> --credential-stdin.
- Local uncensored model: not feasible without work (no GPU, no compiler, ~3 GB free RAM). A no-compiler path exists (llamafile + a 3-4B GGUF, ~2 GB), tight but possible.

## What actually answers on this phone (2026-10-14)

- Only DuckDuckGo web chat is free and working here: the 6 ddgw/* models (in OpenCode as omniroute/ddgw/...). Fast (1-2 s) and keyless, but NO tool calling and it rate-limits fast.
- Pollinations now needs a free key (its models were free before): get one at https://enter.pollinations.ai/keys then run: omniroute providers add pollinations --credential-stdin
- llm7 free tier is out of credits ("402 credits exhausted"); its live model list is https://api.llm7.io/v1/models (the model names shown in the app are stale).
- Easiest free keys that work on this device: Groq https://console.groq.com/keys , Google AI Studio https://aistudio.google.com/apikey , Cloudflare Workers AI https://developers.cloudflare.com/workers-ai/ , OpenRouter https://openrouter.ai/keys
- Add any key with: omniroute providers add <id> --credential-stdin  (paste the key, press Ctrl-D). Models then appear in OpenCode after "opencode reload".
- New combo model "free-all" was added: it fails over across the 6 ddgw models so there is one dependable free chat entry.

## REAL fix: OmniRoute makes the key itself (2026-10-14)

- You were right: OmniRoute creates the credential itself. Command: "omniroute oauth start --provider copilot --no-browser" prints a code and https://github.com/login/device . Approve it on the phone and the GitHub Copilot connection is created and stored by OmniRoute. Nothing is pasted.
- That single login unlocked 93 models: Claude (Sonnet 5, Haiku 4.5), GPT-5.x, GPT-4o, Gemini 3.x, Grok 4.5, MAI-Code, Kimi. They appear in OpenCode as omniroute/gh/... and TOOL CALLING works (verified).
- Caveat: the list is wider than what the account is entitled to. claude-opus-5, claude-opus-4.8, claude-sonnet-4.5 and gpt-5.5 answer "model_not_supported"; gemini-3.1-pro-preview, gemini-3.6-flash and kimi-k2.7-code say "not available for integrator". Copilot also rate-limits per model ("429 ... cooling down"), so do not fire many parallel calls.
- Verified answering here: gh/claude-haiku-4.5, gh/claude-sonnet-5, gh/gpt-5.6-luna, gh/gpt-5.4, gh/gpt-4o-2024-11-20, gh/grok-4.5, gh/gemini-3.7-flash, gh/mai-code-1.1-flash (about 1.5-6 s each).
- New combo model "best" = priority failover over those working Copilot models, so there is one dependable entry that answers.
- Other providers OmniRoute can log into for you (same idea, no pasted key): gemini, antigravity, claude-code (browser flow), codex (callback), kiro (google/github social), cursor and zed (import from local config). List them with: omniroute oauth providers
- The DuckDuckGo option (free-all, 6 models, no tool calling) still works as a fallback if Copilot is rate-limited.
