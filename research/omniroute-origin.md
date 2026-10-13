# OmniRoute — how it was originally built (origin research)

Researched 2026-10-14 from the GitHub REST API, the npm registry, and the
installed package at /usr/local/lib/node_modules/omniroute.

## One line

OmniRoute is an MIT AI gateway: one OpenAI-compatible endpoint in front of
~358 providers. It is the third generation of a lineage — a TypeScript fork of
9Router, which is a JavaScript port of the Go project CLIProxyAPI.

## Lineage (three generations)

| Gen | Project | Lang | Started | Latest | Stars | Forks | Commits | Releases |
|-----|---------|------|---------|--------|-------|-------|---------|----------|
| 1 | router-for-me/CLIProxyAPI | Go | 2025-07-01 (v1.0.0 2025-07-02) | v8.0.27, 2026-10-12 | 55,064 | 8,423 | 4,303 | 889 |
| 2 | decolua/9router | JavaScript | npm 2026-01-03 / repo 2026-01-05 | v0.5.35, 2026-07-16 (npm 0.5.99) | 30,736 | 5,876 | 1,392 | 72 |
| 3 | diegosouzapw/OmniRoute | TypeScript | repo 2026-02-13 / npm 0.1.0 2026-02-14 | v3.8.51, 2026-09-30 | 76,052 | 10,982 | 10,418 | 281 |

All three are MIT. Star/commit/release counts are a dated snapshot (2026-10-14)
and will drift.

## The relationship is stated by the projects themselves

- 9Router README, "Forks" section: "OmniRoute — A full-featured TypeScript fork
  of 9Router. Adds 36+ providers, 4-tier auto-fallback, multi-modal APIs
  (images, embeddings, audio, TTS), circuit breaker, semantic cache, LLM
  evaluations, and a polished dashboard. 368+ unit tests."
- 9Router README, "Acknowledgments": "CLIProxyAPI — original Go implementation
  that inspired this JavaScript port."
- OmniRoute README, "Acknowledgments": "It started as a fork of 9router and a
  TypeScript port of the Go project CLIProxyAPI."

The shared idea across all three: sit between coding CLIs (Claude Code, Codex,
Cursor, Cline, Copilot, Gemini CLI, Antigravity, OpenCode, ...) and many
upstream AI providers, expose one OpenAI-compatible endpoint on port 20128,
translate between formats (OpenAI / Claude / Gemini / Responses), track quota,
and auto-fall-back across providers and accounts. 9Router added RTK token
compression; OmniRoute expanded to 300+ providers, the 12-engine compression
stack, MCP/A2A, and a full dashboard.

Note: 9Router itself began about six months after CLIProxyAPI, so the Go
original is the true root of the idea. CLIProxyAPI is still active (v8.0.27,
2026-10-12). 9Router's last release was 2026-07-16; its momentum moved into
forks like OmniRoute.

## Origin timeline of OmniRoute itself

- 2026-02-13  GitHub repo created. The repo "fork" flag is false: it is a code
  rewrite in the spirit of a fork, not a GitHub fork object.
- 2026-02-14  omniroute@0.1.0 published on npm. Required Node >=18 and ran
  Next.js dev on port 20128 — the same port used today.
- 2026-02-14..17  fast 0.x run (0.1.0 -> 0.9.0 in four days).
- 2026-02-18  v1.0.0 "Intelligent AI Gateway & Universal LLM Proxy": 20+
  providers, 6 routing strategies, OpenAI/Claude/Gemini/Responses translation,
  circuit breakers, semantic cache, combo fallback chains, real-time health
  monitoring, full dashboard, 100% TypeScript, Docker.
- History note: the early 0.x commits were squashed away. The oldest commit on
  the current default branch is the v1.0.0 commit (71d14209a, 2026-02-18,
  300 files, no parent). Only the consolidated v1.0.0 snapshot remains in git
  history.
- Later: v2.0.0 on 2026-03-05, v3.0.0 on 2026-03-24, then a rapid 3.x line to
  v3.8.51. The dev branch is release/v3.8.52, heading to v3.9.0 LTS.

## How the project is built (engineering)

- Runtime: Node.js 22/24 LTS. Language: 100% TypeScript 6.0 (zero any in core
  since v2.0).
- Dashboard: Next.js 16 + React 19 + Tailwind CSS 4.
- Data: better-sqlite3 (SQLite, WAL journaling) plus a LowDB JSON legacy layer.
  122 domain modules, 193 migrations.
- The streaming engine is split into its own workspace package,
  @omniroute/open-sse — provider dispatch, protocol translation, SSE streaming,
  account fallback, token refresh.
- Compression: 12 composable engines (Session-Dedup, CCR, Lite, RTK, Responses
  Tool Output, Headroom/GCF, Relevance, Caveman, Aggressive, LLMLingua-2 ONNX,
  Ultra, OmniGlyph). Default stacked RTK -> Caveman averages ~89% savings.
- Protocols: MCP plus A2A; SSE plus a WebSocket bridge.
- Stealth: wreq-js — JA3/JA4 TLS-fingerprint impersonation, shipped as attested
  native bindings.
- Resilience: circuit breaker, exponential backoff, anti-thundering-herd,
  self-healing auto-combo.
- Build/release: isolated Next.js build, esbuild/tsup for the CLI, dozens of
  native/prebuild guards, hundreds of check:* quality-gate scripts with
  ratchets, and the Node test runner plus Vitest (39,000+ static test
  declarations across 5,100+ test files). Ships via npm, Docker Hub, an
  Electron desktop app, a PWA, and Termux/Android.

## On this phone (installed)

- Global npm package: /usr/local/lib/node_modules/omniroute, with symlink
  /usr/local/bin/omniroute -> bin/omniroute.mjs. Version 3.8.51,
  BUILD_SHA c1e30b7.
- Data/config: /public/.omniroute (.env, storage.sqlite, logs/, exports/,
  server/.pid, shim/lsof).
- Autostarted by ~/.bashrc plus a watchdog; listens on 127.0.0.1:20128.
- Ships @omniroute/opencode-plugin(-v2) and the deprecated
  @omniroute/opencode-provider, which is how it wires into OpenCode here.
- Bonus: the install bundles an offline JS/TS toolchain under its own
  node_modules (esbuild 0.28.2, zod 4.6.5, tsx), so you can build JS/TS here
  without network or a global install. Node 24 also runs .ts directly.

## On this phone — current operational state (2026-10-14)

Folded in from the updated environment notes after the origin research.

- Two OpenCode plugins are loaded: phone-guard (auto-discovered) and
  omniroute-v2. The latter lives in /public/.opencode/plugins-available/ (not
  auto-scanned) and loads only via an explicit plugins entry with options. It is
  a working V2 build of OmniRoute's own @omniroute/opencode-plugin-v2, built
  2026-10-14 with OmniRoute's bundled esbuild.
- Effect of that plugin: the OpenCode "omniroute" provider grew from 52
  hardcoded models to ~500 fetched live from the gateway (505 after a dedupe
  pass; 542 raw). The number tracks OmniRoute's live catalog, so it drifts.
- Provider connections right now: github (GitHub Copilot via OAuth) is the one
  that works; llm7 is credits_exhausted, pollinations is enabled, ovhcloud is
  banned. The github connection was created 2026-10-14 with
  "omniroute oauth start --provider copilot --no-browser" (device code — the
  phone-friendly flow; no key pasting needed).
- Models verified answering through gh/*: claude-haiku-4.5, claude-sonnet-5,
  gpt-5.6-luna, gpt-5.4, gpt-4o-2024-11-20, grok-4.5, gemini-3.7-flash,
  mai-code-1.1-flash. Tool calling works. The advertised list is wider than the
  account entitlement, and Copilot rate-limits per model, so avoid rapid
  parallel calls.
- Two user combos were created 2026-10-14 and are selectable by name: "best"
  (priority failover across the working Copilot models) and "free-all" (the six
  keyless ddgw DuckDuckGo models, no tool calling).
- Auth is asymmetric: REQUIRE_API_KEY=false, yet POST /v1/chat/completions works
  with no key while GET /v1/models 401s without one. /api/* needs a
  management-scoped key (the client key "opencode-local" has only self:usage, so
  /api/* returns 403 AUTH_001). A second key, "opencode-management" (scopes
  manage + self:usage), was minted 2026-10-14 via the CLI and is the plugin's
  managementReadToken.
- When calling the gateway directly, strip OpenCode's provider prefix: send
  "pol/openai", not "omniroute/pol/openai" (the latter yields a misleading
  "No active credentials for provider: omniroute").
- Expected non-error noise: every omniroute CLI run prints ~46 ".env is
  ignored" lines from .env shadowing.

## Full diagnostics + fallback ladder (2026-10-14)

Checked the whole stack end to end. Green except one dead model entry, now fixed.

Verified working:
- OpenCode v2.0.28, service up (127.0.0.1:49374), config valid (opencode debug
  config OK; opencode.jsonc now also parses as strict JSON -- trailing commas
  removed).
- Both plugins load: phone-guard (auto-discovered) and omniroute-v2 (loaded by
  explicit entry with options). phone-guard matcher tests pass.
- Gateway health ok; watchdog RUNNING (pid tracked); autostart/watchdog/status
  scripts all pass "sh -n".
- Models answering: best (claude-sonnet-5), free-all, auto/best-coding and
  auto/best-chat (now route to gpt-4o-2024-11-20 -- the old "auto combos route
  to dead llm7" note is stale), gh/claude-haiku-4.5, gh/gpt-5.4,
  ddgw/gpt-5.4-mini.
- Disk: 253 GB free on /. State dirs small (tool-output 15.7 MB, log 4.5 MB).

Fixed today:
- Removed the one hard-dead hardcoded model, llm7/turbo (provider llm7 is
  credits_exhausted -> "No active credentials for provider: llm7"). It was the
  source of the recurring gateway error in the logs. It exists ONLY in the
  hardcoded list, not in the live catalog, so removing it removes it from the
  picker. Backup: opencode.jsonc.bak-pre-diag-*.
- Cleaned trailing commas in the plugins options block (v1 strict-parser safety).

Known residual (not a fault):
- "[400] The requested model is not supported": a session picked an advertised
  but unentitled Copilot model (e.g. claude-opus-5, gpt-5.5). Copilot advertises
  more than the account gets. Use the "best" combo or a known-good gh/* model.
- Memory is the real constraint: ~2 GB available, swap ~8/12 GB used. An
  Android low-memory kill of OmniRoute is the likely failure mode; the watchdog
  recovers it in under ~75 s.

Fallback ladder (each layer independent):
1. A model errors -> OpenCode retry; or switch to "best" (priority failover
   combo) or "free-all" (keyless ddgw).
2. The gateway process dies -> omniroute-watchdog.sh restarts it (<= ~75 s); a
   new terminal also triggers omniroute-autostart.sh.
3. The gateway is down entirely -> the OpenCode default model is native
   (opencode/big-pickle) and small_model is native, so chat/coding still work
   with no gateway.
4. A config mistake -> six timestamped opencode.jsonc.bak* copies to restore.

## Incident + memory-prevention work (2026-10-14)

### Latent fault found: a damaged install that would have killed the gateway for good

- The running gateway (pid 498, up since 2026-10-13 18:42) was only alive
  because it had the modules loaded in MEMORY. Between 02:23 and 02:27 that day,
  114 scoped packages under
  /usr/local/lib/node_modules/omniroute/node_modules/@* were emptied (only
  "@scope/name" dirs; every unscoped package stayed intact).
- ROOT CAUSE (found 2026-10-14 by reading
  /public/.local/share/opencode/log/opencode.log and the matching shell *.out
  files). It was NOT an npm run. Building the omniroute-v2 plugin did:
    02:22:40  ln -sfn /usr/local/lib/node_modules/omniroute/node_modules
                       /public/.opencode/plugins/omniroute-v2/node_modules
    02:22:47  rm -f node_modules              (hung)
    02:25:09  rm -f .../node_modules          (hung; produced NO output)
    02:27:25  timeout 20 rm -f .../node_modules
              -> "Terminated ... rm exit=143", symlink STILL present
    02:27:54  python3 -c "os.unlink(...)"     -> "unlinked", exit 0
  A plain `rm -f` on that symlink HUNG, and under proot the traversal went INTO
  the target and deleted package FILES (leaving empty dir skeletons) across
  02:23-02:27. `rm` never removed the link; only unlink/python3 did. This is the
  documented AGENTS.md gotcha, and it really did the damage. Lesson: never
  `ln -s <tree>/node_modules <dir>/node_modules`, and never `rm` such a symlink
  on this filesystem -- use unlink or python3.
- Consequence: the gateway ran on until the next restart. Any restart since
  02:27 died with "Cannot find module '@next/env'" (first in the load order).
  So the first Android OOM kill after 02:27 would have taken the gateway down
  for good, with the watchdog looping on a restart that could never work.
- Repair (all verified this day):
  1. Fetched @next/env@16.3.5 and @next/swc-linux-arm64-musl@16.3.5 straight
     from registry.npmjs.org and extracted them in place (damaged dirs moved to
     /data/data/com.foxdebug.acode/cache/opencode/fix-next/*.damaged). Boot then
     advanced past @next/env and stopped on @modelcontextprotocol/sdk, proving
     the damage was wider.
  2. There is NO package-lock.json, so the rest was restored with
     "cd /usr/local/lib/node_modules/omniroute && npm install --legacy-peer-deps".
     Plain "npm install" FAILS with ERESOLVE (peer conflict: marked@18 vs
     marked-terminal@7 which wants marked<16). --legacy-peer-deps is required.
     npm reported: added 1721, removed 278, changed 27, in ~7 min, exit 0. It also
     wrote a package-lock.json (2898 packages), copied to
     /public/backups/omniroute-npm-lock-20261014/ -- so a future restore can be
     reproducible with "npm ci --legacy-peer-deps" from that lock.
  3. npm then left 8 FILE-LESS skeletons (@ant-design/cssinjs, @babel/runtime,
     @emotion/*, ...). Those are build-time-only for the PREBUILT dashboard, not
     needed at runtime. Removed them (python shutil.rmtree) -> 0 incomplete dirs.
- Verified after repair: gateway boots and /api/health = {"status":"ok"} in ~9 s;
  keyless /v1/chat/completions answers on gh/claude-haiku-4.5 and on the "best"
  combo (routes to claude-sonnet-5); the dashboard serves (307 from /); both
  plugins load.

### Prevention added (the host killer itself cannot be changed)

- Host LMK is untouchable from here: ro.lmk.kill_heaviest_task and
  ro.lmk.use_minfree_levels are read-only props, no adb device is attached,
  /proc/sys/vm/* is unreadable, our cgroup memory files are absent, and
  oom_score_adj writes are denied. So the killer cannot be tuned -- the only
  levers are to stay small and to act before the kill.
- Heap caps: omniroute-autostart.sh launches the gateway with
  NODE_OPTIONS="--max-old-space-size=512 --max-semi-space-size=16" (measured RSS
  ~400 MB, so 384 was raised to 512 to avoid a "JavaScript heap out of memory"
  crash, while still bounding a runaway; a V8 default on this 11 GB box is
  2-4 GB). ~/.bashrc exports BUN_JSC_forceRAMSize=1073741824 for the OpenCode
  (Bun/JSC) processes (best-effort). Backups: *.bak-pre-mem-*.
- New ~/.config/opencode/bin/memory-guard.sh: polls MemAvailable every 30 s,
  logs a timeline, WARN below 1000 MB, and below 600 MB restarts the gateway
  (rate-limited to once per 10 min) to shed heap before the killer fires.
  Started detached from .bashrc beside the watchdog.
- autostart verify-window bug fixed: it probed health for only 5x2=10 s but
  startup takes ~25-30 s, so it always logged "start FAILED" even on success.
  Now 20x2=40 s.
- healthcheck.sh now checks dependency integrity: three runtime canaries must
  resolve (next, @next/env, @modelcontextprotocol/sdk) and a spike of more than
  20 incomplete @scope dirs is a FAIL -- so this class of damage is caught
  BEFORE a restart is needed, without false-alarming on build-time leftovers.
- phone-guard now BLOCKS creating the symlink that started it all: any `ln`
  command with a symlink flag whose argument names a /node_modules tree is
  denied (matcher isForbiddenLink, in both the permission and shell hooks).
  Test: node ~/.opencode/plugins/phone-guard/matchers.test.mjs (3 blocked /
  4 allowed link cases + the original 19/11 command cases).
- healthcheck.sh also RUNS that matcher test every time, and scans
  ~/.opencode for any leftover node_modules symlink (both are PASS/FAIL), so a
  regression of the guard shows up on the next check.
- memory-guard.sh now logs a gateway-RSS heartbeat every 5 min
  ("rss gateway=..MB peak=..MB avail=..MB") plus a --rss one-shot, giving the
  data to confirm the 512 MB cap against the real trend instead of guessing.
- ONE-COMMAND REPAIR: ~/.config/opencode/bin/omniroute-repair.sh. Check mode
  prints HEALTHY/DAMAGED; --repair runs npm install --legacy-peer-deps in the
  OmniRoute dir (~7 min), refuses under 500 MB free, then re-checks. The
  healthcheck FAIL lines now name it. Detection branches tested against
  simulated trees (missing canary / 25 empty scoped dirs / healthy).
- Everything above is backed up in /public/backups/phone-guard-20261014/
  (guard files + healthcheck.sh + memory-guard.sh + omniroute-repair.sh).
- LOCK-INHERITANCE FIX (found by audit, 2026-10-14). The watchdog and memory-guard
  use flock on a lock file for single-instance. The old claim "the kernel frees
  the lock when the process dies" was WRONG: children inherit open fds, so an
  orphaned `sleep` (spawned by the guard, holding the inherited lock fd) kept the
  flock held after the guard was killed -- and the next start then silently
  exit 0'd. Seen live: an orphan `sleep 45` held omniroute-watchdog.lock.
  Fix: close the lock fd on every long-lived child (`sleep ... 9>&-`,
  `sh "$AUTOSTART" ... 9>&-`, `wget ... 9>&-`) in BOTH scripts. Verified: the
  guard shells hold the lock while their sleep children hold only fds 0-2, so a
  kill frees the lock immediately. If a guard ever fails to start, find and kill
  the holder: `ls -l /proc/*/fd/9 2>/dev/null | grep omniroute-watchdog.lock`.
- The watchdog now also states WHY a restart failed (runs the repair check, logs
  "deps: ..." + a HINT), and every ~15 min while healthy logs "LATENT ..." if the
  tree is damaged -- closing the 02:27-04:52 blind spot.

## Mobile-useful takeaway

- Runs on this hardware as pure Node/Next.js — yes. No compiler, no Docker, no
  GPU needed; it is an API router, not a local model runner.
- All 12 compression engines are present, but the ONNX LLMLingua-2 engine is
  heavy for this phone's memory budget — leave it off and use RTK / Caveman /
  Lite.

## Links

- https://github.com/diegosouzapw/OmniRoute
- https://github.com/decolua/9router
- https://github.com/router-for-me/CLIProxyAPI
- https://www.npmjs.com/package/omniroute
- https://omniroute.online
