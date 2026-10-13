# Uncensored models on this phone (OmniRoute gateway)

Researched 2026-10-14 against the live gateway at 127.0.0.1:20128, the installed
OmniRoute package, and the upstream provider APIs.

## Short answer

- The gateway has **no mainstream uncensored/abliterated text model** built in.
  The only "NSFW"/"uncensored" entries are 164 AI Horde **image** checkpoints
  (Stable Diffusion style), not chat models.
- There **is** one free, keyless uncensored-ish chat route: the **UncloseAI /
  Hermes** provider (alias `unc`, no API key). It answers today.
- Everything genuinely uncensored otherwise needs an API key (Venice,
  uncensored.com, Nous Research, OpenRouter).
- Nothing uncensored runs **locally** on the phone.

## The working keyless option: UncloseAI (unc)

- Provider: `uncloseai`, alias `unc`, "UncloseAI", noauth, free, passthrough.
- Real endpoint (from the package registry file): base
  `https://hermes.ai.unturf.com/v1/chat/completions`, models
  `https://hermes.ai.unturf.com/v1/models`. A permacomputer community host.
- The built-in catalog id is **stale**: it ships
  `Lorbus/Qwen3.6-27B-int4-AutoRound`, which now returns HTTP 404
  ("The model ... does not exist"). The package's own note warns the roster
  rotates and to re-import from /models.
- **Live id (2026-10-14): `turboderp/Qwen3.8-27B-exl3`** (vLLM backend,
  max_model_len 65536). Confirmed three ways: the upstream /v1/models, the
  gateway's own `GET /api/providers/uncloseai/models`, and a real completion.
- Because the provider is **passthrough**, you can call it even though the
  stale id is the one shown in `GET /v1/models`:

  ```
  curl -s http://127.0.0.1:20128/v1/chat/completions \
    -H 'content-type: application/json' \
    -d '{"model":"unc/turboderp/Qwen3.8-27B-exl3",
         "messages":[{"role":"user","content":"hi"}],
         "max_tokens":200,
         "chat_template_kwargs":{"enable_thinking":false}}'
  ```

  Verified: HTTP 200, normal content, `finish_reason: stop`. Tool calling was
  not tested.

### Caveats for unc

- It is a **reasoning model**. Without `chat_template_kwargs.enable_thinking:
  false` it spends the output budget inside the `reasoning` field and returns
  `content: null`. Disable thinking or give it a large max_tokens.
- Shared free host: it returns `429 model_cooldown` (reset 1-2 min) when busy.
  Retry after the cooldown.
- It is Qwen3.8-27B under the hood. Asked directly, it says it follows safety
  guidelines and declines the label "uncensored". So treat `unc` as
  "lightly filtered / permissive", not as a true abliterated model.

## Uncensored-capable providers OmniRoute supports (need a key)

All are `api-key` category, none keyless. Add with
`omniroute providers add <id> --credential-stdin`.

| Provider id | Name | Notes |
|---|---|---|
| `venice` | Venice.ai | privacy/no-filter-first host |
| `uc-direct` | UC Direct (uncensored.com) | literally an uncensored host, alias `ucd` |
| `nous-research` | Nous Research | Hermes family, alias `nous` |
| `openrouter` | OpenRouter | widest choice of abliterated/uncensored community models |

`omniroute providers available | grep -i -e venice -e uncensored -e nous -e openrouter`
lists them. Same list also has many OpenAI-compatible aggregators
(featherless-style, NanoGPT-style, etc.) that carry uncensored models.

## Local option (not feasible here without work)

Running an abliterated GGUF on the phone would need a runtime and RAM. There is
no compiler (`gcc`/`make` missing) and no Ollama. A no-compiler path exists --
`llamafile` (single-file, cosmopolitan libc, aarch64 Linux) -- but a usable
3-4B Q4 model is roughly 2 GB and only ~3 GB is free, so it is tight. Reference
only; not set up.

## How to make the id stick in the catalog

`omniroute models add` failed with HTTP 401, and `POST /api/models` is 405, and
`POST /api/providers/uncloseai/sync-models` is 404 ("Connection not found",
because `uncloseai` is a catalog provider, not a saved connection). The
reliable path is the dashboard's Available Models -> Import from /models. Not
required for calling: passthrough already works with the correct id.

## X-rated (explicit) capability

Two different problems, two different answers.

### Explicit images - available now, free, keyless

Via AI Horde (`aihorde`, noauth, image-only). NSFW-capable checkpoints exposed
on the gateway include: WAI-NSFW-illustrious-SDXL, WAI-ANI-NSFW-PONYXL,
Hentai Diffusion, Grapefruit Hentai, Babes, Pony Realism, Yiffy,
AbyssOrangeMix-AfterDark, Deliberate. Caveats: volunteer queue (waits of
minutes), anonymous key, no tool calling, image output only.

### Explicit text - needs an uncensored LLM; nothing keyless is reliable

- `unc` (UncloseAI/Hermes) is permissive-ish but Qwen-tuned; treat as likely to
  refuse explicit sexual content.
- Cheapest real option: `nlpcloud` (catalog marks it free) carries the classic
  uncensored models `dolphin` and `chatdolphin`. Free key at nlpcloud.io, then
  `omniroute providers add nlpcloud --credential-stdin`.
- No-filter-first hosts: `venice` (api-key) or `venice-web` (web-cookie) for
  Venice.ai; `uc-direct` (api-key) or `uc` (web-cookie) for uncensored.com.
- Open-model hosts that carry uncensored/abliterated models: `chutes`,
  `deepinfra`, `nanogpt`, `featherless-ai`.
- Local abliterated / Dolphin GGUF via llamafile = the only route with no
  provider policy at all; ~2 GB and tight RAM. See the local note above.

## Links

- https://uncloseai.com (already in CATALOG.md)
- https://hermes.ai.unturf.com/v1/models (live roster, keyless)
